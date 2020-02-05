# XCCDF Data Model

## Introduction

The XCCDF data model and its XML representation are intended to be
platform-independent and portable to foster broad adoption and sharing
of rules. The fundamental data model for XCCDF consists of the following
main element data types:

-   **Benchmark.** An XCCDF benchmark document holds an
    > *\<xccdf:Benchmark\>* element, which acts as a container for other
    > elements, including *\<xccdf:Group\>*, *\<xccdf:Rule\>*,
    > *\<xccdf:Value\>*, *\<xccdf:Profile\>,* and *\<xccdf:TestResult\>*
    > elements.

-   **Item.** An item is a named constituent of an
    > *\<xccdf:Benchmark\>*. There are three types of items:

    -   **Group.** An *\<xccdf:Group\>* holds other items. An
        > *\<xccdf:Group\>* collects related *\<xccdf:Rule\>* and
        > *\<xccdf:Value\>* elements into a common structure and can
        > provide descriptive text and references about them. An
        > *\<xccdf:Group\>* allows benchmark users to select and
        > deselect related *\<xccdf:Rule\>* elements together; since a
        > deselected *\<xccdf:Group\>* is not processed, none of its
        > contained items are processed either. Selection of an
        > *\<xccdf:Group\>* allows its children to be processed normally
        > based on their individual selection states.

    -   **Rule.** An *\<xccdf:Rule\>* element holds check references and
        > can also hold remediation information.

    -   **Value.** An *\<xccdf:Value\>* element is a named data value
        > that can be substituted into other items' properties or into
        > checks. It can have an associated data type and metadata that
        > express how the value should be used and how it can be
        > tailored.

<!-- -->

-   **Profile.** An *\<xccdf:Profile\>* element is a named tailoring of
    > a benchmark using a collection of attributed references to
    > *\<xccdf:Rule\>*, *\<xccdf:Group\>*, and *\<xccdf:Value\>*
    > elements. It allows definition of named levels or baselines in a
    > benchmark (see Section 5.2).

-   **TestResult.** An *\<xccdf:TestResult\>* element holds the results
    > of performing a test or check against a single target device or
    > system. An *\<xccdf:TestResult\>* element references
    > *\<xccdf:Rule\>* and *\<xccdf:Value\>* elements and may also
    > reference an *\<xccdf:Profile\>* element.

-   **Tailoring.** A tailoring document holds exactly one
    > *\<xccdf:Tailoring*\> element, which contains *\<xccdf:Profile\>*
    > elements to modify the behavior of an *\<xccdf:Benchmark\>*.

The rest of this section explains the relationships between these main
element data types and examines each of the types in more detail.
Section 6.2 discusses general information that applies to most or all of
the main element data types. Sections 6.3 through 6.7 cover
*\<xccdf:Benchmark\>*, item (*\<xccdf:Group\>*, *\<xccdf:Rule\>*,
*\<xccdf:Value\>*), *\<xccdf:Profile\>*, *\<xccdf:TestResult\>*, and
*\<xccdf:Tailoring\>* elements, respectively.

## General XML Information

### XCCDF Namespace and XML Schema

The namespace URI for this specification shall be
"http://checklists.nist.gov/xccdf/1.2". Applications that process XCCDF
should use the namespace URI to decide whether or not they can process a
given document. The XML representation of XCCDF is expressed as an XML
schema at <http://scap.nist.gov/specifications/xccdf/#resource-1.2>. The
XML schema implementation of XCCDF shall be the authoritative XML
binding definition for XCCDF.

### Element and Attribute Formatting

The structure of an XCCDF document supports transformation into HTML and
various XML formats to promote portability and interoperability. The
XCCDF language also allows for the inclusion of content that does not
contribute directly to the technical content, such as an introduction, a
rationale, warnings, and external references. XCCDF also provides
mechanisms to document authors for formatting text, including images,
and referencing other information resources (e.g., prose publications),
but these mechanisms are separable from the text itself so they can be
filtered out by applications that do not support or require them.

Throughout the rest of this section, there are tables that show the
possible properties of each main element in the XCCDF data model. In the
tables, properties of type "identifier" must be strings obeying the
definition of "NCName" from \[XMLSCHEMA\]. Properties of type "string"
and "text" must not include XHTML formatting. Properties of type
"HTML-enabled text" are string data that may include embedded
formatting, presentation, and hyperlink structure; if present, these
must be expressed using Extensible Hypertext Markup Language (XHTML)
Basic tags \[XHTML\]. The core modules noted in \[XHTML\] and the
Presentation module may be used for this.

XHTML markup allows authors to specify how the content of certain fields
should be displayed to a user. However, while this allows authors to
specify every detail of a field\'s display, authors may use classifiers
in these fields, using class attributes in \<div\> or \<span\> elements.
These tell products the type of content contained within a text block
and allow the products to display this content according to their own
conventions. Authors may wish to do this so their content fits better
with other automated formatting choices made by a product or a
stylesheet, and also because some products may not support the complete
range of HTML tags in their displays, thus causing some explicit
formatting to be ignored. A list of recommended class values appears in
Table 2. Authors may use class values not included in this list, and it
is optional for products to have special formatting for any of the
listed classifiers. However, both authors and products should use these
class designators when appropriate to provide more effective display of
content. Use of these class attribute values shall be applicable
wherever HTML content can be included in XCCDF documents.

