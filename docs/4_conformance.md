# 4.Conformance

Products and organizations may want to claim conformance with this specification for a variety of reasons. For example, a software vendor may want to assert that its product generates and/or processes XCCDF benchmark documents properly. Another example is a policy mandating that an organization use XCCDF for documenting and executing its security configuration checklists.

This section provides the high-level requirements that a product or benchmark document must meet for conformance with this specification. Most of the requirements listed in this section reference other sections in the report that fully define the requirements.

Other specifications that use XCCDF may define additional requirements and recommendations for XCCDF's use. Such requirements and recommendations are outside the scope of this publication.

## 4.1 Product Conformance

There are two types of XCCDF products: benchmark producers and benchmark consumers. _Benchmark producers_ are products that generate XCCDF benchmark documents, while _benchmark consumers_ are products that accept an existing XCCDF benchmark document, process it, and produce an XCCDF results document. Products claiming conformance with this specification SHALL adhere to the following requirements:

1. For benchmark producers, generate well-formed XCCDF benchmark documents. This includes following the benchmark document requirements specified in Section 4.2 and all of the pertinent processes defined in Sections 6 and 7.

1. For benchmark consumers, consume and process well-formed XCCDF benchmark documents, and generate well-formed XCCDF results documents. This includes following all of the pertinent processes defined in Sections 6 and 7.

1. Make an explicit claim of conformance to this specification in any documentation provided to end users.

## 4.2 Benchmark Document Conformance

XCCDF benchmark documents claiming conformance with this specification SHALL follow these requirements:

1. Adhere to the official XCCDF schema as explained in Section 6.

1. Adhere to the syntax, structural, and other XCCDF benchmark document requirements defined in Sections 6 and 7.
