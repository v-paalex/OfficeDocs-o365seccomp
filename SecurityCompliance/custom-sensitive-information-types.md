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

- **The primary pattern that you're trying to match in content**: employee ID numbers, project numbers, etc. This is typically identified by a regular expression (RegEx), but it can also be a list of keywords.

- **Supporting evidence**: Suppose you're looking for a nine-digit employee ID number. Not all nine-digit numbers are employee ID numbers, so you can look for additional text: "card", "badge", "ID", or other text based on additional regular expressions. This supporting evidence (also known as corroborative evidence) with the nine-digit number increases the likelyhood that number found in content is really an employee ID number.

- **Character proximity**: It makes sense that the closer together the primary pattern and the supporting evidence is, the more likely the pattern is going to be what you're looking for. You can specify the character distance between the primary pattern and the supporting evidence as show in the following diagram:

    ![Diagram of corroborative evidence and proximity window](media/dc68e38e-dfa1-45b8-b204-89c8ba121f96.png)

- **Confidence level**: The more supporting evidence you have, the higher the likelyhood that the content contains the sensitive information you're looking for. You can assign higher levels of confidence for matches based on more evidence. When you use the sensitive information type in DLP policies, you can then take more restrictive action on higher confidence match.

To create custom sensitive information types, you have the following options:

- **Use the graphical user interface**: This method is easier and faster, but you have less configuration options than PowerShell.

- **Use PowerShell**: This method requires that you first create an XML file (called a _rule package_) that contains one or more sensitive information types, and then you use use PowerShell to import the rule package (importing the rule package is not too difficult). This method is much more complex than the graphical user interface, but you have more configuration options.

The key differences are described in the following table:

|Sensitive information types in the UI|Sensitive information types in PowerShell|
|:-----|:-----|
|Name and Description are in one language|Supports mulitple languages for Name and Description|
|Supports only one pattern (the primary pattern).|Supports multiple patterns in addition to the primary pattern.|
|Confidence level is configurable for the sensitive information type.|Confidence leve is configurable for the sensitive information type and each individual pattern within.|
|Supporting evidence can be: <br/>• Regular expressions <br/>• Keywords <br/>• Keyword dictionaries|Supporting evidence can be: <br/>• Regular expressions <br/>• Keywords <br/>• Keyword dictionaries <br/>• [Built-in DLP functions](what-the-dlp-functions-look-for.md)|
|Supporting evidence is implicitly joined by the OR operator.|Supporting evidence can be implicitly joined by combination of AND and OR operators.|

The rest of this topic explains the elements of the XML file that defines your own custom sensitive information type. You need to know how to create a regular expression. As an example, this topic explores a custom sensitive information type that identifies an employee ID. You can use this example XML as a starting point for your own XML file.

Then you're ready to use your custom sensitive information type in your DLP policies and test that it's detecting the sensitive information as you intended.

## Important disclaimer

Due to the variances in customer environments and content match requirements, Microsoft Support cannot assist in providing custom content-matching definitions; e.g., defining custom classifications or regular expression (also known as RegEx) patterns. For custom content-matching development, testing, and debugging, Office 365 customers will need to rely upon internal IT resources, or use an external consulting resource such as Microsoft Consulting Services (MCS). Support engineers can provide limited support for the feature, but cannot provide assurances that any custom content-matching development will fulfill the customer's requirements or obligations. As an example of the type of support that can be provided, sample regular expression patterns may be provided for testing purposes. Or, support can assist with troubleshooting an existing RegEx pattern which is not triggering as expected with a single specific content example.

## Sample XML of a rule package

Here's the sample XML of the rule package that we'll explore in this topic. Elements and attributes are explained in the sections below.

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

## What are your key requirements? [Rule, Entity, Pattern elements]


## Structure of the XML file
Before you get started, it's helpful to understand the basic structure of the XML schema for a rule, and how you can use this structure to define your custom sensitive information type so that it will identify the right content.

In the XML file, a **rule** defines one or more **entities** (sensitive information types), and each entity defines one or more **patterns**. A pattern is what DLP looks for when it evaluates content such as email and documents.

**Note**: DLP policies contain rules with conditions, optional exceptions and actions. But in the XML file, a rule is the patterns that define the entity (sensitive information type). So, in the explantion of the XML file, when you see rule, think think sensitive information type, not conditions, exceptions, and actions.

### Objectives

Here's the simplest scenario: You want your DLP policy to identify content that contains your organization's employee ID, which is formatted as a nine-digit number. So the pattern refers to a regular expression contained in the rule that identifies nine-digit numbers. Any content containing a nine-digit number satisfies the pattern.

![Diagram of entity with one pattern](media/4cc82dcf-068f-43ff-99b2-bac3892e9819.png)

