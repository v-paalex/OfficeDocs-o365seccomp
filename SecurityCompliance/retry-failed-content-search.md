---
title: "Retry a failed Content Search caused by a content location error"
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
- MOE150
- MED150
- MET150
ms.assetid: 
description: ""
---

# Retry a failed Content Search caused by a content location error

When you use Content Search in the Office 365 Security & Compliance Center to search a very large number of mailboxes (for example, searching 100,000 mailboxes or more in a single search), you may get search errors that are similar to the following:

```
Error

The search on the following locations failed:

User1@contoso.com: Problem in processing the request. Please try again later. If you keep getting this error, contact your admin. (CS008-009)

User2@contoso.com: Application error occurred. Please try again later. (CS012-002)
```

These errors indicate that the search failed to search specific content location; in this example, two mailboxes were not searched. Note that these errors might be displayed on the status details page of the Content Search.

## Cause of content location errors

When searching a large number of mailboxes, the Content Search is distributed across thousands of servers in one or more Microsoft datacenters. At any one time, some of these servers could be in reboot state or in the process of failing over to redundant copies. In either of these cases, the Content Search's request to retrieve will timeout. In the example above, the content locations that failed were a results of search timing out. 

## Resolving content location errors

Restarting the search will often result in similar errors on different servers, instead, use the Retry button. This will cause the Content Search to only retry the failed mailboxes while keeping the remaining results intact.

## Note

We are working on a feature that will automatically retry failed mailboxes until all results are received.