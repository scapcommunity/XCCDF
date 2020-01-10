# XCCDF Overview

## Introduction

XCCDF was created to document technical and non-technical security
checklists using a standardized format. The general objective is to
allow security analysts and IT experts to create effective,
interoperable automated checklists, and to support the use of these
checklists with a wide variety of tools. A checklist is an organized
collection of rules about a particular kind of system or platform.
Automation is necessary for consistent and rapid verification of system
security because of the sheer number of things to check and the number
of hosts within an organization that need to be assessed (often many
thousands).

XCCDF enables easier, more uniform creation of security checklists,
which in turn helps to improve system security by more consistent and
accurate application of sound security practices. Adoption of XCCDF lets
security professionals, security tool vendors, and system auditors
exchange information more quickly and precisely, and also permits
greater automation of security testing and configuration assessment.
Additional capabilities provided by XCCDF include the following:

-   Ensure compliance to multiple policies (systems subject to the
    Federal Information Security Management Act \[FISMA\], Security
    Technical Implementation Guide \[STIG\], Health Insurance
    Portability and Accountability Act \[HIPAA\], etc.)

-   Permit faster, more cooperative, and more automated definition of
    security rules, procedures, guidance documents, alerts, advisories,
    and remediation measures

-   Permit fast, uniform, manageable administration of security checks
    and audits

-   Permit composition of security rules and tests from different
    community groups and vendors

-   Facilitate scoring, reporting, and tracking of security status and
    checklist conformance for systems

The XCCDF specification, which is vendor-neutral, is suited for a wide
variety of checklist applications. XCCDF has an open, standardized
format, amenable to generation by and editing with a variety of tools.
In addition, because it is expressed using XML, an XCCDF document is
embeddable inside other documents. XCCDF also includes provisions for
incorporating other data formats, and it is extensible to include new
functionality, features, and data stores without hindering the
functionality of existing XCCDF tools.

Since XCCDF's creation, various commercial, government, and community
developers have created tools that support XCCDF, allowing a single
XCCDF checklist to be used by many organizations and many tools. These
tools read an XCCDF checklist and follow it to perform the necessary
checks and ask the necessary questions to measure conformance with the
checklist and generate corresponding reports.

A common use case for an XCCDF checklist is normalizing security
configuration content through automated tools. Such tools accept one or
more XCCDF checklists along with supporting system test definitions, and
determine whether the specified rules are satisfied by a target system.
The XCCDF checklist supports generation of a report, including a
weighted score. XCCDF checklists can also be used to test whether or not
a system is vulnerable to a particular kind of attack. For this purpose,
the XCCDF checklist plays the role of a vulnerability alert, but with
the ability to describe the problem, drive automated verification of its
presence, and convey recommendations for corrective actions.

The scenarios below illustrate some uses of XCCDF security checklists
and tools.

-   Scenario 1 -- An industry consortium, in conjunction with a product
    vendor, wants to produce a security checklist for an application
    server. The core security settings are the same for all OS platforms
    on which the server runs, but a few settings are OS-specific. The
    consortium crafts one checklist for the core settings and writes
    several OS-specific ones that supplement the core settings. Users
    download the core checklist and the OS-specific checklists that
    apply to their installations, and then run an assessment tool to
    score their compliance with the checklists.

-   Scenario 2 -- An academic group produces a checklist for secure
    configuration of a particular server operating system version. A
    government agency issues a set of rules extending the academic
    checklist to meet more stringent user authorization criteria imposed
    by statute. A medical enterprise downloads both the academic
    checklist and the government extension, tailors them to fit their
    internal security policy, and uses them for an enterprise-wide audit
    using a commercial security audit tool. Reports outputted by the
    tool include remediation measures which the IT staff can use to
    bring their systems into full internal policy compliance. (Note that
    remediation processes should be carefully planned and implemented.)

These scenarios demonstrate some of XCCDF's range of capabilities. XCCDF
can represent complex conditions and relationships about the systems to
be assessed, and it can incorporate descriptive material and remediative
measures. It is also designed to be modular; for example, XCCDF
benchmarks acquire programmatically ascertainable information through
lower-level check system languages.

