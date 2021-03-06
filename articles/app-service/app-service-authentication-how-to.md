---
title: AuthN/AuthZ 的高级用法
description: 了解如何针对不同情况自定义应用服务中的身份验证和授权功能，并获取用户声明和不同令牌。
ms.topic: article
origin.date: 07/08/2020
ms.date: 10/19/2020
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: be024ed20e09f09dd3354f01dba7eaef15bc5afd
ms.sourcegitcommit: e2e418a13c3139d09a6b18eca6ece3247e13a653
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92170649"
---
# <a name="advanced-usage-of-authentication-and-authorization-in-azure-app-service"></a>Azure 应用服务中的身份验证和授权的高级用法

本文介绍了如何自定义[应用服务中的内置身份验证和授权](overview-authentication-authorization.md)，以及如何从应用程序管理标识。 

若要快速入门，请参阅以下教程之一：

<!--* [How to configure your app to use Facebook login](configure-authentication-provider-facebook.md)-->
<!--* [How to configure your app to use Google login](configure-authentication-provider-google.md)-->
<!--* [How to configure your app to use Twitter login](configure-authentication-provider-twitter.md)-->
<!-- * [Tutorial: Authenticate and authorize users end-to-end in Azure App Service](tutorial-auth-aad.md) -->

* [How to configure your app to use Azure Active Directory login](configure-authentication-provider-aad.md)
* [How to configure your app to use Microsoft Account login](configure-authentication-provider-microsoft.md)
* [如何将应用配置为使用 OpenID Connect 提供程序（预览版）进行登录](configure-authentication-provider-openid-connect.md)

## <a name="use-multiple-sign-in-providers"></a>使用多个登录提供程序

门户配置不会提供向用户显示多个登录提供程序的统包方式。 但是，将此功能添加到应用并不困难。 步骤概括如下：

首先，在 Azure 门户中的“身份验证/授权”页上，配置想要启用的每个标识提供者。 

在“请求未经身份验证时需执行的操作”中，选择“允许匿名请求(无操作)”。  

在登录页、导航栏或应用的其他任何位置中，将一个登录链接添加到已启用的每个提供程序 (`/.auth/login/<provider>`)。 例如：

```html
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
```

当用户单击其中一个链接时，系统会打开相应的登录页让用户登录。

若要将登录后用户重定向到自定义 URL，请使用 `post_login_redirect_url` 查询字符串参数（不要与标识提供者配置中的重定向 URI 混淆）。 例如，若要在登录后将用户导航至 `/Home/Index`，使用以下 HTML 代码：

```html
<a href="/.auth/login/<provider>?post_login_redirect_url=/Home/Index">Log in</a>
```

## <a name="validate-tokens-from-providers"></a>验证来自提供程序的令牌

