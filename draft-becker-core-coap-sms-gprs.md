---
stand_alone: true
ipr: trust200902
docname: draft-becker-core-coap-sms-gprs-latest
cat: std
pi:
  strict: 'no'
  toc: 'yes'
  tocdepth: '4'
  symrefs: 'yes'
  sortrefs: 'yes'
  compact: 'yes'
  subcompact: 'no'
title: Transport of CoAP over SMS
abbrev: CoAP over SMS
area: Application
wg: CoRE
kw: CoAP, SMS, M2M
date: 2017-01-24
author:
- role: editor
  ins: M. Becker
  name: Markus Becker
  org: Tridonic GmbH & Co KG
  street: Färbergasse 15
  code: '6851'
  city: Dornbirn
  country: Austria
  phone: "+43 5572 395 45637"
  email: markus.becker@tridonic.com
- ins: K. Li
  name: Kepeng LI
  org: Alibaba Group
  abbrev: Alibaba Group
  street: Wenyixi Road, Yuhang District
  city: Hangzhou
  region: Zhejiang
  code: '311121'
  country: China
  email: kepeng.lkp@alibaba-inc.com
- ins: K. Kuladinithi
  name: Koojana Kuladinithi
  org: ComNets, Hamburg University of Technology
  street: Am Schwarzenberg-Campus 3
  code: '21073'
  city: Hamburg
  country: Germany
  phone: "+49 40 428 783533"
  email: koojana.kuladinithi@tuhh.de
- ins: T. Pötsch
  name: Thomas Pötsch
  org: New York University Abu Dhabi
  street: P.O. Box 129188
  code: '129188'
  city: Abu Dhabi
  country: United Arab Emirates
  phone: "+971 2 628 5069"
  email: thomas.poetsch@nyu.edu
normative:
  RFC2119:
#  RFC3966:
#  RFC3986:
  RFC4648:
#  RFC5234:
  RFC7252:
  RFC7595: -urischemes
  RFC7959: -block
  iso_ucs2:
    title: >
      ISO/IEC10646: "Universal Multiple-Octet Coded Character Set (UCS)";
      UCS2, 16 bit coding.
    author:
    - org: ISO
    date: 2000
  etsi_ts101_748:
    title: >
      Technical Report: Digital cellular telecommunications system; Abbreviations
      and acronyms (GSM 01.04 version 8.0.0 release 1999)
    author:
    - org: ETSI
    date: 2000
  ts23_038:
    title: >
      Technical Specification: Alphabets and language-specific
      information (3GPP TS 23.038 version 11.0.0 Release 11)
    author:
    - org: ETSI 3GPP
    date: 2012
informative:
  RFC1924: # April 1 RFC
  I-D.silverajan-core-coap-alternative-transports:
  cimd:
    title: CIMD Interface Specification (SMSCDOC8000.00, Nokia SMS Center 8.0)
    author:
    - org: Nokia
    date: 2005
  ucp:
    title: Short Message Service Centre (SMSC) External Machine Interface (EMI) Description
      Version 4.3d
    author:
    - org: Vodafone
    date: 2011
  smpp:
    title: Short Message Peer to Peer Protocol Specification v3.4 Issue 1.2
    author:
    - org: SMPP Developers Forum
    date: 1999
  ts23_888:
    title: Technical Specification Group Services and System Aspects; System Improvements
      for Machine-Type Communications; (3GPP TR 23.888 version 1.6.0, Release 11)
    author:
    - org: ETSI 3GPP
    date: 2011
  ts23_040:
    title: Technical realization of the Short Message Service (SMS)
    author:
    - org: 3GPP
    date: 2011-Mar
    seriesinfo:
      3GPP-23.040: a00
  ts23_682:
    title: >
      Technical Specification Group Services and System Aspects; Architecture
      Enhancements to facilitate communications with Packet Data Networks and Applications;
      (Release 11)
    author:
    - org: ETSI 3GPP
    date: 2012
  oma_lightweightm2m_ts:
    title: Lightweight Machine to Machine Technical Specification
    author:
    - org: OMA
    date: 2013

--- abstract


