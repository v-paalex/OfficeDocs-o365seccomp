---
title: "Custom sensitive information types in the Office 365 Security & Compliance Center"
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
ms.assetid: 82c382a5-b6db-44fd-995d-b333b3c7fc30
description: "Learn about custom sentisive information types for DLP in the Office 365 Security & Compliance Center."
---

# Custom sensitive information types in the Office 365 Security & Compliance Center

Data loss prevention (DLP) in Office 365 includes many [sensitive information types](what-the-sensitive-information-types-look-for.md) that are ready for you to use in your DLP policies. These built-in types can help identify and protect credit card numbers, bank account numbers, passport numbers, and more. 

But if you need to identify and protect a different type of sensitive information (for example, employee IDs or project numbers that uses a format specific to your organization) you can create a custom sensitive information type.

The fundamental parts of a custom sensitive information type are:

- **Primary pattern**: employee ID numbers, project numbers, etc. This is typically identified by a regular expression (RegEx), but it can also be a list of keywords.

- **Additional evidence**: Suppose you're looking for a nine-digit employee ID number. Not all nine-digit numbers are employee ID numbers, so you can look for additional text: kewords like "employee", "badge", "ID", or other text patterns based on additional regular expressions. This supporting evidence (also known as _supporting_ or _corroborative_ evidence) increases the likelyhood that nine-digit number found in content is really an employee ID number.

- **Character proximity**: It makes sense that the closer the primary pattern and the supporting evidence are to each other, the more likely the detected content is going to be what you're looking for. You can specify the character distance between the primary pattern and the supporting evidence (also known as the _proximity window_) as shown in the following diagram:

    ![Diagram of corroborative evidence and proximity window](media/dc68e38e-dfa1-45b8-b204-89c8ba121f96.png)

- **Confidence level**: The more supporting evidence you have, the higher the likelyhood that a match contains the sensitive information you're looking for. You can assign higher levels of confidence for matches that are detected by using more evidence.

  When satisfied, a pattern returns a count and confidence level, which you can use in the conditions in your DLP policies. When you add a condition for detecting a sensitive information type to a DLP policy, you can edit the count and confidence level as shown in the following diagram:

    ![Instance count and match accuracy options](media/11d0b51e-7c3f-4cc6-96d8-b29bcdae1aeb.png)

To create custom sensitive information types in the Office 365 Security & Compliance Center, you have the following options:

- **Use the graphical user interface**: This method is easier and faster, but you have less configuration options than PowerShell.

- **Use PowerShell**: This method requires that you first create an XML file (called a _rule package_) that contains one or more sensitive information types, and then you use use PowerShell to import the rule package (importing the rule package is trivial compared to creating the rule package). This method is much more complex than the graphical user interface, but you have more configuration options.

The key differences are described in the following table:

|Sensitive information types in the UI|Sensitive information types in PowerShell|
|:-----|:-----|
|Name and Description are in one language|Supports mulitple languages for Name and Description|
|Supports one pattern (the primary pattern).|Supports multiple patterns in addition to the primary pattern.|
|Supporting evidence can be: <br/>• Regular expressions <br/>• Keywords <br/>• Keyword dictionaries|Supporting evidence can be: <br/>• Regular expressions <br/>• Keywords <br/>• Keyword dictionaries <br/>• [Built-in DLP functions](what-the-dlp-functions-look-for.md)|
|Confidence level is configurable for the sensitive information type.|Confidence leve is configurable for the sensitive information type and each individual pattern within.|
|Pattern match requires the dectection of the primary pattern and all supporting evidence (the implicit AND operator is used).|Pattern match requires the detection of the primary pattern and a configurable amount of supporting evidence to be detected (implicit AND and OR operators can be used).|

For custom sensitive information type procedures, see [Procedures for custom sensitive information types in the Office 365 Security & Compliance Center](custom-sensitive-information-type-procedures.md).

The rest of this topic explains the XML file that's required if you want to create new custom sensitive information types in Security & Compliance Center PowerShell. You can use the example XML file in this topic as a starting point for your own XML file.

**Note**: Custom sensitive information types require familiarity with regular expressions (RegEx). For additional information on the .NET RegEx engine that's used for processing the text, see [.NET Regular Expressions](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions).

## Important disclaimer

Due to the variances in customer environments and content match requirements, Microsoft Support cannot assist in providing custom content-matching definitions; e.g., defining custom classifications or regular expression (also known as RegEx) patterns. For custom content-matching development, testing, and debugging, Office 365 customers will need to rely upon internal IT resources, or use an external consulting resource such as Microsoft Consulting Services (MCS). Support engineers can provide limited support for the feature, but cannot provide assurances that any custom content-matching development will fulfill the customer's requirements or obligations. As an example of the type of support that can be provided, sample regular expression patterns may be provided for testing purposes. Or, support can assist with troubleshooting an existing RegEx pattern which is not triggering as expected with a single specific content example.

