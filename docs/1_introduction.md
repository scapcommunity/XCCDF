NIST Interagency Report 7275

Revision 4

 ![](NISTIR-7275r4-updated-20120315_html_ba75ffc4034a1370.png)
 ![](NISTIR-7275r4-updated-20120315_html_115eef49b1bf5a41.gif) 

# Specification for the Extensible Configuration Checklist Description Format (XCCDF) Version 1.2**
___

### David Waltermire
### Charles Schmidt
### Karen Scarfone
### Neal Ziring

**NIST Interagency Report 7275
 Revision 4**

Specification for the Extensible Configuration Checklist Description Format (XCCDF) Version 1.2

David Waltermire

Charles Schmidt

Karen Scarfone

Neal Ziring

### COMPUTER SECURITY

Computer Security Division

Information Technology Laboratory

National Institute of Standards and Technology

Gaithersburg, MD 20899-8930

March 2012

![](NISTIR-7275r4-updated-20120315_html_d2db93b4f80a7a37.gif)

U.S. Department of Commerce

**John E. Bryson, Secretary**

National Institute of Standards and Technology

**Patrick D. Gallagher, Under Secretary for Standards and Technology and Director**

**Reports on Computer Systems Technology**

The Information Technology Laboratory (ITL) at the National Institute of Standards and Technology (NIST) promotes the U.S. economy and public welfare by providing technical leadership for the nation's measurement and standards infrastructure. ITL develops tests, test methods, reference data, proof of concept implementations, and technical analysis to advance the development and productive use of information technology. ITL's responsibilities include the development of technical, physical, administrative, and management standards and guidelines for the cost-effective security and privacy of sensitive unclassified information in Federal computer systems. This Interagency Report discusses ITL's research, guidance, and outreach efforts in computer security and its collaborative activities with industry, government, and academic organizations.

**National Institute of Standards and Technology Interagency Report 7275 Revision 4**

**80 pages (Mar. 2012)**

Certain commercial entities, equipment, or materials may be identified in this document in order to describe an experimental procedure or concept adequately. Such identification is not intended to imply recommendation or endorsement by the National Institute of Standards and Technology, nor is it intended to imply that the entities, materials, or equipment are necessarily the best available for the purpose.

Acknowledgments

The authors of this report, David Waltermire of the National Institute of Standards and Technology (NIST), Charles Schmidt of The MITRE Corporation,Karen Scarfone of Scarfone Cybersecurity, and Neal Ziring of the National Security Agency (NSA), wish to thank all contributors to this revision of the publication, particularly Adam Halbardier of Booz Allen Hamilton, Vladimir Giszpenc, Kent Landfield and Richard Whitehurst of McAfee, Lisa Nordman of The MITRE Corporation, Joe Wolfkiel of DISA, and Shane Shaffer and Matt Kerr of G2, Inc.

The authors would also like to acknowledge the following individuals who contributed to the initial definition and development of the Extensible Configuration Checklist Description Format (XCCDF): David Proulx, Mike Michnikov, Andrew Buttner, Todd Wittbold, Adam Compton, George Jones, Chris Calabrese, John Banghart, Murugiah Souppaya, John Wack, Trent Pitsenbarger, and Robert Stafford. Stephen D. Quinn, Peter Mell, and Matthew Wojcik contributed to Revisions 1, 2, and 3 of this report. Ryan Wilson of Georgia Institute of Technology also made substantial contributions. Thanks also go to the Defense Information Systems Agency (DISA) Field Security Office (FSO) Vulnerability Management System (VMS)/Gold Disk team for extensive review and many suggestions.

Abstract

This report specifies the data model and Extensible Markup Language (XML) representation for the Extensible Configuration Checklist Description Format (XCCDF) Version 1.2. An XCCDF document is a structured collection of security configuration rules for some set of target systems. The XCCDF specification is designed to support information interchange, document generation, organizational and situational tailoring, automated compliance testing, and scoring. The specification also defines a data model and format for storing results of security guidance or checklist testing. The intent of XCCDF is to provide a uniform foundation for expression of security checklists and other configuration guidance, and thereby foster more widespread application of good security practices.

Audience

The primary audience of the XCCDF specification is government and industry security analysts, and security management product developers.

Trademark Information

All names are registered trademarks or trademarks of their respective companies.

