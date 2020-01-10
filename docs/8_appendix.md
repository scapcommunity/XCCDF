###### Converting XCCDF 1.1.4 Content to XCCDF 1.2

XCCDF 1.2 contains several changes that are not transparently backwards
compatible with XCCDF 1.1.4 content. This said, converting content
between the two versions can be done easily. This appendix notes changes
that prevent transparent backwards compatibility and describes
procedures to convert content from the older version to the newer one.
See Appendix B for a more comprehensive list of changes from XCCDF 1.1.4
to XCCDF 1.2.

####### Changes to the XCCDF XML Namespace

The XML namespace of XCCDF changed from
\"http://checklists.nist.gov/xccdf/1.1\" in XCCDF 1.1.4 to
\"http://checklists.nist.gov/xccdf/1.2\" in XCCDF 1.2. All XCCDF 1.1.4
content that needs to be validated against the XCCDF 1.2 schema must use
the XCCDF 1.2 namespace.

####### Conversion of Identifiers

XCCDF 1.2 enforces a canonical format for the *\@id* attributes
(identifiers) of all major XCCDF elements: *\<xccdf:Benchmark\>*,
*\<xccdf:Rule\>*, *\<xccdf:Group\>*, *\<xccdf:Value\>*,
*\<xccdf:Profile\>*, *\<xccdf:TestResult\>*, and *\<xccdf:Tailoring\>*.
As such, the values of *\@id* attributes in XCCDF 1.1.4 are unlikely to
be compliant with this new format. Fortunately, conversion from XCCDF
1.1.4 identifiers to XCCDF 1.2 identifiers is simple and mechanical
using the following steps:

1.  Specify a reverse-DNS style namespace (e.g., com.company or
    gov.agency), denoted as N

2.  For each major XCCDF element (as listed above)

    a.  Denote the type T as the name of that type of element expressed
        > in all lower case (i.e., benchmark, rule, group, value,
        > profile, testresult)

    b.  Denote the XCCDF 1.1.4 id of that element as I

    c.  The new id value of that element becomes: xccdf\_N\_T\_I

This procedure allows any XCCDF 1.1.4 identifier to be replaced with a
recognizably similar identifier value (since the old identifier value
becomes part of the new identifier value) that complies with the new
restrictions imposed by XCCDF 1.2. Note that references to identifiers
will also need to be updated to match the changed identifier values.

####### Conversion of \<xccdf:sub\> Elements

XCCDF 1.2 gives authors a greater degree of control of how
*\<xccdf:sub\>* elements get replaced during text substitution. In
previous versions, when an *\<xccdf:sub\>* element referenced an
*\<xccdf:Value\>* element, either the *\<xccdf:Value\>* element\'s title
or currently-selected value would be substituted for the *\<xccdf:sub\>*
element, depending on the processing model. In XCCDF 1.2, authors can
use the *\<xccdf:sub\>* element\'s *\@use* attribute to control
substitution regardless of the processing model.

In XCCDF 1.2 the default value of the *\@use* attribute is \"value\",
which causes the referenced *\<xccdf:Value\>* element\'s
currently-selected value to be inserted during text substitution. In all
legacy content, which would not have a *\@use* attribute and would
therefore use this default, this would represent a change in behavior.
To ensure that documents converted from XCCDF 1.1.4 to XCCDF 1.2
continue to have the same text substitution processing as before, every
*\<xccdf:sub\>* element in the resulting XCCDF 1.2 document should be
given a *\@use* attribute with a value of \"legacy\". The \"legacy\"
setting indicates that substitution processing should be performed in
the context-dependent manner employed by XCCDF 1.1.4 and before.

####### Properties Removed or Deprecated Since XCCDF 1.1.4

Several properties of *\<xccdf:Benchmark\>*, *\<xccdf:Rule\>*, and
*\<xccdf:Group\>* elements were removed or deprecated between XCCDF
1.1.4 and XCCDF 1.2. Table 42 identifies these properties, explains the
rationale behind their removal or deprecation, and identifies
alternative operations that subsume their capabilities. Converting
content from XCCDF 1.1.4 to XCCDF 1.2 should make use of these
alternative operations.

