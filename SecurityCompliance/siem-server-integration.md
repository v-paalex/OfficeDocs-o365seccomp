---
title: "SIEM server integration with Microsoft cloud services and applications"
ms.author: deniseb
author: denisebmsft
manager: laurawi
ms.audience: ITPro
ms.topic: article
ms.service: o365-solutions
localization_priority: Normal
ms.collection: Ent_O365
ms.custom: 
 - Ent_Solutions
 - SIEM
description: "Summary: Read this article to get an overview of SIEM server integration with the Microsoft cloud stack."
---

# SIEM server integration with Microsoft cloud services and applications

 **Summary:** If your organization is using a Security Information and Event Management (SIEM) server, or you are planning to get a SIEM server soon, you might be wondering how that'll integrate with your Microsoft cloud services, including Office 365 Enterprise. Read this article to get an overview of SIEM server integration and architecture.

## We already have a SIEM server. How do I integrate it?

If your organization is already using a SIEM server, integration with Microsoft cloud services and applications is typically done through either connectors or JSON files. The method you use depends on the kind of SIEM server you have. For example, Splunk offers an [Azure Monitor Add-on For Splunk](https://splunkbase.splunk.com/app/3534/), whereas ArcSight is integrated by using the [Azure Log Integration tool](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview) (AzLog). 

Contact your SIEM vendor to find out if they have a special connector you can use.


## We are planning to get a SIEM server. What do I need to know?

Whether you need a SIEM server depends on many factors, such as your organization's security requirements. Microsoft cloud services offer a variety of security features, but if your organization has content and applications on premises as well as in the cloud (as in a hybrid cloud deployment), you might consider adding a SIEM server for extra protection.


## Architecture for SIEM servers and Microsoft cloud services and applications

text text

 

## How SIEM servers integrate with Microsoft cloud services and applications

A SIEM server can receive information, such as alerts and audit logs, from a wide variety of Microsoft cloud services and applications, such as described in the following table:

| Microsoft Cloud Service or Application | SIEM server inputs | Resources to learn more |
| --- | --- | --- |
| Azure Security Center (Threat Protection and Threat Detection) | Alerts | [Azure Security data export to SIEM - Pipeline Configuration - Preview](https://docs.microsoft.com/azure/security-center/security-center-export-data-to-siem) |
| Azure Active Directory Identity Protection | Audit logs | [Integrate Azure Active Directory audit logs](https://docs.microsoft.com/azure/security/security-azure-log-integration-ad) |
| Azure Advanced Threat Analytics | Log integration | [ATA SIEM log reference](https://docs.microsoft.com/advanced-threat-analytics/cef-format-sa) |
| Windows Defender Advanced Threat Protection | Log integration | [Pull alerts to your SIEM tools](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-siem-windows-defender-advanced-threat-protection) |
| Microsoft Cloud App Security | Log integration | [SIEM integration with Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/siem) |
| Office 365 Cloud App Security | Log integration | [Integrate your SIEM server with Office 365 Cloud App Security](https://support.office.com/article/dd6d2417-49c4-4de6-9294-67fdabbf8532) |
| Office 365 Threat Intelligence | Audit logs | [SIEM integration with Office 365 Threat Intelligence](https://support.office.com/article/eb56b69b-3170-4086-82cf-ba40a530fa1b) |
| Office 365 Advanced Threat Protection (Email Gateway and Anti-malware) | Audit logs | [Office 365 Management Activity API reference](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference) |

<br/><br/>
    
**Join the discussion**

|**Contact us**|**Description**|
|:-----|:-----| 
|**What cloud adoption content do you need?** <br/> |We are creating content for cloud adoption that spans multiple Microsoft cloud platforms and services. Let us know what you think about our cloud adoption content, or ask for specific content by sending email to [cloudadopt@microsoft.com](mailto:cloudadopt@microsoft.com?Subject=[Cloud%20Adoption%20Content%20Feedback]:%20).  <br/> |
|**Join the cloud adoption discussion** <br/> |If you are passionate about cloud-based solutions, consider joining the Cloud Adoption Advisory Board (CAAB) to connect with a larger, vibrant community of Microsoft content developers, industry professionals, and customers from around the globe. To join, add yourself as a member of the [CAAB (Cloud Adoption Advisory Board) space](https://aka.ms/caab) of the Microsoft Tech Community and send us a quick email at [CAAB@microsoft.com](mailto:caab@microsoft.com?Subject=I%20just%20joined%20the%20Cloud%20Adoption%20Advisory%20Board!). Anyone can read community-related content on the [CAAB blog](https://blogs.technet.com/b/solutions_advisory_board/). However, CAAB members get invitations to private webinars that describe new cloud adoption resources and solutions.  <br/> |
|**Get the art you see here** <br/> |If you want an editable copy of the art you see in this article, we'll be glad to send it to you. Email your request, including the URL and title of the art, to [cloudadopt@microsoft.com](mailto:cloudadopt@microsoft.com?subject=[Art%20Request]:%20).  <br/> |
   
## See Also

[Cloud adoption and hybrid solutions](cloud-adoption-and-hybrid-solutions.md)
  
[Microsoft Cloud IT architecture resources](microsoft-cloud-it-architecture-resources.md)
  
[Cloud adoption Test Lab Guides (TLGs)](cloud-adoption-test-lab-guides-tlgs.md)
  
[Architectural models for SharePoint, Exchange, Skype for Business, and Lync](architectural-models-for-sharepoint-exchange-skype-for-business-and-lync.md)
  
[Hybrid solutions](hybrid-solutions.md)


