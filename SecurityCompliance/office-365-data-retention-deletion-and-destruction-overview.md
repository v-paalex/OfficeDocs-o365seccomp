---
title: "Data Retention, Deletion, and Destruction in Office 365"
ms.author: robmazz
author: robmazz
manager: laurawi
audience: ITPro
ms.topic: article
ms.service: Office 365 Administration
localization_priority: None
search.appverid:
- MET150
ms.collection: Strat_O365_Enterprise
description: "An overview of Microsoft's policies for Office 365 regarding data retention, deletion, and destruction."
---

# Data Retention, Deletion, and Destruction in Office 365

Microsoft has a Data Handling Standard policy for Office 365 that specifies how long customer data will be retained after being deleted. There are generally two scenarios in which customer data is deleted:

- **Active Deletion** - The tenant has an active subscription and a user deletes data, or data provided by a user is deleted by the administrator.
- **Passive Deletion** - The tenant subscription ends.

## Data Retention

For each of these deletion scenarios, the following table shows the maximum data retention period, by data classification:

| Data Category | Data Classification | Description | Examples | Retention Period |
|-----------------|-----------------|-----------------|----------------------------------|-------------------------------|
| Customer Data | Customer Content| Content directly provided/created by admins and users. This includes all text, sound, video, image files, and software created and stored in Microsoft data centers when using the services in Office 365. | Examples of the most commonly used Office 365 applications which allow users to author data include Word, Excel, PowerPoint, Outlook and OneNote. <br><br> Customer content also includes customer-owned/provided secrets (passwords, certificates, encryption keys, storage keys) | **Active Deletion Scenario:** at most 30 days <br><br> **Passive Deletion Scenario:** at most 180 days |
| Customer Data | End User Identifiable Information (EUII) | Data that identifies or could be used to identify the user of a Microsoft service. EUII does not contain Customer content | User name or display name (DOMAIN\UserName) <br><br> user principal name (name@domain) <br><br>  User-specific IP addresses | **Active Deletion Scenario:** at most 180 days (only a tenant administrator action) <br><br> **Passive Deletion Scenario:** at most 180 days |
| Personal Data <br> (Data not included in Customer Data) | End User Pseudonymous Identifiers (EUPI) | An identifier created by Microsoft tied to the user of a Microsoft service. When EUPI is combined with other information, such as a mapping table, it identifies the end user. EUPI does not contain information uploaded or created by the customer | User GUIDs, PUIDs, or SIDs <br><br> Session IDs | **Active Deletion Scenario:** at most 30 days <br><br> **Passive Deletion Scenario:** at most 180 days |

## Subscription Retention

At all times during the term of an active subscription, a subscriber can access, extract or delete customer data stored in Office 365. If a paid subscription ends or is terminated, Microsoft will retain customer data stored in Office 365 in a limited-function account for 90 days to enable the subscriber to extract the data. After the 90-day retention period ends, Microsoft will disable the account and delete the customer data. No more than 180 days after expiration or termination of a subscription to Office 365, Microsoft will disable the account and delete all customer data from the account. Once the maximum retention period for any data has elapsed, the data is rendered commercially unrecoverable.

In the case of a free trial, your account will move into a grace status for 30 days in most countries and regions. During this grace period, you have the option to purchase Office 365. If you decide not to buy Office 365, you can either cancel your trial or let the grace period expire, and your trial account information and data will be deleted.

## Expedited Deletion
At all times during the term of any subscription, a subscriber can contact Microsoft Support and request expedited subscription deprovisioning. In this process, all user data, including data in SharePoint Online, Exchange Online that may be under hold or stored in inactive mailboxes, is deleted three days after the administrator enters the lockout code provided by Microsoft. For more information on expedited deprovisioning, see [Cancel Office 365](https://support.office.com/article/Cancel-Office-365-for-business-b1bc0bef-4608-4601-813a-cdd9f746709a).

## Related Links
- [Data Destruction](office-365-data-destruction.md)
- [Immutability in Office 365](office-365-data-immutability.md)
- [Exchange Online Data Deletion](office-365-exchange-online-data-deletion.md)
- [SharePoint Online Data Deletion](office-365-sharepoint-online-data-deletion.md)
- [Skype for Business Data Deletion](office-365-skype-data-deletion.md)