## Sample XML of a custom rule package that contains sensitive information types

Here's the sample XML of the custm rule package that we'll explore in this topic. This sample XML file defines a custom sesitive information type that detects nine-digit employee ID numbers. The XML elements and attributes are explained in the remaining sections.

```
<?xml version="1.0" encoding="UTF-16"?>
<RulePackage xmlns="http://schemas.microsoft.com/office/2011/mce">
<RulePack id="DAD86A2-AB18-43BB-AB35-96F7C594ADAA">
	<Version build="0" major="1" minor="0" revision="0"/>
	<Publisher id="619DD8C3-7B80-4998-A312-4DF0402BAC04"/>
	<Details defaultLangCode="en-us">
		<LocalizedDetails langcode="en-us">
			<PublisherName>Contoso</PublisherName>
			<Name>Employee ID Custom Rule Pack</Name>
			<Description>This rule package contains the custom Employee ID entity.</Description>
		</LocalizedDetails>
	</Details>
</RulePack>
<Rules>
<!-- Employee ID -->
	<Entity id="E1CC861E-3FE9-4A58-82DF-4BD259EAB378" patternsProximity="300" recommendedConfidence="70">
		<Pattern confidenceLevel="60">
			<IdMatch idRef="Regex_employee_id"/>
		</Pattern>
		<Pattern confidenceLevel="70">
			<IdMatch idRef="Regex_employee_id"/>
			<Match idRef="Func_us_date"/>
		</Pattern>
		<Pattern confidenceLevel="80">
			<IdMatch idRef="Regex_employee_id"/>
			<Match idRef="Func_us_date"/>
			<Any minMatches="1">
				<Match idRef="Keyword_badge" minCount="2"/>
				<Match idRef="Keyword_employee"/>
			</Any>
			<Any minMatches="0" maxMatches="0">
				<Match idRef="Keyword_false_positives_local"/>
				<Match idRef="Keyword_false_positives_intl"/>
			</Any>
		</Pattern>
	</Entity>
	<Regex id="Regex_employee_id">(\s)(\d{9})(\s)</Regex>
	<Keyword id="Keyword_employee">
		<Group matchStyle="word">
			<Term>Identification</Term>
			<Term>Contoso Employee</Term>
		</Group>
	</Keyword>
	<Keyword id="Keyword_badge">
		<Group matchStyle="string">
			<Term>card</Term>
			<Term>badge</Term>
			<Term caseSensitive="true">ID</Term>
		</Group>
	</Keyword>
	<Keyword id="Keyword_false_positives_local">
		<Group matchStyle="word">
			<Term>credit card</Term>
			<Term>national ID</Term>
		</Group>
	</Keyword>
	<Keyword id="Keyword_false_positives_intl">
		<Group matchStyle="word">
			<Term>identity card</Term>
			<Term>national ID</Term>
			<Term>EU debit card</Term>
		</Group>
	</Keyword>
	<LocalizedStrings>
		<Resource idRef="E1CC861E-3FE9-4A58-82DF-4BD259EAB378">
			<Name default="true" langcode="en-us">Employee ID</Name>
			<Description default="true" langcode="en-us">
			A custom classification for detecting Employee IDs.
			</Description>
			<Name default="true" langcode="de-de">Name for German locale</Name>
			<Description default="true" langcode="de-de">
			Description for German locale.
			</Description>
		</Resource>
	</LocalizedStrings>
</Rules>
</RulePackage>
```

## Explanation of the XML file

### Basic custom rule package identifiers [RulePackage element]

The custom rule package requires some basic identitfying information that's basically independent of the custom sensitive information types. You can use the following markup as a template and replace the ". . ." placeholders with your own values. The diagram at the end of this section shows a sample of what your values will look like.

```
<?xml version="1.0" encoding="utf-16"?>
<RulePackage xmlns="http://schemas.microsoft.com/office/2011/mce">
  <RulePack id=". . .">
    <Version major="1" minor="0" build="0" revision="0" />
    <Publisher id=". . ." /> 
    <Details defaultLangCode=". . .">
      <LocalizedDetails langcode=" . . . ">
         <PublisherName>. . .</PublisherName>
         <Name>. . .</Name>
         <Description>. . .</Description>
      </LocalizedDetails>
    </Details>
  </RulePack>
</RulePackage>
```

**Notes**

- You need to generate a globally unique identifier (GUID) for the custom rule package tself (the `RulePack id=` value), which must be different than the GUID that you'll generate later for the sensitive information type in the `Entity` element. You can generate a GUID in Windows PowerShell by running the following command:

    ```
    [guid]::NewGuid()
    ```

- Mantaining the `Version` element is important. When you upload your custom rule package for the first time, Office 365 notes the version number. Later, if you try to upload a modified version of the same rule package, the verson number must be different; otherwise, the upload will fail.

The following diagram shows what your `RulePackage` element should look like:

![XML markup showing RulePack element](media/fd0f31a7-c3ee-43cd-a71b-6a3813b21155.png)

