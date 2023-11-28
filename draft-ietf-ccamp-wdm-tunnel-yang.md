---
title: "A YANG Data Model for WDM Tunnels"
abbrev: "WDM Tunnel YANG Model"
category: std
ipr: trust200902

docname: draft-ietf-ccamp-wdm-tunnel-yang-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Routing"
workgroup: "CCAMP Working Group"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Common Control and Measurement Plane"
  type: "Working Group"
  mail: "ccamp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ccamp/"
  github: "ietf-ccamp-wg/draft-ietf-ccamp-wdm-tunnel-yang"
  latest: "https://ietf-ccamp-wg.github.io/draft-ietf-ccamp-wdm-tunnel-yang/draft-ietf-ccamp-wdm-tunnel-yang.html"

author:
  -
    name: Aihua Guo
    org: Futurewei Technologies
    email: aihuaguo.ietf@gmail.com
  -
    name: Sergio Belotti
    org: Nokia
    email: Sergio.belotti@nokia.com
  -
    name: G. Galimberti
    org: Individual
    email: ggalimbe56@gmail.com
  -
    name: Jorge E. Lopez de Vergara Mendez
    org: Naudit HPCN
    email: jorge.lopez_vergara@uam.es
  -
    name: Daniel Perdices Burrero
    org: Universidad Autonoma de Madrid
    email: daniel.perdices@uam.es

contributor:
  -
    name: Daniel King
    org: Old Dog Consulting
    email: daniel@olddog.co.uk
  -
    name: Haomian Zheng
    org: Huawei Technologies
    street: H1, Xiliu Beipo Village, Songshan Lake
    city: Dongguan
    country: China
    email: zhenghaomian@huawei.com
  -
    name: Italo Busi
    org: Huawei Technologies
    email: italo.busi@huawei.com
  -
    name: Oscar Gonzalez de Dios
    ins: O. Gonzalez de Dios
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
  -
    name: Victor Lopez
    org: Nokia
    email: victor.lopez@nokia.com
  -
    name: Dieter Beller
    org: Nokia
    email: Dieter.Beller@nokia.com
  -
    name: Ricard Vilalta
    org: CTTC
    email: ricard.vilalta@cttc.es
  -
    name: Young Lee
    org: Samsung
    email: younglee.tx@gmail.com
  -
    name: Bin Yeong Yoon
    org: ETRI
    email: byyun@etri.re.kr
  -
    name: Daniel Michaud Vallinoto
    org: Universidad Autonoma de Madrid
    email: daniel.michaud@estudiante.uam.es
  -
    name: Zafar Ali
    org: Cisco
    email: zali@cisco.com
  -
    name: Esther Le Rouzic
    org: Orange
    email: esther.lerouzic@orange.com
  -
    name: Julien Meuric
    org: Orange
    email: julien.meuric@orange.com
  -
    name: Gert Grammel
    org: Juniper
    email: ggrammel@juniper.net
  -
    name: Roberto Manzotti
    org: Cisco
    email: manzoro@gmail.com

normative:

informative:

--- abstract

This document defines a YANG data model for the provisioning and management of Traffic Engineering (TE) tunnels and Label Switched Paths (LSPs) in Optical Networks (Wavelength Switched Optical Networks (WSON) and Flexi-Grid Dense Wavelength Division Multiplexing (DWDM) Networks).

The YANG data model defined in this document conforms to the Network Management Datastore Architecture (NMDA).

--- middle

# Introduction

Transport networks have evolved from traditional Wavelength Switched Optical Networks (WSON) systems {{?RFC6163}} based on only fixed-grid wavelength switching, towards elastic optical networks,
based on flexi-grid Dense Wavelength Division Multiplexing (DWDM) transmission and switching technologies {{?RFC7698}}. Such technology aims
at increasing transport network scalability and flexibility, allowing bandwidth usage optimization.

