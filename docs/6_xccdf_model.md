# 6.XCCDF Data Model

## 6.1 Introduction

The XCCDF data model and its XML representation are intended to be platform-independent and portable to foster broad adoption and sharing of rules. The fundamental data model for XCCDF consists of the following main element data types:

- **Benchmark.** An XCCDF benchmark document holds an _\<xccdf:Benchmark\>_ element, which acts as a container for other elements, including _\<xccdf:Group\>_, _\<xccdf:Rule\>_, _\<xccdf:Value\>_, _\<xccdf:Profile\>,_ and _\<xccdf:TestResult\>_ elements.
- **Item.** An item is a named constituent of an _\<xccdf:Benchmark\>_. There are three types of items:
  - **Group.** An _\<xccdf:Group\>_ holds other items. An _\<xccdf:Group\>_ collects related _\<xccdf:Rule\>_ and _\<xccdf:Value\>_ elements into a common structure and can provide descriptive text and references about them. An _\<xccdf:Group\>_ allows benchmark users to select and deselect related _\<xccdf:Rule\>_ elements together; since a deselected _\<xccdf:Group\>_ is not processed, none of its contained items are processed either. Selection of an _\<xccdf:Group\>_ allows its children to be processed normally based on their individual selection states.
  - **Rule.** An _\<xccdf:Rule\>_ element holds check references and can also hold remediation information.
  - **Value.** An _\<xccdf:Value\>_ element is a named data value that can be substituted into other items' properties or into checks. It can have an associated data type and metadata that express how the value should be used and how it can be tailored.

- **Profile.** An _\<xccdf:Profile\>_ element is a named tailoring of a benchmark using a collection of attributed references to _\<xccdf:Rule\>_, _\<xccdf:Group\>_, and _\<xccdf:Value\>_ elements. It allows definition of named levels or baselines in a benchmark (see Section 5.2).
- **TestResult.** An _\<xccdf:TestResult\>_ element holds the results of performing a test or check against a single target device or system. An _\<xccdf:TestResult\>_ element references _\<xccdf:Rule\>_ and _\<xccdf:Value\>_ elements and may also reference an _\<xccdf:Profile\>_ element.
- **Tailoring.** A tailoring document holds exactly one _\<xccdf:Tailoring\>_ element, which contains _\<xccdf:Profile\>_ elements to modify the behavior of an _\<xccdf:Benchmark\>_.

The rest of this section explains the relationships between these main element data types and examines each of the types in more detail. Section 6.2 discusses general information that applies to most or all of the main element data types. Sections 6.3 through 6.7 cover _\<xccdf:Benchmark\>_, item (_\<xccdf:Group\>_, _\<xccdf:Rule\>_, _\<xccdf:Value\>_), _\<xccdf:Profile\>_, _\<xccdf:TestResult\>_, and _\<xccdf:Tailoring\>_ elements, respectively.

## 6.2 General XML Information

### 6.2.1 XCCDF Namespace and XML Schema

