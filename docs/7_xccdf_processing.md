# XCCDF Processing

## Introduction

The XCCDF specification is designed to support automated XCCDF document
processing by a variety of products. There are three basic types of
processing that a benchmark consumer might apply to an XCCDF document:

-   **Tailoring** involves loading an XCCDF benchmark document, applying
    > customizations through the application of profiles or through user
    > actions, and then generating an XCCDF benchmark document that
    > incorporates the tailoring.

-   **Document Generation** involves loading an XCCDF benchmark document
    > and generating textual or formatted output, usually in a form
    > suitable for printing or human perusal.

-   **Assessing** involves loading an XCCDF benchmark document, checking
    > target systems or data sets that represent the target systems,
    > computing one or more scores, and generating one or more
    > *\<xccdf:TestResult\>* elements. Some products also generate other
    > outputs or store compliance information in some kind of database.

XML schema validation SHOULD be performed by benchmark consumers prior
to processing XCCDF benchmark documents.

Digital signature generation and validation MAY be performed by products
as part of processing. However, because signatures are only valid on a
source document, not valid after the document has been processed,
products that perform signature validation on source documents MUST do
so after XInclude processing and before performing any other processing
on those documents.

## Loading and Traversal

### Introduction

Each type of processing includes two common steps: loading the XCCDF
document, then traversing its contents to generate output. Loading and
traversal are discussed below. Note that loading must be complete before
traversal begins.

### Loading

Table 32 explains the basics of the loading processing sequence.