While {{!RFC9094}} {{!I-D.ietf-ccamp-flexigrid-yang}} focus on flexi-grid objects such as nodes, transponders
and links, this document presents a YANG {{!RFC7950}} model for the provisioning and management of Traffic Engineering (TE) tunnels and Label Switched Paths (LSPs) in DWDM Optical Networks, which can be Wavelength Switched Optical Networks (WSON) networks or Flexi-Grid Dense Wavelength Division Multiplexing (DWDM) Networks, or a mix of these two networks. This YANG module defines the path from a source transponder or node to the destination through several intermediate nodes in such a WDM optical network.

This document identifies the WDM tunnel components, parameters and their values, and
characterizes the features and the performances of the WDM elements. An application example is
provided towards the end of the document to understand their utility better.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

   The terminology for describing YANG data models is found in
   {{!RFC7950}}.

   Refer to {{!RFC7446}} and {{!RFC7699}} for the key terms used in this document.

   The following terms are defined in {{!RFC7950}} and are not redefined here:
   -  client

   -  server

   -  augment

   -  data model

   -  data node

   The following terms are defined in {{!RFC6241}} and are not redefined here:
   -  configuration data

   -  state data

# Overview

The generic TE tunnel attributes, such as source and destination node addresses, source and destination tunnel termination points (TTPs), are already defined by the base data model in {{!I-D.ietf-teas-yang-te}}. The present model defines a WDM tunnel by augmenting the base model with the following
WDM technology-specific constructs:

   -  Global WDM layer constraints that influence the TE path selection, e.g., whether wavelength conversion or regeneration is considered

   -  Global transponder/transceiver configuration constraints, e.g., operational modes, tuning constraints of the transceiver

   -  Global optical performance constraints, e.g. generalized Signal-to-noise (G-SNR) margin of a feasible optical path

   -  Path-scope WDM layer constraints, e.g. identities of transceivers assigned to the primary or secondary path

   -  List of links that defines the path

   -  Other optical attributes


Each path can be a segment path (only defined by the source and destination nodes or link termination points)
or an end-to-end path (additionally needs source and destination transponders). Therefore, all the attributes
are optional to support both situations.


# Example of Use

   In order to explain how this model is used, the following
   example is provided.  An optical network usually has multiple transponders,
   switches (nodes) and links. {{fig-topology-example}} shows a simple
   topology, where two physical paths interconnect two optical
   transponders via a combination of both WSON and Flexi-grid wavelength
   switched nodes and links.


~~~~
                              WDM Tunnel
        <===================================================>
                         WDM Primary Path
        <--------------------------------------------------->

   +----------+                                        +----------+
   |  Flexi-  |                                        |  Flexi-  |
   |   grid   |                                        |   grid   |
   |  node A  |                                        |  node E  |
   |          |        +------+        +------+        |          |
   |          | Link 1 |Flexi-| Link 2 | WSON | Link 3 |          |
   |          |<------>| grid |<------>|      |<------>|          |
   |......... |        |node B|        |node C|        | .........|
   | Trans- : |        +------+        +------+        | : Trans- |
   | ponder : |                                        | : ponder |
   |    A   : |              +----------+              | :    E   |
   |........: |     Link 4   |Flexi-grid|   Link 5     | :........|
   |          |              |    D     |              |          |
   |          |<------------>|   node   |<------------>|          |
   |          |              +----------+              |          |
   +----------+                                        +----------+

        <--------------------------------------------------->
                          WDM Secondary Path