The namespace URI for this specification shall be "http://checklists.nist.gov/xccdf/1.2". Applications that process XCCDF should use the namespace URI to decide whether or not they can process a given document. The XML representation of XCCDF is expressed as an XML schema at [http://scap.nist.gov/specifications/xccdf/#resource-1.2](http://scap.nist.gov/specifications/xccdf/#resource-1.2). The XML schema implementation of XCCDF shall be the authoritative XML binding definition for XCCDF.

### 6.2.2 Element and Attribute Formatting

The structure of an XCCDF document supports transformation into HTML and various XML formats to promote portability and interoperability. The XCCDF language also allows for the inclusion of content that does not contribute directly to the technical content, such as an introduction, a rationale, warnings, and external references. XCCDF also provides mechanisms to document authors for formatting text, including images, and referencing other information resources (e.g., prose publications), but these mechanisms are separable from the text itself so they can be filtered out by applications that do not support or require them.

Throughout the rest of this section, there are tables that show the possible properties of each main element in the XCCDF data model. In the tables, properties of type "identifier" must be strings obeying the definition of "NCName" from [XMLSCHEMA]. Properties of type "string" and "text" must not include XHTML formatting. Properties of type "HTML-enabled text" are string data that may include embedded formatting, presentation, and hyperlink structure; if present, these must be expressed using Extensible Hypertext Markup Language (XHTML) Basic tags [XHTML]. The core modules noted in [XHTML]and the Presentation module may be used for this.

XHTML markup allows authors to specify how the content of certain fields should be displayed to a user. However, while this allows authors to specify every detail of a field's display, authors may use classifiers in these fields, using class attributes in \<div\> or \<span\> elements. These tell products the type of content contained within a text block and allow the products to display this content according to their own conventions. Authors may wish to do this so their content fits better with other automated formatting choices made by a product or a stylesheet, and also because some products may not support the complete range of HTML tags in their displays, thus causing some explicit formatting to be ignored. A list of recommended class values appears in Table 2. Authors may use class values not included in this list, and it is optional for products to have special formatting for any of the listed classifiers. However, both authors and products should use these class designators when appropriate to provide more effective display of content. Use of these class attribute values shall be applicable wherever HTML content can be included in XCCDF documents.

**Table 2: Recommended Class Values**

| Class Value | Meaning |
| --- | --- |
| license | Licensing and use information |
| copyright | Copyright and ownership information |
| tangent | A block of text that contains tangentially related information (possibly appropriate for inclusion as a sidebar or a pop-up) |
| warning | Pitfalls or cautions relative to the surrounding text. High-level and general warnings should appear in designated warning fields, if available. |
| critical | Content of critical importance to the user |
| example | An example of some kind |
| instructions | Special instructions to the user |
| default | General information. Empty or absent class attributes also imply "default" appearance. This tag allows authors to explicitly indicate that text should appear in the default format. |

### 6.2.3 Element Identifiers

The elements listed in Table 3 have special conventions around the format of their identifiers (_@id_ attribute). Authors MUST follow these conventions because they make it easier to preserve the global uniqueness of the resulting identifiers.

**Table 3: Element Identifier Format Conventions**

| Element | Format Convention |
| --- | --- |
| Benchmark | xccdf\_*namespace*\_benchmark\_*name* |
| Profile | xccdf\_*namespace*\_profile\_*name* |
| Group | xccdf\_*namespace*\_group\_*name* |
| Rule | xccdf\_*namespace*\_rule\_*name* |
| Value | xccdf\_*namespace*\_value\_*name* |
| TestResult | xccdf\_*namespace*\_testresult\_*name* |
| Tailoring | xccdf\_*namespace*\_tailoring\_*name* |

In Table 3, _namespace_ contains a valid reverse-DNS style string (limited to letters, numbers, periods, and the hyphen character) that is associated with the content author. Examples include "com.acme.finance" and "gov.tla". These _namespace_ strings may have any number of parts, and benchmark consumers processing them SHALL treat them as case-insensitive. (That is, com.ABC is considered identical to com.abc.) This association with the content author is solely to ensure that different content authors will not use the same namespace value, as this could lead to identifier collisions. Readers should not automatically infer any special authority or trust of the content based on the namespace since reuse of the element or changes to referenced content may not align with the author's original intent.The _name_ component in the format conventions may be any NCName-compliant string [XMLSCHEMA]. Identifier fields other than those noted in Table 3 have no additional formatting conventions beyond compliance with the NCName data type.

See the example _\<xccdf:Profile\>_ in Section 6.5.1 for several examples of the element identifier format conventions.

### 6.2.4 \<xccdf:metadata\> Element

XCCDF supports inclusion of metadata about a document, including title, name of author(s), organization providing the guidance, version number, release date, update URL, and a description. This is particularly useful for facilitating the discovery and retrieval of XCCDF checklists from public repositories.

When used, the _\<xccdf:metadata\>_ element shall contain document metadata expressed in XML. The metadata element can appear one or more times as a child of the _\<xccdf:Benchmark\>_, _\<xccdf:Rule\>_, _\<xccdf:Group\>_, _\<xccdf:Value\>_, _\<xccdf:Profile\>_, _\<xccdf:TestResult\>_, _\<xccdf:rule-result\>_, and _\<xccdf:Tailoring\>_ elements. The _\<xccdf:Benchmark\>_ element SHOULD contain metadata information formatted using the Dublin Core Metadata Initiative (DCMI) Simple DC Element specification, as described in [DCES] and [DCXML]. Benchmark consumers should be prepared to process Dublin Core metadata in the _\<xccdf:metadata\>_ element. An example of the Dublin Core format is shown below.

```xml
<xccdf:metadata xmlns:dc="http://purl.org/dc/elements/1.1/">
    <dc:title>Security Benchmark for Ethernet Hubs</dc:title>
    <dc:creator>James Smith</dc:creator>
    <dc:publisher>Center for Internet Security</dc:publisher>
    <dc:subject>network security for layer 2 devices</dc:subject>
</xccdf:metadata>
```

Elements that support the _\<xccdf:metadata\>_ element MAY use any desired metadata format; for _\<xccdf:Benchmark\>_, this is in addition to the Dublin Core recommendations already mentioned. Any XML metadata structures, including ad hoc structures, may be included in an_ \<xccdf:metadata\>_ element. Because any structures can appear in this field, authors should tag metadata with the _xmlns_ prefix [XMLNAME] and, if available, the _@xsi:schemaLocation_ attribute in order to identify the metadata structure utilized.Metadata should comply with existing commercial or government metadata specifications to allow benchmarks to be discovered and indexed.

Metadata is a powerful feature for authors. However, XCCDF puts some limits on the use of metadata:

- Metadata should not replace the functionality of existing fields within XCCDF. If the information matches the common use of some other XCCDF field, the information must appear in that XCCDF field. It MAY also appear in the _\<xccdf:metadata\>_ element.
- Metadata shall not change the processing model as outlined in Section 7. Benchmark consumer products shall perform XCCDF assessments in the same way regardless of the presence of metadata. Metadata may still be used to support assessment features that are outside the purview of XCCDF—for example, to hold post-processing instructions to be performed on the completed XCCDF results or to display additional information to the user before or during the assessment.
- Metadata SHOULD not alter the character content of the XCCDF properties used to generate output during document generation. Contents of the _\<xccdf:metadata\>_ element MAY be added to the output above and beyond the XCCDF properties. Metadata may contain instructions that cause different stylistic conventions to be adopted in the conversion of an XCCDF document to prose.

### 6.2.5 Platform Names

The Common Platform Enumeration (CPE) specification allows a specific hardware or software platform to be identified by a unique name. CPE names can express only single platforms (e.g., "cpe:2.3:o:microsoft:windows\_xp:\*:gold:professional:\*:\*:\*:\*:\*" for Microsoft Windows XP Professional Edition). CPE applicability language statements can express complex logical constructions of CPE names.

_\<xccdf:Benchmark\>_, _\<xccdf:Profile\>_, _\<xccdf:Rule\>_, and _\<xccdf:Group\>_ elements may be qualified by applicable platform using the _\<xccdf:platform\>_ element. _\<xccdf:TestResult\>_ elements may also include _\<xccdf:platform\>_ elements. The _\<xccdf:platform\>_ element's _@idref_ attribute may hold a reference to either a CPE name or the _@id_ attribute of a CPE applicability language expression that SHALL be defined as a child of the _\<xccdf:Benchmark\>_ using a _\<cpe2:platform-specification\>_ element (see Table 4). (The syntax for referring to a local CPE applicability language identifier SHOULD be a "#" character before the identifier, such as "#cpeid1".)

Within XCCDF documents, all CPE names SHALL comply with the CPE 2.3 Naming specification [IR7695], and all CPE applicability language expressions SHALL comply with the CPE 2.3 Applicability Language specification [IR7698]. CPE 2.0 names MAY be used for backwards compatibility, but their use has been deprecated for XCCDF 1.2. All CPE 2.3 names and applicability language expressions in XCCDF documents SHOULD use formatted string bindings but MAY use URI bindings instead, both as defined in [IR7695].

Here is an example of a _\<cpe2:platform-specification\>_ element:

```xml
<cpe2:platform-specification>
  <cpe2:platform id="xp_and_acrobat_9.0">
    <cpe2:logical-test operator="AND" negate="false">
      <cpe2:fact-ref
         name="cpe:2.3:o:microsoft:windows\_xp:\*:\*:\*:\*:\*:\*:\*:\*"/>
      <cpe2:fact-ref
         name="cpe:2.3:a:adobe:acrobat:9.0:\*:\*:\*:\*:\*:\*:\*"/>
    </cpe2:logical-test>
   </cpe2:platform>
</cpe2:platform-specification>
<xccdf:platform idref="#xp_and_acrobat_9.0"/>
```

If an _\<xccdf:Profile\>_, _\<xccdf:Rule\>_, or _\<xccdf:Group\>_ does not possess any _\<xccdf:platform\>_ elements, then it shall apply to the same set of platforms as its nearest enclosing ancestor _\<xccdf:Group\>_ or _\<xccdf:Benchmark\>_. If the enclosing ancestor has no _\<xccdf:platform\>_ element, then consult the next enclosing ancestor, repeating until either an ancestor has an _\<xccdf:platform\>_ element or the _\<xccdf:Benchmark\>_ element is reached. If no ancestor, including the _\<xccdf:Benchmark\>_ element, possesses any _\<xccdf:platform\>_ elements, then the _\<xccdf:Profile\>,\<xccdf:Rule\>_, or _\<xccdf:Group\>_ shall nominally apply to all platforms.

### 6.2.6 \<xccdf:reference\> Element

The _\<xccdf:reference\>_ element provides a reference to a document or resource where the user can learn more about the subject of the parent element. It may be included within _\<xccdf:Benchmark\>_, _\<xccdf:Rule\>_, _\<xccdf:Group\>_, _\<xccdf:Value\>_, and _\<xccdf:Profile\>_ elements. When used, it SHALL have either a simple string value or a value consisting of simple Dublin Core elements as described in [DCXML]. It MAY also have an _@href_ attribute that gives a URL for the referenced resource. References SHOULD be given as Dublin Core descriptions; a bare string may be used for simplicity. If a bare string appears, then it is taken to be the string content for a _\<dc:title\>_ element. For more information, consult [DCES]. Multiple _\<xccdf:reference\>_ elements may appear; a benchmark consumer product MAY concatenate them or put them into a reference list, and MAY choose to number them.

### 6.2.7 \<xccdf:signature\> Element

XCCDF supports a digital signature mechanism whereby XCCDF document users can validate the integrity, origin, and authenticity of documents. XCCDF provides a means to hold such signatures and a uniform method for applying and validating them [XMLDSIG].

The _\<xccdf:signature\>_ element may hold an enveloped digital signature asserting authorship and allowing verification of the integrity of associated data (e.g., its parent element, other documents, portions of other documents). The signature shall apply only after inclusion (i.e., XInclude) processing. The _\<xccdf:signature\>_ element is optional, and it may appear as a child of the _\<xccdf:Benchmark\>_, _\<xccdf:Rule\>_, _\<xccdf:Group\>_, _\<xccdf:Value\>_, _\<xccdf:Profile\>_, _\<xccdf:TestResult\>_, or _\<xccdf:Tailoring\>_ elements.

Any digital signature format employed for XCCDF must be capable of identifying the signer, storing all information needed to verify the signature (usually a certificate or certificate chain), and detecting any change to the signed content. XCCDF products that support signatures must support the W3C XML-Signature standard enveloped signatures, as defined in [XMLDSIG].

If multiple signatures are needed in an XCCDF document, at most one of them MAY be enveloped; all others must be detached [XMLDSIG Section 2].

The single child element of the _\<xccdf:signature\>_ element should be _\<dsig:Signature\>_ and that _\<dsig:Signature\>_ should contain one or more _\<dsig:Reference\>_ elements indicating each XML element to be included in the signature. The _\<dsig:Reference\>_ URI should be a relative URI to the _@Id_ attribute value of the enclosing object (prefixed by "#"). For example, if the Id="abc", then the _\<Reference\>_ URI would be "#abc".

### 6.2.8 Status Tracking

XCCDF provides two elements for status tracking: _\<xccdf:status\>_ and _\<xccdf:dc-status\>_. Both elements are available for _\<xccdf:Benchmark\>_, _\<xccdf:Rule\>_, _\<xccdf:Group\>_, _\<xccdf:Value\>_, _\<xccdf:Profile\>_, and _\<xccdf:Tailoring\>_ elements.

The intent of the _\<xccdf:status\>_ element is to record the maturity or consensus level for its parent element. Permissible values are "incomplete" (under initial development),"draft" (released in draft state), "interim" (revised and in the process of being finalized), "accepted" (released as final),and "deprecated" (no longer needed). If there is more than one _\<xccdf:status\>_ element for a single parent element, then every instance of the _\<xccdf:status\>_ element MUST have a date attribute, and the _\<xccdf:status\>_ element with the latest date shall be considered the latest status.

The _\<xccdf:dc-status\>_ element holds additional status information using the Dublin Core format, expressed as elements of the DCMI Simple DC Element specification, as described in [DCES] and [DCXML].

### 6.2.9 Text Substitution

XCCDF provides capabilities to perform substitution within certain properties of an _\<xccdf:Benchmark\>_.

An _\<xccdf:plain-text\>_ element is a reusable text block for reference by the _\<xccdf:sub\>_ element. This allows text to be defined once and then reused multiple times. Each _\<xccdf:plain-text\>_ element MUST have a unique NCName identifier. The identifiers for _\<xccdf:plain-text\>_ MUST NOT collide with item identifiers within the _\<xccdf:Benchmark\>_.

The _\<xccdf:sub\>_ element represents a reference to a parameter value that may be set during tailoring. The _\<xccdf:sub\>_ element is supported by several elements, which are noted as they are defined throughout the rest of this section. The _\<xccdf:sub\>_ element shall not have any content. It must have a single _@idref_ attribute, which must equal the _@id_ attribute of an _\<xccdf:Value\>_ element or an _\<xccdf:plain-text\>_ element. During document generation, each instance of the _\<xccdf:sub\>_ element will be replaced by the text value of the referenced element's contents. If an _\<xccdf:Value\>_ element is referenced, the _\<xccdf:sub\>_ element MAY have a _@use_ attribute that indicates whether the _\<xccdf:Value\>_ element's title or value should replace the _\<xccdf:sub\>_ element. If an _\<xccdf:plain-text\>_ element is referenced, the _@use_ attribute is ignored and the body of the _\<xccdf:plain-text\>_ element is always used in the substitution.

See Section 7.2.3.6.3 for additional requirements related to substitution processing.

### 6.2.10 @xml:lang Attribute

Some textual elements of XCCDF, such as titles and descriptions, support the _@xml:lang_ attribute to denote the language they use.<sup>1</sup>
Elements that have an _@xml:lang_ attribute may appear multiple times within a single parent element to support multiple languages. If there are multiple instances of such a textual element within a single parent element, each instance SHOULD have a value for its _@xml:lang_ attribute. Each value SHOULD specify the natural language locale for which the instance is written (e.g., "en" for English, "fr" for French). Values SHOULD be valid language tags as defined by [RFC5646]. Although any valid language tag MAY be used, only tags containing language codes (with or without region codes) SHOULD be used. All language and region codes used SHOULD be in the _Internet Assigned Numbers Authority (IANA) Language Subtag Registry_ [ILSR]. An example of using the _@xml:lang_ attribute is shown below.

```xml
<xccdf:Value id="xccdf_org.example_value\web-server-port" type="number">
   <xccdf:title xml:lang="en">Web Server Port</xccdf:title>
   <xccdf:title xml:lang="fr">Le Port Du Web Serveur</xccdf:title>
   <xccdf:question xml:lang="en">
      What is the web server's TCP port?
   </xccdf:question>
   <xccdf:question xml:lang="fr">
      Quel est le port du TCP du web serveur?
   </xccdf:question>
   <xccdf:value>80</xccdf:value>
</xccdf:Value>
```

If an element other than _\<xccdf:Benchmark\>_ that supports the _@xml:lang_ attribute omits it, the _@xml:lang_ attribute of the nearest ancestor element that has the _@xml:lang_ attribute SHALL be consulted.

## 6.3 \<xccdf:Benchmark\>

### 6.3.1 Basics

Each XCCDF benchmark document SHALL consist of a single root _\<xccdf:Benchmark\>_ element, which encloses the entire benchmark. The example below illustrates the outermost structure of an XCCDF XML document.

```xml
<?xml version="1.0" ?>
<xccdf:Benchmark id="xccdf_org.example_benchmark_example1" xml:lang="en" Id="toSign"
      xmlns:htm="http://www.w3.org/1999/xhtml"
      xmlns:xccdf="http://checklists.nist.gov/xccdf/1.2"
      xmlns:cpe2-dict="http://cpe.mitre.org/dictionary/2.0"/>
  <xccdf:status date="2010-06-01">draft</xccdf:status>
  <xccdf:title>Example Benchmark File</xccdf:title>
  <xccdf:description>
      A <htm:b>Small</htm:b> Example
  </xccdf:description>
  <xccdf:platform idref="cpe:2.3:o:cisco:ios:12.0:\*:\*:\*:\*:\*:\*:\*"/>
  <xccdf:version>0.2</xccdf:version>
  <xccdf:reference href="http://www.ietf.org/rfc/rfc822.txt">
      Standard for the Format of ARPA Internet Text Messages
  </xccdf:reference>
</xccdf:Benchmark>
```

Figure 1 illustrates a typical internal structure for a Benchmark.

| ![](NISTIR-7275r4-updated-20120315_html_e34a4e492b9f2def.gif) |
| --- |

**Figure 1: Typical Structure of a Benchmark**

The possible inheritance relations between Rule and Value instances are constrained by the tree structure of the Benchmark. All extension relationships must be resolved before the Benchmark can be applied. An item may only extend another item of the same type that is visible from its scope. In other words, an item Y may extend a base item X, as long as they are the same type and one of the following visibility conditions holds:

- X is a direct child of the Benchmark.
- X is a direct child of a Group which is also an ancestor<sup>2</sup> of Y.
- X is a direct child of a Group which is extended by any ancestor of Y.

For example, in the Benchmark structure shown in Figure 1, it would be legal for Rule (g) to extend Rule (f) or extend Rule (h). It would not be legal for Rule (i) to extend Rule (m), because (m) is not visible from the scope of (i). It would not be legal for Rule (l) to extend Value (k), because they are not of the same type.

The ability for an item to be extended gives benchmark authors the ability to create variations or specialized versions of items without making copies. An extending item can inherit properties from the extended (base item) and sometimes can override properties with new values if needed; however, there are several restrictions on these inheritance capabilities. For example, some properties have different inheritance behaviors depending on how their _@override_ attribute is set. Also, group extension has been deprecated as of XCCDF 1.2 (see Table 42) and authors are strongly discouraged from using it due to the risks it poses to interoperability. See Section 7.2.2 for more information on inheritance.

Circular dependencies caused by extension MUST not be defined.

### 6.3.2 Properties

Table 4 describes the _\<xccdf:Benchmark\>_ element's properties.

In this table and in similar tables throughout the rest of the section, the Count column indicates how many times each property shall be permitted within a single instance of the parent element. Each Count is expressed as either a single value or a range of values. If the Count entry for a property includes the value 0, the property is optional, otherwise the property is required. If the Count entry for a property includes the value n, the property MAY be used more than one time, with no upper bound. Here are some examples:

- 1: the property shall be used once
- 1-n: the property shall be used at least once and may be used more than once
- 0-1: the property is optional and may be used at most once
- 0-n: the property is optional and may be used more than once

**Table 4: \<xccdf:Benchmark\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| status (element) | _special_ | 1-n | Status of the benchmark (REQUIRED) and date at which it attained that status (OPTIONAL). The status should indicate the level of maturity or consensus for the benchmark. See Section 6.2.8. |
| dc-status (element) | _special_ | 0-n | Holds additional status information using the Dublin Core format. See Section 6.2.8. |
| title (element) | string | 0-n | Title of the benchmark; a benchmark SHOULD have a title. It may have an _@xml:lang_ attribute (see Section 6.2.10) and/or an _@override_ attribute (see Section 6.3.1). |
| description (element) | HTML-enabled text | 0-n | Text that describes the benchmark; a benchmark SHOULD have a description. It MAY have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9), an _@override_ attribute (see Section 6.3.1), and/or an _@xml:lang_ attribute (see Section 6.2.10). |
| notice (element) | HTML-enabled text | 0-n | Legal notices (licensing information, terms of use, etc.), copyright statements, warnings, and other advisory notices about this benchmark and its use. Each element shall have a unique NCName identifier as its _@id_ attribute. Each element MAY have an _@xml:lang_ attribute (see Section 6.2.10) and/or an _@xml:base_ attribute (which defines the context for all relative URIs within the _\<xccdf:Benchmark\>_ element). |
| front-matter (element) | HTML-enabled text | 0-n | Introductory matter for the beginning of the benchmark document; intended for use during Document Generation processing only (see Section 7.1). It MAY have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9), an _@override_ attribute (see Section 6.3.1), and/or an _@xml:lang_ attribute (see Section 6.2.10). |
| rear-matter (element) | HTML-enabled text | 0-n | Concluding material for the end of the benchmark document; intended for use during Document Generation processing only (see Section 7.1). It MAY have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9), an _@override_ attribute (see Section 6.3.1), and/or an _@xml:lang_ attribute (see Section 6.2.10). |
| reference (element) | _special_ | 0-n | Supporting references for the benchmark document. See Section 6.2.6. |
| plain-text (element) | string | 0-n | Definitions for reusable text blocks, each with a unique identifier. See Section 6.2.9. |
| cpe2: platform-specification (element) | _special_ | 0-1 | A list of identifiers for complex platform definitions, written in CPE applicability language format. Authors may define complex platforms within this element, and then use their locally unique identifiers anywhere in the _\<xccdf:Benchmark\>_ element in place of a CPE name. See Section 6.2.5. |
| platform (element) | string | 0-n | Applicable platforms for this benchmark. Authors should use the element to identify the systems or products to which the benchmark applies. See Section 6.2.5. |
| version (element) | string | 1 | Version number of the benchmark, with an optional _@time_ timestamp attribute (when the version was completed) and an optional _@update_ URI attribute (where updates may be obtained). |
| metadata (element) | _special_ | 0-n | XML metadata for the benchmark. Benchmark metadata allows authorship, publisher, support, and other information to be embedded in a benchmark. See Section 6.2.4. |
| model (element) | URI | 0-n | URIs of suggested scoring models to be used when computing a score for this benchmark. See Section 7.3.2 for more information on models. Parameters may be provided as needed for particular models through the _@param_ attribute. _@param_ attributes with equal name values shall not appear as children of the same element. |
| Profile (element) | _special_ | 0-n | Profiles that reference and customize sets of items in the benchmark; see Section 6.5. |
| Value (element) | _special_ | 0-n | Parameter values that support Rules and descriptions in the benchmark; see Section 6.4.5. |
| Group (element) | _special_ | 0-n | Groups that comprise the benchmark; each may contain additional Values, Rules, and other Groups. See Section 6.4.3. |
| Rule (element) | _special_ | 0-n | Rules that comprise the benchmark; see Section 6.4.4. |
| TestResult (element) | _special_ | 0-n | Benchmark test result records (one per benchmark run); see Section 6.6. |
| signature (element) | _special_ | 0-1 | A digital signature asserting authorship and allowing verification of the integrity of the benchmark. See Section 6.2.7. |
| id (attribute) | _special_ | 1 | Unique benchmark identifier. See Section 6.2.3. |
| Id (attribute) | _special_ | 0-1 | An identifier used for referencing elements included in an XML signature. See Section 6.2.7. |
| resolved (attribute) | boolean | 0-1 | True if benchmark has already undergone the resolution process (default: false). See Section 7.2.2. |
| style (attribute) | string | 0-1 | Name of a benchmark authoring style or set of conventions or constraints to which this benchmark conforms (e.g., "SCAP 1.2"). |
| style-href (attribute) | URI | 0-1 | URL of a supplementary stylesheet or schema extension that can be used to verify conformance to the named style. |
| xml:lang (attribute) | _special_ | 0-1 | The language for the benchmark; see Section 6.2.10. |

