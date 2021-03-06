---
title: 教程：在单页应用中启用身份验证
titleSuffix: Azure AD B2C
description: 本教程介绍如何使用 Azure Active Directory B2C 为基于 JavaScript 的单页应用程序 (SPA) 提供用户登录功能。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: v-junlch
ms.date: 11/04/2020
ms.custom: mvc, seo-javascript-september2019, devx-track-js
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 93e21b80ee929ee05204b23e1a8ee2a0bead48cd
ms.sourcegitcommit: 33f2835ec41ca391eb9940edfcbab52888cf8a01
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94326433"
---
# <a name="tutorial-enable-authentication-in-a-single-page-application-with-azure-ad-b2c"></a>教程：使用 Azure AD B2C 在单页应用程序中启用身份验证

本教程介绍如何使用 Azure Active Directory B2C (Azure AD B2C) 在使用 OAuth 2.0 隐式授权流的单页应用程序 (SPA) 中进行用户登录和注册。

本教程是由两个部分组成的教程系列的第一部分，将介绍以下操作：

> [!div class="checklist"]
> * 将回复 URL 添加到在 Azure AD B2C 租户中注册的应用程序
> * 从 GitHub 下载代码示例
> * 修改示例应用程序的代码，使其适用于你的租户
> * 使用注册/登录用户流进行注册

本教程系列中的[下一篇教程](tutorial-single-page-app-webapi.md)将启用代码示例的 Web API 部分。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="prerequisites"></a>先决条件

在继续执行本教程中的步骤以前，需将以下 Azure AD B2C 资源准备到位：

* [Azure AD B2C 租户](tutorial-create-tenant.md)
* 在租户中[注册的应用程序](tutorial-register-spa.md)（使用“隐式流”选项）
* 在租户中[创建的用户流](tutorial-create-user-flows.md)

另外，需要在本地开发环境中备好以下项：