## Checklist Structure and Tailoring

The basic unit of structure for a checklist is a rule. A rule simply
describes a state or condition which the target of the document should
exhibit. A simple checklist might consist of a list of rules, but richer
ones require additional structure. XCCDF allows checklist authors to
impose organization within the checklist, such as putting related rules
into named groups and designating the order for processing rules and
groups.

Checklist users can employ tailoring tools to customize a checklist's
rules for their local environment or policies. For example, an auditor
might need to set the password policy requirement to be more stringent
than the default recommendation. Another example is that an organization
may have trouble applying particular settings because of legacy systems
or conflicts with other software. In cases such as these, the checklist
users may need to tailor the checklist. The following customization
options are available:

-   **Selectability** -- A tailoring action selects or deselects a rule
    or group of rules. For example, an entire group of rules that relate
    to physical security might not apply to a network scan, so that
    group could be deselected. In the case of NIST Special Publication
    (SP) 800-53, certain rules apply according to the impact rating of
    the system. For example, systems that have an impact rating of *low*
    might not have all of the same access control requirements as a
    system with a *high* impact rating, so the rules that are not
    applicable for the *low* system can be deselected.

-   **Value Modification** -- A tailoring action substitutes a
    locally-significant value for a general value in an XCCDF variable
    (*\<xccdf:Value\>*). This locally-significant value then gets used
    wherever the variable is referenced. For example, at a site where
    all logs are sent to a single host, the address of that log server
    could be substituted into an audit configuration variable. Using the
    NIST SP 800-53 example, a system with a *moderate* impact rating
    might require a 12-character password, whereas a system with a *low*
    impact rating might only require an 8-character password.

-   **Property Modification** -- A tailoring action modifies a property
    for an element not addressed by selectability or value modification.
    For example, an author could alter the relative weight of particular
    rules or groups of rules.

XCCDF 1.2 supports the creation and use of tailoring documents, which
define tailoring profiles available for use with a particular benchmark
document. Having a tailoring document allows sets of checklist
customizations to be recorded in a consistent manner.

XCCDF allows checklists to include descriptive and interrogative text to
help checklist users make tailoring decisions, even directing users
through the process. Some combinations of rules within the same
checklist might conflict or be mutually exclusive. To avert problems,
the checklist author can identify particular tailoring choices as
incompatible. Checklist authors can also designate the modes (e.g.,
Gold, Platinum, High Impact Rating, Level 1) under which a rule should
be processed. Checklist authors should include the appropriate text in
their checklists to aid in tailoring.

Another important facet of customization is extension. XCCDF supports
mechanisms for authors to extend (inherit from) existing rules, values,
and groups, in addition to expressing rules, values, and groups in their
entirety. For example, in XCCDF checklists, it is desirable to share
descriptive material among several rules, and to allow a specialized
rule to be created by extending a base rule. (Note that group extension
has been deprecated in XCCDF 1.2. Use of group extension is strongly
discouraged because it has known problems, such as those described in
Section 7.2.2, and content that uses it is unlikely to be interoperable
between XCCDF products.)

## Test Results

Many organizations use several security products to determine the
security of IT systems and their compliance to various policies.
Unfortunately, if the outputs from these products are not standardized,
costly customization and integration can be required for trending,
aggregation, and reporting. Addressing this, XCCDF provides a
standardized reporting format for storing the results of the rule
checking subsystem. Security tools sometimes include only the results of
the test or tests in the form of a pass/fail status. Other tools provide
additional information (e.g., instead of simply indicating that more
than one privileged account exists on a system, certain tools also
provide the list of privileged accounts). The following information is
basic to all XCCDF results:

-   The security guidance document or checklist used, along with any
    > adaptations via customization or tailoring applied

-   Information about the target system to which the test was applied,
    > including arbitrary identification and configuration information
    > about the target system

-   The time interval of the test, and the time instant at which each
    > individual rule was evaluated

-   One or more scores

-   References to lower-level details possibly stored in other output
    > files.
