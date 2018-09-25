---
title: "Overview of file plan manager"
ms.author: stephow
author: stephow-MSFT
manager: laurawi
ms.date: 9/25/2018
ms.audience: Admin
ms.topic: overview
ms.service: o365-administration
localization_priority: Priority
ms.collection: Strat_O365_IP
search.appverid: 
- MOE150
- MET150
ms.assetid: af398293-c69d-465e-a249-d74561552d30
description: "File plan manager provides advanced management capabilities for retention labels and policies, and provides an integrated way to traverse label and label-to-content activity for your entire content lifecycle – from creation, through collaboration, record declaration, retention, and finally disposition."
---

# Overview of file plan manager

File plan manager provides advanced management capabilities for retention labels and policies, and provides an integrated way to traverse label and label-to-content activity for your entire content lifecycle – from creation, through collaboration, record declaration, retention, and finally disposition.

![File plan page](media/file-plan-page.png)

## Important: This feature is currently available only as part of the Office 365 Preview program

You will see this feature in your tenant only if your organization has enrolled in the Office 365 Preview program.

## Accessing file plan manager

There are two requirements to access file plan manager, they are:
- An Office 365 Enterprise E5 subscription.
- The user has been in assigned one of the following roles of the Security &amp; Compliance Center: 
    - Retention Manager
    - View-only Retention Manager

## Navigating your file plan

File plan manager makes it easier see into and across the settings of all your retention labels and policies from one view.

Note that retention labels created outside of the file plan will be available in the file plan and vice versa.

On the **file plan labels** tab, the following additional information and capabilities are available:

### Label settings columns
 
- **Based on** identifies the type of trigger that will start the retention period. Valid values are: 
    - Event
    - When created
    - When last modified
    - When labeled
- **Record** identifies if the item will become a declared record when the label is applied. Valid values are:
    - No
    - Yes
    - Yes(Regulatory)
- **Retention** identifies the retention type. Valid values are:
    - Keep
    - Keep and delete
    - Delete
- **Disposition** identifies what will happen to the content at the end of the retention period. Valid values are: 
    - null
    - No action
    - Auto-delete
    - Review required (aka Disposition review)

![Label settings in file plan](media/file-plan-label-columns.png)

### Label file plan descriptors columns

You can now include more information in the configuration of your retention labels. Inserting file plan descriptors into labels will improve the manageability and organization of your file plan.

To get you started, file plan manager provides some out-of-box values for: Function/department, Category, Authority type and Provision/citation. You can add new file plan descriptor values when creating or editing a retention label.

Here's a view of the file plan descriptors step when creating or editing a retention label.

![File plan descriptors](media/file-plan-descriptors.png)

Here's a view of the file plan descriptors columns on the labels tab of file plan manager.

![file-plan-descriptors-on-labels-tab.png](media/file-plan-descriptors-on-labels-tab.png)

## Export labels out of your file plan

From file plan manager, you can export the details of all retention labels into a .csv file to assist you in facilitating periodic compliance reviews with data governance stakeholders in your organization.

To export all retention labels, go to **file plan manager** \> **file plan actions** \> **export labels**.

![Option to export file plan](media/file-plan-export-labels-option.png)

A *.csv file containing all existing retention labels will open.

![CSV file showing all retention labels](media/file-plan-csv-file.png)

## Import labels into your file plan

From file plan manager, you can bulk import new labels as well as modify existing retention labels.

To import new retention labels and make updates existing retention labels, go to **file plan manager** \> **file plan actions** \> **import labels**.

![Option to import file plan](media/file-plan-import-labels-option.png)

![Option to download a blank file plan template](media/file-plan-blank-template-option.png)

Download a blank template (or start from an export of your current file plan).

![Blank file plan template open in Excel](media/file-plan-blank-template.png)

Fill-out the template (coming soon is reference information about valid values for entries).

![File plan template with information filled in](media/file-plan-filled-out-template.png)

Upload the filled-out template, and file plan manager will validate the entries and display import statistics.

![File plan import statistics](media/file-plan-import-statistics.png)

When the import is complete, return to file plan manager to assign new labels to new or existing policies.

![Option to publish labels](media/file-plan-publish-labels-option.png)