## Contents

**1. Introduction 1**

1.1 Purpose and Scope 1

1.2 Document Structure 1

1.3 Document Conventions 1

**2. Normative References 2**

**3. Terms, Definitions, and Abbreviations 3**

3.1 XCCDF Terminology 3

3.2 Acronyms and Abbreviations 3

**4. Conformance 4**

4.1 Product Conformance 4

4.2 Benchmark Document Conformance 4

**5. XCCDF Overview 5**

5.1 Introduction 5

5.2 Checklist Structure and Tailoring 6

5.3 Test Results 7

**6. XCCDF Data Model 8**

6.1 Introduction 8

6.2 General XML Information 9

_6.2.1 XCCDF Namespace and XML Schema 9_

_6.2.2 Element and Attribute Formatting 9_

_6.2.3 Element Identifiers 10_

_6.2.4 \<xccdf:metadata\> Element 10_

_6.2.5 Platform Names 11_

_6.2.6 \<xccdf:reference\> Element 12_

_6.2.7 \<xccdf:signature\> Element 13_

_6.2.8 Status Tracking 13_

_6.2.9 Text Substitution 13_

_6.2.10 @xml:lang Attribute 14_

6.3 \<xccdf:Benchmark\> 15

_6.3.1 Basics 15_

_6.3.2 Properties 16_

6.4 Item Elements 18

_6.4.1 Properties 18_

_6.4.2 \<xccdf:warning\> Element 20_

_6.4.3 \<xccdf:Group\> Element 21_

_6.4.4 \<xccdf:Rule\> Element 21_

_6.4.5 \<xccdf:Value\> Element 30_

6.5 \<xccdf:Profile\> Element 34

_6.5.1 Basics 34_

_6.5.2 Properties 34_

_6.5.3 Selectors 36_

6.6 \<xccdf:TestResult\> Element 37

_6.6.1 Basics 37_

_6.6.2 Properties 38_

_6.6.3 \<xccdf:fact\> Element 41_

_6.6.4 \<xccdf:rule-result\> Element 41_

_6.6.5 \<xccdf:tailoring-file\> Element 44_

6.7 \<xccdf:Tailoring\> Element 45

_6.7.1 Basics 45_

_6.7.2 Properties 46_

_6.7.3 Profile Shadowing 46_

_6.7.4 Tailoring Actions and Profile Selectors 47_

**7. XCCDF Processing 48**

7.1 Introduction 48

7.2 Loading and Traversal 48

_7.2.1 Introduction 48_

_7.2.2 Loading 48_

_7.2.3 Traversal 51_

7.3 Assessment Outputs 63

_7.3.1 Overview 63_

_7.3.2 Scoring Models 63_

**Appendix A— Converting XCCDF 1.1.4 Content to XCCDF 1.2 66**

A.1 Changes to the XCCDF XML Namespace 66

A.2 Conversion of Identifiers 66

A.3 Conversion of \<xccdf:sub\> Elements 66

A.4 Properties Removed or Deprecated Since XCCDF 1.1.4 67

Appendix B — Change Log 68

Tables

Table 1: Conventional XML Mappings 1

**Table 2: Recommended Class Values 9**

**Table 3: Element Identifier Format Conventions 10**

**Table 4: \<xccdf:Benchmark\> Element Properties 16**

**Table 5: Item Element Properties 18**

**Table 6: Properties Specific to \<xccdf:Group\> and \<xccdf:Rule\> Elements 20**

**Table 7: \<xccdf:warning\> Element @category Attribute Values 21**

**Table 8: \<xccdf:Group\> Element Properties 21**

**Table 9: \<xccdf:Rule\> Element Properties 23**

**Table 10: Assigned Values for the @system Attribute of an \<xccdf:ident\> Element 24**

**Table 11: \<xccdf:check\> Element Properties 25**

**Table 12: Truth Table for AND 27**

**Table 13: Truth Table for OR 27**

**Table 14: Truth Table for Negation 27**

**Table 15: Possible Properties for \<xccdf:fixtext\> Element 28**

**Table 16: Possible Properties for \<xccdf:fix\> Element 29**

**Table 17: Predefined Values for @system Attribute of \<xccdf:fix\> Element 30**

**Table 18: \<xccdf:Value\> Element Properties 31**

