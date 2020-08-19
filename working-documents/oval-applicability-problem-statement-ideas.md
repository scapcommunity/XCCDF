# Add/Improve OVAL Applicability

OVAL doesn’t currently offer an applicability mechanism that returns a result `not applicable` when the definition is not applicable to the scan target. Instead, definitions often return a misleading `true` or `false` result when they are evaluated against inapplicable targets. At best, this is confusing. In practice, it can easily lead to inaccurate results.

Content authors need a reliable way to indicate the applicability of a definition to specific operating systems(s), hardware architecture(s), applications, etc. such that when definitions are run on targets that they haven't been written for (that the author might consider "not applicable") they will yield a `not applicable` result.

## An Example

Consider a vulnerability definition for "FooBrowser" running on Windows 2012 R2 (32-bit) with a single test returns true (vulnerable) if a specific file/version is found. The author only researched and tested this definition to work on 2012 R2 (32-bit):

 - if evaluated against another variant of Windows (Windows 10, Server 2016, 2012 R2 32-bit), it will return `true` or `false`... but we have no idea if those are valid results. 
 - if evaluated against another platform entirely (Linux, macOS, etc.), it may return `true` or `false` or `not applicable` depending on the implementation. 
 
Suppose the author ANDs the above test with criteria testing for Windows Server 2012 R2 (32-bit), like most real-world content authors currently do:
 - if evaluated against another variant of Windows, it will return `false` (not vulnerable)... even if the device is in fact vulnerable 
 - if evaluated against another platform entirely (Linux, macOS, etc.), it may return `false` or `not applicable` depending on the implementation

Consistently returning `not applicable` on all platforms other than the ones the author intended would avoid these misleading (arguably inaccurate) results.

## Existing, Related OVAL Features

There are several existing OVAL features related to applicability, but they don't address the current requirement. Here is a brief refresher on each, for the sake of clarity.

### Not Applicable Result

As currently defined by the specification, a `not applicable` result value "indicates that the specified OVAL Object is not applicable to the system under test." The schema includes a bit more detail:

> For example, trying to collect Linux RPM information on a Windows system is not possible and so a result of not applicable is used. Another example would be in trying to collect RPM information on a linux system that does not have the RPM packaging system installed.

This is useful, but too narrow for the purposes considered here.

### Affected Family

The `definition/metadata/affected@family` attribute is required, but is of limited utility for these purposes because:
- it isn't specific enough to indicate most specific operating systems (e.g. Windows 10, RHEL 7, etc.)
- it is metadata and is not specified to impact the results

### Affected Platform and Product

The `definition/metadata/affected/platform` and `definition/metadata/affected/product` are of limited utility for these purposes because:
- the values are strings (not an enumeration) and therefore unreliable for automated consumption
- according to the specification they are metadata and do not impact the results
- they apply to the whole definition and do not allow authors to specify the applicability of a subset of criteria

### The Applicability Check Attribute

The `criteria@applicability_check`, `criterion@applicability_check` and `extend_definition@applicability_check` are currently "under-specified":

> A boolean flag that when ‘true’ indicates that the criteria is being used to determine whether the OVAL Definition applies to a given system. No additional meaning is assumed when ‘false’.

It is not specified to impact definition results, so... it's unclear when this attribute should be used and what impact is might have.

### XCCDF platform applicability

This isn't a part of OVAL, but can be used to address some of the requirements discussed herein but only when SCAP (XCCDF + OVAL) Benchmarks are being evaluated. Vulnerability, patch and inventory content are commonly developed, distributed and evaluated as OVAL definitions without XCCDF.



## Additional Related Issues & Opportunities

The primary goal is to provide an applicability mechanism that returns `not applicable` when the definition is not applicable to the scan target and thereby avoid returning a misleading `true` or `false` result. 

### Filtering Definitions

OVAL content is typically filtered by OS major version before evaluation for several important reasons:
- to reduce file size (the current OVAL Repository CVE package is ~135M; filtered to Server 2016 it is ~22m)
- to reduce scan times and output file sizes (avoids massive numbers of inapplicable definitions)
- to avoid misleading/inaccurate results from running inapplicable content

Currently, this filtering is typically by matching strings in the `definition/metadata/affected/platform`, which is not reliable, or by the content creator (i.e. the content creator generates pre-filtered packages).

A solution to the applicability issues discussed here could also provide a reliable, general purpose mechanism for filtering OVAL content by OS, architecture, application, etc.

### OVAL Inventory Content Re-use

The most commonly re-used OVAL definitions are inventory definitions for common operating systems, architectures, applications. However, there is currently no reliable or automation-friendly way to identify these definitions within a body of content. Most content authors end up filtering down to inventory definitions with matching `affected/platform` or `affected/product` and then reading through the titles until they find a match.

A solution to the applicability issues discussed here could also provide a reliable, automation-friendly mechanism for content authors to identify inventory definitions for the purpose of re-use.

### Pre-evaluation of Applicability Using Inventory Data 

OVAL content consumers may have access to inventory data before running a scan. For example, they may have a CMDB containing OS versions and application inventory for devices to be scanned. With that information, it should be possible to evaluate (or partially) evaluate OVAL content in advance of the actual scan.

A solution to the applicability issues discussed here could also provide a reliable mechanism for correlating functional components of OVAL definitions (e.g. criteria) with external inventory identifiers (e.g. SWID, CPE, etc.) to enable this use case.