### Key requirements [Rules, Entity, and Pattern elements]

Now that you've completed the preliminary information, it's helpful to understand how custom sensitive information types are defined in the XML of the custom rule package.

The `Rules` element contains all of the sensitive information types in the rule package. The term "rule" is bit confusing because you also rules in DLP policies to define the conditions, exceptions, and actions for detected content. However, the `Rules` element in the rule package (or even in the name "rule package") aren't the same as the rules in DLP policies. That's OK, because there's little need to talk about "rules" in the XML file after this (for example, thery's no `Rule` element within the `Rules` element).

Within the `Rules` element, each sensitive information type is definied in the `Entity` element, so the terms "entity" and "sensitive information type" are interchangeable. Each `Entity` element contains:

- A required GUID that's different from the GUID value you used for the rule package in the previous section. You can generate a GUID in Windows PowerShell by running the following command:

    ```
    [guid]::NewGuid()
    ```

    In the following diagram, the example value is `<Entity id="E1CC861E-3FE9-4A58-82DF-4BD259EAB378">`.

- An optional comment, which makes it easier for you to identify the sensitive information types within the XML of the rule package. The example value in the following diagram is `<!-- Employee ID -->`.

![XML markup showing Rules and Entity elements](media/c46c0209-0947-44e0-ac3a-8fd5209a81aa.png)

Each `Entity` element contains one or more `Pattern` elements that define the text patterns to detect in content. A `Pattern` element contains:

- A primary pattern (for example, an employee ID number or project number). This is typically defined by a regular expression (RegEx), but could also be a list of keywords.