However, this pattern by itself will match content that contains any nine-digit number, including numbers that aren't employee IDs.

For this reason, it's more common to define an entity by using more than one pattern, where the patterns identify supporting evidence (such as a keyword or date) in addition to the entity (such as a nine-digit number).

For example, to increase the likelihood of identifying content that contains an employee ID, in addition to the nine-digit number pattern, you can define another pattern that also identifies a hire date, and define yet another pattern that identifies both a hire date and a keyword (such as "employee ID"), .

![Diagram of entity with multiple patterns](media/c8dc2c9d-00c6-4ebc-889a-53b41a90024a.png)

Note a couple of important aspects of this structure:

- Patterns that require more evidence have a higher confidence level. This is useful because when you later use this sensitive information type in a DLP policy, you can use more restrictive actions (such as block content) with only the higher-confidence matches, and you can use less restrictive actions (such as send notification) with the lower-confidence matches.

- The supporting IdMatch and Match elements reference RegExes and keywords that are actually children of the Rule element, not the Pattern. These supporting elements are referenced by the Pattern but included in the Rule. This means that a single definition of a supporting element, like a regular expression or a keyword list, can be referenced by multiple entities and patterns.

## Name the entity and generate its GUID [Rules, Entity, and Entity id elements]

In the XML file, an **entity** is a sensitive information type that has a well-defined pattern: (for example, an employee ID number, credit card number, or social security number) Each entity has a unique GUID as its ID.

1. Add the `<Rules>` `</Rules>` tags.

2. Inside the Rules tags, add the `<!-- -->` comment tag that contains the the name of your custom entity (`<!-- Employee ID -->` in this example).

3. After the comment tag, add the `<Entity>` `</Entity>` tags. The opening Entity tag requires an addtional `id=` value that contains a unique GUID for the entity. You can generate a GUID in Windows PowerShell by running the following command:

    ```
    [guid]::NewGuid()
    ```

    The example value in the following diagram is `<Entity id="E1CC861E-3FE9-4A58-82DF-4BD259EAB378">`

![XML markup showing Rules and Entity elements](media/c46c0209-0947-44e0-ac3a-8fd5209a81aa.png)

## Identify the text pattern to match [Regex, Pattern, and IdMatch elements]

Every

In this example, the `Regex` element is the fundamental item that's used in the entity, because that's what defines an employee ID:

- The regular expression `(\s)(\d{9})(\s)` looks for nine-digit numbers `(\d{9})` surrounded by white space `(\s)`.

- The Regex element is named `Regex_employee_id`, and that name is referenced by all patterns in the entity. Note that the `Regex` element is actually _outside_ the Employee ID entity itself (below the `</Entity>` tag), which means `Regex_employee_id` can be referenced by other entities within the rule package.

The `Pattern` elements contains the list of what the entity is looking for. Each `Pattern` element has one and only one `IdMatch` element that identifies what needs to be matched (`Regex_employee_id` in this example). Regardless of how many patterns exists in the entity, they all have the primary requirement of trying to match the criteria defined in the `IdMatch` value (for example, an employee ID, credit card number, or social security number).

The only reason to have multiple `Pattern` elements is so each pattern uses different levels of evidence to augment the `IdMatch` search criteria (additional keywords, RegExes, or built-in functions). And, the best use of patterns with different levels of evidence is to assign a different confidence level to each pattern. This allows your DLP policies to take more or less aggressive action on content that's identified with more or less confidence (levels of evidence).

All of this is to say: the sample XML file as shown in the diagram in this section is incomplete because (at the moment) it contains identical `Pattern` elements.

![XML markup showing multiple Pattern elements referencing single RegEx element](media/8f3f497b-3b8b-4bad-9c6a-d9abf0520854.png)

**A note about regular expressions**: If your XML file contains a RegEx that matches too much content, your upload could fail. There are built-in checks that prevent uploading rule packages that could negatively impact performance. For more information, see the __ section in this topic.


----------------- Relocate this ---------------------------
When satisfied, a pattern returns a count and confidence level, which you can use in the conditions in your DLP policy. When you add a condition for detecting a sensitive information type to a DLP policy, you can edit the count and confidence level as shown here. Confidence level (also called match accuracy) is explained later in this topic.

![Instance count and match accuracy options](media/11d0b51e-7c3f-4cc6-96d8-b29bcdae1aeb.png)
-------------------------------------------------------------

## Additional evidence [Match element and minCount attribute]

`Match` elements in a pattern identify the additional supporting evidence (for example, keywords, RegExes, dates, or addresses) that used along with the `IdMatch` element. All `Match` elements and the `IdMatch` element in the pattern are joined by the implicit AND operator (all elements in the pattern must be satisfied for the pattern to be matched).

