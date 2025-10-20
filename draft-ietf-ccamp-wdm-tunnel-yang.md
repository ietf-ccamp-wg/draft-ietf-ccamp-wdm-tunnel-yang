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

Transport networks have evolved from traditional fixed-grid Wavelength Switched Optical Networks (WSON) {{?RFC6163}} to more scalable and flexible elastic optical networks. These utilize flexi-grid Dense Wavelength Division Multiplexing (DWDM) technologies {{?RFC7698}} to optimize bandwidth usage. Current DWDM Optical Network deployments may include fixed-grid WSON, flexi-grid DWDM, or a combination of both.

In the optical domain, a WDM tunnel typically originates and concludes at a pair of transponders using one or more transceivers dependent upon the data rate and encoding type of the transceivers. These transponders are then connected to an intermediate line system composed of optical switches and multiplexers, including Reconfigurable Optical Add-Drop Multiplexers (ROADMs) and add-drop multiplexers, complemented by optical amplifiers to boost the transmission distance. The optical wavelength can be routed from the transponder or an incoming fiber, through multiplexing, to various outgoing fibers in the DWDM network. At optical nodes, wavelengths may undergo conversion via optical-electrical-optical (OEO) regenerators, depending on the switching setup and fiber configuration.

Optical services, transmitted via analog signals, require careful provisioning across the network to maintain signal quality and prevent interference between different wavelength channels. The technology within optical nodes, like tunable transceivers or Colorless, Directionless and Contentionless Flexi-grid (CDC-F) ROADMs, introduces specific constraints that can limit WDM tunnel path options. These constraints must be factored into WDM tunnel provisioning and pre-computation. Additionally, assessing the end-to-end optical performance metrics like Generalized Signal-to-noise Ratio (G-SNR), Bit Error Rate (BER), and Q-factor is crucial to ensure transmission quality and receiver signal integrity.

This draft introduces a YANG {{!RFC7950}} data model for setting up and managing TE tunnels and LSPs in DWDM Optical Networks. It aims to provide an intent-based interface used by a control entity such as a Software-defined Network (SDN) controller at its northbound to establish services between endpoints, typically optical transponders. Clients can utilize this model to either partially or fully delegate service provisioning to the SDN controller, while still capable to express additional constraints to guide its operation. Service provisioning can be as simple as identifying the source and destination transponders and delegate the rest of determination to the SDN controller, or as explicit as specifying a complete detailed path complete with tuned wavelengths and transceiver details.

This document identifies the WDM tunnel components, parameters and their values, and characterizes the features and the performances of the WDM elements. An application example is provided towards the end of the document to understand their utility better.


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

The YANG data model in this draft builds upon the generic TE tunnel model from {{!I-D.ietf-teas-yang-te}}. This base model is suitable for all TE-enabled networks and includes universal TE tunnel elements like node addresses, tunnel termination points (TTPs), and path-level constraints such as explicit path hops, label restrictions, and path diversity. The current model enhances {{!I-D.ietf-teas-yang-te}} by incorporating WDM-specific attributes and constraints relevant to WDM tunnels, including definitions for:

-  Network-scope optical transceiver configuration constraints, e.g., operational modes, transceiver tuning constraints

-  Network-scope WDM path routing policies for influencing WDM TE path selection. For exmaple,  whether or not using regenerator or wavelength conversion is allowed, whether or not wavelength retuing is allowed for tunable transceivers, etc.

-  Network-scope optical performance constraints, e.g. the generalized Signal-to-noise (G-SNR) margin and delta power of a feasible optical path

-  Path-scope WDM layer constraints and transceiver configurations for working and protection path within a WDM tunnel

-  List of WDM nodes, links, and optical wavelength that constitute an end-to-end WDM path

-  Other relevant optical attributes which characterize the optical signal

The attributes described above are optional, allowing the model to support both simplified and fully-explicit WDM tunnel provisioning to meet diverse client requirements.

Additionally, the YANG model provides the status of a WDM tunnel, which includes:

- Computed paths for various roles such as working, protection, and restoration, indicating potential optical paths confirmed by the SDN controller via pre-computation.

- Actual LSPs for each tunnel path, representing the optical paths currently established in the network.

## Integrated vs. External Optical Transponder

In optical networks built with traditional chassis-based DWDM optical equipment, optical transponder (OTs) are typically inserted into the chassis installed as cards. WDM tunnels are established between pairs of OTs, with the SDN controller serving as the central entity for provisioning and managing these tunnels.

In scenarios like data center interconnects (DCI), optical transponders may be externally mounted on a 'pizza box' and linked via dedicated fiber or wavelength multiplexer/demultiplexer to the optical line system. These external OTs could be managed by the same SDN controller or a different entity, such as an orchestrator. Consequently, a WDM tunnel might be composed of several segments joined to create a continuous end-to-end tunnel.

