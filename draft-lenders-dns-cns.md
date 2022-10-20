---
title: "Guidance on DNS message composition in constraint networks"
abbrev: "TODO - Abbreviation"
category: bcp

docname: draft-lenders-dns-cns-latest
number:
date:
consensus: true
v: 3
submissiontype: IETF
area: "Applications"
workgroup: TBD
keyword:
 - Internet-Draft
 - CBOR
 - DNS
venue:
  group: TBD
  type: Working Group
  mail: TBD@example.com
  arch: "nicfs.nic.ddn.mil:~/namedroppers/*.Z"
  github: "anr-bmbf-pivot/draft-lenders-dns-cns"
  latest: "https://anr-bmbf-pivot.github.io/draft-lenders-dns-cns/draft-lenders-dns-cns.html"

author:
 -  fullname: Martine Sophie Lenders
    organization: Freie Universität Berlin
    abbrev: FU Berlin
    email: m.lenders@fu-berlin.de
 -  fullname: Thomas C. Schmidt
    organization: HAW Hamburg
    email: t.schmidt@haw-hamburg.de
 -  fullname: Matthias Wählisch
    organization: Freie Universität Berlin
    abbrev: FU Berlin
    email: m.waehlisch@fu-berlin.de

normative:

informative:
  RFC7228: cns
  RFC4944: 6lowpan
  RFC8724: schc

--- abstract

This document provides guidance on the composition of DNS messages in low power and lossy
networks, where payload sizes can be heavily restricted by the link layer.

--- middle

# Introduction

Constrained networks {{-cns}}, such as IEEE 802.15.4 or LoRaWAN, often come with restrictions
to the protocol data unit (PDU) in the link layer. While adoption layers such as 6LoWPAN
{{-6lowpan}} and SCHC {{-schc}} provide mitigation in the form of compression and fragmentation
to allow for IPv6 traffic, restricting the application data to a minimum size greatly increases
reliability. This document provides guidance on the composition of DNS messages to prevent
fragmentation in constrained networks.


# Terminology

TBD

{::boilerplate bcp14-tagged}

# Constrained Node Considerations

Nodes within an constrained networks are only to assumed stub resolvers.
That means they only query information from an upstream DNS server but MUST NOT distribute DNS
information, possibly received from an upstream DNS server, themselves.

# DNS Server Consideration

A DNS server that is aware that the querying node is a node within a constrained network SHOULD
resolve a CNAME
until the originally requested resource record type is reached. This reduces the number of message
exchanges within a constrained network.

The DNS server SHOULD send compact answers, i.e., additional or authority sections in the DNS
response should only be sent if needed or if it is anticipated that they help the DoC client to
reduce additional queries.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:unnumbered}

TODO acknowledge.

- Carsten Bormann
- Ben Schwartz