~~~~
{: #fig-topology-example title="Topology Example"}

   To configure an end-to-end WDM tunnel to interconnect
   transponders A and E, first of all we have to populate the
   flexi-grid topology YANG model with all elements in the network:

   -  We define the transponders within nodes A and E as tunnel termination
      points (TTPs) and provide their internal local link connectivity
      towards the node interfaces.  We also provide nodes A and B identifiers,
      addresses and interfaces.

   -  We do the same for the nodes B, C and D, providing their
      identifiers, addresses and interfaces, as well as the internal
      connectivity matrix between interfaces.

   -  Then, we also define the links 1 to 5 that interconnect nodes,
      indicating which WSON or flexi-grid labels are available.

   -  Other information, such as the slot frequency and granularity are
      also provided.

   After the nodes, links and transponders have been defined using
   {{!I-D.ietf-ccamp-flexigrid-yang}} and {{!RFC9094}} we can
   configure the tunnel from the information we have stored in the
   flexi-grid topology, by querying which elements are available, and
   planning the resources that have to be provided on each situation, taking into
   account the global and path-specific WDM tunnel constraints.
   Note that every element in the flexi-grid topology has a reference,
   and this is the way in which they are called in the tunnel.

   -  Depending on the case, it is possible to define either the source
      and destination node ports, or the source and destination node and
      transponder.  In our case, we would define a network tunnel, with
      source transponder A and source node B, and destination
      transponder E and destination node C.  Thus, we are going to
      follow path x.

   -  Then, for each link in the path x, we indicate which channel we
      are going to use, providing information about the slots, and what
      nodes are connected.

   -  Finally, the flexi-grid topology has to be updated with each
      element usage status each time a tunnel is created or torn down.

# YANG Model for WDM Tunnel

## YANG Tree

~~~~
{::include ./ietf-wdm-tunnel.tree}
~~~~

## YANG Code

~~~~
   <CODE BEGINS> file "ietf-wdm-tunnel@2023-10-22.yang"
{::include ./ietf-wdm-tunnel.yang}
   <CODE ENDS>
~~~~

# Security Considerations

   The configuration, state, and action data defined in this document
   are designed to be accessed via a management protocol with a secure
   transport layer, such as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.
   The NETCONF access control model {{!RFC8341}} provides the means to
   restrict access for particular NETCONF users to a preconfigured
   subset of all available NETCONF protocol operations and content.

   There are a number of data nodes defined in this YANG module that are
   writable/creatable/deletable (i.e., config true, which is the
   default).  These data nodes may be considered sensitive or vulnerable
   in some network environments.  Write operations (e.g., edit-config)
   to these data nodes without proper protection can have a negative
   effect on network operations.  These are the subtrees and data nodes
   and their sensitivity/vulnerability:

   -  /te:te/te:tunnels/te:tunnel

   -  /te:te/.../te:te-bandwidth/te:technology

   -  /te:te/.../te:type/te:label/te:label-hop/te:te-label/te:technology

   -  /te:te/.../te:label-restrictions/te:label-restriction/te:label-
      start/te:te-label/te:technology

   -  /te:te/.../te:label-restrictions/te:label-restriction/te:label-
      end/te:te-label/te:technology

   -  /te:te/.../te:label-restrictions/te:label-restriction/

   Editors note: we are using simplified description by folding similar
   branches to avoid repetition.

# IANA Considerations

   It is proposed to IANA to assign new URIs from the "IETF XML
   Registry" {{!RFC3688}} as follows:

~~~~
   URI: urn:ietf:params:xml:ns:yang:ietf-wdm-tunnel
   Registrant Contact: The IESG
   XML: N/A; the requested URI is an XML namespace.
~~~~

   This document registers the following YANG module in the YANG Module Names
   registry {{!RFC6020}}.

~~~~
   name: ietf-wdm-tunnel
   namespace: urn:ietf:params:xml:ns:yang:ietf-wdm-tunnel
   prefix: wdm-tnl
   reference: RFC XXXX
~~~~

--- back

# Acknowledgments
{:numbered="false"}

   This work is also partially funded by the Spanish State Research
   Agency under the project AgileMon (AEI PID2019-104451RB-C21) and by
   the Spanish Ministry of Science, Innovation and Universities under
   the program for the training of university lecturers (Grant number:
   FPU19/05678).
