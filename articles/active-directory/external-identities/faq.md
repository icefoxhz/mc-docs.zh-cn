---
title: B2B 协作常见问题解答 - Azure Active Directory | Microsoft Docs
description: 获取有关 Azure Active Directory B2B 协作的常见问题的解答。
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: reference
ms.date: 10/09/2020
ms.author: v-junlch
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: be4a7973d52b507e58c5a362953eaf0c21483597
ms.sourcegitcommit: 63b9abc3d062616b35af24ddf79679381043eec1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2020
ms.locfileid: "91937574"
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Azure Active Directory B2B 协作常见问题解答

有关 Azure Active Directory (Azure AD) 企业到企业 (B2B) 协作的常见问题解答 (FAQ) 会定期更新，以包含新主题。

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>B2B 协作用户能否访问 SharePoint Online 和 OneDrive？
是的。 不过，在 SharePoint Online 中使用人员选取器搜索现有来宾用户的功能默认已**关闭**。 要启用该选项来搜索现有来宾用户，请将 **ShowPeoplePickerSuggestionsForGuestUsers** 为 **On**。 可以在租户级别或站点集合级别启用此设置。 可以使用 Set-SPOTenant 和 Set-SPOSite cmdlet 更改此设置。 使用这些 cmdlet，成员可以搜索目录中的所有现有来宾用户。 租户范围中的更改不会影响已经预配的 SharePoint Online 站点。

### <a name="is-the-csv-upload-feature-still-supported"></a>是否仍支持 CSV 上传功能？
是的。 有关使用 .csv 文件上传功能的详细信息，请参阅[此 PowerShell 示例](code-samples.md)。

### <a name="how-can-i-customize-my-invitation-emails"></a>我能否自定义邀请电子邮件？
使用 [B2B 邀请 API](customize-invitation-api.md)，几乎可以自定义有关邀请者过程的所有内容。

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>来宾用户是否可以重置其多重身份验证方法？
是的。 来宾用户可以像普通用户一样重置其多重身份验证方法。

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>哪家组织负责提供多重身份验证许可证？
邀请方组织执行多重身份验证。 邀请方组织必须确保为其使用多重身份验证的 B2B 用户提供足够的许可证。

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>如果合作伙伴组织已设置多重身份验证会怎样？ 我们是否可以信任他们的多重身份验证，而不使用我们自己的多重身份验证？
目前不支持此功能。 如果需要进行多重身份验证才能访问你的组织的资源，则合作伙伴组织需要在（发出邀请的）组织中注册多重身份验证。

### <a name="how-can-i-use-delayed-invitations"></a>如何使用延迟的邀请？
某家组织想要添加 B2B 协作用户，根据需要将这些用户预配到应用程序中，然后发送邀请。 可以使用 B2B 协作邀请 API 自定义载入工作流。