The `Match` element consists of:

- `IdRef=`: Identifies what's in the `Match` element. For example:

  - Keywords: Idefined by the name of the `Keyword` element that exists ouside of the entity (for example, `Keyword_employee` and `Keyword_badge` in the diagram below). A `Keyword` element can be referenced by multiple `Match` elements in multiple `Pattern` elements (within a single entity or across multiple entities). See the next section for more details about Keywords.

  - RegExes: Identified by the name of the `Regex` element that exists outside of the entity. Note that our example doesn't use _additional_ RegExes, but a `Regex` element follows the same structure as the `Regex_employee_id` that we're using in our entity (a name and the regular expression code itself). As described earlier, a `Regex` element can be referenced by multiple patterns and/or multiple entities. Just remember, you can't use the same RegEx in an entity for the `IdMatch` and `Match` elements in the same pattern.

  - Functions: Built-in DLP functions that can identify corroborative evidence, and you identify them by name. For example, `Func_us_date` identify dates in the format commonly used in the US. For more information, see [What the DLP functions look for](what-the-dlp-functions-look-for.md).

      ![XML markup showing Match element referencing built-in function](media/dac6eae3-9c52-4537-b984-f9f127cc9c33.png)

- `minCount`: This optional attribute specifies the required number instances that need to be found for the `Match` element. In the diagram below, the value `minCount=2` on the `Match idRef="Keyword_badge"` element indicates at least two of the `Keyword_badge` keywords need to be detected to satisfy the `Match` element in the pattern.

    ![XML markup showing Match element with minOccurs attribute](media/607f6b5e-2c7d-43a5-a131-a649f122e15a.png)

### Keywords [Keyword, Group, and Term elements; matchStyle and caseSensitive attributes]

A `Keyword` element consists of:

- A name (for example, `Keyword_badge` and `Keyword_employee` in the diagram below). This name value is referenced in the `Match` element in the pattern (for example, `<Match id=Keyword_badge>` in the diagram below).

- The `Group` and `matchStyle` attributes used together. The valid values for `matchStyle` are:

  - `word`: Identifies whole words surrounded by white space or other delimiters. You should always use this value unless you need to match parts of words or match words in Asian languages. 

  - `string`: Identifies strings no matter what they're surrounded by. For example, "id" will match "bid" and "idea". Use this value only when you need to match Asian words or if your keyword may be included as part of other strings.

- `Term` elements that consist of:

  - Each word in the keyword list specified in a `<Term>` `</Term>` tag. For example, in the diagram below, the words in the `Keyword_badge` keyword list are `card` and `badge`.

  - The optional `caseSensitive="true"` attribute on any term. This value specifies that the content must match the keyword exactly, including lowercase and uppercase letters.

![XML markup showing Match elements referencing keywords](media/e729ba27-dec6-46f4-9242-584c6c12fd85.png)

## Different combinations of evidence [Any element, minMatches and maxMatches attributes]

As explained earlier, all `Match` elements in a `Pattern` element are always joined with the implicit AND operator (all matches must be satisfied before the entire pattern is satisfied).

However, you can create more flexible matching logic by using `Any` elements.

An `Any` element defines a group of `Match` elements. You use the `minMatches` and `maxMatches` attributes on the `Any` element to match some, all, none, or an exact subset of the collection of `Match` elements. Note that the `minMatches` and `maxMatches` attributes specify the number of `Match` elements that are satisfied (or not satisfied), not the individual number of hits returned by some or all of the `Match` elements. For example:

- **Use the minMatches attribute by itself**: For example, your `Any` element has three `Match` elements, and you use the value `minMatches=1`. The `Any` element is satisfied if one or more of the `Match` elements are satisfied (regardless of the number of times). This effectively joins the `Match` elements with the implict OR operator. In the following diagram, the `Any` element is satisfied if a US-formatted date or a keyword from the `Keyword_badge` or `Keyword_Employee` keyword lists is found.

  ![XML markup showing Any element with minMatches attribute](media/385db1b1-571b-4a05-81b3-db28f5099c17.png)

- **Set minMatches and maxMatches to the same value**: For example, your `Any` element has three `Match` elements, and you use the values `minMatches=1` and `maxMatches=1`. This `Any` element is satisfied if _only_ one of the `Match` elements is satisfied; any more than that, and the `Any` element isn't satisfied. In the following diagram, the `Any` element is statisfied only if exactly one date or keyword is found.

  ![XML markup showing Any element wtih minMatches and maxMatches attributes](media/97b10002-7781-42e8-ac5a-20ad8c5a887e.png)