[]{#_Ref294717611 .anchor}Table : Loading Processing Sequence Sub-Steps

+-----------------------------------+-----------------------------------+
| Sub-Step                          | Description                       |
+===================================+===================================+
| Loading.Import                    | Import the XCCDF document into    |
|                                   | the program and build an initial  |
|                                   | internal representation of its    |
|                                   | elements and attributes. If the   |
|                                   | document cannot be read or        |
|                                   | parsed, then Loading fails. (At   |
|                                   | the beginning of this step, any   |
|                                   | inclusion processing specified    |
|                                   | with XInclude elements MUST be    |
|                                   | performed in compliance with      |
|                                   | \[XINCLUDE\]. The resulting XML   |
|                                   | information set should be         |
|                                   | validated against the XCCDF       |
|                                   | schema; if validation fails, then |
|                                   | Loading fails. XML Inclusion      |
|                                   | processing is independent of all  |
|                                   | XCCDF processing and must happen  |
|                                   | before any XCCDF validation or    |
|                                   | other processing.) Go to the next |
|                                   | sub-step.                         |
+-----------------------------------+-----------------------------------+
| Loading.Noticing                  | For each *\<xccdf:notice\>*       |
|                                   | element of the                    |
|                                   | *\<xccdf:Benchmark\>* element,    |
|                                   | add the notice to the product's   |
|                                   | set of legal notices. If a notice |
|                                   | with an identical *\@id* value is |
|                                   | already a member of the set, then |
|                                   | an error should be raised. Go to  |
|                                   | the next sub-step.                |
+-----------------------------------+-----------------------------------+
| Loading.Resolve                   | If the *\@resolved* attribute of  |
|                                   | the *\<xccdf:Benchmark\>* element |
|                                   | is set to true, which asserts     |
|                                   | that the other Loading.Resolve    |
|                                   | sub-steps are unnecessary, then   |
|                                   | Loading succeeds, otherwise go to |
|                                   | the next sub-step.                |
+-----------------------------------+-----------------------------------+
| Loading.Resolve.Items             | For each item in the              |
|                                   | *\<xccdf:Benchmark\>* that has an |
|                                   | *\@extends* attribute, resolve it |
|                                   | by using the following steps:     |
|                                   |                                   |
|                                   | \(1) resolve the extended item    |
|                                   | (i.e., perform                    |
|                                   | Loading.Resolve.Items on the      |
|                                   | extended item)                    |
|                                   |                                   |
|                                   | \(2) insert the necessary         |
|                                   | property sequences from the       |
|                                   | extended item into the            |
|                                   | appropriate locations in the      |
|                                   | extending item (see Table 33      |
|                                   | below)                            |
|                                   |                                   |
|                                   | \(3) remove the *\@extends*       |
|                                   | attribute                         |
|                                   |                                   |
|                                   | If any item's *\@extends*         |
|                                   | identifier does not match the     |
|                                   | identifier of a visible item of   |
|                                   | the same type, then Loading       |
|                                   | fails. If the directed graph      |
|                                   | formed by the *\@extends*         |
|                                   | attributes includes a loop, then  |
|                                   | indicate a processing error and   |
|                                   | Loading fails, otherwise go to    |
|                                   | the next sub-step.                |
+-----------------------------------+-----------------------------------+
| Loading.Resolve.Profiles          | For each *\<xccdf:Profile\>* in   |
|                                   | the *\<xccdf:Benchmark\>* that    |
|                                   | has an *\@extends* attribute,     |
|                                   | resolve the set of properties in  |
|                                   | the extending *\<xccdf:Profile\>* |
|                                   | by applying the following steps:  |
|                                   |                                   |
|                                   | \(1) resolve the extended         |
|                                   | *\<xccdf:Profile\>* (i.e.,        |
|                                   | perform                           |
|                                   | Loading.Resolve.Profiles on the   |
|                                   | extended profile)                 |
|                                   |                                   |
|                                   | \(2) insert the necessary         |
|                                   | property sequences from the       |
|                                   | extended *\<xccdf:Profile\>*      |
|                                   | into the appropriate locations    |
|                                   | in the extending                  |
|                                   | *\<xccdf:Profile\>* (see Table    |
|                                   | 33 below)                         |
|                                   |                                   |
|                                   | If any *\@extends* identifier     |
|                                   | does not match the identifier of  |
|                                   | another *\<xccdf:Profile\>* in    |
|                                   | the *\<xccdf:Benchmark\>*, then   |
|                                   | Loading fails. If the directed    |
|                                   | graph formed by the *\@extends*   |
|                                   | attributes includes a loop, then  |
|                                   | indicate a processing error and   |
|                                   | Loading fails. Otherwise, go to   |
|                                   | the next sub-step.                |
+-----------------------------------+-----------------------------------+
| Loading.Resolve.Tailoring         | If no *\<xccdf:Tailoring\>*       |
|                                   | element is being applied to this  |
|                                   | *\<xccdf:Benchmark\>*, go to the  |
|                                   | next sub-step. Otherwise, for     |
|                                   | each *\<xccdf:Profile\>* in the   |
|                                   | *\<xccdf:Tailoring\>* element     |
|                                   | that has an *\@extends*           |
|                                   | attribute, resolve the set of     |
|                                   | properties in the extending       |
|                                   | *\<xccdf:Profile\>* by applying   |
|                                   | the following steps:              |
|                                   |                                   |
|                                   | \(1) prepend the property         |
|                                   | sequence from the extended        |
|                                   | *\<xccdf:Profile\>* to that of    |
|                                   | the extending                     |
|                                   | *\<xccdf:Profile\>* (see Table    |
|                                   | 33 below)                         |
|                                   |                                   |
|                                   | \(2) if the *\<xccdf:Profile\>*   |
|                                   | element's *\@id* and              |
|                                   | *\@extends* attributes are both   |
|                                   | identical to the *\@id* of an     |
|                                   | *\<xccdf:Profile\>* element in    |
|                                   | the source                        |
|                                   | *\<xccdf:Benchmark\>*, then set   |
|                                   | the *\@abstract* attribute of     |
|                                   | the extended                      |
|                                   | *\<xccdf:Profile\>* in the        |
|                                   | source *\<xccdf:Benchmark\>* to   |
|                                   | true.                             |
|                                   |                                   |
|                                   | If any *\<xccdf:Profile\>*        |
|                                   | *\@extends* attribute identifier  |
|                                   | does not match the identifier of  |
|                                   | another *\<xccdf:Profile\>* in    |
|                                   | the source *\<xccdf:Benchmark\>*, |
|                                   | then Loading fails. If any        |
|                                   | tailoring profile\'s identifier   |
|                                   | duplicates the identifier of a    |
|                                   | benchmark profile in the source   |
|                                   | benchmark without also extending  |
|                                   | that profile, then Loading fails. |
|                                   | Otherwise, go to the next         |
|                                   | sub-step.                         |
+-----------------------------------+-----------------------------------+
| Loading.Resolve.Abstract          | For each item in the              |
|                                   | *\<xccdf:Benchmark\>* for which   |
|                                   | the *\@abstract* attribute is     |
|                                   | true, remove the item. Any item   |
|                                   | for which the *\@abstract*        |
|                                   | attribute is true shall not be    |
|                                   | included in any generated         |
|                                   | document and shall not be         |
|                                   | exported to any checking engine   |
|                                   | or used in any check. For each    |
|                                   | *\<xccdf:Profile\>* in the        |
|                                   | *\<xccdf:Benchmark\>* for which   |
|                                   | the *\@abstract* attribute is     |
|                                   | true, remove the                  |
|                                   | *\<xccdf:Profile\>*. Go to the    |
|                                   | next sub-step.                    |
+-----------------------------------+-----------------------------------+
| Loading.Resolve.Finalize          | Set the *\<xccdf:Benchmark\>*     |
|                                   | *\@resolved* attribute to true;   |
|                                   | Loading succeeds.                 |
+-----------------------------------+-----------------------------------+

If Loading succeeds for an XCCDF document, the internal data model
should be complete and every item should contain all of its own content.
An XCCDF document that has no *\@extends* or *\@abstract* attributes is
called a resolved document.

During the Loading.Resolve.Items and Loading.Resolve.Profiles steps, the
processor must flatten inheritance relationships. The conceptual model
for XCCDF object properties is a list of name-value pairs; property
values defined in an extending object are added to the list inherited
from the extending object. Where they are added to this list depends on
the inheritance processing model for the given property. There are five
such models:

-   **None** -- the property value or values are not inherited.

-   **Prepend** -- the property values are inherited from the extended
    > object, but values on the extending object come first, and
    > inherited values follow.

-   **Append** -- the property values are inherited from the extended
    > object; additional values may be defined on the extending object
    > and appear after the inherited values.

-   **Replace** -- the property value is inherited; a property value
    > explicitly defined on the extending object replaces an inherited
    > value.

-   **Override** -- if explicitly tagged as 'override' (by setting the
    > *\@override* attribute to "true"), the property is processed as if
    > it uses the Replace model. Otherwise, the property is processed as
    > if it uses the Append model.

Table 33 shows the inheritance processing model for each of the
properties supported on *\<xccdf:Rule\>, \<xccdf:Group\>*,
*\<xccdf:Value\>,* and *\<xccdf:Profile\>* elements.

[]{#_Ref294717623 .anchor}Table : Inheritance Processing Model

  Processing Model   Properties                                                                                                                                                                                        Remarks
  ------------------ ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- --------------------------------------------------------------------------------------------------------------------------------
  None               abstract, cluster-id, extends, id, signature, status, dc-status                                                                                                                                   These properties cannot be inherited at all; they must be given explicitly
  Prepend            source, choices                                                                                                                                                                                   
  Append             requires, conflicts, ident, fix, value, complex-value, default, complex-default, lower-bound, upper-bound, match, select, refine-value, refine-rule, set-value, set-complex-value, profile-note   
  Replace            hidden, prohibitChanges, selected, version, weight, operator, interfaceHint, check, complex-check, role, severity, type, interactive, multiple, note-tag, impact-metric                           For the check property, checks with different systems or different selectors shall be considered different properties
  Override           title, description, platform, question, rationale, warning, reference, fixtext                                                                                                                    For properties that have a locale specified (xml:lang), values with different locales shall be considered different properties

Group extension is deprecated in XCCDF 1.2; however, if it is used, the
Loading.Resolve.Items step MUST generate a fresh unique id for any
Group, Rule, or Value object that gets created through extension of its
enclosing Group. This could be accomplished by generating and assigning
a random unique id during Loading.Resolve.Items. It should be
emphasized, however, that use of this feature is strongly discouraged
because the lack of any standardized procedure for id generation means
that tools from different vendors are unlikely to handle group extension
the same way, leading to problems with interoperability.

### Traversal

#### Introduction

The second processing step is Traversal. The concept behind Traversal is
basically an in-order, depth-first walk through all the items that make
up a benchmark. The following subsections explain how Traversal works
for Benchmark, item, Profile, and check elements.

#### Benchmark Processing Algorithm

Table 34 explains the basics of the benchmark processing algorithm.

[]{#_Ref294765933 .anchor}Table : Benchmark Processing Algorithm
Sub-Steps

+-----------------------------------+-----------------------------------+
| Sub-Step                          | Description                       |
+===================================+===================================+
| Benchmark.Front                   | Process the properties of the     |
|                                   | *\<xccdf:Benchmark\>* element     |
|                                   | that are not processed in other   |
|                                   | sub-steps.                        |
+-----------------------------------+-----------------------------------+
| Benchmark.Profile                 | If the identifier of an           |
|                                   | *\<xccdf:Profile\>* was           |
|                                   | specified, then apply the         |
|                                   | settings in the                   |
|                                   | *\<xccdf:Profile\>* to the        |
|                                   | *\<xccdf:Benchmark\>*. At most    |
|                                   | one *\<xccdf:Profile\>* id may be |
|                                   | specified in a single instance of |
|                                   | document generation or            |
|                                   | assessment.                       |
+-----------------------------------+-----------------------------------+
| Benchmark.ManualTailoring         | Benchmark consumer products that  |
|                                   | are also benchmark producers MAY  |
|                                   | allow users to apply manual       |
|                                   | tailoring actions at this time.   |
|                                   | If that happens, the product      |
|                                   | SHOULD generate a new             |
|                                   | *\<xccdf:Tailoring\>* element to  |
|                                   | record these actions. The nature  |
|                                   | of this element depends on prior  |
|                                   | actions:                          |
|                                   |                                   |
|                                   | -   If no *\<xccdf:Profile\>* was |
|                                   |     > applied in the              |
|                                   |     > Benchmark.Profile step,     |
|                                   |     > then the new                |
|                                   |     > *\<xccdf:Tailoring\>*       |
|                                   |     > element contains a single   |
|                                   |     > *\<xccdf:Profile\>*         |
|                                   |     > consisting of selectors     |
|                                   |     > documenting user tailoring. |
|                                   |     > This new                    |
|                                   |     > *\<xccdf:Profile\>* does    |
|                                   |     > not extend any other        |
|                                   |     > *\<xccdf:Profile\>* and     |
|                                   |     > must have its own unique    |
|                                   |     > identifier.                 |
|                                   |                                   |
|                                   | -   If an *\<xccdf:Profile\>*     |
|                                   |     > from the source             |
|                                   |     > *\<xccdf:Benchmark\>* was   |
|                                   |     > applied in the              |
|                                   |     > Benchmark.Profile step,     |
|                                   |     > then the new                |
|                                   |     > *\<xccdf:Tailoring\>*       |
|                                   |     > element contains a single   |
|                                   |     > *\<xccdf:Profile\>*         |
|                                   |     > consisting of selectors     |
|                                   |     > documenting user tailoring. |
|                                   |     > This *\<xccdf:Profile\>*    |
|                                   |     > extends the applied source  |
|                                   |     > benchmark profile and       |
|                                   |     > duplicates its identifier   |
|                                   |     > (i.e., it shadows that      |
|                                   |     > source benchmark profile).  |
|                                   |                                   |
|                                   | -   If an *\<xccdf:Profile\>*     |
|                                   |     > from some other             |
|                                   |     > *\<xccdf:Tailoring\>*       |
|                                   |     > element was applied in the  |
|                                   |     > Benchmark.Profile step,     |
|                                   |     > then the new                |
|                                   |     > *\<xccdf:Tailoring\>*       |
|                                   |     > element is created as a     |
|                                   |     > copy of the utilized        |
|                                   |     > *\<xccdf:Tailoring\>*       |
|                                   |     > element with the same id,   |
|                                   |     > an iterated version, and an |
|                                   |     > updated timestamp.          |
|                                   |     > Selectors documenting user  |
|                                   |     > tailoring actions are then  |
|                                   |     > appended to the copy of the |
|                                   |     > applied tailoring profile.  |
|                                   |                                   |
|                                   | In all cases,                     |
|                                   | *\<xccdf:TestResult\>* elements   |
|                                   | will record the new/modified      |
|                                   | *\<xccdf:Profile\>* in the new    |
|                                   | *\<xccdf:Tailoring\>* element in  |
|                                   | order to provide traceability of  |
|                                   | user tailoring actions.           |
+-----------------------------------+-----------------------------------+
| Benchmark.Content                 | If the processing type is         |
|                                   | Tailoring, skip to the next       |
|                                   | sub-step. Otherwise, for each     |
|                                   | item in the                       |
|                                   | *\<xccdf:Benchmark\>*, initiate   |
|                                   | Item.Process (see Table 35).      |
+-----------------------------------+-----------------------------------+
| Benchmark.Back                    | Finalize the processing of the    |
|                                   | *\<xccdf:Benchmark\>*.            |
+-----------------------------------+-----------------------------------+

The sub-steps Front and Back will be different for each kind of
processing, and each product may perform specialized handling of the
benchmark properties that are processed during the Front and Back
sub-steps. For document generation, profiles may be processed separately
as part of Benchmark.Back to generate part of the output document.

#### Item Processing Algorithm

##### Basics

Table 35 explains the basics of the item processing algorithm. Note that
when the processing type is Tailoring, Item processing is not performed.

[]{#_Ref294765962 .anchor}Table : Item Processing Algorithm Sub-Steps

+-----------------------------------+-----------------------------------+
| Sub-Step                          | Description                       |
+===================================+===================================+
| Item.Process                      | Examine the contents of the       |
|                                   | *\<xccdf:requires\>* and          |
|                                   | *\<xccdf:conflicts\>* elements;   |
|                                   | if any instances of               |
|                                   | *\<xccdf:requires\>* have all     |
|                                   | their items unselected, or any    |
|                                   | *\<xccdf:conflicts\>* instances   |
|                                   | have any items selected, then set |
|                                   | the *\@selected* attribute to     |
|                                   | false. See Section 7.2.3.3.2.     |
+-----------------------------------+-----------------------------------+
| Item.Select                       | If any of the following           |
|                                   | conditions holds, cease           |
|                                   | processing of this item.          |
|                                   |                                   |
|                                   | -   The processing type is        |
|                                   |     > Document Generation, and    |
|                                   |     > the item's *\@hidden*       |
|                                   |     > attribute is true.          |
|                                   |                                   |
|                                   | -   The processing type is        |
|                                   |     > Assessing, and the item's   |
|                                   |     > *\@selected* attribute is   |
|                                   |     > false. If this item is a    |
|                                   |     > rule, its result becomes    |
|                                   |     > notselected (see Table 26). |
|                                   |                                   |
|                                   | -   The processing type is        |
|                                   |     > Assessing, and the item is  |
|                                   |     > a rule with a *\@role*      |
|                                   |     > attribute whose value is    |
|                                   |     > "unchecked". The result of  |
|                                   |     > this rule becomes           |
|                                   |     > notchecked (see Table 26).  |
|                                   |                                   |
|                                   | -   The processing type is        |
|                                   |     > Assessing, and the current  |
|                                   |     > platform (if known by the   |
|                                   |     > product) is not a member of |
|                                   |     > the set of platforms for    |
|                                   |     > this item. If this item is  |
|                                   |     > a rule, its result becomes  |
|                                   |     > notapplicable (see Table    |
|                                   |     > 26).                        |
|                                   |                                   |
|                                   | At the beginning of Document      |
|                                   | Generation, a user may have       |
|                                   | specified a platform to constrain |
|                                   | document generation. If the       |
|                                   | user-defined platform used for    |
|                                   | document generation is not a      |
|                                   | member of the set of platforms    |
|                                   | for this item, then the product   |
|                                   | MAY stop processing of this item. |
+-----------------------------------+-----------------------------------+
| Group.Front                       | If the item is an                 |
|                                   | *\<xccdf:Group\>*, then process   |
|                                   | its properties.                   |
+-----------------------------------+-----------------------------------+
| Group.Content                     | If the item is an                 |
|                                   | *\<xccdf:Group\>*, then for each  |
|                                   | item in the *\<xccdf:Group\>*,    |
|                                   | initiate Item.Process.            |
+-----------------------------------+-----------------------------------+
| Rule.Content                      | If the item is an                 |
|                                   | *\<xccdf:Rule\>*, then process    |
|                                   | its properties (see Section       |
|                                   | 7.2.3.5).                         |
+-----------------------------------+-----------------------------------+
| Value.Content                     | If the item is an                 |
|                                   | *\<xccdf:Value\>*, then process   |
|                                   | its properties.                   |
+-----------------------------------+-----------------------------------+

The list below describes some of the processing in more detail.

-   For Document Generation, the key to processing is to generate an
    > output stream that can be formatted as a readable or printable
    > document. The exact formatting discipline depends on the tool and
    > the target output format. In general, the *\@selected* attribute
    > is not germane to Document Generation.

-   For Assessing, the key to processing is applying the
    > *\<xccdf:Rule\>* checks to the target system or collecting data
    > about the target system. It is also possible that some
    > *\<xccdf:Rule\>* checks will need to be applied to multiple
    > contexts or features of the target system or trigger multiple
    > blocks of code in the checking language, generating multiple pass
    > or fail results for a single *\<xccdf:Rule\>* element. For more
    > information, see the *\<xccdf:multi-check\>* discussion in Section
    > 6.4.4.4 and the *\<xccdf:multiple\>* discussion in Section
    > 6.4.4.2.

##### \<xccdf:requires\> and \<xccdf:conflicts\> Elements

To prevent ambiguity, benchmark consumers must process the items of the
*\<xccdf:Benchmark\>* in order, and must not change the selected
property of any *\<xccdf:Rule\>* or *\<xccdf:Group\>* more than once
during a processing session. It should be emphasized that
*\<xccdf:Group\>* and *\<xccdf:Rule\>* elements shall not change from
deselected to selected based on their *\<xccdf:requires\>* and
*\<xccdf:conflicts\>* elements. Also, *\<xccdf:requires\>* and
*\<xccdf:conflicts\>* elements shall only change their parent item; they
shall not modify other items in the *\<xccdf:Benchmark\>*. Finally, note
also that *\<xccdf:requires\>* and *\<xccdf:conflicts\>* elements shall
not be evaluated more than once. Later changes to a Benchmark\'s state
might result in deselections that would cause a previous evaluation of
requires/conflicts properties to come to a different conclusion.
However, prior evaluations shall not change, even if their results
become \"incorrect\" after subsequent items are processed.

Rules and Groups may contain any number of *\<xccdf:requires\>* and
*\<xccdf:conflicts\>* elements and if any of these elements do not
evaluate to true, then that item shall become deselected. Essentially,
the results of the individual *\<xccdf:requires\>* and
*\<xccdf:conflicts\>* elements are ANDed together to determine whether a
given item\'s *\<xccdf:requires\>* and *\<xccdf:conflicts\>* elements
are met.

Here are a few examples of the processing of *\<xccdf:requires\>* and
*\<xccdf:conflicts\>* elements. In all examples, it is assumed that
application of profiles and/or manual tailoring has already occurred.

*Example \#1 -- Simple requires/conflicts example *

Below is a simple example of an *\<xccdf:Rule\>* that uses
*\<xccdf:requires\>* and *\<xccdf:conflicts\>*:

+-------------------------------------------------------------------------+
| \<xccdf:Rule id=\"xccdf\_org.example\_rule\_Rule1\" selected=\"true\"\> |
|                                                                         |
| ..                                                                      |
|                                                                         |
| \<xccdf:requires idref=\"xccdf\_org.example\_rule\_Rule2\               |
| xccdf\_org.example\_rule\_Rule3\"/\>                                    |
|                                                                         |
| \<xccdf:requires idref=\"xccdf\_org.example\_group\_Group1\" /\>        |
|                                                                         |
| \<xccdf:conflicts idref=\"xccdf\_org.example\_rule\_Rule4\" /\>         |
|                                                                         |
| \...                                                                    |
|                                                                         |
| \</xccdf:Rule\>                                                         |
+-------------------------------------------------------------------------+

The above *\<xccdf:Rule\>* would only be selected if at least one of
Rule2 or Rule3 was selected, if Group1 was selected, and if Rule4 was
not selected. Expressed in boolean logic format, Rule1\'s
requires/conflicts parameters would be met if and only if:

((Rule2 OR Rule3) AND Group1 AND \~Rule4)

In the above algebra, a name evaluates to \"true\" if and only if the
named item is selected. Note that if Rule1 were already de-selected, the
requires/conflicts evaluation becomes moot -- Rule1 would never change
to selected even if all its requires and conflicts properties were met.
Likewise, Rule1\'s requires and conflicts statements never affect Rule2,
Rule3, Rule4, or Group1.

*Example \#2 -- Ordering of requires/conflicts *

As mentioned earlier, an item\'s compliance with its
*\<xccdf:requires\>* and *\<xccdf:conflicts\>* elements is only
evaluated at one point in time and subsequent changes to the
benchmark\'s state, even if they would make those evaluations
\"incorrect\" if re-run, do not change the prior results. Consider the
following *\<xccdf:Rule\>* elements:

+--------------------------------------------------------------------------+
| \<xccdf:Rule id=\"xccdf\_org.example\_rule\_Rule1\" selected=\"true\"\>  |
|                                                                          |
| \...                                                                     |
|                                                                          |
| \<xccdf:requires idref=\"xccdf\_org.example\_rule\_Rule2\"\>             |
|                                                                          |
| \...                                                                     |
|                                                                          |
| \</xccdf:Rule\>                                                          |
|                                                                          |
| \<xccdf:Rule id=\"xccdf\_org.example\_rule\_Rule2\" selected=\"true\"\>  |
|                                                                          |
| \...                                                                     |
|                                                                          |
| \<xccdf:requires idref=\"xccdf\_org.example\_rule\_Rule3\"\>             |
|                                                                          |
| \...                                                                     |
|                                                                          |
| \</xccdf:Rule\>                                                          |
|                                                                          |
| \<xccdf:Rule id=\"xccdf\_org.example\_rule\_Rule3\" selected=\"false\"\> |
|                                                                          |
| \...                                                                     |
|                                                                          |
| \<xccdf:requires idref=\"xccdf\_org.example\_rule\_Rule4\"\>             |
|                                                                          |
| \...                                                                     |
|                                                                          |
| \</xccdf:Rule\>                                                          |
|                                                                          |
| \<xccdf:Rule id=\"xccdf\_org.example\_rule\_Rule4\" selected=\"true\"\>  |
|                                                                          |
| \...                                                                     |
|                                                                          |
| \</xccdf:Rule\>                                                          |
+--------------------------------------------------------------------------+

In the above example, Rule1 requires Rule2, Rule2 requires Rule3, and
Rule3 requires Rule4, although since Rule3 is already deselected, its
requires statements are irrelevant. Looking at the above scenario, one
might be tempted to believe that Rule1, Rule2, and Rule3 will all end up
deselected, but this is not the case. The following steps show how item
processing of these Rules would proceed. For this example, we will
assume we are doing assessment.

1.  **Item.Process(Rule1)** -- Because Rule2 is required, we see if
    Rule2 is selected. It is, so we make no change to Rule1\'s selection
    status.

2.  **Item**.**Select(Rule1)** -- Rule1 is selected. Continue processing
    Rule1.

3.  **Rule.Content(Rule1)** -- Process Rule1\'s content.

4.  **Item.Process(Rule2)** -- Because Rule3 is required, we see if
    Rule3 is selected. It is not, so we set selected on Rule2 to false.

5.  **Item**.**Select(Rule2)** -- Rule2 is not selected. Terminate
    processing of Rule2.

6.  **Item.Process(Rule3)** -- Because Rule4 is required, we see if
    Rule4 is selected. It is, but Rule 3 is already de-selected and
    remains so.

7.  **Item**.**Select(Rule3)** -- Rule3 is not selected. Terminate
    processing of Rule3.

8.  **Item.Process(Rule4)** -- Rule4 has no requires/conflicts
    properties so this step is skipped.

9.  **Item**.**Select(Rule4)** -- Rule4 is selected. Continue processing
    Rule4.

10. **Rule.Content(Rule4)** -- Process Rule4\'s content.

The final result was that Rule1 and Rule4 were selected and processed
while Rule2 and Rule3 were de-selected and not processed. This happens
even though Rule1 requires Rule2. Because we have completed processing
of Rule1\'s content before we start processing of Rule2, by the time we
realize that Rule2\'s requires statement cannot be met and Rule2 becomes
deselected, the effect this change would have on Rule1 is moot because
Rule1 has already been run.

This example demonstrates the importance of processing items in the
order in which they appear in the benchmark XML. If a benchmark consumer
processed these items in a different order (for example, from the bottom
up), this would result in a different set of Rule contents being
processed, which would violate the XCCDF specification.

*Example \#3 -- Requires, conflicts, and Groups *

Example \#2 shows how a Rule\'s content might be processed even though a
Rule that it requires is not (eventually) selected. Another way this can
happen is if a required Rule is contained in a deselected Group.
Consider the following example:

+-----------------------------------------------------------------------+
| \<xccdf:Rule id=\"xccdf\_org.example\_rule\_Rule1\"                   |
| selected=\"false\"\>                                                  |
|                                                                       |
| \...                                                                  |
|                                                                       |
| \</xccdf:Rule\>                                                       |
|                                                                       |
| \<xccdf:Group id=\"xccdf\_org.example\_group\_Group1\"                |
| selected=\"false\"\>                                                  |
|                                                                       |
| \...                                                                  |
|                                                                       |
| \<xccdf:Rule id=\"xccdf\_org.example\_rule\_Rule2\"                   |
| selected=\"true\"\>                                                   |
|                                                                       |
| \...                                                                  |
|                                                                       |
| \<xccdf:requires idref=\"xccdf\_org.example\_rule\_Rule1\" /\>        |
|                                                                       |
| \...                                                                  |
|                                                                       |
| \</xccdf:Rule\>                                                       |
|                                                                       |
| \...                                                                  |
|                                                                       |
| \</xccdf:Group\>                                                      |
|                                                                       |
| \<xccdf:Rule id=\"xccdf\_org.example\_rule\_Rule3\"                   |
| selected=\"true\"\>                                                   |
|                                                                       |
| \...                                                                  |
|                                                                       |
| \<xccdf:requires idref=\"xccdf\_org.example\_rule\_Rule2\" /\>        |
|                                                                       |
| \...                                                                  |
|                                                                       |
| \</xccdf:Rule\>                                                       |
+-----------------------------------------------------------------------+

Because Group1 is deselected, none of its contents will ever be
processed. Thus, even though Rule2 would never be run and even though
its requires property would not be met, it remains selected and, as
such, allows the requires statement of Rule3 to evaluate to true. The
following steps show how Item Processing of these Rules would proceed.
For this example, we will assume we are doing assessment.

1.  **Item.Process(Rule1)** -- Rule1 has no requires/conflicts
    properties so this step is skipped.

2.  **Item**.**Select(Rule1)** -- Rule1 is not selected. Terminate
    processing of Rule1.

3.  **Item.Process(Group1)** -- Group1 has no requires/conflicts
    properties so this step is skipped.

4.  **Item**.**Select(Group1)** -- Group1 is not selected. Terminate
    processing of Group1. *Note that we never get to the Group.Content
    step so Rule2 never undergoes any form of processing. *

5.  **Item.Process(Rule3)** -- Because Rule2 is required, we see if
    Rule2 is selected. It is, so we make no change to Rule3\'s selection
    status.

6.  **Item**.**Select(Rule3)** -- Rule3 is selected. Continue processing
    Rule3.

7.  **Rule.Content(Rule3)** -- Process Rule3\'s content.

Rule3 is run even though it requires a Rule that is not run.

#### Profile Selector Processing 

Profile selectors (*\<xccdf:select\>*, *\<xccdf:refine-value\>*,
*\<xccdf:set-value\>*, *\<xccdf:set-complex-value\>*, and
*\<xccdf:refine-rule\>* elements) shall be processed in the order in
which they appear in the XML. Since these selectors are processed under
the "append" extension processing model, an extending
*\<xccdf:Profile\>* may override the inherited selectors of the
*\<xccdf:Profile\>* it extends.

*\<xccdf:Profile\>* selector processing can be understood more easily by
looking at an example. Assume the existence and initial configuration of
an *\<xccdf:Benchmark\>* with the *\<xccdf:Rule\>*, *\<xccdf:Group\>*,
and *\<xccdf:Value\>* elements listed in Table 36:

[]{#_Ref296003864 .anchor}Table : Profile Selector Example: Initial
Configuration

  ------- -------- ------------ ---------- ---------------------------
  Item    id       cluster-id   selected   Defined Selectors
  Rule    Rule1    Cluster2     false      (empty)
  Rule    Rule2                 true       (empty), sel3
  Rule    Rule3    Cluster1     true       (empty)
  Rule    Rule4    Cluster1     true       sel1, sel5
  Rule    Rule5    Cluster3     true       sel1
  Group   Group1   Cluster1     true       
  Group   Group2   Cluster1     true       
  Value   Value1                           (empty), sel1, sel2
  Value   Value2   Cluster2                (empty), sel1, sel2, sel5
  Value   Value3   Cluster2                (empty), sel1, sel5, sel6
  Value   Value4   Cluster3                (empty), sel4
  ------- -------- ------------ ---------- ---------------------------

Based on this configuration, the initial state of the
*\<xccdf:Benchmark\>* is as listed in Table 37:

[]{#_Ref296003949 .anchor}Table : Profile Selector Example: Initial
Benchmark State

  --------- -------------- ---------------------
  Item id   Selected?      Applicable Selector
  Rule1     not selected   (empty)
  Rule2     selected       (empty)
  Rule3     selected       (empty)
  Rule4     selected       -none-
  Rule5     selected       -none-
  Group1    selected       -none-
  Group2    selected       -none-
  Value1                   (empty)
  Value2                   (empty)
  Value3                   (empty)
  Value4                   (empty)
  --------- -------------- ---------------------

Now consider the following *\<xccdf:Profile\>* definitions:

+-----------------------------------------------------------------------+
| \<xccdf:Profile id=\"xccdf\_org.example\_profile\_Profile1\"          |
| abstract=\"true\"\>                                                   |
|                                                                       |
| \...                                                                  |
|                                                                       |
| \<xccdf:select idref=\"xccdf\_org.example\_rule\_Rule1\"              |
| selected=\"true\" /\>                                                 |
|                                                                       |
| \<xccdf:select idref=\"Cluster1\" selected=\"false\" /\>              |
|                                                                       |
| \<xccdf:select idref=\"xccdf\_org.example\_group\_Group1\"            |
| selected=\"true\" /\>                                                 |
|                                                                       |
| \<xccdf:refine-value idref=\"xccdf\_org.example\_value\_Value1\"      |
| selector=\"sel1\" /\>                                                 |
|                                                                       |
| \<xccdf:refine-value idref=\"Cluster2\" selector=\"sel2\" /\>         |
|                                                                       |
| \<xccdf:set-value                                                     |
| idref=\"xccdf\_org.example\_value\_Value4\"\>NEWVALUE\</set-value\>   |
|                                                                       |
| \<xccdf:refine-rule idref=\"xccdf\_org.example\_rule\_Rule2\"         |
| selector=\"sel3\" /\>                                                 |
|                                                                       |
| \<xccdf:refine-rule idref=\"Cluster3\" selector=\"sel1\" /\>          |
|                                                                       |
| \</xccdf:Profile\>                                                    |
|                                                                       |
| \<xccdf:Profile id=\"xccdf\_org.example\_profile\_Profile2\"          |
| extends=\"Profile1\"\>                                                |
|                                                                       |
| \...                                                                  |
|                                                                       |
| \<xccdf:select idref=\"xccdf\_org.example\_rule\_Rule1\"              |
| selected=\"false\" /\>                                                |
|                                                                       |
| \<xccdf:select idref=\"xccdf\_org.example\_rule\_Rule3\"              |
| selected=\"true\" /\>                                                 |
|                                                                       |
| \<xccdf:refine-value idref=\"Cluster2\" selector=\"sel5\" /\>         |
|                                                                       |
| \<xccdf:refine-rule idref=\"xccdf\_org.example\_rule\_Rule5\"         |
| selector=\"sel6\" /\>                                                 |
|                                                                       |
| \</xccdf:Profile\>                                                    |
+-----------------------------------------------------------------------+

Because Profile selectors are appended under extension, after Loading
steps have completed, Profile2\'s effective list of selectors would look
like the following (with numbers added for reference and identifier
names condensed for brevity):

1.  \<select idref=\"Rule1\" selected=\"true\" /\>

2.  \<select idref=\"Cluster1\" selected=\"false\" /\>

3.  \<select idref=\"Group1\" selected=\"true\" /\>

4.  \<refine-value idref=\"Value1\" selector=\"sel1\" /\>

5.  \<refine-value idref=\"Cluster2\" selector=\"sel2\" /\>

6.  \<set-value idref=\"Value4\"\>NEWVALUE\</set-value\>

7.  \<refine-rule idref=\"Rule2\" selector=\"sel3\" /\>

8.  \<refine-rule idref=\"Cluster3\" selector=\"sel1\" /\>

9.  \<select idref=\"Rule1\" selected=\"false\" /\>

10. \<select idref=\"Rule3\" selected=\"true\" /\>

11. \<refine-value idref=\"Cluster2\" selector=\"sel5\" /\>

12. \<refine-rule idref=\"Rule5\" selector=\"sel6\" /\>

If Profile2 is selected, then the Benchmark.Profile processing step will
cause the following changes to the resolved Benchmark as each selector
is processed:

1.  Rule1 becomes \"selected\"

2.  Rule3, Rule4, Group1, and Group2 become \"not selected\" due to
    > their associations with Cluster1

3.  Group1 becomes \"selected\", overriding the change to this setting
    > from line 2

4.  Value1 changes to using the \"sel1\" selector

5.  Value2 and Value3 change to using the \"sel2\" selector due to their
    > associations with Cluster2. However, since Value3 does not utilize
    > any selector named \"sel2\", the effectively associates Value3
    > with the empty selector. Note that Rule1 is not affected even
    > though it is a member of Cluster2. This is because a refine-value
    > selector only affects Values.

6.  The effective value of Value4 changes to \"NEWVALUE\"

7.  Rule2 changes to using the \"sel3\" selector

8.  Rule5 changes to using the \"sel1\" selector due to its association
    > with Cluster3

9.  Rule1 becomes \"not selected\", overriding the change to this
    > setting from line 1

10. Rule3 becomes \"selected\", overriding the change to this setting
    > from line 2

11. Value2 and Value3 change to using the \"sel5\" selector due to their
    > associations with Cluster2. This overrides the change to these
    > settings from line 5.

12. Rule5 changes to using the \"sel6\" selector, but since it does not
    > utilize any selector named \"sel6\" this would lead to the use of
    > the empty selector. Since Rule5 does not define any empty selector
    > either, this Rule would effectively not utilize any selectable
    > field. Since the check field is the only selectable field in a
    > Rule, this means that Rule5 would have no check associated with it
    > (i.e., under evaluation, it would return a result of
    > \"notchecked\"). This overrides the change to this setting from
    > line 8.

The final configuration of the *\<xccdf:Benchmark\>* due to application
of Profile2 would be as shown in Table 38:

[]{#_Ref294717771 .anchor}Table : Profile Selector Example: Final
Benchmark State

  --------- -------------- -------------------------------------------
  Item id   Selected?      Applicable Selector
  Rule1     not selected   (empty)
  Rule2     selected       sel3
  Rule3     selected       (empty)
  Rule4     not selected   -none-
  Rule5     selected       sel6 (Not defined so this becomes -none-)
  Group1    selected       -none-
  Group2    not selected   -none-
  Value1                   sel1
  Value2                   sel5
  Value3                   sel5
  Value4                   =\"NEWVALUE\"
  --------- -------------- -------------------------------------------

This example demonstrates how a single item could be tailored multiple
times due to the influence of multiple selectors.

It should also be noted that selectors do not need to be of the same
type to override each other\'s behaviors. All three of the value
selectors, *\<xccdf:refine-value\>*, *\<xccdf:set-value\>*, and
*\<xccdf:set-complex-value\>*, can affect the *\<xccdf:value\>* or
*\<xccdf:complex-value\>* element of a named *\<xccdf:Value\>*. However,
an *\<xccdf:Value\>* may have only one *\<xccdf:value\>* or one
*\<xccdf:complex-value\>* element selected at any time. As a result, the
use of any of the aforementioned selectors to change an
*\<xccdf:value\>* or *\<xccdf:complex-value\>* will replace any prior
tailoring of that *\<xccdf:value\>* or *\<xccdf:complex-value\>*.

#### Check Processing 

##### Basics

During the Rule.Content sub-step of the item processing algorithm, the
properties of a given *\<xccdf:Rule\>* are processed, including its
check structures. If an *\<xccdf:Rule\>* contains an
*\<xccdf:complex-check\>*, then the benchmark consumer must process it
and must ignore any *\<xccdf:check\>* elements that are also contained
by the *\<xccdf:Rule\>*. However, within a given
*\<xccdf:complex-check\>*, processing of component checks shall follow
the same procedures described below.

Check processing involves selecting one supported *\<xccdf:check\>*
element and then executing its check code. Table 39 explains the basics
of the check processing algorithm.

[]{#_Ref299516213 .anchor}Table : Check Processing Algorithm Sub-Steps

+-----------------------------------+-----------------------------------+
| Sub-Step                          | Description                       |
+===================================+===================================+
| Check.Initialize                  | Create an empty list of candidate |
|                                   | checks.                           |
+-----------------------------------+-----------------------------------+
| Check.Selector                    | Identify the *\<xccdf:check\>*    |
|                                   | elements that are candidates for  |
|                                   | use.                              |
|                                   |                                   |
|                                   | -   If a selector has been        |
|                                   |     > identified for this Rule    |
|                                   |     > and there is at least one   |
|                                   |     > *\<xccdf:check\>* element   |
|                                   |     > with a matching             |
|                                   |     > *\@selector* attribute,     |
|                                   |     > then add all such           |
|                                   |     > *\<xccdf:check\>*.elements  |
|                                   |     > to the candidate list in    |
|                                   |     > the order in which they     |
|                                   |     > appear and proceed to the   |
|                                   |     > next sub-step.              |
|                                   |                                   |
|                                   | -   Otherwise, if there is at     |
|                                   |     > least one *\<xccdf:check\>* |
|                                   |     > element with an absent or   |
|                                   |     > empty *\@selector*          |
|                                   |     > attribute, then add all     |
|                                   |     > such                        |
|                                   |     > *\<xccdf:check\>*.elements  |
|                                   |     > to the candidate list in    |
|                                   |     > the order in which they     |
|                                   |     > appear and proceed to the   |
|                                   |     > next sub-step.              |
|                                   |                                   |
|                                   | -   Otherwise, there are no valid |
|                                   |     > candidate checks. Stop      |
|                                   |     > check processing and give   |
|                                   |     > this *\<xccdf:Rule\>* a     |
|                                   |     > result of \"notchecked\".   |
+-----------------------------------+-----------------------------------+
| Check.System                      | Iterate through each              |
|                                   | *\<xccdf:check\>* element that    |
|                                   | appears in the candidate list (in |
|                                   | order). If the system given in    |
|                                   | the element\'s *\@system*         |
|                                   | attribute is supported by an      |
|                                   | available checking engine:        |
|                                   |                                   |
|                                   | -   If the parent element is an   |
|                                   |     > *\<xccdf:Rule\>*, terminate |
|                                   |     > this sub-step and proceed   |
|                                   |     > to the next sub-step using  |
|                                   |     > that *\<xccdf:check\>*. The |
|                                   |     > next sub-step will only be  |
|                                   |     > applied to a single         |
|                                   |     > *\<xccdf:check\>* element.  |
|                                   |                                   |
|                                   | -   If the parent element is an   |
|                                   |     > *\<xccdf:complex-check\>*,  |
|                                   |     > add this *\<xccdf:check\>*  |
|                                   |     > to the list of applicable   |
|                                   |     > checks and continue         |
|                                   |     > iterating through the list  |
|                                   |     > of checks. After all        |
|                                   |     > *\<xccdf:check\>* elements  |
|                                   |     > have been processed, the    |
|                                   |     > remaining check processing  |
|                                   |     > sub-steps must be applied   |
|                                   |     > to each *\<xccdf:check\>*   |
|                                   |     > in the list of applicable   |
|                                   |     > checks. This may result in  |
|                                   |     > the following sub-steps     |
|                                   |     > being applied to multiple   |
|                                   |     > *\<xccdf:check\>* elements. |
|                                   |                                   |
|                                   | -   If the list of candidate      |
|                                   |     > *\<xccdf:check\>* elements  |
|                                   |     > is exhausted without        |
|                                   |     > finding one that uses a     |
|                                   |     > supported system, stop      |
|                                   |     > check processing and give   |
|                                   |     > this *\<xccdf:Rule\>* a     |
|                                   |     > result of \"notchecked\".   |
+-----------------------------------+-----------------------------------+
| Check.Content                     | Iterate through each              |
|                                   | *\<xccdf:check-content-ref\>*     |
|                                   | element in the *\<xccdf:check\>*  |
|                                   | element (in order). If the        |
|                                   | reference can be resolved (i.e.,  |
|                                   | if the checking-language code can |
|                                   | be made available to the checking |
|                                   | engine) then terminate this       |
|                                   | sub-step and proceed to the next  |
|                                   | sub-step using the referenced     |
|                                   | content. If the list of           |
|                                   | *\<xccdf:check-content-ref\>*     |
|                                   | elements is exhausted without any |
|                                   | reference being resolvable, then  |
|                                   | if there is an                    |
|                                   | *\<xccdf:check-content\>*         |
|                                   | element, proceed to the next step |
|                                   | using the included content.       |
|                                   | Otherwise, stop check processing  |
|                                   | and give this *\<xccdf:Rule\>* a  |
|                                   | result of \"notchecked\".         |
+-----------------------------------+-----------------------------------+
| Check.Export                      | Make the referenced               |
|                                   | checking-language code, as well   |
|                                   | as any exported values as         |
|                                   | indicated by                      |
|                                   | *\<xccdf:check-export\>*          |
|                                   | elements, available to the        |
|                                   | checking engine. (This can be     |
|                                   | done immediately or it can be     |
|                                   | done in a batch after all         |
|                                   | *\<xccdf:Rule\>* elements in the  |
|                                   | benchmark have been processed.)   |
|                                   | If this *\<xccdf:Rule\>* has a    |
|                                   | *\@role* attribute whose value is |
|                                   | "unscored", give this             |
|                                   | *\<xccdf:Rule\>* a result of      |
|                                   | "informational". Otherwise, give  |
|                                   | this *\<xccdf:Rule\>* a result    |
|                                   | appropriate to the result         |
|                                   | returned by the checking engine.  |
+-----------------------------------+-----------------------------------+

A benchmark consumer shall not "backtrack" in the processing of these
steps. For example, once a check with a preferred system is selected by
the benchmark consumer (Check.System), the benchmark consumer shall not
attempt to use a different check that uses a different system, even if
none of the originally selected check\'s content can be resolved.

Figure 2 provides a flowchart that illustrates check processing.

![](media/image4.wmf){width="6.489583333333333in"
height="3.3881944444444443in"}

##### Rules with Multiple Results

Many XCCDF documents include *\<xccdf:Rule\>* elements that apply to
system components. For example, a host OS benchmark could contain
*\<xccdf:Rule\>* elements that apply to all users, and a router
benchmark could contain *\<xccdf:Rule\>* elements that apply to all
network interfaces. When the system holds many such components, it may
not be adequate for a benchmark consumer to report that an
*\<xccdf:Rule\>* failed; it may report exactly which components failed
the *\<xccdf:Rule\>*.

A processing engine that performs a checking system test may deliver one
or more results in response to a check. In the most common case, each
*\<xccdf:Rule\>* will yield one *\<xccdf:rule-result\>* element. In a
case where an *\<xccdf:Rule\>* was applied multiple times to multiple
components of the system under test, a single *\<xccdf:Rule\>* could
yield multiple *\<xccdf:rule-result\>* elements. If the *\<xccdf:Rule\>*
*\@multiple* attribute is set to true, then each instance of the
assessment target should be reported separately. Similarly, if an
*\<xccdf:check\>* element leads to the execution of multiple checks
(i.e., an *\<xccdf:check-content-ref\>* that lacks a *\@name* attribute
is used) and the *\@multi-check* attribute is set to true, each check
executed MUST be reported separately. Otherwise, an *\<xccdf:Rule\>*
contributes to the positive score only if ANDing the results of all
instances of that *\<xccdf:Rule\>* produces a test result of 'pass'
according to the truth table that appears in the description of the
*\<xccdf:complex-check\>* element in Section 6.4.4.4. If any component
of the target system fails the checking system test, then the entire
*\<xccdf:Rule\>* shall be considered to have failed. This is sometimes
called "strict scoring". See Section 6.4.4.2 for more information on the
*\@multiple* attribute and Section 6.4.4.4 for the *\@multi-check*
attribute.

When creating multiple *\<xccdf:rule-result\>* elements that stem from a
single *\<xccdf:Rule\>* , each of these *\<xccdf:rule-result\>* elements
MUST identify the same *\<xccdf:Rule\>* in its *\@idref* attribute.

-   When multiple *\<xccdf:rule-result\>* elements are caused by
    multiple target instances with *\@multiple* set to true, the
    *\<xccdf:instance\>* element of the *\<xccdf:rule-result\>* element
    should include, at minimum, the instance name. It may include
    additional information to provide additional context for that
    instance.

-   When multiple *\<xccdf:rule-result\>* elements are caused by
    multiple executed checks with *\@multi-check* set to true, the
    *\<xccdf:check\>* element of the *\<xccdf:rule-result\>* element
    MUST identify the executed check. This should be done by including
    an *\<xccdf:check-content-ref\>* element that explicitly points to
    the corresponding result content for each of the checks executed to
    produce this particular result. Alternatively, it MAY be done by
    including the checking result structure directly in an
    *\<xccdf:check-content\>* element.

It is possible for a single *\<xccdf:Rule\>* to reference multiple
checks, some of which test multiple target instances. This would lead to
both the *\<xccdf:instance\>* and *\<xccdf:check\>* elements being
utilized in the manner described above.

#### Other Processing

##### XHTML Formatting

Some text-valued XCCDF elements may contain formatting specified with
elements from \[XHTML\] (see Section 6.2.2). How a benchmark consumer
handles embedded XHTML content in XCCDF text properties is
implementation-dependent, but every benchmark consumer must be able to
process XCCDF content even when embedded XHTML elements are present.
Products that perform document generation processing should attempt to
preserve the formatting semantics implied by the Text and List modules,
support the link semantics implied by the Hypertext module, and
incorporate the images referenced via the Image module. Such products
may also wish to establish conventions for each of the \<div\> or
\<span\> class attribute values (see Table 2).

##### Locale

XCCDF textual content may use the *\@xml:lang* attribute to specify
natural language locales. Benchmark producers and consumers should
employ *\@xml:lang* attributes whenever possible to create localized
output. If a product has an effective language selected, it SHOULD use
textual content corresponding to that language and SHOULD NOT use
textual content corresponding to other languages. If a product does not
have an effective language selected or ignores *\@xml:lang* attributes,
it MUST display all textual content in order.

##### Text Substitution

Text substitution when the *\<xccdf:sub\>* element\'s *\@idref*
attribute holds the id of an *\<xccdf:plain-text\>* element always
behaves the same way: any *\<xccdf:sub\>* element reference to an
*\<xccdf:plain-text\>* element should be replaced by the string content
of that element.

When the *\<xccdf:sub\>* element\'s *\@idref* attribute holds the id of
an *\<xccdf:Value\>* element, the *\<xccdf:sub\>* element\'s *\@use*
attribute MUST be consulted.

-   If the value of the *\@use* attribute is \"value\", then the
    *\<xccdf:sub\>* element SHOULD be replaced by a string
    representation of the content of the currently-selected
    *\<xccdf:value\>* or *\<xccdf:complex-value\>* element of the
    referenced *\<xccdf:Value\>* element.

-   If the value of the *\@use* attribute is \"title\", then the
    *\<xccdf:sub\>* element SHOULD be replaced by the body of the
    *\<xccdf:title\>* element of the referenced *\<xccdf:Value\>*
    element. The *\<xccdf:title\>* element\'s *\@xml:lang* attribute may
    be used to select the appropriate title to use if multiple titles
    are present. At the tool author's discretion, the title may be
    followed by the *\<xccdf:Value\>* element's currently-selected
    *\<xccdf:value\>* element, suitably demarcated.

-   If the value of the *\@use* attribute is \"legacy\", then during
    Tailoring, process the *\<xccdf:sub\>* element as if *\@use* was set
    to \"title\", but during Document Generation or Assessment, process
    the *\<xccdf:sub\>* element as if *\@use* was set to \"value\".

Any appearance of the *\<xccdf:instance\>* element in the content of an
*\<xccdf:fix\>* element should be replaced by a locale-appropriate
string to represent a target system instance name.

During creation of *\<xccdf:TestResult\>* elements, any *\<xccdf:fix\>*
elements present in applied Rules and matching the platform to which the
test was applied should be subjected to substitution and the resulting
string used as the value of the *\<xccdf:fix\>* element for the
*\<xccdf:rule-result\>* element. Each *\<xccdf:sub\>* element SHALL be
replaced by what the *\@idref* attribute references, which is either the
appropriate string from the referenced *\<xccdf:Value*\> element, as
described above, or the *\<xccdf:plain-text\>* definition used during
the test. Formatting for this replacement is implementation-dependent
for a referenced *\<xccdf:Value*\> element, but for an
*\<xccdf:plain-text\>* definition it is a simple string replacement.
Also, each *\<xccdf:instance\>* element should be replaced by the value
of the *\<xccdf:rule-result\>* element's *\<xccdf:instance\>* element.

Benchmark consumers MUST support resolution of XHTML *\<object\>*
elements, regardless of whether XHTML rendering is supported. The XHTML
*\<object\>* element supports substitutions of a variety of information
from an item or profile, or the string content of an
*\<xccdf:plain-text\>* definition. To avoid possible conflicts with uses
of an XHTML *\<object\>* that should not be processed specially, each
XCCDF *\<object\>* reference must be a relative URI beginning with
"\#xccdf:". The following URI values can be used to refer to things from
an XHTML *\<object\>* element, using the *\@data* attribute:

-   **\#xccdf:value:*id.***Insert the value of the
    *\<xccdf:plain-text\>* block, *\<xccdf:Value\>*, or *\<xccdf:fact\>*
    with id *id*. The value of the reference should be substituted for
    the entire *\<object\>* element and its content (if any). If the
    *id* cannot be resolved, then the textual content of the
    *\<object\>* element should be retained.

-   **\#xccdf:title:*id.*** Insert the string content of the
    *\<xccdf:title\>* element of the item with id *id*. Use the current
    language value locale setting, if any. The *\<xccdf:title\>* string
    should be substituted for the entire *\<object\>* element and its
    content (if any). If the *id* cannot be resolved, then the textual
    content of the *\<object\>* element should be retained.

##### Reference Processing

XCCDF benchmark consumers MUST support reference processing that uses
the XHTML anchor ("a") element. The anchor element can be used to create
an intra-document link to an XCCDF item or profile. To avoid possible
conflicts with uses of the XHTML anchor element that should not be
processed specially, each XCCDF anchor reference must be a relative URI
beginning with "\#xccdf:". The URI value **\#xccdf:link:*id*** can be
used to refer to things from an anchor element, using the *\@href*
attribute. This creates an intra-document link to the point in the
document where the item *id* is described. The content of the element
should be the text of the link.

## Assessment Outputs

### Overview

When a benchmark consumer performs an assessment against a system, it
accepts as inputs the state of the system and a benchmark, and MAY
produce any of the following output (also shown in Figure 3):

-   **Benchmark report** -- A human-readable report about testing,
    > including the score, and a listing of which rules passed and which
    > failed on the system. If a given rule applies to multiple parts or
    > components of the system, then multiple pass/fail entries may
    > appear on this list. The report may also include recommended steps
    > for improving compliance. The format of the report is not
    > specified here, but might be some form of formatted or rich text
    > (e.g., HTML).

-   **Benchmark results** -- Machine-readable testing results, meant for
    > storage, long-term tracking, or incorporation into other reports
    > (e.g., a site-wide report). This should be in XCCDF, using the
    > *\<xccdf:TestResult\>* element.

-   **Fix scripts** -- Machine-readable content, usually text, the
    > application of which will remediate some or all of the
    > non-compliance issues found by the benchmark consumer. These
    > scripts may be included in *\<xccdf:TestResult\>* elements (see
    > Section 6.6).

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ![Simple workflow diagram showing XCCDF being used with a benchmark compliance tool](media/image6.wmf){width="5.833333333333333in" height="2.2604166666666665in"}
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------

[]{#_Ref294717949 .anchor}Figure : Workflow for Assessing Benchmark
Compliance

### Scoring Models

#### Overview

XCCDF has four scoring models, which are defined below. Tools may
support additional proprietary or community models. A benchmark may
recommend one or more scoring models to be used when computing a
benchmark score by indicating them in the *\<xccdf:Benchmark\>*
element's *\<xccdf:model\>* element. A tool may use any score
computation model designated by the user. In the four models defined
below, a result of "fixed" shall be treated as a "pass" result for
scoring purposes; other scoring models MAY handle this differently.

#### The Default Model

This model is identified by the URI "urn:xccdf:scoring:default". Tools
must support this model.

In the default model, computation of the XCCDF score proceeds
independently for each collection of siblings in each Group, and then
for the siblings within the *\<xccdf:Benchmark\>*. This
relative-to-siblings weighted scoring model is designed for flexibility
and to foster independent authorship of collections of Rules. Benchmark
authors should keep the model in mind when assigning weights to Groups
and Rules.

The elements of an *\<xccdf:Benchmark\>* form the nodes of a tree. The
default model score computation algorithm simply computes a normalized
weighted sum at each tree node, omitting Rules and Groups that are not
selected and Groups that have no selected Rules under them. The
algorithm that shall be used at each selected node is listed in Table
40.

[]{#_Ref294718067 .anchor}Table : Default Model Algorithm Sub-Steps

+-----------------------------------+-----------------------------------+
| Sub-Step                          | Description                       |
+===================================+===================================+
| Score.Default.Rule                | If the node is a Rule, initialize |
|                                   | rule\_count and rule\_score to 0. |
|                                   | Then for each associated          |
|                                   | rule-result:                      |
|                                   |                                   |
|                                   | \- if the rule-result's result is |
|                                   | not a member of the set           |
|                                   | {notapplicable, notchecked,       |
|                                   | informational, notselected}, then |
|                                   | add 1 to rule\_count.             |
|                                   |                                   |
|                                   | \- if the rule-result's result is |
|                                   | "pass", add 1 to rule\_score.     |
|                                   |                                   |
|                                   | When this has been done for every |
|                                   | rule-result associated with this  |
|                                   | Rule:                             |
|                                   |                                   |
|                                   | \- if rule\_count is 0, set this  |
|                                   | Rule's score and count to 0.      |
|                                   |                                   |
|                                   | \- otherwise, set the Rule's      |
|                                   | count to 1 and the Rule's score   |
|                                   | to 100 \* rule\_score /           |
|                                   | rule\_count.                      |
+-----------------------------------+-----------------------------------+
| Score.Default.Group.Init          | If the node is a Group or the     |
|                                   | Benchmark, assign a count of 0, a |
|                                   | score *s* of 0.0, and an          |
|                                   | accumulator *a* of 0.0.           |
+-----------------------------------+-----------------------------------+
| Score.Default.Group.Recurse       | For each selected child of this   |
|                                   | Group or Benchmark, do the        |
|                                   | following: (1) compute the count  |
|                                   | and weighted score for the child  |
|                                   | using this algorithm, (2) if the  |
|                                   | child's count value is not 0,     |
|                                   | then add the child's weighted     |
|                                   | score to this node's score *s*,   |
|                                   | add 1 to this node's count, and   |
|                                   | add the child's weight value to   |
|                                   | the accumulator *a*.              |
+-----------------------------------+-----------------------------------+
| Score.Default.Group.Normalize     | Normalize this node's score:      |
|                                   | compute *s* = *s* / *a.*          |
+-----------------------------------+-----------------------------------+
| Score.Default.Weight              | Assign the node a weighted score  |
|                                   | equal to the product of its score |
|                                   | and its weight.                   |
+-----------------------------------+-----------------------------------+

The final test score is the normalized score value on the root node of
the tree (the *\<xccdf:Benchmark\>* element).

#### The Flat Model

This model is identified by the URI "urn:xccdf:scoring:flat". The
algorithm in Table 41 shall be used to compute the score.

[]{#_Ref294718084 .anchor}Table : Flat Model Algorithm Sub-Steps

+-----------------------------------+-----------------------------------+
| Sub-Step                          | Description                       |
+===================================+===================================+
| Score.Flat.Init                   | Initialize both the score *s* and |
|                                   | the maximum score *m* to 0.0.     |
+-----------------------------------+-----------------------------------+
| Score.Flat.Rules                  | For each Rule:                    |
|                                   |                                   |
|                                   | Initialize rule\_count and        |
|                                   | rule\_score to 0.                 |
|                                   |                                   |
|                                   | For each rule-result associated   |
|                                   | with that Rule:                   |
|                                   |                                   |
|                                   | \- if the rule-result's result is |
|                                   | not a member of the set           |
|                                   | {notapplicable, notchecked,       |
|                                   | informational, notselected}, then |
|                                   | add 1 to rule\_count.             |
|                                   |                                   |
|                                   | \- if the rule-result's result is |
|                                   | "pass", add 1 to rule\_score.     |
|                                   |                                   |
|                                   | If rule\_count is not 0:\         |
|                                   | - add the Rule's weight to *m.*\  |
|                                   | - add the Rule's weight \*        |
|                                   | rule\_score / rule\_count to *s.* |
+-----------------------------------+-----------------------------------+

Thus, the flat model simply computes the sum of the weights for the
Rules that passed as the score, and the sum of the weights of all the
applicable Rules as the maximum possible score. This model is simple and
easy to compute, but scores between different target systems may not be
directly comparable because the maximum score can vary. Tools should
support this model.

#### The Flat Unweighted Model

This model is identified by the URI "urn:xccdf:scoring:flat-unweighted".
It is computed exactly the same way as the flat model, except that all
weights not set to 0 are taken to be 1.0. Items with weights of 0 remain
0 in this model and, as such, do not contribute to the final score.
Essentially, the model computes the number of rules that passed. Tools
should support this model.

#### The Absolute Model

This model is identified by the URI "urn:xccdf:scoring:absolute". It
gives a score of 1 only when all applicable Rules in the benchmark pass,
and 0 otherwise. It is computed by applying the Flat Model and returning
1 if *s*=*m*, and 0 otherwise. Tools may support this model.

