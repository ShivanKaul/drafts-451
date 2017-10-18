---
title: New protocol elements for HTTP Status Code 451
docname: draft-new-protocol-elements-451
date: 2017-10-12
category: info

area: IRTF
workgroup: Human Rights Protocol Considerations Research Group
keyword: Internet-Draft

stand_alone: yes
pi: 
  rfcedstyle: yes
  toc: yes
  tocindent: yes
  sortrefs: yes
  symrefs: yes
  strict: yes
  comments: yes
  inline: yes
  text-list-symbols: -o*+

author:
-
     ins: S.K. Sahib
     name: Shivan Kaul Sahib
     email: shivankaulsahib@gmail.com

normative:

  RFC7725:

  IMPL_REPORT:
    title: "Implementation Report for HTTP Status Code 451"
    date: 2017
    author:
      - ins: S. Abraham
      - ins: MP. Canales
      - ins: J. Hall
      - ins: O. Khrustaleva
      - ins: N. ten Oever
      - ins: C. Runnegar
      - ins: S.K. Sahib
    target: https://tools.ietf.org/html/draft-451-imp-report-00


  RFC3986:
    

  RFC5988:
    

informative:

   

--- abstract

This draft recommends protocol updates to Hypertext Transfer Protocol (HTTP) status
code 451 {{RFC7725}} based on an examination of how the new status code is being used by parties involved in denial of Internet resources because of legal demands.


--- middle


Introduction
============
{{RFC7725}} was standardized by the IETF in February 2016. It defined HTTP status code 451 - to be used when a "a server operator has received a legal demand to deny access to a resource or to a set of resources that includes the requested resource". The intention was to provide a uniform mechanism to indicate online censorship. 

Subsequently, an effort was made to investigate usage of 451 status code and evaluate if it fulfills its mandate of "provid[ing] transparency in circumstances where issues of law or public policy affect server operations" {{IMPL_REPORT}}. This draft attempts to explicate the protocol recommendations arising out of that investigation.


Existing Protocol Elements
============

The status code as standardized by the IETF specifies the following elements {{RFC7725}} -

- A server can return status code 451 to indicate that it is denying access to a resource or multiple resources on account of a legal demand.
- Responses using the status code SHOULD include an explanation in the response body of the details of the legal demand.
- Responses SHOULD include a "Link" HTTP header field {{RFC5988}} whose value is a URI reference {{RFC3986}} identifying itself.  The "Link" header field MUST have a "rel" parameter whose value is "blocked-by". The intent is that the header be used to identify the entity actually implementing blockage, not any other entity mandating it.


Recommendations
============

- In addition to the "blocked-by" header, an HTTP response with status code 451 SHOULD include another "Link" HTTP header field which has a "rel" parameter whose value is "blocking-authority". It's important to distinguish between the implementer of the block, and the authority that mandated the block in the first place. This is because these two organizations might not be the same - a government (the blocking authority) could force an Internet Service Provider (the implementer of the block) to deny access to a certain resource. 
- HTTP status code 451 is increasingly being used to deny access to resources based on geographical IP. The scope of this denial is sometimes as finely scoped as a city or a province. The response SHOULD contain a provisional header with geographical scope of block.

IANA Considerations
============
The Link Relation Type Registry should be updated with the following entry:

- Relation Name: blocking-authority
- Description: Identifies the authority that has issued the block.
- Reference: This document

In addition, IANA should be updated with the following provisional header:

- Header field name: geo-scope-block
- Applicable protocol: http
- Status: provisional
- Specification document(s): this document