The YANG data model offers a cohesive interface for managing WDM tunnels and tunnel segments, irrespective of transponder location.

## 3R Regenerators

A desired optical path may span a distance beyond the reach of a single optical transponder. In that case, one or more 3R regenerators can be deployed at intermediate nodes, reamplifying, reshaping, and retiming the optical signal before transmission continues. This scenario is described in detail in Section 2.7 of {{!I-D.ietf-ccamp-optical-impairment-topology-yang}}. Deploying 3R regeneration is costly because it requires additional transponders; consequently, at most a single regenerator is typically used to regenerate the optical signals of an end-to-end optical tunnel.

According to {{!I-D.ietf-ccamp-optical-impairment-topology-yang}}, there are multiple ways to implement a 3R regenerator:
 - Back-to-back (bi-directional) regeneration, where two optical transponders are connected back-to-back; each transceiver receives and transmits the optical signal for the same segment of the end-to-end tunnel, operating in both directions.
 - Uni-directional regeneration, where two transponders are used with one performing 3R regeneration in the forward direction (source to destination) and the other performing 3R regeneration in the reverse direction.

When an optical signal is regenerated, it can be terminated and restored at different protocol layers depending on the transponder's capabilities. For example, if the transponder supports OTN, the signal can be regenerated at the OTU layer or at the ODU layer after the OTU overhead has been stripped; if the transponder supports Ethernet clients, the signal can be terminated at the Ethernet layer, which commonly occurs in the back-to-back configuration. Typically, the signal is regenerated at the lowest possible layer to minimize processing delay and complexity, but termination layer may be chosen deliberately during network planning for specific scenarios. To ensure correct transmission, the termination method used between the source and the receiving transponder at the regenerator node must match the method used between the sending transponder at the regenerator node and the destination transponder (or the receiving transponder at the next regenerator node).

The termination of an optical signal occurs at the reference point immediately before inverse multiplexing in the transmitting direction or immediately after inverse multiplexing in the receiving direction for the corresponding transceiver, and this applies to the entire OTSiG used by the optical tunnel. For 3R regeneration, to identify the transceiver configuration associated with these reference points consistently between uni-directional and bi-directional regeneration, the following definitions are used in this draft:
 - Forward direction: the direction of the optical path from the source to the destination.
 - Reverse direction: the direction of the optical path from the destination back to the source.
 - Incoming transponder: for back-to-back regeneration, the transponder in the regenerator that receives from and transmits toward the same segment of the optical path toward the source; for uni-directional regeneration, the transponder that performs regeneration in the forward direction of the optical path.
 - Outgoing transponder: for back-to-back regeneration, the transponder in the regenerator that receives from and transmits toward the same segment of the optical path toward the destination; for uni-directional regeneration, the transponder that performs regeneration in the reverse direction of the optical path.

Utilizing the figures in {{!I-D.ietf-ccamp-optical-impairment-topology-yang}} for 3R regeneration, {{fig-back2back}} and {{fig-unidir}} further illustrate the aforementioned reference points for back-to-back regeneration and uni-directional regeneration, respectively.

~~~~ ascii-art
                          Forward Direction
  |------------------------------>---------------------------------|
                          Reverse Direction
  |------------------------------<---------------------------------|

            Incoming Transponder     Outgoing Transponder          D
            +-------------------+   +--------------------+         E
  S         |Transceiver        |   |         Transceiver|         S
  O         |-----------+ +-----|   |-----+ +------------|         T
  U ------->|Receiver   |-|Sig. |-->|Sig. |-| Transmitter|-------> I
  R         |-----------+ |     |   |     | +------------|         N
  C <-------|Transmitter|-|Proc.|<--|Proc.|-|    Receiver|<------- A
  E         |-----------+ +-----|   |-----+ +------------|         T
            +-------------------+   +--------------------+         I
                                                                   O
                                                                   N
