---
title: Troubleshooting B2B collaboration - Azure Active Directory | Microsoft Docs
description: Remedies for common problems with Azure Active Directory B2B collaboration

services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/25/2017

ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.custom: "it-pro, seo-update-azuread-jan"
ms.collection: M365-identity-device-management
---

# Troubleshooting Azure Active Directory B2B collaboration

Here are some remedies for common problems with Azure Active Directory (Azure AD) B2B collaboration.


## I’ve added an external user but do not see them in my Global Address Book or in the people picker

In cases where external users are not populated in the list, the object might take a few minutes to replicate.

## A B2B guest user is not showing up in SharePoint Online/OneDrive people picker 
 
The ability to search for existing guest users in the SharePoint Online (SPO) people picker is OFF by default to match legacy behavior.

You can enable this feature by using the setting 'ShowPeoplePickerSuggestionsForGuestUsers' at the tenant and site collection level. You can set the feature using the Set-SPOTenant and Set-SPOSite cmdlets, which allow members to search all existing guest users in the directory. Changes in the tenant scope do not affect already provisioned SPO sites.

## Invitations have been disabled for directory

If you are notified that you do not have permissions to invite users, verify that your user account is authorized to invite external users under User Settings:

![External Users settings](media/troubleshoot/external-user-settings.png)

If you have recently modified these settings or assigned the Guest Inviter role to a user, there might be a 15-60 minute delay before the changes take effect.

## The user that I invited is receiving an error during redemption

Common errors include:

### Invitee’s Admin has disallowed EmailVerified Users from being created in their tenant

When inviting users whose organization is using Azure Active Directory, but where the specific user’s account does not exist (for example, the user does not exist in Azure AD contoso.com). The administrator of contoso.com may have a policy in place preventing users from being created. The user must check with their admin to determine if external users are allowed. The external user’s admin may need to allow Email Verified users in their domain (see this [article](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) on allowing Email Verified Users).

![Error stating this tenant does not allow email verified users to be added](media/troubleshoot/allow-email-verified-users.png)

### External user does not exist already in a federated domain

If you are using federation authentication and the user does not already exist in Azure Active Directory, the user cannot be invited.

To resolve this issue, the external user’s admin must synchronize the user’s account to Azure Active Directory.

## How does ‘\#’, which is not normally a valid character, sync with Azure AD?

“\#” is a reserved character in UPNs for Azure AD B2B collaboration or external users, because the invited account user@contoso.com becomes user_contoso.com#EXT#@fabrikam.onmicrosoft.com. Therefore, \# in UPNs coming from on-premises aren't allowed to sign in to the Azure portal. 

## I receive an error when adding external users to a synchronized group

External users can be added only to “assigned” or “Security” groups and not to groups that are mastered on-premises.

## My external user did not receive an email to redeem

The invitee should check with their ISP or spam filter to ensure that the following address is allowed: Invites@microsoft.com

## I notice that the custom message does not get included with invitation messages at times

To comply with privacy laws, our APIs do not include custom messages in the email invitation when:

- The inviter doesn’t have an email address in the inviting tenant
- When an appservice principal sends the invitation

If this scenario is important to you, you can suppress our API invitation email, and send it through the email mechanism of your choice. Consult your organization’s legal counsel to make sure any email you send this way also complies with privacy laws.

## A guest user with a just-in-time or "viral" tenant is unable to reset their password

If the identity tenant is a just-in-time (JIT) or viral tenant (meaning it's a separate, unmanaged Azure tenant), only the guest user can reset their password. Sometimes an organization will [take over management of viral tenants](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover) that are created when employees use their work email addresses to sign up for services. After the organization takes over a viral tenant, only an administrator in that organization can reset the user's password or enable SSPR. If necessary, as the inviting organization, you can remove the guest user account from your directory and resend an invitation.

## Next steps

- [Get support for B2B collaboration](get-support.md)
