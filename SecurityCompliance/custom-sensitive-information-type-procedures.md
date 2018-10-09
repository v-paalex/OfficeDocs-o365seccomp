---
title: "Procedures for custom sensitive information types in the Office 365 Security & Compliance Center"
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
description: "Learn how to add, modify and remove custom sensitive information types in the Office 365 Security & Compliance Center"
---

# Procedures for custom sensitive information types in the Office 365 Security & Compliance Center

A sensitive information type in Office 365 defines one or more patterns that data loss prevention (DLP) looks for when it evaluates email and documents. The Office 365 Security & Compliance Center comes with many [built-in sensitive information types](what-the-sensitive-information-types-look-for.md), but you can create your own for specific scenarios (for example, to dected content that contains your company's employee ID numbers). For more information, see __.

This topic describes the following procedures for managing custom sensitive information types in the Security & Compliance Center:

- Create custom sensitive information types.

- Modify custom senstive information types.

- Remove custom sensitive information types.

- Test custom sensitive information types.

## What do you need to know before you begin?

- To connect to Security & Compliance Center PowerShell, see [Connect to Office 365 Security & Compliance Center PowerShell](https://docs.microsoft.com/powershell/exchange/office-365-scc/connect-to-scc-powershell/connect-to-scc-powershell).

- To open the Security & Compliance Center, see [Go to the Office 365 Security & Compliance Center](go-to-the-securitycompliance-center.md).

- Custom sensitive information types require familiarity with regular expressions (RegEx). For additional information on the .NET RegEx engine that's used for processing the text, see [.NET Regular Expressions](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions).

  Microsoft Customer Service & Support can't assist with providing custom content-matching definitions (creating custom classifications or regular expression patterns). Support engineers can provide limited support for the feature (for example, providing sample regular expression patterns for testing purposes, or assisting with troubleshooting an existing regular expression pattern that's not triggering as expected), but can't provide assurances that any custom content-matching development will fulfill your requirements or obligations.

- For more information about the .NET RegEx engine that's used for processing text, see [Regular Expressions in .NET](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions).

## Create custom sensitive information types

### Use the Security & Compliance Center to create custom sensitive information types

In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types** and click **Create**.

The settings are fairly self-evident, and are explained on the associate page of the wizard:

- **Name**

- **Description**

- **Proximity**

- **Confidence level**

- **Primary pattern element** (keywords, regular expression, or dictionary)

- Optional **Supporting pattern elements** (keywords, regular expression, or dictionary) and a corresponding **Minimum cost** value.

For more information about these settings, see __.

Here's a scenario: You want a custom sensitive information type that detects 9-digit employee numbers in content, along with the keywords "employee" "ID" and "badge". To create this custom sensitive information type, do the following steps:

1. In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types** and click **Create**.

2. In the **Choose a name and description** page that opens, enter the following values:

  - **Name**: Employee ID.

  - **Description** Detect nine-digit Contoso employee ID numbers.

  When you're finished, click **Next**

3. In the **Requirements for matchng** page that opens, click **Add an element** configure the following settings:

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

### Use Security & Compliance Center PowerShell to create custom sensitive information types

Creating custom sensitive information types in PowerShell is really about creating a sensitive information type rule package (also known as a rule package). The rule package is a Unicode XML file that contains one or more sensitive information types. For more information about the rule package that you need to create, see ___.

After you create the rule package, you use Security & Compliance Center PowerShell to upload the rule package.

To upload rule packages, use the following syntax:

```
New-DlpSensitiveInformationTypeRulePackage -FileData (Get-Content -Path "PathToUnicodeXMLFile" -Encoding Byte)
```

This example uploads the Unicode XML file named MyNewRulePack.xml fron C:\My Documents.

```
New-DlpSensitiveInformationTypeRulePackage -FileData (Get-Content -Path "C:\My Documents\MyNewRulePack.xml" -Encoding Byte)
```

For detailed syntax and parameter information, see [New-DlpSensitiveInformationTypeRulePackage](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-dlp/new-dlpsensitiveinformationtyperulepackage).

**Notes**:

- When you upload your rule package, the system validates the XML and checks for known bad patterns and obvious performance issues. For more information, see __.

- DLP uses the search crawler to identify and classify sensitive information in SharePoint Online and OneDrive for Business sites. To identify your new custom sensitive information type in existing content, the content must be recrawled. Content is recrawled based on a schedule, but you can manually recrawl content for a site collection, list, or library. For more information, see [Manually request crawling and re-indexing of a site, a library or a list](https://docs.microsoft.com/sharepoint/crawl-site-content).

### How do you know this worked?

To verify that you've successfully created a new sensitive information type, do any of the following steps:

- In the Security & Compliance Center:

  - Go to **Classifications** \> **Sensitive info types** and verify the new custom sensitive information type is listed.

  - Test the new custom sensitive information type. For more information, see [Use the Security & Compliance Center to test custom sensitive information types](#use-the-security--compliance-center-to-test-custom-sensitive-information-types).

- In Security & Compliance Center PowerShell:

  - Run the following command and verify the new rule package is listed:

    ```
    Get-DlpSensitiveInformationTypeRulePackage
    ``` 

  - Run the following command and verify the sensitive information type is listed:

    ```
    Get-DlpSensitiveInformationType
    ``` 

    For custom sensitive information types, the Publisher property value will be something other than Microsoft Corporation.

  - Replace \<Name\> with the Name value of the sensitive information type (for example, Employee ID) and run the following command:

    ```
    Get-DlpSensitiveInformationType -Identity "<Name>"
    ```

## Modify custom sensitive information types

**Note**: You can only modify customer sensitive information types; you can't modify built-in sensitive information types. But, you can use PowerShell to export built-in custom sensitive information types, customize them, and import them as custom senstive information types. For more information, see [Customize a built-in sensitive information type](customize-a-built-in-sensitive-information-type.md).

### Use the Security & Compliance Center to modify custom sensitive information types

In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types** and select the custom sensitive information type that you want to modify.

The same options are available here as when you created the custom sensitive information type in the Security & Compliance Center. For more information, see [Use the Security & Compliance Center to create custom sensitive information types](#use-the-security--compliance-center-to-create-custom-sensitive-information-types).

### Use Security & Compliance Center PowerShell to modify custom sensitive information types

In Security & Compliance Center PowerShell, modifying a custom sensitive information type requires you to:

1. Export the existing rule package that contains the custom sensitive information type to an XML file. 

2. Modify the custom sensitive information type in the exported XML file.

3. Import the updated XML file back into the existing rule package.

#### Step 1: Export the existing rule package to an XML file

1. Run the following command to find the name of the custom rule package:

    ```
    Get-DlpSensitiveInformationTypeRulePackage
    ```

    The built-in rule package that contains the built-in sensitive information types is named Microsoft Rule Package. The rule package that contains the custom sensitive information types that you created in the Security & Compliance Center is named Microsoft.SCCManaged.CustomRulePack.

2. Use the following syntax to store the custom rule package to a variable:

    ```
    $rulepak = Get-DlpSensitiveInformationTypeRulePackage -Identity "RulePackageName"
    ```

   For example, if the name of the rule package is "Employee ID Custom Rule Pack", run the following command:

    ```
    $rulepak = Get-DlpSensitiveInformationTypeRulePackage -Identity "Employee ID Custom Rule Pack"
    ```

3. Use the following syntax to export the custom rule package to an XML file:

    ```
    Set-Content -Path "XMLFileAndPath" -Encoding Byte -Value $rulepak.SerializedClassificationRuleCollection
    ```

    This example export the rule package to the file named ExportedRulePackage.xml in the C:\My Documents folder.

    ```
    Set-Content -Path "C:\My Documents\ExportedRulePackage.xml" -Encoding Byte -Value $rulepak.SerializedClassificationRuleCollection
    ```

#### Step 2: Modify the sensitive information type in the exported XML file

For more information about the sensitive information types in the XML file and other elements in the file, see __.

#### Step 3: Import the updated XML file back into the existing rule package

To import the updated XML back into the existing rule package, use the following syntax:

```
Set-DlpSensitiveInformationTypeRulePackage -Identity "RulePackageIdentity" -FileData (Get-Content -Path "PathToUnicodeXMLFile" -Encoding Byte)
```

You can use the Name value (for any language) or the `RulePack id` (GUID) value to identify the rule package.

This example uploads the updated Unicode XML file named MyUpdatedRulePack.xml fron C:\My Documents into the existing rule package named "Employee ID Custom Rule Pack".

```
Set-DlpSensitiveInformationTypeRulePackage -Identity "Employee ID Custom Rule Pack" -FileData (Get-Content -Path "C:\My Documents\MyUpdatedRulePack.xml" -Encoding Byte)
```

For detailed syntax and parameter information, see [Set-DlpSensitiveInformationTypeRulePackage](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-dlp/set-dlpsensitiveinformationtyperulepackage).


### How do you know this worked?

To verify that you've successfully modified a sensitive information type, do any of the following steps:

- In the Security & Compliance Center: 

  - Go to **Classifications** \> **Sensitive info types** to verify the properties of the modified custom sensitive information type.

  - Test the modified custom sensitive information type. For more information, see [Use the Security & Compliance Center to test custom sensitive information types](#use-the-security--compliance-center-to-test-custom-sensitive-information-types).

- In Security & Compliance Center PowerShell:

  - Replace RulePackageName with the name of the modified rule package (for example, Employee ID Custom Rule Pack) and run the following command:

    ```
    Get-DlpSensitiveInformationTypeRulePackage -Identity "Employee ID Custom Rule Pack" | Format-List
    ``` 

  - Replace \<Name\> with the Name value of a sensitive information type (for example, Employee ID) within the modified rule packed and run the following command to verify the properties of the sensitive information type:

    ```
    Get-DlpSensitiveInformationType -Identity "<Name>" | Format-List
    ```

## Remove custom sensitive information types

**Notes**:

- You can only remove custom sensitive information types; you can't remove built-in sensitive information types.

- Before your remove a custom sensitive information type, verify that no DLP policies or Exchange mail flow rules (also known as transport rules) still reference the sensitive information type.

### Use the Security & Compliance Center to remove custom senstive information types

1. In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types** and select one or more custom sensitive information types that you want to remove.

2. In the fly-out that opens, click **Delete** (or **Delete sensitive info types** if you selected more than one).

3. In the warning message that appears, click **Yes**.

### Use Security & Compliance Center PowerShell to remove custom senstive information types

In Security & Compliance Center PowerShell, there are two methods to remove custom sensitive information types:

- **Remove individual custom sensitive information types**: Use the method documented in [Use Security & Compliance Center PowerShell to modify custom sensitive information types](#use-security--compliance-center-powershell-to-modify-custom-sensitive-information-types). You export the custom rule package that contains the custom sensitive information type, remove the sensitive information type from the XML file, and import the updated XML file back into the existing custom rule package.

- **Remove a custom rule package and all custom sensitive information types that it contains**: This method is documented here.

To remove a custom rule package, use the following syntax:

```
Remove-DlpSensitiveInformationTypeRulePackage -Identity "RulePackageIdentity"
```

You can use the Name value (for any language) or the `RulePack id` (GUID) value to identify the rule package.

This example removes the rule package named "Employee ID Custom Rule Pack".

```
Remove-DlpSensitiveInformationTypeRulePackage -Identity "Employee ID Custom Rule Pack"
```

For detailed syntax and parameter information, see [Remove-DlpSensitiveInformationTypeRulePackage](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-dlp/remove-dlpsensitiveinformationtyperulepackage).

### How do you know this worked?

To verify that you've successfully removed a custom sensitive information type, do any of the following steps:

- In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types** to verify the custom sensitive information type is no longer listed.

- In Security & Compliance Center PowerShell:

  - Run the following command and verify the rule package is no longer listed:

    ```
    Get-DlpSensitiveInformationTypeRulePackage
    ``` 

  - Run the following command and verify the sensitive information types in the removed rule package are no longer listed:

    ```
    Get-DlpSensitiveInformationType
    ``` 

    For custom sensitive information types, the Publisher property value will be something other than Microsoft Corporation.

  - Replace \<Name\> with the Name value of the sensitive information type (for example, Employee ID) and run the following command to verify the sensitive information type is no longer listed:

    ```
    Get-DlpSensitiveInformationType -Identity "<Name>"
    ```

## Use the Security & Compliance Center to test custom sensitive information types 

1. In the Security & Compliance Center, go to **Classifications** \> **Sensitive info types**.

2. Select one or more custom sensitive information types to test. In the fly-out that opens, click **Test type** (or **Test sensitive info types** if you selected more than one).

3. On the page that opens, upload a document to test by dragging and droping a file or by clicking **Browse** and selecting a file.

4. Click the **Test** button to test the document for pattern matches in the file.
