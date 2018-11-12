---
title: "Search the Office 365 audit log to troubleshoot common scenarios"
ms.author: markjjo
author: markjjo
manager: laurawi
ms.date: 
ms.audience: Admin
ms.topic: article
ms.service: o365-administration
localization_priority: Normal
ms.collection: Strat_O365_IP
search.appverid:
- MET150
- MOE150
ms.assetid: 
description: "You can use the Office 365 audit log search tool to help you troubleshoot common issues such as inbox rules that forward email.
---

# Search the Office 365 audit log to troubleshoot common scenarios



## Using the Office 365 audit log search tool

## Find the IP address of the computer used to access a compromised account

IP address corresponding to an activity performed by the user or administrator is shown in the Audit Logs. The client information is also logged. Here are the steps to identifying such information.

Login to the Office 365 Security & Compliance center 
Click on “Search and Investigation” and select “Audit Log Search” 
If you are interested in a specific activity, select it from ‘Activities’ list. If not, all activities will be returned for the selected user (default setting). Note: Certain activities may not be available to be selected using the Activities menu, however those audit items will be returned if “Show Results for all activities” is selected (default setting) 
Specify the user name in the ‘Users’ field and select the appropriate date range for the activity and click on “Search” 
Once the results are displayed, the IP address for that activity can be seen in the results pane. 
To review the activity in detail, simply click on the Activity and detailed information will be shown in the right-hand side pane. This will provide more information such as Client, User that performed action etc.

## Determine if email forwarding has been set up for a user in your organization

Will help customers with how to identify forwarding set at the mailbox and how to look at audit logs on who/how forwarding was set at the mailbox level.

If external forwarding were set on a mailbox, the activity is audited as part of ‘Set-Mailbox’ cmdlet. We can see the activity using the audit log search in the Security and Compliance Center.

Login to the Office 365 Security & Compliance center 

Click on “Search and Investigation” and select “Audit Log Search” 

Specify date range by adjusting dates in Start and End date fields. You do not need to specify any username. 

Ensure that the Activities field is set to ‘Show results for all activities’. 
 
Click on ‘Search’ 

Once the results are loaded, click on ‘Filter Results’. Type ‘Set-mailbox’ in the activity filter text box. This should return all ‘Set-Mailbox’ activities.  

When you click on the activity, a blade opens up in the righthand side pane. Click on ‘More Information’ and under the Parameters section you can see the forwarding email address that was set on the mailbox. The UserID represents the user that set up external forwarding on the mailbox

## Determine if a user deleted email items

Help customers with enabling audit actions for delete events and how to review them using Unified Audit logs.

We are in the process of enabling mailbox auditing by default. Until then to review delete events for a certain user, that delete actions need to be enabled for auditing.  This article can help you with enabling mailbox auditing. If you had enabled mailbox auditing already, follow the steps below.

Login to the Office 365 Security & Compliance center 

Click on “Search and Investigation” and select “Audit Log Search: 

Specify date range by adjusting dates in Start and End date fields. 

Specify username for the user that you want to investigate. 

In the Activities field select the following: “Deleted messages from Deleted Items folder” , “Moved messages to Deleted Items folder”. 

When you click on the activity, a blade opens up in the righthand side pane. 

Click on ‘More Information’ and under the ‘Affected Items’ Section you can see the Subject of the deleted message, the original folder path of the message. 

The ClientInfoString property will show if the activity was performed from OWA or Outlook or any other device. 

Note: Admin or users can't retrieve deleted items using the audit log feature. In order to recover deleted email, users can follow instructions in the topic, [Recover deleted items or email in Outlook Web App](https://support.office.com/article/Recover-deleted-items-or-email-in-Outlook-Web-App-C3D8FC15-EEEF-4F1C-81DF-E27964B7EDD4).

## Determine if a user created an inbox rule for their Exchange Online mailbox

Creation, modification and deletion of inbox rules events are audited and can be reviewed using audit log search in the Security and Compliance center. Here are the instructions to find and review those events.

Login to the Office 365 Security & Compliance center 

Click on “Search and Investigation” and select “Audit Log Search” 

Specify date range by adjusting dates in Start and End date fields. Specify the user name for the user you would like to investigate. Ensure that the Activities field is set to ‘New-InboxRule
 
Create/modify/enable/disable inbox rule’ which can be found under Exchange Mailbox Activities.  
Click on ‘Search’ 

When you click on the activity, a blade opens up in the righthand side pane. Click on ‘More Information’ and under the Parameters section you can see the name of the rule, conditions set and action that the rule will take.