* [Visual Studio Code](https://code.visualstudio.com/) 或其他代码编辑器
* [Node.js](https://nodejs.org/en/download/)

## <a name="update-the-application"></a>更新应用程序

在按照先决条件完成的第二个教程中，你已在 Azure AD B2C 中注册了 Web 应用程序。 若要使用本教程中的代码示例实现通信，请将一个回复 URL（也称为重定向 URI）添加到应用程序注册。

要更新 Azure AD B2C 租户中的应用程序，可以使用新的统一“应用注册”体验或旧版“应用程序(旧版)”体验 。 [详细了解此新体验](/active-directory-b2c/app-registrations-training-guide)。

#### <a name="app-registrations"></a>[应用注册](#tab/app-reg-ga/)

1. 登录 [Azure 门户](https://portal.azure.cn)。
1. 在顶部菜单中选择“目录 + 订阅”筛选器，然后选择包含Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”。 或者，选择“所有服务”并搜索并选择“Azure AD B2C”。
1. 依次选择“应用注册(预览版)”、“拥有的应用程序”选项卡，然后选择“webapp1”应用程序 。
1. 在“Web”下，选择“添加 URI”链接，输入 `http://localhost:6420`。
1. 在“隐式授权”下，选中“访问令牌”和“ID 令牌”复选框（如果尚未选中），然后选择“保存”   。
1. 选择“概述”。
1. 记录“应用程序(客户端) ID”，以便稍后在单页 Web 应用程序中更新代码时使用。

#### <a name="applications-legacy"></a>[应用程序（旧版）](#tab/applications-legacy/)

1. 登录 [Azure 门户](https://portal.azure.cn)。
1. 请确保使用包含 Azure AD B2C 租户的目录，方法是选择顶部菜单中的“目录 + 订阅”筛选器，然后选择包含租户的目录。
1. 选择 Azure 门户左上角的“所有服务”，然后搜索并选择“Azure AD B2C” 。
1. 选择“应用程序(旧版)”，然后选择“webapp1”应用程序。
1. 在“回复 URL”下添加 `http://localhost:6420`。
1. 选择“保存”。
1. 在属性页上记录“应用程序 ID”。 在后面的步骤中，当你在单页 Web 应用程序中更新代码时，需使用此应用 ID。

* * *

## <a name="get-the-sample-code"></a>获取示例代码

在本教程中，你将配置从 GitHub 下载的代码示例，以其适用于你的 B2C 租户。 该示例演示了单页应用程序如何使用 Azure AD B2C 进行用户注册和登录，并调用受保护的 Web API（将在本系列教程的下一篇教程中启用该 Web API）。

从 GitHub [下载 zip 文件](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip)或克隆该示例。

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

## <a name="update-the-sample"></a>更新示例

获得示例以后，即可使用在此前的步骤中记录的 Azure AD B2C 租户名称和应用程序 ID 来更新代码。

1. 打开 JavaScriptSPA 文件夹中的 authConfig.js 文件。 
1. 在 `msalConfig` 对象中：
    * 使用在前面的步骤中记下的“应用程序(客户端) ID”值更新 `clientId`
    * 使用你的 Azure AD B2C 租户名称，以及在先决条件部分中创建的注册/登录用户流的名称（例如 B2C_1_signupsignin1）更新 `authority` URI

    ```javascript
    const msalConfig = {
        auth: {
          clientId: "00000000-0000-0000-0000-000000000000", // Replace this value with your Application (client) ID
          authority: b2cPolicies.authorities.signUpSignIn.authority,
          validateAuthority: false
        },
        cache: {
          cacheLocation: "localStorage",
          storeAuthStateInCookie: true
        }
    };

    const loginRequest = {
       scopes: ["openid", "profile"],
    };

    const tokenRequest = {
      scopes: apiConfig.b2cScopes // i.e. ["https://fabrikamb2c.partner.onmschina.cn/helloapi/demo.read"]
    };
    ```

1. 打开 JavaScriptSPA 文件夹中的 authConfig.js 文件。 
1. 在 `msalConfig` 对象中：
    * 使用在前面的步骤中记下的“应用程序(客户端) ID”更新 `clientId`
    * 使用你的 Azure AD B2C 租户名称，以及在先决条件部分中创建的注册/登录用户流的名称（例如 B2C_1_signupsignin1）更新 `authority` URI
1. 打开“policies.js”文件。
1. 找到 `names` 和 `authorities` 条目，并将其替换为在步骤 2 中创建的策略的名称。 将 `fabrikamb2c.partner.onmschina.cn` 替换为 Azure AD B2C 租户的名称，例如 `https://<your-tenant-name>.b2clogin.cn/<your-tenant-name>.partner.onmschina.cn/<your-sign-in-sign-up-policy>`。
1. 打开“apiConfig.js”文件。
1. 找到范围 `b2cScopes` 的分配，并将 URL 替换为为 Web API 创建的范围 URL，例如 `b2cScopes: ["https://<your-tenant-name>.partner.onmschina.cn/helloapi/demo.read"]`。
1. 找到 API URL `webApi` 的分配，并将当前 URL 替换为在步骤 4 中部署了 Web API 的 URL，例如 `webApi: http://localhost:5000/hello`。

结果代码应如下所示：

### <a name="authconfigjs"></a>authConfig.js

```javascript
const msalConfig = {
  auth: {
    clientId: "e760cab2-b9a1-4c0d-86fb-ff7084abd902",
    authority: b2cPolicies.authorities.signUpSignIn.authority,
    validateAuthority: false
  },
  cache: {
    cacheLocation: "localStorage",
    storeAuthStateInCookie: true
  }
};

const loginRequest = {
  scopes: ["openid", "profile"],
};

const tokenRequest = {
  scopes: apiConfig.b2cScopes // i.e. ["https://fabrikamb2c.partner.onmschina.cn/helloapi/demo.read"]
};
```
### <a name="policiesjs"></a>policies.js

```javascript
const b2cPolicies = {
    names: {
        signUpSignIn: "b2c_1_susi",
        forgotPassword: "b2c_1_reset",
        editProfile: "b2c_1_edit_profile"
    },
    authorities: {
        signUpSignIn: {
            authority: "https://fabrikamb2c.b2clogin.cn/fabrikamb2c.partner.onmschina.cn/b2c_1_susi",
        },
        forgotPassword: {
            authority: "https://fabrikamb2c.b2clogin.cn/fabrikamb2c.partner.onmschina.cn/b2c_1_reset",
        },
        editProfile: {
            authority: "https://fabrikamb2c.b2clogin.cn/fabrikamb2c.partner.onmschina.cn/b2c_1_edit_profile"
        }
    },
}
```
### <a name="apiconfigjs"></a>apiConfig.js

```javascript
const apiConfig = {
  b2cScopes: ["https://fabrikamb2c.partner.onmschina.cn/helloapi/demo.read"],
  webApi: "https://fabrikamb2chello.chinacloudsites.cn/hello"
};
```

## <a name="run-the-sample"></a>运行示例

1. 打开控制台窗口，切换到包含此示例的目录。 例如：

    ```console
    cd active-directory-b2c-javascript-msal-singlepageapp
    ```
1. 运行以下命令：

    ```console
    npm install && npm update
    npm start
    ```

    控制台窗口显示本地运行的 Node.js 服务器的端口号：

    ```console
    Listening on port 6420...
    ```
1. 浏览到 `http://localhost:6420`，查看在本地计算机上运行的 Web 应用程序。

    :::image type="content" source="media/tutorial-single-page-app/web-app-spa-01-not-logged-in.png" alt-text="显示了本地运行的单页应用程序的 Web 浏览器":::

### <a name="sign-up-using-an-email-address"></a>使用电子邮件地址注册

此示例应用程序支持注册、登录和密码重置。 在本教程中，你将使用电子邮件地址进行注册。

1. 选择“登录”以启动在前面步骤中指定的 B2C_1_signupsignin1 用户流。
1. Azure AD B2C 会显示一个包含注册链接的登录页。 由于你还没有帐户，因此请选择“立即注册”链接。
1. 注册工作流会显示一个页面，用于收集用户的标识并通过电子邮件地址对其进行验证。 注册工作流还会收集用户的密码和所请求的属性（在用户流中定义）。

    请使用有效的电子邮件地址，并使用验证码进行验证。 设置密码。 输入请求的属性的值。

    :::image type="content" source="media/tutorial-single-page-app/user-flow-sign-up-workflow-01.png" alt-text="Azure AD B2C 用户流显示的注册页":::

1. 选择“创建”，在 Azure AD B2C 目录中创建本地帐户。

选择“创建”时，应用程序会显示已登录用户的名称。

:::image type="content" source="media/tutorial-single-page-app/web-app-spa-02-logged-in.png" alt-text="显示了包含已登录用户的单页应用程序的 Web 浏览器":::

若要测试登录，请选择“注销”按钮，然后选择“登录”，并使用注册时输入的电子邮件地址和密码进行登录。 

### <a name="what-about-calling-the-api"></a>调用 API 时会发生什么情况？

如果在登录后选择“调用 API”按钮，将会看到注册/登录用户流页面，而不是 API 调用结果。 这种情况是意料之中的，因为尚未配置应用程序的 API 部分来与你的 Azure AD B2C 租户中注册的 Web API 应用程序进行通信。

此时，应用程序仍会尝试与演示租户 (fabrikamb2c.partner.onmschina.cn) 中注册的 API 进行通信，同时，由于你尚未在该租户中进行身份验证，将显示注册/登录页。

请继续学习本教程系列的下一篇教程来启用受保护的 API（参阅[后续步骤](#next-steps)部分）。

## <a name="next-steps"></a>后续步骤

在本教程中，你已将某个单页应用程序配置为使用 Azure AD B2C 租户中的用户流来提供注册和登录功能。 你已完成以下步骤：

> [!div class="checklist"]
> * 将回复 URL 添加到了在 Azure AD B2C 租户中注册的应用程序
> * 从 GitHub 下载了代码示例
> * 修改了示例应用程序的代码，使其适用于你的租户
> * 使用注册/登录用户流完成了注册

现在可转到此系列的下一教程，了解如何授予用户访问权限，以便从 SPA 访问受保护的 Web API：

> [!div class="nextstepaction"]
> [教程：从单页应用程序保护 Web API 及授予对 Web API 的访问权限 >](tutorial-single-page-app-webapi.md)

