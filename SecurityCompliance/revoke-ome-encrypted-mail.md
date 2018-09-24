---
title: "Revoke email encrypted by Office 365 Message Encryption"
ms.author: krowley
author: kccross
manager: laurawi
ms.date: 9/24/2018
ms.audience: Admin
ms.topic: conceptual
ms.service: o365-administration
localization_priority: Normal
search.appverid:
- MET150
description: "As an Office 365 administrator, you can revoke certain emails that were encrypted with Office 365 Message Encryption."
---

# Office 365 Message Encryption email revocation

This article is part of a larger series of articles about [Office 365 Message Encryption](ome.md). Right now, encrypted email revocation is in preview. Expect updates and changes to the feature and the content as we continue to improve our offering.

You may find it necessary to revoke an email that has already been sent. If the email was encrypted using Office 365 Message Encryption, and you are an Office 365 admin, you can do this for email under certain conditions. This article describes under what circumstances this is possible and how to do it.
  
## Encrypted emails that you can revoke
You can revoke encrypted emails if the recipient received a link-based, branded encrypted email. If the recipient received a native inline experience in a supported Outlook client, then those emails cannot be revoked.

Whether a recipient receives a link-based experience or an inline experience depends on the recipient identity type: 
Office 365 and Microsoft Account recipients (for example, outlook.com users) get an inline experience in supported Outlook clients.  
All other recipient types, such as Gmail recipients, get a link-based experience. 

Coming soon, organizations will have the ability to force a link-based experience regardless of the recipient identity. This way, all recipients will get a branded email with a link to the Office 365 Message Encryption portal where they will be able to read and reply to encrypted emails. All such encrypted emails will be revocable. 
  
## Recipient experience for revoked encrypted emails

Once an email has been revoked, the recipient will get an error when trying to access the encrypted email through the Office 365 Message Encryption portal: “The message has been revoked by the sender”.

![Screenshot that shows a revoked encrypted email.](media/revoked-encrypted-email.png)
    
## How to revoke an encrypted email

### Step 1. Obtain the Message ID of the email

Before you can revoke an encrypted mail you need to gather the Message ID of the mail. The MessageId is usually of the format:

`<xxxxxxxxxxxxxxxxxxxxxxx@xxxxxx.xxxx.prod.outlook.com>`  

There are multiple ways to find the Message ID of the email that you want to revoke. This section describes a couple of options, but you can use any method that provides the ID.

  #### To identify the Message ID of the email you want to revoke by using Message Trace in the Security &amp; Compliance Center

1. Search for the email by sender or recipient using [New Message Trace in Office 365 Security & Compliance Center](https://blogs.technet.microsoft.com/exchange/2018/05/02/new-message-trace-in-office-365-security-compliance-center/).
2. Once you've located the email select it to bring up the **Message trace details** pane. Expand **More Information** to locate the Message ID.

  #### To identify the Message ID of the email you want to revoke by using Office Message Encryption reports in the Security &amp; Compliance Center
1. In the Security &amp; Compliance Center, navigate to the **Message Encryption Report**.
2. Choose the **View details** table and identify the message that you want to revoke. 
3. Double-click the message to view details that include the Message ID. 

### Step 2. Revoke the mail  

Once you know the Message ID of the email you want to revoke, you can revoke the email by using the Set-OMEMessageRevocation cmdlet. 

1. [Connect to Exchange Online Using Remote PowerShell](http://technet.microsoft.com/library/jj984289%28v=exchg.150%29.aspx).
    
2. Run the Set-OMEMessageRevocation cmdlet as follows:
    
    ```
    Set-OMEMessageRevocation -Revoke $true -MessageId "<messageId>"
    ```  

3. To check whether the email was revoked, run the Get-OMEMessageStatus cmdlet as follows:
    
    ```
    Get-OMEMessageStatus -MessageId "<messageId>"
    ```  
    If revocation was successful, the cmdlet returns the following result:  

    ```The encrypted email with the subject "<subject>" and Message ID "<messageId>" was successfully revoked.```