~~~~
{: #fig-back2back title="Reference Points in Back-to-back 3R Regeneration"}

~~~~ ascii-art

                          Forward Direction
  |------------------------------>---------------------------------|
                          Reverse Direction
  |------------------------------<---------------------------------|

                           Incoming Transponder
                           3R in forward direction
                         +-----------------------+
                         |Transceiver            |
                         |-----------+ +--------+|
                 ------->|Receiver   |-|Sig. --+||
                         |-----------+ |       |||
                     +---|Transmitter|-|Proc.<-+||
                     |   |-----------+ +--------+|
                     |   +-----------------------+                 E
  S                  |                                             S
  O                  +------------------------------------>        T
  U                                                                I
  R              <-----------------------------------+             N
  C                                                  |             A
  E                      +-----------------------+   |             T
                         |            Transceiver|   |             I
                         |+--------+ +-----------|   |             O
                         ||+->Sig. |-|Transmitter|---+             N
                         |||       | +-----------|
                         ||+--Proc.|-|Receiver   |<--------
                         |+--------+ +-----------|
                         +-----------------------+
                          Outgoing Transponder
                          3R in reverse direction

~~~~
{: #fig-unidir title="Reference Points in Uni-directional 3R Regeneration"}


# Example of Use

To illustrate the model's application, consider an optical network with various transponders, switches, and links. A depicted topology outlines two WDM tunnel scenarios. In the first, an end-to-end WDM tunnel (WDM Tunnel 1) comprises two physical paths (WDM Primary Path 1 and 2) linking two integrated optical transponders, Transponder A and E, through WSON and Flexi-grid nodes. The second scenario describes three WDM tunnel segments (WDM Tunnel Segment 2a to 2c) connecting two external OTs, External OT node X and Y, via the same nodes and links.

~~~~ ascii-art
                              WDM Tunnel 1
        <===================================================>
                         WDM Primary Path 1
        <--------------------------------------------------->

            WDM Tunnel         WDM Tunnel      WDM Tunnel
            Segment 2a         Segment 2b      Segment 2c
        <=================><===============><===============>
   +----------+                                        +----------+
   | External |                                        | External |
   |    OT    |<----------+                +---------->|    OT    |
   |  node X  |           |                |           |  node Y  |
   +----------+           |                |           +----------+
                          |                |
                          |                |
   +----------+           |                |           +----------+
   |  Flexi-  |           |                |           |  Flexi-  |
   |   grid   |           |                |           |   grid   |
   |  node A  |           |                |           |  node E  |
   |          |        +--v---+        +---v--+        |          |
   |          | Link 1 |Flexi-| Link 2 | WSON | Link 3 |          |
   |          |<------>| grid |<------>|      |<------>|          |
   |......... |        |node B|        |node C|        | .........|
   | Trans- : |        +------+        +------+        | : Trans- |
   | ponder : |                                        | : ponder |
   |    A   : |                +------+                | :    E   |
   |........: |     Link 4     |Flexi-|     Link 5     | :........|
   |          |                | grid |                |          |
   |          |<-------------->|node D|<-------------->|          |
   |          |                +------+                |          |
   +----------+                                        +----------+

        <--------------------------------------------------->
                          WDM Secondary Path 1
~~~~
{: #fig-topology-example title="Topology Example"}

To configure an end-to-end WDM tunnel to interconnect
transponders A and E, first of all we have to populate the
flexi-grid topology YANG model with all elements in the network:

-  We define the transponders within nodes A and E as tunnel termination
   points (TTPs) and provide their internal local link connectivity
   towards the node interfaces.  We also provide nodes A and E identifiers,
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

~~~~ ascii-art
{::include ./ietf-wdm-tunnel.tree}
~~~~
{: #fig-wdm-tunnel-tree title="WDM Tunnel YANG tree" artwork-name="ietf-wdm-tunnel.tree"}

## YANG Code

~~~~ yang
{::include ./ietf-wdm-tunnel.yang}
~~~~
{: #fig-wdm-tunnel-yang title="WDM Tunnel YANG module" sourcecode-markers="true" sourcecode-name="ietf-wdm-tunnel@2024-07-02.yang"}

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

This document requests IANA to register the following URIs in the "ns" subregistry within the "IETF XML Registry" {{!RFC3688}}. Following the format in {{!RFC3688}}, the following registrations are requested.

~~~~
   URI: urn:ietf:params:xml:ns:yang:ietf-wdm-tunnel
   Registrant Contact: The IESG
   XML: N/A; the requested URI is an XML namespace.
~~~~

This document requests IANA to register the following YANG modules in the "IANA Module Names" {{!RFC6020}}. Following the format in {{!RFC6020}}, the following registrations are requested:

~~~~
   name: ietf-wdm-tunnel
   namespace: urn:ietf:params:xml:ns:yang:ietf-wdm-tunnel
   prefix: wdm-tnl
   reference: RFC XXXX
~~~~

RFC Editor: Please replace XXXX with the RFC number assigned to this document.

--- back

# Acknowledgments
{:numbered="false"}

This work is also partially funded by the Spanish State Research
Agency under the project AgileMon (AEI PID2019-104451RB-C21) and by
the Spanish Ministry of Science, Innovation and Universities under
the program for the training of university lecturers (Grant number:
FPU19/05678).
