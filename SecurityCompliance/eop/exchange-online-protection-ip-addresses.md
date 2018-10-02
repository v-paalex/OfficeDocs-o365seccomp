---
title: "Exchange Online Protection IP addresses"
ms.author: josephd
author: JoeDavies-MSFT
manager: laurawi
ms.date: 10/2/2018
ms.audience: ITPro
ms.topic: reference
ms.service: O365-seccomp
ms.custom: TN2DMC
localization_priority: Normal
ms.assetid: eb14f38b-7b55-4a47-84a0-4a56a59e4111

description: "The following Microsoft data center IP addresses are used by Microsoft Exchange Online Protection (EOP) when sending email, receiving email, or for Exchange Online Protection portal and administrative services. In order to send and receive messages from EOP or use the administrative services, make sure your network allows connections from these IP addresses."
---

# Exchange Online Protection IP addresses

The following Microsoft data center IP addresses are used by Microsoft Exchange Online Protection (EOP) when sending email, receiving email, or for Exchange Online Protection portal and administrative services. In order to send and receive messages from EOP or use the administrative services, make sure your network allows connections from these IP addresses.
 
> [!NOTE]
> Microsoft has developed a REST-based web service for the IP address and FQDN entries on this page. This new service helps you configure and update network perimeter devices such as firewalls and proxy servers. You can download the list of endpoints, the current version of the list, or specific changes. This service replaces the XML document, RSS feed, and the IP address and FQDN entries on this page. To try out this new service, go to [Web service](https://support.office.com/article/managing-office-365-endpoints-99cab9d4-ef59-4207-9f2b-3728eb46bf9a#webservice). 
 
## EOP IP address ranges

||||
|:-----|:-----|:-----|
|**IPv4 address ranges** <br/> |**IPv6 address ranges** <br/> |
| 23.103.132.0/22 | 2a01:111:f400:7c00::/54 |
| 23.103.136.0/21 | 2a01:111:f400:fc00::/54 |
| 23.103.144.0/20 | 2a01:111:f403::/48 |
| 23.103.198.0/23 |  |
| 23.103.200.0/22 |  |
| 40.92.0.0/14 |  |
| 40.107.0.0/17 |  |
| 52.100.0.0/14 |  |
| 52.238.78.88/32 |  |
| 65.55.88.0/24 |  |
| 65.55.169.0/24 |  |
| 94.245.120.64/26 |  |
| 104.47.0.0/17 |  |
| 157.55.234.0/24 |  |
| 157.56.110.0/23 |  |
| 157.56.112.0/24 |  |
| 207.46.100.0/24 |  |
| 207.46.163.0/24 |  |
| 213.199.154.0/24 |  |
| 213.199.180.128/26 |  |
| 216.32.180.0/23 |  |
||||
 
> [!IMPORTANT]
> The IP address ranges provided here are only used for relay through customer connectors. Changes to the list of IP addresses are rare, and are communicated in advance. To ensure that messages that you send to your business partners, a smart host, or an on-premises environment route through the service's published range of IP addresses, you must configure the correct connector for routing to each destination. For more information about connectors, see [Decide Which Connector to use](https://docs.microsoft.com/exchange/mail-flow-best-practices/use-connectors-to-configure-mail-flow/set-up-connectors-to-route-mail).  The IP addresses in this topic can change over time. For a record of all IP addresses that have been added, changed, or deprecated in the past year, see [Change notification for EOP IP addresses](change-notification-for-eop-ip-addresses.md). 
 
For information about IP addresses used by Microsoft Office 365, see [Office 365 URLs and IP address ranges](https://go.microsoft.com/fwlink/p/?LinkId=324165).