[]{#_Ref294715794 .anchor}Table : Recommended Class Values

  -------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Class Value    Meaning
  license        Licensing and use information
  copyright      Copyright and ownership information
  tangent        A block of text that contains tangentially related information (possibly appropriate for inclusion as a sidebar or a pop-up)
  warning        Pitfalls or cautions relative to the surrounding text. High-level and general warnings should appear in designated warning fields, if available.
  critical       Content of critical importance to the user
  example        An example of some kind
  instructions   Special instructions to the user
  default        General information. Empty or absent class attributes also imply \"default\" appearance. This tag allows authors to explicitly indicate that text should appear in the default format.
  -------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Element Identifiers

The elements listed in Table 3 have special conventions around the
format of their identifiers (*\@id* attribute). Authors MUST follow
these conventions because they make it easier to preserve the global
uniqueness of the resulting identifiers.

[]{#_Ref297105058 .anchor}Table : Element Identifier Format Conventions

  Element      Format Convention
  ------------ ----------------------------------------
  Benchmark    xccdf\_*namespace*\_benchmark\_*name*
  Profile      xccdf\_*namespace*\_profile\_*name*
  Group        xccdf\_*namespace*\_group\_*name*
  Rule         xccdf\_*namespace*\_rule\_*name*
  Value        xccdf\_*namespace*\_value\_*name*
  TestResult   xccdf\_*namespace*\_testresult\_*name*
  Tailoring    xccdf\_*namespace*\_tailoring\_*name*

In Table 3, *namespace* contains a valid reverse-DNS style string
(limited to letters, numbers, periods, and the hyphen character) that is
associated with the content author. Examples include
\"com.acme.finance\" and \"gov.tla\". These *namespace* strings may have
any number of parts, and benchmark consumers processing them SHALL treat
them as case-insensitive. (That is, com.ABC is considered identical to
com.abc.) This association with the content author is solely to ensure
that different content authors will not use the same namespace value, as
this could lead to identifier collisions. Readers should not
automatically infer any special authority or trust of the content based
on the namespace since reuse of the element or changes to referenced
content may not align with the author\'s original intent. The *name*
component in the format conventions may be any NCName-compliant string
\[XMLSCHEMA\]. Identifier fields other than those noted in Table 3 have
no additional formatting conventions beyond compliance with the NCName
data type.

See the example *\<xccdf:Profile\>* in Section 6.5.1 for several
examples of the element identifier format conventions.

### \<xccdf:metadata\> Element

XCCDF supports inclusion of metadata about a document, including title,
name of author(s), organization providing the guidance, version number,
release date, update URL, and a description. This is particularly useful
for facilitating the discovery and retrieval of XCCDF checklists from
public repositories.

When used, the *\<xccdf:metadata\>* element shall contain document
metadata expressed in XML. The metadata element can appear one or more
times as a child of the *\<xccdf:Benchmark\>*, *\<xccdf:Rule\>*,
*\<xccdf:Group\>*, *\<xccdf:Value\>*, *\<xccdf:Profile\>*,
*\<xccdf:TestResult\>*, *\<xccdf:rule-result\>*, and
*\<xccdf:Tailoring\>* elements. The *\<xccdf:Benchmark\>* element SHOULD
contain metadata information formatted using the Dublin Core Metadata
Initiative (DCMI) Simple DC Element specification, as described in
\[DCES\] and \[DCXML\]. Benchmark consumers should be prepared to
process Dublin Core metadata in the *\<xccdf:metadata\>* element. An
example of the Dublin Core format is shown below.

+-------------------------------------------------------------------+
| \<xccdf:metadata xmlns:dc=\"http://purl.org/dc/elements/1.1/\"\>  |
|                                                                   |
| \<dc:title\>Security Benchmark for Ethernet Hubs\</dc:title\>     |
|                                                                   |
| \<dc:creator\>James Smith\</dc:creator\>                          |
|                                                                   |
| \<dc:publisher\>Center for Internet Security\</dc:publisher\>     |
|                                                                   |
| \<dc:subject\>network security for layer 2 devices\</dc:subject\> |
|                                                                   |
| \</xccdf:metadata\>                                               |
+-------------------------------------------------------------------+

Elements that support the *\<xccdf:metadata\>* element MAY use any
desired metadata format; for *\<xccdf:Benchmark\>*, this is in addition
to the Dublin Core recommendations already mentioned. Any XML metadata
structures, including ad hoc structures, may be included in an
*\<xccdf:metadata\>* element. Because any structures can appear in this
field, authors should tag metadata with the *xmlns* prefix \[XMLNAME\]
and, if available, the *\@xsi:schemaLocation* attribute in order to
identify the metadata structure utilized. Metadata should comply with
existing commercial or government metadata specifications to allow
benchmarks to be discovered and indexed.

Metadata is a powerful feature for authors. However, XCCDF puts some
limits on the use of metadata:

-   Metadata should not replace the functionality of existing fields
    within XCCDF. If the information matches the common use of some
    other XCCDF field, the information must appear in that XCCDF field.
    It MAY also appear in the *\<xccdf:metadata\>* element.

-   Metadata shall not change the processing model as outlined in
    Section 7. Benchmark consumer products shall perform XCCDF
    assessments in the same way regardless of the presence of metadata.
    Metadata may still be used to support assessment features that are
    outside the purview of XCCDF---for example, to hold post-processing
    instructions to be performed on the completed XCCDF results or to
    display additional information to the user before or during the
    assessment.

-   Metadata SHOULD not alter the character content of the XCCDF
    properties used to generate output during document generation.
    Contents of the *\<xccdf:metadata\>* element MAY be added to the
    output above and beyond the XCCDF properties. Metadata may contain
    instructions that cause different stylistic conventions to be
    adopted in the conversion of an XCCDF document to prose.

### Platform Names

The Common Platform Enumeration (CPE) specification allows a specific
hardware or software platform to be identified by a unique name. CPE
names can express only single platforms (e.g.,
\"cpe:2.3:o:microsoft:windows\_xp:\*:gold:professional:\*:\*:\*:\*:\*\"
for Microsoft Windows XP Professional Edition). CPE applicability
language statements can express complex logical constructions of CPE
names.

*\<xccdf:Benchmark\>*, *\<xccdf:Profile\>,* *\<xccdf:Rule\>*, and
*\<xccdf:Group\>* elements may be qualified by applicable platform using
the *\<xccdf:platform\>* element. *\<xccdf:TestResult\>* elements may
also include *\<xccdf:platform\>* elements. The *\<xccdf:platform\>*
element's *\@idref* attribute may hold a reference to either a CPE name
or the *\@id* attribute of a CPE applicability language expression that
SHALL be defined as a child of the *\<xccdf:Benchmark\>* using a
*\<cpe2:platform-specification\>* element (see Table 4). (The syntax for
referring to a local CPE applicability language identifier SHOULD be a
"\#" character before the identifier, such as "\#cpeid1".)

Within XCCDF documents, all CPE names SHALL comply with the CPE 2.3
Naming specification \[IR7695\], and all CPE applicability language
expressions SHALL comply with the CPE 2.3 Applicability Language
specification \[IR7698\]. CPE 2.0 names MAY be used for backwards
compatibility, but their use has been deprecated for XCCDF 1.2. All CPE
2.3 names and applicability language expressions in XCCDF documents
SHOULD use formatted string bindings but MAY use URI bindings instead,
both as defined in \[IR7695\].

Here is an example of a *\<cpe2:platform-specification\>* element:

```xml
<cpe2:platform-specification>
  <cpe2:platform id="xp_and_acrobat_9.0">
    <cpe2:logical-test operator="AND" negate="false">
      <cpe2:fact-ref
         name="cpe:2.3:o:microsoft:windows_xp:*:*:*:*:*:*:*:*"/>
      <cpe2:fact-ref
         name="cpe:2.3:a:adobe:acrobat:9.0:*:*:*:*:*:*:*"/>
    </cpe2:logical-test>
   </cpe2:platform>
</cpe2:platform-specification>
<xccdf:platform idref="#xp_and_acrobat_9.0"/>
```

If an *\<xccdf:Profile\>,* *\<xccdf:Rule\>*, or *\<xccdf:Group\>* does
not possess any *\<xccdf:platform\>* elements, then it shall apply to
the same set of platforms as its nearest enclosing ancestor
*\<xccdf:Group\>* or *\<xccdf:Benchmark\>*. If the enclosing ancestor
has no *\<xccdf:platform\>* element, then consult the next enclosing
ancestor, repeating until either an ancestor has an *\<xccdf:platform\>*
element or the *\<xccdf:Benchmark\>* element is reached. If no ancestor,
including the *\<xccdf:Benchmark\>* element, possesses any
*\<xccdf:platform\>* elements, then the *\<xccdf:Profile\>,*
*\<xccdf:Rule\>*, or *\<xccdf:Group\>* shall nominally apply to all
platforms.

### \<xccdf:reference\> Element

The *\<xccdf:reference\>* element provides a reference to a document or
resource where the user can learn more about the subject of the parent
element. It may be included within *\<xccdf:Benchmark\>*,
*\<xccdf:Rule\>*, *\<xccdf:Group\>*, *\<xccdf:Value\>*, and
*\<xccdf:Profile\>* elements. When used, it SHALL have either a simple
string value or a value consisting of simple Dublin Core elements as
described in \[DCXML\]. It MAY also have an *\@href* attribute that
gives a URL for the referenced resource. References SHOULD be given as
Dublin Core descriptions; a bare string may be used for simplicity. If a
bare string appears, then it is taken to be the string content for a
*\<dc:title\>* element. For more information, consult \[DCES\]. Multiple
*\<xccdf:reference\>* elements may appear; a benchmark consumer product
MAY concatenate them or put them into a reference list, and MAY choose
to number them.

### \<xccdf:signature\> Element

XCCDF supports a digital signature mechanism whereby XCCDF document
users can validate the integrity, origin, and authenticity of documents.
XCCDF provides a means to hold such signatures and a uniform method for
applying and validating them \[XMLDSIG\].

The *\<xccdf:signature\>* element may hold an enveloped digital
signature asserting authorship and allowing verification of the
integrity of associated data (e.g., its parent element, other documents,
portions of other documents). The signature shall apply only after
inclusion (i.e., XInclude) processing. The *\<xccdf:signature\>* element
is optional, and it may appear as a child of the *\<xccdf:Benchmark\>*,
*\<xccdf:Rule\>*, *\<xccdf:Group\>*, *\<xccdf:Value\>*,
*\<xccdf:Profile\>*, *\<xccdf:TestResult\>*, or *\<xccdf:Tailoring\>*
elements.

Any digital signature format employed for XCCDF must be capable of
identifying the signer, storing all information needed to verify the
signature (usually a certificate or certificate chain), and detecting
any change to the signed content. XCCDF products that support signatures
must support the W3C XML-Signature standard enveloped signatures, as
defined in \[XMLDSIG\].

If multiple signatures are needed in an XCCDF document, at most one of
them MAY be enveloped; all others must be detached \[XMLDSIG Section
2\].

The single child element of the *\<xccdf:signature\>* element should be
*\<dsig:Signature\>* and that *\<dsig:Signature\>* should contain one or
more *\<dsig:Reference\>* elements indicating each XML element to be
included in the signature. The *\<dsig:Reference\>* URI should be a
relative URI to the *\@Id* attribute value of the enclosing object
(prefixed by "\#"). For example, if the Id="abc", then the
*\<Reference\>* URI would be "\#abc".

### Status Tracking

XCCDF provides two elements for status tracking: *\<xccdf:status\>* and
*\<xccdf:dc-status\>*. Both elements are available for
*\<xccdf:Benchmark\>*, *\<xccdf:Rule\>*, *\<xccdf:Group\>*,
*\<xccdf:Value\>*, *\<xccdf:Profile\>*, and *\<xccdf:Tailoring\>*
elements.

The intent of the *\<xccdf:status\>* element is to record the maturity
or consensus level for its parent element. Permissible values are
"incomplete" (under initial development), "draft" (released in draft
state), "interim" (revised and in the process of being finalized),
"accepted" (released as final), and "deprecated" (no longer needed). If
there is more than one *\<xccdf:status\>* element for a single parent
element, then every instance of the *\<xccdf:status\>* element MUST have
a date attribute, and the *\<xccdf:status\>* element with the latest
date shall be considered the latest status.

The *\<xccdf:dc-status\>* element holds additional status information
using the Dublin Core format, expressed as elements of the DCMI Simple
DC Element specification, as described in \[DCES\] and \[DCXML\].

### Text Substitution

XCCDF provides capabilities to perform substitution within certain
properties of an *\<xccdf:Benchmark\>*.

An *\<xccdf:plain-text\>* element is a reusable text block for reference
by the *\<xccdf:sub\>* element. This allows text to be defined once and
then reused multiple times. Each *\<xccdf:plain-text\>* element MUST
have a unique NCName identifier. The identifiers for
*\<xccdf:plain-text\>* MUST NOT collide with item identifiers within the
*\<xccdf:Benchmark\>*.

The *\<xccdf:sub\>* element represents a reference to a parameter value
that may be set during tailoring. The *\<xccdf:sub\>* element is
supported by several elements, which are noted as they are defined
throughout the rest of this section. The *\<xccdf:sub\>* element shall
not have any content. It must have a single *\@idref* attribute, which
must equal the *\@id* attribute of an *\<xccdf:Value\>* element or an
*\<xccdf:plain-text\>* element. During document generation, each
instance of the *\<xccdf:sub\>* element will be replaced by the text
value of the referenced element's contents. If an *\<xccdf:Value\>*
element is referenced, the *\<xccdf:sub\>* element MAY have a *\@use*
attribute that indicates whether the *\<xccdf:Value\>* element\'s title
or value should replace the *\<xccdf:sub\>* element. If an
*\<xccdf:plain-text\>* element is referenced, the *\@use* attribute is
ignored and the body of the *\<xccdf:plain-text\>* element is always
used in the substitution.

See Section 7.2.3.6.3 for additional requirements related to
substitution processing.

### \@xml:lang Attribute

Some textual elements of XCCDF, such as titles and descriptions, support
the *\@xml:lang* attribute to denote the language they use.[^1] Elements
that have an *\@xml:lang* attribute may appear multiple times within a
single parent element to support multiple languages. If there are
multiple instances of such a textual element within a single parent
element, each instance SHOULD have a value for its *\@xml:lang*
attribute. Each value SHOULD specify the natural language locale for
which the instance is written (e.g., "en" for English, "fr" for French).
Values SHOULD be valid language tags as defined by \[RFC5646\]. Although
any valid language tag MAY be used, only tags containing language codes
(with or without region codes) SHOULD be used. All language and region
codes used SHOULD be in the *Internet Assigned Numbers Authority (IANA)
Language Subtag Registry* \[ILSR\]. An example of using the *\@xml:lang*
attribute is shown below.

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

If an element other than *\<xccdf:Benchmark\>* that supports the
*\@xml:lang* attribute omits it, the *\@xml:lang* attribute of the
nearest ancestor element that has the *\@xml:lang* attribute SHALL be
consulted.

## \<xccdf:Benchmark\>

### Basics

Each XCCDF benchmark document SHALL consist of a single root
*\<xccdf:Benchmark\>* element, which encloses the entire benchmark. The
example below illustrates the outermost structure of an XCCDF XML
document.

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
  <xccdf:platform idref="cpe:2.3:o:cisco:ios:12.0:*:*:*:*:*:*:*"/>
  <xccdf:version>0.2</xccdf:version>
  <xccdf:reference href="http://www.ietf.org/rfc/rfc822.txt">
      Standard for the Format of ARPA Internet Text Messages
  </xccdf:reference>
</xccdf:Benchmark>
```

Figure 1 illustrates a typical internal structure for a Benchmark.

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ![Diagram of XCCDF object nesting: Benchmark contains Profiles, Values and Groups. Groups contain Values, Groups, and Rules.](media/image3.wmf){width="6.5in" height="2.9166666666666665in"}
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

[]{#_Ref294716197 .anchor}Figure : Typical Structure of a Benchmark

The possible inheritance relations between Rule and Value instances are
constrained by the tree structure of the Benchmark. All extension
relationships must be resolved before the Benchmark can be applied. An
item may only extend another item of the same type that is visible from
its scope. In other words, an item Y may extend a base item X, as long
as they are the same type and one of the following visibility conditions
holds:

-   X is a direct child of the Benchmark.

-   X is a direct child of a Group which is also an ancestor[^2] of Y.

-   X is a direct child of a Group which is extended by any ancestor
    of Y.

For example, in the Benchmark structure shown in Figure 1, it would be
legal for Rule (g) to extend Rule (f) or extend Rule (h). It would not
be legal for Rule (i) to extend Rule (m), because (m) is not visible
from the scope of (i). It would not be legal for Rule (l) to extend
Value (k), because they are not of the same type.

The ability for an item to be extended gives benchmark authors the
ability to create variations or specialized versions of items without
making copies. An extending item can inherit properties from the
extended (base item) and sometimes can override properties with new
values if needed; however, there are several restrictions on these
inheritance capabilities. For example, some properties have different
inheritance behaviors depending on how their *\@override* attribute is
set. Also, group extension has been deprecated as of XCCDF 1.2 (see
Table 42) and authors are strongly discouraged from using it due to the
risks it poses to interoperability. See Section 7.2.2 for more
information on inheritance.

Circular dependencies caused by extension MUST not be defined.

### Properties

Table 4 describes the *\<xccdf:Benchmark\>* element's properties.

In this table and in similar tables throughout the rest of the section,
the Count column indicates how many times each property shall be
permitted within a single instance of the parent element. Each Count is
expressed as either a single value or a range of values. If the Count
entry for a property includes the value 0, the property is optional,
otherwise the property is required. If the Count entry for a property
includes the value n, the property MAY be used more than one time, with
no upper bound. Here are some examples:

-   1: the property shall be used once

-   1-n: the property shall be used at least once and may be used more
    than once

-   0-1: the property is optional and may be used at most once

-   0-n: the property is optional and may be used more than once

[]{#_Ref294716667 .anchor}Table : \<xccdf:Benchmark\> Element Properties

+-----------------+-----------------+-----------------+-----------------+
| Property        | Type            | Count           | Description     |
+=================+=================+=================+=================+
| status          | *special*       | 1-n             | Status of the   |
| (element)       |                 |                 | benchmark       |
|                 |                 |                 | (REQUIRED) and  |
|                 |                 |                 | date at which   |
|                 |                 |                 | it attained     |
|                 |                 |                 | that status     |
|                 |                 |                 | (OPTIONAL). The |
|                 |                 |                 | status should   |
|                 |                 |                 | indicate the    |
|                 |                 |                 | level of        |
|                 |                 |                 | maturity or     |
|                 |                 |                 | consensus for   |
|                 |                 |                 | the benchmark.  |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.8.          |
+-----------------+-----------------+-----------------+-----------------+
| dc-status       | *special*       | 0-n             | Holds           |
| (element)       |                 |                 | additional      |
|                 |                 |                 | status          |
|                 |                 |                 | information     |
|                 |                 |                 | using the       |
|                 |                 |                 | Dublin Core     |
|                 |                 |                 | format. See     |
|                 |                 |                 | Section 6.2.8.  |
+-----------------+-----------------+-----------------+-----------------+
| title (element) | string          | 0-n             | Title of the    |
|                 |                 |                 | benchmark; a    |
|                 |                 |                 | benchmark       |
|                 |                 |                 | SHOULD have a   |
|                 |                 |                 | title. It may   |
|                 |                 |                 | have an         |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.2.10) |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@override*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1). |
+-----------------+-----------------+-----------------+-----------------+
| description     | HTML-enabled    | 0-n             | Text that       |
| (element)       | text            |                 | describes the   |
|                 |                 |                 | benchmark; a    |
|                 |                 |                 | benchmark       |
|                 |                 |                 | SHOULD have a   |
|                 |                 |                 | description. It |
|                 |                 |                 | MAY have one or |
|                 |                 |                 | more            |
|                 |                 |                 | *\<xccdf:sub\>* |
|                 |                 |                 | elements (see   |
|                 |                 |                 | Section 6.2.9), |
|                 |                 |                 | an *\@override* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1), |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10).        |
+-----------------+-----------------+-----------------+-----------------+
| notice          | HTML-enabled    | 0-n             | Legal notices   |
| (element)       | text            |                 | (licensing      |
|                 |                 |                 | information,    |
|                 |                 |                 | terms of use,   |
|                 |                 |                 | etc.),          |
|                 |                 |                 | copyright       |
|                 |                 |                 | statements,     |
|                 |                 |                 | warnings, and   |
|                 |                 |                 | other advisory  |
|                 |                 |                 | notices about   |
|                 |                 |                 | this benchmark  |
|                 |                 |                 | and its use.    |
|                 |                 |                 | Each element    |
|                 |                 |                 | shall have a    |
|                 |                 |                 | unique NCName   |
|                 |                 |                 | identifier as   |
|                 |                 |                 | its *\@id*      |
|                 |                 |                 | attribute. Each |
|                 |                 |                 | element MAY     |
|                 |                 |                 | have an         |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.2.10) |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@xml:base*    |
|                 |                 |                 | attribute       |
|                 |                 |                 | (which defines  |
|                 |                 |                 | the context for |
|                 |                 |                 | all relative    |
|                 |                 |                 | URIs within the |
|                 |                 |                 | *\<xccdf:Benchm |
|                 |                 |                 | ark\>*          |
|                 |                 |                 | element).       |
+-----------------+-----------------+-----------------+-----------------+
| front-matter    | HTML-enabled    | 0-n             | Introductory    |
| (element)       | text            |                 | matter for the  |
|                 |                 |                 | beginning of    |
|                 |                 |                 | the benchmark   |
|                 |                 |                 | document;       |
|                 |                 |                 | intended for    |
|                 |                 |                 | use during      |
|                 |                 |                 | Document        |
|                 |                 |                 | Generation      |
|                 |                 |                 | processing only |
|                 |                 |                 | (see Section    |
|                 |                 |                 | 7.1). It MAY    |
|                 |                 |                 | have one or     |
|                 |                 |                 | more            |
|                 |                 |                 | *\<xccdf:sub\>* |
|                 |                 |                 | elements (see   |
|                 |                 |                 | Section 6.2.9), |
|                 |                 |                 | an *\@override* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1), |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10).        |
+-----------------+-----------------+-----------------+-----------------+
| rear-matter     | HTML-enabled    | 0-n             | Concluding      |
| (element)       | text            |                 | material for    |
|                 |                 |                 | the end of the  |
|                 |                 |                 | benchmark       |
|                 |                 |                 | document;       |
|                 |                 |                 | intended for    |
|                 |                 |                 | use during      |
|                 |                 |                 | Document        |
|                 |                 |                 | Generation      |
|                 |                 |                 | processing only |
|                 |                 |                 | (see Section    |
|                 |                 |                 | 7.1). It MAY    |
|                 |                 |                 | have one or     |
|                 |                 |                 | more            |
|                 |                 |                 | *\<xccdf:sub\>* |
|                 |                 |                 | elements (see   |
|                 |                 |                 | Section 6.2.9), |
|                 |                 |                 | an *\@override* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1), |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10).        |
+-----------------+-----------------+-----------------+-----------------+
| reference       | *special*       | 0-n             | Supporting      |
| (element)       |                 |                 | references for  |
|                 |                 |                 | the benchmark   |
|                 |                 |                 | document. See   |
|                 |                 |                 | Section 6.2.6.  |
+-----------------+-----------------+-----------------+-----------------+
| plain-text      | string          | 0-n             | Definitions for |
| (element)       |                 |                 | reusable text   |
|                 |                 |                 | blocks, each    |
|                 |                 |                 | with a unique   |
|                 |                 |                 | identifier. See |
|                 |                 |                 | Section 6.2.9.  |
+-----------------+-----------------+-----------------+-----------------+
| > cpe2:\        | *special*       | 0-1             | A list of       |
| > platform-spec |                 |                 | identifiers for |
| ification       |                 |                 | complex         |
| > (element)     |                 |                 | platform        |
|                 |                 |                 | definitions,    |
|                 |                 |                 | written in CPE  |
|                 |                 |                 | applicability   |
|                 |                 |                 | language        |
|                 |                 |                 | format. Authors |
|                 |                 |                 | may define      |
|                 |                 |                 | complex         |
|                 |                 |                 | platforms       |
|                 |                 |                 | within this     |
|                 |                 |                 | element, and    |
|                 |                 |                 | then use their  |
|                 |                 |                 | locally unique  |
|                 |                 |                 | identifiers     |
|                 |                 |                 | anywhere in the |
|                 |                 |                 | *\<xccdf:Benchm |
|                 |                 |                 | ark\>*          |
|                 |                 |                 | element in      |
|                 |                 |                 | place of a CPE  |
|                 |                 |                 | name. See       |
|                 |                 |                 | Section 6.2.5.  |
+-----------------+-----------------+-----------------+-----------------+
| platform        | string          | 0-n             | Applicable      |
| (element)       |                 |                 | platforms for   |
|                 |                 |                 | this benchmark. |
|                 |                 |                 | Authors should  |
|                 |                 |                 | use the element |
|                 |                 |                 | to identify the |
|                 |                 |                 | systems or      |
|                 |                 |                 | products to     |
|                 |                 |                 | which the       |
|                 |                 |                 | benchmark       |
|                 |                 |                 | applies. See    |
|                 |                 |                 | Section 6.2.5.  |
+-----------------+-----------------+-----------------+-----------------+
| version         | string          | 1               | Version number  |
| (element)       |                 |                 | of the          |
|                 |                 |                 | benchmark, with |
|                 |                 |                 | an optional     |
|                 |                 |                 | *\@time*        |
|                 |                 |                 | timestamp       |
|                 |                 |                 | attribute (when |
|                 |                 |                 | the version was |
|                 |                 |                 | completed) and  |
|                 |                 |                 | an optional     |
|                 |                 |                 | *\@update* URI  |
|                 |                 |                 | attribute       |
|                 |                 |                 | (where updates  |
|                 |                 |                 | may be          |
|                 |                 |                 | obtained).      |
+-----------------+-----------------+-----------------+-----------------+
| metadata        | *special*       | 0-n             | XML metadata    |
| (element)       |                 |                 | for the         |
|                 |                 |                 | benchmark.      |
|                 |                 |                 | Benchmark       |
|                 |                 |                 | metadata allows |
|                 |                 |                 | authorship,     |
|                 |                 |                 | publisher,      |
|                 |                 |                 | support, and    |
|                 |                 |                 | other           |
|                 |                 |                 | information to  |
|                 |                 |                 | be embedded in  |
|                 |                 |                 | a benchmark.    |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.4.          |
+-----------------+-----------------+-----------------+-----------------+
| model (element) | URI             | 0-n             | URIs of         |
|                 |                 |                 | suggested       |
|                 |                 |                 | scoring models  |
|                 |                 |                 | to be used when |
|                 |                 |                 | computing a     |
|                 |                 |                 | score for this  |
|                 |                 |                 | benchmark. See  |
|                 |                 |                 | Section 7.3.2   |
|                 |                 |                 | for more        |
|                 |                 |                 | information on  |
|                 |                 |                 | models.         |
|                 |                 |                 | Parameters may  |
|                 |                 |                 | be provided as  |
|                 |                 |                 | needed for      |
|                 |                 |                 | particular      |
|                 |                 |                 | models through  |
|                 |                 |                 | the *\@param*   |
|                 |                 |                 | attribute.      |
|                 |                 |                 | *\@param*       |
|                 |                 |                 | attributes with |
|                 |                 |                 | equal name      |
|                 |                 |                 | values shall    |
|                 |                 |                 | not appear as   |
|                 |                 |                 | children of the |
|                 |                 |                 | same element.   |
+-----------------+-----------------+-----------------+-----------------+
| Profile         | *special*       | 0-n             | Profiles that   |
| (element)       |                 |                 | reference and   |
|                 |                 |                 | customize sets  |
|                 |                 |                 | of items in the |
|                 |                 |                 | benchmark; see  |
|                 |                 |                 | Section 6.5.    |
+-----------------+-----------------+-----------------+-----------------+
| Value (element) | *special*       | 0-n             | Parameter       |
|                 |                 |                 | values that     |
|                 |                 |                 | support Rules   |
|                 |                 |                 | and             |
|                 |                 |                 | descriptions in |
|                 |                 |                 | the benchmark;  |
|                 |                 |                 | see Section     |
|                 |                 |                 | 6.4.5.          |
+-----------------+-----------------+-----------------+-----------------+
| Group (element) | *special*       | 0-n             | Groups that     |
|                 |                 |                 | comprise the    |
|                 |                 |                 | benchmark; each |
|                 |                 |                 | may contain     |
|                 |                 |                 | additional      |
|                 |                 |                 | Values, Rules,  |
|                 |                 |                 | and other       |
|                 |                 |                 | Groups. See     |
|                 |                 |                 | Section 6.4.3.  |
+-----------------+-----------------+-----------------+-----------------+
| Rule (element)  | *special*       |                 | Rules that      |
|                 |                 |                 | comprise the    |
|                 |                 |                 | benchmark; see  |
|                 |                 |                 | Section 6.4.4.  |
+-----------------+-----------------+-----------------+-----------------+
| TestResult      | *special*       | 0-n             | Benchmark test  |
| (element)       |                 |                 | result records  |
|                 |                 |                 | (one per        |
|                 |                 |                 | benchmark run); |
|                 |                 |                 | see Section     |
|                 |                 |                 | 6.6.            |
+-----------------+-----------------+-----------------+-----------------+
| signature       | *special*       | 0-1             | A digital       |
| (element)       |                 |                 | signature       |
|                 |                 |                 | asserting       |
|                 |                 |                 | authorship and  |
|                 |                 |                 | allowing        |
|                 |                 |                 | verification of |
|                 |                 |                 | the integrity   |
|                 |                 |                 | of the          |
|                 |                 |                 | benchmark. See  |
|                 |                 |                 | Section 6.2.7.  |
+-----------------+-----------------+-----------------+-----------------+
| id (attribute)  | *special*       | 1               | Unique          |
|                 |                 |                 | benchmark       |
|                 |                 |                 | identifier. See |
|                 |                 |                 | Section 6.2.3.  |
+-----------------+-----------------+-----------------+-----------------+
| Id (attribute)  | *special*       | 0-1             | An identifier   |
|                 |                 |                 | used for        |
|                 |                 |                 | referencing     |
|                 |                 |                 | elements        |
|                 |                 |                 | included in an  |
|                 |                 |                 | XML signature.  |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.7.          |
+-----------------+-----------------+-----------------+-----------------+
| resolved        | boolean         | 0-1             | True if         |
| (attribute)     |                 |                 | benchmark has   |
|                 |                 |                 | already         |
|                 |                 |                 | undergone the   |
|                 |                 |                 | resolution      |
|                 |                 |                 | process         |
|                 |                 |                 | (default:       |
|                 |                 |                 | false). See     |
|                 |                 |                 | Section 7.2.2.  |
+-----------------+-----------------+-----------------+-----------------+
| style           | string          | 0-1             | Name of a       |
| (attribute)     |                 |                 | benchmark       |
|                 |                 |                 | authoring style |
|                 |                 |                 | or set of       |
|                 |                 |                 | conventions or  |
|                 |                 |                 | constraints to  |
|                 |                 |                 | which this      |
|                 |                 |                 | benchmark       |
|                 |                 |                 | conforms (e.g., |
|                 |                 |                 | "SCAP 1.2").    |
+-----------------+-----------------+-----------------+-----------------+
| style-href      | URI             | 0-1             | URL of a        |
| (attribute)     |                 |                 | supplementary   |
|                 |                 |                 | stylesheet or   |
|                 |                 |                 | schema          |
|                 |                 |                 | extension that  |
|                 |                 |                 | can be used to  |
|                 |                 |                 | verify          |
|                 |                 |                 | conformance to  |
|                 |                 |                 | the named       |
|                 |                 |                 | style.          |
+-----------------+-----------------+-----------------+-----------------+
| xml:lang        | *special*       | 0-1             | The language    |
| (attribute)     |                 |                 | for the         |
|                 |                 |                 | benchmark; see  |
|                 |                 |                 | Section 6.2.10. |
+-----------------+-----------------+-----------------+-----------------+

The properties that comprise an *\<xccdf:Benchmark\>* or items are an
ordered sequence of property values. The order of *\<xccdf:Group\>* and
*\<xccdf:Rule\>* child elements matters for the appearance of a
generated document, so *\<xccdf:Group\>* and *\<xccdf:Rule\>* child
elements may be freely intermingled. All other child elements must
appear in the order shown in Table 4, and multiple instances of a child
element must be adjacent.

## Item Elements

### Properties

Table 5 describes the properties common to all three classes of items:
*\<xccdf:Group\>*, *\<xccdf:Rule\>*, and *\<xccdf:Value\>* elements.

[]{#_Ref294716810 .anchor}Table : Item Element Properties

+-----------------+-----------------+-----------------+-----------------+
| Property        | Type            | Count           | Description     |
+=================+=================+=================+=================+
| status          | *special*       | 0-n             | Status of the   |
| (element)       |                 |                 | item and date   |
|                 |                 |                 | at which it     |
|                 |                 |                 | attained that   |
|                 |                 |                 | status.         |
|                 |                 |                 | Benchmark       |
|                 |                 |                 | authors may use |
|                 |                 |                 | this element to |
|                 |                 |                 | record the      |
|                 |                 |                 | maturity or     |
|                 |                 |                 | consensus level |
|                 |                 |                 | for item        |
|                 |                 |                 | elements in the |
|                 |                 |                 | benchmark. If   |
|                 |                 |                 | an item does    |
|                 |                 |                 | not have an     |
|                 |                 |                 | explicit status |
|                 |                 |                 | given, then its |
|                 |                 |                 | status shall be |
|                 |                 |                 | that of its     |
|                 |                 |                 | parent. See     |
|                 |                 |                 | Section 6.2.8.  |
+-----------------+-----------------+-----------------+-----------------+
| dc-status       | *special*       | 0-n             | Holds           |
| (element)       |                 |                 | additional      |
|                 |                 |                 | status          |
|                 |                 |                 | information     |
|                 |                 |                 | using the       |
|                 |                 |                 | Dublin Core     |
|                 |                 |                 | format. See     |
|                 |                 |                 | Section 6.2.8.  |
+-----------------+-----------------+-----------------+-----------------+
| version         | string          | 0-1             | Version number  |
| (element)       |                 |                 | of the item,    |
|                 |                 |                 | with an         |
|                 |                 |                 | optional        |
|                 |                 |                 | *\@time*        |
|                 |                 |                 | timestamp       |
|                 |                 |                 | attribute (when |
|                 |                 |                 | the version was |
|                 |                 |                 | completed) and  |
|                 |                 |                 | an optional     |
|                 |                 |                 | *\@update* URI  |
|                 |                 |                 | attribute       |
|                 |                 |                 | (where updates  |
|                 |                 |                 | may be          |
|                 |                 |                 | obtained).      |
+-----------------+-----------------+-----------------+-----------------+
| title (element) | string          | 0-n             | Title of the    |
|                 |                 |                 | item. Every     |
|                 |                 |                 | item should     |
|                 |                 |                 | have a title,   |
|                 |                 |                 | because this    |
|                 |                 |                 | helps people    |
|                 |                 |                 | understand the  |
|                 |                 |                 | purpose of the  |
|                 |                 |                 | item. It may    |
|                 |                 |                 | have one or     |
|                 |                 |                 | more            |
|                 |                 |                 | *\<xccdf:sub\>* |
|                 |                 |                 | elements (see   |
|                 |                 |                 | Section 6.2.9), |
|                 |                 |                 | an *\@xml:lang* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10), and/or |
|                 |                 |                 | an *\@override* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1). |
+-----------------+-----------------+-----------------+-----------------+
| description     | HTML-enabled    | 0-n             | Text that       |
| (element)       | text            |                 | describes the   |
|                 |                 |                 | item. It MAY    |
|                 |                 |                 | have one or     |
|                 |                 |                 | more            |
|                 |                 |                 | *\<xccdf:sub\>* |
|                 |                 |                 | elements (see   |
|                 |                 |                 | Section 6.2.9), |
|                 |                 |                 | an *\@override* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1), |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10).        |
+-----------------+-----------------+-----------------+-----------------+
| warning         | HTML-enabled    | 0-n             | A note or       |
| (element)       | text            |                 | caveat about    |
|                 |                 |                 | the item        |
|                 |                 |                 | intended to     |
|                 |                 |                 | convey          |
|                 |                 |                 | important       |
|                 |                 |                 | cautionary      |
|                 |                 |                 | information for |
|                 |                 |                 | the benchmark   |
|                 |                 |                 | user (e.g.,     |
|                 |                 |                 | "Complying with |
|                 |                 |                 | this rule will  |
|                 |                 |                 | cause the       |
|                 |                 |                 | system to       |
|                 |                 |                 | reject all IP   |
|                 |                 |                 | packets"). If   |
|                 |                 |                 | multiple        |
|                 |                 |                 | warning         |
|                 |                 |                 | elements        |
|                 |                 |                 | appear,         |
|                 |                 |                 | benchmark       |
|                 |                 |                 | consumers       |
|                 |                 |                 | should          |
|                 |                 |                 | concatenate     |
|                 |                 |                 | them for        |
|                 |                 |                 | generating      |
|                 |                 |                 | reports or      |
|                 |                 |                 | documents.      |
|                 |                 |                 | Benchmark       |
|                 |                 |                 | consumers may   |
|                 |                 |                 | present this    |
|                 |                 |                 | information in  |
|                 |                 |                 | a special       |
|                 |                 |                 | manner in       |
|                 |                 |                 | generated       |
|                 |                 |                 | documents.      |
|                 |                 |                 |                 |
|                 |                 |                 | It MAY have one |
|                 |                 |                 | or more         |
|                 |                 |                 | *\<xccdf:sub\>* |
|                 |                 |                 | elements (see   |
|                 |                 |                 | Section 6.2.9), |
|                 |                 |                 | an *\@override* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1), |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10). Also,  |
|                 |                 |                 | see Section     |
|                 |                 |                 | 6.4.2 for the   |
|                 |                 |                 | possible values |
|                 |                 |                 | of the warning  |
|                 |                 |                 | element's       |
|                 |                 |                 | optional        |
|                 |                 |                 | *\@category*    |
|                 |                 |                 | attribute,      |
|                 |                 |                 | which provides  |
|                 |                 |                 | a hint as to    |
|                 |                 |                 | the nature of   |
|                 |                 |                 | the warning.    |
+-----------------+-----------------+-----------------+-----------------+
| question        | string          | 0-n             | Interrogative   |
| (element)       |                 |                 | text to present |
|                 |                 |                 | to the user     |
|                 |                 |                 | during          |
|                 |                 |                 | tailoring. It   |
|                 |                 |                 | may also be     |
|                 |                 |                 | included into a |
|                 |                 |                 | generated       |
|                 |                 |                 | document. For   |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | and             |
|                 |                 |                 | *\<xccdf:Group\ |
|                 |                 |                 | >*              |
|                 |                 |                 | elements, the   |
|                 |                 |                 | question text   |
|                 |                 |                 | should be a     |
|                 |                 |                 | simple binary   |
|                 |                 |                 | (yes/no)        |
|                 |                 |                 | question        |
|                 |                 |                 | because it is   |
|                 |                 |                 | supporting the  |
|                 |                 |                 | selection       |
|                 |                 |                 | aspect of       |
|                 |                 |                 | tailoring. For  |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | elements, the   |
|                 |                 |                 | question should |
|                 |                 |                 | solicit the     |
|                 |                 |                 | user to provide |
|                 |                 |                 | a specific      |
|                 |                 |                 | value. Tools    |
|                 |                 |                 | MAY also        |
|                 |                 |                 | display         |
|                 |                 |                 | constraints on  |
|                 |                 |                 | values and any  |
|                 |                 |                 | defaults as     |
|                 |                 |                 | specified by    |
|                 |                 |                 | the other       |
|                 |                 |                 | \<xccdf:Value\> |
|                 |                 |                 | properties (see |
|                 |                 |                 | Section 6.4.5). |
|                 |                 |                 | It MAY have an  |
|                 |                 |                 | *\@override*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1)  |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10).        |
+-----------------+-----------------+-----------------+-----------------+
| reference       | *special*       | 0-n             | References      |
| (element)       |                 |                 | where the user  |
|                 |                 |                 | can learn more  |
|                 |                 |                 | about the       |
|                 |                 |                 | subject of this |
|                 |                 |                 | item. See       |
|                 |                 |                 | Section 6.2.6.  |
+-----------------+-----------------+-----------------+-----------------+
| metadata        | *special*       | 0-n             | XML metadata    |
| (element)       |                 |                 | associated with |
|                 |                 |                 | this item, such |
|                 |                 |                 | as sources,     |
|                 |                 |                 | special         |
|                 |                 |                 | information, or |
|                 |                 |                 | other details.  |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.4.          |
+-----------------+-----------------+-----------------+-----------------+
| abstract        | boolean         | 0-1             | If true, then   |
| (attribute)     |                 |                 | this item is    |
|                 |                 |                 | abstract and    |
|                 |                 |                 | exists only to  |
|                 |                 |                 | be extended     |
|                 |                 |                 | (default:       |
|                 |                 |                 | false). ***The  |
|                 |                 |                 | use of this     |
|                 |                 |                 | attribute for   |
|                 |                 |                 | \<xccdf:Group\> |
|                 |                 |                 | elements is     |
|                 |                 |                 | deprecated and  |
|                 |                 |                 | should be       |
|                 |                 |                 | avoided (see*** |
|                 |                 |                 | **Table         |
|                 |                 |                 | 42*).***        |
+-----------------+-----------------+-----------------+-----------------+
| cluster-id      | identifier      | 0-1             | An identifier   |
| (attribute)     |                 |                 | to be used as a |
|                 |                 |                 | means to        |
|                 |                 |                 | identify (refer |
|                 |                 |                 | to) related     |
|                 |                 |                 | *\<xccdf:Group\ |
|                 |                 |                 | >*,             |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *,              |
|                 |                 |                 | and             |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | elements        |
|                 |                 |                 | throughout the  |
|                 |                 |                 | *\<xccdf:Benchm |
|                 |                 |                 | ark\>.*         |
|                 |                 |                 | It designates   |
|                 |                 |                 | membership in a |
|                 |                 |                 | cluster of      |
|                 |                 |                 | items, which    |
|                 |                 |                 | are used for    |
|                 |                 |                 | controlling     |
|                 |                 |                 | items via       |
|                 |                 |                 | profiles. All   |
|                 |                 |                 | the items with  |
|                 |                 |                 | the same        |
|                 |                 |                 | cluster         |
|                 |                 |                 | identifier      |
|                 |                 |                 | belong to the   |
|                 |                 |                 | same cluster. A |
|                 |                 |                 | selector in an  |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*            |
|                 |                 |                 | may refer to a  |
|                 |                 |                 | cluster, thus   |
|                 |                 |                 | making it       |
|                 |                 |                 | easier for      |
|                 |                 |                 | authors to      |
|                 |                 |                 | create and      |
|                 |                 |                 | maintain        |
|                 |                 |                 | profiles in a   |
|                 |                 |                 | complex         |
|                 |                 |                 | benchmark.      |
+-----------------+-----------------+-----------------+-----------------+
| extends         | identifier      | 0-1             | The identifier  |
| (attribute)     |                 |                 | of an item on   |
|                 |                 |                 | which to base   |
|                 |                 |                 | this item. If   |
|                 |                 |                 | present, it     |
|                 |                 |                 | MUST have a     |
|                 |                 |                 | value equal to  |
|                 |                 |                 | the *\@id*      |
|                 |                 |                 | attribute of    |
|                 |                 |                 | another item.   |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.3.1 and       |
|                 |                 |                 | Section 7.2.2.  |
|                 |                 |                 | ***The use of   |
|                 |                 |                 | this attribute  |
|                 |                 |                 | for             |
|                 |                 |                 | \<xccdf:Group\> |
|                 |                 |                 | elements is     |
|                 |                 |                 | deprecated and  |
|                 |                 |                 | should be       |
|                 |                 |                 | avoided (see*** |
|                 |                 |                 | **Table         |
|                 |                 |                 | 42*).***        |
+-----------------+-----------------+-----------------+-----------------+
| hidden          | boolean         | 0-1             | If this item    |
| (attribute)     |                 |                 | should be       |
|                 |                 |                 | excluded from   |
|                 |                 |                 | any generated   |
|                 |                 |                 | documents       |
|                 |                 |                 | (default:       |
|                 |                 |                 | false). For     |
|                 |                 |                 | example, an     |
|                 |                 |                 | author might    |
|                 |                 |                 | set the         |
|                 |                 |                 | *\@hidden*      |
|                 |                 |                 | attribute on an |
|                 |                 |                 | incomplete item |
|                 |                 |                 | in a draft      |
|                 |                 |                 | benchmark. The  |
|                 |                 |                 | item may still  |
|                 |                 |                 | be used during  |
|                 |                 |                 | assessments,    |
|                 |                 |                 | but it shall    |
|                 |                 |                 | not appear in   |
|                 |                 |                 | generated       |
|                 |                 |                 | documents.      |
+-----------------+-----------------+-----------------+-----------------+
| prohibitChanges | boolean         | 0-1             | If benchmark    |
| (attribute)     |                 |                 | producers       |
|                 |                 |                 | should prohibit |
|                 |                 |                 | changes to this |
|                 |                 |                 | item during     |
|                 |                 |                 | tailoring       |
|                 |                 |                 | (default:       |
|                 |                 |                 | false). An      |
|                 |                 |                 | author should   |
|                 |                 |                 | use this when   |
|                 |                 |                 | they do not     |
|                 |                 |                 | want to allow   |
|                 |                 |                 | end users to    |
|                 |                 |                 | change the      |
|                 |                 |                 | item. For       |
|                 |                 |                 | example,        |
|                 |                 |                 | benchmark users |
|                 |                 |                 | may use this to |
|                 |                 |                 | define items    |
|                 |                 |                 | that are        |
|                 |                 |                 | integral to     |
|                 |                 |                 | compliance, or  |
|                 |                 |                 | enterprise      |
|                 |                 |                 | security        |
|                 |                 |                 | officers may    |
|                 |                 |                 | use it to       |
|                 |                 |                 | constrain a     |
|                 |                 |                 | benchmark so it |
|                 |                 |                 | reflects        |
|                 |                 |                 | organizational  |
|                 |                 |                 | policies.       |
+-----------------+-----------------+-----------------+-----------------+
| xml:lang        | *special*       | 0-1             | The language    |
| (attribute)     |                 |                 | for the item;   |
|                 |                 |                 | see Section     |
|                 |                 |                 | 6.2.10.         |
+-----------------+-----------------+-----------------+-----------------+
| xml:base        | *special*       | 0-1             | The context for |
| (attribute)     |                 |                 | all relative    |
|                 |                 |                 | URIs within the |
|                 |                 |                 | item.           |
+-----------------+-----------------+-----------------+-----------------+
| Id (attribute)  | *special*       | 0-1             | An identifier   |
|                 |                 |                 | used for        |
|                 |                 |                 | referencing     |
|                 |                 |                 | elements        |
|                 |                 |                 | included in an  |
|                 |                 |                 | XML signature.  |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.7.          |
+-----------------+-----------------+-----------------+-----------------+

In addition to the properties listed in Table 5, *\<xccdf:Group\>* and
*\<xccdf:Rule\>* elements also support the properties listed in Table 6.
See Sections 6.4.3, 6.4.4, and 6.4.5 for information on additional
properties specific to *\<xccdf:Group\>*, *\<xccdf:Rule\>*, and
*\<xccdf:Value\>* elements, respectively.

[]{#_Ref295830814 .anchor}Table : Properties Specific to \<xccdf:Group\>
and \<xccdf:Rule\> Elements

  Property               Type                Count   Description
  ---------------------- ------------------- ------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  rationale (element)    HTML-enabled text   0-n     Descriptive text giving rationale or motivations for abiding by this group/rule (i.e., why it is important to the security of the target platform). It MAY have one or more *\<xccdf:sub\>* elements (see Section 6.2.9), an *\@override* attribute (see Section 6.3.1), and/or an *\@xml:lang* attribute (see Section 6.2.10).
  platform (element)     *special*           0-n     Platforms to which this group/rule applies. See Section 6.2.5.
  requires (element)     *special*           0-n     The identifiers of other *\<xccdf:Group\>* or *\<xccdf:Rule\>* elements in the *\<xccdf:Benchmark\>* that must be selected for this group/rule to be evaluated and scored properly. Each *\<xccdf:requires\>* element specifies a list of one or more required items by their identifiers, using the *\@idref* attribute. If at least one of the specified *\<xccdf:Group\>* or *\<xccdf:Rule\>* elements is selected, the requirement is met. An *\<xccdf:requires\>* element that looked like *\<xccdf:requires idref=\"A B C\"\>* could be read as \"requires that item A or item B or item C be selected\".
  conflicts (element)    identifier          0-n     The identifier of another *\<xccdf:Group\>* or *\<xccdf:Rule\>* in the *\<xccdf:Benchmark\>* that must be unselected for this group/rule to be evaluated and scored properly. Each *\<xccdf:conflicts\>* element specifies a single conflicting item using its *\@idref* attribute. If the specified *\<xccdf:Group\>* or *\<xccdf:Rule\>* element is not selected, the requirement is met.
  selected (attribute)   boolean             0-1     If true, this group/rule shall be selected to be processed as part of the *\<xccdf:Benchmark\>* when it is applied to a target system (default: true). An unselected group shall not be processed, and its contents shall NOT be processed either (i.e., all descendants of an unselected group are implicitly unselected). An unselected rule shall not be checked and shall not contribute to scoring. May be overridden by a profile; see Section 6.5.3.
  weight (attribute)     decimal             0-1     The relative scoring weight of this group/rule, for computing a score, expressed as a non-negative real number (0.0 or greater, maximum 3 digits, default 1.0). It denotes the importance of a group/rule. Under some scoring models, scoring is computed independently for each collection of sibling groups and rules, then normalized as part of the overall scoring process. See Section 7.3.2 for more information on scoring. May be overridden by a profile; see Section 6.5.3.

### \<xccdf:warning\> Element

If the *\<xccdf:warning\>* element's *\@category* attribute is used, it
must have one of the values specified in Table 7.

[]{#_Ref294716855 .anchor}Table : \<xccdf:warning\> Element \@category
Attribute Values

  --------------- ---------------------------------------------------------------------------------------------------------------
  Value           Meaning
  general         Broad or general-purpose warning (default)
  functionality   Warning about possible impacts to functionality or operational features
  performance     Warning about changes to target system performance or throughput
  hardware        Warning about hardware restrictions or possible impacts to hardware
  legal           Warning about legal implications
  regulatory      Warning about regulatory obligations or compliance implications
  management      Warning about impacts to the management or administration of the target system
  audit           Warning about impacts to audit or logging
  dependency      Warning about dependencies between this element and other parts of the target system, or version dependencies
  --------------- ---------------------------------------------------------------------------------------------------------------

### \<xccdf:Group\> Element

An *\<xccdf:Group\>* element contains descriptive information about a
portion of a benchmark, as well as *\<xccdf:Rule\>*, *\<xccdf:Value\>*,
and/or other *\<xccdf:Group\>* elements. Table 8 describes the
*\<xccdf:Group\>* element's properties (in addition to the properties in
Table 5 and Table 6).

[]{#_Ref294716878 .anchor}Table : \<xccdf:Group\> Element Properties

  Property              Type        Count   Description
  --------------------- ----------- ------- ----------------------------------------------------------------------------------------------------------------------
  Value (element)       Value       0-n     Values that belong to this group; see Section 6.4.5.
  Group (element)       Group       0-n     Sub-groups under this group; see Section 6.4.3.
  Rule (element)        Rule                Rules that belong to this group; see Section 6.4.4.
  signature (element)   *special*   0-1     A digital signature asserting authorship and allowing verification of the integrity of the group. See Section 6.2.7.
  id (attribute)        *special*   1       Unique element identifier; SHALL be used by other elements to refer to this element. See Section 6.2.3.

### \<xccdf:Rule\> Element

#### Basics

An *\<xccdf:Rule\>* element defines a single item to be checked as part
of a benchmark, or an extendable base definition for such items. The
*\<xccdf:check\>* child element of an *\<xccdf:Rule\>* specifies how to
verify compliance with a security practice or guideline. See Table 9 and
Section 6.4.4.4 for more information on the *\<xccdf:check\>* element.

The example below shows a very simple *\<xccdf:Rule\>* element.

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

One of XCCDF's main features is the organization and selection of
target-applicable groups and rules for performing security and
operational checks on systems. XCCDF can access granular and expressive
mechanisms for assessing the state of a system according to the rule
criteria. Examples of these mechanisms are definitions expressed in the
Open Vulnerability and Assessment Language (OVAL) and questionnaires
expressed in the Open Checklist Interactive Language (OCIL). These
checking mechanisms follow the conceptual model of collecting or
acquiring the state of a target system, and then assessing the state for
conformance to conditions and criteria expressed as rules.

XCCDF may use rule checking systems other than (or in addition to) OVAL
and OCIL. To facilitate this, XCCDF supports referencing by including
the appropriate check content location and check reference in the
*\<xccdf:check\>* element. Rule checking systems are defined separately
from XCCDF itself so that both XCCDF and the rule checking system can
evolve and be used independently. It is helpful for rule checking
mechanisms used by XCCDF to comply with the following:

-   **The mechanism can express both positive and negative criteria.** A
    > positive criterion means that if certain conditions are met, then
    > the system satisfies the check, while a negative criterion means
    > that if the conditions are met, the system fails the check.
    > Experience has shown that both kinds are necessary for checks.

-   **The mechanism can express Boolean combinations of criteria.** It
    > is often impossible to express a high-level security property as a
    > single quantitative or qualitative statement about a system's
    > state. Therefore, the ability to combine statements with 'and' and
    > 'or' is critical.

-   **The mechanism can incorporate tailoring values set by the user.**
    > Value modification is important for XCCDF document tailoring,
    > including substitution of tailored values as well as tailoring of
    > a selected set of rules.

#### Properties

Table 9 describes the *\<xccdf:Rule\>* element's properties (in addition
to the properties in Table 5 and Table 6).

[]{#_Ref294716912 .anchor}Table : \<xccdf:Rule\> Element Properties

+-----------------+-----------------+-----------------+-----------------+
| Property        | Type            | Count           | Description     |
+=================+=================+=================+=================+
| ident (element) | string          | 0-n             | A globally      |
|                 |                 |                 | meaningful      |
|                 |                 |                 | identifier for  |
|                 |                 |                 | this rule. May  |
|                 |                 |                 | be the name or  |
|                 |                 |                 | identifier of a |
|                 |                 |                 | security        |
|                 |                 |                 | configuration   |
|                 |                 |                 | issue or        |
|                 |                 |                 | vulnerability   |
|                 |                 |                 | that the rule   |
|                 |                 |                 | remediates. Has |
|                 |                 |                 | an associated   |
|                 |                 |                 | URI that shall  |
|                 |                 |                 | denote the      |
|                 |                 |                 | organization or |
|                 |                 |                 | naming scheme   |
|                 |                 |                 | that assigned   |
|                 |                 |                 | the name (see   |
|                 |                 |                 | Table 10 for    |
|                 |                 |                 | assigned URIs). |
|                 |                 |                 | By setting an   |
|                 |                 |                 | *\<xccdf:ident\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element on a    |
|                 |                 |                 | rule, the       |
|                 |                 |                 | benchmark       |
|                 |                 |                 | author          |
|                 |                 |                 | effectively     |
|                 |                 |                 | declares that   |
|                 |                 |                 | the rule        |
|                 |                 |                 | instantiates,   |
|                 |                 |                 | implements, or  |
|                 |                 |                 | remediates the  |
|                 |                 |                 | issue for which |
|                 |                 |                 | the name was    |
|                 |                 |                 | assigned. For   |
|                 |                 |                 | example, the    |
|                 |                 |                 | *\<xccdf:ident\ |
|                 |                 |                 | >*              |
|                 |                 |                 | value might be  |
|                 |                 |                 | a CVE           |
|                 |                 |                 | identifier; the |
|                 |                 |                 | rule could be a |
|                 |                 |                 | check that the  |
|                 |                 |                 | target platform |
|                 |                 |                 | was not subject |
|                 |                 |                 | to the          |
|                 |                 |                 | vulnerability   |
|                 |                 |                 | named by the    |
|                 |                 |                 | CVE identifier, |
|                 |                 |                 | and the URI     |
|                 |                 |                 | would be that   |
|                 |                 |                 | of the CVE      |
|                 |                 |                 | website. An     |
|                 |                 |                 | *\<xccdf:ident\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element MAY     |
|                 |                 |                 | also have other |
|                 |                 |                 | attributes from |
|                 |                 |                 | other           |
|                 |                 |                 | namespaces in   |
|                 |                 |                 | order to        |
|                 |                 |                 | provide         |
|                 |                 |                 | additional      |
|                 |                 |                 | metadata for    |
|                 |                 |                 | the given       |
|                 |                 |                 | identifier.     |
+-----------------+-----------------+-----------------+-----------------+
| impact-metric   | string          | 0-1             | The potential   |
| (element)       |                 |                 | impact of       |
|                 |                 |                 | failure to      |
|                 |                 |                 | conform to the  |
|                 |                 |                 | rule, expressed |
|                 |                 |                 | as a CVSS 2.0   |
|                 |                 |                 | base vector     |
|                 |                 |                 | (defined by     |
|                 |                 |                 | NIST IR 7435).  |
|                 |                 |                 | ***The property |
|                 |                 |                 | is deprecated   |
|                 |                 |                 | and its use     |
|                 |                 |                 | should be       |
|                 |                 |                 | avoided (see*** |
|                 |                 |                 | **Table 42*).   |
|                 |                 |                 | ***A suggested  |
|                 |                 |                 | alternate       |
|                 |                 |                 | location for    |
|                 |                 |                 | impact metric   |
|                 |                 |                 | data is the     |
|                 |                 |                 | *\<xccdf:metada |
|                 |                 |                 | ta\>*           |
|                 |                 |                 | element within  |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *.              |
+-----------------+-----------------+-----------------+-----------------+
| profile-note    | HTML-enabled    | 0-n             | Text that       |
| (element)       | text            |                 | describes       |
|                 |                 |                 | special aspects |
|                 |                 |                 | of the rule     |
|                 |                 |                 | related to one  |
|                 |                 |                 | or more         |
|                 |                 |                 | profiles. This  |
|                 |                 |                 | allows an       |
|                 |                 |                 | author to       |
|                 |                 |                 | document things |
|                 |                 |                 | within rules    |
|                 |                 |                 | that are        |
|                 |                 |                 | specific to a   |
|                 |                 |                 | given profile,  |
|                 |                 |                 | and then select |
|                 |                 |                 | the appropriate |
|                 |                 |                 | text based on   |
|                 |                 |                 | the selected    |
|                 |                 |                 | profile and     |
|                 |                 |                 | display it to   |
|                 |                 |                 | the reader. The |
|                 |                 |                 | element MUST    |
|                 |                 |                 | have a *\@tag*  |
|                 |                 |                 | attribute that  |
|                 |                 |                 | holds an        |
|                 |                 |                 | identifier; an  |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*            |
|                 |                 |                 | may refer to    |
|                 |                 |                 | this tag        |
|                 |                 |                 | through the     |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*            |
|                 |                 |                 | *\@note-tag*    |
|                 |                 |                 | attribute. The  |
|                 |                 |                 | element also    |
|                 |                 |                 | MAY have one or |
|                 |                 |                 | more            |
|                 |                 |                 | *\<xccdf:sub\>* |
|                 |                 |                 | elements (see   |
|                 |                 |                 | Section 6.2.9)  |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10).        |
+-----------------+-----------------+-----------------+-----------------+
| fixtext         | *special*       | 0-n             | Data that       |
| (element)       |                 |                 | describes how   |
|                 |                 |                 | to bring a      |
|                 |                 |                 | target system   |
|                 |                 |                 | into compliance |
|                 |                 |                 | with this rule. |
|                 |                 |                 | Each            |
|                 |                 |                 | *\<xccdf:fixtex |
|                 |                 |                 | t\>*            |
|                 |                 |                 | element may be  |
|                 |                 |                 | associated with |
|                 |                 |                 | one or more     |
|                 |                 |                 | *\<xccdf:fix\>* |
|                 |                 |                 | element values. |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.4.4.5.        |
+-----------------+-----------------+-----------------+-----------------+
| fix (element)   | *special*       | 0-n             | A command       |
|                 |                 |                 | string, script, |
|                 |                 |                 | or other system |
|                 |                 |                 | modification    |
|                 |                 |                 | statement that, |
|                 |                 |                 | if executed on  |
|                 |                 |                 | the target      |
|                 |                 |                 | system, can     |
|                 |                 |                 | bring it into   |
|                 |                 |                 | full, or at     |
|                 |                 |                 | least better,   |
|                 |                 |                 | compliance with |
|                 |                 |                 | this rule. See  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.4.5.        |
+-----------------+-----------------+-----------------+-----------------+
| check (element) | *special*       | (1-n instances  | The definition  |
|                 |                 | of check)       | of, or a        |
|                 |                 |                 | reference to,   |
|                 |                 | XOR             | the target      |
|                 |                 |                 | system check    |
|                 |                 | (1 instance of  | needed to test  |
|                 |                 | complex-check)  | compliance with |
|                 |                 |                 | this rule.      |
|                 |                 |                 | Sibling         |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | elements must   |
|                 |                 |                 | have different  |
|                 |                 |                 | values for the  |
|                 |                 |                 | combination of  |
|                 |                 |                 | their           |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | and *\@system*  |
|                 |                 |                 | attributes, and |
|                 |                 |                 | different       |
|                 |                 |                 | values for      |
|                 |                 |                 | their *\@id*    |
|                 |                 |                 | attribute (if   |
|                 |                 |                 | any). See       |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.4.4.        |
+-----------------+-----------------+-----------------+-----------------+
| complex-check   | *special*       |                 | A boolean       |
| (element)       |                 |                 | expression      |
|                 |                 |                 | composed of     |
|                 |                 |                 | operators (and, |
|                 |                 |                 | or, not) and    |
|                 |                 |                 | individual      |
|                 |                 |                 | checks. See     |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.4.4.        |
+-----------------+-----------------+-----------------+-----------------+
| signature       | *special*       | 0-1             | A digital       |
| (element)       |                 |                 | signature       |
|                 |                 |                 | asserting       |
|                 |                 |                 | authorship and  |
|                 |                 |                 | allowing        |
|                 |                 |                 | verification of |
|                 |                 |                 | the integrity   |
|                 |                 |                 | of the rule.    |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.7.          |
+-----------------+-----------------+-----------------+-----------------+
| role            | string          | 0-1             | The rule's role |
| (attribute)     |                 |                 | in scoring and  |
|                 |                 |                 | reporting. It   |
|                 |                 |                 | May be one of   |
|                 |                 |                 | the following:  |
|                 |                 |                 |                 |
|                 |                 |                 | -   "full" (if  |
|                 |                 |                 |     > the rule  |
|                 |                 |                 |     > is        |
|                 |                 |                 |     > selected, |
|                 |                 |                 |     > then      |
|                 |                 |                 |     > check it  |
|                 |                 |                 |     > and let   |
|                 |                 |                 |     > the       |
|                 |                 |                 |     > result    |
|                 |                 |                 |     > contribut |
|                 |                 |                 | e               |
|                 |                 |                 |     > to the    |
|                 |                 |                 |     > score and |
|                 |                 |                 |     > appear in |
|                 |                 |                 |     > reports)  |
|                 |                 |                 |     > (default) |
|                 |                 |                 |                 |
|                 |                 |                 | -   "unscored"  |
|                 |                 |                 |     > (if the   |
|                 |                 |                 |     > rule is   |
|                 |                 |                 |     > selected, |
|                 |                 |                 |     > then      |
|                 |                 |                 |     > check it  |
|                 |                 |                 |     > and       |
|                 |                 |                 |     > include   |
|                 |                 |                 |     > the       |
|                 |                 |                 |     > results   |
|                 |                 |                 |     > in any    |
|                 |                 |                 |     > report,   |
|                 |                 |                 |     > but do    |
|                 |                 |                 |     > not       |
|                 |                 |                 |     > include   |
|                 |                 |                 |     > the       |
|                 |                 |                 |     > result in |
|                 |                 |                 |     > score     |
|                 |                 |                 |     > computati |
|                 |                 |                 | ons)            |
|                 |                 |                 |                 |
|                 |                 |                 | -   "unchecked" |
|                 |                 |                 |     > (if the   |
|                 |                 |                 |     > rule is   |
|                 |                 |                 |     > selected, |
|                 |                 |                 |     > then do   |
|                 |                 |                 |     > not check |
|                 |                 |                 |     > it; just  |
|                 |                 |                 |     > force the |
|                 |                 |                 |     > result    |
|                 |                 |                 |     > status to |
|                 |                 |                 |     > "notcheck |
|                 |                 |                 | ed")            |
|                 |                 |                 |                 |
|                 |                 |                 | May be          |
|                 |                 |                 | overridden by a |
|                 |                 |                 | profile.        |
+-----------------+-----------------+-----------------+-----------------+
| id (attribute)  | *special*       | 1               | Unique element  |
|                 |                 |                 | identifier;     |
|                 |                 |                 | SHALL be used   |
|                 |                 |                 | by other        |
|                 |                 |                 | elements to     |
|                 |                 |                 | refer to this   |
|                 |                 |                 | element. See    |
|                 |                 |                 | Section 6.2.3.  |
+-----------------+-----------------+-----------------+-----------------+
| severity        | string          | 0-1             | Severity level  |
| (attribute)     |                 |                 | code to be used |
|                 |                 |                 | for metrics and |
|                 |                 |                 | tracking. When  |
|                 |                 |                 | used, it shall  |
|                 |                 |                 | have one of the |
|                 |                 |                 | following       |
|                 |                 |                 | values:         |
|                 |                 |                 |                 |
|                 |                 |                 | -   "unknown"   |
|                 |                 |                 |     > (severity |
|                 |                 |                 |     > not       |
|                 |                 |                 |     > defined)  |
|                 |                 |                 |     > (default) |
|                 |                 |                 |                 |
|                 |                 |                 | -   "info"      |
|                 |                 |                 |     > (rule is  |
|                 |                 |                 |     > informati |
|                 |                 |                 | onal            |
|                 |                 |                 |     > only;     |
|                 |                 |                 |     > failing   |
|                 |                 |                 |     > the rule  |
|                 |                 |                 |     > does not  |
|                 |                 |                 |     > imply     |
|                 |                 |                 |     > failure   |
|                 |                 |                 |     > to        |
|                 |                 |                 |     > conform   |
|                 |                 |                 |     > to the    |
|                 |                 |                 |     > security  |
|                 |                 |                 |     > guidance  |
|                 |                 |                 |     > of the    |
|                 |                 |                 |     > benchmark |
|                 |                 |                 | )               |
|                 |                 |                 |                 |
|                 |                 |                 | -   "low" (not  |
|                 |                 |                 |     > a serious |
|                 |                 |                 |     > problem)  |
|                 |                 |                 |                 |
|                 |                 |                 | -   "medium"    |
|                 |                 |                 |     > (fairly   |
|                 |                 |                 |     > serious   |
|                 |                 |                 |     > problem)  |
|                 |                 |                 |                 |
|                 |                 |                 | -   "high"      |
|                 |                 |                 |     > (grave or |
|                 |                 |                 |     > critical  |
|                 |                 |                 |     > problem)  |
|                 |                 |                 |                 |
|                 |                 |                 | May be          |
|                 |                 |                 | overridden by a |
|                 |                 |                 | profile; see    |
|                 |                 |                 | Section 6.5.3.  |
+-----------------+-----------------+-----------------+-----------------+
| multiple        | boolean         | 0-1             | Applicable in   |
| (attribute)     |                 |                 | cases where     |
|                 |                 |                 | there are       |
|                 |                 |                 | multiple        |
|                 |                 |                 | instances of a  |
|                 |                 |                 | target. For     |
|                 |                 |                 | example, a rule |
|                 |                 |                 | may provide a   |
|                 |                 |                 | recommendation  |
|                 |                 |                 | about the       |
|                 |                 |                 | configuration   |
|                 |                 |                 | of application  |
|                 |                 |                 | user accounts,  |
|                 |                 |                 | but an          |
|                 |                 |                 | application may |
|                 |                 |                 | have many user  |
|                 |                 |                 | accounts. Each  |
|                 |                 |                 | account would   |
|                 |                 |                 | be considered   |
|                 |                 |                 | an instance of  |
|                 |                 |                 | the broader     |
|                 |                 |                 | assessment      |
|                 |                 |                 | target of user  |
|                 |                 |                 | accounts. If    |
|                 |                 |                 | the             |
|                 |                 |                 | *\@multiple*    |
|                 |                 |                 | attribute is    |
|                 |                 |                 | set to true,    |
|                 |                 |                 | each instance   |
|                 |                 |                 | of the target   |
|                 |                 |                 | to which the    |
|                 |                 |                 | rule can apply  |
|                 |                 |                 | should be       |
|                 |                 |                 | tested          |
|                 |                 |                 | separately and  |
|                 |                 |                 | the results     |
|                 |                 |                 | should be       |
|                 |                 |                 | recorded        |
|                 |                 |                 | separately. If  |
|                 |                 |                 | *\@multiple* is |
|                 |                 |                 | set to false,   |
|                 |                 |                 | the test        |
|                 |                 |                 | results of such |
|                 |                 |                 | instances       |
|                 |                 |                 | should be       |
|                 |                 |                 | combined. If    |
|                 |                 |                 | the checking    |
|                 |                 |                 | system does not |
|                 |                 |                 | combine these   |
|                 |                 |                 | results         |
|                 |                 |                 | automatically,  |
|                 |                 |                 | the results of  |
|                 |                 |                 | each instance   |
|                 |                 |                 | should be ANDed |
|                 |                 |                 | together to     |
|                 |                 |                 | produce a       |
|                 |                 |                 | single result   |
|                 |                 |                 | using the AND   |
|                 |                 |                 | truth table in  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.4.4.        |
|                 |                 |                 | (default:false) |
|                 |                 |                 |                 |
|                 |                 |                 | If the          |
|                 |                 |                 | benchmark       |
|                 |                 |                 | consumer cannot |
|                 |                 |                 | perform         |
|                 |                 |                 | multiple        |
|                 |                 |                 | instantiation,  |
|                 |                 |                 | or if multiple  |
|                 |                 |                 | instantiation   |
|                 |                 |                 | of the rule is  |
|                 |                 |                 | not applicable  |
|                 |                 |                 | for the target  |
|                 |                 |                 | system, then    |
|                 |                 |                 | the benchmark   |
|                 |                 |                 | consumer may    |
|                 |                 |                 | ignore this     |
|                 |                 |                 | attribute. For  |
|                 |                 |                 | example, OVAL   |
|                 |                 |                 | checks cannot   |
|                 |                 |                 | produce         |
|                 |                 |                 | separate        |
|                 |                 |                 | results for     |
|                 |                 |                 | individual      |
|                 |                 |                 | instances of a  |
|                 |                 |                 | target.         |
|                 |                 |                 |                 |
|                 |                 |                 | See Section     |
|                 |                 |                 | 7.2.3.5.2.      |
+-----------------+-----------------+-----------------+-----------------+

#### \<xccdf:ident\> Elements

Table 10 lists assigned URIs that may appear as the value of the
*\@system* attribute of an *\<xccdf:ident\>* element. If an
identification system included in Table 10 is being specified, the
*\@system* attribute's value SHALL correspond to the appropriate URI in
the table. An author may create a new URI for an identification system
not listed in the table; this URI shall not duplicate any of the Table
10 URI values.

[]{#_Ref294716940 .anchor}Table : Assigned Values for the \@system
Attribute of an \<xccdf:ident\> Element

  **URI**                                 **Identifier Value Description**
  --------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------
  http://cce.mitre.org                    Common Configuration Enumeration (CCE) -- the identifier value MUST be a CCE version 5 number
  http://cpe.mitre.org                    CPE -- the identifier value MUST be a CPE version 2.0 or 2.3 name
  http://cve.mitre.org                    CVE -- the identifier value MUST be a CVE number
  http://www.cert.org                     CERT Coordination Center -- the identifier value should be a CERT advisory identifier (e.g., "CA-2004-02")
  http://www.kb.cert.org                  US-CERT vulnerability notes database -- the identifier value should be a vulnerability note number (e.g., "709220")
  http://www.us-cert.gov/cas/techalerts   US-CERT technical cyber security alerts -- the identifier value should be a technical cyber security alert ID (e.g., "TA05-189A")

An *\<xccdf:ident\>* element MAY also have additional attributes from
schemas other than the XCCDF schema. Individual organizations and
standards MAY associate specific interpretations of rules based on the
value of an *\<xccdf:ident\>* element and these additional attributes
are allowed in order to refine those interpretations. These additional
attributes MUST NOT alter the processing of a benchmark document as
described in Section 7, although they MAY be added to the output of
Document Generation or displayed to users during processing.

#### \<xccdf:check\> and \<xccdf:complex-check\> Elements

Table 11 shows the possible properties of the *\<xccdf:check\>* element.

[]{#_Ref294717011 .anchor}Table : \<xccdf:check\> Element Properties

+-----------------+-----------------+-----------------+-----------------+
| Property        | Type            | Count           | Description     |
+=================+=================+=================+=================+
| check-import    | *special*       | 0-n             | Identifies a    |
| (element)       |                 |                 | value to be     |
|                 |                 |                 | retrieved from  |
|                 |                 |                 | the checking    |
|                 |                 |                 | system during   |
|                 |                 |                 | testing of a    |
|                 |                 |                 | target system.  |
|                 |                 |                 | This element    |
|                 |                 |                 | must be empty.  |
|                 |                 |                 | After the       |
|                 |                 |                 | associated      |
|                 |                 |                 | check results   |
|                 |                 |                 | have been       |
|                 |                 |                 | collected, the  |
|                 |                 |                 | result          |
|                 |                 |                 | structure       |
|                 |                 |                 | returned by the |
|                 |                 |                 | checking engine |
|                 |                 |                 | is processed to |
|                 |                 |                 | collect the     |
|                 |                 |                 | named           |
|                 |                 |                 | information.    |
|                 |                 |                 | This            |
|                 |                 |                 | information is  |
|                 |                 |                 | then recorded   |
|                 |                 |                 | in the          |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | import\>*       |
|                 |                 |                 | element in the  |
|                 |                 |                 | corresponding   |
|                 |                 |                 | *\<xccdf:rule-r |
|                 |                 |                 | esult\>*.       |
|                 |                 |                 | This could be a |
|                 |                 |                 | single value or |
|                 |                 |                 | structured XML. |
|                 |                 |                 | In the latter   |
|                 |                 |                 | case, the       |
|                 |                 |                 | *\@import-xpath |
|                 |                 |                 | *               |
|                 |                 |                 | attribute can   |
|                 |                 |                 | be used to      |
|                 |                 |                 | traverse the    |
|                 |                 |                 | structured XML  |
|                 |                 |                 | and identify    |
|                 |                 |                 | specific        |
|                 |                 |                 | information of  |
|                 |                 |                 | interest to     |
|                 |                 |                 | record in the   |
|                 |                 |                 | result.         |
+-----------------+-----------------+-----------------+-----------------+
| check-export    | *special*       | 0-n             | A mapping from  |
| (element)       |                 |                 | an              |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element to a    |
|                 |                 |                 | checking system |
|                 |                 |                 | variable (i.e., |
|                 |                 |                 | external name   |
|                 |                 |                 | or id for use   |
|                 |                 |                 | by the checking |
|                 |                 |                 | system). This   |
|                 |                 |                 | supports export |
|                 |                 |                 | of tailoring    |
|                 |                 |                 | values from the |
|                 |                 |                 | XCCDF           |
|                 |                 |                 | processing      |
|                 |                 |                 | environment to  |
|                 |                 |                 | the checking    |
|                 |                 |                 | system. The     |
|                 |                 |                 | *\@value-id*    |
|                 |                 |                 | attribute of    |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | export\>*       |
|                 |                 |                 | element must    |
|                 |                 |                 | match the       |
|                 |                 |                 | *\@id*          |
|                 |                 |                 | attribute of an |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element in the  |
|                 |                 |                 | benchmark. The  |
|                 |                 |                 | interface       |
|                 |                 |                 | between the     |
|                 |                 |                 | XCCDF benchmark |
|                 |                 |                 | consumer and    |
|                 |                 |                 | the checking    |
|                 |                 |                 | system SHOULD   |
|                 |                 |                 | support, at a   |
|                 |                 |                 | minimum,        |
|                 |                 |                 | passing the     |
|                 |                 |                 | following       |
|                 |                 |                 | properties of   |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element:        |
|                 |                 |                 | *\<xccdf:value\ |
|                 |                 |                 | >*,             |
|                 |                 |                 | *\@type*, and   |
|                 |                 |                 | *\@operator*.   |
+-----------------+-----------------+-----------------+-----------------+
| check-content-r | *special*       | 0-n             | Points to code  |
| ef              |                 |                 | for a detached  |
| (element)       |                 |                 | check in        |
|                 |                 |                 | another         |
|                 |                 |                 | location that   |
|                 |                 |                 | uses the        |
|                 |                 |                 | language or     |
|                 |                 |                 | system          |
|                 |                 |                 | specified by    |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element's       |
|                 |                 |                 | *\@system*      |
|                 |                 |                 | attribute. The  |
|                 |                 |                 | *\@href*        |
|                 |                 |                 | attribute       |
|                 |                 |                 | identifies the  |
|                 |                 |                 | code location,  |
|                 |                 |                 | and the         |
|                 |                 |                 | optional        |
|                 |                 |                 | *\@name*        |
|                 |                 |                 | attribute MUST  |
|                 |                 |                 | refer to a      |
|                 |                 |                 | particular      |
|                 |                 |                 | part, element,  |
|                 |                 |                 | or component of |
|                 |                 |                 | the code. The   |
|                 |                 |                 | default         |
|                 |                 |                 | behavior of an  |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content-ref\>*  |
|                 |                 |                 | element that    |
|                 |                 |                 | does not have a |
|                 |                 |                 | *\@name*        |
|                 |                 |                 | attribute shall |
|                 |                 |                 | be to execute   |
|                 |                 |                 | all checks in   |
|                 |                 |                 | the referenced  |
|                 |                 |                 | code and AND    |
|                 |                 |                 | their results   |
|                 |                 |                 | together into a |
|                 |                 |                 | single          |
|                 |                 |                 | *\<xccdf:rule-r |
|                 |                 |                 | esult\>.*       |
|                 |                 |                 | However, if the |
|                 |                 |                 | *\@multi-check* |
|                 |                 |                 | attribute       |
|                 |                 |                 | (below) is set  |
|                 |                 |                 | to true and a   |
|                 |                 |                 | nameless        |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content-ref\>*  |
|                 |                 |                 | is used, each   |
|                 |                 |                 | check in the    |
|                 |                 |                 | targeted code   |
|                 |                 |                 | is reported as  |
|                 |                 |                 | a separate      |
|                 |                 |                 | *\<xccdf:rule-r |
|                 |                 |                 | esult\>.*       |
|                 |                 |                 |                 |
|                 |                 |                 | If multiple     |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content-ref\>*  |
|                 |                 |                 | elements        |
|                 |                 |                 | appear, they    |
|                 |                 |                 | represent       |
|                 |                 |                 | alternative     |
|                 |                 |                 | locations from  |
|                 |                 |                 | which a         |
|                 |                 |                 | benchmark       |
|                 |                 |                 | consumer may    |
|                 |                 |                 | obtain the      |
|                 |                 |                 | check content.  |
|                 |                 |                 | Benchmark       |
|                 |                 |                 | consumers       |
|                 |                 |                 | should process  |
|                 |                 |                 | the             |
|                 |                 |                 | alternatives in |
|                 |                 |                 | the order in    |
|                 |                 |                 | which they      |
|                 |                 |                 | appear in the   |
|                 |                 |                 | XML. The first  |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content-ref\>*  |
|                 |                 |                 | from which      |
|                 |                 |                 | content can be  |
|                 |                 |                 | successfully    |
|                 |                 |                 | retrieved       |
|                 |                 |                 | should be used. |
|                 |                 |                 | (Note that      |
|                 |                 |                 | ensuring the    |
|                 |                 |                 | validity of     |
|                 |                 |                 | this content is |
|                 |                 |                 | not the         |
|                 |                 |                 | responsibility  |
|                 |                 |                 | of a benchmark  |
|                 |                 |                 | consumer.)      |
|                 |                 |                 |                 |
|                 |                 |                 | If both         |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content-ref\>*  |
|                 |                 |                 | and             |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content\>*      |
|                 |                 |                 | elements appear |
|                 |                 |                 | in a single     |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element,        |
|                 |                 |                 | benchmark       |
|                 |                 |                 | consumers       |
|                 |                 |                 | should use the  |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content\>*      |
|                 |                 |                 | element only if |
|                 |                 |                 | none of the     |
|                 |                 |                 | references can  |
|                 |                 |                 | be resolved to  |
|                 |                 |                 | provide         |
|                 |                 |                 | content.        |
+-----------------+-----------------+-----------------+-----------------+
| check-content   | *special*       | 0-1             | Holds the       |
| (element)       |                 |                 | actual code of  |
|                 |                 |                 | a check, in the |
|                 |                 |                 | language or     |
|                 |                 |                 | system          |
|                 |                 |                 | specified by    |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element's       |
|                 |                 |                 | *\@system*      |
|                 |                 |                 | attribute. The  |
|                 |                 |                 | body of this    |
|                 |                 |                 | element may be  |
|                 |                 |                 | any XML, but    |
|                 |                 |                 | shall not       |
|                 |                 |                 | contain any     |
|                 |                 |                 | XCCDF elements. |
|                 |                 |                 | It is optional  |
|                 |                 |                 | for benchmark   |
|                 |                 |                 | consumers to    |
|                 |                 |                 | process this    |
|                 |                 |                 | element;        |
|                 |                 |                 | typically it    |
|                 |                 |                 | will be passed  |
|                 |                 |                 | to a checking   |
|                 |                 |                 | system or       |
|                 |                 |                 | engine.         |
|                 |                 |                 |                 |
|                 |                 |                 | If both         |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content-ref\>*  |
|                 |                 |                 | and             |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content\>*      |
|                 |                 |                 | elements appear |
|                 |                 |                 | in a single     |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element,        |
|                 |                 |                 | benchmark       |
|                 |                 |                 | consumers       |
|                 |                 |                 | should use the  |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content\>*      |
|                 |                 |                 | element only if |
|                 |                 |                 | none of the     |
|                 |                 |                 | references can  |
|                 |                 |                 | be resolved to  |
|                 |                 |                 | provide         |
|                 |                 |                 | content.        |
+-----------------+-----------------+-----------------+-----------------+
| system          | URI             | 1               | The URI for a   |
| (attribute)     |                 |                 | checking        |
|                 |                 |                 | system. The URI |
|                 |                 |                 | shall be        |
|                 |                 |                 | compliant with  |
|                 |                 |                 | the appropriate |
|                 |                 |                 | checking system |
|                 |                 |                 | specification   |
|                 |                 |                 | (e.g., OVAL,    |
|                 |                 |                 | OCIL). If the   |
|                 |                 |                 | checking system |
|                 |                 |                 | uses XML        |
|                 |                 |                 | namespaces,     |
|                 |                 |                 | then the        |
|                 |                 |                 | *\@system*      |
|                 |                 |                 | attribute for   |
|                 |                 |                 | the system      |
|                 |                 |                 | should be its   |
|                 |                 |                 | namespace.      |
+-----------------+-----------------+-----------------+-----------------+
| negate          | boolean         | 0-1             | If set to true, |
| (attribute)     |                 |                 | the final       |
|                 |                 |                 | result of the   |
|                 |                 |                 | check is        |
|                 |                 |                 | negated         |
|                 |                 |                 | according to    |
|                 |                 |                 | the truth table |
|                 |                 |                 | in Table 14.    |
|                 |                 |                 | (default:       |
|                 |                 |                 | false)          |
+-----------------+-----------------+-----------------+-----------------+
| id (attribute)  | identifier      | 0-1             | Unique          |
|                 |                 |                 | identifier for  |
|                 |                 |                 | this element.   |
+-----------------+-----------------+-----------------+-----------------+
| selector        | string          | 0-1             | This may be     |
| (attribute)     |                 |                 | referenced from |
|                 |                 |                 | a profile or    |
|                 |                 |                 | used during     |
|                 |                 |                 | manual          |
|                 |                 |                 | tailoring to    |
|                 |                 |                 | refine the      |
|                 |                 |                 | application of  |
|                 |                 |                 | the rule. If no |
|                 |                 |                 | selectors are   |
|                 |                 |                 | specified for a |
|                 |                 |                 | given           |
|                 |                 |                 | *\<xccdf:rule\> |
|                 |                 |                 | *,              |
|                 |                 |                 | all             |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | elements with   |
|                 |                 |                 | non-empty       |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attributes      |
|                 |                 |                 | shall be        |
|                 |                 |                 | ignored. If a   |
|                 |                 |                 | rule has        |
|                 |                 |                 | multiple        |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | elements with   |
|                 |                 |                 | the same        |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attribute, each |
|                 |                 |                 | must employ a   |
|                 |                 |                 | different       |
|                 |                 |                 | checking        |
|                 |                 |                 | system, as      |
|                 |                 |                 | identified by   |
|                 |                 |                 | the *\@system*  |
|                 |                 |                 | attribute of    |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element.        |
+-----------------+-----------------+-----------------+-----------------+
| multi-check     | boolean         | 0-1             | Applicable in   |
| (attribute)     |                 |                 | cases where     |
|                 |                 |                 | multiple checks |
|                 |                 |                 | are executed to |
|                 |                 |                 | determine       |
|                 |                 |                 | compliance with |
|                 |                 |                 | a single rule.  |
|                 |                 |                 | This situation  |
|                 |                 |                 | can arise when  |
|                 |                 |                 | a check         |
|                 |                 |                 | includes an     |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content-ref\>*  |
|                 |                 |                 | element that    |
|                 |                 |                 | does not        |
|                 |                 |                 | include a       |
|                 |                 |                 | *\@name*        |
|                 |                 |                 | attribute. The  |
|                 |                 |                 | default         |
|                 |                 |                 | behavior of a   |
|                 |                 |                 | nameless        |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content-ref\>*  |
|                 |                 |                 | shall be to     |
|                 |                 |                 | execute all     |
|                 |                 |                 | checks in the   |
|                 |                 |                 | referenced      |
|                 |                 |                 | check content   |
|                 |                 |                 | location and    |
|                 |                 |                 | AND their       |
|                 |                 |                 | results         |
|                 |                 |                 | together into a |
|                 |                 |                 | single          |
|                 |                 |                 | *\<xccdf:rule-r |
|                 |                 |                 | esult\>*        |
|                 |                 |                 | using the AND   |
|                 |                 |                 | truth table     |
|                 |                 |                 | below (Table    |
|                 |                 |                 | 12). This       |
|                 |                 |                 | corresponds to  |
|                 |                 |                 | a               |
|                 |                 |                 | *\@multi-check* |
|                 |                 |                 | attribute value |
|                 |                 |                 | of "false". If, |
|                 |                 |                 | however, the    |
|                 |                 |                 | *\@multi-check* |
|                 |                 |                 | attribute is    |
|                 |                 |                 | set to \"true\" |
|                 |                 |                 | and a nameless  |
|                 |                 |                 | *\<xccdf:check- |
|                 |                 |                 | content-ref\>*  |
|                 |                 |                 | is used, the    |
|                 |                 |                 | rule produces a |
|                 |                 |                 | separate        |
|                 |                 |                 | *\<xccdf:rule-r |
|                 |                 |                 | esult\>*        |
|                 |                 |                 | for each check  |
|                 |                 |                 | in the check    |
|                 |                 |                 | content         |
|                 |                 |                 | location.       |
|                 |                 |                 | (default:       |
|                 |                 |                 | false) See      |
|                 |                 |                 | Section         |
|                 |                 |                 | 7.2.3.5.2.      |
+-----------------+-----------------+-----------------+-----------------+
| xml:base        | *special*       | 0-1             | The context for |
| (attribute)     |                 |                 | all relative    |
|                 |                 |                 | URIs within the |
|                 |                 |                 | check.          |
+-----------------+-----------------+-----------------+-----------------+

In place of an *\<xccdf:check\>* element, XCCDF allows an
*\<xccdf:complex-check\>* element. An *\<xccdf:complex-check\>* is a
boolean logical expression whose individual terms are *\<xccdf:check\>*
and/or *\<xccdf:complex-check\>* elements. This allows benchmark authors
to create more sophisticated checks and to mix checks written with
different checking systems. An *\<xccdf:Rule\>* may have at most one
*\<xccdf:complex-check\>* element; on inheritance, the extending rule's
*\<xccdf:complex-check\>* shall replace the extended rule's
*\<xccdf:complex-check\>*. A benchmark consumer processing a benchmark
picks at most one *\<xccdf:check\>* or *\<xccdf:complex-check\>* element
to process for each rule. If both *\<xccdf:check\>* elements and an
*\<xccdf:complex-check\>* element appear in a rule, then the
*\<xccdf:check\>* elements will be ignored. See Section 7.2.3.5 for
additional information on check processing.

When an *\<xccdf:check\>* element is used with an
*\<xccdf:complex-check\>* element, the *\<xccdf:check\>* element's
*\@multi-check* attribute MUST be ignored.

Operators that may be used to combine the constituents of an
*\<xccdf:complex-check\>* are AND and OR. Truth tables that must be used
when evaluating these operators appear in Table 12 (AND) and Table 13
(OR). Each *\<xccdf:complex-check\>* may also specify that the
expression should be negated (NOT); see Table 14 for the corresponding
truth table. All of the abbreviations in the truth tables come from the
description of the *\<xccdf:rule-result\>* element (see Table 26 in
Section 6.6.4.2 for definitions of each abbreviation).

With an "AND" operator, the *\<xccdf:complex-check\>* evaluates to Pass
only if all of its enclosed terms (*\<xccdf:check\>* and
*\<xccdf:complex-check\>* elements) evaluate to Pass. Table 12 shows the
truth table for "AND".

[]{#_Ref294717068 .anchor}Table : Truth Table for AND

  ***AND ***   P   F   U   E   N   K   S   I
  ------------ --- --- --- --- --- --- --- ---
  P            P   F   U   E   P   P   P   P
  F            F   F   F   F   F   F   F   F
  U            U   F   U   U   U   U   U   U
  E            E   F   U   E   E   E   E   E
  N            P   F   U   E   N   N   N   N
  K            P   F   U   E   N   K   K   K
  S            P   F   U   E   N   K   S   S
  I            P   F   U   E   N   K   S   I

The "OR" operator evaluates to Pass if any of its enclosed terms
evaluate to Pass. The truth table for "OR" is shown in Table 13.

[]{#_Ref294717106 .anchor}Table : Truth Table for OR

  ***OR ***   P   F   U   E   N   K   S   I
  ----------- --- --- --- --- --- --- --- ---
  P           P   P   P   P   P   P   P   P
  F           P   F   U   E   F   F   F   F
  U           P   U   U   U   U   U   U   U
  E           P   E   U   E   E   E   E   E
  N           P   F   U   E   N   N   N   N
  K           P   F   U   E   N   K   K   K
  S           P   F   U   E   N   K   S   S
  I           P   F   U   E   N   K   S   I

If the *\@negate* attribute is set to true, then the result of the
*\<xccdf:complex-check\>* is complemented (inverted). The full truth
table for negation is listed in Table 14.

[]{#_Ref294717135 .anchor}Table : Truth Table for Negation

              P   F   U   E   N   K   S   I
  ----------- --- --- --- --- --- --- --- ---
  ***not***   F   P   U   E   N   K   S   I

The example below shows an *\<xccdf:complex-check\>* with several
components.

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

#### \<xccdf:fixtext\> and \<xccdf:fix\> Elements

The *\<xccdf:fixtext\>* and *\<xccdf:fix\>* elements help tools support
sophisticated facilities for automated and interactive remediation of
benchmark findings. Table 15 lists the possible properties of an
*\<xccdf:fixtext\>* element.

[]{#_Ref295835498 .anchor}Table : Possible Properties for
\<xccdf:fixtext\> Element

+-----------------+-----------------+-----------------+-----------------+
| Property        | Type            | Count           | Description     |
+=================+=================+=================+=================+
| sub (element)   | identifier      | 0-n             | Specifies an    |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | or              |
|                 |                 |                 | *\<xccdf:plain- |
|                 |                 |                 | text\>*         |
|                 |                 |                 | substitution.   |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.9.          |
+-----------------+-----------------+-----------------+-----------------+
| xml:lang        | *special*       | 0-1             | The language    |
| (attribute)     |                 |                 | for the         |
|                 |                 |                 | element; see    |
|                 |                 |                 | Section 6.2.10. |
+-----------------+-----------------+-----------------+-----------------+
| override        | boolean         | 0-1             | Specifies       |
| (attribute)     |                 |                 | inheritance     |
|                 |                 |                 | behavior (see   |
|                 |                 |                 | Section 6.3.1). |
|                 |                 |                 | (default:       |
|                 |                 |                 | false)          |
+-----------------+-----------------+-----------------+-----------------+
| fixref          | identifier      | 0-1             | A reference to  |
| (attribute)     |                 |                 | a specific      |
|                 |                 |                 | *\<xccdf:fix\>* |
|                 |                 |                 | *\@id*          |
|                 |                 |                 | attribute. This |
|                 |                 |                 | allows pairing  |
|                 |                 |                 | explanatory     |
|                 |                 |                 | text with       |
|                 |                 |                 | specific fix    |
|                 |                 |                 | procedures.     |
+-----------------+-----------------+-----------------+-----------------+
| reboot          | boolean         | 0-1             | Whether or not  |
| (attribute)     |                 |                 | remediation     |
|                 |                 |                 | will require a  |
|                 |                 |                 | reboot or hard  |
|                 |                 |                 | reset of the    |
|                 |                 |                 | target. When    |
|                 |                 |                 | specified, it   |
|                 |                 |                 | SHALL have one  |
|                 |                 |                 | of two values:  |
|                 |                 |                 | true (1) or     |
|                 |                 |                 | false (0)       |
|                 |                 |                 | (default: 0).   |
+-----------------+-----------------+-----------------+-----------------+
| strategy        | string          | 0-1             | The method or   |
| (attribute)     |                 |                 | approach for    |
|                 |                 |                 | fixing the      |
|                 |                 |                 | problem. When   |
|                 |                 |                 | specified, the  |
|                 |                 |                 | attribute shall |
|                 |                 |                 | have one of the |
|                 |                 |                 | following       |
|                 |                 |                 | values:         |
|                 |                 |                 |                 |
|                 |                 |                 | -   unknown     |
|                 |                 |                 |     > (strategy |
|                 |                 |                 |     > not       |
|                 |                 |                 |     > defined)  |
|                 |                 |                 |     > (default  |
|                 |                 |                 |     > )         |
|                 |                 |                 |                 |
|                 |                 |                 | -   configure   |
|                 |                 |                 |     > (adjust   |
|                 |                 |                 |     > target    |
|                 |                 |                 |     > configura |
|                 |                 |                 | tion/settings)  |
|                 |                 |                 |                 |
|                 |                 |                 | -   combination |
|                 |                 |                 |     > (combinat |
|                 |                 |                 | ion             |
|                 |                 |                 |     > of two or |
|                 |                 |                 |     > more      |
|                 |                 |                 |     > approache |
|                 |                 |                 | s)              |
|                 |                 |                 |                 |
|                 |                 |                 | -   disable     |
|                 |                 |                 |     > (turn off |
|                 |                 |                 |     > or        |
|                 |                 |                 |     > uninstall |
|                 |                 |                 |     > a target  |
|                 |                 |                 |     > component |
|                 |                 |                 | )               |
|                 |                 |                 |                 |
|                 |                 |                 | -   enable      |
|                 |                 |                 |     > (turn on  |
|                 |                 |                 |     > or        |
|                 |                 |                 |     > install a |
|                 |                 |                 |     > target    |
|                 |                 |                 |     > component |
|                 |                 |                 | )               |
|                 |                 |                 |                 |
|                 |                 |                 | -   patch       |
|                 |                 |                 |     > (apply a  |
|                 |                 |                 |     > patch,    |
|                 |                 |                 |     > hotfix,   |
|                 |                 |                 |     > update,   |
|                 |                 |                 |     > etc.)     |
|                 |                 |                 |                 |
|                 |                 |                 | -   policy      |
|                 |                 |                 |     > (remediat |
|                 |                 |                 | ion             |
|                 |                 |                 |     > requires  |
|                 |                 |                 |     > out-of-ba |
|                 |                 |                 | nd              |
|                 |                 |                 |     > adjustmen |
|                 |                 |                 | ts              |
|                 |                 |                 |     > to        |
|                 |                 |                 |     > policies  |
|                 |                 |                 |     > or        |
|                 |                 |                 |     > procedure |
|                 |                 |                 | s)              |
|                 |                 |                 |                 |
|                 |                 |                 | -   restrict    |
|                 |                 |                 |     > (adjust   |
|                 |                 |                 |     > permissio |
|                 |                 |                 | ns,             |
|                 |                 |                 |     > access    |
|                 |                 |                 |     > rights,   |
|                 |                 |                 |     > filters,  |
|                 |                 |                 |     > or other  |
|                 |                 |                 |     > access    |
|                 |                 |                 |     > restricti |
|                 |                 |                 | ons)            |
|                 |                 |                 |                 |
|                 |                 |                 | -   update      |
|                 |                 |                 |     > (install  |
|                 |                 |                 |     > upgrade   |
|                 |                 |                 |     > or update |
|                 |                 |                 |     > the       |
|                 |                 |                 |     > system)   |
+-----------------+-----------------+-----------------+-----------------+
| disruption      | string          | 0-1             | An estimate of  |
| (attribute)     |                 |                 | the potential   |
|                 |                 |                 | for disruption  |
|                 |                 |                 | or operational  |
|                 |                 |                 | degradation     |
|                 |                 |                 | that the        |
|                 |                 |                 | application of  |
|                 |                 |                 | this fix will   |
|                 |                 |                 | impose on the   |
|                 |                 |                 | target. When    |
|                 |                 |                 | specified, the  |
|                 |                 |                 | attribute shall |
|                 |                 |                 | have one of the |
|                 |                 |                 | following       |
|                 |                 |                 | values:         |
|                 |                 |                 |                 |
|                 |                 |                 | -   unknown     |
|                 |                 |                 |     > (disrupti |
|                 |                 |                 | on              |
|                 |                 |                 |     > not       |
|                 |                 |                 |     > defined)  |
|                 |                 |                 |     > (default) |
|                 |                 |                 |                 |
|                 |                 |                 | -   low (little |
|                 |                 |                 |     > or no     |
|                 |                 |                 |     > disruptio |
|                 |                 |                 | n               |
|                 |                 |                 |     > expected) |
|                 |                 |                 |                 |
|                 |                 |                 | -   medium      |
|                 |                 |                 |     > (potentia |
|                 |                 |                 | l               |
|                 |                 |                 |     > for minor |
|                 |                 |                 |     > or        |
|                 |                 |                 |     > short-liv |
|                 |                 |                 | ed              |
|                 |                 |                 |     > disruptio |
|                 |                 |                 | n)              |
|                 |                 |                 |                 |
|                 |                 |                 | -   high        |
|                 |                 |                 |     > (potentia |
|                 |                 |                 | l               |
|                 |                 |                 |     > for       |
|                 |                 |                 |     > serious   |
|                 |                 |                 |     > disruptio |
|                 |                 |                 | n)              |
+-----------------+-----------------+-----------------+-----------------+
| complexity      | string          | 0-1             | The estimated   |
| (attribute)     |                 |                 | complexity or   |
|                 |                 |                 | difficulty of   |
|                 |                 |                 | applying the    |
|                 |                 |                 | fix to the      |
|                 |                 |                 | target. When    |
|                 |                 |                 | specified, the  |
|                 |                 |                 | attribute shall |
|                 |                 |                 | have one of the |
|                 |                 |                 | following       |
|                 |                 |                 | values:         |
|                 |                 |                 |                 |
|                 |                 |                 | -   unknown     |
|                 |                 |                 |     > (complexi |
|                 |                 |                 | ty              |
|                 |                 |                 |     > not       |
|                 |                 |                 |     > defined)  |
|                 |                 |                 |     > (default) |
|                 |                 |                 |                 |
|                 |                 |                 | -   low (fix is |
|                 |                 |                 |     > very      |
|                 |                 |                 |     > simple to |
|                 |                 |                 |     > apply)    |
|                 |                 |                 |                 |
|                 |                 |                 | -   medium (fix |
|                 |                 |                 |     > is        |
|                 |                 |                 |     > moderatel |
|                 |                 |                 | y               |
|                 |                 |                 |     > difficult |
|                 |                 |                 |     > or        |
|                 |                 |                 |     > complex)  |
|                 |                 |                 |                 |
|                 |                 |                 | -   high (fix   |
|                 |                 |                 |     > is very   |
|                 |                 |                 |     > complex   |
|                 |                 |                 |     > to apply) |
+-----------------+-----------------+-----------------+-----------------+

Table 16 lists the possible properties of an *\<xccdf:fix\>* element.

[]{#_Ref298343814 .anchor}Table : Possible Properties for \<xccdf:fix\>
Element

  Property                 Type         Count   Description
  ------------------------ ------------ ------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  sub (element)            identifier   0-n     Specifies an *\<xccdf:Value\>* or *\<xccdf:plain-text\>* substitution. See Section 6.2.9.
  instance (element)       string               Designates a spot where the name of the instance should be substituted into the fix template to generate the final fix data. If the *\@context* attribute is omitted, the value of the context shall default to "undefined".
  id (attribute)           identifier   0-1     A local identifier for the element. It is optional for the id to be unique; multiple *\<xccdf:fix\>* elements may have the same id but different values for their other attributes.
  reboot (attribute)       boolean      0-1     Whether or not remediation will require a reboot or hard reset of the target. Permitted values: true (1) and false (0) (default: 0).
  strategy (attribute)     string       0-1     The method or approach for fixing the problem. See Table 15 for the list of possible values.
  disruption (attribute)   string       0-1     An estimate of the potential for disruption or operational degradation that the application of this fix will impose on the target. See Table 15 for the list of possible values.
  complexity (attribute)   string       0-1     The estimated complexity or difficulty of applying the fix to the target. See Table 15 for the list of possible values.
  system (attribute)       URI          0-1     A URI that identifies the scheme, language, engine, or process for which the fix contents are written. Table 17 defines several general-purpose URNs that may be used for this, and tool vendors and system providers may define and use target-specific URNs.
  platform (attribute)     URI          0-1     In case different fix scripts or procedures are required for different target platform types (e.g., different patches for Windows Vista and Windows 7), this attribute allows a CPE name or CPE applicability language expression to be associated with an *\<xccdf:fix\>* element. This should appear on an *\<xccdf:fix\>* when the content applies to only one platform out of several to which the rule could apply.

Table 17 lists predefined values for the *\@system* attribute of an
*\<xccdf:fix\>* element.

[]{#_Ref294717201 .anchor}Table : Predefined Values for \@system
Attribute of \<xccdf:fix\> Element

+-----------------------------------+-----------------------------------+
| **URI**                           | **Content of the \<xccdf:fix\>    |
|                                   | Element**                         |
+===================================+===================================+
| urn:xccdf:fix:commands            | A list of target system commands; |
|                                   | executed in order, the commands   |
|                                   | should bring the target system    |
|                                   | into compliance with the rule.    |
+-----------------------------------+-----------------------------------+
| urn:xccdf:fix:urls                | A list of one or more URLs. The   |
|                                   | resources identified by the URLs  |
|                                   | should be applied to bring the    |
|                                   | system into compliance with the   |
|                                   | rule.                             |
+-----------------------------------+-----------------------------------+
| urn:xccdf:fix:script:*language*   | A script written in the given     |
|                                   | *language*. Executing the script  |
|                                   | should bring the target system    |
|                                   | into compliance with the rule.    |
|                                   | The following languages are       |
|                                   | pre-defined:                      |
|                                   |                                   |
|                                   | sh -- Bourne shell                |
|                                   |                                   |
|                                   | csh -- C Shell                    |
|                                   |                                   |
|                                   | perl -- Perl                      |
|                                   |                                   |
|                                   | batch -- Windows batch script     |
|                                   |                                   |
|                                   | python -- Python and all          |
|                                   | Python-based scripting languages  |
|                                   |                                   |
|                                   | vbscript -- Visual Basic Script   |
|                                   | (VBS)                             |
|                                   |                                   |
|                                   | javascript -- Javascript          |
|                                   | (ECMAScript, Jscript)             |
|                                   |                                   |
|                                   | tcl -- Tcl and all Tcl-based      |
|                                   | scripting languages               |
+-----------------------------------+-----------------------------------+
| urn:xccdf:fix:patch:*vendor*      | A patch identifier, in            |
|                                   | proprietary format as defined by  |
|                                   | the vendor. The vendor string     |
|                                   | should be the DNS domain name of  |
|                                   | the vendor. For example, for      |
|                                   | Microsoft Corporation, the DNS    |
|                                   | domain is "Microsoft.com".        |
+-----------------------------------+-----------------------------------+

### \<xccdf:Value\> Element

#### Basics

An *\<xccdf:Value\>* is a named parameter (with a unique *\@id*
attribute) that can be substituted into properties of other elements
within the benchmark, including the interior of structured check
specifications and fix scripts. An *\<xccdf:Value\>* can hold string,
boolean, or numeric content, or lists thereof. *\<xccdf:Value\>*
elements can include information about permissible values, display
defaults, and restrictions on tailoring for the Value. For example, an
*\<xccdf:Value\>* might be used to hold a benchmark's lower limit for
password length on some OS. In a profile for that OS to be used in a
closed lab, the default value might be 8, but in a profile for that OS
to be used on the Internet, the default value might be 12.

The example below shows an *\<xccdf:Value\>* element.

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

*\<xccdf:Value\>* elements may encapsulate values that are lists
(possibly zero-length) of simple types. These structures are supported
by the *\<xccdf:complex-value\>* element, the
*\<xccdf:complex-default\>* element, and within the *\<xccdf:choices\>*
element, the *\<xccdf:complex-choice\>* element. If any of these
elements has no child elements, it represents an empty list.

A single *\<xccdf:Value\>* may use a mixture of both simple (i.e., a
single number, string, or boolean value) and complex types. Apart from
its ability to encapsulate different types of information, a complex
field is used in the same way as its simple counterpart.

#### Properties

Table 18 describes the *\<xccdf:Value\>* element's properties (in
addition to the core item properties previously described in Table 5).

[]{#_Ref294717285 .anchor}Table : \<xccdf:Value\> Element Properties

+-----------------+-----------------+-----------------+-----------------+
| Property        | Type            | Count           | Description     |
+=================+=================+=================+=================+
| value (element) | string          | 1-n             | The current     |
|                 |                 |                 | value of this   |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*.             |
|                 |                 |                 | This element    |
|                 |                 |                 | may have a      |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.5.5).       |
+-----------------+-----------------+-----------------+-----------------+
| complex-value   | *special*       |                 | Similar to      |
| (element)       |                 |                 | *\<xccdf:value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | except that     |
|                 |                 |                 | this element    |
|                 |                 |                 | supports values |
|                 |                 |                 | that are lists  |
|                 |                 |                 | of simple       |
|                 |                 |                 | types. This     |
|                 |                 |                 | element MAY     |
|                 |                 |                 | have a          |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.5.5).       |
+-----------------+-----------------+-----------------+-----------------+
| default         | string          | 0-n             | The default     |
| (element)       |                 |                 | value displayed |
|                 |                 |                 | to the user as  |
|                 |                 |                 | a suggestion by |
|                 |                 |                 | benchmark       |
|                 |                 |                 | producers       |
|                 |                 |                 | during          |
|                 |                 |                 | tailoring of    |
|                 |                 |                 | this            |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element. (This  |
|                 |                 |                 | is not the      |
|                 |                 |                 | default value   |
|                 |                 |                 | of an           |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*.)            |
|                 |                 |                 | This element    |
|                 |                 |                 | may have a      |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.5.5).       |
+-----------------+-----------------+-----------------+-----------------+
| complex-default | *special*       |                 | Similar to      |
| (element)       |                 |                 | *\<xccdf:defaul |
|                 |                 |                 | t\>*            |
|                 |                 |                 | except that     |
|                 |                 |                 | this element    |
|                 |                 |                 | supports        |
|                 |                 |                 | default values  |
|                 |                 |                 | that are lists  |
|                 |                 |                 | of simple       |
|                 |                 |                 | types. This     |
|                 |                 |                 | element MAY     |
|                 |                 |                 | have a          |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.5.5).       |
+-----------------+-----------------+-----------------+-----------------+
| match (element) | string          | 0-n             | A Perl          |
|                 |                 |                 | Compatible      |
|                 |                 |                 | Regular         |
|                 |                 |                 | Expression      |
|                 |                 |                 | (PCRE) (see     |
|                 |                 |                 | \[PCRE\] and    |
|                 |                 |                 | \[UNICODE\])    |
|                 |                 |                 | that a          |
|                 |                 |                 | benchmark       |
|                 |                 |                 | producer may    |
|                 |                 |                 | apply during    |
|                 |                 |                 | tailoring to    |
|                 |                 |                 | validate a      |
|                 |                 |                 | user's input    |
|                 |                 |                 | for the Value.  |
|                 |                 |                 | It shall use    |
|                 |                 |                 | implicit        |
|                 |                 |                 | anchoring. It   |
|                 |                 |                 | shall apply     |
|                 |                 |                 | only when the   |
|                 |                 |                 | *\@type* is     |
|                 |                 |                 | "string" or     |
|                 |                 |                 | "number" or a   |
|                 |                 |                 | list of strings |
|                 |                 |                 | and/or numbers. |
|                 |                 |                 | For example, if |
|                 |                 |                 | the *\@type*    |
|                 |                 |                 | was "string",   |
|                 |                 |                 | but the value   |
|                 |                 |                 | was meant to be |
|                 |                 |                 | a Cisco IOS     |
|                 |                 |                 | router          |
|                 |                 |                 | interface name, |
|                 |                 |                 | then the        |
|                 |                 |                 | *\<xccdf:match\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element might   |
|                 |                 |                 | be set to       |
|                 |                 |                 | "\[A-Za-z\]+    |
|                 |                 |                 | \*\[0-9\]+(/\[0 |
|                 |                 |                 | -9.\]+)\*".     |
|                 |                 |                 | This would      |
|                 |                 |                 | allow a         |
|                 |                 |                 | tailoring tool  |
|                 |                 |                 | to reject an    |
|                 |                 |                 | invalid user    |
|                 |                 |                 | input like      |
|                 |                 |                 | "f8xq+" but     |
|                 |                 |                 | accept a legal  |
|                 |                 |                 | one like        |
|                 |                 |                 | "Ethernet1/3".  |
|                 |                 |                 | This element    |
|                 |                 |                 | may have a      |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.5.5).       |
+-----------------+-----------------+-----------------+-----------------+
| lower-bound     | decimal         | 0-n             | Minimum legal   |
| (element)       |                 |                 | value for this  |
|                 |                 |                 | Value. It is    |
|                 |                 |                 | used to         |
|                 |                 |                 | constrain value |
|                 |                 |                 | input during    |
|                 |                 |                 | tailoring, when |
|                 |                 |                 | the *\@type* is |
|                 |                 |                 | "number".       |
|                 |                 |                 | Values supplied |
|                 |                 |                 | by the user for |
|                 |                 |                 | tailoring the   |
|                 |                 |                 | benchmark must  |
|                 |                 |                 | be equal to or  |
|                 |                 |                 | greater than    |
|                 |                 |                 | this number.    |
|                 |                 |                 | This element    |
|                 |                 |                 | may have a      |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.5.5).       |
+-----------------+-----------------+-----------------+-----------------+
| upper-bound     | decimal         | 0-n             | Maximum legal   |
| (element)       |                 |                 | value for this  |
|                 |                 |                 | Value. It is    |
|                 |                 |                 | used to         |
|                 |                 |                 | constrain value |
|                 |                 |                 | input during    |
|                 |                 |                 | tailoring, when |
|                 |                 |                 | the *\@type* is |
|                 |                 |                 | "number".       |
|                 |                 |                 | Values supplied |
|                 |                 |                 | by the user for |
|                 |                 |                 | tailoring the   |
|                 |                 |                 | benchmark must  |
|                 |                 |                 | be less than or |
|                 |                 |                 | equal to this   |
|                 |                 |                 | number. This    |
|                 |                 |                 | element may     |
|                 |                 |                 | have a          |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.5.5).       |
+-----------------+-----------------+-----------------+-----------------+
| choices         | *special*       | 0-n             | A list of legal |
| (element)       |                 |                 | or suggested    |
|                 |                 |                 | choices         |
|                 |                 |                 | (values) for an |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element, to be  |
|                 |                 |                 | used during     |
|                 |                 |                 | tailoring and   |
|                 |                 |                 | document        |
|                 |                 |                 | generation. See |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.5.3.        |
+-----------------+-----------------+-----------------+-----------------+
| source          | URI             | 0-n             | URI indicating  |
| (element)       |                 |                 | where the tool  |
|                 |                 |                 | may acquire     |
|                 |                 |                 | values, value   |
|                 |                 |                 | bounds, or      |
|                 |                 |                 | value choices   |
|                 |                 |                 | for this        |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element. XCCDF  |
|                 |                 |                 | does not attach |
|                 |                 |                 | any meaning to  |
|                 |                 |                 | the URI; it may |
|                 |                 |                 | be an arbitrary |
|                 |                 |                 | community or    |
|                 |                 |                 | tool-specific   |
|                 |                 |                 | value, or a     |
|                 |                 |                 | pointer         |
|                 |                 |                 | directly to a   |
|                 |                 |                 | resource. If    |
|                 |                 |                 | several values  |
|                 |                 |                 | for the         |
|                 |                 |                 | *\<xccdf:source |
|                 |                 |                 | \>*             |
|                 |                 |                 | element appear, |
|                 |                 |                 | then they shall |
|                 |                 |                 | represent       |
|                 |                 |                 | alternative     |
|                 |                 |                 | means or        |
|                 |                 |                 | locations for   |
|                 |                 |                 | obtaining the   |
|                 |                 |                 | value in        |
|                 |                 |                 | descending      |
|                 |                 |                 | order of        |
|                 |                 |                 | preference      |
|                 |                 |                 | (i.e., most     |
|                 |                 |                 | preferred       |
|                 |                 |                 | first).         |
+-----------------+-----------------+-----------------+-----------------+
| signature       | *special*       | 0-1             | A digital       |
| (element)       |                 |                 | signature       |
|                 |                 |                 | asserting       |
|                 |                 |                 | authorship and  |
|                 |                 |                 | allowing        |
|                 |                 |                 | verification of |
|                 |                 |                 | the integrity   |
|                 |                 |                 | of the Value.   |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.7.          |
+-----------------+-----------------+-----------------+-----------------+
| id (attribute)  | *special*       | 1               | Unique element  |
|                 |                 |                 | identifier. See |
|                 |                 |                 | Section 6.2.3.  |
+-----------------+-----------------+-----------------+-----------------+
| type            | string          | 0-1             | The data type   |
| (attribute)     |                 |                 | of the Value:   |
|                 |                 |                 | "string",       |
|                 |                 |                 | "number", or    |
|                 |                 |                 | "boolean"       |
|                 |                 |                 | (default:       |
|                 |                 |                 | "string"). A    |
|                 |                 |                 | tool may choose |
|                 |                 |                 | any convenient  |
|                 |                 |                 | form to store a |
|                 |                 |                 | Value's         |
|                 |                 |                 | *\<xccdf:value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element, but    |
|                 |                 |                 | the *\@type*    |
|                 |                 |                 | attribute       |
|                 |                 |                 | conveys how the |
|                 |                 |                 | Value should be |
|                 |                 |                 | treated for     |
|                 |                 |                 | user input      |
|                 |                 |                 | validation      |
|                 |                 |                 | purposes during |
|                 |                 |                 | tailoring       |
|                 |                 |                 | processing. The |
|                 |                 |                 | *\@type*        |
|                 |                 |                 | attribute may   |
|                 |                 |                 | also be used to |
|                 |                 |                 | give additional |
|                 |                 |                 | guidance to the |
|                 |                 |                 | user or to      |
|                 |                 |                 | validate the    |
|                 |                 |                 | user's input.   |
|                 |                 |                 | For example, if |
|                 |                 |                 | an              |
|                 |                 |                 | *\<xccdf:value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element's       |
|                 |                 |                 | *\@type*        |
|                 |                 |                 | attribute is    |
|                 |                 |                 | "number", then  |
|                 |                 |                 | a tool might    |
|                 |                 |                 | choose to       |
|                 |                 |                 | reject user     |
|                 |                 |                 | tailoring input |
|                 |                 |                 | that is not     |
|                 |                 |                 | composed of     |
|                 |                 |                 | digits. In the  |
|                 |                 |                 | case of a list  |
|                 |                 |                 | of values, the  |
|                 |                 |                 | *\@type*        |
|                 |                 |                 | attribute, if   |
|                 |                 |                 | present, shall  |
|                 |                 |                 | be applied to   |
|                 |                 |                 | all elements of |
|                 |                 |                 | the list        |
|                 |                 |                 | individually.   |
+-----------------+-----------------+-----------------+-----------------+
| operator        | string          | 0-1             | The operator to |
| (attribute)     |                 |                 | be used for     |
|                 |                 |                 | comparing this  |
|                 |                 |                 | Value to some   |
|                 |                 |                 | part of the     |
|                 |                 |                 | test system's   |
|                 |                 |                 | configuration   |
|                 |                 |                 | during rule     |
|                 |                 |                 | checking        |
|                 |                 |                 | (default:       |
|                 |                 |                 | "equals"). See  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.5.4.        |
+-----------------+-----------------+-----------------+-----------------+
| interactive     | boolean         | 0-1             | Whether         |
| (attribute)     |                 |                 | tailoring for   |
|                 |                 |                 | this Value      |
|                 |                 |                 | should also be  |
|                 |                 |                 | performed       |
|                 |                 |                 | during          |
|                 |                 |                 | benchmark       |
|                 |                 |                 | application     |
|                 |                 |                 | (default:       |
|                 |                 |                 | false). The     |
|                 |                 |                 | benchmark       |
|                 |                 |                 | consumer may    |
|                 |                 |                 | ignore the      |
|                 |                 |                 | attribute if    |
|                 |                 |                 | asking the user |
|                 |                 |                 | is not feasible |
|                 |                 |                 | or not          |
|                 |                 |                 | supported.      |
+-----------------+-----------------+-----------------+-----------------+
| interfaceHint   | string          | 0-1             | A hint or       |
| (attribute)     |                 |                 | recommendation  |
|                 |                 |                 | to a benchmark  |
|                 |                 |                 | consumer or     |
|                 |                 |                 | producer about  |
|                 |                 |                 | how the user    |
|                 |                 |                 | might select or |
|                 |                 |                 | adjust the      |
|                 |                 |                 | Value. If used, |
|                 |                 |                 | this element    |
|                 |                 |                 | shall have one  |
|                 |                 |                 | of the          |
|                 |                 |                 | following       |
|                 |                 |                 | interface       |
|                 |                 |                 | pattern values: |
|                 |                 |                 |                 |
|                 |                 |                 | -   "choice"    |
|                 |                 |                 |     > (multiple |
|                 |                 |                 |     > choice)   |
|                 |                 |                 |                 |
|                 |                 |                 | -   "textline"  |
|                 |                 |                 |     > (multiple |
|                 |                 |                 |     > lines of  |
|                 |                 |                 |     > text)     |
|                 |                 |                 |                 |
|                 |                 |                 | -   "text"      |
|                 |                 |                 |     > (single   |
|                 |                 |                 |     > line of   |
|                 |                 |                 |     > text)     |
|                 |                 |                 |                 |
|                 |                 |                 | -   "date"      |
|                 |                 |                 |     > (date)    |
|                 |                 |                 |                 |
|                 |                 |                 | -   "datetime"  |
|                 |                 |                 |     > (date and |
|                 |                 |                 |     > time)     |
+-----------------+-----------------+-----------------+-----------------+

#### \<xccdf:choices\> Element

The *\<xccdf:choices\>* element SHOULD be used when there are a moderate
number of known values that are most appropriate. For example, if the
Value were the authentication mode for a server, the choices might be
"password" and "pki". Table 19 lists the possible properties for an
*\<xccdf:choices\>* element. If a product presents the choice values
from an *\<xccdf:choices\>* element to a user, they SHOULD be presented
in the order in which they appear.

[]{#_Ref295984487 .anchor}Table : Possible Properties for
\<xccdf:choices\> Element

  Property                   Type        Count   Description
  -------------------------- ----------- ------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  choice (element)           string      1-n     The text of a possible choice.
  complex-choice (element)   *special*           A possible choice consisting of a list of values. If this element has no *\<xccdf:item\>* children, it SHALL represent an empty list.
  mustMatch (attribute)      boolean     0-1     If this is true, the list shall represent all the legal values. If this is false or absent, the list shall represent suggested values, and other values may also be legal (subject to the parent *\<xccdf:Value\>* element's *\@upper-bound*, *\@lower-bound*, and *\@match* attributes).
  selector (attribute)       string      0-1     Used for tailoring via a profile. See Section 6.4.5.5.

#### @operator Attribute

When defining an *\<xccdf:Value\>* element, the benchmark author may
specify the operator to be used for comparing the Value to a
configuration during rule checking. For example, one part of an OS
benchmark might be verifying that the configuration included a minimum
password length; the *\<xccdf:Value\>* element that holds the tailorable
minimum could have *\@type* "number" and *\@operator* "greater than".
Exactly how *\<xccdf:Value\>* elements are used in rules depends on the
capabilities of the checking system. Benchmark consumers may ignore the
*\@operator* attribute; therefore, authors should include sufficient
information in the *\<xccdf:description\>* and *\<xccdf:question\>*
elements to make the role of the Value clear. Table 20 describes the
*\@operator* values permitted for each *\@type* value.

[]{#_Ref294717389 .anchor}Table : Permitted Operators by Value Type

  Value Type   Available Operators
  ------------ -----------------------------------------------------------------------------------------------------------------------------------
  number       equals, not equal, less than, greater than, less than or equal, greater than or equal (default: equals)
  boolean      equals, not equal (default: equals)
  string       equals, not equal, pattern match (pattern match means regular expression match; should comply with \[UNICODE\]) (default: equals)
  lists        as component data type

#### \@selector Attribute

As detailed in Table 18, an *\<xccdf:Value\>* may include various
elements that constrain or limit the values that the Value may be given.
Authors may use these elements to assist users in tailoring the
benchmark. These elements all support the *\@selector* attribute. If
there are multiple instances of one of these elements within a single
*\<xccdf:Value\>* element, no more than one of the instances may omit
the *\@selector* attribute, and every other instance must have a
different value specified for its *\@selector* attribute. For elements
that may be substituted for each other---specifically, *\<xccdf:value\>*
and *\<xccdf:complex-value\>* elements, and *\<xccdf:default\>* and
*\<xccdf:complex-default\>* elements---no more than one instance of
either element may omit the *\@selector* attribute, and every other
instance of both elements must have a different value specified for its
*\@selector* attribute. For more information about selector types, see
Section 6.5.3.

In the absence of any profile or tailoring actions, the default
*\<xccdf:value\>* or *\<xccdf:complex-value\>* element in an
*\<xccdf:Value\>* shall be the one with an empty or absent *\@selector*.
If there is no *\<xccdf:value\>* or *\<xccdf:complex-value\>* element
with an empty or absent *\@selector*, the first *\<xccdf:value\>* or
*\<xccdf:complex-value\>* element in top-down processing of the XML
shall be the default element. For all other selectable *\<xccdf:Value\>*
elements (i.e., *\<xccdf:default\>, \<xccdf:complex-default\>,
\<xccdf:match\>*, *\<xccdf:upper-bound\>, \<xccdf:lower-bound\>*, and
*\<xccdf:choices\>*), the default activity shall be to ignore all
elements without an empty or absent *\@selector*.

## \<xccdf:Profile\> Element

### Basics

An *\<xccdf:Profile\>* element is a named tailoring for a benchmark.
While a benchmark can be tailored in place by setting properties of
various elements, *\<xccdf:Profile\>* elements allow one benchmark
document to hold several independent tailorings. The *\<xccdf:select\>*
element children of the *\<xccdf:Profile\>* affect which
*\<xccdf:Group\>* and *\<xccdf:Rule\>* elements are selected for
processing when the *\<xccdf:Profile\>* is in effect. The
*\<xccdf:refine-rule\>* element allows modification of properties in
*\<xccdf:Group\>* and *\<xccdf:Rule\>* elements, while the
*\<xccdf:refine-value\>* element allows modification of
*\<xccdf:Value\>* properties, including selection of the effective
value. Finally, *\<xccdf:set-value\>* and *\<xccdf:set-complex-value\>*
allow an *\<xccdf:Value\>* element\'s value to be set directly to a
simple or complex setting, respectively. The example below shows a
simple *\<xccdf:Profile\>*.

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

An *\<xccdf:Profile\>* MAY extend another *\<xccdf:Profile\>* in the
same benchmark by using the *\@extends* attribute. An
*\<xccdf:Profile\>* in an *\<xccdf:Tailoring\>* element MAY extend any
*\<xccdf:Profile\>* in a benchmark, but *\<xccdf:Profile\>* elements in
an *\<xccdf:Tailoring\>* element SHALL NOT be extended.

Certain properties of the extended *\<xccdf:Profile\>* appear before the
corresponding properties in the extending *\<xccdf:Profile\>*. Since
*\<xccdf:Profile\>* properties are processed in the order in which they
appear in the XML, this means that properties of the extending
*\<xccdf:Profile\>* can override corresponding properties inherited from
the extended *\<xccdf:Profile\>*. See Sections 7.2.2 and 7.2.3.4 for
additional information on extension and inheritance.

### Properties

Table 21 describes the *\<xccdf:Profile\>* element's properties.

[]{#_Ref294717444 .anchor}Table : \<xccdf:Profile\> Element Properties

+-----------------+-----------------+-----------------+-----------------+
| Property        | Type            | Count           | Description     |
+=================+=================+=================+=================+
| status          | *special*       | 0-n             | Status of this  |
| (element)       |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*and         |
|                 |                 |                 | date at which   |
|                 |                 |                 | it attained     |
|                 |                 |                 | that status.    |
|                 |                 |                 | Authors may use |
|                 |                 |                 | this element to |
|                 |                 |                 | record the      |
|                 |                 |                 | maturity or     |
|                 |                 |                 | consensus level |
|                 |                 |                 | of a profile.   |
|                 |                 |                 | If the status   |
|                 |                 |                 | is not given    |
|                 |                 |                 | explicitly,     |
|                 |                 |                 | then the        |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*            |
|                 |                 |                 | is taken to     |
|                 |                 |                 | have the same   |
|                 |                 |                 | status as its   |
|                 |                 |                 | parent          |
|                 |                 |                 | *\<xccdf:Benchm |
|                 |                 |                 | ark\>*.         |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.8.          |
+-----------------+-----------------+-----------------+-----------------+
| dc-status       | *special*       | 0-n             | Holds           |
| (element)       |                 |                 | additional      |
|                 |                 |                 | status          |
|                 |                 |                 | information     |
|                 |                 |                 | using the       |
|                 |                 |                 | Dublin Core     |
|                 |                 |                 | format. See     |
|                 |                 |                 | Section 6.2.8.  |
+-----------------+-----------------+-----------------+-----------------+
| version         | string          | 0-1             | Version number  |
| (element)       |                 |                 | of this         |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*,           |
|                 |                 |                 | with an         |
|                 |                 |                 | optional        |
|                 |                 |                 | *\@time*        |
|                 |                 |                 | timestamp       |
|                 |                 |                 | attribute (when |
|                 |                 |                 | the version was |
|                 |                 |                 | completed) and  |
|                 |                 |                 | an optional     |
|                 |                 |                 | *\@update* URI  |
|                 |                 |                 | attribute       |
|                 |                 |                 | (where updates  |
|                 |                 |                 | may be          |
|                 |                 |                 | obtained).      |
+-----------------+-----------------+-----------------+-----------------+
| title (element) | string          | 1-n             | Title of this   |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*.           |
|                 |                 |                 | It may have one |
|                 |                 |                 | or more         |
|                 |                 |                 | *\<xccdf:sub\>* |
|                 |                 |                 | elements (see   |
|                 |                 |                 | Section 6.2.9), |
|                 |                 |                 | an *\@xml:lang* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10), and/or |
|                 |                 |                 | an *\@override* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1). |
+-----------------+-----------------+-----------------+-----------------+
| description     | HTML-enabled    | 0-n             | Text that       |
| (element)       | text            |                 | describes this  |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*.           |
|                 |                 |                 | It MAY have one |
|                 |                 |                 | or more         |
|                 |                 |                 | *\<xccdf:sub\>* |
|                 |                 |                 | elements (see   |
|                 |                 |                 | Section 6.2.9), |
|                 |                 |                 | an *\@override* |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section 6.3.1), |
|                 |                 |                 | and/or an       |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10).        |
+-----------------+-----------------+-----------------+-----------------+
| reference       | *special*       | 0-n             | A reference     |
| (element)       |                 |                 | where the user  |
|                 |                 |                 | can learn more  |
|                 |                 |                 | about the       |
|                 |                 |                 | subject of this |
|                 |                 |                 | profile. See    |
|                 |                 |                 | Section 6.2.6.  |
+-----------------+-----------------+-----------------+-----------------+
| platform        | string          | 0-n             | A target        |
| (element)       |                 |                 | platform for    |
|                 |                 |                 | this profile.   |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.5.          |
+-----------------+-----------------+-----------------+-----------------+
| select,         | *special*       | 0-n             | References to   |
| set-complex-val |                 |                 | Groups, Rules,  |
| ue,             |                 |                 | and Values for  |
| set-value,      |                 |                 | customization   |
| refine-value,   |                 |                 | and tailoring.  |
| refine-rule     |                 |                 | See Section     |
| (element)       |                 |                 | 6.5.3.          |
+-----------------+-----------------+-----------------+-----------------+
| metadata        | *special*       | 0-n             | Metadata        |
| (element)       |                 |                 | associated with |
|                 |                 |                 | this            |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*.           |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.4.          |
+-----------------+-----------------+-----------------+-----------------+
| signature       | *special*       | 0-1             | A digital       |
| (element)       |                 |                 | signature       |
|                 |                 |                 | asserting       |
|                 |                 |                 | authorship and  |
|                 |                 |                 | allowing        |
|                 |                 |                 | verification of |
|                 |                 |                 | the integrity   |
|                 |                 |                 | of the          |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*.           |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.7.          |
+-----------------+-----------------+-----------------+-----------------+
| id (attribute)  | *special*       | 1               | Unique          |
|                 |                 |                 | identifier for  |
|                 |                 |                 | this            |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*.           |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.3.          |
+-----------------+-----------------+-----------------+-----------------+
| prohibitChanges | boolean         | 0-1             | Whether or not  |
| (attribute)     |                 |                 | products should |
|                 |                 |                 | prohibit        |
|                 |                 |                 | changes to this |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*            |
|                 |                 |                 | (default:       |
|                 |                 |                 | false).         |
+-----------------+-----------------+-----------------+-----------------+
| profileOrdering | boolean         | 0-1             | If true, then   |
| (attribute)     |                 |                 | processing of   |
|                 |                 |                 | the profile is  |
|                 |                 |                 | ordered by the  |
|                 |                 |                 | *\<xccdf:select |
|                 |                 |                 | \>*             |
|                 |                 |                 | elements as     |
|                 |                 |                 | they are listed |
|                 |                 |                 | from top to     |
|                 |                 |                 | bottom in the   |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*            |
+-----------------+-----------------+-----------------+-----------------+
| abstract        | boolean         | 0-1             | If true, then   |
| (attribute)     |                 |                 | this            |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*            |
|                 |                 |                 | exists solely   |
|                 |                 |                 | to be extended  |
|                 |                 |                 | by other        |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*            |
|                 |                 |                 | elements        |
|                 |                 |                 | (default:       |
|                 |                 |                 | false). See     |
|                 |                 |                 | Sections 6.3.1  |
|                 |                 |                 | and 7.2.2.      |
+-----------------+-----------------+-----------------+-----------------+
| note-tag        | identifier      | 0-1             | Tag identifier  |
| (attribute)     |                 |                 | to specify      |
|                 |                 |                 | which           |
|                 |                 |                 | *\<xccdf:profil |
|                 |                 |                 | e-note\>*       |
|                 |                 |                 | element from an |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | should be       |
|                 |                 |                 | associated with |
|                 |                 |                 | this            |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>.*           |
+-----------------+-----------------+-----------------+-----------------+
| extends         | identifier      | 0-1             | The id of an    |
| (attribute)     |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>*            |
|                 |                 |                 | on which to     |
|                 |                 |                 | base this       |
|                 |                 |                 | *\<xccdf:Profil |
|                 |                 |                 | e\>.*           |
+-----------------+-----------------+-----------------+-----------------+
| xml:base        | *special*       | 0-1             | The context for |
| (attribute)     |                 |                 | all relative    |
|                 |                 |                 | URIs within the |
|                 |                 |                 | profile.        |
+-----------------+-----------------+-----------------+-----------------+
| Id (attribute)  | *special*       | 0-1             | An identifier   |
|                 |                 |                 | used for        |
|                 |                 |                 | referencing     |
|                 |                 |                 | elements        |
|                 |                 |                 | included in an  |
|                 |                 |                 | XML signature.  |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.2.7.          |
+-----------------+-----------------+-----------------+-----------------+

### Selectors

Each *\<xccdf:Profile\>* can contain one or more selectors to express a
particular customization or tailoring of the *\<xccdf:Benchmark\>.*
Table 22 defines the five kinds of selectors.

[]{#_Ref296340762 .anchor}Table : Selectors

+-----------------+-----------------+-----------------+-----------------+
| Property        | Type            | Count           | Description     |
+=================+=================+=================+=================+
| select          | *special*       | 0-n             | Designates a    |
| (element)       |                 |                 | Rule, Group, or |
|                 |                 |                 | cluster of      |
|                 |                 |                 | Rules and       |
|                 |                 |                 | Groups. It      |
|                 |                 |                 | overrides the   |
|                 |                 |                 | *\@selected*    |
|                 |                 |                 | attribute on    |
|                 |                 |                 | the designated  |
|                 |                 |                 | items,          |
|                 |                 |                 | providing a     |
|                 |                 |                 | means for       |
|                 |                 |                 | including or    |
|                 |                 |                 | excluding rules |
|                 |                 |                 | from an         |
|                 |                 |                 | assessment.     |
|                 |                 |                 |                 |
|                 |                 |                 | The             |
|                 |                 |                 | *\<xccdf:select |
|                 |                 |                 | \>*             |
|                 |                 |                 | element may     |
|                 |                 |                 | have one or     |
|                 |                 |                 | more            |
|                 |                 |                 | *\<xccdf:remark |
|                 |                 |                 | \>*             |
|                 |                 |                 | elements        |
|                 |                 |                 | (strings)       |
|                 |                 |                 | containing      |
|                 |                 |                 | explanatory     |
|                 |                 |                 | material or     |
|                 |                 |                 | other prose.    |
|                 |                 |                 | Each instance   |
|                 |                 |                 | of              |
|                 |                 |                 | *\<xccdf:remark |
|                 |                 |                 | \>*             |
|                 |                 |                 | MAY have an     |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10).The     |
|                 |                 |                 | *\<xccdf:select |
|                 |                 |                 | \>*             |
|                 |                 |                 | element's       |
|                 |                 |                 | *\@idref*       |
|                 |                 |                 | attribute       |
|                 |                 |                 | (identifier)    |
|                 |                 |                 | shall specify   |
|                 |                 |                 | the designated  |
|                 |                 |                 | Group, Rule, or |
|                 |                 |                 | cluster of      |
|                 |                 |                 | Rules and       |
|                 |                 |                 | Groups; it must |
|                 |                 |                 | match the       |
|                 |                 |                 | *\@id*          |
|                 |                 |                 | attribute of a  |
|                 |                 |                 | Group or Rule   |
|                 |                 |                 | in the          |
|                 |                 |                 | Benchmark, or   |
|                 |                 |                 | the cluster id  |
|                 |                 |                 | assigned to one |
|                 |                 |                 | or more Rules   |
|                 |                 |                 | or Groups. If   |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:select |
|                 |                 |                 | \>*             |
|                 |                 |                 | element's       |
|                 |                 |                 | required        |
|                 |                 |                 | *\@selected*    |
|                 |                 |                 | attribute       |
|                 |                 |                 | (boolean) is    |
|                 |                 |                 | set to true,    |
|                 |                 |                 | the Rule,       |
|                 |                 |                 | Group, or Rules |
|                 |                 |                 | and Groups in   |
|                 |                 |                 | the cluster     |
|                 |                 |                 | shall have      |
|                 |                 |                 | their           |
|                 |                 |                 | *\@selected*    |
|                 |                 |                 | attribute set   |
|                 |                 |                 | to true,        |
|                 |                 |                 | otherwise they  |
|                 |                 |                 | shall have      |
|                 |                 |                 | their           |
|                 |                 |                 | *\@selected*    |
|                 |                 |                 | attribute set   |
|                 |                 |                 | to false.       |
|                 |                 |                 | Subsequent      |
|                 |                 |                 | tailoring       |
|                 |                 |                 | actions may     |
|                 |                 |                 | further modify  |
|                 |                 |                 | these           |
|                 |                 |                 | properties.     |
+-----------------+-----------------+-----------------+-----------------+
| set-value       | string          | 0-n             | Points to an    |
| (element)       |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element and     |
|                 |                 |                 | overrides its   |
|                 |                 |                 | *\<xccdf:value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | and             |
|                 |                 |                 | *\<xccdf:comple |
|                 |                 |                 | x-value\>*      |
|                 |                 |                 | element(s)      |
|                 |                 |                 | without         |
|                 |                 |                 | changing any    |
|                 |                 |                 | other           |
|                 |                 |                 | properties. It  |
|                 |                 |                 | provides a      |
|                 |                 |                 | means for       |
|                 |                 |                 | directly        |
|                 |                 |                 | specifying the  |
|                 |                 |                 | value of a      |
|                 |                 |                 | variable to be  |
|                 |                 |                 | used in         |
|                 |                 |                 | benchmark       |
|                 |                 |                 | processing.     |
|                 |                 |                 | This selector   |
|                 |                 |                 | may also be     |
|                 |                 |                 | applied to the  |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | elements in a   |
|                 |                 |                 | cluster by      |
|                 |                 |                 | specifying the  |
|                 |                 |                 | cluster id in   |
|                 |                 |                 | the *\@idref*   |
|                 |                 |                 | attribute, in   |
|                 |                 |                 | which case it   |
|                 |                 |                 | shall override  |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | elements of all |
|                 |                 |                 | of them.        |
+-----------------+-----------------+-----------------+-----------------+
| set-complex-val | *special*       | 0-n             | Supports the    |
| ue              |                 |                 | direct          |
| (element)       |                 |                 | specification   |
|                 |                 |                 | of complex      |
|                 |                 |                 | value types     |
|                 |                 |                 | such as lists.  |
|                 |                 |                 | Zero or more    |
|                 |                 |                 | item elements   |
|                 |                 |                 | MAY appear as   |
|                 |                 |                 | children of     |
|                 |                 |                 | this element;   |
|                 |                 |                 | if no child     |
|                 |                 |                 | elements are    |
|                 |                 |                 | present, this   |
|                 |                 |                 | element SHALL   |
|                 |                 |                 | represent an    |
|                 |                 |                 | empty list.     |
|                 |                 |                 | This overrides  |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | and             |
|                 |                 |                 | *\<xccdf:comple |
|                 |                 |                 | x-value\>*      |
|                 |                 |                 | element(s) of   |
|                 |                 |                 | an              |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element. Like   |
|                 |                 |                 | *\<xccdf:set-va |
|                 |                 |                 | lue\>*,         |
|                 |                 |                 | *\<xccdf:set-co |
|                 |                 |                 | mplex-value\>*  |
|                 |                 |                 | may also be     |
|                 |                 |                 | applied to      |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | elements in a   |
|                 |                 |                 | cluster.        |
+-----------------+-----------------+-----------------+-----------------+
| refine-value    | *special*       | 0-n             | Designates the  |
| (element)       |                 |                 | Value           |
|                 |                 |                 | constraints to  |
|                 |                 |                 | be applied      |
|                 |                 |                 | during          |
|                 |                 |                 | tailoring, for  |
|                 |                 |                 | an              |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element or the  |
|                 |                 |                 | *\<xccdf:Value\ |
|                 |                 |                 | >*              |
|                 |                 |                 | members of a    |
|                 |                 |                 | cluster. The    |
|                 |                 |                 | element may     |
|                 |                 |                 | have one or     |
|                 |                 |                 | more            |
|                 |                 |                 | *\<xccdf:remark |
|                 |                 |                 | \>*             |
|                 |                 |                 | elements        |
|                 |                 |                 | containing      |
|                 |                 |                 | explanatory     |
|                 |                 |                 | material or     |
|                 |                 |                 | other prose.    |
|                 |                 |                 | Each instance   |
|                 |                 |                 | of              |
|                 |                 |                 | *\<xccdf:remark |
|                 |                 |                 | \>*             |
|                 |                 |                 | MAY have an     |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10).        |
|                 |                 |                 | Possible        |
|                 |                 |                 | attributes for  |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:refine |
|                 |                 |                 | -value\>*       |
|                 |                 |                 | element are the |
|                 |                 |                 | id (required)   |
|                 |                 |                 | of a Value or   |
|                 |                 |                 | item cluster,   |
|                 |                 |                 | the id of a     |
|                 |                 |                 | Value selector, |
|                 |                 |                 | and a new       |
|                 |                 |                 | setting for the |
|                 |                 |                 | Value operator. |
|                 |                 |                 | The element     |
|                 |                 |                 | provides a      |
|                 |                 |                 | means for       |
|                 |                 |                 | authors to      |
|                 |                 |                 | impose          |
|                 |                 |                 | different       |
|                 |                 |                 | constraints on  |
|                 |                 |                 | tailoring for   |
|                 |                 |                 | different       |
|                 |                 |                 | profiles.       |
|                 |                 |                 | (Constraints    |
|                 |                 |                 | must be         |
|                 |                 |                 | designated with |
|                 |                 |                 | a selector id.  |
|                 |                 |                 | For example, a  |
|                 |                 |                 | particular      |
|                 |                 |                 | numeric Value   |
|                 |                 |                 | might have      |
|                 |                 |                 | several         |
|                 |                 |                 | different sets  |
|                 |                 |                 | of              |
|                 |                 |                 | *\<xccdf:value\ |
|                 |                 |                 | >,*             |
|                 |                 |                 | *\<xccdf:upper- |
|                 |                 |                 | bound\>,*       |
|                 |                 |                 | and             |
|                 |                 |                 | *\<xccdf:lower- |
|                 |                 |                 | bound\>*        |
|                 |                 |                 | elements,       |
|                 |                 |                 | designated with |
|                 |                 |                 | different       |
|                 |                 |                 | selector ids.   |
|                 |                 |                 | The             |
|                 |                 |                 | *\<xccdf:refine |
|                 |                 |                 | -value\>*       |
|                 |                 |                 | selector tells  |
|                 |                 |                 | benchmark       |
|                 |                 |                 | consumers which |
|                 |                 |                 | value to employ |
|                 |                 |                 | and bounds to   |
|                 |                 |                 | enforce when    |
|                 |                 |                 | that particular |
|                 |                 |                 | profile is in   |
|                 |                 |                 | effect.         |
+-----------------+-----------------+-----------------+-----------------+
| refine-rule     | *special*       | 0-n             | Allows the      |
| (element)       |                 |                 | author to       |
|                 |                 |                 | select check    |
|                 |                 |                 | statements and  |
|                 |                 |                 | override the    |
|                 |                 |                 | *\@weight*,     |
|                 |                 |                 | *\@severity*,   |
|                 |                 |                 | and *\@role* of |
|                 |                 |                 | a Rule, Group,  |
|                 |                 |                 | or cluster of   |
|                 |                 |                 | Rules and       |
|                 |                 |                 | Groups. The     |
|                 |                 |                 | element may     |
|                 |                 |                 | have one or     |
|                 |                 |                 | more            |
|                 |                 |                 | *\<xccdf:remark |
|                 |                 |                 | \>*             |
|                 |                 |                 | elements        |
|                 |                 |                 | containing      |
|                 |                 |                 | explanatory     |
|                 |                 |                 | material or     |
|                 |                 |                 | other prose.    |
|                 |                 |                 | Each instance   |
|                 |                 |                 | of              |
|                 |                 |                 | *\<xccdf:remark |
|                 |                 |                 | \>*             |
|                 |                 |                 | MAY have an     |
|                 |                 |                 | *\@xml:lang*    |
|                 |                 |                 | attribute (see  |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.2.10). The    |
|                 |                 |                 | *\<xccdf:refine |
|                 |                 |                 | -rule\>*        |
|                 |                 |                 | element has an  |
|                 |                 |                 | *\@idref*       |
|                 |                 |                 | attribute       |
|                 |                 |                 | (NCName) that   |
|                 |                 |                 | shall refer to  |
|                 |                 |                 | a Rule, Group,  |
|                 |                 |                 | or cluster.     |
|                 |                 |                 | Despite the     |
|                 |                 |                 | name, this      |
|                 |                 |                 | selector does   |
|                 |                 |                 | apply for       |
|                 |                 |                 | Groups and for  |
|                 |                 |                 | clusters that   |
|                 |                 |                 | include Groups, |
|                 |                 |                 | but it shall    |
|                 |                 |                 | only affect     |
|                 |                 |                 | their           |
|                 |                 |                 | *\@weight*      |
|                 |                 |                 | attribute. If   |
|                 |                 |                 | the selector    |
|                 |                 |                 | specified in an |
|                 |                 |                 | *\<xccdf:refine |
|                 |                 |                 | -rule\>*        |
|                 |                 |                 | element does    |
|                 |                 |                 | not match any   |
|                 |                 |                 | of the          |
|                 |                 |                 | selectors       |
|                 |                 |                 | specified on    |
|                 |                 |                 | any of the      |
|                 |                 |                 | check children  |
|                 |                 |                 | of a Rule, then |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | child element   |
|                 |                 |                 | without a       |
|                 |                 |                 | *\@selector*    |
|                 |                 |                 | attribute must  |
|                 |                 |                 | be used.        |
+-----------------+-----------------+-----------------+-----------------+

The example below illustrates how selectors work with the
*\<xccdf:refine-value\>* element. See Section 6.4.5.5 for more
information on the *\@selector* attribute. In the example, before the
*\<xccdf:refine-value\>* is applied, the effective value is 8 and the
effective lower bound is 8. (These are the two properties that have no
selectors and are therefore the default.) After the
*\<xccdf:refine-value\>* is applied, the effective value is 14 and the
effective lower bound is 12.

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

## \<xccdf:TestResult\> Element

### Basics

An *\<xccdf:TestResult\>* element encapsulates the results of a single
application of a benchmark to a single target platform. The
*\<xccdf:TestResult\>* element normally appears as the child of the
*\<xccdf:Benchmark\>* element, although it may also appear as the
top-level element of an XCCDF results document. XCCDF is not intended to
be a database format for detailed results; the *\<xccdf:TestResult\>*
element offers a way to store the results of individual tests in modest
detail, with the ability to reference lower-level testing data.

Benchmark consumers should include mechanisms so that
*\<xccdf:TestResult\>* elements can retain their context even if
separated from their source benchmark since the *\<xccdf:TestResult\>*
only has meaning relative to the Rules that produced it. There are
several ways to preserve this context, including making sure that the
*\<xccdf:benchmark\>* element in the *\<xccdf:TestResult\>* is a
persistent link to the specific document and version of the benchmark
that produced the result, filling in the relevant version, organization,
and similar fields, and/or using descriptive metadata. This document
does not mandate any specific approach, but benchmark consumers should
ensure some mechanism is in place to avoid context loss.

The example below shows an *\<xccdf:TestResult\>* element with a few
*\<xccdf:rule-result\>* children.

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

### Properties

Table 23 describes the *\<xccdf:TestResult\>* element's properties.
Although several of its child elements technically support the
*\@override* attribute (discussed in Section 6.3.1), the
*\<xccdf:TestResult\>* element cannot be extended. Therefore,
*\@override* has no meaning within an *\<xccdf:TestResult\>* element and
its children, and it should not be used for them.

[]{#_Ref294717473 .anchor}Table : \<xccdf:TestResult\> Element
Properties

  Property                      Type         Count   Description
  ----------------------------- ------------ ------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  benchmark (element)           *special*    0-1     Reference to the *\<xccdf:Benchmark\>* for which the *\<xccdf:TestResult\>* records results. Required if this *\<xccdf:TestResult\>* element is the top-level element, optional otherwise. The required *\@href* attribute gives the URI of the benchmark document, and the optional *\@id* attribute holds the value of that *\<xccdf:Benchmark\>* element\'s *\@id* attribute.
  tailoring-file (element)      *special*    0-1     Holds identifying information about an *\<xccdf:Tailoring\>* element. See Section 6.6.5.
  title (element)               string       0-n     Title of the test. It may have an *\@xml:lang* attribute (see Section 6.2.10).
  remark (element)              string       0-n     A remark about the test, possibly supplied by the person administering the benchmark assessment. It may have an *\@xml:lang* attribute (see Section 6.2.10).
  organization (element)        string       0-n     The name of the organization or other entity responsible for applying this benchmark and generating this result. When multiple organization elements are used to indicate multiple organization names in a hierarchical organization, the highest-level organization should appear first (e.g., "U.S. Government") followed by subordinate organizations (e.g., "Department of Defense").
  identity (element)            string       0-1     Information about the system identity or user employed during application of the benchmark. If used, Shall specify the name of the authenticated identity. Has required boolean attributes that specify whether the identity was authenticated with the target system during the application of the benchmark *(\@authenticated)*, and whether the identity was granted administrative or other special privileges beyond those of a normal user *(\@privileged)*.
  profile (element)             identifier   0-1     The identifier of the *\<xccdf:Profile\>* used for the test. If the *\<xccdf:TestResult\>* element contains an *\<xccdf:tailoring-file\>* element, then this SHALL be the identifier of the associated tailoring profile. Otherwise, if there is no *\<xccdf:tailoring-file\>* element present, this SHALL be the identifier of the associated benchmark profile if a profile was applied, and it SHALL be absent if no profile was applied. See Section 6.6.5.
  target (element)              string       1-n     Name or description of the target system whose test results are recorded in the *\<xccdf:TestResult\>* element (the system to which a benchmark test was applied). Each appearance of the element supplies a name by which the target host or device was identified at the time the test was run. The name may be any string, but applications should include the fully qualified DNS name whenever possible.
  target-address (element)      string       0-n     Network address of the target system to which a benchmark test was applied. Typical forms for the address include IP version 4 (IPv4), IP version 6 (IPv6), and Ethernet media access control (MAC).
  target-facts (element)        string       0-1     A list of named facts about the target system or platform. Each *\<xccdf:fact\>* in the list shall specify the value of the fact itself. Each *\<xccdf:fact\>* SHALL have a *\@name* attribute (URI) and may include a *\@type* attribute ("string", "number", or "boolean") (default: "boolean"). Pre-defined *\@name* attribute values are listed in Table 24; product developers may define additional platform-specific and product-specific facts.
  target-id-ref (element)       *special*    0-n     Used to reference target identification information located in an external document. The assessment target MAY be identified using other identification schemas such as Asset Identification (see \[IR7693\] for additional information). Each instance of this element identifies the same target of this report (multiple identifiers for a single target). Inclusion of an *\<xccdf:target-id-ref\>* element does not remove the requirement to identify the target using the *\<xccdf:target\>* element.
  any                                                Arbitrary contents, namespace="\#\#other". This is for other target identification structures, such as those defined in the Asset Identification schema (see \[IR7693\]). Inclusion of an identifying element does not remove the requirement to identify the target using the *\<xccdf:target\>* element.
  platform (element)            string       0-n     The platforms that the target system was found to meet. See Section 6.2.5.
  set-value (element)           string       0-n     Specific setting for a single *\<xccdf:Value\>* element used during the test.
  set-complex-value (element)   *special*            Specific setting for a single *\<xccdf:Value\>* element used during the test when the given value is set to a complex type, such as a list.
  rule-result (element)         *special*    0-n     The result of a single instance of a rule application against the target. The *\<xccdf:TestResult\>* must include one *\<xccdf:rule-result\>* record for each *\<xccdf:Rule\>* that was selected in the resolved benchmark; it may also include *\<xccdf:rule-result\>* records for *\<xccdf:Rule\>* elements that were unselected in the benchmark. See Section 6.6.4.
  score (element)               decimal      1-n     An overall score for this benchmark test. The optional *\@system* attribute identifies the URI for a scoring model (see Section 7.3.2 for more information on pre-defined models). The optional *\@maximum* attribute specifies the maximum possible value of the score.
  metadata (element)            *special*    0-n     XML metadata associated with this *\<xccdf:TestResult\>*. For example, this element can hold a copy of the *\<xccdf:metadata\>* element of the *\<xccdf:Benchmark\>* which served as the source for these results. This is especially useful if the *\<xccdf:TestResult\>* will be separated from its source benchmark because the publication and support information in the benchmark can travel with the *\<xccdf:TestResult\>*. Benchmark consumers may also add their own metadata to *\<xccdf:TestResult\>* elements they produce. See Section 6.2.3.
  signature (element)           *special*    0-1     A digital signature asserting authorship and allowing verification of the integrity of the *\<xccdf:TestResult\>*. See Section 6.2.7.
  id (attribute)                *special*    1       Unique identifier for this element; see Section 6.2.3.
  start-time (attribute)        dateTime     0-1     Time when test began.
  end-time (attribute)          dateTime     1       Time when test was completed and the results recorded.
  test-system (attribute)       string       0-1     Name of the benchmark consumer program that generated this *\<xccdf:TestResult\>* element; should be either a CPE name or a CPE applicability language expression.
  version (attribute)           string       0-1     The version number string copied from the *\<xccdf:Benchmark\>*.
  Id (attribute)                *special*    0-1     An identifier used for referencing elements included in an XML signature. See Section 6.2.7.

### \<xccdf:fact\> Element

The URIs listed in Table 24 should be used, when applicable to the
target, to record facts about the IT asset to which an
*\<xccdf:TestResult\>* applies.

[]{#_Ref294717488 .anchor}Table : Predefined \@name Attribute Values for
\<xccdf:fact\> Elements

  **URI**                                                                           **Description**
  --------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------
  urn:xccdf:fact:asset:identifier:mac                                               Ethernet media access control address (should be sent as a pair with the IPv4 or IPv6 address to ensure uniqueness)
  urn:xccdf:fact:asset:identifier:ipv4                                              IPv4 address
  urn:xccdf:fact:asset:identifier:ipv6                                              IPv6 address
  urn:xccdf:fact:asset:identifier:host\_name                                        Host name of the asset, if assigned
  urn:xccdf:fact:asset:identifier:fqdn                                              Fully qualified domain name
  urn:xccdf:fact:asset:identifier:ein                                               Equipment identification number or other inventory tag number
  urn:xccdf:fact:asset:identifier:pki:                                              X.509 PKI certificate for the asset (encoded in Base-64)
  urn:xccdf:fact:asset:identifier:pki:thumbprint                                    SHA-1 hash of the PKI certification for the asset (encoded in Base-64)
  urn:xccdf:fact:asset:identifier:guid                                              Globally unique identifier for the asset
  urn:xccdf:fact:asset:identifier:ldap                                              LDAP directory string (distinguished name) of the asset, if assigned
  urn:xccdf:fact:asset:identifier:active\_directory                                 Active Directory realm to which the asset belongs, if assigned
  urn:xccdf:fact:asset:identifier:nis\_domain                                       NIS domain of the asset, if assigned
  urn:xccdf:fact:asset:environmental\_information:owning\_organization              Organization that tracks the asset on its inventory
  urn:xccdf:fact:asset:environmental\_information:current\_region                   Geographic region where the asset is located
  urn:xccdf:fact:asset:environmental\_information:administration\_unit              Name of the organization that does system administration for the asset
  urn:xccdf:fact:asset:environmental\_information:administration\_poc:title         Title (e.g., Mr, Ms, Col) of the system administrator for an asset
  urn:xccdf:fact:asset:environmental\_information:administration\_poc:e-mail        E-mail address of the system administrator for the asset
  urn:xccdf:fact:asset:environmental\_information:administration\_poc:first\_name   First name of the system administrator for the asset
  urn:xccdf:fact:asset:environmental\_information:administration\_poc:last\_name    Last name of the system administrator for the asset

### \<xccdf:rule-result\> Element

#### Properties

The *\<xccdf:rule-result\>* element within the *\<xccdf:TestResult\>*
element holds the result of applying an *\<xccdf:Rule\>* from the
benchmark to a target system or component of a target system. Table 25
describes the *\<xccdf:rule-result\>* element's properties.

[]{#_Ref294717520 .anchor}Table : \<xccdf:rule-result\> Element
Properties

+-----------------+-----------------+-----------------+-----------------+
| Property        | Type            | Count           | Description     |
+=================+=================+=================+=================+
| result          | string          | 1               | Result of       |
| (element)       |                 |                 | applying this   |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | to a target or  |
|                 |                 |                 | target          |
|                 |                 |                 | component: must |
|                 |                 |                 | be set to one   |
|                 |                 |                 | of the values   |
|                 |                 |                 | listed in Table |
|                 |                 |                 | 26.             |
+-----------------+-----------------+-----------------+-----------------+
| override        | *special*       | 0-n             | An XML block    |
| (element)       |                 |                 | explaining how  |
|                 |                 |                 | and why an      |
|                 |                 |                 | auditor chose   |
|                 |                 |                 | to override the |
|                 |                 |                 | result. See     |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.6.4.3.        |
+-----------------+-----------------+-----------------+-----------------+
| ident (element) | string          | 0-n             | A long-term     |
|                 |                 |                 | globally        |
|                 |                 |                 | meaningful      |
|                 |                 |                 | identifier for  |
|                 |                 |                 | the issue,      |
|                 |                 |                 | vulnerability,  |
|                 |                 |                 | platform, etc.  |
|                 |                 |                 | copied from the |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *.              |
|                 |                 |                 | Shall have a    |
|                 |                 |                 | *\@system*      |
|                 |                 |                 | attribute       |
|                 |                 |                 | designating the |
|                 |                 |                 | URI of the      |
|                 |                 |                 | organization or |
|                 |                 |                 | scheme that     |
|                 |                 |                 | assigned the    |
|                 |                 |                 | identifier. An  |
|                 |                 |                 | *\<xccdf:ident\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element MAY     |
|                 |                 |                 | also have other |
|                 |                 |                 | attributes      |
|                 |                 |                 | pulled from     |
|                 |                 |                 | other           |
|                 |                 |                 | namespaces in   |
|                 |                 |                 | order to        |
|                 |                 |                 | provide         |
|                 |                 |                 | additional      |
|                 |                 |                 | metadata for    |
|                 |                 |                 | the given       |
|                 |                 |                 | identifier. See |
|                 |                 |                 | Section 6.4.4.3 |
|                 |                 |                 | for more on     |
|                 |                 |                 | this element.   |
+-----------------+-----------------+-----------------+-----------------+
| metadata        | *special*       | 0-n             | XML metadata    |
| (element)       |                 |                 | associated with |
|                 |                 |                 | this            |
|                 |                 |                 | *\<xccdf:rule-r |
|                 |                 |                 | esult\>*.       |
|                 |                 |                 | For example,    |
|                 |                 |                 | this element    |
|                 |                 |                 | could hold a    |
|                 |                 |                 | copy of the     |
|                 |                 |                 | metadata of the |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | that served as  |
|                 |                 |                 | the source for  |
|                 |                 |                 | these results.  |
|                 |                 |                 | Benchmark       |
|                 |                 |                 | consumers may   |
|                 |                 |                 | also add their  |
|                 |                 |                 | own metadata to |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:rule-r |
|                 |                 |                 | esult\>*        |
|                 |                 |                 | elements they   |
|                 |                 |                 | produce. See    |
|                 |                 |                 | Section 6.2.4   |
|                 |                 |                 | for additional  |
|                 |                 |                 | information.    |
+-----------------+-----------------+-----------------+-----------------+
| message         | string          | 0-n             | Diagnostic      |
| (element)       |                 |                 | messages from   |
|                 |                 |                 | the checking    |
|                 |                 |                 | engine, with a  |
|                 |                 |                 | REQUIRED        |
|                 |                 |                 | *\@severity*    |
|                 |                 |                 | attribute that  |
|                 |                 |                 | denotes the     |
|                 |                 |                 | seriousness or  |
|                 |                 |                 | conditions of   |
|                 |                 |                 | the message.    |
|                 |                 |                 | There are three |
|                 |                 |                 | message         |
|                 |                 |                 | severity        |
|                 |                 |                 | values:         |
|                 |                 |                 | "error",        |
|                 |                 |                 | "warning", and  |
|                 |                 |                 | "info". These   |
|                 |                 |                 | elements shall  |
|                 |                 |                 | not affect      |
|                 |                 |                 | scoring; they   |
|                 |                 |                 | are present     |
|                 |                 |                 | merely to       |
|                 |                 |                 | convey          |
|                 |                 |                 | diagnostic      |
|                 |                 |                 | information     |
|                 |                 |                 | from the        |
|                 |                 |                 | checking        |
|                 |                 |                 | engine.         |
|                 |                 |                 | Benchmark       |
|                 |                 |                 | consumers may   |
|                 |                 |                 | choose to log   |
|                 |                 |                 | these messages  |
|                 |                 |                 | or display them |
|                 |                 |                 | to the user.    |
+-----------------+-----------------+-----------------+-----------------+
| instance        | string          | 0-n             | Name of the     |
| (element)       |                 |                 | target          |
|                 |                 |                 | subsystem or    |
|                 |                 |                 | component to    |
|                 |                 |                 | which this      |
|                 |                 |                 | result applies, |
|                 |                 |                 | for a multiply  |
|                 |                 |                 | instantiated    |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *.              |
|                 |                 |                 | The element is  |
|                 |                 |                 | important for   |
|                 |                 |                 | an              |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | that applies to |
|                 |                 |                 | components of   |
|                 |                 |                 | the target      |
|                 |                 |                 | system,         |
|                 |                 |                 | especially when |
|                 |                 |                 | a target might  |
|                 |                 |                 | have several    |
|                 |                 |                 | such            |
|                 |                 |                 | components, and |
|                 |                 |                 | where the       |
|                 |                 |                 | *\@multiple*    |
|                 |                 |                 | attribute of    |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | is set to true. |
|                 |                 |                 | For example, an |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | might specify a |
|                 |                 |                 | particular      |
|                 |                 |                 | setting that    |
|                 |                 |                 | needs to be     |
|                 |                 |                 | applied on      |
|                 |                 |                 | every interface |
|                 |                 |                 | of a firewall;  |
|                 |                 |                 | for benchmark   |
|                 |                 |                 | results, a      |
|                 |                 |                 | firewall target |
|                 |                 |                 | with three      |
|                 |                 |                 | interfaces      |
|                 |                 |                 | could have      |
|                 |                 |                 | three           |
|                 |                 |                 | *\<xccdf:rule-r |
|                 |                 |                 | esult\>*        |
|                 |                 |                 | elements with   |
|                 |                 |                 | the same rule   |
|                 |                 |                 | id, each with   |
|                 |                 |                 | an independent  |
|                 |                 |                 | value for the   |
|                 |                 |                 | *\<xccdf:result |
|                 |                 |                 | \>*             |
|                 |                 |                 | element. For    |
|                 |                 |                 | more discussion |
|                 |                 |                 | of multiply     |
|                 |                 |                 | instantiated    |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | elements, see   |
|                 |                 |                 | Section         |
|                 |                 |                 | 7.2.3.4.        |
|                 |                 |                 |                 |
|                 |                 |                 | The optional    |
|                 |                 |                 | *\@context* and |
|                 |                 |                 | *\@parentContex |
|                 |                 |                 | t*              |
|                 |                 |                 | attributes      |
|                 |                 |                 | provide context |
|                 |                 |                 | and hierarchy   |
|                 |                 |                 | information for |
|                 |                 |                 | nested          |
|                 |                 |                 | contexts. If    |
|                 |                 |                 | the *\@context* |
|                 |                 |                 | attribute is    |
|                 |                 |                 | omitted, the    |
|                 |                 |                 | value of the    |
|                 |                 |                 | context shall   |
|                 |                 |                 | default to      |
|                 |                 |                 | "undefined". At |
|                 |                 |                 | most one        |
|                 |                 |                 | *\<xccdf:instan |
|                 |                 |                 | ce\>*           |
|                 |                 |                 | child of an     |
|                 |                 |                 | *\<xccdf:rule-r |
|                 |                 |                 | esult\>*        |
|                 |                 |                 | may have a      |
|                 |                 |                 | context of      |
|                 |                 |                 | "undefined".    |
+-----------------+-----------------+-----------------+-----------------+
| fix (element)   | string          | 0-n             | Fix script for  |
|                 |                 |                 | this target     |
|                 |                 |                 | platform, if    |
|                 |                 |                 | available       |
|                 |                 |                 | (would normally |
|                 |                 |                 | appear only for |
|                 |                 |                 | result values   |
|                 |                 |                 | of "fail"). See |
|                 |                 |                 | Table 9. It is  |
|                 |                 |                 | assumed to have |
|                 |                 |                 | been            |
|                 |                 |                 | 'instantiated'  |
|                 |                 |                 | by the testing  |
|                 |                 |                 | tool and any    |
|                 |                 |                 | substitutions   |
|                 |                 |                 | or platform     |
|                 |                 |                 | selection       |
|                 |                 |                 | already made.   |
+-----------------+-----------------+-----------------+-----------------+
| check (element) | *special*       | (1-n instances  | Encapsulated or |
|                 |                 | of check)       | referenced      |
|                 |                 |                 | results to      |
|                 |                 | XOR             | detailed        |
|                 |                 |                 | testing output  |
|                 |                 | (1 instance of  | from the        |
|                 |                 | complex-check)  | checking engine |
|                 |                 |                 | (if any).       |
|                 |                 |                 | Consists of the |
|                 |                 |                 | URI that        |
|                 |                 |                 | designates the  |
|                 |                 |                 | checking        |
|                 |                 |                 | system, and     |
|                 |                 |                 | detailed output |
|                 |                 |                 | data from the   |
|                 |                 |                 | checking        |
|                 |                 |                 | engine. The     |
|                 |                 |                 | detailed output |
|                 |                 |                 | data may take   |
|                 |                 |                 | the form of     |
|                 |                 |                 | encapsulated    |
|                 |                 |                 | XML or text     |
|                 |                 |                 | data, or it may |
|                 |                 |                 | be a reference  |
|                 |                 |                 | to an external  |
|                 |                 |                 | URI. (Note:     |
|                 |                 |                 | this is         |
|                 |                 |                 | analogous to    |
|                 |                 |                 | the form of the |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | element's       |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element, used   |
|                 |                 |                 | for referring   |
|                 |                 |                 | to checking     |
|                 |                 |                 | engine input.)  |
|                 |                 |                 | See Section     |
|                 |                 |                 | 6.4.4.4.        |
+-----------------+-----------------+-----------------+-----------------+
| complex-check   | *special*       |                 | A copy of the   |
| (element)       |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | element's       |
|                 |                 |                 | *\<xccdf:comple |
|                 |                 |                 | x-check\>*      |
|                 |                 |                 | element where   |
|                 |                 |                 | each component  |
|                 |                 |                 | *\<xccdf:check\ |
|                 |                 |                 | >*              |
|                 |                 |                 | element of the  |
|                 |                 |                 | *\<xccdf:comple |
|                 |                 |                 | x-check\>*      |
|                 |                 |                 | element is an   |
|                 |                 |                 | encapsulated or |
|                 |                 |                 | referenced      |
|                 |                 |                 | results to      |
|                 |                 |                 | detailed        |
|                 |                 |                 | testing output  |
|                 |                 |                 | from the        |
|                 |                 |                 | checking engine |
|                 |                 |                 | (if any) as     |
|                 |                 |                 | described       |
|                 |                 |                 | above. See      |
|                 |                 |                 | Section         |
|                 |                 |                 | 6.4.4.4.        |
+-----------------+-----------------+-----------------+-----------------+
| idref           | identifier      | 1               | Identifier of   |
| (attribute)     |                 |                 | an              |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | (from the       |
|                 |                 |                 | benchmark       |
|                 |                 |                 | designated in   |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:TestRe |
|                 |                 |                 | sult\>*).       |
+-----------------+-----------------+-----------------+-----------------+
| role            | string          | 0-1             | The role string |
| (attribute)     |                 |                 | copied from the |
|                 |                 |                 | *\@role*        |
|                 |                 |                 | attribute of    |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *.              |
+-----------------+-----------------+-----------------+-----------------+
| severity        | string          | 0-1             | The             |
| (attribute)     |                 |                 | *\@severity*    |
|                 |                 |                 | attribute value |
|                 |                 |                 | copied from the |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | (default:       |
|                 |                 |                 | "unknown").     |
+-----------------+-----------------+-----------------+-----------------+
| time            | dateTime        | 0-1             | Time when       |
| (attribute)     |                 |                 | application of  |
|                 |                 |                 | this instance   |
|                 |                 |                 | of this         |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *               |
|                 |                 |                 | was completed.  |
+-----------------+-----------------+-----------------+-----------------+
| version         | string          | 0-1             | The version     |
| (attribute)     |                 |                 | number string   |
|                 |                 |                 | copied from the |
|                 |                 |                 | *\<xccdf:versio |
|                 |                 |                 | n\>*            |
|                 |                 |                 | element of the  |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | .*              |
+-----------------+-----------------+-----------------+-----------------+
| weight          | decimal         | 0-1             | The weight      |
| (attribute)     |                 |                 | number copied   |
|                 |                 |                 | from the        |
|                 |                 |                 | *\@weight*      |
|                 |                 |                 | attribute of    |
|                 |                 |                 | the             |
|                 |                 |                 | *\<xccdf:Rule\> |
|                 |                 |                 | *.              |
|                 |                 |                 | Expressed as a  |
|                 |                 |                 | non-negative    |
|                 |                 |                 | real number     |
|                 |                 |                 | (0.0 or         |
|                 |                 |                 | greater,        |
|                 |                 |                 | maximum 3       |
|                 |                 |                 | digits, default |
|                 |                 |                 | 1.0).           |
+-----------------+-----------------+-----------------+-----------------+

#### \<xccdf:result\> Element

Table 26 lists the possible results of a single test (assigned to the
*\<xccdf:result\>* element).

[]{#_Ref294717545 .anchor}Table : Possible Results for a Single Test

  **Result and Abbreviation**   **Description**
  ----------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  pass \[P\]                    The target system or system component satisfied all the conditions of the *\<xccdf:Rule\>*.
  fail \[F\]                    The target system or system component did not satisfy all the conditions of the *\<xccdf:Rule\>*.
  error \[E\]                   The checking engine could not complete the evaluation, therefore the status of the target's compliance with the *\<xccdf:Rule\>* is not certain. This could happen, for example, if a testing tool was run with insufficient privileges and could not gather all of the necessary information.
  unknown \[U\]                 The testing tool encountered some problem and the result is unknown. For example, a result of 'unknown' might be given if the testing tool was unable to interpret the output of the checking engine (the output has no meaning to the testing tool).
  notapplicable \[N\]           The *\<xccdf:Rule\>* was not applicable to the target of the test. For example, the *\<xccdf:Rule\>* might have been specific to a different version of the target OS, or it might have been a test against a platform feature that was not installed.
  notchecked \[K\]              The *\<xccdf:Rule\>* was not evaluated by the checking engine. This status is designed for *\<xccdf:Rule\>* elements that have no *\<xccdf:check\>* elements or that correspond to an unsupported checking system. It may also correspond to a status returned by a checking engine if the checking engine does not support the indicated check code.
  notselected \[S\]             The *\<xccdf:Rule\>* was not selected in the benchmark.
  informational \[I\]           The *\<xccdf:Rule\>* was checked, but the output from the checking engine is simply information for auditors or administrators; it is not a compliance category. This status value is designed for *\<xccdf:Rule\>* elements whose main purpose is to extract information from the target rather than test the target.
  fixed \[X\]                   The *\<xccdf:Rule\>* had failed, but was then fixed (possibly by a tool that can automatically apply remediation, or possibly by the human auditor).

#### \<xccdf:override\> Element

The *\<xccdf:override\>* element provides a mechanism for an auditor to
change the result assigned by the checking tool or to document the
reason for deviation from the rule requirement. This is necessary when
checking a rule requires reviewing manual procedures or other non-IT
conditions, and/or when a check gives an inaccurate result on a
particular target system. The *\<xccdf:override\>* element shall contain
the properties listed in Table 27. Note that the *\<xccdf:override\>*
element is unrelated to the *\@override* attribute (Section 6.3.1). If
the *\<xccdf:override\>* element is present, the *\<xccdf:result\>*
element SHALL be changed to match the *\<xccdf:override\>* element\'s
*\<xccdf:new-result\>* property.

[]{#_Ref294717579 .anchor}Table : \<xccdf:override\> Element Properties

  Property                Type       Count   Description
  ----------------------- ---------- ------- ------------------------------------------------------------------------------------------------------------------------------------
  old-result (element)    string     1       The rule result status before this override
  new-result (element)    string     1       The new, override rule result status
  remark (element)        string     1       Rationale or explanation text for why or how the override was applied. It MAY have an *\@xml:lang* attribute (see Section 6.2.10).
  time (attribute)        dateTime   1       When the override was applied
  authority (attribute)   string     1       Name or other identification for the human principal authorizing the override

The example below shows how an *\<xccdf:override\>* element would appear
in an *\<xccdf:rule-result\>.* Note: if an *\<xccdf:override\>* element
is added to an *\<xccdf:rule-result\>* that was previously signed, it
will break any XML digital signature applied to the enclosing
*\<xccdf:TestResult\>* element.

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

### \<xccdf:tailoring-file\> Element

The *\<xccdf:tailoring-file\>* element in the *\<xccdf:TestResult\>*
element is used to provide information about an *\<xccdf:Tailoring\>*
element. Table 28 shows the properties of an *\<xccdf:tailoring-file\>*
element. The *\<xccdf:tailoring-file\>* element MUST be present if and
only if one of the *\<xccdf:Profile\>* elements of the
*\<xccdf:Tailoring\>* element was applied or created during the
activities that created the given *\<xccdf:TestResult\>* element.
"Applied" refers to applying a profile during benchmark profile
processing, while "created" refers to a user's manual tailoring actions
resulting in the generation of a new *\<xccdf:Tailoring\>* element. (See
Section 6.7 for information on *\<xccdf:Tailoring\>*.)

[]{#_Ref298935137 .anchor}Table : \<xccdf:tailoring-file\> Element
Properties

  Property              Type         Count   Description
  --------------------- ------------ ------- ----------------------------------------------------------
  href (attribute)      URI          1       Persistent location of the *\<xccdf:Tailoring\>* element
  id (attribute)        identifier   1       Identifier for the *\<xccdf:Tailoring\>* element
  version (attribute)   string       1       Version of the *\<xccdf:Tailoring\>* element
  time (attribute)      dateTime     1       Timestamp for the *\<xccdf:Tailoring\>* element

## \<xccdf:Tailoring\> Element

### Basics

An *\<xccdf:Tailoring\>* element allows named tailorings (i.e.,
profiles) of a benchmark to be defined separately from the benchmark
itself. There are several reasons why this might be desirable:

-   The benchmark might not be controlled by the organization wishing to
    add the profile to it.

-   The benchmark might have digital signatures that would be corrupted
    by benchmark modification.

-   The benchmark might undergo revision by its author, so modifications
    by a different party would represent a development fork that
    complicates maintenance.

-   It enables the capturing of manual tailoring actions in a
    well-defined format (see Section 5.2).

The profiles in an *\<xccdf:Tailoring\>* element can be used in two
ways. First, an organization might wish to pre-define a set of tailoring
actions to be applied on top of or instead of the tailoring performed by
a benchmark\'s profiles. This may be necessary to adjust the benchmark
to the local needs of an enterprise. By creating new *\<xccdf:Profile\>*
elements with these tailorings and saving them in the
*\<xccdf:Tailoring\>* element, these can be applied to future
assessments. The *\<xccdf:Tailoring\>* elements also serve as records of
these additional tailoring actions, providing the context needed to
interpret *\<xccdf:TestResult\>* elements.

In addition, an *\<xccdf:Tailoring\>* element can be used to to record
manual tailoring actions performed during the course of an assessment.
Manual tailoring actions SHOULD be limited to actions that could be
performed using selectors in an *\<xccdf:Profile\>* element. If this is
done, any manual tailoring action can be recorded in a series of
selectors in a newly defined *\<xccdf:Profile\>* element. By storing
this *\<xccdf:Profile\>* in an *\<xccdf:Tailoring\>* element, the
processes that led to a particular set of *\<xccdf:TestResult\>*
elements can be saved for future reference. Of course, such an
*\<xccdf:Tailoring\>* element could then be used as input to subsequent
assessments.

### Properties

An *\<xccdf:Tailoring\>* element holds one or more *\<xccdf:Profile\>*
elements. These *\<xccdf:Profile\>* elements record additional tailoring
activities that apply to a given *\<xccdf:Benchmark\>*.
*\<xccdf:Tailoring\>* elements are separate from benchmark documents,
but each *\<xccdf:Tailoring\>* element is associated with a specific
benchmark document. By defining these tailoring actions separately from
the benchmark document to which they apply, these actions can be
recorded without affecting the integrity of the source itself.

Table 29 lists the possible properties for the *\<xccdf:Tailoring\>*
element.

[]{#_Ref298931237 .anchor}Table : \<xccdf:Tailoring\> Element Properties

  --------------------- ----------- ------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Property              Type        Count   Description
  benchmark (element)   *special*   0-1     Identifies the Benchmark to which this tailoring applies. A tailoring document is only applicable to a single Benchmark. The *\<xccdf:benchmark\>* element holds the associated Benchmark\'s location URI, version string, and id value. Note, however, that this is a purely informative field. Benchmark consumers are not required to perform any lookup or even verify that a tailoring document is being applied to the correct Benchmark.
  status (element)      *special*   0-n     Status of the tailoring and date at which it attained that status. Authors MAY use this element to record the maturity or consensus level of a tailoring. See Section 6.2.8.
  dc-status (element)   *special*   0-n     Holds additional status information using the Dublin Core format. See Section 6.2.8.
  version (element)     *special*   1       The version of this tailoring, with a REQUIRED *\@time* attribute (when it was created). This is necessary because, under some circumstances, a copy of a tailoring document might be automatically generated. Without the version and timestamp, tracking of these automatically created tailoring documents could become problematic.
  metadata (element)    *special*   0-n     XML metadata for the tailoring. See Section 6.2.4.
  Profile (element)     Profile     1-n     Profiles that reference and customize sets of items in a Benchmark; see Section 6.5. These elements are identical to Profiles that might be found in a normal Benchmark. Moreover, they can extend the Profiles in the tailoring document\'s associated Benchmark using normal Profile extension mechanics. However, Profiles in the tailoring document cannot themselves be extended. For this reason, no Profile in the tailoring document should have its abstract property set to \"true\".
  signature (element)   *special*   0-1     A digital signature asserting authorship and allowing verification of the integrity of the tailoring. See Section 6.2.7.
  id (attribute)        *special*   1       Unique identifier for this element. See Section 6.2.3.
  Id (attribute)        *special*   0-1     An identifier used for referencing elements included in an XML signature. See Section 6.2.7.
  --------------------- ----------- ------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Profile Shadowing

Profiles in an *\<xccdf:Tailoring\>* element MAY "shadow" profiles in
the associated benchmark document. When a tailoring profile shadows a
benchmark profile, it assumes the identifier of that profile in all
subsequent processing, and the shadowed benchmark profile effectively
becomes invisible. Shadowing occurs when the tailoring profile\'s *\@id*
AND *\@extends* attributes are both identical to the *\@id* attribute of
the benchmark profile. Note that a tailoring profile MAY extend a
benchmark profile without shadowing it; the tailoring profile would
simply use an identifier different from the identifier of the benchmark
profile. However, a tailoring profile\'s *\@id* attribute SHALL NOT
duplicate a benchmark profile\'s id unless the tailoring profile is
extending the benchmark profile.

Table 30 summarizes this behavior. In this table, consider an
*\<xccdf:Tailoring\>* element with a single *\<xccdf:Profile\>* whose
*\@id* and *\@extends* attributes have the given values. This
*\<xccdf:Tailoring\>* element is associated with an
*\<xccdf:Benchmark\>* that likewise has an *\<xccdf:Profile\>* whose
*\@id* attribute has the given value.

[]{#_Ref298933911 .anchor}Table : Profile Shadowing Behavior

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Tailoring\   Benchmark Profile   Behavior   
  Profile                                     
  ------------ ------------------- ---------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  id           extends             id         

  A            A                   A          The tailoring profile shadows the benchmark profile. If profile \"A\" is selected and applied, it will be the profile of that name as defined in the *\<xccdf:Tailoring\>* element.

  B            A                   A          The tailoring profile extends but does not shadow the benchmark profile. If profile \"A\" is applied, it is the benchmark profile of that name that is applied. If profile \"B\" is applied, it is the tailoring profile of that name that is applied.

  A            B                   A          An error is thrown during processing: the tailoring profile identifier duplicates the identifier of a benchmark profile without extending that profile.
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Tailoring Actions and Profile Selectors

As mentioned earlier, a tailoring profile can be used to record manual
tailoring actions and to serve as a record of these actions when
evaluating a given *\<xccdf:TestResult\>*. In such a case, the following
user tailoring actions are represented by the profile selectors
designated in Table 31.

[]{#_Ref298933736 .anchor}Table : Tailoring Actions and Profile
Selectors

  Action                                                      Selector
  ----------------------------------------------------------- -------------------------------
  User selects or de-selects a Rule                           *\<xccdf:select\>*
  User provides a value for use with a given Value            *\<xccdf:set-value\>*
  User provides a list of values for use with a given Value   *\<xccdf:set-complex-value\>*
  User switches the check that a Rule is to perform           *\<xccdf:refine-rule\>*
  User changes the weight of a Rule or Group                  *\<xccdf:refine-rule\>*

