---
title: "SIEM server integration with Microsoft 365"
ms.author: deniseb
author: denisebmsft
manager: laurawi
ms.audience: ITPro
ms.topic: article
ms.date: 10/23/2018
ms.service: o365-solutions
localization_priority: Normal
ms.collection: Ent_O365
ms.custom: 
 - Ent_Solutions
 - SIEM
description: "Summary: Read this article to get an overview of SIEM server integration with the Microsoft 365."
---

# SIEM server integration with Microsoft 365 services and applications

## Overview

If your organization is using a Security Information and Event Management (SIEM) server, or if you are planning to get a SIEM server soon, you might be wondering how that'll integrate with your Microsoft 365, including Office 365 Enterprise. Read this article to get an overview of SIEM server integration

Whether you need a SIEM server depends on many factors, such as your organization's security requirements. Microsoft 365 services offer a variety of security features; however, if your organization has content and applications on premises and in the cloud (as in the case of a hybrid cloud deployment), you might consider adding a SIEM server for extra protection. Or, if your organization has particularly stringent security requirements you must meet, you might consider adding a SIEM server to your environment.

## SIEM server connectors and JSON files 

Integration with Microsoft 365 is typically done by using either connectors or JSON files. The method you use depends on what SIEM server you're using, and whether the SIEM server supplier offers a connector.

- Some SIEM vendors provide and support connectors that enable real-time event reporting. One example is Splunk, who offers an [Azure Monitor Add-on For Splunk](https://splunkbase.splunk.com/app/3534/).

- If a SIEM vendor does not provide a connector, you can configure your SIEM connection using the [Azure Log Integration tool](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview) (AzLog) and JSON files. One example is [ArcSight](https://software.microfocus.com/products/siem-security-information-event-management/overview).

Contact your SIEM server supplier to find out if they have a connector you can use.

## Audit logging must be turned on

Make sure audit logging is turned on before you configure SIEM server integration. 

- For SharePoint Online, OneDrive for Business, and Azure Active Directory, [audit logging is turned on in the Security & Compliance Center](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off).

- For Exchange Online, [audit logging is turned on with Windows PowerShell](https://docs.microsoft.com/office365/securitycompliance/enable-mailbox-auditing).

## SIEM server integration Microsoft 365

A SIEM server can receive data from a wide variety of Microsoft 365 services and applications. In some cases, such as Office 365 Advanced Threat Protection, SIEM server integration uses the Office 365 Management Activity API. In other cases, such as Cloud App Security, you set up a SIEM server agent and configure the connection. 

The following table lists several Microsoft 365 services and applications with SIEM server inputs and where to go to learn more about how SIEM server integration is configured. 

| Microsoft 365 Service or Application | SIEM server inputs | Resources to learn more |
| --- | --- | --- |
| [Office 365 Advanced Threat Protection](office-365-atp.md) <br/>   or   <br/>[Office 365 Threat Intelligence](office-365-ti.md) | Audit logs | [SIEM integration with Office 365 Threat Intelligence and Advanced Threat Protection](siem-integration-with-office-365-ti.md) |
| [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) | Log integration | [SIEM integration with Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/siem) |
| [Office 365 Cloud App Security](office-365-cas-overview.md) | Log integration | [Integrate your SIEM server with Office 365 Cloud App Security](integrate-your-siem-server-with-office-365-cas.md) |
| [Windows Defender Advanced Threat Protection](https://docs.microsoft.com/windows/security/threat-protection/) | Log integration | [Pull alerts to your SIEM tools](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-siem-windows-defender-advanced-threat-protection) |
| [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) (Threat Protection and Threat Detection) | Alerts | [Azure Security data export to SIEM - Pipeline Configuration - Preview](https://docs.microsoft.com/azure/security-center/security-center-export-data-to-siem) |
| [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview) | Audit logs | [Integrate Azure Active Directory audit logs](https://docs.microsoft.com/azure/security/security-azure-log-integration-ad) |
| [Azure Advanced Threat Analytics](https://docs.microsoft.com/azure/security/azure-threat-detection) | Log integration | [ATA SIEM log reference](https://docs.microsoft.com/advanced-threat-analytics/cef-format-sa) |

 
## See Also

[Cloud adoption and hybrid solutions](https://docs.microsoft.com/office365/enterprise/cloud-adoption-and-hybrid-solutions)
  
[Cloud adoption Test Lab Guides (TLGs)](https://docs.microsoft.com/office365/enterprise/cloud-adoption-test-lab-guides-tlgs)


