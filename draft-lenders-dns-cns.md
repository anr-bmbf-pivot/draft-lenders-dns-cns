---
title: "Guidance on DNS Message Composition in Constrained Networks"
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
  RFC1035: dns

informative:
  RFC4944: 6lowpan
  RFC7228: cns
  RFC7858: dot
  RFC8484: doh
  RFC8724: schc
  I-D.ietf-core-dns-over-coap: doc

--- abstract

This document provides guidance on the composition of DNS messages in constrained
networks, where payload sizes can be heavily restricted by the link layer and power consumption must
be taken into account.

--- middle

# Introduction

Constrained networks {{-cns}}, such as IEEE 802.15.4 or LoRaWAN, often come with restrictions
to the protocol data unit (PDU) in the link layer.
Adoption layers such as 6LoWPAN {{-6lowpan}} and SCHC {{-schc}} provide mitigation in the form of
compression and fragmentation to allow for IPv6 traffic.
However, restricting the application data to a minimum size greatly increases reliability by
decreasing potentially lossy fragmentation.

Constrained nodes {{-cns}} are furthermore often battery powered and follow a strict duty cycle.
To reduce unnecessary device wake-ups, the number of DNS messages should be reduced to a minimum.

This document provides guidance on the composition of DNS messages to prevent fragmentation and
reduce power consumption in constrained networks.


# Terminology

A "DNS server" is a server that provides DNS information to a querying client.
For the purpose of this document this term may refer to queries over any transport for DNS, not just
classic DNS {{-dns}}, but also transports such as DNS over TLS {{-dot}}, DNS over HTTPS {{-doh}}, or
DNS over CoAP {{-doc}}.

The terms "constrained node" and "constrained network" are used as defined in {{-cns}}.

{::boilerplate bcp14-tagged}

# Constrained Node Considerations {#sec:node-considerations}

Nodes within an constrained networks are only to assumed stub resolvers.
That means they only query information from an upstream DNS server but MUST NOT distribute any DNS
information, received from an upstream DNS server, themselves.

# DNS Server Consideration

A DNS server that is aware that the querying node is a node within a constrained network SHOULD
resolve a CNAME or PTR record until the originally requested resource record type is reached.
This reduces the number of message exchanges within a constrained network.

The DNS server SHOULD send compact answers, i.e., additional or authority sections in the DNS
response MAY only be sent if needed or if it is anticipated that they help the DoC client to
reduce additional queries.


# Security Considerations

In the case when DNS clients act themselves as DNS servers, resolving CNAME and PTR records at the
upstream DNS server may lead to the DNS client forwarding incorrect DNS information to its clients.
As such, this document prohibits the distribution of such information in {{sec:node-considerations}}

TODO more security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:unnumbered}

TODO acknowledge.

- Carsten Bormann
- Ben Schwartz
