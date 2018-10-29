---
title: "Create a custom sensitive information type"
ms.author: stephow
author: stephow-MSFT
manager: laurawi
ms.date: 
ms.audience: Admin
ms.topic: article
ms.service: o365-administration
localization_priority: Priority
ms.collection: Strat_O365_IP
search.appverid: 
- MOE150
- MET150
ms.assetid: 
description: "Learn how to create, modify, remove, and test custom sensitive information types for DLP in the graphical users interface in Office 365 Security & Compliance Center."
---

# Create a custom sensitive information type

Data loss prevention (DLP) in Office 365 includes many [sensitive information types](what-the-sensitive-information-types-look-for.md) that are ready for you to use in your DLP policies. These built-in types can help identify and protect credit card numbers, bank account numbers, passport numbers, and more. 

But if you need to identify and protect a different type of sensitive information (for example, employee IDs or project numbers that uses a format specific to your organization) you can create a custom sensitive information type.

The fundamental parts of a custom sensitive information type are:

- **Primary pattern**: employee ID numbers, project numbers, etc. This is typically identified by a regular expression (RegEx), but it can also be a list of keywords.

- **Additional evidence**: Suppose you're looking for a nine-digit employee ID number. Not all nine-digit numbers are employee ID numbers, so you can look for additional text: keywords like "employee", "badge", "ID", or other text patterns based on additional regular expressions. This supporting evidence (also known as _supporting_ or _corroborative_ evidence) increases the likelihood that nine-digit number found in content is really an employee ID number.

- **Character proximity**: It makes sense that the closer the primary pattern and the supporting evidence are to each other, the more likely the detected content is going to be what you're looking for. You can specify the character distance between the primary pattern and the supporting evidence (also known as the _proximity window_) as shown in the following diagram:

    ![Diagram of corroborative evidence and proximity window](media/dc68e38e-dfa1-45b8-b204-89c8ba121f96.png)

- **Confidence level**: The more supporting evidence you have, the higher the likelihood that a match contains the sensitive information you're looking for. You can assign higher levels of confidence for matches that are detected by using more evidence.

  When satisfied, a pattern returns a count and confidence level, which you can use in the conditions in your DLP policies. When you add a condition for detecting a sensitive information type to a DLP policy, you can edit the count and confidence level as shown in the following diagram:

    ![Instance count and match accuracy options](media/11d0b51e-7c3f-4cc6-96d8-b29bcdae1aeb.png)

To create custom sensitive information types in the Office 365 Security & Compliance Center, you have the following options:

- **Use the graphical user interface**: This method is easier and faster, but you have less configuration options than PowerShell. The rest of this topic describes these procedures.

- **Use PowerShell**: This method requires that you first create an XML file (called a _rule package_) that contains one or more sensitive information types, and then you use PowerShell to import the rule package (importing the rule package is trivial compared to creating the rule package. This method is much more complex than the graphical user interface, but you have more configuration options. For instructions, see [Create a custom sensitive information type in Office 365 Security & Compliance Center PowerShell](create-a-custom-sensitive-information-type-in-scc-powershell.md).

The key differences are described in the following table:

|Sensitive information types in the UI|Sensitive information types in PowerShell|
|:-----|:-----|
|Name and Description are in one language|Supports multiple languages for Name and Description|
|Supports one pattern (the primary pattern).|Supports multiple patterns in addition to the primary pattern.|
|Supporting evidence can be: <br/>• Regular expressions <br/>• Keywords <br/>• Keyword dictionaries|Supporting evidence can be: <br/>• Regular expressions <br/>• Keywords <br/>• Keyword dictionaries <br/>• [Built-in DLP functions](what-the-dlp-functions-look-for.md)|
|Confidence level is configurable for the sensitive information type.|Confidence level is configurable for the sensitive information type and each individual pattern within.|
|Pattern match requires the detection of the primary pattern and all supporting evidence (the implicit AND operator is used).|Pattern match requires the detection of the primary pattern and a configurable amount of supporting evidence (implicit AND and OR operators can be used).|

## What do you need to know before you begin?

- To open the Security & Compliance Center, see [Go to the Office 365 Security & Compliance Center](go-to-the-securitycompliance-center.md).

- Custom sensitive information types require familiarity with regular expressions (RegEx). For additional information on the .NET RegEx engine that's used for processing the text, see [.NET Regular Expressions](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions).

  Microsoft Customer Service & Support can't assist with providing custom content-matching definitions (creating custom classifications or regular expression patterns). Support engineers can provide limited support for the feature (for example, providing sample regular expression patterns for testing purposes, or assisting with troubleshooting an existing regular expression pattern that's not triggering as expected), but can't provide assurances that any custom content-matching development will fulfill your requirements or obligations.