The properties that comprise an _\<xccdf:Benchmark\>_ or items are an ordered sequence of property values. The order of _\<xccdf:Group\>_ and _\<xccdf:Rule\>_ child elements matters for the appearance of a generated document, so _\<xccdf:Group\>_ and _\<xccdf:Rule\>_ child elements may be freely intermingled. All other child elements must appear in the order shown in Table 4, and multiple instances of a child element must be adjacent.

## 6.4 Item Elements

### 6.4.1 Properties

Table 5 describes the properties common to all three classes of items: _\<xccdf:Group\>_, _\<xccdf:Rule\>_, and _\<xccdf:Value\>_ elements.

**Table 5: Item Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| status (element) | _special_ | 0-n | Status of the item and date at which it attained that status. Benchmark authors may use this element to record the maturity or consensus level for item elements in the benchmark. If an item does not have an explicit status given, then its status shall be that of its parent. See Section 6.2.8. |
| dc-status (element) | _special_ | 0-n | Holds additional status information using the Dublin Core format. See Section 6.2.8. |
| version (element) | string | 0-1 | Version number of the item, with an optional _@time_ timestamp attribute (when the version was completed) and an optional _@update_ URI attribute (where updates may be obtained). |
| title (element) | string | 0-n | Title of the item. Every item should have a title, because this helps people understand the purpose of the item. It may have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9), an _@xml:lang_ attribute (see Section 6.2.10), and/or an _@override_ attribute (see Section 6.3.1). |
| description (element) | HTML-enabled text | 0-n | Text that describes the item. It MAY have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9), an _@override_ attribute (see Section 6.3.1), and/or an _@xml:lang_ attribute (see Section 6.2.10). |
| warning (element) | HTML-enabled text | 0-n | A note or caveat about the item intended to convey important cautionary information for the benchmark user (e.g., "Complying with this rule will cause the system to reject all IP packets"). If multiple warning elements appear, benchmark consumers should concatenate them for generating reports or documents. Benchmark consumers may present this information in a special manner in generated documents.It MAY have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9), an _@override_ attribute (see Section 6.3.1), and/or an _@xml:lang_ attribute (see Section 6.2.10). Also, see Section 6.4.2 for the possible values of the warning element's optional _@category_ attribute, which provides a hint as to the nature of the warning. |
| question (element) | string | 0-n | Interrogative text to present to the user during tailoring. It may also be included into a generated document. For _\<xccdf:Rule\>_ and _\<xccdf:Group\>_ elements, the question text should be a simple binary (yes/no) question because it is supporting the selection aspect of tailoring. For _\<xccdf:Value\>_ elements, the question should solicit the user to provide a specific value. Tools MAY also display constraints on values and any defaults as specified by the other \<xccdf:Value\> properties (see Section 6.4.5). It MAY have an _@override_ attribute (see Section 6.3.1) and/or an _@xml:lang_ attribute (see Section 6.2.10). |
| reference (element) | _special_ | 0-n | References where the user can learn more about the subject of this item. See Section 6.2.6. |
| metadata (element) | _special_ | 0-n | XML metadata associated with this item, such as sources, special information, or other details. See Section 6.2.4. |
| abstract (attribute) | boolean | 0-1 | If true, then this item is abstract and exists only to be extended (default: false). _ **The use of this attribute for** _ _ **\<xccdf:Group\>** _ _**elements is deprecated and should be avoided (see Table 42).**_ |
| cluster-id (attribute) | identifier | 0-1 | An identifier to be used as a means to identify (refer to) related _\<xccdf:Group\>_, _\<xccdf:Rule\>_, and _\<xccdf:Value\>_ elements throughout the _\<xccdf:Benchmark\>._ It designates membership in a cluster of items, which are used for controlling items via profiles. All the items with the same cluster identifier belong to the same cluster. A selector in an _\<xccdf:Profile\>_may refer to a cluster, thus making it easier for authors to create and maintain profiles in a complex benchmark. |
| extends (attribute) | identifier | 0-1 | The identifier of an item on which to base this item. If present, it MUST have a value equal to the _@id_ attribute of another item. See Section 6.3.1 and Section 7.2.2. _ **The use of this attribute for** _ _ **\<xccdf:Group\>** _ _**elements is deprecated and should be avoided (see Table 42).**_ |
| hidden (attribute) | boolean | 0-1 | If this item should be excluded from any generated documents (default: false). For example, an author might set the _@hidden_ attribute on an incomplete item in a draft benchmark. The item may still be used during assessments, but it shall not appear in generated documents. |
| prohibitChanges (attribute) | boolean | 0-1 | If benchmark producers should prohibit changes to this item during tailoring (default: false). An author should use this when they do not want to allow end users to change the item. For example, benchmark users may use this to define items that are integral to compliance, or enterprise security officers may use it to constrain a benchmark so it reflects organizational policies. |
| xml:lang (attribute) | _special_ | 0-1 | The language for the item; see Section 6.2.10. |
| xml:base (attribute) | _special_ | 0-1 | The context for all relative URIs within the item. |
| Id (attribute) | _special_ | 0-1 | An identifier used for referencing elements included in an XML signature. See Section 6.2.7. |

In addition to the properties listed in Table 5, _\<xccdf:Group\>_ and _\<xccdf:Rule\>_ elements also support the properties listed in Table 6. See Sections 6.4.3, 6.4.4, and 6.4.5 for information on additional properties specific to _\<xccdf:Group\>_, _\<xccdf:Rule\>_, and _\<xccdf:Value\>_ elements, respectively.

**Table 6: Properties Specific to \<xccdf:Group\> and \<xccdf:Rule\> Elements**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| rationale (element) | HTML-enabled text | 0-n | Descriptive text giving rationale or motivations for abiding by this group/rule (i.e., why it is important to the security of the target platform). It MAY have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9), an _@override_ attribute (see Section 6.3.1), and/or an _@xml:lang_ attribute (see Section 6.2.10). |
| platform (element) | _special_ | 0-n | Platforms to which this group/rule applies. See Section 6.2.5. |
| requires (element) | _special_ | 0-n | The identifiers of other _\<xccdf:Group\>_ or _\<xccdf:Rule\>_ elements in the _\<xccdf:Benchmark\>_ that must be selected for this group/rule to be evaluated and scored properly. Each _\<xccdf:requires\>_ element specifies a list of one or more required items by their identifiers, using the _@idref_ attribute. If at least one of the specified _\<xccdf:Group\>_ or _\<xccdf:Rule\>_ elements is selected, the requirement is met.An _\<xccdf:requires\>_ element that looked like _\<xccdf:requires idref="A B C"\>_ could be read as "requires that item A or item B or item C be selected". |
| conflicts (element) | identifier | 0-n | The identifier of another _\<xccdf:Group\>_ or _\<xccdf:Rule\>_ in the _\<xccdf:Benchmark\>_ that must be unselected for this group/rule to be evaluated and scored properly. Each _\<xccdf:conflicts\>_ element specifies a single conflicting item using its _@idref_ attribute. If the specified _\<xccdf:Group\>_ or _\<xccdf:Rule\>_ element is not selected, the requirement is met. |
| selected (attribute) | boolean | 0-1 | If true, this group/rule shall be selected to be processed as part of the _\<xccdf:Benchmark\>_ when it is applied to a target system (default: true). An unselected group shall not be processed, and its contents shall NOT be processed either (i.e., all descendants of an unselected group are implicitly unselected). An unselected rule shall not be checked and shall not contribute to scoring. May be overridden by a profile; see Section 6.5.3. |
| weight (attribute) | decimal | 0-1 | The relative scoring weight of this group/rule, for computing a score, expressed as a non-negative real number (0.0 or greater, maximum 3 digits, default 1.0). It denotes the importance of a group/rule. Under some scoring models, scoring is computed independently for each collection of sibling groups and rules, then normalized as part of the overall scoring process. See Section 7.3.2 for more information on scoring. May be overridden by a profile; see Section 6.5.3. |

### 6.4.2 \<xccdf:warning\> Element

If the _\<xccdf:warning\>_ element's _@category_ attribute is used, it must have one of the values specified in Table 7.

**Table 7: \<xccdf:warning\> Element @category Attribute Values**

| Value | Meaning |
| --- | --- |
| general | Broad or general-purpose warning (default) |
| functionality | Warning about possible impacts to functionality or operational features |
| performance | Warning about changes to target system performance or throughput |
| hardware | Warning about hardware restrictions or possible impacts to hardware |
| legal | Warning about legal implications |
| regulatory | Warning about regulatory obligations or compliance implications |
| management | Warning about impacts to the management or administration of the target system |
| audit | Warning about impacts to audit or logging |
| dependency | Warning about dependencies between this element and other parts of the target system, or version dependencies |

### 6.4.3 \<xccdf:Group\> Element

An _\<xccdf:Group\>_ element contains descriptive information about a portion of a benchmark, as well as _\<xccdf:Rule\>_, _\<xccdf:Value\>_, and/or other _\<xccdf:Group\>_ elements.Table 8 describes the _\<xccdf:Group\>_ element's properties (in addition to the properties in Table 5 and Table 6).

**Table 8: \<xccdf:Group\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| Value (element) | Value | 0-n | Values that belong to this group; see Section 6.4.5. |
| Group (element) | Group | 0-n | Sub-groups under this group; see Section 6.4.3. |
| Rule (element) | Rule | 0-n | Rules that belong to this group; see Section 6.4.4. |
| signature (element) | _special_ | 0-1 | A digital signature asserting authorship and allowing verification of the integrity of the group. See Section 6.2.7. |
| id (attribute) | _special_ | 1 | Unique element identifier; SHALL be used by other elements to refer to this element. See Section 6.2.3. |

### 6.4.4 \<xccdf:Rule\> Element

#### 6.4.4.1 Basics

An _\<xccdf:Rule\>_ element defines a single item to be checked as part of a benchmark, or an extendable base definition for such items. The _\<xccdf:check\>_ child element of an _\<xccdf:Rule\>_ specifies how to verify compliance with a security practice or guideline. See Table 9 and Section 6.4.4.4 for more information on the _\<xccdf:check\>_ element.

The example below shows a very simple _\<xccdf:Rule\>_ element.

```xml
<xccdf:Rule id="xccdf_org.example_rule_pwd-perm" selected="1" weight="6.5" severity="high">
  <xccdf:title>Password File Permission</xccdf:title>
  <xccdf:description>Check the access control on the password file.
    Normal users should not be able to write to it.
  </xccdf:description>
  <xccdf:requires idref="xccdf_org.example_rule_passwd-exists"/>
  <xccdf:fixtext>
    Set permissions on the passwd file to owner-write, world-read
  </xccdf:fixtext>
  <xccdf:fix strategy="restrict" reboot="0" disruption="low">
    chmod 644 /etc/passwd
  </xccdf:fix>
  <xccdf:check system="http://oval.mitre.org/XMLSchema/oval-definitions-5">
    <xccdf:check-content-ref href="ovaldefs.xml" name="oval:org.example:def:123"/>
  </xccdf:check>
</xccdf:Rule>
```

One of XCCDF's main features is the organization and selection of target-applicable groups and rules for performing security and operational checks on systems. XCCDF can access granular and expressive mechanisms for assessing the state of a system according to the rule criteria. Examples of these mechanisms are definitions expressed in the Open Vulnerability and Assessment Language (OVAL) and questionnaires expressed in the Open Checklist Interactive Language (OCIL). These checking mechanisms follow the conceptual model of collecting or acquiring the state of a target system, and then assessing the state for conformance to conditions and criteria expressed as rules.

XCCDF may use rule checking systems other than (or in addition to) OVAL and OCIL. To facilitate this, XCCDF supports referencing by including the appropriate check content location and check reference in the _\<xccdf:check\>_ element. Rule checking systems are defined separately from XCCDF itself so that both XCCDF and the rule checking system can evolve and be used independently. It is helpful for rule checking mechanisms used by XCCDF to comply with the following:

- **The mechanism can express both positive and negative criteria.** A positive criterion means that if certain conditions are met, then the system satisfies the check, while a negative criterion means that if the conditions are met, the system fails the check. Experience has shown that both kinds are necessary for checks.
- **The mechanism can express Boolean combinations of criteria.** It is often impossible to express a high-level security property as a single quantitative or qualitative statement about a system's state. Therefore, the ability to combine statements with 'and' and 'or' is critical.
- **The mechanism can incorporate tailoring values set by the user.** Value modification is important for XCCDF document tailoring, including substitution of tailored values as well as tailoring of a selected set of rules.

#### 6.4.4.2 Properties

Table 9 describes the _\<xccdf:Rule\>_ element's properties (in addition to the properties in Table 5 and Table 6).

