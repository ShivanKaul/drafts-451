---
title: New protocol elements for HTTP Status Code 451
abbrev: New elements for HTTP 451
docname: draft-sahib-451-new-protocol-elements-00
date: 2018-03-19
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

  RFC2119:

  RFC8280:

  RFC2277:

  RFC7725:

  IMPL_REPORT_DRAFT:
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
    

  RFC8288:
    

informative:

   

--- abstract

This draft recommends protocol updates to Hypertext Transfer Protocol (HTTP) status
code 451 (defined by RFC7725) based on an examination of how the new status code is being used by parties involved in denial of Internet resources because of legal demands. Also included is an analysis of HTTP 451 from a human rights perspective using guidelines from RFC8280.

Discussion of this draft is at <https://www.irtf.org/mailman/listinfo/hrpc> and <https://lists.ghserv.net/mailman/listinfo/statuscode451>.


--- middle


# Introduction

{{RFC7725}} was standardized by the IETF in February 2016. It defined HTTP status code 451 - to be used when a "a server operator has received a legal demand to deny access to a resource or to a set of resources".

Subsequently, an effort was made to investigate usage of HTTP 451 and evaluate if it fulfills its mandate of providing "transparency in circumstances where issues of law or public policy affect server operations" {{IMPL_REPORT_DRAFT}}. This draft attempts to explicate the protocol recommendations arising out of that investigation.


# Requirements

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in {{RFC2119}}.


# Existing Protocol Elements

The status code as standardized by the IETF specifies the following elements {{RFC7725}} -

- A server can return status code 451 to indicate that it is denying access to a resource or multiple resources on account of a legal demand.
- Responses using the status code SHOULD include an explanation in the response body of the details of the legal demand.
- Responses SHOULD include a "Link" HTTP header field {{RFC8288}} whose value is a URI reference {{RFC3986}} identifying itself.  The "Link" header field MUST have a "rel" parameter whose value is "blocked-by". The intent is that the header be used to identify the entity actually implementing blockage, not any other entity mandating it.


# Recommendations

- In addition to the "blocked-by" header, an HTTP response with status code 451 SHOULD include another "Link" HTTP header field which has a "rel" parameter whose value is "blocking-authority". It's important to distinguish between the implementer of the block, and the authority that mandated the block in the first place. This is because these two organizations might not be the same - a government (the blocking authority) could force an Internet Service Provider (the implementer of the block) to deny access to a certain resource. {{IMPL_REPORT_DRAFT}} notes that  there is confusion amongst implementors of {{RFC7725}} about the meaning of the term "blocked-by", perhaps in part because of the example given {{ERRATA_ID-5181}} - it would be useful to explicitly include space for both, as both provide essential information about the legal block.
- HTTP status code 451 is increasingly being used to deny access to resources based on geographical IP. The response SHOULD contain a provisional header with geographical scope of block. This scope SHOULD correspond to alpha-2 country code defined in {{ISO.3166-1}}. The rationale for keeping the geographical scope to country-level granularity is that most blocks are mandated by national governments {{IMPL_REPORT_DRAFT}}. In addition, as the serving of this status code is voluntary, there is value in not complicating the specification.

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

# Human Rights Considerations

The use of HTTP 451 has implications for human rights, and is thus worth examining under the guidelines for developing human rights considerations for protocols {{RFC8280}}. 


## Connectivity

HTTP 451 status code response can be sent by the server hosting the blocked resource (end node) or an intermediary node that implements the block. If a resource is "blocked-by" a particular ISP then the ISP will be the one serving the status code. 


## Privacy

HTTP 451 is used as a response when the client requests a resource that's unavailable because of legal reasons. Consequentially, there are potential privacy (and anonymity) ramifications if there is leakage of who accessed what resource. 

HTTP status code 451 is cacheable by default {{RFC7725}}. Caching status code 451 on users' devices means that there will be a record of their attempt to access the blocked content stored on their devices. If caching is used, the software used to access the web resource should notify users that the HTTP 451 response has been received and cached.


## Content Agnosticism

The scope of the blocking order may be geographical in nature. This results in an issue related to content agnosticism. This is not a protocol issue, but rather an artefact of the blocking order.


## Security

Refer to section 5, Security Considerations, of {{RFC7725}}.


## Internationalization

{{RFC7725}} does not require the use of any particular language for user-facing strings such as the response body or the value of the header fields. The header field names, however, are in English. This is a common attribute of protocol elements within the IETF {{RFC2277}}. 


## Censorship Resistance

While HTTP 451 status code cannot prevent censorship, it can help make censorship more transparent and make assessment of Internet censorship cases easier by providing information about the block in a parseable manner.


## Open Standards

{{RFC7725}} is an open standard.


## Heterogeneity Support

An HTTP 451 status code response can be used for any web resource and for any software that is capable of understanding HTTP header responses.


## Anonymity

HTTP 451 is an HTTP status code. If the connection between the client and the server is not over HTTPS, then there is potential for a network observer to identify what a particular user is accessing. Even with HTTPS, meta-data might be available that could be used to identify a user.


## Pseudonymity

HTTP 451 does not involve pseudonyms or nicknames.


## Accessibility

HTTP 451, being an HTTP status code, does not prescribe how the client should parse the information contained in the response fields. Depending on the software that is used to access the resource, there could be accessibility concerns related to understanding this information.


## Localization

The values of the header fields and the response body can be localized by the blocker to suit the geographical scope of the block. 


## Decentralization

HTTP 451 is used to provide information about a block mandated by a (centralized) blocking authority. The status code itself does not add any additional centralization. 


## Reliability

HTTP 451 does not by itself prevent the misuse of the status code or wrong tagging of resources. The informational requirement as part of the protocol address this concern to some extent, but commonly it will be difficult for the end user to verify if the code has been correctly used. Objective of this draft is to increase reliability by making it clearer what a good HTTP 451 response looks like.


## Confidentiality

HTTP 451 status code use implies sharing of information by the reporter that make it easier to identify where censorship is taking place. See section on Privacy and Anonymity.


## Integrity

HTTP 451 does not prescribe the use of any encryption to transport it, and is thus susceptible to leakage through man-in-the-middle attacks.


## Authenticity

Authenticity of the server implementing HTTP 451 is dependent on the connection being over HTTPS.


## Adaptability

HTTP 451 does not have any legal or technical limitations which prevents the development of other standards / protocols.


## Outcome Transparency

The assumption behind the development of the status code 451 is that transparency has a chilling effect on censorship and that transparency will enable the process of justice by allowing acts of censorship to be challenged. In some countries, blocks orders are unevenly implemented by ISPs either because it does not serve their bottom-lines or they are resisting censorship - governments in those countries could mandate the implementation of status code 451 which will make it easier for them to monitor the implementation of their block orders. Surveillance systems in some countries could be updated to watch out for the 451 error code on unencrypted traffic making it easier to identify those trying to access prohibited content. Before the implementation of this standard there would be no uniformity in which websites would implement a block order increasing the number of false positives for any automated monitoring systems.
