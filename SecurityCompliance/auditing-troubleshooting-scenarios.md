---
title: "Search the Office 365 audit log to troubleshoot common scenarios"
ms.author: markjjo
author: markjjo
manager: laurawi
ms.audience: Admin
ms.topic: article
ms.service: o365-administration
localization_priority: Normal
ms.collection: Strat_O365_IP
search.appverid:
- MET150
- MOE150
description: "You can use the Office 365 audit log search tool to help you troubleshoot common issues such as inbox rules that forward email."
---

# Search the Office 365 audit log to troubleshoot common scenarios



## Using the Office 365 audit log search tool

Each of the troubleshooting scenarios in this article are based on using the Office 365 audit log search tool in the Office 365 Security & Compliance Center. This section lists the permissions requirements to search the audit log and describes the steps to access and run audit log searches. Each scenario provides specific guidance about how to configure the search query and what to look for in the detail information in the audit records that match the search criteria.

### Permissions required to use the audit log search tool

You have to be assigned the View-Only Audit Logs or Audit Logs role in Exchange Online to search the Office 365 audit log. By default, these roles are assigned to the Compliance Management and Organization Management role groups on the **Permissions** page in the Exchange admin center. For more information, see [Manage role groups in Exchange Online](https://go.microsoft.com/fwlink/p/?LinkID=730688).

### Running audit log searches

This section describes the basics for creating and running audit log searches. Use these instructions as a starting point for each troubleshooting scenario in this article. For more detailed step-by-step instructions, see [Search the audit log in the Office 365 Security & Compliance Center ](search-the-audit-log-in-security-and-compliance.md#step-1-run-an-audit-log-search).

1. Go to [https://protection.office.com](https://protection.office.com).
  
2. Sign in to Office 365 using your work or school account.

3. In the left pane of the Security &amp; Compliance Center, click **Search & investigation** > **Audit log search**.
    
    The **Audit log search** page is displayed. 
    
    ![Configure criteria and then click Search to run the search](media/8639d09c-2843-44e4-8b4b-9f45974ff7f1.png)
  
4. You can configure the following search criteria. Note that each troubleshooting scenario in this article will recommend specific guidance for configuring these fields.
    
    a. **Activities** - Click the drop-down list to display the activities that you can search for. After you run the search, only the audit log entries for the selected activities are displayed. Selecting **Show results for all activities** will display results for all activities that meet the other search criteria. You'll also have to leave this field blank return audit records from the Exchange admin audit log.
    
    b. **Start date** and **End date** - Select a date and time range to display the events that occurred within that period. The last seven days are selected by default. The date and time are presented in Coordinated Universal Time (UTC) format. The maximum date range that you can specify is 90 days.

    c. **Users** - Click in this box and then select one or more users to display search results for. The audit log entries for the selected activity performed by the users you select in this box are displayed in the list of results. Leave this box blank to return entries for all users (and service accounts) in your organization. 
    
    d. **File, folder, or site** - Type some or all of a file or folder name to search for activity related to the file of folder that contains the specified keyword. You can also specify a URL of a file or folder. If you use a URL, be sure the type the full URL path or if you just type a portion of the URL, don't include any special characters or spaces. Leave this box blank to return entries for all files and folders in your organization.
    
5. Click **Search** to run the search using your search criteria. 
    
    The search results are loaded, and after a few moments they are displayed under **Results** on the **Audit log search** page. Each to the following sections will provide guidance about things to look for the specific troubleshooting scenario. 

    For more information about viewing, filtering, or exporting audit log search results, see: 

    - [View search results](search-the-audit-log-in-security-and-compliance.md#step-2-view-the-search-results)
    - [Filter search results](search-the-audit-log-in-security-and-compliance.md#step-3-filter-the-search-results)
    - [Export search results](search-the-audit-log-in-security-and-compliance.md#step-4-export-the-search-results-to-a-file)

## Finding the IP address of the computer used to access a compromised account

IP address corresponding to an activity performed by the user or administrator is shown in the Audit Logs. The client information is also logged. Here are the steps to identifying such information.

Login to the Office 365 Security & Compliance center 
Click on “Search and Investigation” and select “Audit Log Search” 
If you are interested in a specific activity, select it from ‘Activities’ list. If not, all activities will be returned for the selected user (default setting). Note: Certain activities may not be available to be selected using the Activities menu, however those audit items will be returned if “Show Results for all activities” is selected (default setting) 
Specify the user name in the ‘Users’ field and select the appropriate date range for the activity and click on “Search” 
Once the results are displayed, the IP address for that activity can be seen in the results pane. 
To review the activity in detail, simply click on the Activity and detailed information will be shown in the right-hand side pane. This will provide more information such as Client, User that performed action etc.

## Determining if email forwarding has been set up for a user

Will help customers with how to identify forwarding set at the mailbox and how to look at audit logs on who/how forwarding was set at the mailbox level.

If external forwarding were set on a mailbox, the activity is audited as part of ‘Set-Mailbox’ cmdlet. We can see the activity using the audit log search in the Security and Compliance Center.

Login to the Office 365 Security & Compliance center 

Click on “Search and Investigation” and select “Audit Log Search” 

Specify date range by adjusting dates in Start and End date fields. You do not need to specify any username. 

Ensure that the Activities field is set to ‘Show results for all activities’. 
 
Click on ‘Search’ 

Once the results are loaded, click on ‘Filter Results’. Type ‘Set-mailbox’ in the activity filter text box. This should return all ‘Set-Mailbox’ activities.  

When you click on the activity, a blade opens up in the righthand side pane. Click on ‘More Information’ and under the Parameters section you can see the forwarding email address that was set on the mailbox. The UserID represents the user that set up external forwarding on the mailbox

## Determining if a user deleted email items

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

## Determining if a user created an inbox rule

Creation, modification and deletion of inbox rules events are audited and can be reviewed using audit log search in the Security and Compliance center. Here are the instructions to find and review those events.

Login to the Office 365 Security & Compliance center 

Click on “Search and Investigation” and select “Audit Log Search” 

Specify date range by adjusting dates in Start and End date fields. Specify the user name for the user you would like to investigate. Ensure that the Activities field is set to ‘New-InboxRule
 
Create/modify/enable/disable inbox rule’ which can be found under Exchange Mailbox Activities.  
Click on ‘Search’ 

When you click on the activity, a blade opens up in the righthand side pane. Click on ‘More Information’ and under the Parameters section you can see the name of the rule, conditions set and action that the rule will take.