**Table 9: \<xccdf:Rule\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| ident (element) | string | 0-n | A globally meaningful identifier for this rule. May be the name or identifier of a security configuration issue or vulnerability that the rule remediates. Has an associated URI that shall denote the organization or naming scheme that assigned the name (see Table 10 for assigned URIs). By setting an _\<xccdf:ident\>_ element on a rule, the benchmark author effectively declares that the rule instantiates, implements, or remediates the issue for which the name was assigned. For example, the _\<xccdf:ident\>_ value might be a CVE identifier; the rule could be a check that the target platform was not subject to the vulnerability named by the CVE identifier, and the URI would be that of the CVE website. An _\<xccdf:ident\>_ element MAY also have other attributes from other namespaces in order to provide additional metadata for the given identifier. |
| impact-metric (element) | string | 0-1 | The potential impact of failure to conform to the rule, expressed as a CVSS 2.0 base vector (defined by NIST IR 7435). _**The property is deprecated and its use should be avoided (see Table 42).**_ A suggested alternate location for impact metric data is the _\<xccdf:metadata\>_ element within the _\<xccdf:Rule\>_. |
| profile-note (element) | HTML-enabled text | 0-n | Text that describes special aspects of the rule related to one or more profiles. This allows an author to document things within rules that are specific to a given profile, and then select the appropriate text based on the selected profile and display it to the reader. The element MUST have a _@tag_ attribute that holds an identifier; an _\<xccdf:Profile\>_may refer to this tag through the _\<xccdf:Profile\>@note-tag_ attribute. The element also MAY have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9) and/or an _@xml:lang_ attribute (see Section 6.2.10). |
| fixtext (element) | _special_ | 0-n | Data that describes how to bring a target system into compliance with this rule. Each _\<xccdf:fixtext\>_ element may be associated with one or more _\<xccdf:fix\>_ element values. See Section 6.4.4.5. |
| fix (element) | _special_ | 0-n | A command string, script, or other system modification statement that, if executed on the target system, can bring it into full, or at least better, compliance with this rule. See Section 6.4.4.5. |
| check (element) | _special_ | (1-n instances of check) XOR (1 instance of complex-check) | The definition of, or a reference to, the target system check needed to test compliance with this rule. Sibling _\<xccdf:check\>_ elements must have different values for the combination of their _@selector_ and _@system_ attributes, and different values for their _@id_ attribute (if any). See Section 6.4.4.4. |
| complex-check (element) | _special_ | (1-n instances of check) XOR (1 instance of complex-check) | A boolean expression composed of operators (and, or, not) and individual checks. See Section 6.4.4.4. |
| signature (element) | _special_ | 0-1 | A digital signature asserting authorship and allowing verification of the integrity of the rule. See Section 6.2.7. |
| role (attribute) | string | 0-1 | The rule's role in scoring and reporting. It May be one of the following:<ul><li>"full" (if the rule is selected, then check it and let the result contribute to the score and appear in reports) (default)</li><li>"unscored" (if the rule is selected, then check it and include the results in any report, but do not include the result in score computations)</li><li>"unchecked" (if the rule is selected, then do not check it; just force the result status to "notchecked")</li></ul>May be overridden by a profile. |
| id (attribute) | _special_ | 1 | Unique element identifier; SHALL be used by other elements to refer to this element. See Section 6.2.3. |
| severity (attribute) | string | 0-1 | Severity level code to be used for metrics and tracking. When used, it shall have one of the following values:<ul><li>unknown" (severity not defined) (default)</li><li>"info" (rule is informational only; failing the rule does not imply failure to conform to the security guidance of the benchmark)</li><li>"low" (not a serious problem)</li><li>"medium" (fairly serious problem)</li><li>"high" (grave or critical problem)</li></ul>May be overridden by a profile; see Section 6.5.3. |
| multiple (attribute) | boolean | 0-1 | Applicable in cases where there are multiple instances of a target. For example, a rule may provide a recommendation about the configuration of application user accounts, but an application may have many user accounts. Each account would be considered an instance of the broader assessment target of user accounts. If the _@multiple_ attribute is set to true, each instance of the target to which the rule can apply should be tested separately and the results should be recorded separately. If _@multiple_ is set to false, the test results of such instances should be combined. If the checking system does not combine these results automatically, the results of each instance should be ANDed together to produce a single result using the AND truth table in Section 6.4.4.4. (default:false)If the benchmark consumer cannot perform multiple instantiation, or if multiple instantiation of the rule is not applicable for the target system, then the benchmark consumer may ignore this attribute. For example, OVAL checks cannot produce separate results for individual instances of a target. See Section 7.2.3.5.2. |

#### 6.4.4.3 \<xccdf:ident\> Elements

Table 10 lists assigned URIs that may appear as the value of the _@system_ attribute of an _\<xccdf:ident\>_ element. If an identification system included in Table 10 is being specified, the _@system_ attribute's value SHALL correspond to the appropriate URI in the table. An author may create a new URI for an identification system not listed in the table; this URI shall not duplicate any of the Table 10 URI values.

**Table 10: Assigned Values for the @system Attribute of an \<xccdf:ident\> Element**

| **URI** | **Identifier Value Description** |
| --- | --- |
| http://cce.mitre.org | Common Configuration Enumeration (CCE) – the identifier value MUST be a CCE version 5 number |
| http://cpe.mitre.org | CPE – the identifier value MUST be a CPE version 2.0 or 2.3 name |
| http://cve.mitre.org | CVE – the identifier value MUST be a CVE number |
| http://www.cert.org | CERT Coordination Center – the identifier value should be a CERT advisory identifier (e.g., "CA-2004-02") |
| http://www.kb.cert.org | US-CERT vulnerability notes database – the identifier value should be a vulnerability note number (e.g., "709220") |
| http://www.us-cert.gov/cas/techalerts | US-CERT technical cyber security alerts – the identifier value should be a technical cyber security alert ID (e.g., "TA05-189A") |

An _\<xccdf:ident\>_ element MAY also have additional attributes from schemas other than the XCCDF schema. Individual organizations and standards MAY associate specific interpretations of rules based on the value of an _\<xccdf:ident\>_ element and these additional attributes are allowed in order to refine those interpretations. These additional attributes MUST NOT alter the processing of a benchmark document as described in Section 7, although they MAY be added to the output of Document Generation or displayed to users during processing.

#### 6.4.4.4 \<xccdf:check\> and \<xccdf:complex-check\>Elements

Table 11 shows the possible properties of the _\<xccdf:check\>_ element.

**Table 11: \<xccdf:check\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| check-import (element) | _special_ | 0-n | Identifies a value to be retrieved from the checking system during testing of a target system. This element must be empty. After the associated check results have been collected, the result structure returned by the checking engine is processed to collect the named information. This information is then recorded in the _\<xccdf:check-import\>_ element in the corresponding _\<xccdf:rule-result\>_. This could be a single value or structured XML. In the latter case, the _@import-xpath_ attribute can be used to traverse the structured XML and identify specific information of interest to record in the result. |
| check-export (element) | _special_ | 0-n | A mapping from an _\<xccdf:Value\>_ element to a checking system variable (i.e., external name or id for use by the checking system). This supports export of tailoring values from the XCCDF processing environment to the checking system. The _@value-id_ attribute of the _\<xccdf:check-export\>_ element must match the _@id_ attribute of an _\<xccdf:Value\>_ element in the benchmark. The interface between the XCCDF benchmark consumer and the checking system SHOULD support, at a minimum, passing the following properties of the _\<xccdf:Value\>_ element: _\<xccdf:value\>_, _@type_, and _@operator_. |
| check-content-ref (element) | _special_ | 0-n | Points to code for a detached check in another location that uses the language or system specified by the _\<xccdf:check\>_ element's _@system_ attribute. The _@href_ attribute identifies the code location, and the optional _@name_ attribute MUST refer to a particular part, element, or component of the code. The default behavior of an _\<xccdf:check-content-ref\>_ element that does not have a _@name_ attribute shall be to execute all checks in the referenced code and AND their results together into a single _\<xccdf:rule-result\>._ However, if the _@multi-check_ attribute (below) is set to true and a nameless _\<xccdf:check-content-ref\>_ is used, each check in the targeted code is reported as a separate _\<xccdf:rule-result\>_. If multiple _\<xccdf:check-content-ref\>_ elements appear, they represent alternative locations from which a benchmark consumer may obtain the check content. Benchmark consumers should process the alternatives in the order in which they appear in the XML. The first _\<xccdf:check-content-ref\>_ from which content can be successfully retrieved should be used. (Note that ensuring the validity of this content is not the responsibility of a benchmark consumer.) If both _\<xccdf:check-content-ref\>_ and _\<xccdf:check-content\>_ elements appear in a single _\<xccdf:check\>_ element, benchmark consumers should use the _\<xccdf:check-content\>_ element only if none of the references can be resolved to provide content. |
| check-content (element) | _special_ | 0-1 | Holds the actual code of a check, in the language or system specified by the _\<xccdf:check\>_ element's _@system_ attribute. The body of this element may be any XML, but shall not contain any XCCDF elements. It is optional for benchmark consumers to process this element; typically it will be passed to a checking system or engine. If both _\<xccdf:check-content-ref\>_ and _\<xccdf:check-content\>_ elements appear in a single _\<xccdf:check\>_ element, benchmark consumers should use the _\<xccdf:check-content\>_ element only if none of the references can be resolved to provide content. |
| system (attribute) | URI | 1 | The URI for a checking system. The URI shall be compliant with the appropriate checking system specification (e.g., OVAL, OCIL). If the checking system uses XML namespaces, then the _@system_ attribute for the system should be its namespace. |
| negate (attribute) | boolean | 0-1 | If set to true, the final result of the check is negated according to the truth table in Table 14. (default: false) |
| id (attribute) | identifier | 0-1 | Unique identifier for this element. |
| selector (attribute) | string | 0-1 | This may be referenced from a profile or used during manual tailoring to refine the application of the rule. If no selectors are specified for a given _\<xccdf:rule\>_, all _\<xccdf:check\>_ elements with non-empty _@selector_ attributes shall be ignored. If a rule has multiple _\<xccdf:check\>_ elements with the same _@selector_ attribute, each must employ a different checking system, as identified by the _@system_ attribute of the _\<xccdf:check\>_ element. |
| multi-check (attribute) | boolean | 0-1 | Applicable in cases where multiple checks are executed to determine compliance with a single rule. This situation can arise when a check includes an _\<xccdf:check-content-ref\>_ element that does not include a _@name_ attribute. The default behavior of a nameless _\<xccdf:check-content-ref\>_ shall be to execute all checks in the referenced check content location and AND their results together into a single _\<xccdf:rule-result\>_ using the AND truth table below (Table 12). This corresponds to a _@multi-check_ attribute value of "false". If, however, the _@multi-check_ attribute is set to "true" and a nameless _\<xccdf:check-content-ref\>_ is used, the rule produces a separate _\<xccdf:rule-result\>_ for each check in the check content location. (default: false) See Section 7.2.3.5.2. |
| xml:base (attribute) | _special_ | 0-1 | The context for all relative URIs within the check. |

In place of an _\<xccdf:check\>_ element, XCCDF allows an _\<xccdf:complex-check\>_ element. An _\<xccdf:complex-check\>_ is a boolean logical expression whose individual terms are _\<xccdf:check\>_ and/or _\<xccdf:complex-check\>_ elements. This allows benchmark authors to create more sophisticated checks and to mix checks written with different checking systems. An _\<xccdf:Rule\>_ may have at most one _\<xccdf:complex-check\>_ element; on inheritance, the extending rule's _\<xccdf:complex-check\>_ shall replace the extended rule's _\<xccdf:complex-check\>_. A benchmark consumer processing a benchmark picks at most one _\<xccdf:check\>_ or _\<xccdf:complex-check\>_ element to process for each rule.If both _\<xccdf:check\>_ elements and an _\<xccdf:complex-check\>_ element appear in a rule, then the _\<xccdf:check\>_ elements will be ignored. See Section 7.2.3.5 for additional information on check processing.

When an _\<xccdf:check\>_ element is used with an _\<xccdf:complex-check\>_ element, the _\<xccdf:check\>_ element's _@multi-check_ attribute MUST be ignored.

Operators that may be used to combine the constituents of an _\<xccdf:complex-check\>_ are AND and OR. Truth tables that must be used when evaluating these operators appear in Table 12 (AND) and Table 13 (OR). Each _\<xccdf:complex-check\>_ may also specify that the expression should be negated (NOT); see Table 14 for the corresponding truth table. All of the abbreviations in the truth tables come from the description of the _\<xccdf:rule-result\>_ element (see Table 26 in Section 6.6.4.2 for definitions of each abbreviation).

With an "AND" operator, the _\<xccdf:complex-check\>_ evaluates to Pass only if all of its enclosed terms (_\<xccdf:check\>_ and _\<xccdf:complex-check\>_ elements) evaluate to Pass. Table 12 shows the truth table for "AND".

**Table 12: Truth Table for AND**

| _ **AND** _ | P | F | U | E | N | K | S | I |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| P | P | F | U | E | P | P | P | P |
| F | F | F | F | F | F | F | F | F |
| U | U | F | U | U | U | U | U | U |
| E | E | F | U | E | E | E | E | E |
| N | P | F | U | E | N | N | N | N |
| K | P | F | U | E | N | K | K | K |
| S | P | F | U | E | N | K | S | S |
| I | P | F | U | E | N | K | S | I |

The "OR" operator evaluates to Pass if any of its enclosed terms evaluate to Pass. The truth table for "OR" is shown in Table 13.

**Table 13: Truth Table for OR**

| _ **OR** _ | P | F | U | E | N | K | S | I |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| P | P | P | P | P | P | P | P | P |
| F | P | F | U | E | F | F | F | F |
| U | P | U | U | U | U | U | U | U |
| E | P | E | U | E | E | E | E | E |
| N | P | F | U | E | N | N | N | N |
| K | P | F | U | E | N | K | K | K |
| S | P | F | U | E | N | K | S | S |
| I | P | F | U | E | N | K | S | I |

If the _@negate_ attribute is set to true, then the result of the _\<xccdf:complex-check\>_ is complemented (inverted). The full truth table for negation is listed in Table 14.

**Table 14: Truth Table for Negation**

|
 | P | F | U | E | N | K | S | I |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| _ **not** _ | F | P | U | E | N | K | S | I |

The example below shows an _\<xccdf:complex-check\>_ with several components.

```xml
<xccdf:complex-check operator="OR">
  <xccdf:check system="http://oval.mitre.org/XMLSchema/oval-definitions-5">
    <xccdf:check-content-ref href="xpDefs.xml" name="oval:org.example:def:456"/>
  </xccdf:check>
  <xccdf:complex-check operator="AND" negate="1">
    <xccdf:check system="http://oval.mitre.org/XMLSchema/oval-definitions-5">
      <xccdf:check-content-ref href="xpDefs.xml" name="oval:org.example:def:789"/>
    </xccdf:check>
    <xccdf:check system="http://scap.nist.gov/schema/ocil/2.0">
      <xccdf:check-content-ref href="xpInter.xml"
          name="ocil:org.example:questionnaire:6"/>
    </xccdf:check>
  </xccdf:complex-check>
</xccdf:complex-check>
```

#### 6.4.4.5 \<xccdf:fixtext\> and \<xccdf:fix\> Elements

The _\<xccdf:fixtext\>_ and _\<xccdf:fix\>_ elements help tools support sophisticated facilities for automated and interactive remediation of benchmark findings. Table 15 lists the possible properties of an _\<xccdf:fixtext\>_ element.

