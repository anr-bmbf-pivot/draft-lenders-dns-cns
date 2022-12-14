---
title: "Guidance on DNS Message Composition in Constrained Networks"
abbrev: "DNS in Constrained Networks"
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
  RFC8499: dnsterm
  RFC6763: dnssd
  I-D.ietf-core-dns-over-coap: doc

--- abstract

This document provides guidance on the composition of DNS messages in
constrained networks, where the link layer may restrict payload sizes
significantly and batteries challenge power consumption.

--- middle

# Introduction

Many IoT scenarios rely on constrained nodes {{-cns}} and need the Domain
Name System (DNS).  Constrained nodes {{-cns}}, however, challenge DNS
resolution for two reasons. First, IoT networks, such as IEEE 802.15.4 and
LoRaWAN, often limit the payload of the data link layer significantly in
terms of size, which prevents common (larger) DNS responses.  Second,
constrained nodes are often battery powered and follow a strict duty cycle,
which would benefit from minimal number of DNS messages to reduce
unnecessary device wake-ups.  Adoption layers such as 6LoWPAN {{-6lowpan}}
and SCHC {{-schc}} provide fragmentation and compression to overcome the
problem, but do not help in principle. Fragmentation requires more buffer
space to account for lost fragments in lossy networks, and compression
introduces additional processing overhead.

This document provides best common practices on DNS behavior, to reduce
fragmentation and power consumption in constrained networks when IoT nodes
resolve names.


# Terminology

A "DNS server" is a server that provides DNS information to a querying DNS
client.  For the purpose of this document, server and client may
communicate based on any DNS transport, not just DNS over UDP {{-dns}}, but
also DNS over TLS {{-dot}}, DNS over HTTPS {{-doh}}, or DNS over CoAP
{{-doc}}.

The terms "constrained node" and "constrained network" are used as defined in {{-cns}}.

{::boilerplate bcp14-tagged}

# Constrained Resolver Considerations {#sec:node-considerations}

Nodes within a constrained network that implement DNS are assumed to be
stub resolvers {{-dnsterm}}.  This means they only query information from a
recursive DNS resolver and they MUST NOT distribute any DNS information
received from an upstream DNS server.

# DNS Server Consideration

A DNS server that is aware that the querying node is a node within a
constrained network SHOULD resolve a CNAME or PTR record until the resource
record type originally requested by the node is reached.
This reduces the number of message exchanges within a constrained network.

The DNS server SHOULD send compact answers, i.e., omit additional or
authority sections in a DNS reply. Additional and authority sections should
only be included if they help a DNS client to reduce queries.
One such example is DNS-SD {{-dnssd}}, where the answer does not only need
to include an SRV record but also TXT and A/AAAA records to make decent use
of the reply.

TBD: Provide more specific example use case? Should the list be almost
complete?


# Security Considerations

In the case when DNS clients act as DNS servers, resolving CNAME and PTR
records at the upstream DNS server may lead to incorrect DNS information
forwarded by the client.  As such, this document prohibits the distribution
of such information in {{sec:node-considerations}}

TODO more security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:unnumbered}

TODO acknowledge.

- Carsten Bormann
- Ben Schwartz
