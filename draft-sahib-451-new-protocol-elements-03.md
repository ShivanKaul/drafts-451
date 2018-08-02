---
title: New protocol elements for HTTP Status Code 451
abbrev: New elements for HTTP 451
docname: draft-sahib-451-new-protocol-elements-03
date: 2018-08-01
category: info

<!-- area: IRTF -->
ipr: trust200902
workgroup: Network Working Group
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

  ERRATA_ID-5181:
    title: "[Technical Errata Reported] RFC7725 (5181)"
    date: 2017
    author:
      - ins: S. Bortzmeyer
    target: https://www.rfc-editor.org/errata/eid5181

  ISO.3166-1:
    title: "Codes for the representation of names of countries and their subdivisions - Part 1: Country code"
    date: 1997
    author:
      - org: International Organization for Standardization
    seriesinfo:
      ISO Standard 3166- 1:1997

  RFC7725:

  RFC8288:
    

informative:

  AUTOMATTIC_COUNTRY_BLOCK_LIST:
    title: "Automattic - Country Block List"
    date: 2018
    target: https://transparency.automattic.com/country-block-list/

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

   

--- abstract

This document recommends additional protocol elements to Hypertext Transfer Protocol (HTTP) status
code 451 (defined by RFC7725). 

Discussion of this document was conducted on the Human Rights Protocol Considerations Research Group mailing list <https://www.irtf.org/mailman/listinfo/hrpc>, briefly on the HTTPBIS Working Group mailing list <ietf-http-wg@w3.org> and on <https://lists.ghserv.net/mailman/listinfo/statuscode451>.


--- middle


# Introduction

{{RFC7725}} was standardized by the IETF in February 2016. It defined HTTP status code 451 - to be used when "a server operator has received a legal demand to deny access to a resource or to a set of resources".

This document attempts to outline protocol recommendations that would help make the status code more useful to users.


# New Protocol Elements

## Blocking Authority

An HTTP response with status code 451 should include a "Link" HTTP header field {{RFC8288}} which has a "rel" parameter whose value is "blocking-authority", in addition to the "blocked-by" header specified in {{RFC7725}}. It's important to distinguish between the implementer of the block, and the authority that mandated the block in the first place {{ERRATA_ID-5181}}. This is because these two organizations might not be the same - a government (the blocking authority) could force an Internet Service Provider (the implementer of the block) to deny access to a certain resource. Both provide essential information about the legal block. 


## Geographical Scope of Block

HTTP status code 451 is increasingly being used to deny access to resources based on  geographical location. The response should contain a provisional header named "geo-scope-block" that specifies the countries in which a resource is blocked. This scope should correspond to a comma-separated list of alpha-2 country codes defined in {{ISO.3166-1}}. The rationale for keeping the geographical scope to country-level granularity is that most blocks are mandated by national governments {{IMPL_REPORT}}, {{AUTOMATTIC_COUNTRY_BLOCK_LIST}}. 

# Security Considerations

This document does not add additional security considerations to {{RFC7725}}.

# IANA Considerations

The Link Relation Type Registry should be updated with the following entry:

- Relation Name: blocking-authority
- Description: Identifies the authority that has issued the block.
- Reference: this document

In addition, IANA should be updated with the following provisional header:

- Header field name: geo-scope-block
- Applicable protocol: http
- Status: provisional
- Specification document(s): this document


Acknowledgements
================
{: numbered="no"}

Thanks to Alp Toker, Niels ten Oever, Stephane Bortzmeyer, Corinne Cath, Christine Runnegar, and many others on the HRPC mailing list (linked above) for reviewing and brainstorming.