### <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>能否在 Exchange 全局地址列表中显示来宾用户？
是的。 默认情况下，来宾对象在组织的全局地址列表 (GAL) 中不可见，但你可以使用 Azure Active Directory PowerShell 使其可见。 请参阅[能否在全局地址列表中显示来宾对象？](https://docs.microsoft.com/office365/admin/create-groups/manage-guest-access-in-groups#add-guests-to-the-global-address-list)

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>是否可将来宾用户指定为受限制的管理员？
绝对是。 有关详细信息，请参阅[将来宾用户添加到角色](add-guest-to-role.md)。

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-to-access-the-azure-portal"></a>Azure AD B2B 协作是否允许 B2B 用户访问 Azure 门户？
除非 B2B 协作用户分配有受限管理员角色，否则他们不需要对 Azure 门户的访问权限。 但是，分配有受限管理员角色的 B2B 协作用户可以访问门户。 另外，如果未被分配这些管理员角色之一的来宾用户访问门户，该用户可以访问某些部分的体验。 来宾用户角色具有目录中的某些权限。

### <a name="can-i-block-access-to-the-azure-portal-for-guest-users"></a>我能否阻止来宾用户访问 Azure 门户？

能！ 你可以创建一个条件访问策略，阻止所有来宾用户和外部用户访问 Azure 门户。 配置此策略时请小心，避免意外阻止成员和管理员的访问。

1. 以安全管理员或条件访问管理员的身份登录到 [Azure 门户](https://portal.azure.cn/)。
2. 在 Azure 门户中，选择“Azure Active Directory”。  
3. 在“管理”下，选择“安全性”   。
4. 在“保护”下，选择“条件访问”   。 选择“新策略”  。
5. 在“新建”  页的“名称”  文本框中，为策略输入一个名称（例如“阻止来宾访问门户”）。
6. 在“分配”  下，选择“用户和组”  。
7. 在“包括”选项卡上，选择“选择用户和组”，然后选择“所有来宾和外部用户(预览版)”    。
9. 选择“完成”  。
10. 在“新建”页的“分配”部分，选择“云应用或操作”    。
11. 在“云应用或操作”页上，选择“选择应用”，然后选择“选择”    。
12. 在“选择”页上，选择“Azure 管理”，然后选择“选择”    。
13. 在“云应用或操作”页上，选择“完成”   。

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Azure AD B2B 协作是否支持多重身份验证和使用者电子邮件帐户？
是的。 Azure AD B2B 协作同时支持多重身份验证和使用者电子邮件帐户。

### <a name="do-you-support-password-reset-for-azure-ad-b2b-collaboration-users"></a>是否支持 Azure AD B2B 协作用户的密码重置？
如果 Azure AD 租户是用户的主目录，则可以从 Azure 门户[重置用户的密码](/active-directory/fundamentals/active-directory-users-reset-password-azure-portal)。 但是，对于使用由其他 Azure AD 目录或外部标识提供者管理的帐户登录的来宾用户，无法直接重置其密码。 只有用户主目录中的来宾用户或管理员可以重置密码。 以下示例演示来宾用户如何重置密码：
 
* 使用 Microsoft 帐户（例如 guestuser@live.com）登录的来宾用户可以使用 Microsoft 帐户自助密码重置 (SSPR) 来重置其自己的密码。 请参阅[如何重置 Microsoft 帐户密码](https://support.microsoft.com/help/4026971/microsoft-account-how-to-reset-your-password)。
* 使用外部标识提供者登录的来宾用户可以使用其标识提供者的 SSPR 方法来重置自己的密码。 
* 如果标识租户是实时 (JIT) 或“病毒性”租户（独立的不受管 Azure 租户），则只有来宾用户可以重置其密码。 有时，组织会接管员工在使用其工作电子邮件地址注册服务时创建的病毒性租户的管理。 组织接管病毒性租户后，只有该组织中的管理员可以重置用户密码或启用 SSPR。 如果需要，作为邀请方组织，你可以从目录中删除来宾用户帐户并重新发送邀请。

* 如果来宾用户的主目录是 Azure AD 租户，则你可以重置该用户的密码。 例如，你可能在本地 Active Directory 中创建了用户或同步了用户，并将其 UserType 设置为 Guest。 由于此用户位于你的目录中，因此，可以从 Azure 门户重置其密码。

### <a name="does-microsoft-dynamics-365-provide-online-support-for-azure-ad-b2b-collaboration"></a>Microsoft Dynamics 365 是否为 Azure AD B2B 协作提供联机支持？
是的，Dynamics 365 (Online) 支持 Azure AD B2B 协作。 有关详细信息，请参阅 Dynamics 365 文章[通过 Azure AD B2B 协作邀请用户](https://docs.microsoft.com/dynamics365/customer-engagement/admin/invite-users-azure-active-directory-b2b-collaboration)。

### <a name="what-is-the-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>新创建的 B2B 协作用户的初始密码的生存期是什么？
Azure AD 具有固定的字符集、密码强度和帐户锁定要求，同样适用于所有 Azure AD 云用户帐户。 云用户帐户是没有与其他标识提供者联合的帐户，例如 
* Active Directory 联合身份验证服务
* 其他云租户（用于 B2B 协作）

对于联合帐户，密码策略取决于在本地租户中应用的策略和用户的 Microsoft 帐户设置。

### <a name="an-organization-might-want-to-have-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-the-presence-of-the-identity-provider-claim-the-correct-model-to-use"></a>组织可能希望在其应用程序中为租户用户和来宾用户提供不同的体验。 是否有针对此事项的标准指南？ 标识提供者声明是否是可用的适当模型？
来宾用户可以使用任何标识提供者进行身份验证。 有关详细信息，请参阅 [B2B 协作用户的属性](user-properties.md)。 使用 **UserType** 属性确定用户体验。 令牌中当前未包括 **UserType** 声明。 应用程序应当使用 Microsoft Graph API 从目录中查询用户并获取其 UserType。

### <a name="where-can-i-find-a-b2b-collaboration-community-to-share-solutions-and-to-submit-ideas"></a>可在何处找到 B2B 协作社区以共享解决方案和提交意见？
我们会在改进 B2B 协作的过程中不断听取反馈。 请分享你的用户方案和最佳做法，并分享 Azure AD B2B 协作中让你钟意的方面。 欢迎在 [Microsoft 技术社区](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b)参与讨论。
 
我们还力邀你在 [B2B 协作意见](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas)中提交意见并为未来的功能投票。

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-the-user-is-just-ready-to-go-or-does-the-user-always-have-to-click-through-to-the-redemption-url"></a>是否可以发送自动兑换的邀请，以便用户只需“准备前往”即可？ 或者，用户始终需要单击到达兑换 URL？
可以使用 UI、PowerShell 脚本或 API 邀请合作伙伴组织中的其他用户。 然后，可以向来宾用户发送指向共享应用的直接链接。 在大多数情况下，不再需要打开电子邮件邀请并单击兑换 URL。 请参阅 [Azure Active Directory B2B 协作邀请兑换](redemption-experience.md)。

### <a name="how-does-b2b-collaboration-work-when-the-invited-partner-is-using-federation-to-add-their-own-on-premises-authentication"></a>受邀合作伙伴使用联合添加自己的本地身份验证时，B2B 协作如何工作？
如果合作伙伴具有联合到本地身份验证基础架构的 Azure AD 租户，则会自动实现本地单一登录 (SSO)。 如果合作伙伴没有 Azure AD 租户，则会为新用户创建 Azure AD 帐户。 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>Azure AD B2B 是否不接受 gmail.com 和 outlook.com 电子邮件地址，此类帐户是否使用 B2C？
我们将去除 B2B 和企业对消费者 (B2C) 的协作在所支持的标识这方面的差异。 根据所使用的标识决定使用 B2B 还是 B2C 并不是一个合理的原因。 有关选择协作选项的信息，请参阅[在 Azure Active Directory 中比较 B2B 协作和 B2C](compare-with-b2c.md)。

### <a name="can-an-azure-ad-b2c-local-account-be-invited-to-an-azure-ad-tenant-for-b2b-collaboration"></a>是否可以邀请 Azure AD B2C 本地帐户加入 Azure AD 租户进行 B2B 协作？
错误。 Azure AD B2C 本地帐户只能用于登录 Azure AD B2C 租户。 该帐户不能用于登录 Azure AD 租户。 不支持邀请 Azure AD B2C 本地帐户加入 Azure AD 租户来进行 B2B 协作。

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>哪些应用程序和服务支持 Azure B2B 来宾用户？
所有与 Azure AD 集成的应用程序都可以支持 Azure B2B 来宾用户，但必须使用设置为租户的终结点对来宾用户进行身份验证。 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>如果合作伙伴未启用多重身份验证，我们是否可以强制 B2B 来宾用户使用多重身份验证？
是的。 有关详细信息，请参阅 [B2B 协作用户的条件访问](conditional-access.md)。

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>在 SharePoint 中，可以为外部用户定义“允许”和“拒绝”列表。 在 Azure 中是否可以执行此操作？
是的。 Azure AD B2B 协作支持允许列表和拒绝列表。 

### <a name="what-licenses-do-we-need-to-use-azure-ad-b2b"></a>使用 Azure AD B2B 需要哪些许可证？
有关组织需要哪些许可证才能使用 Azure AD B2B 的信息，请参阅 [Azure Active Directory B2B 协作许可指南](licensing-guidance.md)。

### <a name="next-steps"></a>后续步骤

- [什么是 Azure AD B2B 协作？](what-is-b2b.md)