**Table 19: Possible Properties for \<xccdf:choices\> Element 32**

**Table 20: Permitted Operators by Value Type 33**

**Table 21: \<xccdf:Profile\> Element Properties 35**

**Table 22: Selectors 36**

**Table 23: \<xccdf:TestResult\> Element Properties 39**

**Table 24: Predefined @name Attribute Values for \<xccdf:fact\> Elements 41**

**Table 25: \<xccdf:rule-result\> Element Properties 42**

**Table 26: Possible Results for a Single Test 43**

**Table 27: \<xccdf:override\> Element Properties 44**

**Table 28: \<xccdf:tailoring-file\> Element Properties 45**

**Table 29: \<xccdf:Tailoring\> Element Properties 46**

**Table 30: Profile Shadowing Behavior 47**

**Table 31: Tailoring Actions and Profile Selectors 47**

**Table 32: Loading Processing Sequence Sub-Steps 48**

**Table 33: Inheritance Processing Model 50**

**Table 34: Benchmark Processing Algorithm Sub-Steps 51**

**Table 35: Item Processing Algorithm Sub-Steps 52**

**Table 36: Profile Selector Example: Initial Configuration 56**

**Table 37: Profile Selector Example: Initial Benchmark State 56**

**Table 38: Profile Selector Example: Final Benchmark State 58**

**Table 39: Check Processing Algorithm Sub-Steps 59**

**Table 40: Default Model Algorithm Sub-Steps 64**

**Table 41: Flat Model Algorithm Sub-Steps 65**

**Table 42: Alternative Operations for Removed and Deprecated XCCDF 1.1.4 Constructs 67**

Table 43: Mapping Previous Release Sections to This Release 69

**Figures**

Figure 1: Typical Structure of a Benchmark 15

**[Figure 2: Check Processing Flowchart (when the check's parent is an \<xccdf:Rule\>)**

Figure 3: Workflow for Assessing Benchmark Compliance 63

# 1. Introduction

## 1.1 Purpose and Scope

This report defines the specification for the Extensible Configuration Checklist Description Format (XCCDF) version 1.2. The report also defines and explains the requirements that XCCDF 1.2 documents and products (i.e., software) must meet to claim conformance with the specification. This report only applies to XCCDF version 1.2. All other versions are outside the scope of this report.

## 1.2 Document Structure

The remainder of this report is composed of the following sections and appendices:

- Section 2 provides a list of normative references for the report.
- Section 3 defines selected terms and abbreviations used in the report.
- Section 4 provides the high-level requirements for claiming conformance with the XCCDF version 1.2 specification.
- Section 5 gives an overview of XCCDF and its capabilities.
- Section 6 provides an introduction to the XCCDF data model and details additional requirements and recommendations for XCCDF's use.
- Section 7 discusses XCCDF processing requirements and recommendations.
- Appendix A explains how to convert XCCDF 1.1.4-specific properties to their XCCDF 1.2 counterparts.
- Appendix B provides a change log that documents significant changes to released drafts of this specification. This includes a section-by-section mapping of how the document was reorganized from the previous drafts to this draft. Readers who are familiar with any previous XCCDF versions may find it helpful to review Appendix B first before the rest of the document.

## 1.3 Document Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in Request for Comment (RFC) 2119 [RFC2119].

Namespace prefixes used in this specification are listed in Table 1.

**Table 1: Conventional XML Mappings**

| Prefix | Namespace | Schema |
| --- | --- | --- |
| cpe2 | http://cpe.mitre.org/language/2.0 | Common Platform Enumeration (CPE) 2.3 Applicability Language |
| cpe2-dict | http://cpe.mitre.org/dictionary/2.0 | CPE 2.3 Dictionary |
| dc | http://purl.org/dc/elements/1.1/ | Simple Dublin Core elements |
| dsig | http://www.w3.org/2000/09/xmldsig# | Interoperable XML digital signatures |
| xccdf | http://checklists.nist.gov/xccdf/1.2 | XCCDF policy documents |
| xml | http://www.w3.org/XML/1998/namespace | Common XML attributes |
| xsd | http://www.w3.org/2001/XMLSchema | XML Schema |
| xsi | http://www.w3.org/2001/XMLSchema-Instance | XML Schema Instance |
