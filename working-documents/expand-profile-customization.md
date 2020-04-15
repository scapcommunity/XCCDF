# Expand Profile-Customizable Elements (Issue #16)

This is intended to be a conversation-starter not a proposal!

## Current Profile Customization Capabilities

Here is a summary of existing capabilities in XCCDF 1.2:

- `<select>` element
  - applies to a Rule element, Group element or cluster of Rule and Group elements
  - override @selected attribute
  - may include <remarks>
- `<refine-rule>` element
  - applies to a Rule element, Group element or cluster of Rule and Group elements
  - override @weight, @severity and/or @role of Rule(s)
  - override @weight of Group(s)
  - apply a selector 
- `<set-value>` element
  - applies to a Value element or a cluster of Value elements
  - overrides <value> property
- `<refine-value>` element
  - applies to a Value element or a cluster of Value elements
  - apply a selector and/or change operator property
- `<set-complex-value>` element
  - omitting detailed discussion of complex values as they are explicitly unsupported in SCAP


## Proposed Profile Customization Capabilities

_Note: we might consider deprecating `<select>`, `<set-value>` and `<set-complex-value>` as their capabilities will be included in the new and revised elements below._

### Refine-Benchmark (`<refine-benchmark>`)

A new element providing the capability to override the following Benchmark attributes:
  - `@style`
  - `@style-href`
  - `@xml:lang`

And the capability to replace (in their entirety) or append the following Benchmark child elements (inheritance model TBD): 
  - `<title>`*
  - `<description>`*
  - `<notice>`*
  - `<front-matter>`*
  - `<rear-matter>`*
  - `<reference>`*
  - `<plain-text>`*
  - `<cpe2-platform-specification>`
  - `<platform>`*
  - `<version>`
  - `<metadata>`*
  - `<model>`*

_Note: the following are not customizable: `<Profile>`, `<Value>`, `<Group>`, `<Rule>`, `<TestResult>`, `<signature>`, `@id`, `@Id`, `@resolved`._

#### Example:
```xml
<refine-benchmark style="scap_12">

	<title>My Custom Title Here</title>
	<title xml:lang="fr">My Title (in French)</title>
	
</refine-benchmark>
```


### Refine-Group (`<refine-group>`)
A new element providing the capability to override the following Group attributes:
  - `@hidden`
  - `@prohibitChanges`
  - `@xml:lang`
  - `@selected`
  - `@weight`
  - `@selector`

And the capability to replace (in their entirety) or append the following Group child elements (inheritance model TBD): 
  - `<status>`*
  - `<dc-status>`*
  - `<version>`
  - `<title>`*
  - `<description>`*
  - `<warning>`*
  - `<question>`*
  - `<reference>`*
  - `<metadata>`*
  - `<rationale>`*
  - `<platform>`*
  - `<requires>`*
  - `<conflicts>`*

_Note: the following are not customizable: `abstract`, `cluster-id`, `extends`, `signature`, `@id`, `@Id`._


### Refine-Rule (`<refine-rule>`)

Update this element to provide the capability to override the following Rule attributes:
  - `@hidden`
  - `@prohibitChanges`
  - `@xml:Lang`
  - `@selected`
  - `@weight`
  - `@severity`
  - `@multiple`
  - `@selector`

And the capability to replace (in their entirety) or append the following Rule child elements (inheritance model TBD): 
  - `<status>`*
  - `<dc-status>`*
  - `<version>`
  - `<title>`*
  - `<description>`*
  - `<warning>`*
  - `<question>`*
  - `<reference>`*
  - `<metadata>`*
  - `<rationale>`*
  - `<platform>`*
  - `<requires>`*
  - `<conflicts>`*
  - `<ident>`*
  - `<impact-metric>`
  - `<profile-note>`*
  - `<fixtext>`*
  - `<fix>`*
  - `<check>`* 
  - `<complex-check>`

_Note: the following are not customizable: `abstract`, `cluster-id`, `extends`, `signature`, `@id`, `@Id`._


### Refine-Value (`<refine-value>`)

Update this element to provide the capability to override the following Value attributes:
  - `@id`
  - `@hidden`
  - `@prohibitChanges`
  - `@xml:Lang`
  - `@selector`

And the capability to replace (in their entirety) or append the following Value child elements (inheritance model TBD): 
  - `<status>`*
  - `<dc-status>`*
  - `<version>`
  - `<title>`*
  - `<description>`*
  - `<warning>`*
  - `<question>`*
  - `<reference>`*
  - `<metadata>`*
  - `<value>`+
  - `<complex-value>`*
  - `<default>`*
  - `<complex-default>`*
  - `<match>`*
  - `<lower-bound>`*
  - `<upper-bound>`* 
  - `<choices>`*
  - `<source>`*
  - `<type>`*
  - `<operator>`*
  - `<interactive>`*
  - `<interfaceHint>`*

_Note: the following are not customizable: `abstract`, `cluster-id`, `extends`, `signature`, `@id`, `@Id`._


### Refine-Referenced-Check-Content (`<refine-referenced-check-content>`)

A new element providing the capability to replace or append arbitrary elements within a specific piece of check-content. Some specification will likely need to be specific to each check-system.

The element would have required `@system` and `@href` attributes specifying the system and document, respectively.

For OVAL (`@system=='http://oval.mitre.org/XMLSchema/oval-definitions-5'`), the `<refine-referenced-check-content>` element would contain 1-n valid OVAL elements (`<definition>`, `<test>`, `<object>`, `<state>`, `<variable>`). When the child element@id already exists in the OVAL document, the element will replace the existing element. When the child element@id does not exist in the OVAL document, the element will be appended to the appropriate section of the OVAL document.

#### Example:
```xml
<refine-referenced-check-content system="http://oval.mitre.org/XMLSchema/oval-definitions-5" href="oval.xml">

    <win:file_object id="oval:com.example:obj:1" version="0" comment="mf.dll">
        <path var_ref="oval:com.example:var:1" var_check="at least one"/>
        <filename>mf.dll</filename>
    </win:file_object>

    <win:file_state comment="Version is 4" id="oval:com.example:ste:1" version="1">
        <version datatype="version" operation="equals">4</version>
    </win:file_state>

</refine-referenced-check-content>
```
