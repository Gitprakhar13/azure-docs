---
title: Configure permission classifications
titleSuffix: Azure AD
description: Learn how to manage delegated permission classifications.
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 10/23/2021
ms.author: davidmu
ms.reviewer: arvindh, luleon, phsignor
ms.custom: contperf-fy21q2

#customer intent: As an admin, I want configure permission classifications for applications in Azure AD
---

# Configure permission classifications in Azure Active Directory

In this article you'll learn how to configure permissions classifications in Azure Active Directory (Azure AD). Permission classifications allow you to identify the impact that different permissions have according to your organization's policies and risk evaluations. For example, you can use permission classifications in consent policies to identify the set of permissions that users are allowed to consent to.

Currently, only the "Low impact" permission classification is supported. Only delegated permissions that don't require admin consent can be classified as "Low impact".

The minimum permissions needed to do basic sign in are `openid`, `profile`, `email`, `User.Read` and `offline_access`, which are all delegated permissions on the Microsoft Graph. With these permissions an app can read the full profile details of the signed-in user and can maintain this access even when the user is no longer using the app.

## Prerequisites

To configure permission classifications, you need:

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- One of the following roles: Global Administrator, Cloud Application Administrator, Application Administrator, or owner of the service principal.

## Manage permission classifications

# [Portal](#tab/azure-portal)

Follow these steps to classify permissions using the Azure portal:

1. Sign in to the [Azure portal](https://portal.azure.com) as a [Global Administrator](../roles/permissions-reference.md#global-administrator), [Application Administrator](../roles/permissions-reference.md#application-administrator), or [Cloud Application Administrator](../roles/permissions-reference.md#cloud-application-administrator)
1. Select **Azure Active Directory** > **Enterprise applications** > **Consent and permissions** > **Permission classifications**.
1. Choose **Add permissions** to classify another permission as "Low impact".
1. Select the API and then select the delegated permission(s).

In this example, we've classified the minimum set of permission required for single sign-on:

:::image type="content" source="media/configure-permission-classifications/permission-classifications.png" alt-text="Permission classifications":::

# [PowerShell](#tab/azure-powershell)

You can use the latest Azure AD PowerShell Preview module, [AzureADPreview](/powershell/module/azuread/?preserve-view=true&view=azureadps-2.0-preview), to classify permissions. Permission classifications are configured on the **ServicePrincipal** object of the API that publishes the permissions.

#### List the current permission classifications for an API

1. Retrieve the **ServicePrincipal** object for the API. Here we retrieve the ServicePrincipal object for the Microsoft Graph API:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Read the delegated permission classifications for the API:

   ```powershell
   Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId | Format-Table Id, PermissionName, Classification
   ```

#### Classify a permission as "Low impact"

1. Retrieve the **ServicePrincipal** object for the API. Here we retrieve the ServicePrincipal object for the Microsoft Graph API:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Find the delegated permission you would like to classify:

   ```powershell
   $delegatedPermission = $api.OAuth2Permissions | Where-Object { $_.Value -eq "User.ReadBasic.All" }
   ```

1. Set the permission classification using the permission name and ID:

   ```powershell
   Add-AzureADMSServicePrincipalDelegatedPermissionClassification `
      -ServicePrincipalId $api.ObjectId `
      -PermissionId $delegatedPermission.Id `
      -PermissionName $delegatedPermission.Value `
      -Classification "low"
   ```

#### Remove a delegated permission classification

1. Retrieve the **ServicePrincipal** object for the API. Here we retrieve the ServicePrincipal object for the Microsoft Graph API:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Find the delegated permission classification you wish to remove:

   ```powershell
   $classifications = Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId
   $classificationToRemove = $classifications | Where-Object {$_.PermissionName -eq "User.ReadBasic.All"}
   ```

1. Delete the permission classification:

   ```powershell
   Remove-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId `
       -Id $classificationToRemove.Id
   ```

---

## Next steps

To learn more:

- Go to [Permissions and consent in the Microsoft identity platform](../develop/v2-permissions-and-consent.md)