[]{#_Ref299288349 .anchor}Table : Alternative Operations for Removed and
Deprecated XCCDF 1.1.4 Constructs

+---------------------+------------------------+------------------------------------------+
| Parent Element      | Property Name          | Rationale and Alternatives               |
+=====================+========================+==========================================+
| Benchmark           | platform-definitions   | All three of these properties were used  |
|                     |                        | to define sets of platforms to which a   |
|                     |                        | benchmark might apply. All used schemas  | 
+---------------------+------------------------+------------------------------------------+          
| Group               | extends, abstract      | Group extension has been deprecated in   |
|                     |                        | XCCDF 1.2 because it was shown to have   |
|                     |                        | multiple issues that make it unlikely    |
|                     |                        | for content that employs this feature to |
|                     |                        | be interoperable across XCCDF-compliant  |
|                     |                        | tools. When dealing with XCCDF 1.1.4     |
|                     |                        | content that employs group extension,    |
|                     |                        | the groups should be fully resolved when |
|                     |                        | converting to XCCDF 1.2. This is to say, |
|                     |                        | the act of creating the extended groups  |
|                     |                        | should be performed and completed, and   |
|                     |                        | the result then becomes the groups of    |
|                     |                        | the XCCDF 1.2 document.                  |
+---------------------+------------------------+------------------------------------------+
| Rule                | impact-metric          |  The impact-metric element was found to  |
|                     |                        | have little use in standard XCCDF use    |
|                     |                        | cases, so it has been deprecated.        |
|                     |                        | Content stored in the impact-metric      |
|                     |                        | element in XCCDF 1.1.4 content should be |
|                     |                        | copied to an \<impact-metric\> element   |
|                     |                        | within the corresponding Rule\'s         |
|                     |                        | metadata to preserve it when converting  |
|                     |                        | to XCCDF 1.2.                            |
+---------------------+------------------------+------------------------------------------+

###### Change Log

**[Release 0 -- 1 February 2008]{.underline}**

-   The specification for XCCDF 1.1.4 was released as final.

**[Release 1 -- 29 July 2010 (Initial public comment
draft)]{.underline}**

> **[Functional Additions/Changes:]{.underline}**

-   The flexibility of the metadata field was greatly expanded and
    metadata fields were added to all major XCCDF structures.

-   The dc-status field was added to several major XCCDF structures to
    store status information using Dublin Core Elements 1.1 metadata.

-   Results of checks can be negated.

-   XCCDF 1.2 adds the concept of a complex value capable of holding
    lists.

-   The processing of Profile selectors explicitly permits selectors to
    have overlapping scopes.

-   The XCCDF specification defines some sample classes to support
    stylistic labeling of XHTML content.

-   The use of the check-import element was clarified and the
    import-xpath attribute was added to better support import of XML
    structures from checking systems.

> **[Deprecations/Removals:]{.underline}**

-   The impact-metric element in Rules and the role attribute in Rules
    and rule-results were deprecated.

-   Group extension (abstract and extends attributes) was deprecated.

**[Release 2 -- 27 July 2011 (Second public comment
draft)]{.underline}**

> **[Editorial Changes:]{.underline}**

-   The document was completely reorganized (see Table 43 below for
    mappings from the structure of the previous releases to this draft).

-   The document was thoroughly edited. RFC 2119 language (SHALL,
    SHOULD, MAY, etc.) was added to explicitly declare requirements and
    recommendations for XCCDF documents and products.

-   The document has a short glossary of key terms.

> **[Functional Additions:]{.underline}**

-   There is a new section that explicitly defines high-level XCCDF
    conformance requirements for products and documents.

-   This draft introduces the concept of a tailoring document. The
    schema now has a top-level Tailoring element, as well as a
    tailoring-file element within the TestResult element. Also, a
    Benchmark.ManualTailoring sub-step was added to the benchmark
    processing algorithm.

-   There is a new appendix that explains how to convert XCCDF 1.1.4
    content to XCCDF 1.2.

-   The multi-check attribute was added to the Rule's check element, to
    be used to drive result reporting behavior when multiple checks are
    executed to determine compliance with a single Rule.

> **[Functional Changes:]{.underline}**

-   The XCCDF namespace has been changed from
    "http://checklists.nist.gov/xccdf/1.1" to
    "http://checklists.nist.gov/xccdf/1.2". XCCDF 1.2 is no longer
    backwards compatible with XCCDF 1.1.4.

-   Use of Common Platform Enumeration (CPE) version 2.3 for platform
    specification is required. Formatted string bindings are required
    for CPE names and applicability language expressions in XCCDF
    documents.

-   The id attribute of Benchmark, Rule, Group, Value, Profile,
    TestResult, and Tailoring elements has a mandatory standard format
    intended to enable global uniqueness of identifiers.

-   The TestResult element supports referencing asset identification
    information located in an external document.

-   The pre-defined scoring models have been modified to compute scores
    per Rule rather than per rule-result.

-   Complex-values support zero-length lists.

> **[Deprecations/Removals:]{.underline}**

-   The following platform-related properties deprecated in earlier
    versions of XCCDF have been removed from version 1.2: cpe-list,
    platform-definitions, and Platform-Specification.

[]{#_Ref299287570 .anchor}Table : Mapping Previous Release Sections to
This Release

+-------------------------------------------------+--------------------------------------------------------+
| Release 0 (XCCDF 1.1.4 Final) or\               | Release 2 (Second Public Comment Draft)                |
|  Release 1 (Initial Public Comment Draft)       |                                                        |
+=================================================+========================================================+
| 1.1 Background                                  | 5.1                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| 1.2 Vision for Use                              | 5.1                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| 1.3 Summary of Changes since Version 1.0        | Appendix B (also, deleted all changes pertaining to    |
|                                                 | previous XCCDF versions)                               |
+-------------------------------------------------+--------------------------------------------------------+
| 2\. Requirements                                | Dropped                                                |
+-------------------------------------------------+--------------------------------------------------------+
| 2.1 Structure and Tailoring Requirements        | 5.2                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| 2.2 Inheritance and Inclusion Requirements      | 5.2                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| 2.3 Document and Report Formatting Requirements | 6.2.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| 2.4 Rule Checking Requirements                  | 6.4.4.1                                                |
+-------------------------------------------------+--------------------------------------------------------+
| 2.5 Test Results Requirements                   | 5.3                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| 2.6 Metadata and Security Requirements          | 6.2.7 (signatures), 6.2.4 (metadata), rest deleted     |
+-------------------------------------------------+--------------------------------------------------------+
| 3\. Data Model                                  | 6.1 (object data types), 6.4.5.1 (Value), 6.3.1        |
|                                                 | (scoring weight), data model figure deleted            |
+-------------------------------------------------+--------------------------------------------------------+
| 3.1 Benchmark Structure                         | 6.3.1                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| 3.2 Object Content Details                      | 6.2.2 (type conventions), 6.3.2 (table, count          |
|                                                 | conventions)                                           |
+-------------------------------------------------+--------------------------------------------------------+
| Benchmark                                       | 6.3.2 (Benchmark table), 6.2.8 (status, dc-status),    |
|                                                 | 6.2.2 (HTML markup fields, classifiers), 6.2.5 (CPE    |
|                                                 | names), 6.2.9 (plain-text), 6.2.4 (metadata), 6.3.2    |
|                                                 | (style, style-href), 6.2.7 (signature)                 |
+-------------------------------------------------+--------------------------------------------------------+
| Item                                            | 6.4.1 (Item table), 6.2.8 (status, dc-status), 6.4.1   |
|                                                 | (hidden, cluster-id)                                   |
+-------------------------------------------------+--------------------------------------------------------+
| Group                                           | 6.4.1 and 6.4.3 (Group table), 7.2.3.3.2 (requires and |
|                                                 | conflicts), 6.2.5 (platform), 6.4.1 (weight)           |
+-------------------------------------------------+--------------------------------------------------------+
| Rule                                            | 6.4.1 and 6.4.4.2 (Rule table) 6.3.1 and 7.2.2         |
|                                                 | (extension), 6.4.4.2 (multiple), 6.4.4.4               |
|                                                 | (multi-check), 6.2.5 (platform), 6.4.4.2 (ident),      |
|                                                 | 6.4.4.4 (check), 6.4.4.5 (fixtext, fix)                |
+-------------------------------------------------+--------------------------------------------------------+
| Value                                           | 6.4.5.2 (Value table), 6.4.5.1 (Value types), 6.4.5.2  |
|                                                 | and 6.3.1 (type, default, abstract, extends), 6.4.5.4  |
|                                                 | (operator), 6.4.5.5 (selectors), 6.4.5.2 (upper-bound, |
|                                                 | lower-bound), 6.4.5.3 (choices), 6.4.5.2 (match),      |
|                                                 | 6.4.1 (prohibitChanges, hidden), 6.4.5.2 (interactive, |
|                                                 | interfaceHint, source)                                 |
+-------------------------------------------------+--------------------------------------------------------+
| Profile                                         | 6.5.2 (Profile table), 6.5.1 (definition, extension),  |
|                                                 | 6.5.2 (note-tag), 6.2.8 (status, dc-status), 6.5.3     |
|                                                 | (selectors)                                            |
+-------------------------------------------------+--------------------------------------------------------+
| TestResult                                      | 6.6.2 and 6.6.1                                        |
+-------------------------------------------------+--------------------------------------------------------+
| rule-result                                     | 6.6.4.1 (rule-result Table), 6.6.4.2 (test results),   |
|                                                 | 6.6.4.1 (instance, metadata, check), 6.6.4.3           |
|                                                 | (override)                                             |
+-------------------------------------------------+--------------------------------------------------------+
| 3.3 Processing Models                           | 7.1, dropped Transformation and Test Report Generation |
+-------------------------------------------------+--------------------------------------------------------+
| Loading Processing Sequence                     | 7.2.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| Benchmark Processing Algorithm                  | 7.2.3.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| Item Processing Algorithm                       | 7.2.3.3.1, 7.2.3.3.2                                   |
+-------------------------------------------------+--------------------------------------------------------+
| Profile Selector Processing                     | 7.2.3.4                                                |
+-------------------------------------------------+--------------------------------------------------------+
| Substitution Processing                         | 7.2.3.6.3                                              |
+-------------------------------------------------+--------------------------------------------------------+
| Rule Application and Compliance Scoring         | 7.3.1                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| Rule Check Processing                           | 7.2.3.5.1                                              |
+-------------------------------------------------+--------------------------------------------------------+
| Multiply-Instantiated Rules                     | 7.2.3.5.2                                              |
+-------------------------------------------------+--------------------------------------------------------+
| Scoring and Results Model                       | 7.3.2, 7.3.3                                           |
+-------------------------------------------------+--------------------------------------------------------+
| Score Computation Algorithms                    | 7.3.3                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| 4\. XML Representation                          | N/A                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| 4.1 XML Document General Considerations         | 6.3.1, 6.2.1, 7.2.3.6.1 (XHTML requirements)           |
+-------------------------------------------------+--------------------------------------------------------+
| 4.2 XML Element Dictionary                      | N/A                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| \<Benchmark\>                                   | 6.3.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<Group\>                                       | 6.4.1, 6.4.3                                           |
+-------------------------------------------------+--------------------------------------------------------+
| \<Profile\>                                     | 6.5.1, 6.5.2                                           |
+-------------------------------------------------+--------------------------------------------------------+
| \<Rule\>                                        | 6.4.1, 6.4.4.1, 6.4.4.2                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<TestResult\>                                  | 6.6.2, 6.6.1                                           |
+-------------------------------------------------+--------------------------------------------------------+
| \<Value\>                                       | 6.4.1, 6.4.5.1, 6.4.5.2                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<benchmark\>                                   | 6.6.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<check\>                                       | 6.4.4.4, 6.4.4.2                                       |
+-------------------------------------------------+--------------------------------------------------------+
| \<check-import\>                                | 6.4.4.4                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<check-export\>                                | 6.4.4.4                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<check-content\>                               | 6.4.4.4                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<check-content-ref\>                           | 6.4.4.4                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<choices\>                                     | 6.4.5.3                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<choice\>                                      | 6.4.5.3                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<complex-check\>                               | 6.4.4.4                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<complex-choice\>                              | 6.4.5.3                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<complex-default\>                             | 6.4.5.3, 6.4.5.5                                       |
+-------------------------------------------------+--------------------------------------------------------+
| \<complex-value\>                               | 6.4.5.3, 6.4.5.5                                       |
+-------------------------------------------------+--------------------------------------------------------+
| \<conflicts\>                                   | 6.4.1                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<cpe-list\>                                    | Appendix A                                             |
+-------------------------------------------------+--------------------------------------------------------+
| \<dc-status\>                                   | 6.2.8                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<default\>                                     | 6.4.5.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<description\>                                 | Section 6 (several places), 6.2.9 (sub)                |
+-------------------------------------------------+--------------------------------------------------------+
| \<external-type\>                               | Removed from XCCDF                                     |
+-------------------------------------------------+--------------------------------------------------------+
| \<fact\>                                        | 6.6.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<fix\>                                         | 6.4.4.5, 6.6.4.1 (child of rule-result)                |
+-------------------------------------------------+--------------------------------------------------------+
| \<fixtext\>                                     | 6.4.4.5                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<front-matter\>                                | 6.3.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<ident\>                                       | 6.4.4.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<identity\>                                    | 6.6.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<impact-metric\>                               | 6.4.4.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<instance\>                                    | 6.6.4.1 (child of rule-result), 6.4.4.5 (child of fix) |
+-------------------------------------------------+--------------------------------------------------------+
| \<item\>                                        | N/A                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| \<lower-bound\>                                 | 6.4.5.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<match\>                                       | 6.4.5.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<message\>                                     | 6.6.4.1                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<metadata\>                                    | 6.2.4                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<model\>                                       | 6.3.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<new-result\>                                  | 6.6.4.3                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<notice\>                                      | 6.3.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<old-result\>                                  | 6.6.4.3                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<organization\>                                | 6.6.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<override\>                                    | 6.6.4.3                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<param\>                                       | 6.3.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<plain-text\>                                  | 6.3.2, 6.2.9                                           |
+-------------------------------------------------+--------------------------------------------------------+
| \<platform\>                                    | 6.2.5                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<platform-specification\>                      | Appendix A                                             |
+-------------------------------------------------+--------------------------------------------------------+
| \<platform-definitions\>,                       | Appendix A                                             |
| \<Platform-Specification\>                      |                                                        |
+-------------------------------------------------+--------------------------------------------------------+
| \<profile\>                                     | 6.6.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<profile-note\>                                | 6.4.4.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<question\>                                    | 6.4.1                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<rationale\>                                   | 6.4.1                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<rear-matter\>                                 | 6.3.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<reference\>                                   | 6.2.6                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<refine-rule\>                                 | 6.5.2, 6.5.3                                           |
+-------------------------------------------------+--------------------------------------------------------+
| \<refine-value\>                                | 6.2.3, 6.5.3                                           |
+-------------------------------------------------+--------------------------------------------------------+
| \<remark\>                                      | Section 6 (several places)                             |
+-------------------------------------------------+--------------------------------------------------------+
| \<requires\>                                    | 6.4.1                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<result\>                                      | 6.6.4.1, 6.6.4.2                                       |
+-------------------------------------------------+--------------------------------------------------------+
| \<rule-result\>                                 | 6.6.4.1                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<score\>                                       | 6.6.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<select\>                                      | 6.5.2, 6.5.3                                           |
+-------------------------------------------------+--------------------------------------------------------+
| \<set-complex-value\>                           | 6.5.2, 6.5.3                                           |
+-------------------------------------------------+--------------------------------------------------------+
| \<set-value\>                                   | 6.5.2, 6.5.3                                           |
+-------------------------------------------------+--------------------------------------------------------+
| \<signature\>                                   | 6.2.7                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<source\>                                      | 6.4.5.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<status\>                                      | 6.2.8                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<sub\>                                         | 6.2.9                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<target\>                                      | 6.6.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<target-address\>                              | 6.6.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<target-facts\>                                | 6.6.2                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| \<title\>                                       | Section 6 (several places)                             |
+-------------------------------------------------+--------------------------------------------------------+
| \<upper-bound\>                                 | 6.4.5.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<value\>                                       | 6.4.5.2                                                |
+-------------------------------------------------+--------------------------------------------------------+
| \<version\>                                     | Section 6 (several places)                             |
+-------------------------------------------------+--------------------------------------------------------+
| \<warning\>                                     | 6.4.1, 6.4.2                                           |
+-------------------------------------------------+--------------------------------------------------------+
| 4.3 Handling Text and String Content            | N/A                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| XHTML Formatting and Locale                     | 7.2.3.6.1, 6.2.2, 6.2.10                               |
+-------------------------------------------------+--------------------------------------------------------+
| String Substitution and Reference Processing    | 7.2.3.6.3, 7.2.3.6.4                                   |
+-------------------------------------------------+--------------------------------------------------------+
| 5\. Conclusions                                 | N/A                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| Appendix A. XCCDF Schema                        | Removed from specification, posted separately as .xsd  |
|                                                 | file                                                   |
+-------------------------------------------------+--------------------------------------------------------+
| Appendix B. Sample Benchmark File               | Dropped                                                |
+-------------------------------------------------+--------------------------------------------------------+
| Appendix C. Pre-Defined URIs                    | N/A                                                    |
+-------------------------------------------------+--------------------------------------------------------+
| Long-Term Identification Systems                | 6.4.4.3                                                |
+-------------------------------------------------+--------------------------------------------------------+
| Check Systems                                   | 6.4.4.4                                                |
+-------------------------------------------------+--------------------------------------------------------+
| Scoring Models                                  | 7.3.3                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| Target Platform Facts                           | 6.6.3                                                  |
+-------------------------------------------------+--------------------------------------------------------+
| Remediation Systems                             | 6.4.4.5                                                |
+-------------------------------------------------+--------------------------------------------------------+
| Appendix D. References                          | 2                                                      |
+-------------------------------------------------+--------------------------------------------------------+
| Appendix E. Acronym List                        | 3.2                                                    |
+-------------------------------------------------+--------------------------------------------------------+  

**[Release 3 -- 29 September 2011]{.underline}**

> **[Editorial Changes:]{.underline}**

-   Minor editorial changes were made throughout the publication.

-   An example of using the requires and conflicts elements was added.

> **[Functional Additions:]{.underline}**

-   A use attribute was added to the sub element, to distinguish between
    replacement with values and titles during substitution processing.

> **[Functional Changes:]{.underline}**

-   The requirements and recommendations related to limits on the use of
    metadata were clarified.

-   The requirements for CPE version 2.3 names were changed; formatted
    string bindings are recommended for CPE names and applicability
    language expressions in XCCDF documents, but URI bindings may be
    used instead of formatted string bindings.

-   Multiple instances of the dc-status element are allowed in all
    locations, instead of just one instance.

-   The ident property can contain attributes from external namespaces.

> **[Deprecations/Removals:]{.underline}**

-   The role attribute in Rules and rule-results, which had been
    deprecated in an earlier draft, was restored.

**[Release - November 2011]{.underline}**

> **[Editorial Changes:]{.underline}**

-   Typos were fixed in several tables correcting incorrect assertions
    about the presence or requirement of certain attributes and child
    elements

-   Several element types that were previously defined inline were split
    out into their own global types, bringing them into alignment with
    prior conventions in the XCCDF schema.

-   Annotations in the XCCDF schema were extensively revised and
    expanded to better reflect the information present in the revised
    specification

> **[Deprecations/Removals:]{.underline}**

-   The profileIdKeyRef keyref constraint in the Benchmark element was
    removed. This keyref constraint required that profile references in
    a TestResult to match the id of a Profile in a Benchmark and was
    causing problems when the Profile existed only in a Tailoring file.

-   The overridableIdrefType complex type was removed from the schema
    since it proved to be unreferenced.

-   The import statement for the Dublin Core schema was removed as it
    was no longer necessary in the XCCDF schema.

[^1]: See <http://www.w3.org/TR/xml/#sec-lang-tag> for additional
    information on the \@xml:lang attribute.

[^2]: See <http://www.w3.org/TR/xpath/#axes> for the XPath definition of
    the term "ancestor".