**Table 15: Possible Properties for \<xccdf:fixtext\> Element**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| sub (element) | identifier | 0-n | Specifies an _\<xccdf:Value\>_ or _\<xccdf:plain-text\>_ substitution. See Section 6.2.9. |
| xml:lang (attribute) | _special_ | 0-1 | The language for the element; see Section 6.2.10. |
| override (attribute) | boolean | 0-1 | Specifies inheritance behavior (see Section 6.3.1). (default: false) |
| fixref (attribute) | identifier | 0-1 | A reference to a specific _\<xccdf:fix\>@id_ attribute. This allows pairing explanatory text with specific fix procedures. |
| reboot (attribute) | boolean | 0-1 | Whether or not remediation will require a reboot or hard reset of the target. When specified, it SHALL have one of two values: true (1) or false (0) (default: 0). |
| strategy (attribute) | string | 0-1 | The method or approach for fixing the problem. When specified, the attribute shall have one of the following values:<ul><li>unknown (strategy not defined) (default )</li><li>configure (adjust target configuration/settings)</li><li>combination (combination of two or more approaches)</li><li>disable (turn off or uninstall a target component)</li><li>enable (turn on or install a target component)</li><li>patch (apply a patch, hotfix, update, etc.)</li><li>policy (remediation requires out-of-band adjustments to policies or procedures)</li><li>restrict (adjust permissions, access rights, filters, or other access restrictions)</li><li>update (install upgrade or update the system)</li></ul> |
| disruption (attribute) | string | 0-1 | An estimate of the potential for disruption or operational degradation that the application of this fix will impose on the target. When specified, the attribute shall have one of the following values:<ul><li>unknown (disruption not defined) (default)</li><li>low (little or no disruption expected)</li><li>medium (potential for minor or short-lived disruption)</li><li>high (potential for serious disruption)</li></ul> |
| complexity (attribute) | string | 0-1 | The estimated complexity or difficulty of applying the fix to the target. When specified, the attribute shall have one of the following values:<ul><li>unknown (complexity not defined) (default)</li><li>low (fix is very simple to apply)</li><li>medium (fix is moderately difficult or complex)</li><li>high (fix is very complex to apply)</li></ul> |

Table 16 lists the possible properties of an _\<xccdf:fix\>_ element.

**Table 16: Possible Properties for \<xccdf:fix\> Element**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| sub (element) | identifier | 0-n | Specifies an _\<xccdf:Value\>_ or _\<xccdf:plain-text\>_ substitution. See Section 6.2.9. |
| instance (element) | string | 0-n | Designates a spot where the name of the instance should be substituted into the fix template to generate the final fix data. If the _@context_ attribute is omitted, the value of the context shall default to "undefined". |
| id (attribute) | identifier | 0-1 | A local identifier for the element. It is optional for the id to be unique; multiple _\<xccdf:fix\>_ elements may have the same id but different values for their other attributes. |
| reboot (attribute) | boolean | 0-1 | Whether or not remediation will require a reboot or hard reset of the target. Permitted values: true (1) and false (0) (default: 0). |
| strategy (attribute) | string | 0-1 | The method or approach for fixing the problem. See Table 15 for the list of possible values. |
| disruption (attribute) | string | 0-1 | An estimate of the potential for disruption or operational degradation that the application of this fix will impose on the target.See Table 15 for the list of possible values. |
| complexity (attribute) | string | 0-1 | The estimated complexity or difficulty of applying the fix to the target.See Table 15 for the list of possible values. |
| system (attribute) | URI | 0-1 | A URI that identifies the scheme, language, engine, or process for which the fix contents are written. Table 17 defines several general-purpose URNs that may be used for this, and tool vendors and system providers may define and use target-specific URNs. |
| platform (attribute) | URI | 0-1 | In case different fix scripts or procedures are required for different target platform types (e.g., different patches for Windows Vista and Windows 7), this attribute allows a CPE name or CPE applicability language expression to be associated with an _\<xccdf:fix\>_ element. This should appear on an _\<xccdf:fix\>_ when the content applies to only one platform out of several to which the rule could apply. |

Table 17 lists predefined values for the _@system_ attribute of an _\<xccdf:fix\>_ element.

**Table 17: Predefined Values for @system Attribute of \<xccdf:fix\> Element**

| **URI** | **Content of the \<xccdf:fix\> Element** |
| --- | --- |
| urn:xccdf:fix:commands | A list of target system commands; executed in order, the commands should bring the target system into compliance with the rule. |
| urn:xccdf:fix:urls | A list of one or more URLs. The resources identified by the URLs should be applied to bring the system into compliance with the rule. |
| urn:xccdf:fix:script:_language_ | A script written in the given _language_. Executing the script should bring the target system into compliance with the rule. The following languages are pre-defined:<ul><li>sh – Bourne shell</li><li>csh – C Shell</li><li>perl – Perl</li><li>batch – Windows batch script</li><li>python – Python and all Python-based scripting languages</li><li>vbscript – Visual Basic Script (VBS)</li><li>javascript – Javascript (ECMAScript, Jscript)</li><li>tcl – Tcl and all Tcl-based scripting languages</li></ul> |
| urn:xccdf:fix:patch:_vendor_ | A patch identifier, in proprietary format as defined by the vendor. The vendor string should be the DNS domain name of the vendor. For example, for Microsoft Corporation, the DNS domain is "Microsoft.com". |

### 6.4.5 \<xccdf:Value\> Element

#### 6.4.5.1 Basics

An _\<xccdf:Value\>_ is a named parameter (with a unique _@id_ attribute) that can be substituted into properties of other elements within the benchmark, including the interior of structured check specifications and fix scripts. An _\<xccdf:Value\>_ can hold string, boolean, or numeric content, or lists thereof._\<xccdf:Value\>_ elements can include information about permissible values, display defaults, and restrictions on tailoring for the Value. For example, an _\<xccdf:Value\>_ might be used to hold a benchmark's lower limit for password length on some OS. In a profile for that OS to be used in a closed lab, the default value might be 8, but in a profile for that OS to be used on the Internet, the default value might be 12.

The example below shows an _\<xccdf:Value\>_ element.

```xml
<xccdf:Value id="xccdf_org.example_value_web-server-port" type="number" operator="equals">
  <xccdf:title>Web Server Port</xccdf:title>
  <xccdf:description>
    TCP port on which the server listens
  </xccdf:description>
  <xccdf:value>12080</xccdf:value>
  <xccdf:default>80</xccdf:default>
  <xccdf:lower-bound>0</xccdf:lower-bound>
  <xccdf:upper-bound>65535</xccdf:upper-bound>
</xccdf:Value>
```

_\<xccdf:Value\>_ elements may encapsulate values that are lists (possibly zero-length) of simple types. These structures are supported by the _\<xccdf:complex-value\>_ element, the _\<xccdf:complex-default\>_ element, and within the _\<xccdf:choices\>_ element, the _\<xccdf:complex-choice\>_ element. If any of these elements has no child elements, it represents an empty list.

A single _\<xccdf:Value\>_ may use a mixture of both simple (i.e., a single number, string, or boolean value) and complex types. Apart from its ability to encapsulate different types of information, a complex field is used in the same way as its simple counterpart.

#### 6.4.5.2 Properties

Table 18 describes the _\<xccdf:Value\>_ element's properties (in addition to the core item properties previously described in Table 5).

**Table 18: \<xccdf:Value\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| value (element) | string | 1-n | The current value of this _\<xccdf:Value\>_. This element may have a _@selector_ attribute (see Section 6.4.5.5). |
| complex-value (element) | _special_ | 1-n | Similar to _\<xccdf:value\>_ except that this element supports values that are lists of simple types. This element MAY have a _@selector_attribute (see Section 6.4.5.5). |
| default (element) | string | 0-n | The default value displayed to the user as a suggestion by benchmark producers during tailoring of this _\<xccdf:Value\>_ element. (This is not the default value of an _\<xccdf:Value\>_.) This element may have a _@selector_ attribute (see Section 6.4.5.5). |
| complex-default (element) | _special_ | 0-n | Similar to _\<xccdf:default\>_ except that this element supports default values that are lists of simple types. This element MAY have a _@selector_attribute (see Section 6.4.5.5). |
| match (element) | string | 0-n | A Perl Compatible Regular Expression (PCRE) (see [PCRE] and [UNICODE]) that a benchmark producer may apply during tailoring to validate a user's input for the Value. It shall use implicit anchoring. It shall apply only when the _@type_ is "string" or "number" or a list of strings and/or numbers. For example, if the _@type_ was "string", but the value was meant to be a Cisco IOS router interface name, then the _\<xccdf:match\>_ element might be set to "[A-Za-z]+ \*[0-9]+(/[0-9.]+)\*". This would allow a tailoring tool to reject an invalid user input like "f8xq+" but accept a legal one like "Ethernet1/3". This element may have a _@selector_ attribute (see Section 6.4.5.5). |
| lower-bound (element) | decimal | 0-n | Minimum legal value for this Value. It is used to constrain value input during tailoring, when the _@type_ is "number". Values supplied by the user for tailoring the benchmark must be equal to or greater than this number. This element may have a _@selector_ attribute (see Section 6.4.5.5). |
| upper-bound (element) | decimal | 0-n | Maximum legal value for this Value. It is used to constrain value input during tailoring, when the _@type_ is "number". Values supplied by the user for tailoring the benchmark must be less than or equal to this number. This element may have a _@selector_ attribute (see Section 6.4.5.5). |
| choices (element) | _special_ | 0-n | A list of legal or suggested choices (values) for an _\<xccdf:Value\>_ element, to be used during tailoring and document generation. See Section 6.4.5.3. |
| source (element) | URI | 0-n | URI indicating where the tool may acquire values, value bounds, or value choices for this _\<xccdf:Value\>_ element. XCCDF does not attach any meaning to the URI; it may be an arbitrary community or tool-specific value, or a pointer directly to a resource. If several values for the _\<xccdf:source\>_ element appear, then they shall represent alternative means or locations for obtaining the value in descending order of preference (i.e., most preferred first). |
| signature (element) | _special_ | 0-1 | A digital signature asserting authorship and allowing verification of the integrity of the Value. See Section 6.2.7. |
| id (attribute) | _special_ | 1 | Unique element identifier. See Section 6.2.3. |
| type (attribute) | string | 0-1 | The data type of the Value: "string", "number", or "boolean" (default: "string"). A tool may choose any convenient form to store a Value's _\<xccdf:value\>_ element, but the _@type_ attribute conveys how the Value should be treated for user input validation purposes during tailoring processing. The _@type_ attribute may also be used to give additional guidance to the user or to validate the user's input. For example, if an _\<xccdf:value\>_ element's _@type_ attribute is "number", then a tool might choose to reject user tailoring input that is not composed of digits. In the case of a list of values, the _@type_ attribute, if present, shall be applied to all elements of the list individually. |
| operator (attribute) | string | 0-1 | The operator to be used for comparing this Value to some part of the test system's configuration during rule checking (default: "equals"). See Section 6.4.5.4. |
| interactive (attribute) | boolean | 0-1 | Whether tailoring for this Value should also be performed during benchmark application (default: false). The benchmark consumer may ignore the attribute if asking the user is not feasible or not supported. |
| interfaceHint (attribute) | string | 0-1 | A hint or recommendation to a benchmark consumer or producer about how the user might select or adjust the Value. If used, this element shall have one of the following interface pattern values:<ul><li>"choice" (multiple choice)</li><li>"textline" (multiple lines of text)</li><li>"text" (single line of text)</li><li>"date" (date)</li><li>"datetime" (date and time)</li></ul> |

#### 6.4.5.3 \<xccdf:choices\> Element

The _\<xccdf:choices\>_ element SHOULD be used when there are a moderate number of known values that are most appropriate. For example, if the Value were the authentication mode for a server, the choices might be "password" and "pki". Table 19 lists the possible properties for an _\<xccdf:choices\>_ element. If a product presents the choice values from an _\<xccdf:choices\>_ element to a user, they SHOULD be presented in the order in which they appear.

