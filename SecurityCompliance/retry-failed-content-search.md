---
title: "Retry a failed Content Search to resolve a content location error"
ms.author: markjjo
author: markjjo
manager: laurawi
ms.audience: Admin
ms.topic: article
ms.service: o365-administration
localization_priority: Normal
ms.collection: 
search.appverid:
- MOE150
- MET150
ms.assetid: 
description: "Use the Retry button for resolve Content Searches that have content location errors."
---

# Retry a failed Content Search to resolve a content location error

When you use Content Search in the Office 365 Security & Compliance Center to search a very large number of mailboxes (for example, searching 100,000 mailboxes or more in a single Content Search), you may get search errors that are similar to the following:

```
Error

The search on the following locations failed:

User1@contoso.com: Problem in processing the request. Please try again later. If you keep getting this error, contact your admin. (CS008-009)

User2@contoso.com: Application error occurred. Please try again later. (CS012-002)
```

These errors (with error codes of CS008-009 and CS012-002) indicate that Content Search failed to search specific content locations; in this example, two mailboxes weren't searched. These errors are displayed on the status details flyout page of the Content Search.

## Cause of content location errors

When searching a large number of mailboxes, the search is distributed across thousands of servers in a Microsoft datacenter. At any one time, specific servers could be in reboot state or in the process of failing over to redundant copies. In either of these cases, the Content Search's request to retrieve data will timeout. In the previous example, the errors for the mailboxes that failed were the result of the search timing out.

## Resolving content location errors

Restarting the search will often result in similar errors on different servers. Instead of restarting the search, click the **Retry** button that is displayed at the top of the search results page.

![Click the Retry button to resolve content location errors](media/retrycontentsearch3.png)

This will result in the retrying the search only for the mailboxes that failed. When you retry the search, the other results that were successfully returned are retained.

> [!NOTE]
> we're working on improving this experience so that the search will automatically retry failed mailboxes until all search results from all content locations are successfully returned.