- One or more optional pieces of additional evidence (keywords, dates, or text patterns defined by other regular expressions). This _supporting_ or _corroborative_ evidence helps to eliminate false positives (content matches that you aren't interested in).

We'll go into more detail about the `Pattern` element in the next section.

Let's put this all together: You want to identify content that contains your organization's employee ID, which is formatted as a nine-digit number. So the pattern refers to a regular expression that identifies nine-digit numbers. Any content containing a nine-digit number satisfies the pattern.

![Diagram of entity with one pattern](media/4cc82dcf-068f-43ff-99b2-bac3892e9819.png)

However, this pattern by itself will match content that contains _any_ nine-digit number, including numbers that aren't employee IDs (false positives).

For this reason, it's more common to define an entity by using more than one pattern, where the patterns identify supporting evidence (such as a keyword or date) in addition to the entity (such as a nine-digit number).

For example, to increase the likelihood of finding content that contains an employee ID as defined by the nine-digit number pattern, you can define another pattern that also identifies a hire date, and yet another pattern that identifies both a hire date and a keyword (such as "employee ID").

![Diagram of entity with multiple patterns](media/c8dc2c9d-00c6-4ebc-889a-53b41a90024a.png)

**Notes**:

- Patterns that require more evidence have a higher confidence level. This is useful later in DLP policies, because you can use more restrictive actions (such as block content) with only the higher-confidence matches, and you can use less restrictive actions (such as send notification) with the lower-confidence matches.

- The `IdMatch` and `Match` elements reference regular expressions and keywords that are defined _outside_ of `Pattern` elements. This allows other `Entity` and `Pattern` elements in the rule package to use these regular expressions and keywords. The `IdMatch` and `Match` elements are explained in a later section.


### Identify what you want to find [Regex, Pattern, and IdMatch elements]

For a custom sensitive information type that detects nine-digit employee numbers, the `Regex` element is the primary pattern for the entity. This `Regex` element:

- Defines the regular expression `(\s)(\d{9})(\s)` that detects nine-digit numbers `(\d{9})` surrounded by white space `(\s)`.

- Is named `Regex_employee_id`, and as shown in the diagram at the end of this section, is referenced by all patterns in the entity. You can also see that the `Regex` element itself is actually defined _outside_ the Employee ID entity itself (below the `</Entity>` tag), which means the regular expression can be referenced by other entities and patterns within the rule package.

The `Pattern` elements define the content that the entity is trying to detect. Each `Pattern` element contains:

- One and only one `IdMatch` element that identifies the primary text pattern to match (`Regex_employee_id` in the diagram at the end of this section). Regardless of how many patterns exists in the entity, they all have the primary requirement of trying to match the criteria defined in the `IdMatch` value (the employee ID number in this example).

- Corroborative evidence as defined by one or more `Match` elements, which are explained in the next section.

**Notes**:

- The diagram at the end of this section is incomplete. The reason to have multiple `Pattern` elements in an entity is so each pattern uses a different level of evidence to augment the `IdMatch` search criteria (additional keywords, RegExes, or built-in functions). In turn, the different levels of evidence allow you to assign a different _confidence level_ to each pattern. Confidence levels are explained later in this topic.

- If a regular expression in the custom rule package matches too much content, your upload could fail. There are built-in checks that prevent uploading rule packages that could negatively impact performance. For more information, see the [Potential validation issues to be aware of](#potential-validation-issues-to-be-aware-of) section in this topic.

![XML markup showing multiple Pattern elements referencing single RegEx element](media/8f3f497b-3b8b-4bad-9c6a-d9abf0520854.png)

### Additional evidence [Match element and minCount attribute]

`Match` elements in a `Pattern` element identify the additional supporting evidence (for example, keywords, RegExes, dates, or addresses) to use along with the `IdMatch` element. All elements in a pattern must be satisfied for a match, which means the `IdMatch` and `Match` elements in the pattern are joined by the implicit AND operator.

The `Match` element contains:

- `IdRef=`: Identifies what's in the `Match` element. For example:

  - **Keywords**: Idefined by the name of the `Keyword` element that exists ouside of the entity (for example, `Keyword_employee` and `Keyword_badge` in the diagram below). As with `Regex` elments that are defined outside of the entity, a `Keyword` element can be referenced in any `Match` elements in any `Pattern` elements within the rule package. See the next section for more details about `Keyword` elements.

  - **RegExes**: Identified by the name of the `Regex` element that exists outside of the entity. Note that our example doesn't use _additional_ RegExes for supporting evidence, but a `Regex` element follows the same structure as the `Regex_employee_id` that we're using in our entity (a name and the regular expression code itself). As described earlier, a `Regex` element that's defined outside of an entity can be referenced in any `Match` elements in any `Pattern` elements within the rule package. Just remember, you can't use the same regular expression for the `IdMatch` and `Match` elements in the same pattern.

  - **Functions**: Built-in DLP functions that can identify corroborative evidence, and you identify them by name. For example, `Func_us_date` identify dates in the format commonly used in the US. For more information, see [What the DLP functions look for](what-the-dlp-functions-look-for.md).

      ![XML markup showing Match element referencing built-in function](media/dac6eae3-9c52-4537-b984-f9f127cc9c33.png)

- `minCount`: An optional attribute that specifies the required number instances that need to be found for the `Match` element. In the diagram below, the value `minCount=2` on the `Match idRef="Keyword_badge"` element indicates at least two of the `Keyword_badge` keywords need to be detected to satisfy the `Match` element in the pattern.

    ![XML markup showing Match element with minOccurs attribute](media/607f6b5e-2c7d-43a5-a131-a649f122e15a.png)

### Keywords [Keyword, Group, and Term elements; matchStyle and caseSensitive attributes]

A `Keyword` element contains:

- A name (for example, `Keyword_badge` and `Keyword_employee` in the diagram below). This name value is referenced in the `Match` element in the pattern (for example, `<Match id=Keyword_badge>` in the diagram below).

- The `Group` and `matchStyle` attributes used together. Valid values for `matchStyle` are:

  - `word`: Identifies whole words surrounded by white space or other delimiters. You should always use this value unless you need to match parts of words or match words in Asian languages. 

  - `string`: Identifies strings no matter what they're surrounded by. For example, "id" will match "bid" and "idea". Use this value only when you need to match Asian words or if your keyword may be included as part of other strings.

- `Term` elements that contain:

  - Each word in the keyword list specified in a `<Term>` `</Term>` tag. For example, in the diagram below, the words in the `Keyword_badge` keyword list are `card` and `badge`.

  - The optional `caseSensitive="true"` attribute on any term. This value specifies that the content must match the keyword exactly, including lowercase and uppercase letters.

![XML markup showing Match elements referencing keywords](media/e729ba27-dec6-46f4-9242-584c6c12fd85.png)

### Different combinations of evidence [Any element, minMatches and maxMatches attributes]

As explained earlier, all `Match` elements in a `Pattern` element are joined with the implicit AND operator (all matches must be satisfied before the entire pattern is satisfied).

However, you can create more flexible matching logic by using `Any` elements.

An `Any` element defines a group of `Match` elements. You use the `minMatches` and `maxMatches` attributes on the `Any` element to match some, all, none, or an exact subset of the collection of `Match` elements. Note that the `minMatches` and `maxMatches` attributes specify the number of `Match` elements that are satisfied (or not satisfied), not the individual number of hits returned by some or all of the `Match` elements. For example:

- **Use the minMatches attribute by itself**: For example, your `Any` element has three `Match` elements, and you use the value `minMatches=1`. The `Any` element is satisfied if one or more of the `Match` elements are satisfied (regardless of the number of times). This effectively joins the `Match` elements with the implict OR operator. In the following diagram, the `Any` element is satisfied if a US-formatted date or a keyword from the `Keyword_badge` or `Keyword_Employee` keyword lists is found (one out of three).

  ![XML markup showing Any element with minMatches attribute](media/385db1b1-571b-4a05-81b3-db28f5099c17.png)

- **Set minMatches and maxMatches to the same value**: For example, your `Any` element has three `Match` elements, and you use the values `minMatches=1` and `maxMatches=1`. This `Any` element is satisfied if _only_ one of the `Match` elements is satisfied; any more than that, and the `Any` element isn't satisfied. In the following diagram, the `Any` element is statisfied only if exactly one date or keyword is found.

  ![XML markup showing Any element wtih minMatches and maxMatches attributes](media/97b10002-7781-42e8-ac5a-20ad8c5a887e.png)

- **Set minMatches and maxMatches to 0**: The `Any` element is satisfied by the _absence_ of specific evidence (a keyword list or other evidence that likely indicates false positives).

  For example, the employee ID entity looks for the keyword "card" because it might refer to an "ID card". However, you can add "credit card" as a keyword to a `Keyword_Fasle_Positives` keyword list that defines the list of terms to exclude from satisfying the pattern.

  ![XML markup showing maxMatches attribute value of zero](media/f81d44e5-3db8-48a8-8919-f483a386afdf.png)


### Proximity of evidence [patternsProximity attribute]

The `patternsProximity` attribute in each `Entity` element defines the required distance (in Unicode characters) of the supporting evidence in the pattern (keywords, dates, etc.) from the primary item that the pattern is looking for.

![XML markup showing patternsProximity attribute](media/e97eb7dc-b897-4e11-9325-91c742d9839b.png)


This distance to the left or right of the primary item is called the _proximity window_.

![Diagram of proximity window](media/b593dfd1-5eef-4d79-8726-a28923f7c31e.png)

**Note**: For email messages, the message body and each attachment are treated as separate items. One proximity window is the message body; another is each individual message attachment.

### Confidence levels [confidenceLevel attribute, recommendedConfidence attribute]

The more evidence that a pattern requires, the more confidence you have that an actual entity (such as employee ID) has been identified when the pattern is matched. For example, you have more confidence in a pattern that requires a nine-digit ID number, hire date, and keyword in close proximity, than you do in a pattern that requires only a nine-digit ID number.

Every `Pattern` element in an entity has a required `confidenceLevel` attribute that has an integer value from 1 to 100. You can think of the `confidenceLevel` value as  a unique ID for each pattern in an entity, because each pattern requires a different confidence level. A lower confidence patterm should have a lower value than a higher confidence pattern, but the actual number value doesn't matter; simply pick numbers that make sense to your compliance team. You can reference these confidence levels in the conditions of the rules in the DLP policies that you create.

![XML markup showing Pattern elements with different values for confidenceLevel attribute](media/301e0ba1-2deb-4add-977b-f6e9e18fba8b.png)

The `Entity` element itself also has a `recommendedConfidence` attribute. You can think of this value as the default confidence level for the whole entity. When you create a rule in a DLP policy, if you don't specify a confidence level for the rule to use, the rule will match based on the `recommendedConfidence` attribute value for the sensitive information type.

### Other languages [LocalizedStrings element]

If your compliance team uses the Security & Compliance Center to create DLP policies in different languages, you can provide localized versions of the name and description of the custom sensitive information type in the Security & Compliance Center.

The `LocalizedStrings` element that exists outside of the `Entity` element contains the name and description of each sensitive information type in one or more languages.

Within the `LocalizedStrings` element, there's a `Resource` element that references the GUID each entity in the rule package. Each `Resource` element contains one or more `Name` and `Description` elements that use a `langcode` attributes to provide a localized name and description of the sensitive information type for each specified language. You can also use the `default` attribute to specify the default language for each sensitive information type.

![XML markup showing contents of LocalizedStrings element](media/a96fc34a-b93d-498f-8b92-285b16a7bbe6.png)

**Notes**: Localized strings control how the custom sensitive information types in the rule package appear to admins and compliance officers in the Security & Compliance Center. You can't use localized strings to provide different localized versions of keyword lists or regular expressions.

### Potential validation issues to be aware of

When you upload your rule package , the system validates the XML file and checks for known bad patterns and obvious performance issues. These are some known regular expression issues that the validation checks for:

- No beginning or ending alternators `|` (this matches everything because it's considered an empty match).

    For example, `|a` or `b|` won't pass validation.

- No beginning or ending `.{0,m}` patterns (no functional purpose and only impairs performance).

    For example, `.{0,50}ASDF` or `ASDF.{0,50}` won't pass validation.

- No `.{0,m}` or `.{1,m}` in groups, and no `.*` or `.+` in groups.

    For example, `(.{0,50000})` won't pass validation.

- No characters with `{0,m}` or `{1,m}` repeaters in groups.

    For example, `(a*)` won't pass validation.

- No beginning or ending with `.{1,m}`; instead, use just `.`

    For example, `.{1,m}asdf` won't pass validation; instead, use just `.asdf`.

- No unbounded repeaters (such as `*` or `+`) on groups.

    For example, `(xx)*` and `(xx)+` won't pass validation.

If a custom sensitive information type contains an issue that may affect performance, it won't be uploaded and you may see one of these error messages:

- **Generic quantifiers which match more content than expected (e.g., '+', '\*')**

- **Lookaround assertions**

- **Complex grouping in conjunction with general quantifiers**

### Reference: Rule package XML schema definition

You can copy this markup, save it as an XSD file, and use it to validate your rule package XML file.

```
<?xml version="1.0" encoding="utf-8"?>
<xs:schema xmlns:mce="http://schemas.microsoft.com/office/2011/mce"
           targetNamespace="http://schemas.microsoft.com/office/2011/mce" 
           xmlns:xs="http://www.w3.org/2001/XMLSchema"
           elementFormDefault="qualified"
           attributeFormDefault="unqualified"
           id="RulePackageSchema">
  <!-- Use include if this schema has the same target namespace as the schema being referenced, otherwise use import -->
  <xs:element name="RulePackage" type="mce:RulePackageType"/>
  <xs:simpleType name="LangType">
    <xs:union memberTypes="xs:language">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:enumeration value=""/>
        </xs:restriction>
      </xs:simpleType>
    </xs:union>
  </xs:simpleType>
  <xs:simpleType name="GuidType" final="#all">
    <xs:restriction base="xs:token">
      <xs:pattern value="[0-9a-fA-F]{8}\-([0-9a-fA-F]{4}\-){3}[0-9a-fA-F]{12}"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="RulePackageType">
    <xs:sequence>
      <xs:element name="RulePack" type="mce:RulePackType"/>
      <xs:element name="Rules" type="mce:RulesType">
        <xs:key name="UniqueRuleId">
          <xs:selector xpath="mce:Entity|mce:Affinity|mce:Version/mce:Entity|mce:Version/mce:Affinity"/>
          <xs:field xpath="@id"/>
        </xs:key>
        <xs:key name="UniqueProcessorId">
          <xs:selector xpath="mce:Regex|mce:Keyword|mce:Fingerprint"></xs:selector>
          <xs:field xpath="@id"/>
        </xs:key>
        <xs:key name="UniqueResourceIdRef">
          <xs:selector xpath="mce:LocalizedStrings/mce:Resource"/>
          <xs:field xpath="@idRef"/>
        </xs:key>
        <xs:keyref name="ReferencedRuleMustExist" refer="mce:UniqueRuleId">
          <xs:selector xpath="mce:LocalizedStrings/mce:Resource"/>
          <xs:field xpath="@idRef"/>
        </xs:keyref>
        <xs:keyref name="RuleMustHaveResource" refer="mce:UniqueResourceIdRef">
          <xs:selector xpath="mce:Entity|mce:Affinity|mce:Version/mce:Entity|mce:Version/mce:Affinity"/>
          <xs:field xpath="@id"/>
        </xs:keyref>
      </xs:element>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="RulePackType">
    <xs:sequence>
      <xs:element name="Version" type="mce:VersionType"/>
      <xs:element name="Publisher" type="mce:PublisherType"/>
      <xs:element name="Details" type="mce:DetailsType">
        <xs:key name="UniqueLangCodeInLocalizedDetails">
          <xs:selector xpath="mce:LocalizedDetails"/>
          <xs:field xpath="@langcode"/>
        </xs:key>
        <xs:keyref name="DefaultLangCodeMustExist" refer="mce:UniqueLangCodeInLocalizedDetails">
          <xs:selector xpath="."/>
          <xs:field xpath="@defaultLangCode"/>
        </xs:keyref>
      </xs:element>
      <xs:element name="Encryption" type="mce:EncryptionType" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="id" type="mce:GuidType" use="required"/>
  </xs:complexType>
  <xs:complexType name="VersionType">
    <xs:attribute name="major" type="xs:unsignedShort" use="required"/>
    <xs:attribute name="minor" type="xs:unsignedShort" use="required"/>
    <xs:attribute name="build" type="xs:unsignedShort" use="required"/>
    <xs:attribute name="revision" type="xs:unsignedShort" use="required"/>
  </xs:complexType>
  <xs:complexType name="PublisherType">
    <xs:attribute name="id" type="mce:GuidType" use="required"/>
  </xs:complexType>
  <xs:complexType name="LocalizedDetailsType">
    <xs:sequence>
      <xs:element name="PublisherName" type="mce:NameType"/>
      <xs:element name="Name" type="mce:RulePackNameType"/>
      <xs:element name="Description" type="mce:OptionalNameType"/>
    </xs:sequence>
    <xs:attribute name="langcode" type="mce:LangType" use="required"/>
  </xs:complexType>
  <xs:complexType name="DetailsType">
    <xs:sequence>
      <xs:element name="LocalizedDetails" type="mce:LocalizedDetailsType" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="defaultLangCode" type="mce:LangType" use="required"/>
  </xs:complexType>
  <xs:complexType name="EncryptionType">
    <xs:sequence>
      <xs:element name="Key" type="xs:normalizedString"/>
      <xs:element name="IV" type="xs:normalizedString"/>
    </xs:sequence>
  </xs:complexType>
  <xs:simpleType name="RulePackNameType">
    <xs:restriction base="xs:token">
      <xs:minLength value="1"/>
      <xs:maxLength value="64"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="NameType">
    <xs:restriction base="xs:normalizedString">
      <xs:minLength value="1"/>
      <xs:maxLength value="256"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="OptionalNameType">
    <xs:restriction base="xs:normalizedString">
      <xs:minLength value="0"/>
      <xs:maxLength value="256"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="RestrictedTermType">
    <xs:restriction base="xs:string">
      <xs:minLength value="1"/>
      <xs:maxLength value="100"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="RulesType">
    <xs:sequence>
      <xs:choice maxOccurs="unbounded">
        <xs:element name="Entity" type="mce:EntityType"/>
        <xs:element name="Affinity" type="mce:AffinityType"/>
        <xs:element name="Version" type="mce:VersionedRuleType"/>
      </xs:choice>
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element name="Regex" type="mce:RegexType"/>
        <xs:element name="Keyword" type="mce:KeywordType"/>
        <xs:element name="Fingerprint" type="mce:FingerprintType"/>
        <xs:element name="ExtendedKeyword" type="mce:ExtendedKeywordType"/>
      </xs:choice>
      <xs:element name="LocalizedStrings" type="mce:LocalizedStringsType"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="EntityType">
    <xs:sequence>
      <xs:element name="Pattern" type="mce:PatternType" maxOccurs="unbounded"/>
      <xs:element name="Version" type="mce:VersionedPatternType" minOccurs="0" maxOccurs="unbounded" />
    </xs:sequence>
    <xs:attribute name="id" type="mce:GuidType" use="required"/>
    <xs:attribute name="patternsProximity" type="mce:ProximityType" use="required"/>
    <xs:attribute name="recommendedConfidence" type="mce:ProbabilityType"/>
    <xs:attribute name="workload" type="mce:WorkloadType"/>
  </xs:complexType>
  <xs:complexType name="PatternType">
    <xs:sequence>
      <xs:element name="IdMatch" type="mce:IdMatchType"/>
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element name="Match" type="mce:MatchType"/>
        <xs:element name="Any" type="mce:AnyType"/>
      </xs:choice>
    </xs:sequence>
    <xs:attribute name="confidenceLevel" type="mce:ProbabilityType" use="required"/>
  </xs:complexType>
  <xs:complexType name="AffinityType">
    <xs:sequence>
      <xs:element name="Evidence" type="mce:EvidenceType" maxOccurs="unbounded"/>
      <xs:element name="Version" type="mce:VersionedEvidenceType" minOccurs="0" maxOccurs="unbounded" />
    </xs:sequence>
    <xs:attribute name="id" type="mce:GuidType" use="required"/>
    <xs:attribute name="evidencesProximity" type="mce:ProximityType" use="required"/>
    <xs:attribute name="thresholdConfidenceLevel" type="mce:ProbabilityType" use="required"/>
    <xs:attribute name="workload" type="mce:WorkloadType"/>
  </xs:complexType>
  <xs:complexType name="EvidenceType">
    <xs:sequence>
      <xs:choice maxOccurs="unbounded">
        <xs:element name="Match" type="mce:MatchType"/>
        <xs:element name="Any" type="mce:AnyType"/>
      </xs:choice>
    </xs:sequence>
    <xs:attribute name="confidenceLevel" type="mce:ProbabilityType" use="required"/>
  </xs:complexType>
  <xs:complexType name="IdMatchType">
    <xs:attribute name="idRef" type="xs:string" use="required"/>
  </xs:complexType>
  <xs:complexType name="MatchType">
    <xs:attribute name="idRef" type="xs:string" use="required"/>
    <xs:attribute name="minCount" type="xs:positiveInteger" use="optional"/>
    <xs:attribute name="uniqueResults" type="xs:boolean" use="optional"/>
  </xs:complexType>
  <xs:complexType name="AnyType">
    <xs:sequence>
      <xs:choice maxOccurs="unbounded">
        <xs:element name="Match" type="mce:MatchType"/>
        <xs:element name="Any" type="mce:AnyType"/>
      </xs:choice>
    </xs:sequence>
    <xs:attribute name="minMatches" type="xs:nonNegativeInteger" default="1"/>
    <xs:attribute name="maxMatches" type="xs:nonNegativeInteger" use="optional"/>
  </xs:complexType>
  <xs:simpleType name="ProximityType">
    <xs:union>
      <xs:simpleType>
        <xs:restriction base='xs:string'>
          <xs:enumeration value="unlimited"/>
        </xs:restriction>
      </xs:simpleType>
      <xs:simpleType>
        <xs:restriction base="xs:positiveInteger">
          <xs:minInclusive value="1"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:union>
  </xs:simpleType>
  <xs:simpleType name="ProbabilityType">
    <xs:restriction base="xs:integer">
      <xs:minInclusive value="1"/>
      <xs:maxInclusive value="100"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="WorkloadType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Exchange"/>
      <xs:enumeration value="Outlook"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="EngineVersionType">
    <xs:restriction base="xs:token">
      <xs:pattern value="^\d{2}\.01?\.\d{3,4}\.\d{1,3}$"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="VersionedRuleType">
    <xs:choice maxOccurs="unbounded">
      <xs:element name="Entity" type="mce:EntityType"/>
      <xs:element name="Affinity" type="mce:AffinityType"/>
    </xs:choice>
    <xs:attribute name="minEngineVersion" type="mce:EngineVersionType" use="required" />
  </xs:complexType>
  <xs:complexType name="VersionedPatternType">
    <xs:sequence>
      <xs:element name="Pattern" type="mce:PatternType" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="minEngineVersion" type="mce:EngineVersionType" use="required" />
  </xs:complexType>
  <xs:complexType name="VersionedEvidenceType">
    <xs:sequence>
      <xs:element name="Evidence" type="mce:EvidenceType" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="minEngineVersion" type="mce:EngineVersionType" use="required" />
  </xs:complexType>
  <xs:simpleType name="FingerprintValueType">
    <xs:restriction base="xs:string">
      <xs:minLength value="2732"/>
      <xs:maxLength value="2732"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="FingerprintType">
    <xs:simpleContent>
      <xs:extension base="mce:FingerprintValueType">
        <xs:attribute name="id" type="xs:token" use="required"/>
        <xs:attribute name="threshold" type="mce:ProbabilityType" use="required"/>
        <xs:attribute name="shingleCount" type="xs:positiveInteger" use="required"/>
        <xs:attribute name="description" type="xs:string" use="optional"/>
      </xs:extension>
    </xs:simpleContent>
  </xs:complexType>
  <xs:complexType name="RegexType">
    <xs:simpleContent>
      <xs:extension base="xs:string">
        <xs:attribute name="id" type="xs:token" use="required"/>
      </xs:extension>
    </xs:simpleContent>
  </xs:complexType>
  <xs:complexType name="KeywordType">
    <xs:sequence>
      <xs:element name="Group" type="mce:GroupType" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="id" type="xs:token" use="required"/>
  </xs:complexType>
  <xs:complexType name="GroupType">
    <xs:sequence>
      <xs:choice>
        <xs:element name="Term" type="mce:TermType" maxOccurs="unbounded"/>
      </xs:choice>
    </xs:sequence>
    <xs:attribute name="matchStyle" default="word">
      <xs:simpleType>
        <xs:restriction base="xs:NMTOKEN">
          <xs:enumeration value="word"/>
          <xs:enumeration value="string"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  </xs:complexType>
  <xs:complexType name="TermType">
    <xs:simpleContent>
      <xs:extension base="mce:RestrictedTermType">
        <xs:attribute name="caseSensitive" type="xs:boolean" default="false"/>
      </xs:extension>
    </xs:simpleContent>
  </xs:complexType>
  <xs:complexType name="ExtendedKeywordType">
    <xs:simpleContent>
      <xs:extension base="xs:string">
        <xs:attribute name="id" type="xs:token" use="required"/>
      </xs:extension>
    </xs:simpleContent>
  </xs:complexType>
  <xs:complexType name="LocalizedStringsType">
    <xs:sequence>
      <xs:element name="Resource" type="mce:ResourceType" maxOccurs="unbounded">
      <xs:key name="UniqueLangCodeUsedInNamePerResource">
        <xs:selector xpath="mce:Name"/>
        <xs:field xpath="@langcode"/>
      </xs:key>
      <xs:key name="UniqueLangCodeUsedInDescriptionPerResource">
        <xs:selector xpath="mce:Description"/>
        <xs:field xpath="@langcode"/>
      </xs:key>
    </xs:element>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ResourceType">
    <xs:sequence>
      <xs:element name="Name" type="mce:ResourceNameType" maxOccurs="unbounded"/>
      <xs:element name="Description" type="mce:DescriptionType" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="idRef" type="mce:GuidType" use="required"/>
  </xs:complexType>
  <xs:complexType name="ResourceNameType">
    <xs:simpleContent>
      <xs:extension base="xs:string">
        <xs:attribute name="default" type="xs:boolean" default="false"/>
        <xs:attribute name="langcode" type="mce:LangType" use="required"/>
      </xs:extension>
    </xs:simpleContent>
  </xs:complexType>
  <xs:complexType name="DescriptionType">
    <xs:simpleContent>
      <xs:extension base="xs:string">
        <xs:attribute name="default" type="xs:boolean" default="false"/>
        <xs:attribute name="langcode" type="mce:LangType" use="required"/>
      </xs:extension>
    </xs:simpleContent>
  </xs:complexType>
</xs:schema>

```

## More information

- [Overview of data loss prevention policies](data-loss-prevention-policies.md)

- [What the sensitive information types look for](what-the-sensitive-information-types-look-for.md)

- [What the DLP functions look for](what-the-dlp-functions-look-for.md)