**Table 19: Possible Properties for \<xccdf:choices\> Element**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| choice (element) | string | 1-n | The text of a possible choice. |
| complex-choice (element) | _special_ | 1-n | A possible choice consisting of a list of values. If this element has no _\<xccdf:item\>_ children, it SHALL represent an empty list. |
| mustMatch (attribute) | boolean | 0-1 | If this is true, the list shall represent all the legal values. If this is false or absent, the list shall represent suggested values, and other values may also be legal (subject to the parent _\<xccdf:Value\>_ element's _@upper-bound_, _@lower-bound_, and _@match_ attributes). |
| selector (attribute) | string | 0-1 | Used for tailoring via a profile. See Section 6.4.5.5. |

#### 6.4.5.4 @operator Attribute

When defining an _\<xccdf:Value\>_ element, the benchmark author may specify the operator to be used for comparing the Value to a configuration during rule checking. For example, one part of an OS benchmark might be verifying that the configuration included a minimum password length; the _\<xccdf:Value\>_ element that holds the tailorable minimum could have _@type_ "number" and _@operator_"greater than". Exactly how _\<xccdf:Value\>_ elements are used in rules depends on the capabilities of the checking system. Benchmark consumers may ignore the _@operator_ attribute; therefore, authors should include sufficient information in the _\<xccdf:description\>_ and _\<xccdf:question\>_ elements to make the role of the Value clear. Table 20 describes the _@operator_ values permitted for each _@type_ value.

**Table 20: Permitted Operators by Value Type**

| Value Type | Available Operators |
| --- | --- |
| number | equals, not equal, less than, greater than, less than or equal, greater than or equal (default: equals) |
| boolean | equals, not equal (default: equals) |
| string | equals, not equal, pattern match (pattern match means regular expression match; should comply with [UNICODE]) (default: equals) |
| lists | as component data type |

#### 6.4.5.5 @selector Attribute

As detailed in Table 18, an _\<xccdf:Value\>_ may include various elements that constrain or limit the values that the Value may be given. Authors may use these elements to assist users in tailoring the benchmark. These elements all support the _@selector_ attribute. If there are multiple instances of one of these elements within a single _\<xccdf:Value\>_ element, no more than one of the instances may omit the _@selector_ attribute, and every other instance must have a different value specified for its _@selector_ attribute. For elements that may be substituted for each other—specifically, _\<xccdf:value\>_ and _\<xccdf:complex-value\>_ elements, and _\<xccdf:default\>_ and _\<xccdf:complex-default\>_ elements—no more than one instance of either element may omit the _@selector_ attribute, and every other instance of both elements must have a different value specified for its _@selector_ attribute. For more information about selector types, see Section 6.5.3.

In the absence of any profile or tailoring actions, the default _\<xccdf:value\>_ or _\<xccdf:complex-value\>_ element in an _\<xccdf:Value\>_ shall be the one with an empty or absent _@selector_. If there is no _\<xccdf:value\>_ or _\<xccdf:complex-value\>_ element with an empty or absent _@selector_, the first _\<xccdf:value\>_ or _\<xccdf:complex-value\>_ element in top-down processing of the XML shall be the default element. For all other selectable _\<xccdf:Value\>_ elements (i.e.,_\<xccdf:default\>,_ _\<xccdf:complex-default\>,_ _\<xccdf:match\>_, _\<xccdf:upper-bound\>,_ _\<xccdf:lower-bound\>_, and _\<xccdf:choices\>_), the default activity shall be to ignore all elements without an empty or absent _@selector_.

## 6.5 \<xccdf:Profile\> Element

### 6.5.1 Basics

An _\<xccdf:Profile\>_ element is a named tailoring for a benchmark. While a benchmark can be tailored in place by setting properties of various elements, _\<xccdf:Profile\>_ elements allow one benchmark document to hold several independent tailorings. The _\<xccdf:select\>_ element children of the _\<xccdf:Profile\>_ affect which _\<xccdf:Group\>_ and _\<xccdf:Rule\>_ elements are selected for processing when the _\<xccdf:Profile\>_ is in effect. The _\<xccdf:refine-rule\>_ element allows modification of properties in _\<xccdf:Group\>_ and _\<xccdf:Rule\>_ elements, while the _\<xccdf:refine-value\>_ element allows modification of _\<xccdf:Value\>_ properties, including selection of the effective value. Finally, _\<xccdf:set-value\>_ and _\<xccdf:set-complex-value\>_ allow an _\<xccdf:Value\>_ element's value to be set directly to a simple or complex setting, respectively. The example below shows a simple _\<xccdf:Profile\>_.

```xml
<xccdf:Profile id="xccdf_org.example_profile_strict" prohibitChanges="1" extends="xccdf_org.example_profile_lenient" note-tag="strict-tag">
  <xccdf:title>Strict Security Settings</xccdf:title>
  <xccdf:description>
    Strict lockdown rules and values, for hosts deployed to high-risk environments.
  </xccdf:description>
  <xccdf:set-value idref="xccdf_org.example_value_password-len">10</xccdf:set-value>
  <xccdf:refine-value idref="xccdf_org.example_value_session-timeout" selector="quick"/>
  <xccdf:refine-rule idref="xccdf_org.example_value_session-auth-rule" selector="harsh"/>
  <xccdf:select idref="xccdf_org.example_value_password-len-rule" selected="1"/>
  <xccdf:select idref="xccdf_org.example_value_audit-cluster" selected="1"/>
  <xccdf:select idref="xccdf_org.example_value_telnet-disabled-rule" selected="1"/>
  <xccdf:select idref="xccdf_org.example_value_telnet-settings-cluster" selected="0"/>
</xccdf:Profile>
```

An _\<xccdf:Profile\>_ MAY extend another _\<xccdf:Profile\>_ in the same benchmark by using the _@extends_ attribute. An _\<xccdf:Profile\>_ in an _\<xccdf:Tailoring\>_ element MAY extend any _\<xccdf:Profile\>_ in a benchmark, but _\<xccdf:Profile\>_ elements in an _\<xccdf:Tailoring\>_ element SHALL NOT be extended.

Certain properties of the extended _\<xccdf:Profile\>_ appear before the corresponding properties in the extending _\<xccdf:Profile\>_. Since _\<xccdf:Profile\>_ properties are processed in the order in which they appear in the XML, this means that properties of the extending _\<xccdf:Profile\>_ can override corresponding properties inherited from the extended _\<xccdf:Profile\>_. See Sections 7.2.2 and 7.2.3.4 for additional information on extension and inheritance.

### 6.5.2 Properties

Table 21 describes the _\<xccdf:Profile\>_ element's properties.

**Table 21: \<xccdf:Profile\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| status (element) | _special_ | 0-n | Status of this _\<xccdf:Profile\>_ and date at which it attained that status. Authors may use this element to record the maturity or consensus level of a profile. If the status is not given explicitly, then the _\<xccdf:Profile\>_ is taken to have the same status as its parent _\<xccdf:Benchmark\>_. See Section 6.2.8. |
| dc-status (element) | _special_ | 0-n | Holds additional status information using the Dublin Core format. See Section 6.2.8. |
| version (element) | string | 0-1 | Version number of this _\<xccdf:Profile\>_, with an optional _@time_ timestamp attribute (when the version was completed) and an optional_@update_ URI attribute (where updates may be obtained). |
| title (element) | string | 1-n | Title of this _\<xccdf:Profile\>_. It may have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9), an _@xml:lang_ attribute (see Section 6.2.10), and/or an _@override_ attribute (see Section 6.3.1). |
| description (element) | HTML-enabled text | 0-n | Text that describes this _\<xccdf:Profile\>_. It MAY have one or more _\<xccdf:sub\>_ elements (see Section 6.2.9), an _@override_ attribute (see Section 6.3.1), and/or an _@xml:lang_ attribute (see Section 6.2.10). |
| reference (element) | _special_ | 0-n | A reference where the user can learn more about the subject of this profile. See Section 6.2.6. |
| platform (element) | string | 0-n | A target platform for this profile. See Section 6.2.5. |
| select, set-complex-value, set-value, refine-value, refine-rule (element) | _special_ | 0-n | References to Groups, Rules, and Values for customization and tailoring. See Section 6.5.3. |
| metadata (element) | _special_ | 0-n | Metadata associated with this _\<xccdf:Profile\>_. See Section 6.2.4. |
| signature (element) | _special_ | 0-1 | A digital signature asserting authorship and allowing verification of the integrity of the _\<xccdf:Profile\>_. See Section 6.2.7. |
| id (attribute) | _special_ | 1 | Unique identifier for this _\<xccdf:Profile\>_. See Section 6.2.3. |
| prohibitChanges (attribute) | boolean | 0-1 | Whether or not products should prohibit changes to this _\<xccdf:Profile\>_(default: false). |
| abstract (attribute) | boolean | 0-1 | If true, then this _\<xccdf:Profile\>_ exists solely to be extended by other _\<xccdf:Profile\>_ elements (default: false). See Sections 6.3.1 and 7.2.2. |
| note-tag (attribute) | identifier | 0-1 | Tag identifier to specify which _\<xccdf:profile-note\>_ element from an _\<xccdf:Rule\>_ should be associated with this _\<xccdf:Profile\>._ |
| extends (attribute) | identifier | 0-1 | The id of an _\<xccdf:Profile\>_ on which to base this _\<xccdf:Profile\>._ |
| xml:base (attribute) | _special_ | 0-1 | The context for all relative URIs within the profile. |
| Id (attribute) | _special_ | 0-1 | An identifier used for referencing elements included in an XML signature. See Section 6.2.7. |

### 6.5.3 Selectors

Each _\<xccdf:Profile\>_ can contain one or more selectors to express a particular customization or tailoring of the _\<xccdf:Benchmark\>._ Table 22 defines the five kinds of selectors.