Short Message Service (SMS) of mobile cellular networks is
frequently used in Machine-To-Machine (M2M) communications,
such as for telematic devices. The service offers small packet
sizes and high delays just as other typical low-power and
lossy networks (LLNs), i.e. 6LoWPANs. The design of the
Constrained Application Protocol (CoAP, RFC7252), that took the
limitations of LLNs into account, is thus also applicable to
other transports. The adaptation of CoAP to SMS transport
mechanisms is described in this document.

--- middle

# Introduction

This specification details the usage of the Constrained
Application Protocol on the Short Message Service (SMS) of
mobile cellular networks.

## Motivation

In some M2M environments, internet connectivity is not
supported by the constrained end-points, but a cellular
network connection is supported instead. Internet
connectivity might also be switched off for power saving
reasons or the cellular coverage does not allow for Internet
connectivity. In these situations, SMS will be supported,
instead of UDP/IP over General Packet Radio Service (GPRS),
High Speed Packet Access (HSPA) or Long Term Evolution (LTE)
networks.

In 3GPP, SMS is identified as the transport protocol for
small data transmissions (See {{ts23_888}} for Key Issue on Machine Type Communication (MTC) Device
Trigger and the proposed solutions in Sections 6.2, 6.42,
6.44, 6.48, 6.52, 6.60, and 6.61). In {{ts23_682}} 'Architecture Enhancements to
facilitate communications with Packet Data Networks and
Applications' SMS is at the moment the only Trigger Delivery
(Trigger Delivery using T4).

M2M protocols using SMS, e.g. for telematics, are using
mostly various diverse proprietary and closed binary
protocols with limited publicly available documentation at
the moment.

In Open Mobile Alliance (OMA) LightweightM2M technical
specification {{oma_lightweightm2m_ts}}, SMS is
identified as an alternative transport for CoAP messages.



# Terminology

This document uses the following terminology:

{: vspace='0'}
CoAP Server and Client
: The terms CoAP
  Server and CoAP Client are used synonymously to Server and
  Client as specified in the terminology section of {{RFC7252}}.

Mobile Station (MS)
: A Mobile Station
  includes all required user equipment and software that is needed
  for communication with a mobile network. As defined in {{etsi_ts101_748}}.


# Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and
"OPTIONAL" in this document are to be interpreted as described
in [RFC 2119](#RFC2119).


# Scenarios

Several scenarios are presented first for M2M communications
with CoAP over SMS. First Mobile-Originating Mobile-Terminating
(MO-MT) scenarios are presented, where both CoAP endpoints are
in devices in a cellular network. Next, Mobile-Terminating (MT)
scenarios are detailed, where only the CoAP server is in a
cellular network. Finally, Mobile-Originating (MO) scenarios
where the CoAP client is in the cellular network.

## MO-MT Scenarios

Two mobile cellular terminals communicate by exchanging a CoAP
Request and Response embedded into short message protocol data
units (PDUs) (depicted in {{cell2cell_sms}}). Both terminals are
connected via a Short Message Service Centre (SMS-C).

~~~~
         CoAP-REQ            CoAP-REQ
+------+   (SMS)   +-------+   (SMS)  +------+
|  A   | --------> | SMS-C | -------> |  B   |
|(cell)| <-------- |       | <------- |(cell)|
+------+ CoAP-RES  +-------+ CoAP-RES +------+
           (SMS)               (SMS)
~~~~
{: #cell2cell_sms title='Cellular and Cellular Communication (only SMS-based)' artwork-align="center"}

<!--       The support for GPRS for the CoAP responses might be useful, so
             as to use SMS only for the request and as a wake-up signal for
             the device hosting the CoAP server. That device could then
             initiate a packet data protocol (PDP) context with the cellular
             network in order to bring up Internet connectivity. After having
             setup Internet connectivity, further message exchange can fully
             rely on IP. Network initiated PDP contexts could partly obsolete
             this mechanism.
         -->


## MT Scenarios

An IP host and a mobile cellular terminal communicate by
exchanging CoAP Request and Response. The IP host uses
protocols offered by the SMS-C (e.g. Short Message
Peer-to-Peer (SMPP {{smpp}}), Computer
Interface to Message Distribution (CIMD {{cimd}}), Universal Computer
Protocol/External Machine Interface
(UCP/EMI {{ucp}})) to submit a short message
for delivery, which contains the CoAP Request (depicted in {{ip2cell_sms}}).

~~~~
           CIMD               CoAP-REQ
+------+   SMPP    +-------+    (SMS)   +------+
|  A   | --------> | SMS-C | ---------> |  B   |
| (IP) | <-------- |       | <--------- |(cell)|
+------+           +-------+  CoAP-RES  +------+
                                (SMS)
~~~~
{: #ip2cell_sms title='IP and Cellular Communication' artwork-align="center"}

There are service providers that offer SMS delivery and
notification using an HTTP/REST interface (depicted in {{ip2cell_sms_prov}}).

~~~~
          HTTP-REQ                 CIMD            CoAP-REQ
+------+ (CoAP-DATA) +----------+  SMPP   +-----+    (SMS)   +------+
|      |             | SMS      |  SS7    |SMS-C|            |      |
|  A   | ----------> | Service  | ------> |  /  | ---------> |  B   |
| (IP) | <---------- | Provider | <------ | HLR | <--------- |(cell)|
+------+  HTTP-RES   +----------+         +-----+  CoAP-RES  +------+
         (CoAP-DATA)                                 (SMS)
~~~~
{: #ip2cell_sms_prov title='IP and Cellular Communication (using an SMS Service Provider)' artwork-align="center"}



## MO Scenarios

A mobile cellular terminal and an IP host communicate by
exchanging CoAP Request and Response. The mobile cellular
terminal sends a CoAP Request in a short message, which is in
turn forwarded by the SMS-C (e.g. with Short Message
Peer-to-Peer (SMPP {{smpp}}), Computer Interface
to Message Distribution (CIMD {{cimd}}),
Universal Computer Protocol/External Machine Interface
(UCP/EMI {{ucp}})) as depicted in {{cell2ip_sms}}). This scenario can
be a fall-back for mobile-originating communication, when IP
connectivity cannot be setup (e.g. due to missing coverage).

~~~~
          CoAP-REQ              CIMD
+------+   (SMS)   +-------+    SMPP    +------+
|  A   | --------> | SMS-C | ---------> |  B   |
|(cell)| <-------- |       | <--------- | (IP) |
+------+  CoAP-RES +-------+            +------+
          (SMS)
~~~~
{: #cell2ip_sms title='Cellular and IP Communication' artwork-align="center"}
There are service providers offering SMS delivery and
notification using an HTTP/REST interface (depicted in {{cell2ip_sms_prov}}).

~~~~
          CoAP-REQ             CIMD                HTTP-REQ
+------+   (SMS)    +-------+  SMPP  +----------+ (CoAP-DATA) +----+
|      |            | SMS-C |  SS7   | SMS      |             |    |
|  A   | ---------> |   /   | -----> | Service  | ----------> | B  |
|(cell)| <--------- |  HLR  | <----- | Provider | <---------- |(IP)|
+------+  CoAP-RES  +-------+        +----------+  HTTP-RES   +----+
           (SMS)                                  (CoAP-DATA)
~~~~
{: #cell2ip_sms_prov title='IP and Cellular Communication (using an SMS Service Provider)' artwork-align="center"}




# Message Exchanges

## Message Exchange for SMS in a Cellular-To-Cellular Mobile-Originated and Mobile-Terminated Scenario

The CoAP Client works as a Mobile Station to send the short
message, and the CoAP Server works as another Mobile Station
to receive the short message. All short messages are
stored and forwarded by the Service Center.  The message
exchange between the CoAP Client and the CoAP Server is
depicted in the figure below:


~~~~
 MS/CoAP CLIENT        Service Center          MS/CoAP SERVER
     |                       |                        |
     |   ---SMS-SUBMIT--->   |                        |
     | <-SMS-SUBMIT-REPORT-- |                        |
     |                       |                        |
     |                       |    --SMS-DELIVER--->   |
     |                       | <-SMS-DELIVER-REPORT-- |
     |                       |                        |
     | <-SMS-STATUS-REPORT-- |                        |
     |                       |                        |

~~~~
{: #msc_sms title='CoAP Messages over SMS'}

Note that the message exchange is just for one request
message from CoAP Client and CoAP Server. It includes the
following steps:

Step 1: The CoAP Client sends a CoAP request in a SMS-SUBMIT
message to the Service Center.  The CoAP Server address is
specified as TP-Destination-Address (see {{ts23_040}}).

Step 2: The Service Center returns a SMS-SUBMIT-REPORT
message to the CoAP Client.

Step 3: The Service Center stores the received SMS message
and forwards it to the CoAP Server, using an SMS-DELIVER
message. The CoAP Client address is specified as a TP
Originating Address (see {{ts23_040}}).

Step 4: The CoAP Server returns an SMS-DELIVER-REPORT message
to the Service Center.

Step 5: The Service Center returns the SMS-STATUS-REPORT
message to the CoAP Client to indicate the SMS delivery
status, if required by the CoAP Client.

Note that the SMS-STATUS-REPORT message just indicates the
transport layer SMS delivery status and has no relationship
with the confirmable message or non-confirmable message. If
the CoAP Client has sent a confirmable message, the CoAP
Server MUST use a separate SMS message to transmit the ACK.



# Encoding Schemes of CoAP for SMS transport {#sms_encoding}

Short messages can be encoded by using various alphabets: GSM 7 bit
default alphabet ({{ts23_038}}), 8 bit data alphabet, and 16 bit UCS2
data alphabet ({{iso_ucs2}}). These encodings lead to message sizes
of 160, 140, and 70 characters, respectively. Whereas the
support of 7 bit encoding is mandatory on a MS, the two other
encodings are dependent on the language that needs to be
encoded, e.g. UCS2 for Arabic, Chinese, Japanese, etc.
Furthermore, the supported encoding highly depends on the
implementations of the MS itself.

According to {{ts23_038}}, GSM 7 bit
encoding shall be supported by all MSs offering SMS
services. Since not all MSs support 8 bit short message
encoding, the preferred encoding scheme for CoAP messages over
SMS is therefore 7 bit, e.g. Base64 ({{RFC4648}}) or SMS encoding in {{smsenc}}.

More considerations about SMS encoding can be found in {{encode}}.


# Message Size Implementation Considerations

By using 7 bit encoding, a maximum length of 160 characters is
allowed in one short message {{ts23_038}}. Consequently, the maximum length for a CoAP message
results in 140 bytes. 160 characters = (140 bytes \* 8)/7.

Possible options for larger CoAP messages are:

Concatenated short messages
: Most
  MSs are able to send concatenation short messages in order
  to transmit longer messages. The total length of a
  concatenated short message can consist of up to 255 single
  messages and result in total length of 39015 7 bit
  characters or 34170 bytes. Resulting from this, the maximum
  length of each individual message reduces to 153 (160 - 7)
  characters (133 bytes).
{: vspace='0'}


CoAP block-wise transfer
: According to {{-block}}, the Block Size
  (SZX) of block-wise transfer in CoAP is represented as a
  three-bit unsigned integer. Thus, the possible block sizes
  are to the power of two. (Block size = 2\*\*(SZX + 4)).
    Due to the limitations of 160 characters (140 bytes) for one
  short message, the maximum value of SZX is 3 (Block size =
  128 byte).
{: vspace='0'}
However, it is RECOMMENDED that SMS is not used to transfer very large
resource data using block-wise transfer.


# Addressing

For SMS in cellular networks, the CoAP endpoints have to work
with a SIM (Subscriber Identity Module) card and have to be
addressed by the MSISDN (Mobile Station ISDN (MSISDN) number).

To allow the CoAP client to detect that the short message
contains a CoAP message, the TP-DATA-Coding-Scheme SHOULD be
included.


# Options {#opt_uri}

## New Options for mixed IP operation. {#options}

In case a CoAP Server has more than one network interface,
e.g. SMS and IP, the CoAP Client might want the server to
send the response via an alternative transport, i.e. to it's
alternative address. However, that implies that the
initiating CoAP Client is aware of the presence of the
alternative interface. For this reason the new options
Response-To-Uri-Host and Response-To-Uri-Port are proposed.

| No. | C | U | N | R | Name                  | Format | Length  | Default |
| TBD |   |   |   |   | Response-To- Uri-Host | string | 1-270 B | (none)  |
| TBD |   |   |   |   | Response-To- Uri-Port | uint   | 0-2 B   | 5683    |
{: #table_coap_option title='New CoAP Option Numbers' cols='c c c c c c c c c'}

If the Response-To-Uri-Host is present in the request, server
MUST send the response to the indicated URI address,
instead of the client's original request URI.

The options SHOULD NOT be used in the response.

The options MUST NOT occur more than once.



# URI Scheme {#urischeme}

The coap:// scheme defines that a CoAP server is reachable
over UDP/IP. Hence, a new URI scheme is needed for CoAP
servers which are reachable over SMS.

As proposed in {{I-D.silverajan-core-coap-alternative-transports}},
the transport information is expressed as part of the URI scheme component.
This
is performed by minting new schemes for SMS transport using
the form "coap+sms", where the name of the transport is
clearly and unambiguously described. The endpoint
identifier, path and query components together with each scheme name
would be used to uniquely identify each resource.

Example of such URI :

* coap+sms://0015105550101/sensors/temperature

In the URI, 0015105550101 is a telephone subscriber number.

<!-- XXX bad example: E.161 numbers don't start with 00 -->

# Transmission Parameters

It is RECOMMENDED to configure the RESPONSE_TIMEOUT variable
for a higher duration than specified in {{RFC7252}} for the applications described
here. The actual value SHOULD be chosen based on experience
with SMS<!-- and GPRS -->.


# Multicast

Multicast is not possible with SMS transports.<!--
          Multicast MUST NOT be used with the SMS and USSD transports.
       -->


# Security Considerations {#Security}

It is possible that a malicious CoAP Client sends repeated
requests, and it may cost money for the CoAP Server to use SMS
to send back associated responses. To avoid this situation,
the CoAP Server implementation can authenticate the CoAP
Client before responding to the requests. For example, the
CoAP Server can maintain an MSISDN white list. Only the MSISDN
specified in the white list will be allowed to send
requests. The requests from others will be ignored or
rejected.


# IANA Considerations {#IANA}

## CoAP Option Number {#IANA_options}

The IANA is requested to add the following option number
entries to the CoAP Option Number Registry:


~~~~
   +--------+----------------------+----------------------------+
   | Number |       Name           | Reference                  |
   +--------+----------------------+----------------------------+
   |  TBD   | Response-To-Uri-Host | Section 2 of this document |
   +--------+----------------------+----------------------------+
   |  TBD   | Response-To-Uri-Port | Section 2 of this document |
   +--------+----------------------+----------------------------+
~~~~


## URI Scheme Registration {#IANA_scheme}

According to {{I-D.silverajan-core-coap-alternative-transports}} this document requests the registration of the Uniform
Resource Identifier (URI) scheme "coap+sms".  The
registration request complies with {{-urischemes}}.
<!-- XXX check: is that true for RFC 7595? -->


--- back

SMS encoding {#encode}
============

For use in SMS applications, CoAP messages can be transferred using
SMS binary mode.  However, there is operational experience showing
that some environments cannot successfully send a binary mode SMS.

For transferring SMS in character mode (7-bit characters),
base64-encoding {{RFC4648}} is an obvious choice.  3 bytes of message
(24 bits) turn into 4 characters, which consume 28 bits.  The overall
overhead is approximately 17 %; the maximum message size is 120 bytes
(160 SMS characters).

If a more compact encoding is desired, base85 encoding could be employed
(however, probably not the version defined in {{RFC1924}} — instead,
the version used in tools such as btoa and PDF should be chosen).
However, this requires division operations.  Also, the base85
character set includes several characters that cannot be transferred
in a single 7-bit unit in SMS and/or are known to cause operational
problems.  A modified base85 character set can be defined to solve the
latter problem.  4 bytes of message (32 bits) turn into 5 characters,
which consume 35 bits.  The overall overhead is approximately 9.3 %;
the resulting maximum message size is 128 bytes (160 SMS characters).

Base64 and base85 do not make use of the fact that much CoAP data will
be ASCII-based.  Therefore, we define the following ASCII-optimized
SMS encoding.

## ASCII-optimized SMS encoding {#smsenc}

Not all 128 theoretically possible SMS characters are operationally
free of problems.  We therefore define:

Shunned code characters:
: @ sign, as it maps to 0x00
: LF and CR signs (0x0A, 0x0D)
: uppercase C cedilla (0x09), as it is often mistranslated in gateways
: ESC (0x1B), as it is used in certain character combinations only

Some ASCII characters cannot be transferred in the base SMS character
set, as their code positions are taken by non-ASCII characters.  These
are simply encoded with their ASCII code positions, e.g., an
underscore becomes a section mark (even though underscore has a
different code position in the SMS character set).

Equivalently translated input bytes:
: $, @, \[, \\, ], ^, _, \`, {, \|, }, ~, DEL

In other words, bytes 0x20 to 0x7F are encoded into the same code
positions in the 7-bit character set.

Out of the remaining code characters, the following SMS characters are available
for encoding:

Non-equivalently translated (NET) code characters:
: 0x01 to 0x08, (8 characters)
: 0x0B, 0x0C, (2 characters)
: 0x0E to 0x1A, (13 characters)
: 0x1C to 0x1F, (4 characters)

Of the 27 NET code characters, 18 are taken as prefix characters (see
below), and 8 are defined as directly translated characters:

Directly translated bytes:
: Equivalently translated input bytes are represented as themselves
: 0x00 to 0x07 are represented as 0x01 to 0x08

This leaves 0x08 to 0x1F and 0x80 to 0xFF.
Of these, the bytes 0x80 to 0x87 and 0xA0 to 0xFF are represented
as the bytes 0x00 to 0x07 (represented by characters 0x01 to 0x08) and 0x20
to 0x7F, with a prefix of 1 (see below).
The characters 0x08 to 0x1F are represented as the characters 0x28 to
0x3F with a prefix of 2 (see below).
The characters 0x88 to 0x9F are represented as the characters 0x48 to
0x5F with a prefix of 2 (see below).
(Characters 0x01 to 0x08, 0x20 to 0x27, 0x40 to 0x47, and 0x60 to 0x7f
with a prefix of 2 are reserved for future extensions, which could be
used for some backreferencing or run-length compression.)

Bytes that do not need a prefix (directly translated bytes) are sent
as is.  Any byte that does need a prefix (i.e., 1 or 2) is preceded by a prefix
character, which provides a prefix for this and the following two
bytes as follows:


| char | pfx |.| char | pfx |
| 0x0B | 100 |.| 0x15 | 200 |
| 0x0C | 101 |.| 0x16 | 201 |
| 0x0E | 102 |.| 0x17 | 202 |
| 0x0F | 110 |.| 0x18 | 210 |
| 0x10 | 111 |.| 0x19 | 211 |
| 0x11 | 112 |.| 0x1A | 212 |
| 0x12 | 120 |.| 0x1C | 220 |
| 0x13 | 121 |.| 0x1D | 221 |
| 0x14 | 122 |.| 0x1E | 222 |
{: #prefixchars title="SMS prefix character assignment"}



(This leaves one non-shunned character, 0x1F, for future extension.)

The coding overhead of this encoding for random bytes is similar to
Base85, without the need for a division/multiplication.
For bytes that are mostly ASCII characters, the overhead can easily
become negative.  (Conversely, for bytes that for some reason are more likely to be
non-ASCII than in a random sequence of bytes, the overhead becomes
greater.)

So, for instance, for the CoAP message in {{coapdump}}:

    ver     tt      code    mid
    1       ack     2.05    17033
    content_type    40
    token           sometok
    3c 2f 3e 3b 74 69 74 6c 65 3d 22 47 65 6e 65 72  |</>;title="Gener|
    61 6c 20 49 6e 66 6f 22 3b 63 74 3d 30 2c 3c 2f  |al Info";ct=0,</|
    74 69 6d 65 3e 3b 69 66 3d 22 63 6c 6f 63 6b 22  |time>;if="clock"|
    3b 72 74 3d 22 54 69 63 6b 73 22 3b 74 69 74 6c  |;rt="Ticks";titl|
    65 3d 22 49 6e 74 65 72 6e 61 6c 20 43 6c 6f 63  |e="Internal Cloc|
    6b 22 3b 63 74 3d 30 2c 3c 2f 61 73 79 6e 63 3e  |k";ct=0,</async>|
    3b 63 74 3d 30                                   |;ct=0           |
{: #coapdump title="CoAP response message as captured and decoded"}

The 116 byte unencoded message is shown as ASCII characters in
{{coapdump1}} (\xDD stands for the byte with the hex digits DD):

    bEB\x89\x11(\xA7sometok</>;title="General Info";ct=0,</time>
    ;if="clock";rt="Ticks";title="Internal Clock";ct=0,</async>;ct=0
{: #coapdump1 title="CoAP response message shown as unencoded characters"}

The only non-ASCII characters in this example are in the beginning of
the message.  According to the translation instructions above, the four bytes:

    89 11 (  A7

need the prefixes:

    2  2  0  1

As each prefix character always covers three unencoded bytes, we need
the prefix characters for 220 and 100, which are \x1C and \x0B,
respectively ({{prefixchars}}).

The equivalent SMS encoding is shown as equivalent-coded SMS
characters in {{coapdump2}} (7 bits per character, \x1C is the 220
prefix and \x0B is the 100 prefix, the rest is shown in equivalent
encoding), adding two characters of prefix overhead, for a total length
of 118 7-bit characters or 104 (103.25 plus padding) bytes:

    bEB\x1CI1(\x0B'sometok</>;title="General Info";ct=0,</time>
    ;if="clock";rt="Ticks";title="Internal Clock";ct=0,</async>;ct=0
{: #coapdump2 title="CoAP response message shown as SMS-encoded characters"}




# Changelog {#app-additional}

RFC editor: please remove this appendix.

Changed from draft-05 to draft-06:

* Update references and addresses

* Integrate relevant text from coap-misc as an appendix.


Changed from draft-04 to draft-05:

* Removed reference to USSD.

* Updated reference to RFC7252 and 3GPP specs.

* Updated Options.

* Adapted URI scheme.


Changed from draft-03 to draft-04:

* Removed USSD and GPRS related parts.

* Removed section 5: Examples

* Removed section 14: Proxying Considerations

* Added more block size considerations.

* Added more concatenated SMS considerations.

* Rewrote encoding scheme section; 7 bit encoding only.


Changed from draft-02 to draft-03:

* Added reference to OMA LightweightM2M Technical Specification in "Motivation"
  section.

* Chose CoAP option numbers and updated the option number table
  to meet draft-ietf-core-coap-13.


Changed from draft-01 to draft-02:

* Added security considerations: Transport and Object
  Security. {{Security}}

* Reply-To-\* changed to Response-To-\*. {{IANA}} <!-- and <xref target="options" /> -->

* Added URI scheme.

* Added possible CON/NON/ACK interactions.

* Added possible M2M proxy scenarios.

* Added reference to bormann-coap-misc for other SMS encoding. {{sms_encoding}}

* Updated requirements on Uri-Host and Uri-Port for coap+tel://.

* Chose CoAP option numbers and updated the option number table
  to meet draft-ietf-core-coap-10.
  />

* Added an IANA registration for the URI scheme. {{IANA_scheme}}




# Acknowledgements {#Acknowledgements}
{: numbered='no'}

This document is partly based on research for the research
project 'The Intelligent Container' which is supported by the
Federal Ministry of Education and Research, Germany, under
reference number 01IA10001.

The authors of this draft would like to thank Bert
Greevenbosch, Marcus Götting, Nils Schulte and Klaus Hartke
for the discussions on the topic and the reviews of this document.

# Contributors
{: numbered='no'}

{{encode}} has been contributed by Carsten Bormann.

~~~
Carsten Bormann
Universitaet Bremen TZI
Postfach 330440
Bremen  D-28359
Germany

Phone: +49-421-218-63921
EMail: cabo@tzi.org
~~~