- **Set minMatches and maxMatches to 0**: The `Any` element is satisfied by the _absence_ of specific evidence (you have a keyword list or other evidence that's likely to indicate false positives).

  For example, the employee ID entity looks for the keyword "card" because it might refer to an "ID card". However, you can add "credit card" as a keyword to a `Keyword_Fasle_Positives` keyword list that defines the list of terms to exclude from satisfying the pattern.

  ![XML markup showing maxMatches attribute value of zero](media/f81d44e5-3db8-48a8-8919-f483a386afdf.png)


## Proximity of evidence [patternsProximity attribute]


![XML markup showing patternsProximity attribute](media/e97eb7dc-b897-4e11-9325-91c742d9839b.png)

The `patternsProximity` attribute each `Entity` element defines the required distance (in Unicode characters) of the supporting evidence in the pattern (keywords, dates, etc.) from the primary item that the pattern is looking for (the employee ID number, social security number, etc.) as defined by the `IdMatch` element. This distance to the left or right of the primary item is called the _proximity window_.

![Diagram of proximity window](media/b593dfd1-5eef-4d79-8726-a28923f7c31e.png)

The following diagram illustrates how the proximity window affects pattern matching where the primary social security number (SSN) requires at least one corroborating match of keyword or date.

![Diagram of corroborative evidence and proximity window](media/dc68e38e-dfa1-45b8-b204-89c8ba121f96.png)

**Note**: For email messages, the message body and each attachment are treated as separate items. One proximity window is the message body; another is each individual message attachment.

## What are the right confidence levels for different patterns? [confidenceLevel attribute, recommendedConfidence attribute]

The more evidence that a pattern requires, the more confidence you have that an actual entity (such as employee ID) has been identified when the pattern is matched. For example, you have more confidence in a pattern that requires a nine-digit ID number, hire date, and keyword in close proximity, than you do in a pattern that requires only a nine-digit ID number.

Every `Pattern` element in an entity has a required `confidenceLevel` attribute that has an integer value from 1 to 100. You can think of the `confidenceLevel` value as  a unique ID for each pattern in an entity, because each pattern requires a different confidence level. A lower confidence patterm should have a lower value than a higher confidence patter, but the actual number value doesn't matter; simply pick numbers that make sense to your compliance team. You can reference these confidence levels in the conditions of the rules in the DLP policies that you create.

![XML markup showing Pattern elements with different values for confidenceLevel attribute](media/301e0ba1-2deb-4add-977b-f6e9e18fba8b.png)

The `Entity` element itself also has a `recommendedConfidence` attribute. You can think of this value as the default confidence level for the whole entity. When you create a rule in a DLP policy, if you don't specify a confidence level for the rule to use, the rule will use the recommended confidence level for the sensitive information type.

## Other languages [LocalizedStrings element]

If your compliance team uses the Security & Compliance Center to create DLP policies in different languages, you can provide localized versions of the name and description of the entity (custom sensitive information type) that the compliance team sees.

The `LocalizedStrings` element that exists in the XML file outside of the `Entity` element defines the name and description of the entity (sensitive information type) in one or more languages. The `LocalizedStrings` element contains a `Resource` element that references the ID (GUID) of the entity. Each `Resource` element contains one or more `Name` and `Description` elements that use a `langcode` attributes to provide a localized name and description of the sensitive information type for each specified language. You can also use the `default` attribute to specify the default language.

![XML markup showing contents of LocalizedStrings element](media/a96fc34a-b93d-498f-8b92-285b16a7bbe6.png)

**Notes**: Localized strings control how your custom sensitive information type appears to admins and compliance officers in the Security & Compliance Center. You can't use localized strings to provide different localized versions of keyword lists or regular expressions.

## Rule package identifiers [RulePackage element]

The custom rule package itself requires some identitfying information. You can use the following markup as a template and replace the ". . ." placeholders with your own values. The diagram at the end of this section

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

- You need to generate a unique GUID for the custom rule package (the `RulePack` ID), which must be different than the GUID that you generated for the entity. You can generate a GUID in Windows PowerShell by running the following command:

    ```
    [guid]::NewGuid()
    ```

- Mantaining the `Version` element is important. When you upload your custom rule package for the first time, Office 365 notes the version number. Later, if you try to upload a modified version of the same rule package, the verson number must be different; otherwise, the upload will fail.


The following diagram shows what your `RulePackage` element should look like:

![XML markup showing RulePack element](media/fd0f31a7-c3ee-43cd-a71b-6a3813b21155.png)

### Potential validation issues to be aware of

When you upload your rule package XML file, the system validates the XML and checks for known bad patterns and obvious performance issues. These are some known regular expression issues that the validation checks for:

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

## Reference: Rule package XML schema definition

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
