---
title: "New Office 365 Message Encryption policy for sensitive information"
ms.author: robmazz
author: robmazz
manager: laurawi
ms.date: 12/13/2018
ROBOTS: NOINDEX, NOFOLLOW
audience: ITPro
ms.topic: article
ms.service: Office 365 Administration
localization_priority: None
search.appverid:
- MET150
ms.collection: Strat_O365_Enterprise
description: "Summary: New Office 365 Message Encryption policy for sensitive information."
---

# New Office 365 Message Encryption policy for sensitive information

We will be creating a new automatic policy in Office 365 tenants that will apply Office 365 Message Encryption to all emails that contain sensitive information and are being sent outside the organization. A new Exchange mail flow rule will be created in your Office 365 tenant so that your organization will be protected by default.

## How will this work?

Your organization will receive a notification in the Office 365 Message Center notifying you the date on which this automatic policy will be created in your tenant. You will be given at least a 30-day notice and you will also have the option to opt-out. A scenario in which you may want to potentially opt out is if you have a 3rd-party Data Loss Prevention solution that already scans for sensitive types.

## New policy details

A new Exchange mail flow rule will be created in your organization that will automatically encrypt emails going outside your organization with the *Encrypt-Only* policy if they contain the following sensitive information types:

- ABA routing number
- Credit card Number
- Drug Enforcement Agency (DEA) number
- U.S. / U.K. passport number
- U.S. bank account number
- U.S. Individual Taxpayer Identification Number (ITIN)
- U.S. Social Security Number (SSN)

> [!Note]
> The exact sensitive types may differ by your organization’s locale and will be communicated in the Message Center notification.

## What do I need to do to prepare for this change?

You do not have to update or modify any existing Office 365 configuration settings prior to this new change. However, you may want to update any applicable end-user documentation and training materials to prepare people in your organization for this change.

## How do I opt-out?

If you would like to opt-out of this change, please follow these steps:

1. Connect to [Exchange Online PowerShell](https://aka.ms/exopowershell) as a user with the global administrator role.
2.  Run the following code after authenticating:

    ```
    Set-IRMConfiguration -AutomaticServiceUpdateEnabled $false
    ```

## How do I change or disable the automatic policy?

If you didn’t opt-out of this change and the Exchange mail rule has already been created, you can [modify the rule](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/manage-mail-flow-rules#view-or-modify-a-mail-flow-rule) by going to **Mail flow** > **Rules** in the Exchange admin center (EAC) and modifying the rule “*Encrypt outbound sensitive emails (out of box rule)*”.
