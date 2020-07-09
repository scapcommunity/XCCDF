
# XCCDF Extensions (Issue #16)

_Note: this is intended to be a conversation-starter to help us collaboratively define the capabilities we want to add to XCCDF before creating an actual proposal._

## Overview

We will add a new capability, XCCDF Extension, that will allow more extensive customization of an XCCDF Benchmark than is possible using XCCDF Profiles. For example:

- Customizing the XCCDF metadata (titles, descriptions, etc.)
- Adding Rules and check content

This new capability will be additive without replacing XCCDF Profiles and Tailorings, because they serve subtly different functions:

- XCCDF Profiles & Tailorings: limited customizations that remain within the general intent of the original author.
- XCCDF Extensions: extensive customizations that may not remain within the intent of the original author.

## Processing Notes

When an Extension is applied:

- All XCCDF IDs will be changed to use the Extension ID namespace
- All Rules will include an 'ident' indicating the original Rule LRI (see [PR28](https://github.com/scapcommunity/XCCDF/pull/28))
- The Extension's title should be used in place of the extended Benchmark's title in results reporting
- Results will clearly indicate that the Extension has been applied (similar to Tailoring)
- ARF results should include:
  - Benchmark: the result of the applied Extension
  - Benchmark\TestResult: standard processing of the parent Benchmark
  - Extension: the applied XCCDF:Extension element so it's easy to see what was changed
  - All check-content documents used including any added by `Add-Check-Content` elements
  - ? the original Benchmark as well ? 
  

## Elements & Capabilities

### Extension Element (`<Extension>`)

The Extension element includes metadata about the extension and child elements which define the specific content extensions, including:

- `@id_namespace` (required): a reverse-DNS style string that will be applied to all XCCDF IDs when this Extension is used. This must be different than the id namespace used in the extended Benchmark.
- `<title>` (required): a title that will override the extended Benchmark's title in results
- general metadata elements and attributes analogous to those of `<xccdf:Profile>`: `<status>`, `<dc-status>`, `<version>`, `<description>`, `<reference>`, `<platform>`, `<metadata>`, `<signature>`, `@id`, `@extends`, `@xml:base`, `@Id` 


### Add-Groups (`<add-groups>`), Add-Rules (`<add-rules>`), Add-Values (`<add-values>`), Add-Profiles (`<add-profiles>`)

A new element providing the capability to add new Group(s), Rule(s), Value(s), or Profile(s) with the following attributes:
  - `@mode`: before, after, prepend, or append (default)
  - `@location`: id of valid sibling element (if before/after) or parent element (if prepend/append). Note: if not specified elements will be inserted at the earliest (if before/prepend) or latest (if after/append) valid location. 
  
Each Add-Group, Add-Rules, Add-Values, or Add-Profiles element would contain one or more Groups, Rules, Values, or Profiles (respectively) along with all their children.


### Replace-Group (`<replace-group>`), Replace-Rule (`<replace-rule>`), Replace-Value (`<replace-value>`), Replace-Profile (`<replace-profile>`)

A new element providing the capability to replace an entire Group, Rule, Value, or Profile with the following attributes:
  - `@id`: the XCCDF id of the element to be replaced
  
Each Replace-Group, Replace-Rule, Replace-Value, or Replace-Profile element would contain a valid Group, Rule, Value, or Profile (respectively) along with all its children.


### Revise-Benchmark (`<revise-benchmark>`), Revise-Group (`<revise-group>`), Revise-Rule (`<revise-rule>`), Revise-Value (`<revise-value>`), Revise-Profile (`<revise-profile>`)

A new element providing the capability to revise a single Benchmark, Group, Rule, Value, or Profile element with the following attributes:
  - `@id`: the XCCDF id of the element to be revised
  
Each Revise-Benchmark, Revise-Group, Revise-Rule, Revise-Value, or Revise-Profile element would have at least one valid attribute or child element of a Benchmark, Group, Rule, Value, or Profile (respectively) with the exception of following: `<Profile>`, `<Value>`, `<Group>`, `<Rule>`, `<TestResult>`, `<signature>`, `@id`, `@Id`, `@resolved`.

When applied, the attributes and elements would be interpolated as follows:
  - if an attribute/element does not exist in the original, it will be added
  - if an attribute exists in the original, its value will be replaced
  - if an element that may occur only once exists in the original, it will be replaced
  - if an element that may occur multiple times exists in the original, the new value will be interpolated using simple, element-specific logic TBD (`<title>` will be replaced if xml:lang is same)


### Remove-Group (`<remove-group>`), Remove-Rule (`<remove-rule>`), Remove-Value (`<remove-value>`), Remove-Profile (`<remove-profile>`)

A new element providing the capability to remove a Group, Rule, Value, or Profile with the following attributes:
  - `@id`: the XCCDF id of the element to remove
  
This element has no children.


### Add-Check-Content (`<add-check-content>`)

A new element providing the capability to add check content inline or by reference with the following attributes:
  - `@href`: the location of the check content (e.g. file or URI) if the content is not provided inline
  - `@idref`: the idref be used by `<xccdf:check-content-ref>` elements when referring to this check

If element does not contain check content inline, then the `@href` will be used to retrieve it from (e.g. from the filename or URI specified). 


### Revise-Referenced-Check-Content (`<revise-referenced-check-content>`)

A new element providing the capability to replace or append arbitrary elements within a specific piece of check-content. Some specification will likely need to be specific to each check-system.

The element would have required `@system` and `@href` attributes specifying the system and document, respectively.

For OVAL (`@system=='http://oval.mitre.org/XMLSchema/oval-definitions-5'`), the `<refine-referenced-check-content>` element would contain 1-n valid OVAL elements (`<definition>`, `<test>`, `<object>`, `<state>`, `<variable>`). When the child element@id already exists in the OVAL document, the element will replace the existing element. When the child element@id does not exist in the OVAL document, the element will be appended to the appropriate section of the OVAL document.

#### Example:
```xml
<revise-referenced-check-content system="http://oval.mitre.org/XMLSchema/oval-definitions-5" href="oval.xml">

    <win:file_object id="oval:com.example:obj:1" version="0" comment="mf.dll">
        <path var_ref="oval:com.example:var:1" var_check="at least one"/>
        <filename>mf.dll</filename>
    </win:file_object>

    <win:file_state comment="Version is 4" id="oval:com.example:ste:1" version="1">
        <version datatype="version" operation="equals">4</version>
    </win:file_state>

</revise-referenced-check-content>
```