### Short-circuiting Inapplicable Collection

Assuming we have solved the applicability problem, we should consider modifying the specification to allow OVAL engines to skip evaluation of criteria that have been determined to be inapplicable. For example, consider a definition with these criteria:

- OR:
  	- AND:
    	- Applicability Test: Windows Server 2012 R2
    	- Configuration tests for Windows Server 2012 R2
  	- AND:
    	- Applicability Test: Windows Server 2019
    	- Configuration tests for Windows Server 2019

If an applicability test evaluates to "not applicable", we could allow scanners to skip the evaluation of their sibling configuration tests. This could dramatically reduce scan times and data collection volumes.

As a reminder, currently OVAL requires engines to collect sibling criteria even when they cannot impact the results. For example:

- AND:
	- Feature A is enabled
	- Feature B is enabled

Even if Feature A comes back false--at which point collection of Feature B is not necessary to determine the result--engines must collect Feature B. This is valuable because, for example, auditor may wish to know every reason for failure and not the first.

However, this justification does not clearly apply to inapplicable criteria.

### Enabling Easier Identification of Root Causes of Results

Currently, is it possible to identify the criteria path that (most likely) determined the actual result, but it is often challenging to summarize that path succinctly. For example, consider this tree of criteria results:

- AND:
	- TRUE: The installed operating system is part of the Microsoft Windows family
	- TRUE: Windows Server 2012 R2 is installed
	- TRUE: DNS is enabled
	- TRUE: Dnsapi.dll (32-bit) (6.x) version is less than 6.3.9600.19780
	
A person would recognize the first 3 criteria as applicability criteria and summarize the root cause by saying something like: "You need to update Dnsapi.dll"

However, because OVAL does not currently, reliably identify applicability criteria as such, we cannot programmatically differentiate between these criteria and it's challenging to summarize more succinctly than: "The installed operating system is part of the Microsoft Windows family and Windows Server 2012 R2 is installed and DNS is enabled and Dnsapi.dll (32-bit) (6.x) version is less than 6.3.9600.19780."

A solution to the applicability issues discussed here could provide a reliable mechanism for tooling to differentiate between applicability criteria and configuration criteria, allowing for more useful summarization causation.


# Notional Solution

At a high-level, we could:

- Use inventory definitions to identify applicable OS version, architecture, etc.
- Add a new `applicability_definition` element that's just like `extend_definition` but only for referencing inventory for applicability purposes
- Refine the specification (truth tables) such that we get `not applicable` results when appropropriate

Here is a bit more detail:

  
## 1. Add New < applicability_definition > Element

- Almost identical to `extend_definition`: a child of `criteria` with `@definition_ref`, `@negate`, and `@comment`
- `<applicability_definition>` will return a `not applicable` result instead of `false`
- When AND'd, `not applicable` results will override other sibling criteria results
  

## 2. Deprecate Overlapping, Unnecessary Features
- deprecate `extend_definition@applicability_check`, `criteria@applicability_check` and `criterion@applicability_check`
- deprecate metadata/affected and metadata/affected/*

## 3. Add Optional < subject > Element to Inventory Definitions

This new tag would be necessary to address the additional opportunities discussed above (filtering, re-use, pre-evaluation, and root cause identification).

- new optional child element of /definition@class=inventory
- `subject@type`: one of the following (required):
  - `family`: this is an OVAL family inventory def (e.g. Windows)
  - `operating system`: this is an os inventory def (e.g. Windows Server 2012 R2 32-bit)
  - `application`: this is an application inventory def (e.g. Adobe Acrobat DC Classic)
  - `function`: this is a specific function inventory def (e.g. a feature or role like 'Domain Controller')
  - `other`: this is an other inventory def
- optional child elements:
  - reference elements (for CPE, SWID, etc.)
  
#### Examples

```
<definition class="inventory" id="oval:com.sample:def:1" version="1">
    <metadata>
        <title>Microsoft Windows Server 2016 is installed</title>        
    </metadata>
    <subject type="operating system">
         <reference ref_id="cpe:/o:microsoft:windows_server_2016:-" source="CPE"></reference>
    </subject>
    ...
</definition>

<definition class="inventory" id="oval:com.sample:def:2" version="1">
    <metadata>
        <title>Adobe Acrobat DC Classic is installed</title>
    </metadata>
    <subject type="application">
        <oval-def:reference ref_id="cpe:/a:adobe:acrobat_dc:::classic" source="CPE" />
    </subject>        

    ...
</definition>

<definition class="inventory" id="oval:com.sample:def:3" version="1">
    <metadata>
        <title>Microsoft .NET Framework 2.0 Service Pack 2 is installed</title>
    </metadata>
    <subject type="application">
        <reference ref_id="cpe:/a:microsoft:.net_framework:2.0:sp2" source="CPE" />
    </subject>
    ...
</definition>

<definition class="inventory" id="oval:com.sample:def:4" version="1">
    <metadata>
        <title>Microsoft .NET Framework 4.7.2 is installed</title>
    </metadata>
    <subject type="application">
        <reference ref_id="cpe:/a:microsoft:.net_framework:4.7.2" source="CPE" />
    </subject>
    ...
</definition>
```

  
## Questions:
- support inventory for versions/editions of OS/applications?
- how to address hardware / architecture? leave as part of OS name, or separate?