在客户端定向的登录中，应用程序手动将用户登录到提供程序，然后将身份验证令牌提交给应用服务进行验证（请参阅[身份验证流](overview-authentication-authorization.md#authentication-flow)）。 此验证本身不实际向你授予对所需应用资源的访问权限，但成功的验证会向你提供一个会话令牌，可以使用该令牌来访问应用资源。 

若要验证提供程序令牌，必须首先为应用服务应用配置所需的提供程序。 在运行时，从你的提供程序检索身份验证令牌后，将令牌发布到 `/.auth/login/<provider>` 进行验证。 例如： 

```
POST https://<appname>.chinacloudsites.cn/.auth/login/aad HTTP/1.1
Content-Type: application/json

{"id_token":"<token>","access_token":"<token>"}
```

令牌格式根据提供程序而略有不同。 有关详细信息，请参阅下表：

| 提供程序值 | 请求正文中必需的 | 注释 |
|-|-|-|
| `aad` | `{"access_token":"<access_token>"}` | |
| `microsoftaccount` | `{"access_token":"<token>"}` | `expires_in` 属性为可选。 <br/>从 Live 服务请求令牌时，将始终请求 `wl.basic` 作用域。 |

如果提供程序令牌成功通过验证，则 API 将在响应正文中返回一个 `authenticationToken`，这是你的会话令牌。 

```json
{
    "authenticationToken": "...",
    "user": {
        "userId": "sid:..."
    }
}
```

获得此会话令牌后，你可以通过向 HTTP 请求中添加 `X-ZUMO-AUTH` 标头来访问受保护的应用资源。 例如： 

```
GET https://<appname>.chinacloudsites.cn/api/products/1
X-ZUMO-AUTH: <authenticationToken_value>
```

## <a name="sign-out-of-a-session"></a>注销会话

用户可通过向应用的 `/.auth/logout` 终结点发送 `GET` 请求来启动注销。 `GET` 请求可执行以下操作：

- 清除当前会话中的身份验证 Cookie。
- 从令牌存储中删除当前用户的令牌。
- 对于 Azure Active Directory，请对标识提供者执行服务器端注销。

以下是网页中一个简单的注销链接：

```html
<a href="/.auth/logout">Sign out</a>
```

默认情况下，成功注销后，客户端会重定向到 URL `/.auth/logout/done`。 通过添加 `post_logout_redirect_uri` 查询参数可更改注销后的重定向页面。 例如：

```
GET /.auth/logout?post_logout_redirect_uri=/index.html
```

建议对 `post_logout_redirect_uri` 的值进行编码。

使用完全限定的 URL 时，URL 必须托管在同一域中，或配置为允许应用访问的外部重定向 URL。 在以下示例中，若要重定向到未托管在同一域中的 `https://myexternalurl.com`：

```
GET /.auth/logout?post_logout_redirect_uri=https%3A%2F%2Fmyexternalurl.com
```

在 Azure CLI 中运行以下命令：

```azurecli
az webapp auth update --name <app_name> --resource-group <group_name> --allowed-external-redirect-urls "https://myexternalurl.com"
```

## <a name="preserve-url-fragments"></a>保留 URL 片段

用户登录应用后，通常希望会重定向到同一页面的同一部分，例如 `/wiki/Main_Page#SectionZ`。 但是，由于从不向服务器发送 URL 片段（例如，`#SectionZ`），因此默认情况下，OAuth 登录完成并重定向回应用后，会保留这些片段。 然后，当用户需再次导航到所需定位点时，他们无法获得最佳体验。 此限制存在于所有服务器端身份验证解决方案中。

在应用服务身份验证中，可跨 OAuth 登录保留 URL 片段。 为此，请将名为 `WEBSITE_AUTH_PRESERVE_URL_FRAGMENT` 的应用设置设为 `true`。 可在 [Azure 门户](https://portal.azure.cn) 中执行此操作，或只需在 Azure CLI 中运行以下命令：

```azurecli
az webapp config appsettings set --name <app_name> --resource-group <group_name> --settings WEBSITE_AUTH_PRESERVE_URL_FRAGMENT="true"
```

## <a name="access-user-claims"></a>访问用户声明

应用服务使用特殊的标头将用户声明传递给应用程序。 不允许外部请求设置这些标头，因此，只会提供应用服务设置的标头。 部分标头示例如下：

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

使用任何语言或框架编写的代码均可从这些标头获取所需信息。 对于 ASP.NET 4.6 应用， **ClaimsPrincipal** 会自动设置为相应的值。 但是，ASP.NET Core 不提供与应用服务用户声明集成的身份验证中间件。 有关解决方法，请参阅 [MaximeRouiller.Azure.AppService.EasyAuth](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth)。

应用程序也可以通过调用 `/.auth/me` 来获取有关经过身份验证的用户的其他详细信息。 移动应用服务器 SDK 提供处理该数据的帮助器方法。 有关详细信息，请参阅[如何使用 Azure 移动应用 Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity) 和[使用适用于 Azure 移动应用的 .NET 后端服务器 SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info)。

## <a name="retrieve-tokens-in-app-code"></a>检索应用代码中的令牌

在服务器代码中，提供程序特定的令牌将注入到请求标头中，使你可以轻松访问这些令牌。 下表显示了可能的令牌标头名称：

| 提供程序 | 标头名称 |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Microsoft 帐户 | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
|||

在客户端代码（例如移动应用或浏览器中 JavaScript）中，将 HTTP `GET` 请求发送到 `/.auth/me`。 返回的 JSON 包含提供程序特定的令牌。

> [!NOTE]
> 访问令牌用于访问提供程序资源，因此，仅当使用客户端机密配置了提供程序时，才提供这些令牌。 若要了解如何获取刷新令牌，请参阅“刷新访问令牌”。

## <a name="refresh-identity-provider-tokens"></a>刷新标识提供程序令牌

当提供程序的访问令牌（而不是[会话令牌](#extend-session-token-expiration-grace-period)）到期时，需要在再次使用该令牌之前重新验证用户。 向应用程序的 `/.auth/refresh` 终结点发出 `GET` 调用可以避免令牌过期。 调用应用服务时，应用服务会自动刷新已身份验证用户的令牌存储中的访问令牌。 应用代码发出的后续令牌请求将获取刷新的令牌。 但是，若要正常刷新令牌，令牌存储必须包含提供程序的[刷新令牌](https://auth0.com/learn/refresh-tokens/)。 每个提供程序会阐述获取刷新令牌的方式。以下列表提供了简短摘要：

- **Microsoft 帐户** ： [配置 Microsoft 帐户身份验证设置](configure-authentication-provider-microsoft.md)时，请选择 `wl.offline_access` 范围。
- Azure Active Directory

<!-- Azure Resource Manager not available-->

配置提供程序以后，即可在令牌存储中[找到访问令牌的刷新令牌和过期时间](#retrieve-tokens-in-app-code)。 

若要随时刷新访问令牌，只需以任何语言调用 `/.auth/refresh`。 以下代码片段从 JavaScript 客户端使用 jQuery 刷新访问令牌。

```javascript
function refreshTokens() {
  let refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

如果用户撤销了授予应用的权限，对 `/.auth/me` 的调用可能会失败并返回 `403 Forbidden` 响应。 若要诊断错误，请查看应用程序日志了解详细信息。

## <a name="extend-session-token-expiration-grace-period"></a>延长会话令牌过期宽限期

经过身份验证的会话会在 8 小时后过期。 经过身份验证的会话过期后，默认会提供 72 小时的宽限期。 在此宽限期内，可以使用应用服务刷新会话令牌，而无需重新对用户进行身份验证。 会话令牌失效后，只需调用 `/.auth/refresh`，而不需要自行跟踪令牌过期时间。 72 小时的宽限期过后，用户必须重新登录才能获取有效的会话令牌。

如果 72 小时的时间不够，可以延长此过期期限。 大大延长过期时间可能会造成严重的安全风险（例如身份验证令牌泄密或被盗）。 因此，应将宽限期保留为默认 72 小时，或者将延期设为最小值。

若要延长默认的过期期限，请在 Azure CLI 中运行以下命令。

```azurecli
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> 宽限期仅适用于应用服务的已经身份验证的会话，而不适用于来自标识提供者的令牌。 已过期的提供程序令牌没有宽限期。 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>限制登录帐户的域

Microsoft 帐户和 Azure Active Directory 都允许从多个域登录。 Azure AD 允许对登录帐户使用任意数量的自定义域。 但是，建议将用户直接转到自己品牌的 Azure AD 登录页面（如 `contoso.com`）。 若要推荐登录帐户的域名，请执行以下步骤。

此设置将 `domain_hint` 查询字符串参数追加到登录重定向 URL。 

> [!IMPORTANT]
> 接收重定向 URL 之后，客户端可能删除 `domain_hint` 参数，然后使用其他域登录。 所以虽然此功能非常方便，但它不是一项安全功能。
>
## <a name="authorize-or-deny-users"></a>授权或拒绝用户

尽管应用服务会处理最简单的授权问题（例如，拒绝未经身份验证的请求），但应用可能需要更精细的授权行为，例如，仅将访问权限限制给特定的一组用户。 在某些情况下，需要编写自定义应用程序代码以允许或拒绝已登录用户的访问。 在其他情况下，应用服务或标识提供者可能无需进行代码更改即可提供帮助。

- [服务器级别](#server-level-windows-apps-only)
- [应用程序级别](#application-level)

### <a name="server-level-windows-apps-only"></a>服务器级别（仅限 Windows 应用）

对于任何 Windows 应用，可以通过编辑 *Web.config* 文件来定义 IIS Web 服务器的授权行为。 Linux 应用不使用 IIS，无法通过 Web.config 进行配置。

1. 导航到 `https://<app-name>.scm.chinacloudsites.cn/DebugConsole`

1. 在打开应用服务文件的浏览器资源管理器中，导航到“site/wwwroot”。  如果 *Web.config* 不存在，请选择“+” > “新建文件”来创建该文件。   

1. 选择“Web.config”旁边的铅笔图标对其进行编辑。  添加以下配置代码，然后单击“保存”。  如果 *Web.config* 已存在，则只需在其中添加包含任何内容的 `<authorization>` 元素即可。 在 `<allow>` 元素中添加要允许的帐户。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
       <system.web>
          <authorization>
            <allow users="user1@contoso.com,user2@contoso.com"/>
            <deny users="*"/>
          </authorization>
       </system.web>
    </configuration>
    ```

<!-- ### Identity provider level -->

<!-- The identity provider may provide certain turn-key authorization. For example: -->

<!-- - For [Azure App Service](configure-authentication-provider-aad.md), you can [manage enterprise-level access](../active-directory/manage-apps/what-is-access-management.md) directly in Azure AD. For instructions, see [How to remove a user's access to an application](../active-directory/manage-apps/methods-for-removing-user-access.md). -->
<!-- - For [Google](configure-authentication-provider-google.md), Google API projects that belong to an [organization](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#organizations) can be configured to allow access only to users in your organization (see [Google's **Setting up OAuth 2.0** support page](https://support.google.com/cloud/answer/6158849?hl=en)). -->

### <a name="application-level"></a>应用程序级别

如果其他任何级别不提供所需的授权，或者平台或标识提供者不受支持，则必须编写自定义代码，以基于[用户声明](#access-user-claims)为用户授权。

## <a name="configure-using-a-file-preview"></a><a name="config-file"> </a>使用文件进行配置（预览）

可以选择通过部署提供的文件来配置身份验证设置。 应用服务身份验证/授权的某些预览功能可能要求此操作。

> [!IMPORTANT]
> 请记住，应用的有效负载（并由此该文件）可能随[槽](./deploy-staging-slots.md)在环境之间移动。 可能需要将不同的应用注册固定到每个槽，在这些情况下，应继续使用标准配置方法，而非配置文件。

### <a name="enabling-file-based-configuration"></a>启用基于文件的配置

> [!CAUTION]
> 预览期间，启用基于文件的配置会禁止通过某些客户端（例如 Azure 门户、Azure CLI 和 Azure PowerShell）管理应用程序的应用服务身份验证/授权功能。

1. 在项目根目录（部署到 Web/函数应用中的 D:\home\site\wwwroot）为配置创建新的 JSON 文件。 根据[基于文件的配置引用](#configuration-file-reference)填写所需的配置。 如果修改现有 Azure 资源管理器配置，确保将 `authsettings` 集合中捕获的属性转换为配置文件。

2. 修改现有配置，它在 `Microsoft.Web/sites/<siteName>/config/authsettings` 下的 [Azure 资源管理器](../azure-resource-manager/management/overview.md) API 中捕获。 若要修改它，可以使用 [Azure 资源管理器模板](../azure-resource-manager/templates/overview.md)。 在 authsettings 集合中，需要设置三个属性（并可能删除其他属性）：

    1.  将 `enabled` 设为 true
    2.  将 `isAuthFromFile` 设为 true
    3.  将 `authFilePath` 设为文件的名称（例如 auth.json）

> [!NOTE]
> `authFilePath` 的格式在平台之间有所不同。 在 Windows 上，支持相对路径和绝对路径。 建议使用相对路径。 对于 Linux，当前仅支持绝对路径，因此设置的值应为“/home/site/wwwroot/auth.json”或类似。

完成此配置更新后，该文件的内容将用于定义该站点的应用服务身份验证/授权行为。 如果希望回到 Azure 资源管理器配置，可以将 `isAuthFromFile` 设置回 false。

### <a name="configuration-file-reference"></a>配置文件引用

从配置文件引用的任何机密都必须存储为[应用程序设置](./configure-common.md#configure-app-settings)。 可以将设置命名为任何所需名称。 只需确保配置文件中的引用使用相同的键。

以下详尽无遗地介绍文件中的可能配置选项：

```json
{
    "platform": {
        "enabled": <true|false>
    },
    "globalValidation": {
        "requireAuthentication": <true|false>,
        "unauthenticatedClientAction": "RedirectToLoginPage|AllowAnonymous|Return401|Return403",
        "redirectToProvider": "<default provider alias>",
        "excludedPaths": [
            "/path1",
            "/path2"
        ]
    },
    "httpSettings": {
        "requireHttps": <true|false>,
        "routes": {
            "apiPrefix": "<api prefix>"
        },
        "forwardProxy": {
            "convention": "NoProxy|Standard|Custom",
            "customHostHeaderName": "<host header value>",
            "customProtoHeaderName": "<proto header value>"
        }
    },
    "login": {
        "routes": {
            "logoutEndpoint": "<logout endpoint>"
        },
        "tokenStore": {
            "enabled": <true|false>,
            "tokenRefreshExtensionHours": "<double>",
            "fileSystem": {
                "directory": "<directory to store the tokens in if using a file system token store (default)>"
            },
            "azureBlobStorage": {
                "sasUrlSettingName": "<app setting name containing the sas url for the Azure Blob Storage if opting to use that for a token store>"
            }
        },
        "preserveUrlFragmentsForLogins": <true|false>,
        "allowedExternalRedirectUri": [
            "https://uri1.chinacloudsites.cn/",
            "https://uri2.chinacloudsites.cn/",
            "url_scheme_of_your_app://easyauth.callback"
        ],
        "cookieExpiration": {
            "convention": "FixedTime|IdentityProviderDerived",
            "timeToExpiration": "<timespan>"
        },
        "nonce": {
            "validateNonce": <true|false>,
            "nonceExpirationInterval": "<timespan>"
        }
    },
    "identityProviders": {
        "azureActiveDirectory": {
            "enabled": <true|false>,
            "registration": {
                "openIdIssuer": "<issuer url>",
                "clientId": "<app id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_AAD_SECRET",
            },
            "login": {
                "loginParameters": [
                    "paramName1=value1",
                    "paramName2=value2"
                ]
            },
            "validation": {
                "allowedAudiences": [
                    "audience1",
                    "audience2"
                ]
            }
        },
        "facebook": {
            "enabled": <true|false>,
            "registration": {
                "appId": "<app id>",
                "appSecretSettingName": "APP_SETTING_CONTAINING_FACEBOOK_SECRET"
            },
            "graphApiVersion": "v3.3",
            "login": {
                "scopes": [
                    "profile",
                    "email"
                ]
            },
        },
        "gitHub": {
            "enabled": <true|false>,
            "registration": {
                "clientId": "<client id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_GITHUB_SECRET"
            },
            "login": {
                "scopes": [
                    "profile",
                    "email"
                ]
            }
        },
        "google": {
            "enabled": true,
            "registration": {
                "clientId": "<client id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_GOOGLE_SECRET"
            },
            "login": {
                "scopes": [
                    "profile",
                    "email"
                ]
            },
            "validation": {
                "allowedAudiences": [
                    "audience1",
                    "audience2"
                ]
            }
        },
        "twitter": {
            "enabled": <true|false>,
            "registration": {
                "consumerKey": "<consumer key>",
                "consumerSecretSettingName": "APP_SETTING_CONTAINING TWITTER_CONSUMER_SECRET"
            }
        },
        "openIdConnectProviders": {
            "<providerName>": {
                "enabled": <true|false>,
                "registration": {
                    "clientId": "<client id>",
                    "clientCredential": {
                        "secretSettingName": "<name of app setting containing client secret>"
                    },
                    "openIdConnectConfiguration": {
                        "authorizationEndpoint": "<url specifying authorization endpoint>",
                        "tokenEndpoint": "<url specifying token endpoint>",
                        "issuer": "<url specifying issuer>",
                        "certificationUri": "<url specifying jwks endpoint>",
                        "wellKnownOpenIdConfiguration": "<url specifying .well-known/open-id-configuration endpoint - if this property is set, the other properties of this object are ignored, and authorizationEndpoint, tokenEndpoint, issuer, and certificationUri are set to the corresponding values listed at this endpoint>"
                    }
                },
                "login": {
                    "nameClaimType": "<name of claim containing name>",
                    "scope": [
                        "openid",
                        "profile",
                        "email"
                    ],
                    "loginParameterNames": [
                        "paramName1=value1",
                        "paramName2=value2"
                    ],
                }
            },
            //...
        }
    }
}
```

## <a name="pin-your-app-to-a-specific-authentication-runtime-version"></a>将应用固定到特定身份验证运行时版本

启用身份验证/授权时，会将平台中间件注入 HTTP 请求管道，如[功能概述](overview-authentication-authorization.md#how-it-works)中所述。 作为平台例常更新的一部分，此平台中间件定期更新新功能和改进。 默认情况下，Web 或函数应用在此平台中间件的最新版本上运行。 这些自动更新始终向后兼容。 但在极少情况下此自动更新引入 Web 或函数应用的运行时问题，此时可以暂时回滚到以前的中间件版本。 本文介绍如何将应用临时固定到特定版本的身份验证中间件。

### <a name="automatic-and-manual-version-updates"></a>自动和手动版本更新 

可以通过设置应用的 `runtimeVersion` 设置，将应用固定到平台中间件的特定版本。 应用始终在最新版本上运行，除非选择将其显式固定回特定版本。 一次支持几个版本。 如果固定到不再受支持的无效版本，应用将改用最新版本。 若要始终运行最新版本，将 `runtimeVersion` 设为 ~1。 

### <a name="view-and-update-the-current-runtime-version"></a>查看和更新当前运行时版本

可以更改应用使用的运行时版本。 新的运行时版本应在重启应用后生效。 

#### <a name="view-the-current-runtime-version"></a>查看当前运行时版本

可以使用 Azure CLI 或应用中其中一个内置版本 HTTP 终结点来查看平台身份验证中间件的当前版本。

##### <a name="from-the-azure-cli"></a>通过 Azure CLI

使用 Azure CLI 通过 [az webapp auth show](https://docs.azure.cn/cli/webapp/auth?view=azure-cli-latest#az-webapp-auth-show) 命令查看当前中间件版本。

```azurecli
az webapp auth show --name <my_app_name> \
--resource-group <my_resource_group>
```

在此代码中，用应用名称替换 `<my_app_name>`。 还使用应用的资源组名称替换 `<my_resource_group>`。

将在 CLI 输出中看到 `runtimeVersion` 字段。 它类似于以下示例输出，为了清晰起见，该输出已被截断： 
```output
{
  "additionalLoginParams": null,
  "allowedAudiences": null,
    ...
  "runtimeVersion": "1.3.2",
    ...
}
```

##### <a name="from-the-version-endpoint"></a>使用版本终结点

还可点击应用上的 /.auth/version 终结点来查看应用运行所在的当前中间件版本。 它类似于以下示例输出：
```output
{
"version": "1.3.2"
}
```

#### <a name="update-the-current-runtime-version"></a>更新当前运行时版本

使用 Azure CLI，可以通过 [az webapp auth update](https://docs.azure.cn/cli/webapp/auth?view=azure-cli-latest#az-webapp-auth-update) 命令更新应用中的 `runtimeVersion` 设置。

```azurecli
az webapp auth update --name <my_app_name> \
--resource-group <my_resource_group> \
--runtime-version <version>
```

将 `<my_app_name>` 替换为你的应用的名称。 还使用应用的资源组名称替换 `<my_resource_group>`。 另外，将 `<version>` 替换为 1.x 运行时的有效版本，或替换为 `~1` 获取最新版本。 可以在 [此处] (https://github.com/Azure/app-service-announcements) 查找不同运行时版本的发行说明来帮助确定要固定到哪个版本。

<!-- You can run this command from the [Azure Cloud Shell](../cloud-shell/overview.md) by choosing **Try it** in the preceding code sample. -->

可以在执行 [az login](https://docs.azure.cn/cli/reference-index#az-login) 登录后从 [Azure CLI 在本地](https://docs.azure.cn/cli/install-azure-cli)运行此命令以执行此命令。

<!-- 
## Next steps

> [!div class="nextstepaction"]
> [Tutorial: Authenticate and authorize users end-to-end](tutorial-auth-aad.md) -->