**Table 22: Selectors**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| select (element) | _special_ | 0-n | Designates a Rule, Group, or cluster of Rules and Groups. It overrides the _@selected_ attribute on the designated items, providing a means for including or excluding rules from an assessment.The _\<xccdf:select\>_ element may have one or more _\<xccdf:remark\>_ elements (strings) containing explanatory material or other prose. Each instance of _\<xccdf:remark\>_ MAY have an _@xml:lang_ attribute (see Section 6.2.10).The _\<xccdf:select\>_ element's _@idref_ attribute (identifier) shall specify the designated Group, Rule, or cluster of Rules and Groups; it must match the _@id_ attribute of a Group or Rule in the Benchmark, or the cluster id assigned to one or more Rules or Groups. If the _\<xccdf:select\>_ element's required _@selected_ attribute (boolean) is set to true, the Rule, Group, or Rules and Groups in the cluster shall have their _@selected_ attribute set to true, otherwise they shall have their _@selected_ attribute set to false. Subsequent tailoring actions may further modify these properties. |
| set-value (element) | string | 0-n | Points to an _\<xccdf:Value\>_ element and overrides its _\<xccdf:value\>_ and _\<xccdf:complex-value\>_ element(s) without changing any other properties. It provides a means for directly specifying the value of a variable to be used in benchmark processing. This selector may also be applied to the _\<xccdf:Value\>_ elements in a cluster by specifying the cluster id in the _@idref_ attribute, in which case it shall override the _\<xccdf:value\>_ elements of all of them. |
| set-complex-value (element) | _special_ | 0-n | Supports the direct specification of complex value types such as lists. Zero or more item elements MAY appear as children of this element; if no child elements are present, this element SHALL represent an empty list. This overrides the _\<xccdf:value\>_ and _\<xccdf:complex-value\>_ element(s) of an _\<xccdf:Value\>_ element. Like _\<xccdf:set-value\>_, _\<xccdf:set-complex-value\>_ may also be applied to _\<xccdf:Value\>_ elements in a cluster. |
| refine-value (element) | _special_ | 0-n | Designates the Value constraints to be applied during tailoring, for an _\<xccdf:Value\>_ element or the _\<xccdf:Value\>_ members of a cluster. The element may have one or more _\<xccdf:remark\>_ elements containing explanatory material or other prose. Each instance of _\<xccdf:remark\>_ MAY have an _@xml:lang_ attribute (see Section 6.2.10). Possible attributes for the _\<xccdf:refine-value\>_ element are the id (required) of a Value or item cluster, the id of a Value selector, and a new setting for the Value operator. The element provides a means for authors to impose different constraints on tailoring for different profiles. (Constraints must be designated with a selector id. For example, a particular numeric Value might have several different sets of _\<xccdf:value\>_, _\<xccdf:upper-bound\>_, and _\<xccdf:lower-bound\>_ elements, designated with different selector ids. The _\<xccdf:refine-value\>_ selector tells benchmark consumers which value to employ and bounds to enforce when that particular profile is in effect. |
| refine-rule (element) | _special_ | 0-n | Allows the author to select check statements and override the _@weight_, _@severity_, and _@role_ of a Rule, Group, or cluster of Rules and Groups. The element may have one or more _\<xccdf:remark\>_ elements containing explanatory material or other prose. Each instance of _\<xccdf:remark\>_ MAY have an _@xml:lang_ attribute (see Section 6.2.10). The _\<xccdf:refine-rule\>_ element has an _@idref_ attribute (NCName) that shall refer to a Rule, Group, or cluster. Despite the name, this selector does apply for Groups and for clusters that include Groups, but it shall only affect their _@weight_ attribute. If the selector specified in an_\<xccdf:refine-rule\>_ element does not match any of the selectors specified on any of the check children of a Rule, then the _\<xccdf:check\>_ child element without a _@selector_ attribute must be used. |

The example below illustrates how selectors work with the _\<xccdf:refine-value\>_ element. See Section 6.4.5.5 for more information on the _@selector_ attribute. In the example, before the _\<xccdf:refine-value\>_ is applied, the effective value is 8 and the effective lower bound is 8. (These are the two properties that have no selectors and are therefore the default.) After the _\<xccdf:refine-value\>_ is applied, the effective value is 14 and the effective lower bound is 12.

```xml
<xccdf:Value id="xccdf_org.example_value_pw-length" type="number" operator="equals">
  <xccdf:title>Minimum password length policy</xccdf:title>
  <xccdf:value>8</xccdf:value>
  <xccdf:value selector="high">14</xccdf:value>
  <xccdf:lower-bound>8</xccdf:lower-bound>
  <xccdf:lower-bound selector="high">12</xccdf:lower-bound>
</xccdf:Value>
  <xccdf:Profile id="xccdf_org.example_profile_enterprise-internet">
  <xccdf:title>Enterprise internet server profile</xccdf:title>
  <xccdf:refine-value idref="xccdf_org.example_value_pw-length" selector="high"/>
</xccdf:Profile>
```

## 6.6 \<xccdf:TestResult\> Element

### 6.6.1 Basics

An _\<xccdf:TestResult\>_ element encapsulates the results of a single application of a benchmark to a single target platform. The _\<xccdf:TestResult\>_ element normally appears as the child of the _\<xccdf:Benchmark\>_ element, although it may also appear as the top-level element of an XCCDF results document.XCCDF is not intended to be a database format for detailed results; the _\<xccdf:TestResult\>_ element offers a way to store the results of individual tests in modest detail, with the ability to reference lower-level testing data.

Benchmark consumers should include mechanisms so that _\<xccdf:TestResult\>_ elements can retain their context even if separated from their source benchmark since the _\<xccdf:TestResult\>_ only has meaning relative to the Rules that produced it. There are several ways to preserve this context, including making sure that the _\<xccdf:benchmark\>_ element in the _\<xccdf:TestResult\>_ is a persistent link to the specific document and version of the benchmark that produced the result, filling in the relevant version, organization, and similar fields, and/or using descriptive metadata. This document does not mandate any specific approach, but benchmark consumers should ensure some mechanism is in place to avoid context loss.

The example below shows an _\<xccdf:TestResult\>_ element with a few _\<xccdf:rule-result\>_ children.

```xml
<xccdf:TestResult id="xccdf_org.example_testresult_ios-test5"
        end-time="2007-09-25T7:45:02-04:00"
        xmlns:xccdf="http://checklists.nist.gov/xccdf/1.2"
        version="1.0">
  <xccdf:benchmark href="ios-sample-12.4.xccdf.xml" id="xccdf_org.example_benchmark_ios-test-benchmark"/>
  <xccdf:title>Sample Results Block</xccdf:title>
  <xccdf:remark>Test run by Bob on Sept 25, 2007</xccdf:remark>
  <xccdf:organization>Department of Commerce</xccdf:organization>
  <xccdf:organization>National Institute of Standards and Technology</xccdf:organization>
  <xccdf:identity authenticated="1" privileged="1">admin_bob</xccdf:identity>
  <xccdf:target>lower.test.net</xccdf:target>
  <xccdf:target-address>192.168.248.1</xccdf:target-address>
  <xccdf:target-address>2001:8::1</xccdf:target-address>
  <xccdf:target-facts>
    <xccdf:fact type="string" name="urn:xccdf:fact:ethernet:MAC">02:50:e6:c0:14:39</xccdf:fact>
    <xccdf:fact type="string" name="urn:xccdf:fact:ethernet:MAC">02:50:e6:1f:33:b0</xccdf:fact>
  </xccdf:target-facts>
  <xccdf:set-value idref="xccdf_org.example_value_exec-timeout-time">10</xccdf:set-value>
    <xccdf:rule-result idref="xccdf_org.example_rule_ios12-no-finger-service" time="2007-09-25T13:45:00-04:00">
    <xccdf:result>pass</xccdf:result>
  </xccdf:rule-result>
  <xccdf:rule-result idref="xccdf_org.example_rule_req-exec-timeout" time="2007-09-25T13:45:06-04:00">
    <xccdf:result>fail</xccdf:result>
    <xccdf:instance>console</xccdf:instance>
    <xccdf:fix system="urn:xccdf:fix:commands" reboot="0" disruption="low">
    line consoleexec-timeout 10 0
    </xccdf:fix>
  </xccdf:rule-result>
  <xccdf:score>67.5</xccdf:score>
  <xccdf:score system="urn:xccdf:scoring:absolute">0</xccdf:score>
</xccdf:TestResult>
```

### 6.6.2 Properties

Table 23 describes the _\<xccdf:TestResult\>_ element's properties. Although several of its child elements technically support the _@override_ attribute (discussed in Section 6.3.1), the _\<xccdf:TestResult\>_ element cannot be extended. Therefore, _@override_ has no meaning within an _\<xccdf:TestResult\>_ element and its children, and it should not be used for them.

**Table 23: \<xccdf:TestResult\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| benchmark (element) | _special_ | 0-1 | Reference to the _\<xccdf:Benchmark\>_ for which the _\<xccdf:TestResult\>_ records results. Required if this _\<xccdf:TestResult\>_ element is the top-level element, optional otherwise. The required _@href_ attribute gives the URI of the benchmark document, and the optional _@id_ attribute holds the value of that _\<xccdf:Benchmark\>_ element's _@id_ attribute. |
| tailoring-file (element) | _special_ | 0-1 | Holds identifying information about an _\<xccdf:Tailoring\>_ element. See Section 6.6.5. |
| title (element) | string | 0-n | Title of the test. It may have an _@xml:lang_ attribute (see Section 6.2.10). |
| remark (element) | string | 0-n | A remark about the test, possibly supplied by the person administering the benchmark assessment. It may have an _@xml:lang_ attribute (see Section 6.2.10). |
| organization (element) | string | 0-n | The name of the organization or other entity responsible for applying this benchmark and generating this result. When multiple organization elements are used to indicate multiple organization names in a hierarchical organization, the highest-level organization should appear first (e.g., "U.S. Government") followed by subordinate organizations (e.g., "Department of Defense"). |
| identity (element) | string | 0-1 | Information about the system identity or user employed during application of the benchmark. If used, Shall specify the name of the authenticated identity. Has required boolean attributes that specify whether the identity was authenticated with the target system during the application of the benchmark _(@authenticated)_, and whether the identity was granted administrative or other special privileges beyond those of a normal user _(@privileged)_. |
| profile (element) | identifier | 0-1 | The identifier of the _\<xccdf:Profile\>_ used for the test. If the _\<xccdf:TestResult\>_ element contains an _\<xccdf:tailoring-file\>_ element, then this SHALL be the identifier of the associated tailoring profile. Otherwise, if there is no _\<xccdf:tailoring-file\>_ element present, this SHALL be the identifier of the associated benchmark profile if a profile was applied, and it SHALL be absent if no profile was applied. See Section 6.6.5. |
| target (element) | string | 1-n | Name or description of the target system whose test results are recorded in the _\<xccdf:TestResult\>_ element (the system to which a benchmark test was applied). Each appearance of the element supplies a name by which the target host or device was identified at the time the test was run. The name may be any string, but applications should include the fully qualified DNS name whenever possible. |
| target-address (element) | string | 0-n | Network address of the target system to which a benchmark test was applied. Typical forms for the address include IP version 4 (IPv4), IP version 6 (IPv6), and Ethernet media access control (MAC). |
| target-facts (element) | string | 0-1 | A list of named facts about the target system or platform. Each _\<xccdf:fact\>_ in the list shall specify the value of the fact itself. Each _\<xccdf:fact\>_ SHALL have a _@name_ attribute (URI) and may include a _@type_ attribute ("string", "number", or "boolean") (default: "boolean"). Pre-defined _@name_ attribute values are listed in Table 24; product developers may define additional platform-specific and product-specific facts. |
| target-id-ref (element) | _special_ | 0-n | Used to reference target identification information located in an external document. The assessment target MAY be identified using other identification schemas such as Asset Identification (see [IR7693] for additional information). Each instance of this element identifies the same target of this report (multiple identifiers for a single target). Inclusion of an _\<xccdf:target-id-ref\>_ element does not remove the requirement to identify the target using the _\<xccdf:target\>_ element. |
| any | | 0-n | Arbitrary contents, namespace="##other". This is for other target identification structures, such as those defined in the Asset Identification schema (see [IR7693]). Inclusion of an identifying element does not remove the requirement to identify the target using the _\<xccdf:target\>_ element. |
| platform (element) | string | 0-n | The platforms that the target system was found to meet. See Section 6.2.5. |
| set-value (element) | string | 0-n | Specific setting for a single _\<xccdf:Value\>_ element used during the test. |
| set-complex-value (element) | _special_ | 0-n | Specific setting for a single _\<xccdf:Value\>_ element used during the test when the given value is set to a complex type, such as a list. |
| rule-result (element) | _special_ | 0-n | The result of a single instance of a rule application against the target. The _\<xccdf:TestResult\>_ must include one _\<xccdf:rule-result\>_ record for each _\<xccdf:Rule\>_ that was selected in the resolved benchmark; it may also include _\<xccdf:rule-result\>_ records for _\<xccdf:Rule\>_ elements that were unselected in the benchmark. See Section 6.6.4. |
| score (element) | decimal | 1-n | An overall score for this benchmark test. The optional _@system_ attribute identifies the URI for a scoring model (see Section 7.3.2 for more information on pre-defined models). The optional _@maximum_ attribute specifies the maximum possible value of the score. |
| metadata (element) | _special_ | 0-n | XML metadata associated with this _\<xccdf:TestResult\>_. For example, this element can hold a copy of the _\<xccdf:metadata\>_ element of the _\<xccdf:Benchmark\>_ which served as the source for these results. This is especially useful if the _\<xccdf:TestResult\>_ will be separated from its source benchmark because the publication and support information in the benchmark can travel with the _\<xccdf:TestResult\>_. Benchmark consumers may also add their own metadata to _\<xccdf:TestResult\>_ elements they produce. See Section 6.2.3. |
| signature (element) | _special_ | 0-1 | A digital signature asserting authorship and allowing verification of the integrity of the _\<xccdf:TestResult\>_. See Section 6.2.7. |
| id (attribute) | _special_ | 1 | Unique identifier for this element; see Section 6.2.3. |
| start-time (attribute) | dateTime | 0-1 | Time when test began. |
| end-time (attribute) | dateTime | 1 | Time when test was completed and the results recorded. |
| test-system (attribute) | string | 0-1 | Name of the benchmark consumer program that generated this _\<xccdf:TestResult\>_ element; should be either a CPE name or a CPE applicability language expression. |
| version (attribute) | string | 0-1 | The version number string copied from the _\<xccdf:Benchmark\>_. |
| Id (attribute) | _special_ | 0-1 | An identifier used for referencing elements included in an XML signature. See Section 6.2.7. |

### 6.6.3 \<xccdf:fact\> Element

The URIs listed in Table 24 should be used, when applicable to the target, to record facts about the IT asset to which an _\<xccdf:TestResult\>_applies.

**Table 24: Predefined @name Attribute Values for \<xccdf:fact\> Elements**

| **URI** | **Description** |
| --- | --- |
| urn:xccdf:fact:asset:identifier:mac | Ethernet media access control address (should be sent as a pair with the IPv4 or IPv6 address to ensure uniqueness) |
| urn:xccdf:fact:asset:identifier:ipv4 | IPv4 address |
| urn:xccdf:fact:asset:identifier:ipv6 | IPv6 address |
| urn:xccdf:fact:asset:identifier:host\_name | Host name of the asset, if assigned |
| urn:xccdf:fact:asset:identifier:fqdn | Fully qualified domain name |
| urn:xccdf:fact:asset:identifier:ein | Equipment identification number or other inventory tag number |
| urn:xccdf:fact:asset:identifier:pki: | X.509 PKI certificate for the asset (encoded in Base-64) |
| urn:xccdf:fact:asset:identifier:pki:thumbprint | SHA-1 hash of the PKI certification for the asset (encoded in Base-64) |
| urn:xccdf:fact:asset:identifier:guid | Globally unique identifier for the asset |
| urn:xccdf:fact:asset:identifier:ldap | LDAP directory string (distinguished name) of the asset, if assigned |
| urn:xccdf:fact:asset:identifier:active\_directory | Active Directory realm to which the asset belongs, if assigned |
| urn:xccdf:fact:asset:identifier:nis\_domain | NIS domain of the asset, if assigned |
| urn:xccdf:fact:asset:environmental\_information:owning\_organization | Organization that tracks the asset on its inventory |
| urn:xccdf:fact:asset:environmental\_information:current\_region | Geographic region where the asset is located |
| urn:xccdf:fact:asset:environmental\_information:administration\_unit | Name of the organization that does system administration for the asset |
| urn:xccdf:fact:asset:environmental\_information:administration\_poc:title | Title (e.g., Mr, Ms, Col) of the system administrator for an asset |
| urn:xccdf:fact:asset:environmental\_information:administration\_poc:e-mail | E-mail address of the system administrator for the asset |
| urn:xccdf:fact:asset:environmental\_information:administration\_poc:first\_name | First name of the system administrator for the asset |
| urn:xccdf:fact:asset:environmental\_information:administration\_poc:last\_name | Last name of the system administrator for the asset |

### 6.6.4 \<xccdf:rule-result\> Element

#### 6.6.4.1 Properties

The _\<xccdf:rule-result\>_ element within the _\<xccdf:TestResult\>_ element holds the result of applying an _\<xccdf:Rule\>_ from the benchmark to a target system or component of a target system. Table 25 describes the _\<xccdf:rule-result\>_ element's properties.

**Table 25: \<xccdf:rule-result\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| result (element) | string | 1 | Result of applying this _\<xccdf:Rule\>_ to a target or target component: must be set to one of the values listed in Table 26. |
| override (element) | _special_ | 0-n | An XML block explaining how and why an auditor chose to override the result. See Section 6.6.4.3. |
| ident (element) | string | 0-n | A long-term globally meaningful identifier for the issue, vulnerability, platform, etc. copied from the _\<xccdf:Rule\>_. Shall have a _@system_ attribute designating the URI of the organization or scheme that assigned the identifier. An _\<xccdf:ident\>_ element MAY also have other attributes pulled from other namespaces in order to provide additional metadata for the given identifier. See Section 6.4.4.3 for more on this element. |
| metadata (element) | _special_ | 0-n | XML metadata associated with this _\<xccdf:rule-result\>_. For example, this element could hold a copy of the metadata of the _\<xccdf:Rule\>_ that served as the source for these results. Benchmark consumers may also add their own metadata to the _\<xccdf:rule-result\>_ elements they produce. See Section 6.2.4 for additional information. |
| message (element) | string | 0-n | Diagnostic messages from the checking engine, with a REQUIRED _@severity_ attribute that denotes the seriousness or conditions of the message. There are three message severity values: "error", "warning", and "info". These elements shall not affect scoring; they are present merely to convey diagnostic information from the checking engine. Benchmark consumers may choose to log these messages or display them to the user. |
| instance (element) | string | 0-n | Name of the target subsystem or component to which this result applies, for a multiply instantiated _\<xccdf:Rule\>_. The element is important for an _\<xccdf:Rule\>_ that applies to components of the target system, especially when a target might have several such components, and where the _@multiple_ attribute of the _\<xccdf:Rule\>_ is set to true. For example, an _\<xccdf:Rule\>_ might specify a particular setting that needs to be applied on every interface of a firewall; for benchmark results, a firewall target with three interfaces could have three _\<xccdf:rule-result\>_ elements with the same rule id, each with an independent value for the _\<xccdf:result\>_ element. For more discussion of multiply instantiated _\<xccdf:Rule\>_ elements, see Section 7.2.3.4.The optional _@context_ and _@parentContext_ attributes provide context and hierarchy information for nested contexts. If the _@context_ attribute is omitted, the value of the context shall default to "undefined". At most one _\<xccdf:instance\>_ child of an _\<xccdf:rule-result\>_ may have a context of "undefined". |
| fix (element) | string | 0-n | Fix script for this target platform, if available (would normally appear only for result values of "fail"). See Table 9. It is assumed to have been 'instantiated' by the testing tool and any substitutions or platform selection already made. |
| check (element) | _special_ | (1-n instances of check) XOR (1 instance of complex-check) | Encapsulated or referenced results to detailed testing output from the checking engine (if any). Consists of the URI that designates the checking system, and detailed output data from the checking engine. The detailed output data may take the form of encapsulated XML or text data, or it may be a reference to an external URI. (Note: this is analogous to the form of the _\<xccdf:Rule\>_ element's _\<xccdf:check\>_ element, used for referring to checking engine input.) See Section 6.4.4.4. |
| complex-check (element) | _special_ | (1-n instances of check) XOR (1 instance of complex-check) | A copy of the _\<xccdf:Rule\>_ element's _\<xccdf:complex-check\>_ element where each component _\<xccdf:check\>_ element of the _\<xccdf:complex-check\>_ element is an encapsulated or referenced results to detailed testing output from the checking engine (if any) as described above. See Section 6.4.4.4. |
| idref (attribute) | identifier | 1 | Identifier of an _\<xccdf:Rule\>_(from the benchmark designated in the _\<xccdf:TestResult\>_). |
| role (attribute) | string | 0-1 | The role string copied from the _@role_ attribute of the _\<xccdf:Rule\>_. |
| severity (attribute) | string | 0-1 | The _@severity_ attribute value copied from the _\<xccdf:Rule\>_(default: "unknown"). |
| time (attribute) | dateTime | 0-1 | Time when application of this instance of this _\<xccdf:Rule\>_ was completed. |
| version (attribute) | string | 0-1 | The version number string copied from the _\<xccdf:version\>_ element of the _\<xccdf:Rule\>._ |
| weight (attribute) | decimal | 0-1 | The weight number copied from the _@weight_ attribute of the _\<xccdf:Rule\>_. Expressed as a non-negative real number (0.0 or greater, maximum 3 digits, default 1.0). |

#### 6.6.4.2 \<xccdf:result\>Element

Table 26 lists the possible results of a single test (assigned to the _\<xccdf:result\>_ element).

**Table 26: Possible Results for a Single Test**

| **Result and Abbreviation** | **Description** |
| --- | --- |
| pass [P] | The target system or system component satisfied all the conditions of the _\<xccdf:Rule\>_. |
| fail [F] | The target system or system component did not satisfy all the conditions of the _\<xccdf:Rule\>_. |
| error [E] | The checking engine could not complete the evaluation, therefore the status of the target's compliance with the _\<xccdf:Rule\>_ is not certain. This could happen, for example, if a testing tool was run with insufficient privileges and could not gather all of the necessary information. |
| unknown [U] | The testing tool encountered some problem and the result is unknown. For example, a result of 'unknown' might be given if the testing tool was unable to interpret the output of the checking engine (the output has no meaning to the testing tool). |
| notapplicable [N] | The _\<xccdf:Rule\>_ was not applicable to the target of the test. For example, the _\<xccdf:Rule\>_ might have been specific to a different version of the target OS, or it might have been a test against a platform feature that was not installed. |
| notchecked [K] | The _\<xccdf:Rule\>_ was not evaluated by the checking engine. This status is designed for _\<xccdf:Rule\>_ elements that have no _\<xccdf:check\>_ elements or that correspond to an unsupported checking system. It may also correspond to a status returned by a checking engine if the checking engine does not support the indicated check code. |
| notselected [S] | The _\<xccdf:Rule\>_ was not selected in the benchmark. |
| informational [I] | The _\<xccdf:Rule\>_ was checked, but the output from the checking engine is simply information for auditors or administrators; it is not a compliance category. This status value is designed for _\<xccdf:Rule\>_ elements whose main purpose is to extract information from the target rather than test the target. |
| fixed [X] | The _\<xccdf:Rule\>_ had failed, but was then fixed (possibly by a tool that can automatically apply remediation, or possibly by the human auditor). |

#### 6.6.4.3 \<xccdf:override\> Element

The _\<xccdf:override\>_ element provides a mechanism for an auditor to change the result assigned by the checking tool or to document the reason for deviation from the rule requirement. This is necessary when checking a rule requires reviewing manual procedures or other non-IT conditions, and/or when a check gives an inaccurate result on a particular target system. The _\<xccdf:override\>_ element shall contain the properties listed in Table 27. Note that the _\<xccdf:override\>_ element is unrelated to the _@override_ attribute (Section 6.3.1). If the _\<xccdf:override\>_ element is present, the _\<xccdf:result\>_ element SHALL be changed to match the _\<xccdf:override\>_ element's _\<xccdf:new-result\>_ property.

**Table 27: \<xccdf:override\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| old-result (element) | string | 1 | The rule result status before this override |
| new-result (element) | string | 1 | The new, override rule result status |
| remark (element) | string | 1 | Rationale or explanation text for why or how the override was applied. It MAY have an _@xml:lang_ attribute (see Section 6.2.10). |
| time (attribute) | dateTime | 1 | When the override was applied |
| authority (attribute) | string | 1 | Name or other identification for the human principal authorizing the override |

The example below shows how an _\<xccdf:override\>_ element would appear in an _ \<xccdf:rule-result\>._ Note: if an _\<xccdf:override\>_ element is added to an _\<xccdf:rule-result\>_ that was previously signed, it will break any XML digital signature applied to the enclosing _\<xccdf:TestResult\>_ element.

```xml
<xccdf:rule-result idref="xccdf_org.example_rule_rule76" time="2005-04-26T14:38:19Z" severity="low">
  <xccdf:result>pass</xccdf:result>
  <xccdf:override time="2005-04-26T15:03:20Z" authority="Bob Smith">
    <xccdf:old-result>fail</xccdf:old-result>
    <xccdf:new-result>pass</xccdf:new-result>
    <xccdf:remark>
      Manual inspection showed this rule is satisfied. The relevant registrykey was protected per policy, but with a more restrictive ACL than thebenchmark was designed to check. The rule result has been overridden to 'pass'.
    </xccdf:remark>
  </xccdf:override>
  <xccdf:instance context="registry">
    HKLM\SOFTWARE\Policies
  </xccdf:instance>
  <xccdf:check system="http://www.mitre.org/XMLSchema/oval-definitions-5">
    <xccdf:check-content-ref href="oval-out.xml" name="oval:org.example:def:123"/>
  </xccdf:check>
</xccdf:rule-result>
```

### 6.6.5 \<xccdf:tailoring-file\> Element

The _\<xccdf:tailoring-file\>_ element in the _\<xccdf:TestResult\>_ element is used to provide information about an _\<xccdf:Tailoring\>_ element. Table 28 shows the properties of an _\<xccdf:tailoring-file\>_ element. The _\<xccdf:tailoring-file\>_ element MUST be present if and only if one of the _\<xccdf:Profile\>_ elements of the _\<xccdf:Tailoring\>_ element was applied or created during the activities that created the given _\<xccdf:TestResult\>_ element. "Applied" refers to applying a profile during benchmark profile processing, while "created" refers to a user's manual tailoring actions resulting in the generation of a new _\<xccdf:Tailoring\>_ element. (See Section 6.7 for information on _\<xccdf:Tailoring\>_.)

**Table 28: \<xccdf:tailoring-file\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| href (attribute) | URI | 1 | Persistent location of the _\<xccdf:Tailoring\>_ element |
| id (attribute) | identifier | 1 | Identifier for the _\<xccdf:Tailoring\>_ element |
| version (attribute) | string | 1 | Version of the _\<xccdf:Tailoring\>_ element |
| time (attribute) | dateTime | 1 | Timestamp for the _\<xccdf:Tailoring\>_ element |

## 6.7 \<xccdf:Tailoring\> Element

### 6.7.1 Basics

An _\<xccdf:Tailoring\>_ element allows named tailorings (i.e., profiles) of a benchmark to be defined separately from the benchmark itself. There are several reasons why this might be desirable:

- The benchmark might not be controlled by the organization wishing to add the profile to it.
- The benchmark might have digital signatures that would be corrupted by benchmark modification.
- The benchmark might undergo revision by its author, so modifications by a different party would represent a development fork that complicates maintenance.
- It enables the capturing of manual tailoring actions in a well-defined format (see Section 5.2).

The profiles in an _\<xccdf:Tailoring\>_ element can be used in two ways. First, an organization might wish to pre-define a set of tailoring actions to be applied on top of or instead of the tailoring performed by a benchmark's profiles. This may be necessary to adjust the benchmark to the local needs of an enterprise. By creating new _\<xccdf:Profile\>_ elements with these tailorings and saving them in the _\<xccdf:Tailoring\>_ element, these can be applied to future assessments. The _\<xccdf:Tailoring\>_ elements also serve as records of these additional tailoring actions, providing the context needed to interpret _\<xccdf:TestResult\>_ elements.

In addition, an _\<xccdf:Tailoring\>_ element can be used to to record manual tailoring actions performed during the course of an assessment. Manual tailoring actions SHOULD be limited to actions that could be performed using selectors in an _\<xccdf:Profile\>_ element. If this is done, any manual tailoring action can be recorded in a series of selectors in a newly defined _\<xccdf:Profile\>_ element. By storing this _\<xccdf:Profile\>_ in an _\<xccdf:Tailoring\>_ element, the processes that led to a particular set of _\<xccdf:TestResult\>_ elements can be saved for future reference. Of course, such an _\<xccdf:Tailoring\>_ element could then be used as input to subsequent assessments.

### 6.7.2 Properties

An _\<xccdf:Tailoring\>_ element holds one or more _\<xccdf:Profile\>_ elements. These _\<xccdf:Profile\>_ elements record additional tailoring activities that apply to a given _\<xccdf:Benchmark\>_. _\<xccdf:Tailoring\>_ elements are separate from benchmark documents, but each _\<xccdf:Tailoring\>_ element is associated with a specific benchmark document. By defining these tailoring actions separately from the benchmark document to which they apply, these actions can be recorded without affecting the integrity of the source itself.

Table 29 lists the possible properties for the _\<xccdf:Tailoring\>_ element.

**Table 29: \<xccdf:Tailoring\> Element Properties**

| Property | Type | Count | Description |
| --- | --- | --- | --- |
| benchmark (element) | _special_ | 0-1 | Identifies the Benchmark to which this tailoring applies. A tailoring document is only applicable to a single Benchmark. The _\<xccdf:benchmark\>_ element holds the associated Benchmark's location URI, version string, and id value. Note, however, that this is a purely informative field. Benchmark consumers are not required to perform any lookup or even verify that a tailoring document is being applied to the correct Benchmark. |
| status (element) | _special_ | 0-n | Status of the tailoring and date at which it attained that status. Authors MAY use this element to record the maturity or consensus level of a tailoring. See Section 6.2.8. |
| dc-status (element) | _special_ | 0-n | Holds additional status information using the Dublin Core format. See Section 6.2.8. |
| version (element) | _special_ | 1 | The version of this tailoring,with a REQUIRED _@time_ attribute(when it was created). This is necessary because, under some circumstances, a copy of a tailoring document might be automatically generated. Without the version and timestamp, tracking of these automatically created tailoring documents could become problematic. |
| metadata (element) | _special_ | 0-n | XML metadata for the tailoring. See Section 6.2.4. |
| Profile (element) | Profile | 1-n | Profiles that reference and customize sets of items in a Benchmark; see Section 6.5. These elements are identical to Profiles that might be found in a normal Benchmark. Moreover, they can extend the Profiles in the tailoring document's associated Benchmark using normal Profile extension mechanics. However, Profiles in the tailoring document cannot themselves be extended. For this reason, no Profile in the tailoring document should have its abstract property set to "true". |
| signature (element) | _special_ | 0-1 | A digital signature asserting authorship and allowing verification of the integrity of the tailoring. See Section 6.2.7. |
| id (attribute) | _special_ | 1 | Unique identifier for this element. See Section 6.2.3. |
| Id (attribute) | _special_ | 0-1 | An identifier used for referencing elements included in an XML signature. See Section 6.2.7. |

### 6.7.3 Profile Shadowing

Profiles in an _\<xccdf:Tailoring\>_ element MAY "shadow" profiles in the associated benchmark document. When a tailoring profile shadows a benchmark profile, it assumes the identifier of that profile in all subsequent processing, and the shadowed benchmark profile effectively becomes invisible. Shadowing occurs when the tailoring profile's _@id_ AND _@extends_ attributes are both identical to the _@id_ attribute of the benchmark profile. Note that a tailoring profile MAY extend a benchmark profile without shadowing it; the tailoring profile would simply use an identifier different from the identifier of the benchmark profile. However, a tailoring profile's _@id_ attribute SHALL NOT duplicate a benchmark profile's id unless the tailoring profile is extending the benchmark profile.

Table 30 summarizes this behavior. In this table, consider an _\<xccdf:Tailoring\>_ element with a single _\<xccdf:Profile\>_ whose _@id_ and _@extends_ attributes have the given values. This _\<xccdf:Tailoring\>_ element is associated with an _\<xccdf:Benchmark\>_ that likewise has an _\<xccdf:Profile\>_ whose _@id_ attribute has the given value.

**Table 30: Profile Shadowing Behavior**

| Tailoring Profile || Benchmark Profile | Behavior |
| --- | --- | --- | --- |
| id | extends | id |
| A | A | A | The tailoring profile shadows the benchmark profile. If profile "A" is selected and applied, it will be the profile of that name as defined in the _\<xccdf:Tailoring\>_ element. |
| B | A | A | The tailoring profile extends but does not shadow the benchmark profile. If profile "A" is applied, it is the benchmark profile of that name that is applied. If profile "B" is applied, it is the tailoring profile of that name that is applied. |
| A | B | A | An error is thrown during processing: the tailoring profile identifier duplicates the identifier of a benchmark profile without extending that profile. |

### 6.7.4 Tailoring Actions and Profile Selectors

As mentioned earlier, a tailoring profile can be used to record manual tailoring actions and to serve as a record of these actions when evaluating a given _\<xccdf:TestResult\>_. In such a case, the following user tailoring actions are represented by the profile selectors designated in Table 31.

**Table 31: Tailoring Actions and Profile Selectors**

| Action | Selector |
| --- | --- |
| User selects or de-selects a Rule | _\<xccdf:select\>_ |
| User provides a value for use with a given Value | _\<xccdf:set-value\>_ |
| User provides a list of values for use with a given Value | _\<xccdf:set-complex-value\>_ |
| User switches the check that a Rule is to perform | _\<xccdf:refine-rule\>_ |
| User changes the weight of a Rule or Group | _\<xccdf:refine-rule\>_ |