- For more information about the .NET RegEx engine that's used for processing text, see [Regular Expressions in .NET](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions).

- DLP uses the search crawler to identify and classify sensitive information in SharePoint Online and OneDrive for Business sites. To identify your new custom sensitive information type in existing content, the content must be recrawled. Content is recrawled based on a schedule, but you can manually recrawl content for a site collection, list, or library. For more information, see [Manually request crawling and re-indexing of a site, a library or a list](https://docs.microsoft.com/sharepoint/crawl-site-content).

## Create custom sensitive information types in the Security & Compliance Center

In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types** and click **Create**.

The settings are fairly self-evident, and are explained on the associate page of the wizard:

- **Name**

- **Description**

- **Proximity**

- **Confidence level**

- **Primary pattern element** (keywords, regular expression, or dictionary)

- Optional **Supporting pattern elements** (keywords, regular expression, or dictionary) and a corresponding **Minimum cost** value.

Here's a scenario: You want a custom sensitive information type that detects 9-digit employee numbers in content, along with the keywords "employee" "ID" and "badge". To create this custom sensitive information type, do the following steps:

1. In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types** and click **Create**.

2. In the **Choose a name and description** page that opens, enter the following values:

  - **Name**: Employee ID.

  - **Description** Detect nine-digit Contoso employee ID numbers.

  When you're finished, click **Next**.

3. In the **Requirements for matching** page that opens, click **Add an element** configure the following settings:

  - **Detect content containing**:
 
    a. Click **Any of these** and select **Regular expression**.

    b. In the regular expression box, enter `(\s)(\d{9})(\s)` (nine-digit numbers surrounded by white space)
  
  - **Supporting elements**: Click **Add supporting elements** and select **Contains this keyword list**.

  - In the **Contains this keyword list** area that appears, configure the following settings:

    - **Keyword list**: Enter the following value: employee,ID,badge.

    - **Minimum count**: Leave the default value 1.

  - Leave the default **Confidence level** value 60. 

  - Leave the default **Character proximity** value 300.

  When you're finished, click **Next**.

4. On the **Review and finalize** page that opens, review the settings and click **Finish**.

5. The next page encourages you to test the new custom sensitive information type. For more information, see [Test custom sensitive information types in the Security & Compliance Center](#test-custom-sensitive-information-types-in-the-security--compliance-center). Otherwise, click **Cancel**.

### How do you know this worked?

To verify that you've successfully created a new sensitive information type, do any of the following steps:

  - Go to **Classifications** \> **Sensitive info types** and verify the new custom sensitive information type is listed.

  - Test the new custom sensitive information type. For more information, see [Test custom sensitive information types in the Security & Compliance Center](#test-custom-sensitive-information-types-in-the-security--compliance-center).

## Modify custom sensitive information types in the Security & Compliance Center

**Note**: You can only modify custom sensitive information types; you can't modify built-in sensitive information types. But you can use PowerShell to export built-in custom sensitive information types, customize them, and import them as custom sensitive information types. For more information, see [Customize a built-in sensitive information type](customize-a-built-in-sensitive-information-type.md).

In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types** and select the custom sensitive information type that you want to modify.

The same options are available here as when you created the custom sensitive information type in the Security & Compliance Center. For more information, see [Create custom sensitive information types in the Security & Compliance Center](#create-custom-sensitive-information-types-in-the-security--compliance-center).

### How do you know this worked?

To verify that you've successfully modified a sensitive information type, do any of the following steps:

  - Go to **Classifications** \> **Sensitive info types** to verify the properties of the modified custom sensitive information type.

  - Test the modified custom sensitive information type. For more information, see [Test custom sensitive information types in the Security & Compliance Center](#test-custom-sensitive-information-types-in-the-security--compliance-center).

## Remove custom sensitive information types in the Security & Compliance Center 

**Notes**:

- You can only remove custom sensitive information types; you can't remove built-in sensitive information types.

- Before your remove a custom sensitive information type, verify that no DLP policies or Exchange mail flow rules (also known as transport rules) still reference the sensitive information type.

1. In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types** and select one or more custom sensitive information types that you want to remove.

2. In the fly-out that opens, click **Delete** (or **Delete sensitive info types** if you selected more than one).

3. In the warning message that appears, click **Yes**.

### How do you know this worked?

To verify that you've successfully removed a custom sensitive information type, go to **Classifications** \> **Sensitive info types** to verify the custom sensitive information type is no longer listed.


## Test custom sensitive information types in the Security & Compliance Center

1. In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types**.

2. Select one or more custom sensitive information types to test. In the fly-out that opens, click **Test type** (or **Test sensitive info types** if you selected more than one).

3. On the page that opens, upload a document to test by dragging and dropping a file or by clicking **Browse** and selecting a file.

4. Click the **Test** button to test the document for pattern matches in the file.
