# Arm® Memory System Resource Partitioning and Monitoring (MPAM)

### Memory System Component (MSC) Specification

Document number Arm IHI 0099

Document quality EAC

Document version B.a

Document confidentiality Non-Confidential

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved. Document number: Arm IHI 0099

###### Release information

The following releases of this document have been made.

Date Version Changes

01/Dec/2025 B.a • First release of MPAM MSC specification in rules-based writing, also incorporating the 2024 architecture extensions FEAT_MPAM_MSC_DOMAINS and FEAT_MPAM_MSC_DCTRL.

19/Apr/2024 A.a • First release of the MPAM Memory System Component (MSC) specification after the content was separated from Memory System Resource Partitioning and Monitoring (MPAM), for A-profile Architecture, Arm Architecture Reference Manual Supplement, DDI0598.

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

ii

###### Arm Non-Confidential Document License (“License”)

This License is a legal agreement between you and Arm Limited (“Arm”) for the use of Arm’s intellectual property (including, without limitation, any copyright) embodied in the document accompanying this License (“Document”). Arm licenses its intellectual property in the Document to you on condition that you agree to the terms of this License. By using or copying the Document you indicate that you agree to be bound by the terms of this License.

“Subsidiary” means any company the majority of whose voting shares is now or hereafter owned or controlled, directly or indirectly, by you. A company shall be a Subsidiary only for the period during which such control exists.

This Document is NON-CONFIDENTIAL and any use by you and your Subsidiaries (“Licensee”) is subject to the terms of this License between you and Arm.

Subject to the terms and conditions of this License, Arm hereby grants to Licensee under the intellectual property in the Document owned or controlled by Arm, a non-exclusive, non-transferable, non-sub-licensable, royalty-free, worldwide License to:

- (i) use and copy the Document for the purpose of designing and having designed products that comply with the Document;
- (ii) manufacture and have manufactured products which have been created under the License granted in (i) above; and
- (iii) sell, supply and distribute products which have been created under the License granted in (i) above.


Licensee hereby agrees that the Licenses granted above shall not extend to any portion or function of a product that is not itself compliant with part of the Document.

Except as expressly licensed above, Licensee acquires no right, title or interest in any Arm technology or any intellectual property embodied therein.

The content of this document is informational only. Any solutions presented herein are subject to changing conditions, information, scope, and data. This document was produced using reasonable efforts based on information available as of the date of issue of this document. The scope of information in this document may exceed that which Arm is required to provide, and such additional information is merely intended to further assist the recipient and does not represent Arm’s view of the scope of its obligations. You acknowledge and agree that you possess the necessary expertise in system security and functional safety and that you shall be solely responsible for compliance with all legal, regulatory, safety and security related requirements concerning your products, notwithstanding any information or support that may be provided by Arm herein. In addition, you are responsible for any applications which are used in conjunction with any Arm technology described in this document, and to minimize risks, adequate design and operating safeguards should be provided for by you.

Reference by Arm to any third party’s products or services within this document is not an express or implied approval or endorsement of the use thereof.

THE DOCUMENT IS PROVIDED “AS IS”. ARM PROVIDES NO REPRESENTATIONS AND NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, INCLUDING, WITHOUT LIMITATION, THE IMPLIED WARRANTIES OF MERCHANTABILITY, SATISFACTORY QUALITY, NON-INFRINGEMENT OR FITNESS FOR A PARTICULAR PURPOSE WITH RESPECT TO THE DOCUMENT. Arm may make changes to the Document at any time and without notice. For the avoidance of doubt, Arm makes no representation with respect to, and has undertaken no analysis to identify or understand the scope and content of, third party patents, copyrights, trade secrets, or other rights.

NOTWITHSTANDING ANYTHING TO THE CONTRARY CONTAINED IN THIS LICENSE, TO THE FULLEST EXTENT PERMITTED BY LAW, IN NO EVENT WILL ARM BE LIABLE FOR ANY DAMAGES, IN CONTRACT, TORT OR OTHERWISE, IN CONNECTION WITH THE SUBJECT MATTER OF THIS LICENSE (INCLUDING WITHOUT LIMITATION) (I) LICENSEE’S USE OF THE DOCUMENT; AND (II) THE IMPLEMENTATION OF THE DOCUMENT IN ANY PRODUCT CREATED BY LICENSEE UNDER THIS LICENSE). THE EXISTENCE OF MORE THAN ONE CLAIM OR SUIT WILL NOT ENLARGE OR EXTEND THE LIMIT. LICENSEE RELEASES ARM FROM ALL OBLIGATIONS, LIABILITY, CLAIMS OR DEMANDS IN EXCESS OF THIS LIMITATION.

This License shall remain in force until terminated by Licensee or by Arm. Without prejudice to any of its other rights, if Licensee is in breach of any of the terms and conditions of this License then Arm may terminate this License immediately upon giving written notice to Licensee. Licensee may terminate this License at any time. Upon termination of this License by Licensee or by Arm, Licensee shall stop using the Document and destroy all copies of the Document in its possession. Upon termination of this License, all terms shall survive except for the License grants.

Any breach of this License by a Subsidiary shall entitle Arm to terminate this License as if you were the party in breach. Any termination of this License shall be effective in respect of all Subsidiaries. Any rights granted to any Subsidiary

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

iii

hereunder shall automatically terminate upon such Subsidiary ceasing to be a Subsidiary.

The Document consists solely of commercial items. Licensee shall be responsible for ensuring that any use, duplication or disclosure of the Document complies fully with any relevant export laws and regulations to assure that the Document or any portion thereof is not exported, directly or indirectly, in violation of such export laws.

This License may be translated into other languages for convenience, and Licensee agrees that if there is any conflict between the English version of this License and any translation, the terms of the English version of this License shall prevail.

The Arm corporate logo and words marked with ® or ™ are registered trademarks or trademarks of Arm Limited (or its subsidiaries) in the US and/or elsewhere. All rights reserved. Other brands and names mentioned in this document may be the trademarks of their respective owners. No license, express, implied or otherwise, is granted to Licensee under this License, to use the Arm trade marks in connection with the Document or any products based thereon. Visit Arm’s website at http://www.arm.com/company/policies/trademarks for more information about Arm’s trademarks.

The validity, construction and performance of this License shall be governed by English Law. Copyright © 2018-2025 Arm Limited (or its affiliates). All rights reserved. Arm Limited. Company 02557590 registered in England. 110 Fulbourn Road, Cambridge, England CB1 9NJ. Arm document reference: PRE-21585 version 5.0, March 2024

###### Confidentiality Status

This document is Non-Confidential. The right to use, copy and disclose this document may be subject to license restrictions in accordance with the terms of the agreement entered into by Arm and the party that Arm delivered this document to.

###### Product Status

The information in this document is final, that is for a developed product. The information in this supplement is at EAC quality, which means that all features of the specification are described in the supplement.

###### Web Address

https://www.arm.com

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

iv

Contents

#### Arm® MPAM Memory System Component (MSC) Specification

###### Preface

About this book.................................................................................................................................. x Using this book.................................................................................................................................. xi Conventions ..................................................................................................................................... xiii Additional reading............................................................................................................................... xv Feedback......................................................................................................................................... xvi

###### Chapter 1 About MPAM

- 1.1 Introduction to MPAM .............................................................................................................. 18
- 1.2 Versions of the MPAM Memory System Component architecture ......................................................... 20
- 1.3 MPAM and the Arm memory system architecture ............................................................................ 25


###### Chapter 2 PARTID spaces, Partition Identifiers, and Performance Monitoring Groups

- 2.1 PARTID spaces, Partition Identifiers (PARTIDs), and Performance Monitoring Groups (PMGs) .................... 27

Chapter 3 System Model

- 3.1 Overview ............................................................................................................................. 29


- 3.2 Model of an MPAM system ....................................................................................................... 31
- 3.3 The MPAM for RME system ...................................................................................................... 33
- 3.4 Interconnect behavior.............................................................................................................. 39
- 3.5 Cache behavior ..................................................................................................................... 40
- 3.6 Memory-channel controller behavior ............................................................................................ 42
- 3.7 System reset of MPAM controls in MSCs ...................................................................................... 43


###### Chapter 4 MPAM in MSCs

- 4.1 Model of partitioning and monitoring in an MPAM MSC ..................................................................... 45
- 4.2 Resource instance selection...................................................................................................... 46
- 4.3 Virtualization support in MPAM MSCs .......................................................................................... 51
- 4.4 Four-space MPAM MSCs ......................................................................................................... 52
- 4.5 Security in MPAM MSCs .......................................................................................................... 53
- 4.6 Translation sequence for ingress and egress translation regimes in an MPAM MSC.................................. 54


###### Chapter 5 Resource Partitioning Controls

- 5.1 Introduction .......................................................................................................................... 56
- 5.2 Standard partitioning control interfaces......................................................................................... 57


Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

v

Contents

- 5.3 IMPLEMENTATION DEFINED partitioning control interfaces .............................................................. 67
- 5.4 PARTID narrowing.................................................................................................................. 68
- 5.5 MPAM Domains PARTID translation ............................................................................................ 69


###### Chapter 6 Resource Monitors

- 6.1 About MPAM resource monitors ................................................................................................. 71
- 6.2 Common features................................................................................................................... 75
- 6.3 Monitor configuration............................................................................................................... 79
- 6.4 Monitor behavior on overflow..................................................................................................... 80
- 6.5 Monitor behavior in the presence of ingress and egress translation ...................................................... 82


###### Chapter 7 MPAM MSC Errors

- 7.1 Reporting errors..................................................................................................................... 84
- 7.2 Behavior of configuration reads and writes with errors ...................................................................... 88
- 7.3 Optionality of error detection and reporting .................................................................................... 92


###### Chapter 8 MPAM MSC Interrupts

- 8.1 Interrupt sources.................................................................................................................... 94
- 8.2 Error interrupt........................................................................................................................ 95
- 8.3 Overflow interrupt................................................................................................................... 97
- 8.4 Message signaled interrupts...................................................................................................... 99


###### Chapter 9 MPAM MSC Memory-mapped Registers

- 9.1 Overview of MMRs ................................................................................................................. 101
- 9.2 MPAM feature pages............................................................................................................... 104
- 9.3 The fixed-point fractional format ................................................................................................. 106
- 9.4 Summary of memory-mapped registers ........................................................................................ 108
- 9.5 Memory-mapped ID register description ....................................................................................... 110
- 9.6 Memory-mapped partitioning configuration registers......................................................................... 171
- 9.7 Memory-mapped Monitoring_config register description .................................................................... 217
- 9.8 Memory-mapped error control and status register description ............................................................. 290
- 9.9 Memory-mapped translation configuration registers ......................................................................... 310


##### Part A Appendixes

###### Appendix A1 Generic Resource Controls

- A1.1 Portion resource controls.......................................................................................................... 331
- A1.2 Maximum-usage resource controls.............................................................................................. 332
- A1.3 Proportional resource allocation facilities ...................................................................................... 333
- A1.4 Combining resource controls ..................................................................................................... 335


Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

vi

- Appendix A2 MSC Firmware Data

- A2.1 Introduction .......................................................................................................................... 337

Appendix A3 Examples of partial MPAM implementations

- A3.1 Examples............................................................................................................................. 340




Glossary

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

vii

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

viii

### Preface

This preface introduces the MPAM Memory System Component (MSC) specification. It contains the following sections:

- • About this book.
- • Using this book.
- • Conventions.
- • Additional reading.
- • Feedback.


Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

ix

About this book

###### About this book

This book is the MPAM Memory System Component (MPAM MSC) specification . It specifies:

- • Memory-mapped registers and standard types of resource control interfaces for MPAM MSCs.
- • Memory-mapped registers and resource usage monitors for measuring resource usage in MPAM MSCs.

Together, these facilities permit software both to observe memory system usage and to allocate system resources to software by running that software in a memory system partition.

This document defines the versions of the MPAM MSC architecture. See Versions of the MPAM Memory System Component architecture.

This document primarily describes hardware architecture. As such, it does not include information on either the software needed to control these facilities or the ways to implement effective controls of the memory system using the parameters defined by this architecture.

This document does not describe:

- • Which optional features to implement in a MSC.
- • Which resources and MSCs to be controlled by MPAM.


In this document, VMware is a trademark of Broadcom.

###### Intended audience

This document targets the following audience:

• Hardware and software developers interested in the MPAM system component architecture.

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

x

###### Using this book

This book is organized into the following chapters and appendixes:

###### About MPAM

Read this chapter for an introduction to MPAM and the versions of MPAM.

###### PARTID spaces, Partition Identifiers (PARTIDs), and Performance Monitoring Groups (PMGs)

Read this chapter for a description of PARTID spaces, PARTIDs, and PMGs.

###### System Model

Read this chapter for a description of how the memory system propagates MPAM information, an example system model, and the MPAM for RME system.

###### MPAM in MSCs

Read this chapter for a description of a general example model of partitioning and monitoring in an MPAM MSC, and Resource Instance Selection (RIS) in an MPAM MSC.

###### Resource Partitioning Controls

Read this chapter for a description of memory system partitioning.

###### Resource Monitors

Read this chapter for a description of performance monitoring groups.

###### MPAM MSC errors

Read this chapter for a description of errors in MPAM MSCs.

###### MPAM MSC interrupts

Read this chapter for a description of interrupts in MPAM MSCs.

###### MPAM MSC Memory-mapped Registers

Read this chapter for a description of memory-mapped registers.

- Appendix A1: Generic Resource Controls Read this appendix for a description of Generic Resource Controls.
- Appendix A2: MSC Firmware Data Read this appendix for a description of MSC Firmware Data.
- Appendix A3: Examples of Partial MPAM Implementations Read this appendix for examples of partial MPAM implementations.


###### Glossary

Read this glossary for definitions of some of the terms that are used in this manual. The Arm Glossary does not contain terms that are industry standard unless the Arm meaning differs from the generally accepted meaning.

###### Note

Arm publishes a single glossary that relates to most Arm products, see the Arm Glossary through Arm Developer at https://developer.arm.com/documentation/aeg0014/latest. A definition in the glossary in this supplement might be more detailed than the corresponding definition in Arm Glossary.

###### How to read this book

For readers new to MPAM, read Chapters 1 to 3.

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

xi

For readers interested in MPAM MSC resource controls and MPAM MSC resource monitors, read Chapters 4 to 6, and Appendices A1 and A2.

For readers interested in MPAM MSC errors and interrupts, read Chapters 7 and 8. See Chapter 9 for the MPAM MSC memory-mapped registers. For readers interested in pseudocode and pseudocode definitions, read the Arm® Architecture Reference Manual for A-profile architecture (ARM DDI 0487). For readers interested in Realm Management Extension, RME, read the Arm® Architecture Reference Manual for A-profile architecture (ARM DDI 0487).

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

xii

###### Conventions

The following sections describe conventions that this book can use:

- • Typographic conventions.
- • Rules based writing
- • Signals.
- • Numbers.


###### Typographic conventions

The typographical conventions are: italic Introduces special terminology, and denotes citations. bold Denotes signal names, and is used for terms in descriptive lists, where appropriate. monospace Used for assembler syntax descriptions, pseudocode, and source code examples.

Also used in the main text for instruction mnemonics and for references to other items appearing in assembler syntax descriptions, pseudocode, and source code examples.

small capitals

Used for a few terms that have specific technical meanings, and are included in the Glossary LINK. Colored text Indicates a link. This can be:

- • A URL, for example, http://developer.arm.com
- • A link to a chapter or appendix, or to a glossary entry, or to the section of the document that defines the colored term.


###### Rules-based writing

Some sections of this Document use rules-based writing. Rules-based writing consists of a set of individual content items. A content item is classified as one of the following:

- • Rule.
- • Information.
- • Software usage.
- • Declaration.


Rules are normative statements. An implementation that is compliant with this specification must conform to all Rules in this Document that apply to that implementation.

Rules must not be read in isolation. Where a particular feature is specified by multiple Rules, these are generally grouped into sections and subsections that provide context. Where appropriate, these sections begin with a short introduction.

Arm strongly recommends that implementers read all chapters and sections of this Document to ensure that an implementation is compliant.

Content items other than Rules are informative statements. These are provided as an aid to understanding this Document.

Content item identifiers

A content item may have an associated identifier which is unique among content items in this Document. After content reaches beta status, a given content item has the same identifier across subsequent versions of this Document.

Content item rendering

In this Document, a content item is rendered with a token of the following format in the left margin: Liiiii. L is a label that indicates the content class of the content item. iiiii is the identifier of the content item.

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

xiii

###### Signals

###### Numbers

Content item classes

Each of the content item classes has a different function in this Document.

Rule

A Rule is a statement that describes the behavior of a compliant implementation. A Rule explains what happens in a particular situation. A Rule does not define concepts or terminology. A Rule is rendered with the label R.

Information

An Information statement provides information and guidance as an aid to understanding the Document. An Information statement is rendered with the label I.

Software usage

A Software usage statement provides guidance on how software can make use of the features defined by the specification.

A Software usage statement is rendered with the label S.

Declaration

A Declaration statement introduces concepts or terminology. A Declaration does not describe behavior. A Declaration is rendered with the label D.

In general this specification does not define processor signals, but it does include some signal examples and recommendations.

The signal conventions are:

Signal level The level of an asserted signal depends on whether the signal is active-HIGH or active-LOW. Asserted

means:

- • HIGH for active-HIGH signals.
- • LOW for active-LOW signals.


Lower-case n At the start or end of a signal name denotes an active-LOW signal.

Numbers are normally written in decimal. Binary numbers are preceded by 0b, and hexadecimal numbers by 0x. In both cases, the prefix and the associated value are written in a monospace font, for example 0xFFFF0000.

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

xiv

Additional reading

###### Additional reading

This section lists relevant publications from Arm and third parties. See Arm Developer, https://developer.arm.com, for access to Arm documentation.

###### Arm publications

This book contains information that is specific to this product. See the following documents for other relevant information:

- • Arm® Architecture Reference Manual for A-profile architecture (ARM DDI 0487).
- • Arm® CoreSight Architecture Specification v2.0 (ARM IHI 0029).
- • ARM® Generic Interrupt Controller Architecture Specification, GIC architecture version 3.0 and version 4.0 (ARM IHI 0069).
- • Arm® System Memory Management Unit Architecture Specification, SMMU architecture version 3 (ARM IHI 0070).


Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

xv

Feedback

###### Feedback

Arm welcomes feedback on its documentation.

###### Feedback on this Manual

If you have any comments or queries about this Manual, create a ticket at https://support.developer.arm.com. As part of the ticket, include:

- • The title, Arm® MPAM Memory System Component (MSC) Specification.
- • The number, Arm IHI 0099A.b
- • The section name to which your comments refer.
- • The page number(s) to which your comments refer.
- • The rule identifier(s) to which your comments refer, if applicable.
- • A concise explanation of your comments.


Arm also welcomes general suggestions for additions and improvements.

###### Note

Arm tests PDFs only in Adobe Acrobat and Acrobat Reader, and cannot guarantee the appearance or behavior of any document when viewed with any other PDF reader.

###### Inclusive terminology commitment

Arm values inclusive communities. Arm recognizes that we and our industry have used terms that can be offensive. Arm strives to lead the industry and create change.

Previous issues of this document included language that can be offensive. We have replaced this language. To report offensive language in this document, email terms@arm.com.

Arm IHI 0099

Copyright © 2018-2025 Arm Limited or its affiliates. All rights reserved.

xvi

### Chapter 1About MPAM

This chapter contains the following sections:

- • Introduction to MPAM.

— MPAM information bundles. Memory system resource partitioning.

— Memory system resource usage monitoring.

- • Versions of the MPAM Memory System Component architecture.
- • MPAM and the Arm memory system architecture.


###### 1.1 Introduction to MPAM

IYYSRL In many systems, multiple software execution environments such as applications and virtual machines (VMs) run concurrently, sharing memory system resources such as memory controllers, caches, and interconnects. With no controls on the sharing of resources, software execution environments might cause interference for each other as they try to access resources. This might result in increased latency or reduced bandwidth, or both, for memory accesses for workloads that are performance sensitive. Memory system resource Partitioning and Monitoring (MPAM) provides software with control mechanisms to apportion the memory system resources between software execution environments. This is done by:

- • Memory system resource partitioning.
- • Memory system resource usage monitoring, one of the uses of which is to inform adjustment of the partitioning.


IRKDKZ MPAM provides all of:

- • An extension to the Arm® A-profile PE architecture:

— A mechanism for privileged software executing in a PE to attach a Partition Identifier (PARTID) label, and a monitoring property called a Performance Monitoring Group (PMG), to a software execution environment in that PE. If FEAT_MPAM_PE_BW_CTRL is implemented, then the PE implements a mechanism for software to impose a maximum limit on the bandwidth that requests from the PE itself can use. See the Arm® Architecture Reference Manual for A-profile architecture for more information on MPAM PE-side Maximum-bandwidth limit injection control.

- • For the system: A description of the propagation of PARTIDs, PMGs, and PARTID spaces as encoded onto the MPAM_NS or MPAM_SP signal, through the system.


— A framework for implementing MPAM resource controls in MPAM Memory System Components

(MPAM MSCs). These are the controls that partition the resources of MPAM MSCs.

— An extension to that framework, for implementing MPAM resource monitors in MPAM MSCs.

— An extension to that framework, for MPAM MSCs to have performance monitoring that is sensitive to a combination of PARTID and PMG.

— Some implementation-independent, memory-mapped interfaces to memory system component controls for performance resource controls most likely to be deployed in systems. Some implementation-independent, memory-mapped interfaces to memory system component resource monitoring that might be required to monitor the partitioning of memory system resources.

IPKXWQ When designing an SoC, the designer makes several or all the following SoC components MPAM-compliant, meaning

that they implement one of the FEAT_MPAM versions:

- • Requester component types, for example PEs, DSPs, and DMA controllers.
- • Components that provide shared memory system resources that it is possible to partition, for example shared caches, shared interconnects and buffers, Memory Management Units (MMUs), and memory channel controllers.


There is no requirement to make all components in an SoC MPAM-compliant.

ILYBYD In this specification:

- • An MPAM Requester is any type of MPAM-compliant Requester component. These are categorized into: Arm® MPAM PE Requesters. MPAM Non-PE Requesters.
- • An MPAM Memory System Component (MPAM MSC) is an MPAM-compliant component that provides shared memory system resources.


This specification describes MPAM MSCs, and the Arm® Architecture Reference Manual describes MPAM PE Requesters. The MPAM architecture does not describe MPAM Non-PE Requesters. Also see Requester components.

IHRMSG The architecture permits an MPAM MSC to be implemented as part of another system component. For example, a PE can contain caches that are MPAM MSCs.

- 1.1.1 MPAM information bundles RBDJXQ MPAM Requesters label every request to the memory system with an MPAM information bundle, comprising:


- • Two items that together identify a single partition: A number called the Partition Identifier (PARTID).

— A Partition ID space, called the PARTID space. Each PARTID space defines a set of PARTIDs. Each PARTID references one partition.

- • A Performance Monitoring Group (PMG).


IZDKRM An MPAM information bundle is used by each MPAM-controlled shared resource that is accessed in the handling of a request, to:

- • Access the MPAM control settings that are present.
- • Determine the filtering for MPAM monitors that are present.


- 1.1.2 Memory system resource partitioning RDDGSM Each MPAM MSC implements zero or more MPAM resource controls.

IGPWRB Each MPAM resource control has settings for each PARTID.

IDTCTY When an MPAM Requester sends a request, the PARTID space and PARTID in the MPAM information bundle are used as an index into a table in the MPAM MSC, to select the entry that holds the settings for that partition. Those settings configure how the shared partitionable resource is apportioned to that partition.

IPWTBN See also:

- • Model of partitioning and monitoring in an MPAM MSC.
- • Resource Partitioning Controls.


- 1.1.3 Memory system resource usage monitoring RRBCMK Each MPAM MSC implements zero or more MPAM resource monitors.


ILDRSM An MPAM resource monitor is associated with a shared partitionable resource in an MPAM MSC, and is able to measure how much a software execution environment is using that resource, by either:

- • For a memory-bandwidth usage monitor, counting payload bytes that are passing where the monitor is placed.
- • For a cache-storage usage monitor, counting bytes of the cache storage used.


IHQBLT An MPAM resource monitor must be configured and enabled before it can be queried for usage of the MPAM MSC resource. Each monitor has a configurable filter register that the monitor uses to filter which bytes are counted. Fields in the filter register are writes, reads, PARTID, PMG, and other criteria. For example, a monitor can be configured to be sensitive to a PARTID, PMG, or both.

IBKSLY See also:

- • Model of partitioning and monitoring in an MPAM MSC.
- • Resource Monitors.


###### 1.2 Versions of the MPAM Memory System Component architectureThis section comprises:

- • MPAM versions for MPAM MSCs.
- • MSC features added in MPAM v1.1.
- • Interoperation of SoC components implementing different MPAM versions.


- 1.2.1 MPAM versions for MPAM MSCs IMCCFK The MPAM versions that MPAM MSCs use are a subset of those that MPAM PEs use.


RCFXGZ An MPAM MSC implements one of these MPAM versions:

Table 1-1 MPAM MSC architecture versions

MPAMF_AIDR Version Support ArchMajorRev ArchMinorRev

- 0b0000 0b0000 None The memory system component does not implement MPAM.

- 0b0000 0b0001 v0.1 Permitted if and only if all the following apply:


- • The MPAM MSC can initiate requests.
- • Requests can be initiated in the Secure address space.
- • Requests to the Secure address space can have MPAM_NS forced to 1.
- • Software that configures the MPAM MSC to make requests in the Secure address space:


— Cannot control the forcing of MPAM_NS.

— Cannot easily see that MPAM_NS is being forced.

- 0b0001 0b0000 v1.0 The MPAM MSC implements MPAM v1.0 with features as described in the 32-bit MPAMF_IDR.

- 0b0001 0b0001 v1.1 The MPAM MSC implements MPAM v1.1 with features as described in the 64-bit MPAMF_IDR.


IHLKRW v1.0 is the base version of MPAM. v0.1 and v1.1 include features beyond the base version.

ISWKSL Table 1-2 shows MPAM MSC features by MPAM version.

IZFLZF In Table 1-2, MPAM for RME supports the Realm Management Extension (RME) in systems, MPAM PEs and MPAM MSCs.

RFXVBB MPAM for RME requires MPAM v1.1 or higher, and requires support for 4 PARTID spaces. See The MPAM for RME system.

IYPLKN MPAMF_IDR reports which MPAM features are implemented. MPAMF_IDR is permitted to have different MPAM features in different address spaces. If MPAM resource instance selection is implemented, then MPAMF_IDR is also permitted to have different features for different Resource Instances in an MPAM MSC.

Table 1-2 MPAM MSC features by MPAM version

Subordinate feature

Subordinate feature 2 MPAM v1.0

MPAM v0.1/v1.1

MPAM for RME ID field

MPAM feature

Cache capacity partitioning

Optional Optional Optional MPAMF_IDR.HAS_CCAP_PART

Minimum cache capacity partitioning

- Prohibited Optional Optional MPAMF_CCAP_IDR.HAS_CMIN

No maximum cache capacity partitioning

- Prohibited Optional Optional MPAMF_CCAP_IDR.NO_CMAX

Cache maximum associativity partitioning

- Prohibited Optional Optional MPAMF_CCAP_IDR.HAS_ CASSOC

CMAX soft limit

- Prohibited Optional Optional MPAMF_CCAP_IDR.HAS_CMAX_SOFTLIM

Cache portion partitioning

- - Optional Optional Optional MPAMF_IDR.HAS_CPOR_PART

PARTID disable

Prohibited Optional Optional MPAMF_IDR.HAS_ENDIS No Future Use - Prohibited Optional Optional MPAMF_IDR.HAS_NFU

Memory BW partitioning

Optional Optional Optional MPAMF_IDR.HAS_MBW_PART

Minimum BW partitioning

- Optional Optional Optional MPAMF_MBW_IDR.HAS_MIN

Maximum BW partitioning

- Optional Optional Optional MPAMF_MBW_IDR.HAS_MAX

Maximum BW partitioning limit behaviors

- Optional Optional Optional MPAMF_MBW_IDR.MAX_LIM

BW portion partitioning

- Optional Optional Optional MPAMF_MBW_IDR.HAS_PBM

Proportional BW partitioning

- Optional Optional Optional MPAMF_MBW_IDR.HAS_PROP

BW window writable

- Optional Optional Optional MPAMF_MBW_IDR.WINDWR

Priority partitioning

Optional Optional Optional MPAMF_IDR.HAS_PRI_PART

Internal priority partitioning

- Optional Optional Optional MPAMF_PRI_IDR.HAS_INTPRI

Downstream priority partitioning

- Optional Optional Optional MPAMF_PRI_IDR.HAS_DSPRI

Memory Sys resource monitoring

Optional Optional Optional MPAMF_IDR.HAS_MSMON

Cache storage usage monitoring

- Optional Optional Optional MPAMF_MSMON_IDR.MSMON_ CSU

CSU monitor capture

Optional Optional Optional MPAMF_CSUMON_IDR.HAS_ CAPTURE

CSU monitor read-only

Optional Optional Optional MPAMF_CSUMON_IDR.CSU_RO

CSU monitor XCL

Prohibited Optional Optional MPAMF_CSUMON_IDR.HAS_XCL

CSU monitor overflow linkage

Prohibited Optional Optional MPAMF_CSUMON_IDR.HAS_ OFLOW_LNKG

CSU monitor overflow status Reg

Prohibited Optional Optional MPAMF_CSUMON_IDR.HAS_ OFSR

CSU monitor overflow capture

Prohibited Optional Optional MPAMF_CSUMON_IDR.HAS_ CEVNT_OFLW

Memory BW usage monitoring

- Optional Optional Optional MPAMF_MSMON_IDR. MSMON_MBWU

Optional Optional Optional MPAMF_MBWUMON_IDR.HAS_ CAPTURE

MBWU monitor capture

MBWU monitor Long

Optional Optional Optional MPAMF_MBWUMON_IDR.HAS_ LONG

MBWU monitor R/W filtering

Optional Optional Optional MPAMF_MBWUMON_IDR.HAS_ RWBW

MBWU monitor scaling

Optional Optional Optional MPAMF_MBWUMON_IDR. SCALE

MBWU monitor overflow linkage

Prohibited Optional Optional MPAMF_MBWUMON_IDR.HAS_ OFLOW_LNKG

Prohibited Optional Optional MPAMF_MBWUMON_IDR.HAS_ OFSR

MBWU monitor overflow status Reg

Prohibited Optional Optional MPAMF_MBWUMON_IDR.HAS_ CAPTURE

MBWU monitor overflow capture

Monitor overflow status register

- Prohibited Optional Optional MPAMF_MSMON_IDR.HAS_ OFLOW_SR

Monitor overflow MSI

- Prohibited Optional Optional MPAMF_MSMON_IDR.HAS_ OFLW_MSI

No hardwired overflow interrupt

- Prohibited Optional Optional MPAMF_MSMON_IDR.NO_OFLW_INTR

Local monitor capture event generator

- Optional Optional Optional MPAMF_MSMON_IDR.HAS_ LOCAL_CAPT_EVNT

PARTID narrowing

- - Optional Optional Optional MPAMF_IDR.HAS_PARTID_NRW

FEAT_MPAM_ MSC_DOMAINS

Ingress translation

- Optional Optional Optional MPAMF_IDR.HAS_IN_TL

Egress translation

- Optional Optional Optional MPAMF_IDR.HAS_OUT_TL

FEAT_MPAM_ MSC_DCTRL

- - Optional Optional Optional MPAMF_IDR.HAS_DEFAULT_PARTID

Implementationdefined ID Reg

- - Optional Optional Optional MPAMF_IDR.HAS_IMPL_IDR Impl IDR no partitioning

- Prohibited Required Required MPAMF_IDR.NO_IMPL_PART

Impl IDR no monitoring

- Prohibited Required Required MPAMF_IDR.NO_IMPL_MSMON

Extended ID register

- - Prohibited Required Required MPAMF_IDR.EXT

Resource instance selector

- - Prohibited Optional Optional MPAMF_IDR.HAS_RIS

Error status register

Prohibited Optional Optional MPAMF_IDR.HAS_ESR

Extended error status register

- Prohibited Optional Optional MPAMF_IDR.HAS_EXTD_ESR

Error MSI - - Prohibited Optional Optional MPAMF_IDR.HAS_ERR_MSI Four PARTID spaces

- - Required MPAMF_IDR.SP4

- 1.2.2 MSC features added in MPAM v1.1 This subsection describes the features that can be implemented in an MPAM v1.1 MSC.


ISKQSV Expansion of MPAMF_IDR MPAMF_IDR is expanded to 64 bits to support bits that indicate the presence of features added from MPAM v1.1. This feature is mandatory when the MPAM MSC implements MPAM v1.1. This feature is implemented when MPAMF_IDR.EXT is set to 1. For more information, see MPAMF_IDR.

INXHJW IMPLEMENTATION DEFINED resource partitioning controls or resource monitoring This feature defines two fields that allow discovery of any IMPLEMENTATION DEFINED resource partitioning controls or IMPLEMENTATION DEFINED resource monitors that are implemented. This feature is mandatory when the MPAM MSC implements MPAM v1.1 and MPAMF_IDR.HAS_IMPL_IDR is 1. This feature is implemented when MPAMF_IDR.EXT is 1. Furthermore:

- • When MPAMF_IDR.NO_IMPL_PART is 1, MPAMF_IMPL_IDR does not include the description of any implementation-specific resource partitioning controls.
- • When MPAMF_IDR.NO_IMPL_MSMON is 1, MPAMF_IMPL_IDR does not include the description of any implementation-specific resource monitors.


For more information, see MPAMF_IDR.

IZCTYR Resource instance selection Resource instance selection, or RIS, provides access to the control settings of multiple resources of the same type within one MPAM MSC. This feature is optional when the MPAM MSC implements MPAM v1.1. This feature is implemented when MPAMF_IDR.EXT and MPAMF_IDR.HAS_RIS are 1. For more information, see

- • Resource instance selection.
- • MPAMCFG_PART_SEL.
- • MSMON_CFG_MON_SEL.
- • Error conditions in accessing memory-mapped registers.


IDWBWS Greater range for MBWU monitors This feature supports 44-bit and 63-bit memory bandwidth usage counters. This feature is optional when the MPAM MSC implements MPAM v1.1. This feature is implemented when MPAMF_MBWUMON_IDR.HAS_LONG is 1. For more information, see Long MBWU counter and capture.

ICTGJJ Discovery of MPAMF_ESR and MPAMF_ECR This feature supports the MPAMF_IDR.HAS_ESR field. This field indicates whether MPAMF_ESR and MPAMF_ECR are implemented. This feature is mandatory when the MPAM MSC implements MPAM v1.1. This feature is implemented when MPAMF_IDR.EXT is 1. For more information, see MPAMF_IDR.

IBFFDS Expansion of MPAMF_ESR This feature widens MPAMF_ESR to 64 bits to include space for a RIS field. This feature is optional when the MPAM MSC implements MPAM v1.1. Implementation of this feature is mandatory if MPAMF_IDR.{HAS_ESR, HAS_RIS} are 1. This feature is implemented when MPAMF_IDR.{EXT, HAS_EXTD_ESR} are 1. For more information, see

- • MPAMF_ESR
- • Resource instance selection.


###### 1.2.3 Interoperation of SoC components implementing different MPAM versions

RLRZGD MPAM MSCs and MPAM PEs that implement different versions of the MPAM architecture are permitted to coexist in a system.

IWNJJN There is no required relationship between the MPAM version of an MPAM PE and the MPAM version of an MPAM MSC accessed by the MPAM PE.

- 1.3 MPAM and the Arm memory system architecture


###### 1.3 MPAM and the Arm memory system architecture

RPBSTK The requirements for memory behavior, including observation, coherence, caching, order, atomicity, endianness,

alignment, and memory types, specified in the Arm® Architecture Reference Manual for A-profile architecture, always apply regardless of any MPAM configuration or any MPAM partitioning of memory system performance resources. This includes all of the following situations:

- • When an MSC receives MPAM information bundles from one or more requestors, and those information bundles might contain different values, for example different PARTID or PMG values.
- • For all MPAM MSC resource controls and configurations.
- • When MPAM information stored with data accessed from caches is the same as, or different from, MPAM information in requests that access that data.


### Chapter 2PARTID spaces, Partition Identifiers, and PerformanceMonitoring Groups

This chapter contains the following sections:

- • PARTID spaces.
- • Partition Identifiers (PARTIDs).
- • Performance Monitoring Groups (PMGs).


PARTID spaces, Partition Identifiers, and Performance Monitoring Groups

- 2.1 PARTID spaces, Partition Identifiers (PARTIDs), and Performance Monitoring Groups (PMGs)


###### 2.1 PARTID spaces, Partition Identifiers (PARTIDs), and Performance Monitoring Groups (PMGs)

- 2.1.1 PARTID spaces RKXMHB The following physical PARTID spaces are supported:

- • Non-secure PARTID space.
- • If Secure state is implemented, Secure PARTID space.
- • If RME is implemented, both of:


— Realm PARTID space.

— Root PARTID space.

- 2.1.2 Partition Identifiers (PARTIDs)

INHJZF A Partition Identifier (PARTID) is a number that has no inherent meaning and does not imply any ordering. The

number solely references a particular partition in a PARTID space.

RCZHSY Each PARTID can exist once in each PARTID space.

RRBWRS The scope of a PARTID is bound by its PARTID space. If a PARTID is used in multiple PARTID spaces, then a reference to that PARTID in one PARTID space does not also reference that PARTID in the other PARTID spaces.

INFCZF Each MPAM-compliant component in an SoC is permitted to implement a maximum PARTID width that is less than or

equal to the architecturally defined maximum width of 16-bits.

IQDFWX Arm recommends that all MPAM-compliant components in an SoC implement the same maximum PARTID, though this is not required.

INRXGZ In an MPAM MSC, MPAMF_IDR.PARTID_MAX reports the maximum PARTID supported in each PARTID space.

2.1.2.1 Default PARTID

RQKQCF The Default PARTID number, in each PARTID space, is 0. The MPAM architecture permits SoC designers to make non-MPAM Requesters label every request to the memory system with the Default PARTID.

- 2.1.3 Performance Monitoring Groups (PMGs)


INQQHK An MPAM resource monitor measures how much of a shared partitionable resource a software execution environment is using. See Memory system resource usage monitoring. A Performance Monitoring Group (PMG) is an identifier used in the filter register of a monitor, MSMON_CFG_MBWU_FLT or MSMON_CFG_CSU_FLT. Filtering by PMG provides a way to group measurements, and a way to subdivide measurements. For example:

- • Multiple monitors, each measuring usage of a different shared resource, can be made sensitive to a single PMG and no PARTIDs, so that they create a group of measurements for usage of the different shared resources by one PMG value.
- • Monitors can be made sensitive to both a PARTID and a PMG, so that the PMG provides a subdivision of what is measured for the PARTID.


2.1.3.1 Default PMG

RCZSZX The Default PMG number is 0. The MPAM architecture permits SoC designers to make non-MPAM Requesters label

every request to the memory system with the Default PMG.

### Chapter 3System Model

This chapter contains the following sections:

- • Overview.
- • Model of an MPAM system.
- • The MPAM for RME system.
- • Interconnect behavior.
- • Cache behavior.
- • Memory-channel controller behavior.
- • System reset of MPAM controls in MSCs.


###### 3.1 Overview

IYGYJC In this specification, the following definitions apply:

- • The downstream direction is from Requester to Completer.
- • The upstream direction is from Completer to Requester.


IHCPQQ When MPAM information bundles are propagated, all of the following apply:

- • Propagation is downstream.
- • The propagation behavior depends on the function of the MPAM-compliant component. See:


— Requester components. Terminating Completer components.

— Intermediate Completer-Requester components. Request buffering.

— Cache memory.

IQHNZC If an MPAM MSC has no downstream components that use MPAM information, the MPAM MSC is not required to propagate MPAM information.

- 3.1.1 Requester components IVFXYC In an MPAM system, there might be the following types of Requester:


- • MPAM Requesters: MPAM PE-Requesters. MPAM Non-PE Requesters.
- • Non-MPAM Requesters, that do not implement MPAM.


RNVNHD MPAM Requesters must have a mechanism for generating MPAM information to send in MPAM information bundles, as shown in the following table:

MPAM PE Requesters MPAM Non-PE Requesters: MPAM Non-PE Requester:

Other MPAM non-PE requesters

Requests from MPAM non-PE Requesters that the SMMU translates The GIC accessing memory

Use the mechanisms in Arm® Generic Interrupt Controller Architecture Specification, GIC architecture version 3.0 and version 4.0 (ARM IHI 0069).

Use the mechanisms in Arm® Architecture Reference Manual, for A-profile architecture (ARM DDI 0487).

Use the mechanisms in Arm® System Memory Management Unit Architecture Specification, SMMU architecture version 3 (ARM IHI 0070).

Use an

IMPLEMENTATION DEFINED

mechanism.

RSLJDR MPAM Non-PE Requesters must use a mechanism that generates all the following:

- • PARTID space, to encode onto MPAM_NS or MPAM_SP.
- • PARTID.
- • PMG.


RVNVMD For non-MPAM Requesters, either:

- • There must be a system-specific mechanism that adds MPAM information to requests.
- • The non-MPAM Requester sends the default PARTID and default PMG, and sets the PARTID space. See PARTID spaces, Partition Identifiers (PARTIDs), and Performance Monitoring Groups (PMGs).


IPYFCS System-specific options for adding MPAM information to requests from non-MPAM Requesters include:

- • Using an SMMU that supports adding MPAM information to requests.
- • Using a bus bridge or other system component that supports adding MPAM information to requests. Other implementations are permitted.


###### 3.1.2 Terminating Completer components

IDPZRS A terminating Completer services requests received from upstream Requesters, and does not propagate the requests further downstream.

RSJFGX An MPAM MSC terminating Completer does not propagate MPAM information further downstream.

ICCJHT

Note

A DRAM controller is a terminating Completer when the DRAM devices it communicates with do not support MPAM. In this case, the DRAM controller does not propagate MPAM information to the DRAM devices.

###### 3.1.3 Intermediate Completer-Requester components

IYTCLJ Intermediate Completer-Requester components have both one or more Completer interfaces and one or more Requester interfaces.

IZNPHY An intermediate Completer-Requester component either:

- • Routes a request from an upstream Requester to one of its downstream Requester ports.
- • Terminates a request, if the intermediate Completer-Requester component services the request.


RYPHLH When routing a request from upstream to downstream, an MPAM MSC intermediate Completer-Requester component passes the MPAM information unaltered to the downstream Requester port.

###### 3.1.4 Request bufferingIBNWYT Requests can be buffered in any MPAM MSC.

RRZCWR A buffered request must retain its MPAM information if the information is needed for operating MSC monitors or controls.

###### 3.1.5 Cache memoryRMRWWK A cache line must store the MPAM information of the request that caused its allocation. See Cache behavior.

###### 3.2 Model of an MPAM system

IHWMPF Figure 3-1 shows a simplified MPAM system model for the downstream flow, in the direction from Requesters to Completers. All items in the figure implement a version of the MPAM MSC architecture except the MPAM PEs, which implement a version of the MPAM PE architecture, and the I/O Requesters and I/O Completers. The interconnects represent bus, crossbar, packet, or other interconnect technologies.

I/O Requesters

|MPAM PE|MPAM PE|
|---|---|
|*L1-I|*L1-D|


|MPAM PE|MPAM PE|
|---|---|
|* L1-I|*L1-D|


. . .

|* Private L2|* Private L2|
|---|---|
| | |


|* Private L2|* Private L2|
|---|---|
| | |


*

Cluster Interconnect

One of N Clusters

|Cluster Cache<br><br>*|
|---|


|*SMMU|
|---|


I/O Completers

*

SoC Coherent Interconnect

|System Cache<br><br>*|
|---|


|Memory Channel Controller<br><br>*|
|---|


. . .

|Memory Channel Controller<br><br>*|
|---|


* MPAM MSC

Figure 3-1 MPAM system model (downstream flow)

IZXKWF In the model, a request:

- 1. Begins at a Requester, such as an I/O Requester, DMA controller, MPAM PE, or graphics processor. MPAM information comprising the PARTID space encoded onto the signal MPAM_NS or MPAM_SP, the PARTID, and the PMG, is transported with every request. For where the MPAM information comes from, see Requester components.
- 2. Traverses non-cache nodes that might be a transport component such as an interconnect, a bus resizer, or an asynchronous bridge.
- 3. Might reach an MPAM MSC that contains or is a cache:


- • Caches sometimes generate a response (cache hit) and sometimes pass the request on (cache miss).
- • Caches could also allocate entries based on the request.
- • Caches must store the PARTID, PMG, and PARTID space associated with an allocation: Needed for cache-storage usage monitoring.

— Used during eviction to another cache.

- • Cache eviction must attach MPAM information to the eviction request. The source for MPAM information on an eviction might depend on whether the eviction is to memory or to another cache. See Eviction and Optional cache behaviors.


- 4. Might proceed from a cache to another transport component, and to other caches or a memory-channel controller.
- 5. Might result in a memory controller or other terminating Completer responding to a request it receives.


RXPPBS An MPAM MSC responds to the MPAM information that arrives as part of a request.

RCWTSC MPAM MSC partitioning controls find partitioning settings by the resource partition in the MPAM information of the request, and use those settings to control the allocation of a controlled resource.

RJWKJD For caches, a cache line, which has an address, is always associated with the MPAM information of the request that allocated the line, or with the MPAM information of the request that allocated the line into an inner cache that has now been evicted to the current cache.

RSGBFN The inner cache PARTID must be preserved when the line is evicted to an outer cache.

IDXVLJ An address might be accessed by multiple MPAM resource partitions.

###### 3.2.1 PEs with integrated MPAM MSCs

RZTFBQ A PE might have integrated MPAM MSCs. These are discovered and configured in the same way as other MPAM MSCs. See Memory-mapped Registers.

###### 3.2.2 System-wide PARTID and PMG widthsIBTFZC In order to avoid CONSTRAINED UNPREDICTABLE behavior:

- • The PARTID sent to an MPAM MSC must be within the range of 0 to the smallest maximum PARTID supported by any MSC that the request might access.
- • The PMG sent to an MPAM MSC must be within the range of 0 to the smallest maximum PMG supported by any MSC that the request might access.


For example, if MPAM_NS is 0b0 or MPAM_SP is 0b00, then:

- • The PARTID must be in the range of 0 to the smallest maximum Secure PARTID supported by any MPAM MSC that the request might access.
- • The PMG must be in the range of 0 to the smallest maximum Secure PMG supported by any MPAM MSC that the request might access.


INLCBL Arm recommends that a system be configured to support common values of maximum PARTID (PARTID_MAX) and maximum PMG (PMG_MAX) in all MPAM Requesters and MPAM MSCs in the system.

ISHCMM MPAM ID registers in MPAM PEs and MPAM MSCs report the maximum PARTID and PMG widths. For more information, see MSC Firmware Data and Determining presence and location of MMRs.

###### Note

The smallest maximum values supported by any MPAM MSC can be computed by firmware or software during discovery.

IDWCBR If an MPAM MSC receives a PARTID or PMG outside the supported range, the MSC behavior is CONSTRAINED UNPREDICTABLE. For more information, see Behavior of configuration reads and writes with errors.

###### 3.3 The MPAM for RME system

IYTZJB An MPAM for RME system provides support for four physical address spaces and four PARTID spaces.

IFDGYL It is possible to divide an MPAM for RME system into PARTID space regions. Each region contains MPAM MSCs that

support the same number of PARTID spaces. The PARTID space region types are:

- • Four-space regions.
- • Two-space regions.
- • Non-MPAM regions. An MPAM MSC that supports:
- • Four physical address spaces and four PARTID spaces is a four-space MPAM MSC.
- • Four or two physical address spaces and two PARTID spaces is a two-space MPAM MSC.


RJGWSM An MPAM MSC must support the physical address spaces that correspond to the PARTID spaces that it supports.

RLRNPR An MPAM for RME system includes at least one four-space region, but is not required to include any two-space or non-MPAM regions. More than one of each region type is permitted.

IKMGYZ Arm recommends that when possible, the same number and types of PARTID spaces be supported throughout the system.

RVBHPB A four-space region contains at least all the following:

- • One MPAM PE Requester that implements both FEAT_RME and MPAM for RME.
- • One MPAM MSC that implements MPAM for RME. Also, the interconnects support and propagate the MPAM_SP signal:


MPAM_SP Meaning

- 0b00 Secure PARTID space

- 0b01 Non-secure PARTID space


- 0b10 Root PARTID space

- 0b11 Realm PARTID space


RWZXPM A two-space region contains MPAM-compliant components and interconnects that support and propagate the

MPAM_NS signal:

MPAM_NS Meaning

- 0b0 Secure PARTID space

- 0b1 Non-secure PARTID space


RCBBRW If no MPAM PE Requesters implement Secure state, then the MPAM_SP and/or MPAM_NS encodings for the Secure PARTID space are unused and are RESERVED.

###### 3.3.1 Bridging between PARTID space regions

RCNQCS If a two-space region blocks downstream propagation of Root and Realm PARTID spaces from an RME MPAM PE Requester, then at the boundary where the four-space region ends and the two-space region begins, MPAM_SP must be reduced to MPAM_NS by using a bridge.

IMZZNK Example 3-1 shows an MPAM for RME system with a large four-space region and small two-space region. The boxes

labelled 4 to 2 and 2 to 4 are bridges.

###### Example 3-1 Example system with a large four-space region and a small two-space region

4 PARTID Space Region

PE

PE

PE

PE

PE

PE

I$ D$

I$ D$

I$ D$

I$ D$

I$ D$

I$ D$

L2$

L2$

L2$

L2$

L2$

L2$

Non-MPAM Device

SMMU with MPAM

Interconnect

| | |
|---|---|
| | |


2 PARTID Space Region

System Cache

4 to 2

Device

2 PARTID Interconnect

Device

4 to 2

2 to 4

Device

Mem Chan Ctrl

Mem Chan Ctrl

I/O

Device

Memory

Memory

| | |
|---|---|
| | |


| | |
|---|---|
| | |


Memory

Memory

###### Figure 3-2

IBWMHM As an alternative to using 4 to 2 bridges, RME MPAM PE Requesters can use the Alternative PARTID spaces MPAM PE feature to produce two-space requests, thereby eliminating the need for 4 to 2 bridging logic. Example 3-2 shows an MPAM for RME system that is not using bridges because the RME MPAM PE Requesters are using the Alternative PARTID spaces MPAM PE feature.

###### Example 3-2 Example system with a small four-space region and large two-space region

4 PARTID Space Region

PE

PE

PE

PE

PE

PE

I$ D$

I$ D$

I$ D$

I$ D$

I$ D$

I$ D$

Non-MPAM Device

L2$

L2$

L2$

L2$

L2$

L2$

2 PARTID Space Region

Device

SMMU for

|2 PARTID Interconnect|RME with MPAM|
|---|---|
|2 PARTID Interconnect| |


2 PARTID Interconnect

Interconnect

Device

System Cache

| | |
|---|---|
| | |


Device

| | |
|---|---|
| | |


I/O

Device

Mem Chan Ctrl

Mem Chan Ctrl

Memory

Memory

| | |
|---|---|
| | |


| | |
|---|---|
| | |


Memory

Memory

###### Figure 3-3

RBHHCR If a two-space MPAM MSC is downstream from a four-space MPAM Requester and a 4 to 2 bridge is used, the bridging scheme to reduce MPAM_SP to MPAM_NS is permitted to be static or dynamic.

IMYHSW Arm makes no recommendation for which method to use for bridging from a four-space region to a two-space region.

RBSLZQ If a four-space MPAM MSC is downstream from a two-space MPAM Requester, then at the boundary where the two-space region ends and the four-space region begins, MPAM_NS must be expanded to MPAM_SP, for example by using a fixed mapping as follows:

Table 3-4 A two-space to four-space region fixed expansion scheme

Two-space MPAM_NS input Four-space MPAM_SP[1:0] output

- 0b0, Secure PARTID space 0b00, Secure PARTID space

- 0b1, Non-secure PARTID space 0b01, Non-secure PARTID space


Other bridging mechanisms are permitted.

3.3.1.1 Requirements for bridges

RWBZST The requirements for bridges are:

- • Bridging must not alter the physical address space of a request.
- • Bridging requests that use the Secure PARTID space must not be altered to use a different PARTID space.
- • Bridging requests that use the Non-secure PARTID space must not be altered to use a different PARTID space.


- 3.3.2 Control over monitoring of Root and Realm PARTID space requests bridged to Secure or Non-secure PARTID space


IGPJSL In the examples that follow, a NO_MON flag is used to indicate that the transaction must not be monitored by MPAM monitors or other system performance monitors. This capability improves the security by limiting or preventing the system-level activities of a Realm from being collected in monitors accessible from the Non-secure physical address space or Secure physical address space.

RYWPPT The choice of not monitoring some transactions is not available on true two-space components. Support for the ability to mark requests with the NO_MON flag would require modifying the two-space component.

IXSMNP The example options that follow show a small number of recommended choices for including two-space MPAM Completer MSCs that do not have four-space MPAM support in an RME system:

Alter the two-space MPAM MSC to support four PARTID spaces

This is the recommended option. However, it requires work to redesign the MSC. See Four-space MPAM MSCs for how this is implemented.

Alter the two-space MPAM MSC to support a programmable mapping of four PARTID spaces to two

Alter the two-space MPAM MSC to support a programmable mapping of four PARTID spaces to two PARTID spaces with additional control over whether each of the Root and Realm PARTID spaces can be monitored.

Connect the two-space MPAM MSC through a programmable PARTID-space mapping component

Connect the two-space MPAM MSC through a programmable PARTID-space mapping component, or shim. This gives no control of whether the Root or Realm space can be monitored after being mapped into Secure or Non-secure.

Connect the two-space MPAM MSC to be driven only from MPAM_SP[0] Connect the two-space MPAM MSC so that the single-bit MPAM_NS input of the two-space MPAM MSC is driven only from MPAM_SP[0]. See Fixed space mapping at a Completer.

3.3.2.1 Programmable PARTID space mapping within a Completer

IBCFRV Programmable PARTID space mapping can be performed for an MPAM MSC with PARTID space mapping built into the component, and cannot be performed for an MPAM MSC without PARTID space mapping built into it.

IGPQKK The programmable PARTID space mapper accepts requests with four MPAM spaces, maps requests with MPAM_SP

of Root or Realm to one of the Secure or Non-secure PARTID spaces, and passes it on to the two-space MPAM MSC.

IYJSMF The programmable PARTID space mapper can also produce a flag that indicates the two-space MPAM MSC should not perform MPAM monitoring of the request.

IQJXPT The request mapper register might be called, for example, MAP4SPTO2SP. It has the fields shown in Table 3-5:

Table 3-5 Request mapper programming register (MAP4SPTO2SP) fields

Field bits Field name Description

15 Rt_outPARTID_space If a request has a Root PARTID, the output PARTID uses this bit for MPAM_NS. 14 Rt_NO_MON If the request has a Root PARTID, output this bit as the NO_MON flag. 7 Rl_outPARTID_space If a request has a Realm PARTID, the output PARTID uses this bit for MPAM_NS. 6 Rl_NO_MON If the request has a Realm PARTID, output this bit as the NO_MON flag.

RBNKHP The MAP4SPTO2SP register must only be accessible in the Root physical address space.

3.3.2.2 Space mapping external to an MSC

IBFGQN A two-space Completer can be connected using a small component external to the MSC that implements a programmable four-space to two-space mapping similar to MAP4SPTO2SP:

Table 3-6 Space mapping external to the MSC MAP4SPTO2SP fields

Field bits Field name Description

15 Rt_outPARTID_space If a request has a Root PARTID, the output PARTID uses this bit for MPAM_NS. 14 Rt_NO_MON If the request has a Root PARTID, output this bit as the NO_MON flag. 7 Rl_outPARTID_space If a request has a Realm PARTID, the output PARTID uses this bit for MPAM_NS. 6 Rl_NO_MON If the request has a Realm PARTID, output this bit as the NO_MON flag.

RKFDFD The external mapping register must only be accessible in the Root physical address space.

ILJGXZ If the two-space MPAM MSC does not have any way to accept the NO_MON flag at the request input, the NO_MON flag is not used. Two-space MPAM MSCs are not required to support a NO_MON input.

3.3.2.3 Fixed space mapping at a Completer

RXRQHH The fixed MPAM_SP reduction scheme transforms MPAM_SP into a 1-bit MPAM_NS according to Table 3-7:

Table 3-7 Four-space to two-space static reduction scheme

Four-space MPAM_SP Input Two-space MPAM_NS Output

- 0b00 (Secure PARTID space) 0b0 (Secure PARTID space)

- 0b01 (Non-secure PARTID space) 0b1 (Non-secure PARTID space)


- 0b10 (Root PARTID space) 0b0 (Secure PARTID space)

- 0b11 (Realm PARTID space) 0b1 (Non-secure PARTID space)


- 3.3.3 Non-MPAM components RVKQKB Non-MPAM components do not have the ability to:


- • Make requests with non-zero MPAM information or to use MPAM information when completing requests.
- • Propagate MPAM information to downstream MSCs.


3.3.3.1 Non-MPAM Requesters

IJHPBM Arm strongly recommends that a System MMU is to used to add MPAM information to requests from non-MPAM Requesters, see Arm® System Memory Management Unit Architecture Specification, SMMU architecture version 3 (ARM IHI 0070).

RTTSLQ Requesters attached to an SMMU for RME are only associated with the Secure, Non-secure, and Realm Security states, and therefore use three of the four PARTID spaces.

IJQWCD NoStreamID requesters attached to an SMMU for RME can issue transactions to Root or Realm physical address space. For these accesses it is permitted to use Secure and Non-secure PARTID spaces respectively.

3.3.3.2 Non-MPAM Completers

RRGSJV Completers that have no support for the MPAM information accompanying requests must be interfaced to the system by dropping MPAM information from the requests.

IPSMDZ A non-MPAM Completer limits the topology of MPAM in the system because it does not propagate MPAM information to MPAM components downstream.

- 3.4 Interconnect behavior


###### 3.4 Interconnect behavior

RMFRGP Interconnects connect Requesters to Completers, and they must transport MPAM information from MPAM Requester to Completer.

IXTNQW Interconnects are permitted to support MPAM features, for example priority partitioning, monitoring, or other features.

IKMCQK Some interconnect devices include cache functionality, in which case the cache behavior in Cache behavior applies.

###### 3.5 Cache behavior

IWQVMP The term “data” in this section is intended to indicate the content stored in the cache. It is not intended to indicate any restriction on the applicability of this section based on the purpose of the cache or of its content.

RCYKPC A cache must associate the MPAM information of the request that allocated a cache line with any data stored in the cache line. This stored MPAM information is a property of the data.

IFNZTD The MPAM information on a request to the cache from an upstream Requester is used for the following purposes:

- • Source for the MPAM information associated with data when the data is allocated into the cache and is stored in association with the data while the data resides in the cache.
- • Optionally updating the stored MPAM information of the cached data on a write hit (Write hits may update the MPAM information of a cache line).
- • Providing MPAM information for downstream requests to fulfill the incoming request such as a read from downstream on a cache miss that fetches data into the cache.
- • Optionally (Eviction), providing MPAM information for downstream requests generated by evict or clean operations when this cache is the last level of cache upstream of main memory.
- • Selecting settings of partitioning controls implemented in the cache.
- • Measuring or tracking the resource usage by each partition for a capacity control.
- • Measuring or counting to track filtered resource usage for resource usage monitors, if implemented.
- • Triggering and filtering events triggered by requests from upstream Requesters for MPAM resource monitors, if implemented.


IVVKLR The stored MPAM information is used by MPAM for the following purposes:

- • Providing the MPAM information for downstream requests generated by evict or clean operations, when this cache is not the last level of cache.
- • Optionally (Eviction) providing MPAM information for downstream requests generated by evict or clean operations, when this cache is the last level of cache.
- • Triggering and filtering events triggered by internal and downstream requests for MPAM resource monitors, if implemented.
- • Tracking resource usage by partitions, as needed by a partitioning control implementation.


###### 3.5.1 Eviction

RKPNNZ When a cache line that is not in a last-level cache is evicted to another cache, the evicting cache must produce the MPAM information that is associated with the cache line. A system cache (last-level cache) either:

- • Produces the MPAM information of the request that caused the eviction in its request to a memory-channel controller.
- • Produces the stored MPAM information associated with the evicted line.


###### 3.5.2 Cache partitioningIFJXFK A cache may optionally implement cache-partitioning resource controls, such as a cache-portion partitioning control.

INDXCW It is permitted to use both a cache-portion partitioning control and a cache maximum capacity partitioning control in a cache.

###### 3.5.3 Cache-storage usage monitoringRJXRZM A cache may optionally implement cache-storage usage monitoring.

RRHYKB For a monitored PARTID, the monitor gives the total cache storage used by the partitions in the monitor’s PARTID space and matching filter criteria programmed for the monitor.

Filter criteria used to specify parameters for usage monitoring may include:

- • PARTID.
- • PMG.
- • Other implementation specific criteria.


- 3.5.4 Optional cache behaviors The following cache behaviors are permitted but not required.


3.5.4.1 Write hits may update the MPAM information of a cache line

RLQQJN If a write access, or a write-back from a cache level, associated with a request from an MPAM Requester, updates an entry that is present in the cache, it is IMPLEMENTATION SPECIFIC whether the cache entry:

- • Stays associated with its stored MPAM information.
- • Is updated to be associated with the MPAM information of that write access.


IQQDWP It is possible that a change in the resource partition of the data (without moving the data) leaves the data in a portion of the cache that the new resource partition does not have permission to allocate. This can occur if the Cache Portion Bit Map (CPBM) bit for that portion is not set in the CPBM for the new PARTID. The optional behavior in this subsection does not change the location within the cache, even if the new partition for the data does not have a CPBM bit that allows allocation in this portion of the cache.

RKBJPW A write hit to cached data is permitted but not required to change the portion of the cache capacity allocated to the data, if the resource partition of the cache data is updated due to the write hit, and the portion of capacity where the data currently resides is not in the new resource partition’s cache portion bitmap.

- 3.6 Memory-channel controller behavior


###### 3.6 Memory-channel controller behavior

IWLWCL A memory-channel controller may implement MPAM features. Features used in a memory-channel controller are:

- • Memory-bandwidth minimum and maximum partitioning.
- • Memory-bandwidth portion partitioning.
- • Priority partitioning.
- • Memory-bandwidth usage monitors.


- 3.7 System reset of MPAM controls in MSCs


###### 3.7 System reset of MPAM controls in MSCs

RYBYPZ After a system reset, only the resource control settings for the default PARTID in an MPAM MSC must be reset.

IWSRKH All of the following apply when resetting the default PARTID resource controls in MPAM MSCs:

- • The system behaves as if there is no MPAM.
- • Software has access to all of the default PARTID resource controls.


IYXQZN Because MPAMn_ELx.MPAMEN for the highest implemented ELx is reset to 0 by a system reset, the MPAM fields of all requests issued by a PE use the corresponding default PARTID in the Security state of the PE.

IXHCPX The reset values should allow the default PARTID to access all of the resource. This allows the system to boot up to a point where MPAM resource controls can be set before non-default PARTIDs are used to make requests.

IWCFHP For the default PARTID in a Security state, Table 3-8 describes the suggested resource control reset values.

Table 3-8 Suggested reset values for PARTID0 resource controls

Control type Reset value

MPAMCFG_CPBM<n> All ones for all implemented n MPAMCFG_CMAX.CMAX 0xFFFF MPAMCFG_CMAX.SOFTLIM 0xb0 MPAMCFG_MBW_PBM<n> All ones for all implemented n MPAMCFG_MBW_MAX.MAX 0xFFFF MPAMCFG_MBW_MAX.HARDLIM 0b0 MPAMCFG_MBW_MIN.MIN 0xFFFF MPAMCFG_MBW_PROP.EN 0b0 MPAMCFG_CASSOC.CASSOC 0xFFFF MPAMCFG_EN_FLAGS.EN0 0b1

IMPNPB If PARTID narrowing is implemented, then Arm recommends that reqPARTID == 0 map to intPARTID == 0 and that the reset values be applied to that intPARTID in all Security states. See PARTID narrowing.

### Chapter 4MPAM in MSCs

This chapter contains the following sections:

- • Model of partitioning and monitoring in an MPAM MSC.
- • Resource instance selection.
- • Virtualization support in MPAM MSCs.
- • Four-space MPAM MSCs.
- • Security in MPAM MSCs.
- • Translation sequence for ingress and egress translation regimes in an MPAM MSC.


- 4.1 Model of partitioning and monitoring in an MPAM MSC


###### 4.1 Model of partitioning and monitoring in an MPAM MSC

RCNGYT Capacity-based partitioning of a shared resource requires measuring the usage of that resource by different partitions.

IPKSGL Figure 4-1 shows a general model for capacity-based partitioning of, and monitoring the usage of, a shared resource in an MPAM MSC:

Request

| | | |
|---|---|---|
| |Settings Table<br><br>1|Settings Table<br><br>1|
| | | |


###### PARTID, and MPAM_NS or MPAM_SP

Resource Regulator

4 Setting

2

3

|Resource Allocation 5<br><br>| |
|---|---|
|Resource Allocation 5<br><br>| |


Request Handling Function

Measurement

Figure 4-1 General model of MPAM capacity-based resource partitioning controller

The model measures how much of the shared resource the current partition is using, and the measurements inform adjustment of the partitioning, as follows:

- 1. When an MPAM Requester sends a request, the PARTID space and PARTID in the MPAM information bundle that is attached to the request provide an index into component 1, to select the entry that holds the settings for that partition.
- 2. The settings are then passed to component 4.
- 3. Component 3 measures how much of the shared resource the partition is using, and passes the measurements to component 4.
- 4. Component 4 compares the measurements with the settings.
- 5. The comparison result informs resource allocation.


IQZCVW When designing a system, the designer implements components 1-4 into the MSC.

IWLXGL For portion-based partitioning, measuring the usage of a shared resource is not required, because portions are pre-determined and fixed.

###### 4.2 Resource instance selection

RHHQWY An MPAM MSC can contain one or more resource controls.

RWSXSL Although different resources can have different controls, all resource controls have the following in common:

- • Each resource control uses the PARTID and MPAM_NS or MPAM_SP signal from the incoming request to select the appropriate control parameters for the PARTID space.
- • The selected parameters control the MPAM MSC behavior, either to partition the resources or to control monitoring of resource usage.


IGTYPF See also:

- • Model of partitioning and monitoring in an MPAM MSC.
- • Resource Partitioning Controls.


IPGSXL Resource instance selection (RIS) allows an MPAM MSC to support multiple resources, including multiple resources

using the same resource type or partitioning control.

RYQBMW When RIS is implemented, an MPAM MSC must implement independent resource controls and two or more resources of the same type.

IQTQCS In MPAM v0.1 and from MPAM v1.1, RIS is supported when MPAMF_IDR.HAS_RIS is 1.

###### 4.2.1 RIS values

IDQNHJ For a resource in an MPAM MSC with multiple resources or a resource that can be monitored by an MPAM resource usage monitor, all of the following apply:

- • The MPAMCFG_PART_SEL.RIS field is used to select the resource instance described by MPAMF_IDR.
- • MPAMCFG_PART_SEL.RIS is used with MPAMCFG_PART_SEL.PART_SEL to select the resource and PARTID when accessing MPAMCFG_* resource control registers.
- • MSMON_CFG_MON_SEL.RIS is used to select the MPAM resource monitors associated with a resource.


RYXZFG RIS values are IMPLEMENTATION DEFINED. All resources in an MPAM MSC must have different RIS values.

IMRCWC For an MPAM MSC, the largest RIS value is defined by MPAMF_IDR.RIS_MAX.

RDQNFB Any RIS value from 0 to MPAMF_IDR.RIS_MAX can be assigned to a partitioned or monitored resource. There is no requirement that every RIS value be assigned to a partitioned or monitored resource.

IHFXLK If MPAMv1.0 is implemented, then MPAMCFG_PART_SEL.RIS is RES0. The only resource that can be identified and

controlled by software is RIS 0.

RYGZYL If a partitioning control applies to all resource instances in an MPAM MSC, then this common control must be associated with MPAMCFG_PART_SEL.RIS set to 0.

RPLZWM If there is only a single resource instance in an MPAM MSC, all partitioning controls must be associated with MPAMCFG_PART_SEL.RIS set to 0.

IYMFDQ If an MPAMCFG_* resource control register is accessed when MPAMCFG_PART_SEL.RIS is set to a resource instance that does not support the resource control, then the behavior is CONSTRAINED UNPREDICTABLE. See error code 9, RIS in MPAMCFG_PART_SEL.RIS does not have partitioning control, in Table 7-1.

###### 4.2.2 Effects of MPAMCFG_PART_SEL.RIS on values read from other registers

IQZWKF Register fields that control the capabilities of the resource instance selected by MPAMCFG_PART_SEL.RIS might have different values for different resource instances.

ILQCQH The effects of MPAMCFG_PART_SEL.RIS on the MPAM identification registers are shown in Table 4-1.

Table 4-1 MPAM ID register fields affected by a resource instance

Register Field Affected by MPAMCFG_PART_SEL.RIS

MPAMF_CCAP_IDR CMAX_WD This field is permitted to vary between resource instances. HAS_CMAX_SOFTLIM This field is permitted to vary between resource instances. NO_CMAX This field is permitted to vary between resource instances. HAS_CMIN This field is permitted to vary between resource instances. HAS_CASSOC This field is permitted to vary between resource instances. CASSOC_WD This field is permitted to vary between resource instances.

MPAMF_CPOR_IDR CPBM_WD This field is permitted to vary between resource instances. MPAMF_CSUMON_IDR HAS_CAPTURE This field is permitted to vary between resource instances.

CSU_RO This field is permitted to vary between resource instances. NUM_MON This field is permitted to vary between resource instances. HAS_XCL This field is permitted to vary between resource instances. HAS_CEVNT_OFLW This field is permitted to vary between resource instances. HAS_CEVNT_CAPT This field is permitted to vary between resource instances.

MPAMF_IDR NO_IMPL_MSMON MPAMF_IMPL_IDR describes no resource usage monitors. NO_IMPL_PART MPAMF_IMPL_IDR describes no resource partitioning controls. HAS_MSMON The resource usage monitors described in MPAMF_MSMON_IDR,

otherwise this field is 0b0.

HAS_IMPL_IDR The IMPLEMENTATION DEFINED features described in

MPAMF_IMPL_IDR, otherwise this field is 0b0. HAS_PRI_PART The priority partitioning described in MPAMF_PRI_IDR, otherwise 0b0. HAS_MBW_PART The memory bandwidth partitioning described in MPAMF_MBW_IDR, otherwise 0b0. HAS_CPOR_PART The cache portion partitioning described in MPAMF_CPOR_IDR, otherwise 0b0. HAS_CCAP_PART The cache capacity partitioning described in MPAMF_CCAP_IDR, otherwise 0b0. MPAMF_IMPL_IDR IMPLFEAT The IMPLFEAT contents vary according to the resource instance selected, and cannot be specified by the architecture. MPAMF_MSMON_IDR MSMON_MBWU The memory bandwidth usage monitors of the resource, otherwise this field is 0b0. MSMON_CSU The cache storage usage monitors of the selected resource instance. Otherwise this field is 0b0. MPAMF_PRI_IDR a DSPRI_WD The downstream priority width. Ignored if MPAMF_PRI_IDR.HAS_DSPRI is set to 0. DSPRI_0_IS_LOW The downstream priority encoded with 0 being the low priority. Ignored if MPAMF_PRI_IDR.HAS_DSPRI is set to 0.

Register Field Affected by MPAMCFG_PART_SEL.RIS

HAS_DSPRI The downstream priority control. INTPRI_WD The internal priority width. Ignored if

MPAMF_PRI_IDR.HAS_INTPRI is set to 0. INTPRI_0_IS_LOW The internal priority encoded with 0 being low priority. Ignored if

MPAMF_PRI_IDR.HAS_INTPRI is set to 0. HAS_INTPRI The internal priority control.

MPAMF_MBW_IDR BWPBM_WD This field is permitted to vary between resource instances. HAS_PROP This field is permitted to vary between resource instances. HAS_PBM This field is permitted to vary between resource instances. HAS_MAX This field is permitted to vary between resource instances. HAS_MIN This field is permitted to vary between resource instances. HAS_MAX This field is permitted to vary between resource instances. MAX_LIM This field is permitted to vary between resource instances.

MPAMF_MBWUMON_IDR HAS_CAPTURE This field is permitted to vary between resource instances. HAS_RWBW This field is permitted to vary between resource instances. HAS_LONG This field is permitted to vary between resource instances. LWD This field is permitted to vary between resource instances. SCALE This field is permitted to vary between resource instances. NUM_MON This field is permitted to vary between resource instances. HAS_OFLOW_LNKG This field is permitted to vary between resource instances. HAS_OFSR This field is permitted to vary between resource instances. HAS_CEVNT_OFLW This field is permitted to vary between resource instances. HAS_CEVNT_CAPT This field is permitted to vary between resource instances.

a. If the priority partitioning is local to the resource instance, then all fields might vary between resource instances. If the priority partitioning operates at the MPAM MSC level, then MPAMF_PRI_IDR should be non-zero only when RIS is 0.

IPKLPB The following MPAM resource configuration registers access the resource control settings selected by MPAMCFG_PART_SEL.RIS:

- • MPAMCFG_CASSOC.
- • MPAMCFG_CASSOC.
- • MPAMCFG_CMIN.
- • MPAMCFG_CPBM<n>.
- • MPAMCFG_MBW_MAX.
- • MPAMCFG_MBW_MIN.
- • MPAMCFG_MBW_PBM<n>.
- • MPAMCFG_MBW_PROP.
- • MPAMCFG_PRI.


IWKDDZ The following control settings are global to all resources in the MPAM MSC:

- • MPAMCFG_DIS.
- • MPAMCFG_EN.
- • MPAMCFG_EN_FLAGS.
- • MPAMCFG_INTPARTID.
- • MPAMCFG_MBW_WINWD.
- • MPAMCFG_PART_SEL.


IKMLZK The following registers are not affected by MPAMCFG_PART_SEL.RIS:

- • MPAMF_AIDR.
- • MPAMF_ECR.
- • MPAMF_ESR.
- • MPAMF_IIDR.
- • MPAMF_PARTID_NRW_IDR.
- • MPAMF_SIDR.
- • MPAMCFG_PART_SEL.
- • MPAMF_ERR_MSI_ADDR_H.
- • MPAMF_ERR_MSI_ADDR_L.
- • MPAMF_ERR_MSI_ATTR.
- • MPAMF_ERR_MSI_DATA.
- • MPAMF_ERR_MSI_MPAM.


###### 4.2.3 RIS controls in MSMON_CFG_MON_SEL

IWZFQS The following MPAM monitor configuration, capture, and value registers are selected by setting MSMON_CFG_MON_SEL.RIS to the resource associated with the monitor:

- • The MSMON_CFG_* monitor configuration registers.
- • The MSMON_* monitor and monitor capture registers.
- • MSMON_CSU_OFSR and MSMON_MBWU_OFSR overflow status registers.


RDNNTB To select the monitors for a particular resource instance, the value of MSMON_CFG_MON_SEL.RIS must be set to that particular instance.

RLXXDB If a monitor is not associated with any resource or with the MPAM MSC, then that monitor must be associated with MPAMCFG_PART_SEL.RIS set to 0.

INTXDV The following registers re not affected by MPAMCFG_PART_SEL.RIS:

- • MSMON_OFLOW_MSI_ADDR_H.
- • MSMON_OFLOW_MSI_ADDR_L.
- • MSMON_OFLOW_MSI_ATTR.
- • MSMON_OFLOW_MSI_DATA.
- • MSMON_OFLOW_MSI_MPAM.
- • MSMON_OFLOW_SR.
- • MSMON_CAPT_EVNT.


###### 4.2.4 Undefined RIS valuesRMRZLG All of the following are permitted in an MPAM MSC implementation:

- • Fewer RIS bits can be implemented than the architecture defines, but at least enough bits to represent MPAMF_IDR.RIS_MAX must be implemented.
- • RIS values that are within the range of 0 to MPAMF_IDR.RIS_MAX can represent undefined resources.
- • When selecting a resource instance, use only the implemented bits to decode a RIS value.


RNPVVN If one or more of the following apply to the value of MPAMCFG_PART_SEL.RIS or MSMON_CFG_MON_SEL.RIS, then the selected resource is undefined:

- • The value is greater than MPAMF_IDR.RIS_MAX.
- • The value does not correspond to a resource implemented in the MPAM MSC.
- • The value corresponds to an implemented resource, but the resource does not implement any control or monitor corresponding to the selected RIS.


IKVSWT See the following for information on the behavior caused by using an undefined RIS value:

- • Error code 8, undefined RIS in MPAMCFG_PART_SEL.RIS, in Table 7-1.


- • Error code 9, RIS in MPAMCFG_PART_SEL.RIS does not have partitioning control, in Table 7-1.
- • Error code 10, undefined RIS in MSMON_CFG_MON_SEL.RIS, in Table 7-1.
- • Error code 11, RIS selected by MSMON_CFG_MON_SEL.RIS does not have monitor type, in Table 7-1.


RHSFKH If an MPAMF ID register is accessed when MPAMCFG_PART_SEL.RIS is an undefined value, all of the following apply:

- • All HAS_* fields in that MPAMF ID register must read as 0.
- • No error interrupt is generated and no error is recorded in MPAMF_ESR.


- 4.3 Virtualization support in MPAM MSCs


###### 4.3 Virtualization support in MPAM MSCs

RXRHKL An MPAM MSC only supports physical PARTIDs and does not support virtual PARTIDs. Any virtual PARTID generated by an MPAM Requester is resolved into a physical PARTID before it is communicated with the memory system request.

IGDWTC Accesses from a guest to all MPAM MSC configuration registers, and to the System registers that configure the MPAM PE MSCs, can be emulated by the host hypervisor. This allows virtual to physical PARTID mapping to be emulated and hypervisor policies governing resource partitioning to be applied.

###### Note

It is expected that configuration and reconfiguration of MPAM MSC control settings will be infrequent.

IQLFPS Arm recommends that the memory-mapped MPAM MSC configuration registers are located at a 64-KB-aligned address to permit an access trap on that page in the stage 2 translation tables. The stage 2 access traps are taken to EL2 where the hypervisor can emulate the access. For more information on recommended configurations of memory-mapped registers of an MPAM MSC, see MPAM feature page.

- 4.4 Four-space MPAM MSCs


###### 4.4 Four-space MPAM MSCs

RGDMQZ An MPAM MSC with RME must support four PARTID spaces and four PA spaces.

IZZKKM In an MSC with RME, the MPAMF_IDR.SP4 bit must be 1 when read from any PA space and, if RIS is supported, with any MPAMCFG_PART_SEL.RIS value.

RMMXWZ In an MSC with RME, MPAMF_BASE_ns, MPAMF_BASE_s, MPAMF_BASE_rl, MPAMF_BASE_rl must all be defined in the MSC firmware data. See MPAM feature page.

RQPGPM For each PA space, the MPAM memory-mapped registers are at offsets from the MPAM feature page base address in that PA space, as shown in Table 4-2.

Table 4-2 MPAM feature page base address for each PA space

MPAM feature page base address Description

PA space

Non-Secure MPAMF_BASE_ns Base address of the MPAM feature page in the Non-secure PA space. Secure MPAMF_BASE_s Base address of the MPAM feature page in the Secure PA space. Realm MPAMF_BASE_rl Base address of the MPAM feature page in the Realm PA space. Root MPAMF_BASE_rt Base address of the MPAM feature page in the Root PA space.

IZNKGL For each PA space, all of the following apply:

- • The offsets of MPAM memory-mapped registers from the MPAM feature page base address are the same. See Table 9-8.
- • The required minimum set of MPAM registers accessible from the MPAM feature page are described in Minimum required MPAM memory-mapped registers.
- • The MPAM MSC features are described in MPAMF_IDR. This register indicates which additional feature ID registers are supported.
- • If MPAMF_*IDR.HAS_* is 1, then the corresponding MPAMCFG_* registers must be supported.
- • If the PARTID space has resource control registers or resource instances that are accessed in that PA space, then MPAMCFG_PART_SEL must be supported.
- • If the ID registers indicate that a monitor is supported, then the corresponding MSMON_* register must be supported.
- • If any monitor registers are supported, then MSMON_CFG_MON_SEL must be supported.
- • If MPAMF_IDR.HAS_ESR is 1, then MPAMF_ESR and MPAMF_ECR must be supported.


RQFCTM Support for the resource controls and monitors in one PA space are not required in another PA space.

- 4.5 Security in MPAM MSCs


###### 4.5 Security in MPAM MSCs

IFCTPR The request PARTID, PMG, and MPAM_NS or MPAM_SP sent to an MPAM MSC determine which partitioning control settings are used.

IJSQKV If the request PARTID, PMG, and MPAM_NS or MPAM_SP match those configured in a performance monitor, a monitoring event is triggered.

IYYFYN For each implemented resource control, the request PARTID, and MPAM_NS or MPAM_SP select the partitioning configuration from a table of PARTID configurations. MPAM_NS or MPAM_SP select the configuration table for the Security state. The configuration tables are not required to be power-of-two sized and are permitted to be different sizes.

ICKSSY The following example describes the behavior of an MPAM MSC that receives a request from an MPAM PE executing in Secure state:

- • When the PE sends a request PARTID to the MPAM MSC, MPAM_NS is 0b0 or MPAM_SP is 0b00, indicating the request PARTID is in the Secure PARTID space. This is true even when the access is from Secure state to the Non-secure address space.
- • When the MSC receives a request PARTID and MPAM_NS is 0b0 or MPAM_SP is 0b00, control settings for the Secure PARTID are used.


- 4.6 Translation sequence for ingress and egress translation regimes in an MPAM MSC


###### 4.6 Translation sequence for ingress and egress translation regimes in an MPAM MSC

ITHJQW The translation sequence for ingress translation regimes is as follows:

- • The MPAM MSC first applies ingress translation if ingress translation is enabled and it is capable of doing so. If the MPAM MSC is configured for ingress translation (MPAMF_IDR.HAS_IN_TL == 1 && MPAMCFG_IN_TL.ENABLE== 1), it performs a direct translation of the incoming PARTID if capable and configured to do so in MPAMF_IN_TL_IDR.HAS_DIRECT_TL.
- • If a direct translation for the incoming PARTID is found, as configured by software by setting MPAMCFG_PART_SEL.INGRESS_TL = 1, and configuring the translation by having set the incoming PARTID in MPAMCFG_PART_SEL.PARTID_SEL register and the corresponding translation in MPAMCFG_IN_TL.PARTID_TL, then the incoming PARTID is replaced by its translated value as configured in MPAMCFG_IN_TL.PARTID_TL.
- • Otherwise, if a direct translation for the incoming PARTID is not found, or if the MSC isn’t capable of direct translation of incoming PARTIDs, BASE and MASK are applied if the MSC is capable and configured to do so by MPAMF_IN_TL_IDR.HAS_BASE_MASK according to the algorithm below:

— MASK = 2MPAMCFG_IN_TL_MASK.MASK_WD -1. MSB_BITS = ~MASK & MPAMCFG_IN_TL_BASE.BASE.

— TRUNCATED_PARTID = MASK & PARTID. TRANSLATED_PARTID = MSB_BITS | TRUNCATED_PARTID.

- • If no direct translation is supported, enabled, or configured for the incoming PARTID, or if no BASE and MASK translation is supported or enabled, the incoming PARTID is propagated unchanged.


IYKXYG After applying any configured control policy to the incoming PARTID, or translated PARTID if ingress translation applies, the translation sequence for egress translation regimes is as follows:

- • If the MPAM MSC is configured for egress translation (MPAMF_IDR.HAS_OUT_TL == 1 && MPAMCFG_OUT_TL.ENABLE == 1), it performs a direct translation of the outgoing PARTID if capable and configured to do so in MPAMF_OUT_TL_IDR.HAS_DIRECT_TL.
- • If a direct translation for the incoming PARTID is found, as configured by software by setting MPAMCFG_PART_SEL.INGRESS_TL = 0, and configuring the translation by having set the incoming PARTID in MPAMCFG_PART_SEL.PARTID_SEL register and the corresponding translation in MPAMCFG_OUT_TL.PARTID_TL, then the outgoing PARTID is replaced by its translated value as configured in MPAMCFG_OUT_TL.PARTID_TL.
- • Otherwise, if a direct translation for the outgoing PARTID is not found, or if the MSC isn’t capable of direct translation of outgoing PARTIDs, BASE and MASK are applied if the MSC is capable and configured to do so by MPAMF_OUT_TL_IDR.HAS_BASE_MASK according to the algorithm below:

— MASK = 2MPAMCFG_OUT_TL_MASK.MASK_WD -1. MSB_BITS = ~MASK & MPAMCFG_OUT_TL_BASE.BASE.

— TRUNCATED_PARTID = MASK & PARTID. TRANSLATED_PARTID = MSB_BITS | TRUNCATED_PARTID.

- • If no direct translation is supported, enabled, or configured for the outgoing PARTID, or if no BASE and MASK translation is supported or enabled, the outgoing PARTID is propagated unchanged.


### Chapter 5Resource Partitioning Controls

This chapter contains the following sections:

- • Introduction.
- • Standard partitioning control interfaces.
- • IMPLEMENTATION DEFINED partitioning control interfaces.
- • PARTID narrowing.
- • MPAM Domains PARTID translation.


- 5.1 Introduction


###### 5.1 Introduction

IKTMKJ An MSC receives a PARTID with each request, and resource controls determine how that request PARTID uses MSC resources.

IYVQQQ All memory system requests with a specific PARTID share the resource control settings for that partition.

RCZMFT If all of the following apply, then an MSC resource can be partitioned:

- • The MSC supports MPAM on the upstream interface.
- • The MSC has one or more MPAM resource controls for that resource.


IMJSTX A partitionable resource can be partially allocated to a partition according to the programming of the MPAM resource control or controls for that resource.

ICFFDR Implementations can, but are not required to, use the resource partitioning controls defined by the architecture.

Implementations are permitted to define IMPLEMENTATION SPECIFIC resource partitioning controls.

IHBRDM An IMPLEMENTATION SPECIFIC resource control can use a PARTID to either control resources not supported by the

standard controls or that implement unique control methods that cannot be mapped onto the standard control interfaces.

ISXNKG An MPAM MSC can optionally support configuring a default control setting for its resource controls which applies if either of the two conditions apply:

- • The MPAM MSC receives an MPAM PARTID outside the range it supports.
- • The MPAM MSC receives an MPAM PARTID which its resource controls have not been explicitly configured for.


See MPAMF_IDR.HAS_DEFAULT_PARTID.

RXDZGT If an MSC supports the MPAM RIS feature, the MSC can have two or more partitionable resources selected by MPAMCFG_PART_SEL.RIS. For more information, see Resource instance selection.

###### 5.2 Standard partitioning control interfaces

RHLHYS An MSC can implement one or more of the following OPTIONAL standard partitioning control interfaces for memory

system resources:

- • Cache portion partitioning.
- • Cache minimum capacity resource control.
- • Cache associativity partitioning.
- • Cache maximum capacity partitioning.
- • Memory bandwidth portion partitioning.
- • Memory bandwidth minimum and maximum partitioning.
- • Memory bandwidth proportional stride partitioning.
- • Priority partitioning.


RGVCBB The presence of each standard partitioning control interface is indicated by a bit in MPAMF_IDR or in a resource specific memory-mapped ID register. See Memory-mapped ID register description.

RKWGHQ A portion has all of the following properties:

- • It is a uniquely identifiable part of a resource.
- • It has a fixed size or capacity.
- • All portions of a resource are the same size.
- • A particular resource has a constant number of portions.
- • Every partition that is given access to a portion n shares access to portion n.


IFMDCY The standard partitioning control interfaces are configured using a combination of the following in the appropriate address space:

- • The controlling MSC determined by the base address of the MPAM feature page.
- • The PARTID.
- • The PARTID space.
- • If MPAMF_IDR.HAS_RIS is 1, the resource selected by MPAMCFG_PART_SEL.RIS.


###### Note

Software must ensure mutual exclusion for access to MPAMCFG_* registers of each MSC.

- 5.2.1 Disabling a PARTID IWWWHC A PARTID is enabled by writing the PARTID value to MPAMCFG_EN.PARTID.


IFJDHJ A PARTID is disabled by writing the PARTID value to MPAMCFG_DIS.PARTID.

IDGWPN The enabled or disabled PARTID is in the PARTID space that corresponds to the MPAMF_BASE page instance used to access the MPAMCFG_EN or MPAMCFG_DIS register.

RTZWJG The enabled status of a PARTID within a PARTID space is global to the MSC, and applies to all the other partitioning controls in that MSC.

###### Note

This functionality allows resources used by a PARTID to be rapidly reclaimed if software using the PARTID is exited or a system-wide mode change occurs.

RVFJMZ A disabled PARTID behaves as if it is programmed to allocate no resource or allocate resource with the lowest priority.

IKPNCG When a PARTID is disabled, the value of MPAMCFG_DIS.NFU determines the following:

- • When 0, hardware is required to preserve the PARTID settings so that those settings are used when that PARTID is re-enabled.


- • When 1, hardware is permitted to, but not required to, discard the PARTID settings so that new settings can be configured before that PARTID is re-enabled.


IPYHHS A block of 32 PARTIDs can be enabled or disabled using the enable flags in MPAMCFG_EN_FLAGS, as follows:

- • MPAMCFG_EN_FLAGS.EN0 selects (MPAMCFG_PART_SEL.PARTID_SEL & 0xFFE0) + 0. • MPAMCFG_EN_FLAGS.EN1 selects (MPAMCFG_PART_SEL.PARTID_SEL & 0xFFE0) + 1. • . . .
- • MPAMCFG_EN_FLAGS.EN31 selects (MPAMCFG_PART_SEL.PARTID_SEL & 0xFFE0) + 31.


RWFQFS When a PARTID is disabled by writing to MPAMCFG_EN_FLAGS, hardware is required to preserve the PARTID settings so that those settings are used when that PARTID is re-enabled.

RGMCWW If RIS is enabled in the MSC, enabling and disabling a PARTID affects all resource instances in that PARTID. See Resource instance selection.

RCSLHH The enabled status of a PARTID controls the behavior of the request PARTID, even when PARTID narrowing is implemented.

5.2.1.1 Enabled and disabled behavior of resource controls

RQFRFQ The behavior of an enabled resource control is the normal behavior of that control using the programmed control settings.

RRNSLZ The behavior of a disabled resource control depends on the type of control, as follows:

- • A fractional control behaves the same as if the control were set to zero.
- • A portion-based partitioning control behaves as if the control were set to allocate no portions.
- • In a cache, a disabled PARTID has the second lowest allocation priority. That priority is higher than the allocation priority of an empty cache line, indicating it is likely to be replaced.
- • When PARTID narrowing is implemented, the resource control settings could be shared by multiple request PARTIDs. Therefore, disabling a PARTID cannot make the resource control for a shared internal PARTID act as if it has no resource. Instead the disabled PARTID must act as if the request PARTID is temporarily mapped to an internal PARTID that has no resources assigned. This means that MPAMCFG_DIS.NFU must not be implemented in an MSC that has PARTID narrowing. See also PARTID narrowing.


RNJXHP For resource controls, only the control for PARTID 0 must be reset, and that PARTID must be reset to enabled.

###### Note

Firmware must initialize all PARTIDs in all PARTID spaces to be enabled at system boot before transferring to software that might not support this capability.

- 5.2.2 Cache portion partitioning RXPPGG Caches can be partitioned into portions, and those portions can be allocated to partitions.


RZVTKX When a cache request requires allocation of a cache line, the PARTID of that request determines which portions of the cache can allocate that line.

IGKHBQ Cache lookups are not affected by partitioning. If a valid cache line exists it must be returned by a cache lookup even if that line was allocated with a different PARTID.

RRLHRV Cache portion partitioning uses the generic portion control interface described in Portion resource controls.

IJNZCL Cache portion partitioning can be used with cache maximum capacity partitioning. See Using cache maximum capacity partitioning with Cache portion partitioning.

5.2.2.1 Cache portion bit map

RZRSRZ Each bit of a cache portion bitmap (CPBM) controls whether a partition is permitted to allocate lines in a particular

cache portion, as follows:

- • If the bit is 0, then the partition cannot allocate lines in the corresponding cache portion.
- • If the bit is 1, then the partition can allocate lines in the corresponding cache portion.


IGRLDM The number of bits in the CPBM is an IMPLEMENTATION DEFINED parameter that is discoverable in

MPAMF_CPOR_IDR.CPBM_WD.

RWMPLL CPBM is an instance of the generic portion bitmap (PBM) described in Portion resource controls.

- 5.2.2.2 Capacity portion over-allocation

ILTFRF Because the CPBM bits control whether cache portions are allocable, up to the maximum number that are allocable, cache portions cannot be over-allocated.

- 5.2.2.3 Updating the CPBM


IJKCLR Because the CPBM only affects new allocations and does not reallocate previously allocated cache storage, the CPBM can be updated without disrupting system operation.

RLPLMM If a a cache portion allocates a cache line as permitted by a CPBM, and the CPBM is changed such that the cache portion

is not permitted to allocate cache lines, the partition ends up using more cache capacity than permitted by the new CPBM. If a previously allocated cache line is in a portion that is no longer allocable by the new CPBM, one of the following applies:

- • If that cache line is not accessed again, it will eventually be reallocated to a portion that is permitted by the CPBM to allocate cache lines. This represents a temporary misallocation of capacity.
- • If that cache line continues to be accessed, it can represent a long-term misallocation of capacity. The cache line allocated to the portion might be updated, see Write hits that update the resource partition of a cache line may move that line to a different portion.


###### 5.2.3 Cache minimum capacity resource control

IWSXVX Cache minimum capacity resource control is a memory bandwidth minimum control that gives priority to a portion of the cache capacity a PARTID can use. If this control and a cache maximum capacity partitioning control are both implemented, then they can be utilized together.

IWNCYT The MPAMCFG_CMIN.CMIN field specifies the minimum cache capacity, using a 16-bit fixed-point fraction format. See The fixed-point fractional format. A PARTID that uses less than the minimum cache capacity has an elevated allocation priority.

IPTQHV The implemented bit width of MPAMCFG_CMIN.CMIN is determined by MPAMF_CCAP_IDR.CMAX_WD. Bits [15:16-CMAX_WD] are always implemented. If both MPAMCFG_CMAX and MPAMCFG_CMIN are implemented, then the implemented most significant fractional bits of both registers are the same, determined by MPAMF_CCAP_IDR.CMAX_WD.

###### 5.2.4 Cache associativity partitioning

RBDRCM Cache associativity partitioning (CASSOC) sets the amount of associativity that the PARTID can use within any unit of

associativity, as follows:

- • In a set associative cache, CASSOC sets the maximum fraction of the cache ways that a PARTID can allocate within any cache set.
- • In a fully associative cache, CASSOC sets the maximum fraction of all cache entries that the PARTID can allocate.


IXBLKD The MPAMCFG_CASSOC.CASSOC field specifies the cache maximum associativity control, using a 16-bit fixed-point fraction format. See The fixed-point fractional format.

IHWQCV The implemented bit width of MPAMCFG_CASSOC.CASSOC is determined by MPAMF_CCAP_IDR.CASSOC_WD. The implemented bits of the MPAMCFG_CASSOC.CASSOC field are always the most-significant bits.

- 5.2.5 Cache maximum capacity partitioning IMXCHP A limit can be set on the maximum cache capacity that can be allocated by a memory system partition.


RZHBXH Cache maximum capacity partitioning uses the generic maximum-usage control interface described in Maximum usage resource controls.

IPWXZQ The MPAMCFG_CMAX.CMAX field specifies the maximum cache capacity, using a 16-bit unsigned fixed-point fraction format. See The fixed-point fractional format.

RSWNLN The MPAMCFG_CMAX.CMAX setting is a hard limit on the cache capacity. When a PARTID request requires allocation in the cache that is above the capacity set by MPAMCFG_CMAX.CMAX, that capacity cannot be increased even if there is unused capacity.

IFVHPK To set a soft limit on the cache capacity, see Cache maximum capacity control soft limit.

IDGYXL The implemented bit width of MPAMCFG_CMAX.CMAX is determined by MPAMF_CCAP_IDR.CMAX_WD. Bits [15:16-CMAX_WD] are always implemented. If both MPAMCFG_CMAX and MPAMCFG_CMIN are implemented, then the implemented most significant fractional bits of both registers are the same, determined by MPAMF_CCAP_IDR.CMAX_WD.

5.2.5.1 Cache maximum capacity control soft limit

IZDRFW If MPAMF_CCAP_IDR.HAS_CMAX_SOFTLIM is 1, then the MPAMCFG_CMAX.SOFTLIM field is implemented.

IGJPTW When MPAMCFG_CMAX.SOFTLIM is 0, the PARTID is prevented from allocating more cache capacity than permitted by MPAMCFG_CMAX.CMAX.

RVZBFH If a cache line is unallocated or used by a disabled PARTID, then when MPAMCFG_CMAX.SOFTLIM is 1, the PARTID can allocate that line even when doing so exceeds the cache capacity permitted by MPAMCFG_CMAX.CMAX.

IGNKVT For compatibility with software that does not support MPAMCFG_CMAX.SOFTLIM, firmware must reset MPAMCFG_CMAX.SOFTLIM to 0 for all PARTIDs in all PARTID spaces before transferring to that software.

- 5.2.5.2 Using cache maximum capacity partitioning with cache portion partitioning


IMVKRC When Cache portion partitioning is used with cache maximum capacity partitioning, all of the following apply:

- • Cache portion partitioning controls which portions of the capacity can be allocated to a partition.
- • Cache maximum capacity partitioning limits the amount of each portion that can be allocated to a partition.


Example 5-1 Using cache maximum capacity partitioning with cache portion partitioning

This example assumes all of the following:

- • A 1 MB cache is divided into 32 portions.
- • A PARTID has four portions assigned in the CPBM that can share allocation with other partitions.
- • The PARTID is permitted to use no more than 50% of its shared allocation. The controls for cache portion partitioning and cache maximum capacity partitioning are determined as follows:
- • Cache portion partitioning of the 1 MB cache:

— The 1 MB cache is evenly divided into 32 portions, where each portion is 32 KB (1 MB / 32). It is not possible to allocate anything other than an integral number of portions to a PARTID.

— A cache portion can be exclusively allocated to a PARTID or it can be shared by more than one PARTIDs.

— The CPBM assigns four cache portions to the PARTID, which are shared with other partitions.

- • Cache maximum capacity control of the PARTID with four portions: The four portions allocated to the PARTID have a total capacity of 128 KB (32 KB * 4).


— Cache maximum capacity control is used to limit the PARTID to use only 50% of the storage in the portions allocated to it.

— 50% of the storage in the portions allocated to the partition is 6.25% (64 KB / 1MB). In the fixed-point fractional representation, 6.25% is 0x0FFF.

— MPAMCFG_CMAX.CMAX is set to 0x0FFF.

- 5.2.5.3 Capacity over-allocation


IBBBCB The cache capacity control does not guarantee a minimum cache capacity, only a maximum limit to cache capacity. It is possible for cache capacity to be over-allocated because the sum of the cache capacity controls might exceed 100% of the cache size. For this reason, the data of inactive partitions might be evicted from the cache due to the activity of other partitions.

###### 5.2.6 Cache allocation priority

RCXZDR Table 5-1 shows how cache line allocation priority is assigned to a request PARTID using MPAMCFG_CMIN.CMIN and MPAMCFG_CMAX.CMAX. A request PARTID is not assigned a priority of 0.

Table 5-1 Cache line allocation priorities

Cache line occupant Priority Description

Unallocated 0 - lowest Always allocable Disabled PARTID 1 Line occupied by disabled PARTID PARTID with capacity over CMAX

- 2 Line occupied by PARTID greater than maximum

PARTID between CMAX and CMIN, inclusive

- 3 Line occupied by PARTID between maximum and minimum, inclusive


PARTID under CMIN 4 - highest Line occupied by PARTID less than minimum

RDPZKC When a PARTID makes a cache line allocation request, an allocation candidate is selected using following steps:

- 1. All implemented MPAM cache partitioning controls are applied:


- • If MPAMCFG_CPBM<n> is implemented, then only candidates that have the MPAMCFG_CPBM<n> bit set are included.
- • If MPAMCFG_CMAX is implemented and the PARTID currently occupies at least MPAMCFG_CMAX.CMAX fraction of the cache capacity, then the choices are limited to one of the following:


- — If MPAMCFG_CMAX.SOFTLIM is 0, then the partition is not permitted to increase its cache capacity usage beyond CMAX and candidates are selected from lines currently occupied by the request PARTID.
- — If MPAMCFG_CMAX.SOFTLIM is 1, then the partition is permitted to allocate capacity beyond CMAX, but only from invalid lines or lines belonging to disabled PARTIDs.


- • If cache associativity partitioning is implemented and the request PARTID currently occupies at least the MPAMCFG_CASSOC.CASSOC fraction of the associativity unit that the request addresses, then candidates are selected from lines currently occupied by the request PARTID. See Cache associativity partitioning.


- 2. The request PARTID allocation priority is compared to the PARTID allocation priority of the remaining candidates and those candidates that are occupied by a higher priority PARTID than the request PARTID are removed as candidates. See Table 5-1.
- 3. If no replacement candidates remain, no line is allocated.
- 4. If there are remaining candidates with the same or lower priority than the request PARTID, then the implemented cache replacement algorithm is used to select between those remaining candidates.


- 5.2.7 Memory bandwidth portion partitioning RDKYDB The downstream bandwidth from an MSC can be divided into portions, and those portions can be allocated to partitions.

IBMGYP A time-division multiplexing method that allocates traffic to time slots is an example of a bandwidth allocation system that has portions.

RGYMDB Each bit of a bandwidth portion bitmap (BWPBM) controls whether a partition is permitted to allocate bandwidth in a

particular bandwidth portion, as follows:

- • If the bit is 0, then the partition is not permitted to allocate bandwidth within the corresponding bandwidth portion.
- • If the bit is 1, then the partition is permitted to allocate bandwidth within the corresponding bandwidth portion.


IYXVPC The number of bits in the BWPBM is an IMPLEMENTATION DEFINED parameter that is discoverable in

MPAMF_MBW_IDR.BWPBM_WD.

RCFHQL BWPBM is an instance of the generic portion bitmap (PBM) described in Portion resource controls.

IKLJWX When using memory bandwidth portion partitioning, the bandwidth allocated to a PARTID is (N_BITS/N_POR)*B_TOT, where:

- • N_BITS is the number of set bits per PARTID in the bitmap
- • N_POR is the width of the bitmap
- • B_TOT is the total available bandwidth on the controlled resource. The granularity of control for memory-bandwidth portion partitioning is (1/N_POR)*B_TOT.


- 5.2.8 Memory bandwidth minimum and maximum partitioning RHVRZS An MSC can implement any of the following OPTIONAL methods of partitioning bandwidth use:


- • Minimum bandwidth limit available to the PARTID, even in the presence of bandwidth contention.
- • Maximum bandwidth limit available to the PARTID, either the strictly enforced HARDLIM, or SOFTLIM in the presence of contention. MPAMF_MBW_IDR.MAXLIM reports whether a soft limit or a hard limit, or both, is implemented.


IGXQPT The minimum and maximum bandwidth partitioning methods rely on tracking bandwidth use by PARTIDs. Bandwidth measurements have a dependence on time, and that dependence is referred to as the accounting window or accounting period. The accounting window is permitted to be either a moving window, or by resetting bandwidth counts at the beginning of each accounting period. The width of the window is discoverable and can be read from MPAMCFG_MBW_WINWD for the PARTID selected by MPAMCFG_PART_SEL. See Memory bandwidth allocation accounting window width.

IPMWGG In implementations that support a configurable window width per PARTID, MPAMCFG_MBW_WINWD can be written with a fixed-point format, specifying the accounting window width in microseconds.

RWKZBJ The control parameters for minimum and maximum partitioning are expressed in a fixed-point fraction, FRACT, of the available bandwidth, B_TOT. The bandwidth BW allocated to a PARTID at a specific MSC is the following:

Control Type Bandwidth Value

Minimum bandwidth limit partitioning B_TOT*FRACT Maximum bandwidth limit partitioning with SOFTLIM B_TOT if not congested, else B_TOT*FRACT Maximum bandwidth limit partitioning with HARDLIM B_TOT*FRACT Proportional stride partitioning B_TOT/stride

RBPRDL Over the same data path, a concatenation of MSCs of any type listed in this specification guarantees an allocated bandwidth B_PATH where B_PATH is MIN(BW 0..N), and BW 0..N is the bandwidth BW allocated to a PARTID by one MSC.

5.2.8.1 Minimum bandwidth limit partitioning

RWRXBV The minimum bandwidth limit used by a request PARTID is regulated as follows:

- • If the bandwidth used by the request PARTID, as tracked during the accounting window, is less than the minimum bandwidth limit for that PARTID, then requests from that PARTID are preferentially selected to use downstream bandwidth.
- • If the bandwidth used by the request PARTID, as tracked during the accounting window, is greater than or equal to the minimum bandwidth limit for that PARTID, then one of the following applies:


If maximum bandwidth limit partitioning is implemented, then requests from that PARTID compete with other requests as described in Maximum bandwidth limit partitioning.

— If maximum bandwidth limit partitioning is not implemented, all request PARTIDs using bandwidth greater than the PARTID's minimum bandwidth limit compete for bandwidth and do not receive preferential priority.

###### Note

A request PARTID using less than the minimum bandwidth limit is more likely to be scheduled to use downstream bandwidth.

IRYVWR The minimum bandwidth limits of all partitions might sum to more bandwidth than is available. If minimum bandwidth is over-allocated, then a partition’s minimum bandwidth cannot always be met unless unused allocations in other partitions are available for that partition to use. If a system is to reliably deliver the minimum bandwidth allocation, software must ensure that minimum bandwidth is not over-allocated.

ICBQXH The MPAMCFG_MBW_MIN.MIN field specifies the minimum bandwidth limit, using a 16-bit fixed-point fraction format. See The fixed-point fractional format.

5.2.8.2 Maximum bandwidth limit partitioning

RNWVFK The maximum bandwidth limit used by a request PARTID is regulated as follows:

- • If the bandwidth used by the request PARTID, as tracked during the accounting window, is less than the maximum bandwidth limit for that PARTID and, if implemented, greater than or equal to the minimum bandwidth limit, then all of the following apply:

— If there are no competing minimum bandwidth limit requests, then the request is selected to use bandwidth. All request PARTIDs using bandwidth less than the PARTID's maximum bandwidth limit compete for bandwidth.

- • If the bandwidth used by the request PARTID, as tracked during the accounting window, is greater than or equal to the maximum bandwidth limit for that PARTID, then one of the following applies:


- — If MPAMCFG_MBW_MAX.HARDLIM is 0, the request competes with all request PARTIDs using bandwidth greater than or equal to the PARTID's minimum bandwidth limit and less than the maximum bandwidth limit.
- — If MPAMCFG_MBW_MAX.HARDLIM is 1, the request is saved until the PARTID's bandwidth use drops below the maximum bandwidth limit.


###### Note

As the accounting window advances, the tracked bandwidth use resets to zero or otherwise decays, permitting the partition to again use bandwidth.

IMZPWR The maximum bandwidth limits of all partitions might sum to more bandwidth than is available. If maximum bandwidth is over-allocated, then a partition’s maximum bandwidth cannot always be met unless unused allocations in other partitions are available for that partition to use.

IKTKXY The MPAMCFG_MBW_MAX.MAX field specifies the maximum bandwidth limit, using a 16-bit fixed-point fraction format. See The fixed-point fractional format.

5.2.8.3 Using minimum bandwidth limit with maximum bandwidth limit

RPKRGZ If both the minimum bandwidth limit and maximum bandwidth limit are implemented, Table 5-3 shows the request preferences.

Table 5-3 Preference of requests for bandwidth limits

The preference is Description

If used bandwidth is

High Only high requests compete with this requesta.

Below the minimum

Above the minimum

Below the maximum limit. Medium High requests are serviced first, then this request

compete with other medium requestsa. Above the maximum limit, when

- HARDLIM is 0.

Low Requests are not serviced if any high or medium

requests are availablea. Above the maximum limit, when

- HARDLIM is 1.


None Requests are not serviced.

a. Implementations might occasionally deviate from this preference order when servicing requests to meet other goals, such as avoiding bandwidth starvation.

5.2.8.4 Memory bandwidth allocation accounting window width

RVWHYW For PARTID requests, an accounting window tracks bandwidth use over a period of time. If one or both the minimum bandwidth limit and maximum bandwidth limit methods are implemented, they are applied to requests that occur during the accounting window.

IJWFVR Bandwidth tracking can be done using either a moving accounting window or by resetting the bandwidth count at the beginning of each accounting window period. See Fixed accounting window and Moving accounting window.

IRBRXT For the PARTID selected by MPAMCFG_PART_SEL, the width of the accounting window in microseconds is discoverable and can be read from MPAMCFG_MBW_WINWD.

IDJQVQ In implementations that support setting the accounting window width per PARTID, MPAMCFG_MBW_WINWD can be written.

IDCPYV The width of an accounting window is measured in microseconds. The accounting window width in MPAMCFG_MBW_WINWD is specified using a 16-bit fixed-point fraction format. See The fixed-point fractional format.

RPWSWQ Bandwidth that is not used by a partition during an accounting window does not accumulate.

5.2.8.4.1 Fixed accounting window

RXSYFH If a fixed accounting window is used, then all of the following apply:

- • At the start of the window, the bandwidth history is cleared.
- • Bandwidth is tracked over the period of time defined by the window width.
- • Each partition is allocated bandwidth for requests according to the minimum and maximum limits set for that partition. See Minimum bandwidth limit partitioning.
- • When the accounting window width is reached, the bandwidth history is cleared and a new window begins. Any requests queued during the previous window can be serviced in the new window.


RZQXQL Request priority or priority partitioning is used to resolve conflicting requests of the same preference. See Priority partitioning.

5.2.8.4.2 Moving accounting window

RCMZNL If a moving accounting window is used, then all of the following apply:

- • Bandwidth history is never cleared.
- • Bandwidth history is tracked over the period of time defined by the accounting window width.
- • When a bandwidth request processed, it is added to the accounting window.
- • When the age of a processed bandwidth request becomes older than the accounting window width, it is removed from the accounting window.


###### Note

A moving accounting window is relatively free of boundary effects.

IZYPXW A moving accounting window requires hardware to track the history of processed bandwidth requests within the window, in addition to the bandwidth counters per PARTID required by the fixed accounting window.

5.2.8.4.3 Other accounting window methods

RPXSBS An implementation may use any other method for tracking bandwidth requests that broadly complies with the methods described above.

###### Note

For example, the tracked bandwidth might decay at a fixed rate proportional to the bandwidth allocation, but not below zero.

5.2.8.5 Available bandwidth

ICZRXF The bandwidth available downstream from an MSC might not be constant and might affect the operation of minimum and maximum bandwidth partitioning.

Example 5-2

Available bandwidth might depend on one or more clock frequencies in system components, such as a DDR clock. Lowering clock rates without changing bandwidth allocation can cause over-allocation. The available bandwidth on a DRAM channel can vary with the mix of reads and writes, and the bank hit rate. Bandwidth can also vary with burst size.

IWMCYK When available bandwidth is affected by the dynamic behavior of system components, software might be required to reallocate bandwidth.

###### 5.2.9 Memory bandwidth proportional stride partitioning

RZDTLN Memory bandwidth proportional stride partition control is an instance of proportional resource allocation generic control.

IJGRPB Proportional stride partitioning is related to the Linux cgroup weights interface and the VMware® shares interface. For

more information, see Proportional resource allocation facilities.

ILTCCF Memory bandwidth proportional stride partitioning permits a partition to consume bandwidth in proportion to its stride, relative to the strides of other request partitions that are contending for bandwidth. For an example of stride-based proportional bandwidth regulation, see Model of stride-based memory bandwidth scheduling.

RZFCRG MPAMF_MBW_IDR.HAS_PROP indicates the presence of a memory bandwidth proportional stride partitioning control interface in the MSC.

IQHZZJ MPAMCFG_MBW_PROP.STRIDEM1 is the proportional stride partitioning control parameter. It is an unsigned integer representing the normalized cost to a partition for consuming bandwidth.

ITVTLQ Setting MPAMCFG_MBW_PROP.EN to 1 enables proportional stride partitioning for a PARTID,

RTKBDC An active implementation of memory bandwidth proportional stride partitioning is required to work with an active implementation of maximum bandwidth limit partitioning.

###### Note

When multiple bandwidth partitioning controls are implemented and active, it is possible that certain other pairs of bandwidth control cannot work together.

###### 5.2.10 Priority partitioning

IBBNFP Priority affects conflicts that occur during access to resources but does not directly affect the allocation of memory system resources. Prioritization rarely has a substantial performance effect on a properly configured system, but is important in resolving oversubscribed situations. Priority partitioning is a tool for isolating memory system effects between partitions.

RMSPZM A PARTID can be assigned different priorities to different memory system components that implement priority partitioning control.

###### Note

For example, PE requests to system cache might have a higher priority than system cache requests to main memory.

RJJKQP For a system in which the interconnect carries QoS values or priorities, these priorities arrive with incoming requests from upstream. When a system interconnect carries QoS values or priorities, requests arriving at an MSC have an upstream priority and all of the following apply:

• In the absence of internal priority partitioning control, all of the following apply:

— The upstream priority can be used by the MSC to prioritize internal operations.

The upstream priority must be used as the downstream priority. This is referred to as a through priority.

• In the absence of downstream priority partitioning control, the upstream priority must be used as the downstream priority.

RNGDBL Priority partitioning can override the upstream priority as follows:

- • An MSC can use internal priorities to control the internal operation of that MSC.
- • An MSC can set transport priorities for downstream requests generated when servicing an upstream request.


IWWPCV Downstream refers to the communication direction of requests from the MSC. Upstream refers to the response to the

MSC, and it usually uses the same transport priority as the request that generated it.

RMQRPH Internal priority partitioning is OPTIONAL even if downstream priority partitioning is implemented. RRZDSR Downstream priority partitioning is OPTIONAL even if internal priority partitioning is implemented. RMBNHC The priority of a response through an MSC, from downstream to upstream, is always the downstream response priority.

Priority partitioning never alters response priorities received from downstream.

- 5.3 IMPLEMENTATION DEFINED partitioning control interfaces


###### 5.3 IMPLEMENTATION DEFINED partitioning control interfaces

IGSYDZ MPAMF_IIDR uniquely identifies the MSC implementation by the combination of implementer, product ID, variant, and revision.

INPSLG IMPLEMENTATION DEFINED MSC partitioning features, monitoring features, and parameters that do not use the standard MSC interfaces and controls are discoverable in MPAMF_IMPL_IDR. MPAMF_IDR.HAS_IMPL_IDR indicates the presence of MPAMF_IMPL_IDR.

RJPLMZ All of the following apply to MPAM v0.1, and from MPAM v1.1:

• If MPAMF_IMPL_IDR describes no IMPLEMENTATION DEFINED partitioning controls, then

MPAMF_IDR.NO_IMPL_PART must be 1.

• If MPAMF_IMPL_IDR describes no IMPLEMENTATION DEFINED monitors, then

MPAMF_IDR.NO_IMPL_MSMON must be 1.

- 5.4 PARTID narrowing


###### 5.4 PARTID narrowing

RDQTTD It is OPTIONAL whether an implementation supports mapping a request PARTID (reqPARTID) into a smaller internal

PARTID (intPARTID).

RRNWBS The reqPARTID to intPARTID mapping for a physical PARTID address space must be used internally and not for downstream requests.

RGFYDF For each physical PARTID space, if MPAMCFG_PART_SEL.INTERNAL is 1, then the reqPARTID is mapped to the intPARTID in MPAMCFG_INTPARTID.INTPARTID.

IBBVFM If the reqPARTID to intPARTID mapping is invalid, an error code is reported in MPAMF_ESR.ERRCODE.

IGNPJV Loading MPAMCFG_PART_SEL.PARTID_SEL with intPARTID and setting MPAMCFG_PART_SEL.INTERNAL to 1 allows the intPARTID to be configured using the MPAMCFG_* registers.

IMYFTF Only a reqPARTID can configure the intPARTID mapping in MPAMCFG_INTPARTID. Therefore, when accessing MPAMCFG_INTPARTID, MPAMCFG_PART_SEL.INTERNAL must be 0.

ITNVGM If PARTID narrowing not implemented, then MPAMCFG_PART_SEL.INTERNAL must be 0.

- 5.5 MPAM Domains PARTID translation


###### 5.5 MPAM Domains PARTID translation

IHJQYZ If FEAT_MPAM_MSC_DOMAINS is implemented, an MPAM MSC implementing PARTID translation may be enabled to manipulate the MPAM information bundle at different stages of the MPAM MSC processing:

- • A terminating Completer component can be enabled to manipulate the MPAM information bundle as it enters the MSC before being serviced. This is defined as ingress translation.
- • A Requester component can be enabled to manipulate the MPAM information bundle as it exits to be propagated downstream. This is defined as egress translation.
- • An intermediate Completer-Requester component can be enabled to manipulate the information bundle either as it enters the MPAM MSC, as it exits, or both.


See Translation sequence for ingress and egress translation regimes in an MPAM MSC for more information on translation sequences.

###### 5.5.1 Ingress and egress translation behavior of resource controls

IWCPVX The behaviors of resource controls in an MSC with ingress translation configured are the normal behaviors of those controls operating on the PARTIDs translated by the ingress translation feature and according to its configured settings.

INXCJT The behaviors of resource controls in an MSC with egress translation configured are the normal behaviors of those controls operating on the PARTIDs as received with the input requests into the MSC. Any requests leaving the MSC in the downstream direction are translated by the egress translation feature before leaving the MSC.

### Chapter 6Resource Monitors

This chapter contains the following sections:

- • About MPAM resource monitors.
- • Common features.
- • Monitor configuration.
- • Monitor behavior on overflow.
- • Monitor behavior in the presence of ingress and egress translation.


###### 6.1 About MPAM resource monitors

IZDWYG For an MPAM resource that can be partitioned, a resource monitor can be implemented that measures the resource use or capacity use, depending on the resource.

IBMBQY The following are examples of how MPAM MSC resource monitors might be used:

Example 6-1 Examples of MPAM MSC resource monitors usage

Possible MPAM MSC resource monitors:

- • Measuring the bandwidth used by a PARTID and PMG of the memory write subunit.
- • Measuring memory system reads to tune memory system partitioning controls.
- • Measuring cache use by a PARTID and PMG.
- • For finer-grained monitoring, or to make measurements over prospective partitions, use a PMG value to subdivide the software environments within a PARTID.


RWCQKV The architecture defines the following MPAM resource monitors:

- • Memory bandwidth monitors. See Memory bandwidth monitors.
- • Cache storage monitors. See Cache storage monitors.


IGRXQW A monitor can be configured to measure use by PARTID, or a combination of PARTID and PMG.

RZGGMX The PARTID used to filter a resource monitor is always a request PARTID, even if PARTID narrowing is implemented.

ISRZWD Monitors can be configured to measure use of resource subunits.

IHLQRN For each resource monitor type, up to 216 instances can be implemented.

IWCGPX The monitor instance and associated MSMON_<type> registers are selected by MSMON_CFG_MON_SEL.MON_SEL. See Monitor configuration.

IJXJRL If resource instance selection is implemented, then MSMON_CFG_MON_SEL.RIS selects the partitionable resource. See Resource instance selection.

RTMCXJ Resource monitors are associated with a partitionable resource, but memory bandwidth monitors might be placed at the top level of an MSC. Monitors at the top level of an MSC are accessed with a RIS value of 0.

###### 6.1.1 Memory bandwidth monitors

RYFMQP A memory bandwidth monitor counts downstream write or upstream read payload bytes that pass the monitoring point and meet the filter criteria.

IKZLXB For an MSC, the following memory-mapped registers are used to configure each memory bandwidth monitor:

- • A control register, MSMON_CFG_MBWU_CTL, that configures the monitor instance behavior.
- • A filter register, MSMON_CFG_MBWU_FLT, that specifies the criteria for which transfers are counted. This register has read, write, PARTID, PMG, and other criteria fields.
- • A monitor register, MSMON_MBWU, that optionally scales the count of bytes transferred that match the filter register conditions. This register can be reset after each capture event. If scaling is enabled, the actual number of bytes transferred is determined by shifting MSMON_MBWU.VALUE left by MPAMF_MBWUMON_IDR.SCALE bit positions. See Scaled MBWU count value. MSMON_MBWU.NRDY indicates whether the monitor instance is ready and the MSMON_MBWU.VALUE field is accurate. See Not-ready Bit.
- • An optional capture register, MSMON_MBWU_CAPTURE, that is loaded from MSMON_MBWU each time the selected capture event occurs. After a capture event occurs, the monitor register is optionally reset to zero.


- • In MPAM v0.1 and from MPAM v1.1, an optional long count monitor register, MSMON_MBWU_L, that contains a count of 44 bits or 63 bits. See Long MBWU counter and capture. MSMON_MBWU_L.NRDY indicates whether the monitor instance is ready and the MSMON_MBWU_L.VALUE field is accurate. See Not-ready Bit.
- • In MPAM v0.1 and from MPAM v1.1, if MPAMF_MBWUMON_IDR.{HAS_LONG, HAS_CAPTURE} is {1, 1}, the capture register MSMON_MBWU_L_CAPTURE must be implemented. This register is loaded from MSMON_MBWU_L each time the selected capture event occurs. After a capture event occurs, the long count monitor register is optionally reset to zero.


RDLSGF If the optional capture register is implemented, then a capture event must be implemented. A capture event transfers a monitor count register to the corresponding capture register and optionally resets the count register. See Capture event and capture register.

IHBHNX If a capture event resets the count register, then the following byte transfer counts can be determined:

- • The count during the interval between the last two capture events can be read from MSMON_MBWU_CAPTURE.
- • The count since the last capture event can be read from MSMON_MBWU.


IRDVPY Bandwidth use can be computed using the values read from MSMON_MBWU or MSMON_MBWU_CAPTURE and the count collection interval.

IMHDGB Capture events can have external and local sources, specified by MSMON_CFG_MBWU_CTL.CAPT_EVNT. If MPAMF_MSMON_IDR.HAS_LOCAL_CAPT_EVNT is 1, then a local capture event generator is implemented and generates events when certain values are written to MSMON_CAPT_EVNT.

###### Note

It can be advantageous to use a single event to capture several MSC monitors simultaneously. A periodic capture event for multiple MSCs could be generated at the system level, perhaps using a generic timer, and distributed to the MSCs.

IVQSZY For a monitor instance, if MSMON_CFG_MBWU_CTL.OFLOW_LNKG is implemented and non-zero, then when the monitor instance overflows, the capture event number in that field can be signaled. See Control of signaling to other monitor instances.

6.1.1.1 Scaled MBWU count value

IKDTXD The transferred byte count in MSMON_MBWU.VALUE can be scaled.

IVWVVS The value of MSMON_CFG_MBWU_CTL.SCLEN has the following effects:

- • If 0, then MSMON_MBWU.VALUE is not scaled.
- • If 1, then MSMON_MBWU.VALUE is scaled.


IMSWWW MPAMF_MBWUMON_IDR.SCALE is the amount that MSMON_MBWU.VALUE is scaled. The scaled MSMON_MBWU.VALUE is the true count of bytes transferred, rounded to 2SCALE and shifted right by SCALE bit positions.

IWDNND For a monitoring point, SCALE is should be set such that the periodic sampling and reset of MSMON_MBWU_CAPTURE can count the highest traffic rates possible at that monitoring point without overflowing MSMON_MBWU.VALUE. The sampling rate is limited by the target use.

Example 6-2

If the maximum traffic at the monitoring point is 300 GBps and the system supports capturing the counter 30 times per second, the counter must be scaled to no more than 231 - 1 counts per thirtieth of a second. This requires scaling the counter by a factor of at least 5, so the SCALE must be at least 3.

IJSZDB If the memory traffic is distributed across several MSCs, total bandwidth measurement might require reading multiple MSC memory bandwidth monitors and summing the results. Monitor values can be correlated by using the same system level capture event to capture those monitors.

RWHJQG An overflow occurs when MSMON_MBWU.VALUE exceeds the maximum value.

IXDHCT When MSMON_MBWU.VALUE overflows, all of the following apply:

- • MSMON_CFG_MBWU_CTL.OFLOW_STATUS is set to 1.
- • If MSMON_CFG_MBWU_CTL.OFLOW_INTR is set to 1, then an MPAM Overflow interrupt is generated. See MPAM overflow interrupt and Control of monitor overflow behavior.


6.1.1.2 Long MBWU counter and capture

IWYMVS In MPAM v0.1 and from MPAM v1.1, if MPAMF_MBWUMON_IDR.HAS_LONG is 1, then the long count monitor register, MSMON_MBWU_L, is implemented and can be programmed to count 44 bits or 63 bits.

IRYFKB If MPAMF_MBWUMON_IDR.HAS_LONG and MPAMF_MBWUMON_IDR.HAS_CAPTURE are 1, then MSMON_MBWU_L_CAPTURE must be implemented and can be programmed to capture 44 bits or 63 bits.

ILTTSN The following values of MPAMF_MBWUMON_IDR.LWD determine the size of MSMON_MBWU_L.VALUE and MSMON_MBWU_L_CAPTURE.VALUE:

- • If 0, then the VALUE field is 44 bits and VALUE[62:44] is RES0.
- • If 1, then the VALUE field is 63 bits.


RVLSJS MSMON_MBWU_L.VALUE is never scaled.

RRYRQJ An overflow occurs when MSMON_MBWU_L.VALUE exceeds the maximum representable value.

IGBYGY When MSMON_MBWU_L.VALUE overflows, all of the following apply:

- • MSMON_CFG_MBWU_CTL.OFLOW_STATUS_L is set to 1.
- • If MSMON_CFG_MBWU_CTL.OFLOW_INTR_L is set to 1, then an MPAM Overflow interrupt is generated. See MPAM overflow interrupt and Control of monitor overflow behavior.


ITSVTB When both MSMON_MBWU and MSMON_MBWU_L are implemented, MSMON_MBWU.VALUE might overflow before MSMON_MBWU_L.VALUE. This can be prevented by setting MSMON_CFG_MBWU_CTL.OFLOW_INTR to 0 to disable the overflow interrupt for MSMON_MBWU.VALUE.

IGCZSJ Setting MSMON_CFG_MBWU_CTL.OFLOW_FRZ to 1 freezes a monitor instance on overflow. This control affects both MSMON_MBWU and MSMON_MBWU_L.

- 6.1.2 Cache storage monitors ILNKJB For an MSC, the following memory-mapped registers are used to configure each cache storage monitor:


- • A control register, MSMON_CFG_CSU_CTL, that configures the monitor instance behavior.
- • A filter register, MSMON_CFG_CSU_FLT, that sets the PARTID and PMG to be monitored.
- • A monitor register, MSMON_CSU that reports the amount of storage currently present within the cache allocated by the PARTID and PMG. It is IMPLEMENTATION DEFINED whether MSMON_CSU is implemented as RO or RW. MSMON_CSU.NRDY indicates whether the monitor instance is ready and the MSMON_CSU.VALUE field is accurate. See Not-ready Bit.
- • An optional capture register, MSMON_CSU_CAPTURE, that is loaded from MSMON_CSU each time the selected capture event occurs. After a capture event occurs, the monitor register is optionally reset to zero.


RQGXSZ If the optional capture register is implemented, then a capture event must be implemented. A capture event transfers a monitor cache storage usage register to the corresponding capture register and optionally resets the count register. See Capture event and capture register.

ILHXNS Capture events can have external and local sources, specified by MSMON_CFG_CSU_CTL.CAPT_EVNT. If MPAMF_MSMON_IDR.HAS_LOCAL_CAPT_EVNT is 1, then a local capture event generator is implemented and generates events when certain values are written to MSMON_CAPT_EVNT.

###### Note

It can be advantageous to use a single event to capture several MSC monitors simultaneously. A periodic capture event for multiple MSCs could be generated at the system level, perhaps using a generic timer, and distributed to the MSCs.

ITFLMB The cache storage usage monitor architecture supports overflow behavior in CSU monitors. However, Arm recommends that cache storage usage monitor designs prevent overflow.

###### 6.2 Common features

IFJGYQ All of the following are features of MPAM resource monitors:

- • Monitor register. See Monitor register.
- • Not-ready bit. See Not-ready bit.
- • Capture event and capture register. See Capture event and capture register.
- • Overflow status bit. See Overflow status bit.
- • Enable bit. See Enable bit.


###### 6.2.1 Monitor register

RVJDKF A monitor instance has a monitor register that contains a VALUE field and a NRDY field. For more information on the NRDY field, see

RDSCQB All of the following apply to the monitor register VALUE field:

- • When the monitor is enabled, the VALUE field can change at any time.
- • If the monitor counts events, then all of the following apply: When a monitored event occurs that matches the filter criteria, the VALUE count increases.

— The VALUE field can be reset so that the count is based entirely on the updated field.

- • If the monitor measures resource use, then all of the following apply:


— The VALUE field measures bytes used in cache storage. The VALUE field can increase and decrease as the the filter criteria causes resource use to change.

RFWCFZ If a write updates the VALUE field in a monitor register that counts events, then that monitor continues counting the monitored event from the new VALUE.

RPVNHR If a write updates the VALUE field in a monitor register that measures resource, then the VALUE field in that monitor is overwritten by automatic monitor updates.

RKCWDZ A monitor register is permitted to be implemented as RO.

RXLWYW If a monitor register is implemented as RW, it is not mandatory to disable that monitor to reprogram it.

RGKWNB For a monitor instance, when the events counted or the resources measured in the configuration and filter registers are modified, the monitor VALUE field continues counting or measuring using the new criteria without automatically resetting VALUE to 0.

IDGYLF If both MSMON_MBWU and MSMON_MBWU_L are implemented in an MSC as RW, the VALUE fields cannot be reset simultaneously. Arm recommends the following sequence for resetting the VALUE fields:

- 1. Set MSMON_CFG_MON_SEL with the desired monitor instance selector and RIS.
- 2. Set the MSMON_CFG_MBWU_CTL.EN bit to 0.
- 3. Reset the VALUE field in both monitors to 0.
- 4. Change the register filter settings in MSMON_CFG_MBWU_FLT.
- 5. Update MSMON_CFG_MBWU_CTL with the new configuration settings and set the EN field to 1.


###### 6.2.2 Not-ready bit

ILGPFY A monitor instance might need time to converge on an accurate count or measurement. For example, a cache storage monitor might require tens of microseconds to collect enough information to start accurately tracking the resource use.

IPCDHG All of the following apply to the Not-ready bit, NRDY, in MSMON_MBWU and MSMON_MBWU_L:

- • If 0, then the monitor instance is ready and the VALUE field is accurate.
- • If 1, then the monitor instance is not ready and the VALUE field does not have an accurate count or measurement due to recent changes to the monitor settings.


IZYQXY The NRDY bit of one monitor instance is independent of the NRDY bit of all other monitor instances.

RLRTGP If any monitor instance requires time to establish an accurate count or measurement when the monitor settings are updated, then the monitor instance NRDY bit must automatically be set to 1 when those settings are updated.

RNXKSF For a monitor instance that measures resource, the monitor NRDY bit must automatically be set to 0 when the count or measurement is accurately represented in the monitor.

RFVFYC For a monitor instance that counts events, automatic setting the monitor NRDY bit to 0 is not required. If automatic update is not implemented, the monitor instance NRDY bit remains set to 1 until it is set to 0 by software.

RZTXDS If a monitor does not support automatic updates of NRDY, software can use that bit for any purpose.

RVBZTW If the monitor instance settings are not updated again, the NRDY bit must automatically be set to 0 within the

IMPLEMENTATION DEFINED time defined by MAX_NRDY_USEC. See Performance-monitoring parameters.

###### Note

MAX_NRDY_USEC is a discoverable firmware data value.

RFZHDC When a capture event occurs, all of the following apply to the monitor instance NRDY bit:

- • The monitor register, including the NRDY bit, is copied to the capture register.
- • After the monitor register is copied, the NRDY bit is reset to 0.


###### 6.2.3 Capture event and capture register

ICDJPZ Fields in MSMON_CFG_CSU_CTL and MSMON_CFG_MBWU_CTL configure the behavior of a monitor instance to capture events. For all monitor instances of a resource instance and monitor type, both registers must have the same capture event configuration.

RYFVPY All of the following apply to capture events:

- • Capture events are available to all MSC monitors.
- • For all resource instances and monitor types, all monitor instances can be sensitive to any capture event.
- • For a PARTID space, every monitor instance monitoring that space must be sensitive to the same capture event.
- • For a capture event, every monitor that is configured to be sensitive to that event is copied into the capture register for that monitor.


RNKTBN Capture events complete immediately without signaling that the event has completed.

ISFTQP Capture events can be any of the following:

- • Local events to the MSC.
- • System-defined events external to the MSC.
- • Single events initiated by software.
- • A periodically repeating series of events.


ITYKVD A generic counter can be used as the source of a periodically repeating series of events, but this is not required.

IKTZJB If a capture event is periodic, software can read the capture registers at any time to get the most recent capture information.

RNWKJR An external capture event can be distributed system-wide so that all monitors sensitive to that external event are captured.

IVRGXF There following table summarizes the capture event codes:

Table 6-1 Capture event codes and function

Capture event code Function

0 No capture event is triggered. 1 External capture event 1 (optional, but recommended) 2 - 6 External capture events (optional) 7 Local capture event

6.2.3.1 Local capture event generator

IHRXHL When MSMON_CAPT_EVNT.NOW is set to 1, a local capture event is generated to all monitor instances specified by MSMON_CAPT_EVNT.ALL.

IZSZYK For each address space, there are separate MSMON_CAPT_EVNT registers defined.

IJFMXT If MPAMF_MSMON_IDR.HAS_LOCAL_CAPT_EVNT is 0, local capture events are not generated and any monitors that have CAPT_EVNT set to 7 in the corresponding control register do not receive capture events.

6.2.3.2 Reset on capture

RSQSMV A capture event is permitted, but not required, to reset the monitor after it is copied to the capture register.

IXTPRS If MSMON_CFG_*_CTL.CAPT_RESET is 1, the corresponding monitor is reset after the value is copied to the corresponding MSMON_*_CAPTURE register.

INRRJG If a monitor reports a measurement rather than a count, and it is not reasonable to automatically reset that measurement, then Arm recommends that the corresponding MSMON_CFG_*_CTL.CAPT_RESET is RAZ/WI.

###### 6.2.4 Overflow status bit

RWQJKV When the monitor VALUE field exceeds the largest representable value, a monitor overflow occurs and sets the overflow status bit, MSMON_CFG_<type>_CTL.OFLOW_STATUS, to 1.

RXZXVK After the monitor overflow occurs, the monitor VALUE field wraps and might be zero or greater, depending on the counter increment or the measurement value.

IRZWDC When an overflow occurs, the following overflow behaviors are determined by MSMON_CFG_<type>_CTL:

- • MSMON_CFG_<type>_CTL.OFLOW_FRZ determines whether the monitor VALUE field is frozen or continues to count or measure.
- • MSMON_CFG_<type>_CTL.OFLOW_INTR determines whether an MPAM overflow interrupt is signaled by the MSC. See MPAM overflow interrupt.
- • MSMON_CFG_<type>_CTL.OFLOW_CAPT determines whether the overflowed monitor VALUE field is saved in the monitor instance capture register.
- • A capture event can be signaled to other monitor instances that are programmed to be sensitive to that capture event. See Capture event and capture register.
- • The overflowed monitor VALUE and NRDY fields can be reset to zero.


RNDMMK Setting MSMON_CFG_<type>_CTL.OFLOW_STATUS to 0 resets the overflow status.

###### 6.2.5 Enable bit

IPQKYY All of the following apply to MSMON_CFG_<type>_CTL.EN:

- • If 0, then the monitor is disabled. A disabled monitor can be updated by software.
- • If 1, then the monitor is enabled to count events or measure resource use matching the configuration in MSMON_CFG_<type>_CTL and MSMON_CFG_<type>_FLT.


RVBBTL The monitor configuration registers can be read and written regardless of the value of MSMON_CFG_<type>_CTL.EN.

###### 6.3 Monitor configuration6.3 Monitor configuration

IWDSGJ For each resource monitor type, the number of available monitor instances is described in the corresponding MPAMF_<type>MON_IDR.NUM_MON field.

ITLHDL MSMON_CFG_MON_SEL.MON_SEL selects the monitor instance of a particular type to configure.

IFZMRT All monitor types have the following two 32-bit configuration registers:

- • MSMON_CFG_<type>_FLT has fields to select the PARTID and PMG to monitor.
- • MSMON_CFG_<type>_CTL has controls for counting a subset of events, overflow behavior, and capture behavior.


ITRXTJ If a monitor type does not require all fields, those fields must be RAZ/WI or RAO/WI.

###### 6.4 Monitor behavior on overflow

ICDYNR When an MPAM monitor instance overflows, the corresponding MSMON_CFG_<type>_CTL.OFLOW_STATUS flag is set to 1.

RXSTRM For a monitor instance, all of the following optional overflow behaviors can be configured:

- • Signal an overflow interrupt. See Control of monitor overflow behavior and MPAM overflow interrupt.
- • Freeze a monitor instance. See Control of monitor overflow behavior.
- • If the capture register is implemented, then capture the monitor instance in the capture register.
- • Broadcast an external capture event to all MSC monitors. See Control of monitor instance behavior on a capture event and Table 6-1.


RJCNST A group of monitor instances can be linked so that a capture event caused by the overflow of one monitor instance is broadcast to the other monitor instances, and those monitor instances respond to that broadcast capture event as an overflow. See Control of overflow linkage.

RDFWFP Capture events are local to an MSC and are not broadcast to other MSCs.

###### 6.4.1 Control of monitor overflow behavior

INSWLJ When MSMON_<type>.VALUE overflows, the corresponding MSMON_CFG_<type>_CTL.OFLOW_STATUS field changes from 0 to 1.

IHPCFR For a monitor instance, when an overflow occurs, the following MSMON_CFG_<type>_CTL register fields control the resulting behavior:

- • OFLOW_LNKG specifies the capture event number that is signaled to other MSC monitor instances.
- • OFLOW_CAPT determines whether a monitor instance is copied to the corresponding capture register.
- • OFLOW_FRZ determines whether the monitor instance VALUE field is frozen and no longer updated.
- • OFLOW_INTR determines whether an interrupt is signaled.


ILFXTG If MSMON_<type>.VALUE is frozen due to an overflow, then it resumes counting or measuring when MSMON_<type>.VALUE is written with a new starting value.

RGNQFY When MSMON_<type>.VALUE is written, MSMON_CFG_<type>_CTL.OFLOW_STATUS is reset from 1 to 0.

###### 6.4.2 Control of overflow linkage

IBFGLL MPAMF_<type>MON_IDR.HAS_OFLOW_LNKG determines whether the monitor type implements overflow linkage.

IJJQKK If a monitor type implements overflow linkage, MSMON_CFG_<type>_CTL.OFLOW_LNKG controls whether an overflow of the corresponding monitor instance signals a capture event to other monitor instances that monitor the same PARTID space.

ICNHWZ When MSMON_<type>.VALUE overflows, an overflow linkage capture event is signaled when all of the following apply:

- • MSMON_CFG_<type>_CTL.OFLOW_INTR is 1.
- • MSMON_CFG_<type>_CTL.OFLOW_LNKG is non-zero and corresponds to a capture event implemented by the MSC.


IGHQTG When MSMON_<type>_L.VALUE overflows, an overflow linkage capture event is signaled when all of the following apply:

- • MSMON_CFG_<type>_CTL.OFLOW_INTR_L is 1.
- • MSMON_CFG_<type>_CTL.OFLOW_LNKG is non-zero and corresponds to a capture event implemented by the MSC.


IXMMRX For a monitor type, the following MSMON_CFG_<type>_CTL register fields control the behavior of an overflow linkage capture event:

- • CEVNT_OFLW is set to 1 to indicate the monitor instance treats a capture event as an overflow and the overflow behaviors are performed.
- • CAPT_EVNT is set to the capture event number used for overflow linkage.
- • OFLOW_CAPT determines whether a monitor instance is copied to the corresponding capture register. This field is present when MPAMF_<type>MON_IDR.{HAS_CAPTURE, HAS_OFLOW_CAPT} are {1, 1}.
- • OFLOW_CAPT_L determines whether a long monitor instance is copied to the corresponding long capture register. This field is only in MSMON_CFG_MBWU_CTL. This field is present when MPAMF_MBWUMON_IDR.{HAS_CAPTURE, HAS_LONG, HAS_OFLOW_CAPT} are {1, 1, 1}.
- • OFLOW_FRZ determines whether the monitor instance VALUE field is frozen and no longer updated. If the monitor implements both normal and long versions of the count, both are frozen and each must be written to unfreeze the monitor VALUE.
- • CAPT_RESET specifies that the the monitor is set to 0 after the monitor instance is copied to the capture register.


- 6.4.3 Control of monitor instance behavior on a capture event RMGGJW For an MSC, an external capture event is distributed to all monitors of all PARTID spaces.

RHXNGD A local capture event is distributed to monitor instances in all of the following address spaces:

- • If originating from the Non-secure address space, then the Non-secure PARTID space.
- • If originating from the Secure address space, then the Secure address space.
- • If originating from the Realm address space, then the Realm PARTID address space.
- • If originating from the Root address space, then the Root PARTID address space.
- • If originating from the Secure address space that has the MSMON_CAPT_EVNT.ALL bit set to 1, then both the Secure and Non-secure PARTID spaces.
- • If the capture event is raised by a monitor instance overflow with the corresponding MSMON_CFG_<type>_CTL.OFLOW_LNKG set to 1 through 6, then monitor instances for the same PARTID space as the overflowing monitor instance.


- 6.4.4 Monitors without capture on overflow


RRPGBQ For a monitor type that does not implement capture on overflow, if all of the following apply, then that monitor is permitted to signal an overflow linkage event to other monitor instances that implement capture events:

• The monitor implements MSMON_CFG_<type>_CTL.{OFLOW_INTR, OFLOW_LNKG}.

• If the monitor implements the long monitor register, signaling an overflow linkage event of that register requires implementation of MSMON_CFG_MBWU_CTL.{OFLOW_INTR, OFLOW_INTR_L, OFLOW_LNKG}.

RMTGGZ For a monitor type that does not implement capture on overflow, if that monitor type implements MSMON_CFG_<type>_CTL.{CAPT_EVNT, OFLOW_FRZ}, then a monitor instance that has not overflowed can be frozen by a capture event.

###### Note

This behavior requires that MSMON_CFG_<type>_CTL.CAPT_EVNT is set to a non-zero capture event and MSMON_CFG_<type>_CTL.OFLOW_FRZ is set to 1.

###### 6.5 Monitor behavior in the presence of ingress and egress translation6.5 Monitor behavior in the presence of ingress and egress translation

IJTNQC Monitors which support PARTID translation features may support filtering based on the PARTID belonging to the MPAM MSC PARTID_MAX range, which is the translated PARTID after ingress translation or the untranslated PARTID before egress translation. See MPAMF_MSMON_IDR.HAS_TL_MONITORING.

### Chapter 7MPAM MSC Errors

This chapter contains the following sections:

- • Reporting errors.
- • Behavior of configuration reads and writes with errors.
- • Optionality of error detection and reporting.


###### 7.1 Reporting errors

IKNNQB When an MSC detects an error accessing an MPAM memory-mapped register (MMR), information about that error is captured in MPAMF_ESR.

###### Note

The detected errors might be caused by software.

IMTCZQ The cause of an MPAM error is recorded in MPAMF_ESR.ERRCODE. For a description of the errors that are recorded, see Error conditions accessing memory-mapped registers.

IVLNGQ MSCs might signal errors using an error interrupt. See Error interrupt.

RDYNFP Detected and undetected errors must not prevent the MSC from handling an MPAM request.

IKHNQX Errors can cause the MSC to use different MPAM resource control settings or cause monitors to misattribute monitored events. See Optionality of error detection and reporting.

###### Note

Certain errors might be impossible due to MSC implementation choices. For example, if the request interface only implements bits in the range of 0 to PARTID_MAX and does not detect whether the unimplemented high-order PARTID bits are all zero, then an out-of-range request PARTID cannot be detected and an ERRCODE of 2 cannot occur.

ITNYQH When an error occurs and RIS is implemented, hardware is permitted to choose between CONSTRAINED UNPREDICTABLE behaviors and unimplemented RIS bits that can reduce or remove the need to detect or report any of errors related to RIS. For more information on RIS, see Resource instance selection.

###### 7.1.1 Error conditions accessing memory-mapped registers

RWHSLQ Table 7-1 describes the errors recorded that can be recorded in MPAMF_ESR.ERRCODE. All of the following apply to this table:

- • The selection of MPAMF_ESR depends on the memory request Security state.
- • MPAM error code is the binary value recorded in MPAMF_ESR.ERRCODE.
- • Error name is the error mnemonic.
- • Error description describes the reason the error occurred.
- • Fields captured lists the following other fields that are captured in MPAMF_ESR when the error occurs: RIS is the resource instance selector value captured in MPAMF_ESR.RIS. This field is only available when MPAMF_IDR.EXT and MPAMF_IDR.HAS_RIS are 1.

— PMG is the program monitoring group captured in MPAMF_ESR.PMG. PARTID is captured in MPAMF_ESR.PARTID_MON on an error that captures the partition ID. Depending on the error, PARTID might be a PARTID_SEL field in a register, a reqPARTID, or an intPARTID.

— MON_SEL is captured in MPAMF_ESR.PARTID_MON on an error that captures the monitor index.

- • If FEAT_MPAMv0p1 and FEAT_MPAMv1p1 are not implemented, MPAM error codes 0b1000 - 0b1111 are reserved.


Table 7-1 Error conditions in accessing memory-mapped registers

MPAM error code Error name Error description Fields captured

- 0b0000 No Error No error captured in MPAMF_ESR. None

- 0b0001 PARTID_SEL_Range An out-of-range PARTID is written to MPAMCFG_PART_SEL.PARTID_SEL.


PARTID_SEL and RIS

- 0b0010 Req_PARTID_Range The reqPARTID is out-of-range. One of the following CONSTRAINED UNPREDICTABLE behaviors are permitted:

- • The request is processed using any valid PARTID in the same Security state as the reqPARTID.
- • The request is processed using the default PARTID for the Security state.


Arm recommends that the default PARTID for the Security state is used.

reqPARTID and PMG

- 0b0011 MSMONCFG_ID_RANGE The monitor configuration has an out-of-range PARTID or PMG.


PARTID, PMG, and RIS

- 0b0100 Req_PMG_Range The request PMG is out-of-range. One of the following CONSTRAINED UNPREDICTABLE behaviors are permitted:

- • The request is processed using any valid PARTID and PMG in the same Security state as the reqPARTID.
- • The request is processed using the default PARTID and PMG for the Security state.
- • For performance monitoring, the request is IGNORED and behaves as if the monitor PMG filter does not match the PMG value, regardless of whether the PARTID matches.


Arm recommends that the default PARTID and PMG for the Security state are used.

reqPARTID and PMG

- 0b0101 Monitor_Range An out-of-range monitor selector is written to MSMON_CFG_MON_SEL.MON_SEL.


MON_SEL and RIS

- 0b0110 intPARTID_Range If PARTID narrowing is implemented, then any of the following cause this error to occur:

- • When configuring the association of reqPARTID to intPARTID, any of the following:

— An out-of-range intPARTID is written to MPAMCFG_INTPARTID.INTPARTID.

— A write sets MPAMCFG_INTPARTID.INTERNAL to 0.

- • When an intPARTID is expected, MPAMCFG_PART_SEL.INTERNAL is not set to 1. This includes reads and writes to any MPAMCFG_* register, other than MPAMCFG_INTPARTID.


intPARTID

- 0b0111 Unexpected_INTERNAL If PARTID narrowing is implemented, then this error occurs when a reqPARTID is expected and MPAMCFG_PART_SEL.INTERNAL is set to 1. A read that causes this error returns an UNKNOWN value.


reqPARTID

- 0b1000 Undefined_RIS_PART_SEL An attempt to access an MPAM MSC resource is made and MPAMCFG_PART_SEL.RIS does not correspond to a RIS value allocated to that resource. The resulting access is RAZ/WI. It is CONSTRAINED UNPREDICTABLE whether or not this error is reported.

PARTID_SEL and RIS

- 0b1001 RIS_No_Control An attempt to access an MPAM MSC resource is made and MPAMCFG_PART_SEL.RIS selects an existing resource but the selected PARTID is not configured to access that resource. The resulting access is RAZ/WI. It is CONSTRAINED UNPREDICTABLE whether or not this error is reported.


PARTID_SEL and RIS

- 0b1010 Undefined_RIS_MON_SEL An attempt to access an MPAM MSC resource is made and MSMON_CFG_MON_SEL.RIS does not correspond to any resource. The resulting access is RAZ/WI. It is CONSTRAINED UNPREDICTABLE whether or not this error is reported.

MON_SEL and RIS

- 0b1011 RIS_No_Monitor An attempt to access an MSMON_<type> or MSMON_<type>_CAPTURE register is made and MSMON_CFG_MON_SEL.RIS does not correspond to any monitor of that <type>. The resulting access is one of the following CONSTRAINED UNPREDICTABLE behaviors:


MON_SEL and RIS

- • Read as 0xFFFFFFFE and WI, and do not report the error.

This is a value that is unlikely to be returned by a monitor under normal conditions.

- • RAZ/WI, and do not report the error.
- • RAZ/WI, and report the error.


If the access is to an MSMON_<type>_* register, then the resulting access is RAZ/WI, and it is CONSTRAINED UNPREDICTABLE whether or not this error is reported.

0b1100 - 0b1111 Reserved Reserved for future use. -

###### 7.1.2 Overwritten error status

RGVBGS An MPAM error always updates MPAMF_ESR.ERRCODE, whether or not that field contains a previously recorded error syndrome.

IVCBBQ If MPAMF_ESR.ERRCODE is not 0, then when an MPAM error updates MPAMF_ESR, the OVRWR bit is set. Table 7-1 describes the possible states of MPAMF_ESR.{OVRWR, ERRCODE}.

Table 7-2 Overwritten error states

OVRWR ERRCODE Description

- 0 0b0000 No errors are recorded in MPAMF_ESR.


OVRWR ERRCODE Description

- 0 Non-zero Not overwritten. Hardware has written a single error to MPAMF_ESR since it was last cleared to 0.

- 1 0b0000 Only a software write can produce this state.


- 1 Non-zero Overwritten. Hardware has written two or more errors to MPAMF_ESR since it was last cleared to 0. Only the syndrome information from the latest error is recorded in MPAMF_ESR.ERRCODE.


INJLZB To accurately indicate that more than one error has been written before an MPAM error is serviced, software should clear MPAMF_ESR.{OVRWR, ERRCODE} to {0, 0} after reading the register contents.

###### 7.1.3 Reporting errors involving RIS

INRJRS If MPAMCFG_PART_SEL.RIS or MSMON_CFG_MON_SEL.RIS is misconfigured, an error might occur. See Optionality of error detection and reporting.

RQBKDG When a misconfigured RIS value causes an error, the MPAMF_ESR.RIS field must be set to one of the following:

- • If the error is caused by MPAMCFG_* register accesses, then MPAMCFG_PART_SEL.RIS.
- • If the error is caused by MSMON_* register accesses, then MSMON_CFG_MON_SEL.RIS.


IGMYXP For MPAM errors that do not update MPAMF_ESR.RIS as shown in Table 7-1, MPAMF_ESR.RIS must be set to 0.

###### 7.2 Behavior of configuration reads and writes with errors

###### 7.2.1 Attempting to write an out-of-range PARTID to MPAMCFG_PART_SEL.PARTID_SEL

RDLSCZ If a write attempts to update MPAMCFG_PART_SEL.PARTID_SEL with an out-of-range value, then one of the

following IMPLEMENTATION DEFINED behaviors are permitted:

- • MPAMCFG_PART_SEL.PARTID_SEL is updated without the implementation checking the new value. If the updated MPAMCFG_PART_SEL.PARTID_SEL is used to access a configuration register, an out-of-range PARTID error can occur (MPAMF_ESR.ERRCODE is 0b0001). For more information, see Required error condition detection.
- • The implementation checks the new value. If the out-of-range value is detected, all of the following apply: MPAMCFG_PART_SEL is not updated.


— An out-of-range PARTID error occurs (MPAMF_ESR.ERRCODE is 0b0001).

###### 7.2.2 Reading MPAM MSC configuration registers when MPAMCFG_PART_SEL.PARTID_SEL is out-of-range

IZYXGC If MPAMCFG_PART_SEL.PARTID_SEL is out-of-range, then when reading any MPAMCFG_* register other than MPAMCFG_PART_SEL, one of the following applies:

- • If the implementation detects the out-of-range value, then all of the following apply:

— An out-of-range PARTID error occurs (MPAMF_ESR.ERRCODE is 0b0001). For more information, see Required error condition detection. The value read from the configuration register is one of the following IMPLEMENTATION DEFINED choices:

— An UNKNOWN value is read.

— Zero is read.

- • If the implementation does not detect the out-of-range value, then the value read from the configuration register is UNKNOWN.


###### 7.2.3 Writing MPAM MSC configuration registers when MPAMCFG_PART_SEL.PARTID_SEL is out-of-range

RZWRQW If MPAMCFG_PART_SEL.PARTID_SEL is out-of-range, then when writing any MPAMCFG_* register other than MPAMCFG_PART_SEL, all of the following apply:

- • If the implementation detects the out-of-range value, an out-of-range PARTID error occurs (MPAMF_ESR.ERRCODE is 0b0001). For more information, see Required error condition detection.
- • Whether the implementation detects the out-of-range value or not, one of the following IMPLEMENTATION DEFINED behaviors apply:


— The write updates the register using an UNKNOWN in-range PARTID.

— The write is ignored (WI).

###### 7.2.4 Attempting to write an undefined RIS to MPAMCFG_PART_SEL.RIS

RVKKDP If RIS is supported and a write attempts to update MPAMCFG_PART_SEL.RIS with an undefined value, then one of the

following IMPLEMENTATION DEFINED behaviors are permitted:

- • MPAMCFG_PART_SEL.RIS is updated without the implementation checking the new value. If the updated MPAMCFG_PART_SEL.RIS is used to access a configuration register, an undefined RIS error can occur (MPAMF_ESR.ERRCODE is 0b1000).
- • The implementation checks the new value. If the undefined value is detected, all of the following apply:


— MPAMCFG_PART_SEL is not updated. An undefined RIS error occurs (MPAMF_ESR.ERRCODE is 0b1000).

INVKGT An undefined RIS value is a RIS value that does not correspond to an implemented resource instance.

###### 7.2.5 Reading other MPAM MSC registers when MPAMCFG_PART_SEL.RIS contains an undefined RIS value

RXTQZR If RIS is supported and MPAMCFG_PART_SEL.RIS is undefined, then when reading any MPAMF*IDR, or MPAMCFG_* register other than MPAMCFG_PART_SEL, all of the following apply:

• If the implementation detects the undefined value, then an undefined RIS error occurs

(MPAMF_ESR.ERRCODE is 0b1000). For more information, see Required error condition detection. • Whether the implementation detects the undefined value or not, the value read from the register is UNKNOWN.

###### 7.2.6 Writing other MPAM MSC registers when MPAMCFG_PART_SEL.RIS contains an undefined RIS value

RXDCDL If RIS is supported and MPAMCFG_PART_SEL.RIS is undefined, then when writing any MPAMCFG_* register other than MPAMCFG_PART_SEL, all of the following apply:

- • If the implementation detects the undefined value, an undefined RIS error occurs (MPAMF_ESR.ERRCODE is 0b1000). For more information, see Required error condition detection.
- • Whether the implementation detects the undefined value or not, one of the following IMPLEMENTATION DEFINED behaviors apply:


— The write updates the register using an implemented resource instance. The write is ignored (WI).

###### 7.2.7 Reads of MPAM MSC registers with other errors

RJMLNM If a read from an MPAM*IDR or an MPAMCFG_* register detects an error other than one of the following, then that

read returns an UNKNOWN value:

- • PARTID_SEL out-of-range error (MPAMF_ESR.ERRCODE is 0b0001).
- • Undefined RIS error (MPAMF_ESR.ERRCODE is 0b1000).


###### 7.2.8 Writes to MPAM MSC registers with other errors

RXMLDN For the partition selected by MPAMCFG_PART_SEL.{RIS, PARTID_SEL}, if a write to an MPAM*IDR or an MPAMCFG_* register detects an error other than one of the following, then the resulting control settings in that register are UNKNOWN:

- • PARTID_SEL out-of-range error (MPAMF_ESR.ERRCODE is 0b0001).
- • Undefined RIS error (MPAMF_ESR.ERRCODE is 0b1000).


###### 7.2.9 Writes to MSMON_CFG_MON_SEL.MON_SEL

IXWJHR When a write attempts to update MSMON_CFG_MON_SEL.MON_SEL with an out-of-range value, an MSC implementation often cannot detect the out-of-range value because different monitor types might have a different number of supported monitor instances. If RIS is also supported, then an update to MSMON_CFG_MON_SEL.RIS can change which monitor types are implemented and how many monitor instances of each type are supported. This is because each monitor type can have a different number of monitor instances that depends on the resource instance.

IGZDFS When a write attempts to update MSMON_CFG_MON_SEL.MON_SEL, an implementation might check the value before updating the register in the following cases:

- • RIS is not supported and only a single monitor type is implemented.
- • RIS is not supported and all implemented monitor types have exactly the same number of monitor instances.
- • RIS is supported and all monitor types of all resource instances support exactly the same number of monitor instances.
- • RIS is supported, different resource instances support a different number of monitor instances, and all monitor types of each resource instance support exactly the same number of monitor instances. When checking the MSMON_CFG_MON_SEL.MON_SE value, the RIS value must be used to determine the maximum number of monitor instances.


RVMTBP If a write attempts to update MSMON_CFG_MON_SEL.MON_SEL with an out-of-range value, it is

IMPLEMENTATION DEFINED whether:

- • The value is not checked and all of the following apply to the out-of-range value:

— The value is written to MSMON_CFG_MON_SEL.MON_SEL.

— When the value is later used to access an MSMON_CFG_*, MSMON_* monitor, or MSMON_* capture register, an out-of-range monitor selector error (MPAMF_ESR.ERRCODE is 0b0101) might occur.

- • The new value is checked and all of the following apply to the out-of-range value: The value is not written to MSMON_CFG_MON_SEL.MON_SEL.


— An out-of-range monitor selector error (MPAMF_ESR.ERRCODE is 0b0101) occurs. For more information, see Required error condition detection.

###### 7.2.10 Reading MPAM MSC monitor registers when MSMON_CFG_MON_SEL.MON_SEL is out-of-range

RWXKWK If MSMON_CFG_MON_SEL.MON_SEL is an out-of-range monitor instance selector, then when reading any MSMON_* register other than MSMON_CFG_MON_SEL, all of the following apply:

- • If the implementation detects the out-of-range selector, then an out-of-range monitor selector error occurs (MPAMF_ESR.ERRCODE is 0b0101).
- • Whether the implementation detects the out-of-range selector or not, the value read from the register is


UNKNOWN.

###### 7.2.11 Writing MPAM MSC monitor registers when MSMON_CFG_MON_SEL.MON_SEL is out-of-range

RMGXDK If MSMON_CFG_MON_SEL.MON_SEL is out-of-range monitor instance selector, then when writing any MSMON_* register other than MSMON_CFG_MON_SEL, all of the following apply:

- • If the implementation detects the out-of-range selector, then an out-of-range monitor selector error occurs (MPAMF_ESR.ERRCODE is 0b0101). For more information, see Required error condition detection.
- • Whether the implementation detects the out-of-range value or not, one of the following IMPLEMENTATION DEFINED behaviors apply: The write updates the register using an UNKNOWN in-range monitor instance selector.


— The write is ignored (WI).

###### 7.2.12 Attempting to write an undefined RIS to MSMON_CFG_MON_SEL.RIS

RHJRZC If RIS is supported and a write attempts to update MSMON_CFG_MON_SEL.RIS with an undefined value, then one of

the following IMPLEMENTATION DEFINED behaviors are permitted:

- • MSMON_CFG_MON_SEL.RIS is updated without the implementation checking the new value. If the updated MSMON_CFG_MON_SEL.RIS is used to access a monitor register, an undefined RIS monitor selector error can occur (MPAMF_ESR.ERRCODE is 0b1010).
- • The implementation checks the new value. If the undefined value is detected, all of the following apply: MSMON_CFG_MON_SEL is not updated.


— An undefined RIS monitor selector error occurs (MPAMF_ESR.ERRCODE is 0b1010).

###### 7.2.13 Reading other MPAM MSC monitor registers when MSMON_CFG_MON_SEL.RIS contains an undefined RISvalue

RSQTSJ If RIS is supported and MSMON_CFG_MON_SEL.RIS is undefined, then when reading any MSMON_* register other than MSMON_CFG_MON_SEL, all of the following apply:

- • If the implementation detects the undefined value, then an undefined RIS monitor selector error occurs (MPAMF_ESR.ERRCODE is 0b1010). For more information, see Required error condition detection.
- • Whether the implementation detects the undefined value or not, the value read from the register is UNKNOWN.


- 7.2.14 Writing other MPAM MSC monitor registers when MSMON_CFG_MON_SEL.RIS contains an undefined RIS value


RVZWYW If RIS is supported and MSMON_CFG_MON_SEL.RIS is undefined, then when writing any MSMON_* register other than MSMON_CFG_MON_SEL, all of the following apply:

- • If the implementation detects the undefined value, then an undefined RIS monitor selector error occurs (MPAMF_ESR.ERRCODE is 0b1010). For more information, see Required error condition detection.
- • Whether the implementation detects the undefined value or not, one of the following IMPLEMENTATION DEFINED behaviors apply:


— The write updates the register using an implemented resource instance.

— The write is ignored (WI).

- 7.3 Optionality of error detection and reporting


###### 7.3 Optionality of error detection and reporting

RPNPQQ If all of the following apply to an error condition, then an MPAM MSC implementation is required to detect and report that error:

- • At least one feature that can cause the error condition is supported.
- • The MSC is designed such that the error condition can occur.
- • Detection of the error condition is required. See Required error condition detection.


RSJFMB If all of the following apply, MPAMF_IDR.HAS_ESR is permitted to be 0:

- • FEAT_MPAMv0p1, or from FEAT_MPAMv1p1, is implemented.
- • No error conditions meet the criteria in PNPQQ.


IXFGVF If MPAMF_IDR.HAS_ESR is 1, then MPAMF_ESR and MPAMF_ECR must be implemented.

IRLNXX If FEAT_MPAMv1p0 is implemented and it is not possible to detect error conditions, then MPAMF_ESR and MPAMF_ECR must be RAZ/WI.

- 7.3.1 Required error condition detection RVYLZC For the purposes of QBQJG and PFNBV, below, all of the following are MPAM MSC selectors:


- • PARTIDs.
- • PMGs.
- • Monitor selectors.
- • In FEAT_MPAMv0p1 and from FEAT_MPAMv1p1, RIS values.


RQBQJG All of the following apply to the MPAM MSC implementation of a selector:

- • The implemented selector interface is permitted to be narrower than the full width specified by the architecture.
- • The MSC internal selector implementation is permitted to be smaller than the MSC selector interface.
- • Bits beyond the implemented width of any selector are permitted to be silently truncated without any requirement to detect or report bits that are non-zero at the time of truncation.


RPFNBV For a selector field width of n bits, if an MPAM MSC implementation supports a width smaller than 0 to 2n-1, then it is required that values within the field size that are not valid in the implementation be detected and reported. Detection can be applied after silently truncating to the supported bit-width.

RCZZZK If PARTID narrowing is supported, then the Unexpected INTERNAL error condition must be detected and reported (MPAMF_ESR.ERRCODE is 0b0111).

### Chapter 8MPAM MSC Interrupts

This chapter contains the following sections:

- • Interrupt sources.
- • Error interrupt.
- • Overflow interrupt.
- • Message signaled interrupts.


- 8.1 Interrupt sources


###### 8.1 Interrupt sources

RHVNKJ An MPAM MSC can generate the following interrupts:

- • MPAM error interrupt. The conditions that generate an MPAM error interrupt in an MSC are described in MPAM MSC errors. The responses of an MPAM MSC to an MPAM error interrupt are described in Error interrupt.
- • MPAM overflow interrupt. The conditions that generate an MPAM overflow interrupt and the responses of an MPAM MSC to that interrupt are described in Overflow interrupt.


RTMMMD A four-space MSC is permitted to use either a Non-secure MPAM error interrupt or a Secure MPAM error interrupt for signaling an error associated with the Realm or Root PARTID spaces.

###### 8.2 Error interrupt

IPRBTB If MPAMF_ECR.INTEN is 1, MPAM error interrupts can be signaled to software and are recorded in MPAMF_ESR.

RQDSPY If MPAMF_ECR.INTEN is 1 and any of the following apply to an MPAM MSC, an error interrupt cannot be signaled:

- • The MSC does not support any MPAM feature that causes that error.
- • The MSC is designed so that the error cannot occur.
- • The MSC is permitted to not detect that error. See Required error condition detection.


IFKFDD If an MPAM MSC does not support detection of any error condition listed in Error conditions in accessing memory-mapped registers, both MPAMF_ESR and MPAMF_ECR must be RAZ/WI.

IHKSZC If an MPAM MSC supports both Secure and Non-secure address spaces, MPAMF_ESR and MPAMF_ECR have a Secure instance and a Non-secure instance. The Secure registers control and record Secure MPAM error interrupts, and the Non-secure registers control and record Non-secure MPAM error interrupts.

RPPJSM An MPAM MSC can implement the MPAM error interrupt as one of the following.

- • A level-sensitive interrupt. See Level-sensitive interrupts.
- • An edge-triggered interrupt. See Edge-triggered interrupts.


INPSYC When determining whether to implement level-sensitive or edge-triggered interrupts, the following should be considered:

- • Arm recommends that the MPAM error interrupt be implemented as a level-sensitive interrupt.
- • An MPAM error interrupt request from an MPAM MSC resource monitor generates an FIQ or IRQ exception using an IMPLEMENTATION DEFINED mechanism.
- • Arm recommends that the following MPAM error interrupt signals are implemented:

— Secure MPAM error interrupt. Non-secure MPAM error interrupt.

- • Arm recommends that the following apply to MPAM error interrupt requests:


— The request is translated into an MPAM_ERR_IRQ signal, making it observable to external devices. If the MPAM MSC is integrated into a MPAM PE, then for a connection to an Arm Generic Interrupt Controller (GIC), it is IMPLEMENTATION DEFINED whether it connects as a Private Peripheral Interrupt (PPI) or a Locality-specific Peripheral Interrupt (LPI), for that PE.

— If the MPAM MSC is not integrated into a MPAM PE, then for a connection to an Arm GIC, it is IMPLEMENTATION DEFINED whether it connects as a System Peripheral Interrupt (SPI) or Locality-Specific Peripheral Interrupt (LPI).

See the Arm Generic Interrupt Controller Architecture Specification, GIC architecture version 3.0 and version 4.0 for information about PPIs, LPIs, and SPIs.

- 8.2.1 Level-sensitive interrupts RGLPPK When level-sensitive interrupts are implemented, the interrupt is active when MPAMF_ESR.ERRCODE is non-zero.


IVWFQN Software can control a level-sensitive interrupt as follows:

- • The interrupt can be activated by setting MPAMF_ESR.ERRCODE to a non-zero value.
- • The interrupt can be cleared by setting MPAMF_ESR.ERRCODE to 0b0000.


RNRGQD If an MPAM MSC implements level-sensitive interrupts, then that MSC cannot use an MSI to signal an MPAM error interrupt.

###### 8.2.2 Edge-triggered interrupts

RKVGWS When edge-triggered interrupts are implemented, the interrupt is generated when MPAMF_ESR.ERRCODE is written due to an error.

IXDMPW All of the following apply to MPAMF_ESR.ERRCODE when edge-triggered interrupts are implemented:

- • Software cannot generate the interrupt by setting MPAMF_ESR.ERRCODE to a non-zero value.
- • An interrupt service routine does not need to clear the interrupt.


RKVHTZ If an MPAM error interrupt is signaled using an MSI, then an MPAM MSC must implement edge-triggered interrupts. For more information on MSIs, see MSI error interrupt support and configuration.

###### 8.3 Overflow interrupt

IZKBDJ A monitor can overflow, particularly monitors that accumulate counts. IDKBKJ If a monitor can overflow, then bits in MSMON_CFG_*_CTL control the overflow behavior. See Overflow status bit. RZRDMM MPAM MSC support of an MPAM overflow interrupt is OPTIONAL.

###### Note

If an MPAM MSC has monitors that can overflow, Arm recommends that the MPAM overflow interrupt be implemented.

RYLCVK For a monitor instance, if all of the following apply, then an overflow interrupt is generated:

- 1. The control register OFLOW_STATUS flag is 0.
- 2. The control register OFLOW_INTR flag is 1.
- 3. The monitor overflows, setting the control register OFLOW_STATUS flag to 1.


INKMKK If an MPAM MSC supports both Secure and Non-secure address spaces, all of the following apply to overflow interrupts:

- • Secure instances of MSMON_CFG_*_CTL.OFLOW_INTR control whether a Secure MPAM overflow interrupt is generated when the corresponding Secure counter instance overflows.
- • Non-secure instances of MSMON_CFG_*_CTL.OFLOW_INTR control whether a Non-secure MPAM overflow interrupt is generated when the corresponding Non-secure counter instance overflows.


IYXRCB When an MPAM MSC supports the MPAM overflow interrupt, the following should be considered:

- • It is IMPLEMENTATION DEFINED whether an MPAM overflow interrupt request is handled as an FIQ or IRQ exception.
- • Arm recommends that the following MPAM overflow interrupt signals are implemented:

— Secure MPAM overflow interrupt.

— Non-secure MPAM overflow interrupt.

- • Arm recommends that the following apply to MPAM overflow interrupt requests:


— The request is translated into an MPAM_OF_IRQ signal, making it observable to external devices. If the MPAM MSC is integrated into a MPAM PE, then for a connection to an Arm Generic Interrupt Controller (GIC), it is IMPLEMENTATION DEFINED whether it connects as a Private Peripheral Interrupt (PPI) or a Locality-specific Peripheral Interrupt (LPI), for that PE.

— If the MPAM MSC is not integrated into a MPAM PE, then for a connection to an Arm GIC, it is IMPLEMENTATION DEFINED whether it connects as a System Peripheral Interrupt (SPI) or Locality-Specific Peripheral Interrupt (LPI).

See the Arm Generic Interrupt Controller Architecture Specification, GIC architecture version 3.0 and version 4.0 for information about PPIs, LPIs, and SPIs.

IDLHTG The interrupt can be cleared by setting the OFLOW_STATUS field of all overflowed monitor instances MSMON_CFG_*_CTL to 0.

RYJCDR If an MPAM overflow interrupt is signaled using an MSI, then an MPAM MSC must implement edge-triggered interrupts.

###### 8.3.1 Monitor overflow status register

RYVSNK For all RIS values, the optional MSMON_OFLOW_SR register summarizes the MSMON_CFG_<type>_CTL monitor overflow status flags, OFLOW_STATUS and OFLOW_STATUS_L.

IFNNNV For each resource instance r, the MSMON_OFLOW_SR.RIS_PND<r> flag has one of the following values:

- • If 0, then all of the MSMON_CFG_<type>_CTL monitor overflow status flags for that resource instance are 0.
- • If 1, then at least one of the MSMON_CFG_<type>_CTL monitor overflow status flags for that resource instance is 1.


IMDRBP Each monitor type flag MSMON_OFLOW_SR.{CSU_OFLOW_PND, MBWU_OFLOW_PND} has one of the following values:

- • If 0, then all of the MSMON_CFG_<type>_CTL monitor overflow status flags for that monitor are 0.
- • If 1, then at least one of the MSMON_CFG_<type>_CTL monitor overflow status flags for that monitor is 1.


IJWFJQ MSMON_OFLOW_SR is read-only. For each resource instance r, the MSMON_OFLOW_SR.RIS_PND<r> flag is

reset when all MSMON_CFG_<type>_CTL monitor overflow status flags for that resource instance are all reset to 0.

IMGSLC MPAMF_MSMON_IDR.HAS_OFLOW_SR indicates the presence of MSMON_OFLOW_SR.

###### 8.3.2 Monitor type overflow status bitmap registers

IWHVRY For a RIS value, when a monitor overflow interrupt is generated, the monitor overflow interrupt service routine must do all of the following:

- • Set MSMON_CFG_MON_SEL.MON_SEL to a monitor instance.
- • Read the corresponding MSMON_CFG_<type>_CTL to check whether at least one of the monitor overflow status flags in that register is set.


ICLRQV An implementation might have many monitor instances to scan for overflows, even after checking the RIS values and monitor types in MSMON_OFLOW_SR.

IJBPLV To accelerate scanning of many monitor instances, an optional overflow status register can be implemented for each monitor type that shows the overflow status flags in a bitmap of 32 monitor instances.

IKRZRB The following select the optional overflow status bitmap register:

- • MSMON_CFG_MON_SEL.RIS selects the resource instance.
- • MSMON_CFG_MON_SEL.MON_SEL AND 0xFFE0 selects the lowest contiguous 32 monitor instances reported in the bitmap register.


IRVCZS For the CSU monitor type, the CSU overflow status bitmap register is MSMON_CSU_OFSR. The presence of this register is discoverable in MPAMF_CSUMON_IDR.HAS_OFSR.

###### Note

In most implementations, CSU monitor instances cannot overflow because the maximum value in MSMON_CSU is known at design time and will fit within the architectural maximum of MSMON_CSU. In such an implementation, there will be no CSU monitor instance overflows and implementation of MSMON_CSU_OFSR is not necessary.

IKFXRF For the MBWU monitor type, the MBWU overflow status bitmap register is MSMON_MBWU_OFSR. The presence of this register is discoverable in MPAMF_MBWUMON_IDR.HAS_OFSR.

- 8.4 Message signaled interrupts


###### 8.4 Message signaled interrupts

IKCZMQ Message signaled interrupts (MSIs) are signaled by a memory write instead of using dedicated interrupt lines. MSIs are

usually directed at an interrupt translation service.

- 8.4.1 MSI error interrupt support and configuration IMPQKS MSI support for MPAM error interrupts is indicated by MPAMF_IDR.{HAS_ERR_MSI, HAS_ESR}.

IFTSRD MSIs error interrupts are configured using the following registers:

- • MPAMF_ERR_MSI_ADDR_L.
- • MPAMF_ERR_MSI_ADDR_H.
- • MPAMF_ERR_MSI_ATTR.
- • MPAMF_ERR_MSI_DATA.
- • MPAMF_ERR_MSI_MPAM.


IMQKYH An instance of the MSI error interrupt configuration registers exists in each of the Secure PA space and the Non-secure PA space. The registers in each address space configure the MSI writes due to errors generated in the partitions configured and controlled by the MPAMCFG_* and MPAMF_* registers in that address space.

IQDCFD Errors can be generated by the following requests:

- • Requests that have the PARTID space selected when MPAM_NS is 0 are signaled as Secure errors using the MSI write information from the MPAMF_ERR_MSI_* registers in the Secure address space.
- • Requests that have the PARTID space selected when MPAM_NS is 1 are signaled as Non-secure errors using the MSI write information from the MPAMF_ERR_MSI_* registers in the Non-secure address space.


- 8.4.2 Support for MSI writes to signal overflow interrupts IGCHPF MSI support for monitor overflow interrupts is indicated by MPAMF_IDR.{HAS_ERR_MSI, HAS_ESR}.


IRHKHY MSIs overflow interrupts are configured using the following registers:

- • MSMON_OFLOW_MSI_ADDR_L
- • MSMON_OFLOW_MSI_ADDR_H.
- • MSMON_OFLOW_MSI_ATTR.
- • MSMON_OFLOW_MSI_DATA.
- • MSMON_OFLOW_MSI_MPAM.


ICZLSW An instance of the MSI overflow interrupt configuration registers exists in each of the Secure PA space and the Non-secure PA space. The registers in each address space configure the MSI writes due to overflow events generated by monitors accessible in that address space.

### Chapter 9MPAM MSC Memory-mapped Registers

This chapter contains the following sections:

- • Overview of MMRs.
- • MPAM feature pages.
- • The fixed-point fractional format.
- • Summary of memory-mapped registers.
- • Memory-mapped ID register description.
- • Memory-mapped partitioning configuration registers.
- • Memory-mapped monitoring configuration registers.
- • Memory-mapped error control and status registers.
- • Memory-mapped translation configuration registers.


- 9.1 Overview of MMRs RJLYJJ The MPAM MSC behavior is discovered and configured using memory-mapped registers (MMRs).


IHFVNW All MPAM MMRs are located in the MPAM MSC feature pages. See MPAM feature page.

IYDPKM An MPAM MSC feature page is located using device information, which might be provided by firmware data such as a device tree or ACPI. See MSC Firmware Data.

RMBDJF MPAM MMR accesses of 32 bits are required to be single-copy atomic.

RLVCGQ MPAM MMR accesses greater than 32 bits are not required to complete atomically.

ITKRTY The architecture defines the following types of MPAM MMRs:

- • Registers with the MPAMF prefix are ID registers used to specify MPAM parameters and options.
- • Registers with the MPAMCFG prefix are registers used to configure MPAM resource controls.
- • Registers with the MSMON prefix are resource monitor registers used to configure and report MPAM monitor information.
- • The following registers report MPAM MSC programming errors:


— MPAMF_ESR reports the MPAM MSC error status.

— MPAMF_ECR controls whether MPAM MSC error interrupts are reported.

- 9.1.1 Determining presence and location of MMRs IYLVGQ All of the following apply to MPAMF_IDR:


- • The register is located at MPAM feature page offset 0x0000.
- • The register indicates which MPAM MSC resource controls and other MPAM ID registers are supported, and the maximum PARTID, PMG, and RIS that are supported.
- • The register indicates whether the MSC has MPAM monitors.


IHRCXD If MPAMF_IDR indicates support of other MPAM ID registers, those registers identify the implemented values of architecturally-defined parameters associated with a class of MPAM resource control.

ICHZMR If MPAMF_IDR indicates support of MPAM monitors, MPAMF_MSMON_IDR indicates which monitor types are implemented, and those registers identify the implemented values of architecturally-defined parameters associated with the type of MPAM monitor.

ITCKVR For a supported MPAM MSC MMR, the address is an architecturally-defined offset in the MPAM feature page. See Summary of memory-mapped registers.

- 9.1.2 Configuring partition resource controls IGTFCY For all PARTIDs used by an MPAM MSC, resource controls are configured as follows:


- 1. Obtain exclusive access to the memory block containing the MSC partitioning configuration registers. See Memory-mapped partitioning configuration registers.
- 2. Write the PARTID to MPAMCFG_PART_SEL.PARTID_SEL.
- 3. For MPAMCFG_PART_SEL.PARTID_SEL, configure the resource controls in all relevant MPAMCFG_* registers.
- 4. Repeat steps 2 and 3 to configure the resource controls of all additional PARTIDs.
- 5. Release exclusive access to the memory block containing the MSC partitioning configuration registers.


Repeat this procedure for each MSC.

IVQHNZ Software that configures the MPAM MSC resource controls must obtain exclusive access to the memory block containing those registers to prevent other software from accessing the registers until that lock is released. Software must also obtain exclusive access to the memory block containing the MPAM MSC resource controls before reading those registers because reads require updating MPAMCFG_PART_SEL.PARTID_SEL with the appropriate PARTID to access the corresponding configuration registers.

RDMKKT For each supported PARTID space, separate locks can be obtained to access the MPAM MSC resource controls in those spaces.

- 9.1.3 Configuring memory system monitors IWBFZM For an MPAM MSC, the supported monitors are configured as follows:


- 1. Obtain exclusive access to the memory block containing the MSC monitor configuration registers. See Memory-mapped monitoring configuration registers.
- 2. Update MSMON_CFG_MON_SEL.MON_SEL to select the monitor instance to configure.
- 3. For each monitor type supported by the MSC, configure the MSMON_CFG_<type>* registers selected by MSMON_CFG_MON_SEL.MON_SEL.
- 4. Repeat steps 2 and 3 to configure the monitor types of all additional monitor instances.
- 5. Release exclusive access to the memory block containing the MSC monitor configuration registers.


Repeat this procedure for each MSC.

IKDWTY Software that configures the MPAM MSC monitor instance must obtain exclusive access to the memory block containing those registers to prevent other software from accessing the registers until that lock is released. Software must also obtain exclusive access to the memory block containing the MPAM MSC monitor configuration, monitor, and capture registers before reading those registers because reads require updating MSMON_CFG_MON_SEL.MON_SEL with the appropriate monitor instance to access the corresponding configuration registers.

RVTFQK The monitor configuration registers can have a separate lock or share the same lock as the partitioning configuration registers.

RCTGKD For each supported PARTID space, separate locks can be obtained to access the MPAM MSC monitor configuration, monitor, and capture registers in those spaces.

- 9.1.4 Minimum required memory-mapped registers IYXBVB If an MSC implements MPAM, then the following registers are required:


- • MPAMF_IDR.
- • MPAMF_AIDR.
- • MPAMF_IIDR.
- • If the Secure address space is supported, then MPAMF_SIDR.


IRLVBC If an MSC supports resource controls, then the following registers are required:

• MPAMCFG_PART_SEL.

IFCVVT If an MSC supports resource monitors, then the following registers are required:

- • MPAMF_MSMON_IDR.
- • MSMON_CFG_MON_SEL.


IXQNDV If an MSC supports error detection, then the following registers are required:

• MPAMF_ESR. • MPAMF_ECR.

IGMJXX If an MPAM MMR is not required, then that MMR is optional and implemented only if the resource control or monitor that the register supports is also implemented. For examples showing MPAMF_*IDR registers in implementations with few MPAM functions, see Examples of partial MPAM implementations.

- 9.1.5 IMPLEMENTATION DEFINED MMRs and reserved feature page locations RRDMVJ IMPLEMENTATION DEFINED MPAM MMRs are permitted in the MPAM feature page at offset 0x3000 and greater.


RKNNBY All locations in the MPAM feature page at offsets less than the maximum MPAM feature page offset defined in Permitted truncation of an MPAM feature page are reserved. All of the following apply to locations within the reserved address range:

- • Reads and writes of unallocated locations are reserved accesses.
- • Reads and writes of unimplemented optional register locations are reserved accesses.
- • Reads and writes of locations that are greater than the implemented register width defined by the corresponding ID register, but within the range allocated by the architecture, are reserved accesses.
- • Reads of WO locations are reserved accesses.
- • Writes to RO locations are reserved accesses.


IXTTGY Reserved accesses must be implemented as RAZ/WI.

###### Note

Software must not rely on this behavior because reserved values might change in future architecture versions.

###### 9.2 MPAM feature pages

RZPWLC For each supported PA space, an MSC has an MPAM feature page defining the implemented registers used by that address space.

RDYDFT For each supported PA space, Table 4-2 shows the MPAM feature page base address.

Table 9-1 MPAM feature page base address for each PA space

MPAM feature page base address Description

PA space

Non-Secure MPAMF_BASE_ns Base address of the MPAM feature page in the Non-secure PA space. Secure MPAMF_BASE_s Base address of the MPAM feature page in the Secure PA space. Realm MPAMF_BASE_rl Base address of the MPAM feature page in the Realm PA space. Root MPAMF_BASE_rt Base address of the MPAM feature page in the Root PA space.

RHXLMF The MPAM MSC feature pages in each supported PA space are not required to have the same base address.

###### Note

Arm recommends that the MPAM feature page base addresses in each supported PA space are sufficiently different so that the address ranges do not overlap.

RQCYPH For a PA space, an MPAM MSC feature page is an address block with all of the following properties:

- • The base address must be aligned on a 4 KB boundary.
- • The feature page must be completely contained within a single 64 KB aligned memory block.
- • All MPAM MSC MMRs are contained within that feature page.


RFLWNK If the MSC implements non-MPAM MMRs and those MMRs are trapped to a hypervisor, then those MMRs are permitted within the feature page.

- 9.2.1 Non-secure, Secure, Realm, and Root feature pages RGTKCX The Non-secure MPAM feature page must always exist.


RDFBBW If the MSC supports the Secure address space, the Secure MPAM feature page must exist.

RFYTQL If FEAT_RME is implemented, then the Realm and Root MPAM feature pages must exist. See Four-space MSC.

IZQHYS MPAMF_SIDR is only accessible from the Secure address space.

RKZBWG If a read-only MPAM MMR has identical content across Security states, it is permitted to share that register across the corresponding feature pages. The following table lists the MPAM MMRs that are permitted to be shared between feature pages.

Table 9-2 MPAM MMRs that can be shared between feature pages

MPAMF_IDR MPAMF_IMPL_IDR MPAMF_CPOR_IDR MPAMF_CCAP_IDR MPAMF_MBW_IDR MPAMF_PRI_IDR MPAMF_PARTID_NRW_IDR MPAMF_MSMON_IDR MPAMF_CSUMON_IDR

MPAMF_MBWUMON_IDR

IPJHRL The following table lists the two read-only MSC MMRs must have the same contents in the Non-secure, Secure, Realm, and Root feature pages.

Table 9-3 MPAM MMRs that must have the same contents in all feature pages

MPAMF_IIDR MPAMF_AIDR

IVNXPN The following table lists the MPAM MMRs that cannot be shared between the Non-secure, Secure, Realm, and Root feature pages.

Table 9-4 MPAM MMRs that cannot be shared between feature pages

MPAMF_ECR MPAMCFG_PART_SEL MSMON_CFG_MON_SEL MPAMF_ESR MPAMCFG_MBW_MAX MSMON_CFG_CSU_CTL

MPAMCFG_MBW_MIN MSMON_CFG_CSU_FLT MPAMCFG_CMAX MPAMCFG_MBW_PBM MSMON_CSU MPAMCFG_CPBM MPAMCFG_MBW_PROP MSMON_CSU_CAPTURE

MPAMCFG_MBW_WINWD MSMON_CFG_MBWU_CTL MPAMCFG_PRI MSMON_CFG_MBWU_FLT MPAMCFG_INTPARTID MSMON_MBWU

MSMON_MBWU_CAPTURE

###### 9.2.2 Accesses to unimplemented MMR locations

IBKMVD If an access is made to an MPAM MMR other than MPAMCFG_MBW_PBM and MPAMCFG_CPBM, and that MMR is not implemented in a feature page, then that access must be treated as a reserved MPAM feature page location. See IMPLEMENTATION DEFINED memory-mapped registers and reserved feature page locations. If MPAMCFG_MBW_PBM and MPAMCFG_CPBM are not implemented, then see Permitted truncation of an MPAM feature page.

- 9.2.3 Permitted truncation of an MPAM feature page IQGXFF An MPAM feature page can be shortened in the following two cases:


- • If MPAMCFG_MBW_PBM is not implemented, then the MPAM feature page maximum offset is 0x01FFF.
- • If MPAMCFG_MBW_PBM and MPAMCFG_CPBM are not implemented, then the MPAM feature page maximum offset is 0x00FFF.


###### 9.3 The fixed-point fractional format

IKMFYJ Software writes fractions to fractional control fields as 16-bit unsigned fixed-point values. There are no integer bits and 16 fraction bits.

IFXMWV It is permitted to implement fractional control fields as fewer than 16 bits, by truncating least significant bits and implementing the truncated bits as RAZ/WI.

DZBLDQ The implemented width is w. The programmed value is v.

ISJMHV v is the lower bound of a range, v to v + 2-w, and the target percentage lies somewhere within that range. An implementation is permitted to use any value within that range. Depending on the use of the fraction value, the best choice of value within the range could be the center, the smallest end, or the greatest end. Arm recommends that maximum controls use the value at the greatest end of the range, v + 2-w, and that minimum controls use the value at the smallest end, v.

IYSWWB Table 9-5, Table 9-6, and Table 9-7 show, for some example target percentages, the programmed value, effective value, and lower and upper bounds of the range v + 2-w, for three different implemented widths w. The values in the tables apply to a maximum-usage resource control.

Table 9-5 16-bit field width

Target Percentage 16 bits Programmed value Effective value Lower bound Upper bound

25% 0x3FFF 0x4000 25% 25.0015% 33% 0x5479 0x547A 32.9987% 33.0002% 50% 0x7FFF 0x8000 50% 50.0015% 80% 0xCCCB 0xCCCC 79.9988% 80.0003% 100% 0xFFFF Forced to 100% 100% -

Table 9-6 12-bit field width

Target Percentage 12 bits Programmed value Effective value Lower bound Upper bound

25% 0x3FF 0x400 25% 25.0244% 33% 0x546 0x547 32.9834% 33.0078% 50% 0x7FF 0x800 50% 50.0244% 80% 0xCCB 0xCCC 79.98050% 80.0049% 100% 0xFFF Forced to 100% 100% -

Table 9-7 8-bit field width

Target Percentage 8 bits Programmed value Effective value Lower bound Upper bound

25% 0x3F 0x40 25% 25.3906% 33% 0x53 0x54 32.8125% 33.2031% 50% 0x7F 0x80 50% 50.3906% 80% 0xCB 0xCC 79.6875% 80.0781% 100% 0xFF Forced to 100% 100% -

###### 9.4 Summary of memory-mapped registers

IXMNMW Table 9-8 lists the external MPAM registers in order of register offset.

Table 9-8 Index of external MPAM registers ordered by offset

Register Offset Length Description, see:

MPAMF_IDR 0x0000 64 MPAMF_IDR MPAMF_SIDR 0x0008 32 MPAMF_SIDR MPAMF_IIDR 0x0018 32 MPAMF_IIDR MPAMF_AIDR 0x0020 32 MPAMF_AIDR MPAMF_IMPL_IDR 0x0028 32 MPAMF_IMPL_IDR MPAMF_CPOR_IDR 0x0030 32 MPAMF_CPOR_IDR MPAMF_CCAP_IDR 0x0038 32 MPAMF_CCAP_IDR MPAMF_MBW_IDR 0x0040 32 MPAMF_MBW_IDR MPAMF_PRI_IDR 0x0048 32 MPAMF_PRI_IDR MPAMF_PARTID_NRW_IDR 0x0050 32 MPAMF_PARTID_NRW_IDR MPAMF_MSMON_IDR 0x0080 32 MPAMF_MSMON_IDR MPAMF_CSUMON_IDR 0x0088 32 MPAMF_CSUMON_IDR MPAMF_MBWUMON_IDR 0x0090 32 MPAMF_MBWUMON_IDR MPAMF_ERR_MSI_MPAM 0x00DC 32 MPAMF_ERR_MSI_MPAM MPAMF_ERR_MSI_ADDR_L 0x00E0 32 MPAMF_ERR_MSI_ADDR_L MPAMF_ERR_MSI_ADDR_H 0x00E4 32 MPAMF_ERR_MSI_ADDR_H MPAMF_ERR_MSI_DATA 0x00E8 32 MPAMF_ERR_MSI_DATA MPAMF_ERR_MSI_ATTR 0x00EC 32 MPAMF_ERR_MSI_ATTR MPAMF_ECR 0x00F0 32 MPAMF_ECR MPAMF_ESR 0x00F8 64 MPAMF_ESR MPAMCFG_PART_SEL 0x0100 32 MPAMCFG_PART_SEL MPAMCFG_CMAX 0x0108 32 MPAMCFG_CMAX MPAMCFG_CMIN 0x0110 32 MPAMCFG_CMIN MPAMCFG_CASSOC 0x0118 32 MPAMCFG_CASSOC MPAMCFG_MBW_MIN 0x0200 32 MPAMCFG_MBW_MIN MPAMCFG_MBW_MAX 0x0208 32 MPAMCFG_MBW_MAX MPAMCFG_MBW_WINWD 0x0220 32 MPAMCFG_MBW_WINWD MPAMCFG_EN 0x0300 32 MPAMCFG_EN MPAMCFG_DIS 0x0310 32 MPAMCFG_DIS MPAMCFG_EN_FLAGS 0x0320 32 MPAMCFG_EN_FLAGS MPAMCFG_PRI 0x0400 32 MPAMCFG_PRI MPAMCFG_MBW_PROP 0x0500 32 MPAMCFG_MBW_PROP MPAMCFG_INTPARTID 0x0600 32 MPAMCFG_INTPARTID MSMON_CFG_MON_SEL 0x0800 32 MSMON_CFG_MON_SEL

Register Offset Length Description, see:

MSMON_CAPT_EVNT 0x0808 32 MSMON_CAPT_EVNT MSMON_CFG_CSU_FLT 0x0810 32 MSMON_CFG_CSU_FLT MSMON_CFG_CSU_CTL 0x0818 32 MSMON_CFG_CSU_CTL MSMON_CFG_MBWU_FLT 0x0820 32 MSMON_CFG_MBWU_FLT MSMON_CFG_MBWU_CTL 0x0828 32 MSMON_CFG_MBWU_CTL MSMON_CSU 0x0840 32 MSMON_CSU MSMON_CSU_CAPTURE 0x0848 32 MSMON_CSU_CAPTURE MSMON_CSU_OFSR 0x0858 32 MSMON_CSU_OFSR MSMON_MBWU 0x0860 32 MSMON_MBWU MSMON_MBWU_CAPTURE 0x0868 32 MSMON_MBWU_CAPTURE MSMON_MBWU_L 0x0880 64 MSMON_MBWU_L MSMON_MBWU_L_CAPTURE 0x0890 64 MSMON_MBWU_L_CAPTURE MSMON_MBWU_OFSR 0x0898 32 MSMON_MBWU_OFSR MSMON_OFLOW_MSI_MPAM 0x08DC 32 MSMON_OFLOW_MSI_MPAM MSMON_OFLOW_MSI_ADDR_L 0x08E0 32 MSMON_OFLOW_MSI_ADDR_L MSMON_OFLOW_MSI_ADDR_H 0x08E4 32 MSMON_OFLOW_MSI_ADDR_H MSMON_OFLOW_MSI_DATA 0x08E8 32 MSMON_OFLOW_MSI_DATA MSMON_OFLOW_MSI_ATTR 0x08EC 32 MSMON_OFLOW_MSI_ATTR MSMON_OFLOW_SR 0x08F0 32 MSMON_OFLOW_SR MPAMCFG_CPBM<n> 0x1000 32 MPAMCFG_CPBM<n> MPAMCFG_MBW_PBM<n> 0x2000 32 MPAMCFG_MBW_PBM<n> MPAMF_IN_TL_IDR 0x3000 32 MPAMF_IN_TL_IDR MPAMCFG_IN_TL 0x3008 32 MPAMCFG_IN_TL MPAMCFG_IN_TL_BASE 0x3010 32 MPAMCFG_IN_TL_BASE MPAMCFG_IN_TL_MASK 0x3018 32 MPAMCFG_IN_TL_MASK MPAMF_OUT_TL_IDR 0x3200 32 MPAMF_OUT_TL_IDR MPAMCFG_OUT_TL 0x3208 32 MPAMCFG_OUT_TL MPAMCFG_OUT_TL_BASE 0x3210 32 MPAMCFG_OUT_TL_BASE MPAMCFG_OUT_TL_MASK 0x3218 32 MPAMCFG_OUT_TL_MASK

###### 9.5 Memory-mapped ID register descriptionThis section lists the external ID registers.

###### 9.5.1 MPAMF_AIDR, MPAM Architecture Identification RegisterThe MPAMF_AIDR characteristics are:

Purpose

Identifies the version of the MPAM architecture that this MSC implements.

Configuration

The power domain of MPAMF_AIDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv1p0 is implemented or FEAT_MPAMv0p1 is implemented. Otherwise, direct accesses to MPAMF_AIDR are RES0.

Attributes

MPAMF_AIDR is a 32-bit register.

Field descriptions

31 8 7 4 3 0

| | | |
|---|---|---|
|RES0|ArchMajorRev|ArchMinorRev|


Bits [31:8]

Reserved, RES0.

ArchMajorRev, bits [7:4]

Major revision of the MPAM architecture implemented by the MSC. This field has an IMPLEMENTATION DEFINED value. This table shows the only valid combinations of MPAM version numbers in an MSC. FORCE_NS functionality is only available in MPAM v0.1.

ArchMajorRev ArchMinorRev MPAMv Available

- 0 0 None.

- 0 1 v0.1 MPAMv1.0 + MPAMv1.1 + FORCE_NS


- 1 0 v1.0 MPAMv1.0

- 1 1 v1.1 MPAMv1.0 + MPAMv1.1 - FORCE_NS


Use of MPAMv0.1 in MSCs is restricted to limited circumstances. The MSC must be able to initiate requests in the Secure address space which have MPAM PARTID forced to the Non-secure space with that forcing not controllable or observable by the software that configures the device for Secure requests. Please contact Arm before setting MPAMF_AIDR to report MPAMv0.1.

Access to this field is RO.

ArchMinorRev, bits [3:0]

Minor revision of the MPAM architecture implemented by the MSC.

This field has an IMPLEMENTATION DEFINED value. See the table in the description of the ArchMajorRev field in this register. Access to this field is RO.

Accessing MPAMF_AIDR

This register is within the MPAM feature page memory frames. If FEAT_MPAM is implemented, the following statements apply:

- • In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.
- • MPAMF_AIDR must be readable from the Non-secure and Secure MPAM feature pages.
- • MPAMF_AIDR must have the same contents in the Secure and Non-secure MPAM feature pages.


If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_AIDR must be readable from the Secure, Non-secure, Root, and Realm MPAM feature pages.
- • MPAMF_AIDR must have the same contents in the Secure, Non-secure, Root, and Realm MPAM feature pages.


MPAMF_AIDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0020 MPAMF_AIDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0020 MPAMF_AIDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0020 MPAMF_AIDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0020 MPAMF_AIDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

- 9.5.2 MPAMF_CCAP_IDR, MPAM Features Cache Capacity Partitioning ID register The MPAMF_CCAP_IDR characteristics are:


Purpose

Indicates the number of fractional bits in MPAMCFG_CMAX.CMAX. MPAMF_CCAP_IDR_s indicates the number of fractional bits in the Secure instance of MPAMCFG_CMAX. MPAMF_CCAP_IDR_ns indicates the number of fractional bits in the Non-secure instance of MPAMCFG_CMAX. MPAMF_CCAP_IDR_rt indicates the number of fractional bits in the Root cache capacity control settings register field, MPAMCFG_CMAX.CMAX. MPAMF_CCAP_IDR_rl indicates the number of fractional bits in the Realm cache capacity control settings register field, MPAMCFG_CMAX.CMAX.

When MPAMF_IDR.HAS_RIS is 1, some fields in this register give information for the resource instance selected by MPAMCFG_PART_SEL.RIS. The description of every field that is affected by MPAMCFG_PART_SEL.RIS has information within the field description.

Configuration

The power domain of MPAMF_CCAP_IDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented) and MPAMF_IDR.HAS_CCAP_PART == ‘1’. Otherwise, direct accesses to MPAMF_CCAP_IDR are RES0.

Attributes

MPAMF_CCAP_IDR is a 32-bit register.

Field descriptions

31 30 29 28 27 13 12 8 7 6 5 0

| | | | | | | | |
|---|---|---|---|---|---|---|---|
| | | | |RES0|CASSOC_WD|RES0|CMAX_WD|


HAS_CMA X_SOFTL

HAS_CASSOC HAS_CMIN

IM NO_CMAX

HAS_CMAX_SOFTLIM, bit [31] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Has soft limiting selection field in MPAMCFG_CMAX. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CMAX_SOFTLIM Meaning

- 0b0 If MPAMCFG_CMAX is implemented, it has no SOFTLIM field and the maximum capacity is controlled with a hard limit.

- 0b1 If MPAMCFG_CMAX is implemented, that register has a SOFTLIMIT field to select between hard or soft limiting to the CMAX parameter.


If RIS is implemented, this field indicates selectable limiting for the cache maximum capacity control for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

NO_CMAX, bit [30] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Does not have CMAX partitioning. The value of this field is an IMPLEMENTATION DEFINED choice of:

NO_CMAX Meaning

0b0 MPAMCFG_CMAX is implemented. 0b1 MPAMCFG_CMAX is not implemented.

If RIS is implemented, this field indicates the absence of a cache maximum capacity partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_CMIN, bit [29] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Has cache minimum capacity partitioning. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CMIN Meaning

0b0 MPAMCFG_CMIN is not implemented. 0b1 MPAMCFG_CMIN is implemented.

If RIS is implemented, this field indicates the presence of a cache minimum capacity partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_CASSOC, bit [28] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Has cache maximum associativity partitioning. The value of this field is an IMPLEMENTATION DEFINED choice of:

###### HAS_CASSOC Meaning

0b0 MPAMCFG_CASSOC is not implemented. 0b1 MPAMCFG_CASSOC is implemented.

If RIS is implemented, this field indicates the presence of a cache maximum associativity partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

Bits [27:13]

Reserved, RES0.

CASSOC_WD, bits [12:8] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Number of fractional bits implemented in the cache associativity partitioning control, MPAMCFG_CASSOC.CASSOC, of this MSC. See MPAMCFG_CASSOC.

This field has an IMPLEMENTATION DEFINED value. If RIS is implemented, this field indicates the number of fractional bits in the cache capacity partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Otherwise:

Reserved, RES0.

Bits [7:6]

Reserved, RES0.

CMAX_WD, bits [5:0]

Number of fractional bits implemented in the cache capacity partitioning control, MPAMCFG_CMAX.CMAX, of this device. See MPAMCFG_CMAX.

This field must contain a value from 1 to 16, inclusive. This field has an IMPLEMENTATION DEFINED value. If RIS is implemented, this field indicates the number of fractional bits in the cache capacity partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Accessing MPAMF_CCAP_IDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_CCAP_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_CCAP_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:


- • MPAMF_CCAP_IDR_s is permitted to have either the same or different contents to MPAMF_CCAP_IDR_ns, MPAMF_CCAP_IDR_rt, or MPAMF_CCAP_IDR_rl.


- • MPAMF_CCAP_IDR_ns is permitted to have either the same or different contents to MPAMF_CCAP_IDR_rt or MPAMF_CCAP_IDR_rl.
- • MPAMF_CCAP_IDR_rt is permitted to have either the same or different contents to MPAMF_CCAP_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_CCAP_IDR_s), Non-secure (MPAMF_CCAP_IDR_ns), Root (MPAMF_CCAP_IDR_rt), and Realm (MPAMF_CCAP_IDR_rl) MPAM feature pages.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• When MPAMF_IDR.HAS_RIS is 1, MPAMF_CCAP_IDR shows the configuration of cache capacity partitioning for the cache resource instance selected by MPAMCFG_PART_SEL.RIS. Fields that mention RIS in their field descriptions have values that track the implemented properties of the resource instance. Fields that do not mention RIS are constant across all resource instances.

MPAMF_CCAP_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0038 MPAMF_CCAP_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0038 MPAMF_CCAP_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0038 MPAMF_CCAP_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0038 MPAMF_CCAP_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.3 MPAMF_CPOR_IDR, MPAM Features Cache Portion Partitioning ID registerThe MPAMF_CPOR_IDR characteristics are:

Purpose

Indicates the number of bits in MPAMCFG_CPBM<n>. MPAMF_CPOR_IDR_s indicates the number of bits in the Secure instance of MPAMCFG_CPBM<n>. MPAMF_CPOR_IDR_ns indicates the number of bits in the Non-secure instance of MPAMCFG_CPBM<n>. MPAMF_CPOR_IDR_rt indicates the number of bits in the Root instance of MPAMCFG_CPBM<n>. MPAMF_CPOR_IDR_rl indicates the number of bits in the Realm instance of MPAMCFG_CPBM<n>. When MPAMF_IDR.HAS_RIS is 1, some fields in this register give information for the resource instance selector, MPAMCFG_PART_SEL.RIS. The description of every field that is affected by MPAMCFG_PART_SEL.RIS has information within the field description.

Configuration

The power domain of MPAMF_CPOR_IDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv1p0 is implemented or FEAT_MPAMv0p1 is implemented) and MPAMF_IDR.HAS_CPOR_PART == ‘1’. Otherwise, direct accesses to MPAMF_CPOR_IDR are RES0.

Attributes

MPAMF_CPOR_IDR is a 32-bit register.

Field descriptions

31 16 15 0

| | |
|---|---|
|RES0|CPBM_WD|


Bits [31:16]

Reserved, RES0.

CPBM_WD, bits [15:0]

Number of bits in the cache portion partitioning bit map of this device. See MPAMCFG_CPBM<n>. This field must contain a value from 1 to 32768, inclusive. Values greater than 32 require a group of 32-bit registers to access the CPBM, up to 1024 if CPBM_WD is the largest value. This field has an IMPLEMENTATION DEFINED value. If RIS is implemented, this field indicates the number bits in the cache portion bitmap for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Accessing MPAMF_CPOR_IDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_CPOR_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_CPOR_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:


- • MPAMF_CPOR_IDR_s is permitted to have either the same or different contents to MPAMF_CPOR_IDR_ns, MPAMF_CPOR_IDR_rt, or MPAMF_CPOR_IDR_rl.
- • MPAMF_CPOR_IDR_ns is permitted to have either the same or different contents to MPAMF_CPOR_IDR_rt or MPAMF_CPOR_IDR_rl.
- • MPAMF_CPOR_IDR_rt is permitted to have either the same or different contents to MPAMF_CPOR_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_CPOR_IDR_s), Non-secure (MPAMF_CPOR_IDR_ns), Root (MPAMF_CPOR_IDR_rt), and Realm (MPAMF_CPOR_IDR_rl) MPAM feature pages.


If any one of these features is implemented:

- • FEAT_MPAMv0p1
- • FEAT_MPAMv1p1


then, the following statements apply:

• When MPAMF_IDR.HAS_RIS is 1, MPAMF_CPOR_IDR shows the configuration of cache portion partitioning for the cache resource instance selected by MPAMCFG_PART_SEL.RIS. Fields that mention RIS in their field descriptions have values that track the implemented properties of the resource instance. Fields that do not mention RIS are constant across all resource instances.

MPAMF_CPOR_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0030 MPAMF_CPOR_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0030 MPAMF_CPOR_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0030 MPAMF_CPOR_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0030 MPAMF_CPOR_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.4 MPAMF_CSUMON_IDR, MPAM Features Cache Storage Usage Monitoring ID registerThe MPAMF_CSUMON_IDR characteristics are:

Purpose

Indicates the number of cache storage usage monitor instances and other properties of the CSU monitoring.

Configuration

The power domain of MPAMF_CSUMON_IDR is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MPAMF_CSUMON_IDR_s indicates the number and properties of Secure cache storage usage monitoring.
- • MPAMF_CSUMON_IDR_ns indicates the number and properties of Non-secure cache storage usage monitoring.
- • If FEAT_RME is implemented the following statements also apply:
- • MPAMF_CSUMON_IDR_rt indicates the number and properties of Root cache storage usage monitoring.
- • MPAMF_CSUMON_IDR_rl indicates the number and properties of Realm cache storage usage monitoring.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, fields that mention RIS must reflect the properties of the resource instance currently selected by MPAMCFG_PART_SEL.RIS. Fields that do not mention RIS are constant across all resource instances.

The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv1p0) || IsFeatureImplemented(FEAT_MPAMv0p1)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_CSU == ‘1’). Otherwise, direct accesses to MPAMF_CSUMON_IDR are RES0.

Attributes

MPAMF_CSUMON_IDR is a 32-bit register.

Field descriptions

###### When FEAT_MPAMv1p0 is implemented or FEAT_MPAMv0p1 is implemented:

31 30 29 28 27 26 25 24 23 22 21 16 15 0

| | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | |RES0|NUM_MON|


HAS_CAP TURE

NO_MATCH_PMG NO_MATCH_PARTID

CSU_RO HAS_XCL RES0 HAS_OFLOW_LNKG

HAS_OFLOW_CAPT HAS_CEVNT_OFLW

HAS_OFSR

HAS_CAPTURE, bit [31]

The implementation supports copying an MSMON_CSU to the corresponding MSMON_CSU_CAPTURE on a capture event.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CAPTURE Meaning

- 0b0 MSMON_CSU_CAPTURE is not implemented and there is no support for capture events in the CSU monitor.

- 0b1 The MSMON_CSU_CAPTURE register is implemented and the CSU monitor supports the capture event behavior.


If MPAMF_IDR.HAS_RIS is 1, this field reports that CSU monitor capture is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

CSU_RO, bit [30]

The implementation of MSMON_CSU is read-only. The value of this field is an IMPLEMENTATION DEFINED choice of:

CSU_RO Meaning

- 0b0 MSMON_CSU is read/write.

- 0b1 MSMON_CSU is read-only.


If MPAMF_IDR.HAS_RIS is 1, this field reports that the MSMON_CSU monitor register is read-only for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

HAS_XCL, bit [29] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Has filtering to exclude clean data and implements the MSMON_CFG_CSU_FLT.XCL field. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_XCL Meaning

- 0b0 MSMON_CFG_CSU_FLT does not implement the XCL field.

- 0b1 MSMON_CFG_CSU_FLT implements the XCL field to exclude counting data in the clean state in the monitor instance.


If MPAMF_IDR.HAS_RIS is 1, this field reports that the MSMON_CFG_CSU_FLT.XCL field is implemented in the CSU monitor instances for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

Bit [28]

Reserved, RES0.

HAS_OFLOW_LNKG, bit [27] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Supports MSMON_CFG_CSU_CTL.OFLOW_LNKG field to control how overflow on an instance affects other monitor instances in this MSC.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_OFLOW_LNKG Meaning

0b0 Does not support CSU overflow linkage. 0b1 Supports CSU overflow linkage and the MSMON_CFG_CSU_CTL.OFLOW_LNKG

field.

If MPAMF_IDR.HAS_RIS is 1, this field reports that MSMON_CFG_CSU_CTL.OFLOW_LNKG is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_OFSR, bit [26] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

The CSU monitor overflow status bitmap register, MSMON_CSU_OFSR, is implemented. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_OFSR Meaning

- 0b0 MSMON_CSU_OFSR register is not implemented.

- 0b1 MSMON_CSU_OFSR register is implemented.


If MPAMF_IDR.HAS_RIS is 1, this field reports that the CSU monitor overflow status bitmap register is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_CEVNT_OFLW, bit [25] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Supports MSMON_CFG_CSU_CTL.CEVNT_OFLW field which can enable the CSU monitor instance to perform overflow behaviors on a capture event.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CEVNT_OFLW Meaning

0b0 Does not support MSMON_CFG_CSU_CTL.CEVNT_OFLW. 0b1 Supports MSMON_CFG_CSU_CTL.CEVNT_OFLW.

If MPAMF_IDR.HAS_RIS is 1, this field reports that MSMON_CFG_CSU_CTL.CEVNT_OFLW is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_OFLOW_CAPT, bit [24] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Supports MSMON_CFG_CSU_CTL.OFLOW_CAPT field which can enable the CSU monitor instance to capture the monitor on an overflow.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_OFLOW_CAPT Meaning

0b0 Does not support MSMON_CFG_CSU_CTL.OFLOW_CAPT. 0b1 Supports MSMON_CFG_CSU_CTL.OFLOW_CAPT.

If MPAMF_IDR.HAS_RIS is 1, this field reports that MSMON_CFG_CSU_CTL.OFLOW_CAPT is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

NO_MATCH_PARTID, bit [23]

Indicates support for using PARTID identifiers for monitoring. The value of this field is an IMPLEMENTATION DEFINED choice of:

NO_MATCH_PARTID Meaning

- 0b0 Supports cache storage usage monitoring based on PARTID.

- 0b1 Does not support cache storage usage monitoring based on PARTID.


Access to this field is RO.

NO_MATCH_PMG, bit [22]

Indicates support for using PMG identifiers for monitoring. The value of this field is an IMPLEMENTATION DEFINED choice of:

NO_MATCH_PMG Meaning

- 0b0 Supports cache storage usage monitoring based on PMG.

- 0b1 Does not support cache storage usage monitoring based on PMG.


Access to this field is RO.

Bits [21:16]

Reserved, RES0.

NUM_MON, bits [15:0]

The number of cache storage usage monitor instances implemented. The largest MSMON_CFG_MON_SEL.MON_SEL value is NUM_MON minus 1. This field has an IMPLEMENTATION DEFINED value. If MPAMF_IDR.HAS_RIS is 1, this field reports the number of CSU monitor instances implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Accessing MPAMF_CSUMON_IDR

This register is within the MPAM feature page memory frames. If FEAT_MPAM is implemented, the following statements apply:

- • In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.
- • MPAMF_CSUMON_IDR must be readable from the Non-secure and Secure MPAM feature pages.
- • MPAMF_CSUMON_IDR is permitted to have the same contents when read from the Secure or Non-secure MPAM feature pages unless the register contents are different for the different versions, when MPAMF_CSUMON_IDR_s is permitted to have either the same or different contents to MPAMF_CSUMON_IDR_ns.
- • There must be separate registers in the Secure (MPAMF_CSUMON_IDR_s) and Non-secure (MPAMF_CSUMON_IDR_ns) MPAM feature pages.


If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_CSUMON_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_CSUMON_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:


- • MPAMF_CSUMON_IDR_s is permitted to have either the same or different contents to MPAMF_CSUMON_IDR_ns, MPAMF_CSUMON_IDR_rt, or MPAMF_CSUMON_IDR_rl.
- • MPAMF_CSUMON_IDR_ns is permitted to have either the same or different contents to MPAMF_CSUMON_IDR_rt or MPAMF_CSUMON_IDR_rl.


- • MPAMF_CSUMON_IDR_rt is permitted to have either the same or different contents to MPAMF_CSUMON_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_CSUMON_IDR_s), Non-secure (MPAMF_CSUMON_IDR_ns), Root (MPAMF_CSUMON_IDR_rt), and Realm (MPAMF_CSUMON_IDR_rl) MPAM feature pages.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

- • When MPAMF_IDR.HAS_RIS is 1, MPAMF_CSUMON_IDR shows the configuration of cache storage usage monitoring for the cache resource instance selected by MPAMCFG_PART_SEL.RIS. Fields that mention RIS in their field descriptions have values that track the implemented properties of the resource instance. Fields that do not mention RIS are constant across all resource instances.
- • Access to MPAMF_CSUMON_IDR is not affected by MSMON_CFG_MON_SEL.RIS.


MPAMF_CSUMON_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0088 MPAMF_CSUMON_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0088 MPAMF_CSUMON_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0088 MPAMF_CSUMON_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0088 MPAMF_CSUMON_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

- 9.5.5 MPAMF_IDR, MPAM Features Identification Register The MPAMF_IDR characteristics are:


Purpose

Indicates which memory partitioning and monitoring features are present on this MSC.

Configuration

The power domain of MPAMF_IDR is IMPLEMENTATION DEFINED If FEAT_MPAMv1p0 or FEAT_MPAMv0p1 is implemented, the following statements apply:

- • MPAMF_IDR_s indicates the MPAM features accessed from the Secure MPAM feature page.
- • MPAMF_IDR_ns indicates the MPAM features accessed from the Non-secure MPAM feature page.


If both FEAT_MPAMv1p0 or FEAT_MPAMv0p1 is implemented and FEAT_RME are implemented, the following statements apply:

- • MPAMF_IDR_rt indicates the MPAM features accessed from the Root MPAM feature page.
- • MPAMF_IDR_rl indicates the MPAM features accessed from the Realm MPAM feature page.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• When MPAMF_IDR.HAS_RIS is 1, some fields in this register give information for the resource instance selected by MPAMCFG_PART_SEL.RIS. The description of every field that is affected by MPAMCFG_PART_SEL.RIS has that information within the field description.

The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented. Otherwise, direct accesses to MPAMF_IDR are RES0.

Attributes

MPAMF_IDR is a:

- • 64-bit register when FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented.
- • 32-bit register otherwise.


Field descriptions

###### When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

63 60 59 56 55 47 46 45 44 43 42 41 40 39 38 37 36 35 33 32

| | | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|RES0|RIS_MAX|RES0| | | | | |SP4| | | | | |RES0| |


HAS_DEFAULT_PARTID HAS_OUT_TL HAS_IN_TL HAS_NFU HAS_ENDIS

HAS_RIS NO_IMPL_PART

NO_IMPL_MSMON HAS_EXTD_ESR

HAS_ESR HAS_ERR_MSI

31 30 29 28 27 26 25 24 23 16 15 0

| | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|
| | | |EXT| | | | |PMG_MAX|PARTID_MAX|


HAS_PAR TID_NRW

HAS_CCAP_PART HAS_CPOR_PART

HAS_MSMON HAS_IMPL_IDR

HAS_MBW_PART HAS_PRI_PART

Bits [63:60]

Reserved, RES0.

RIS_MAX, bits [59:56] When MPAMF_IDR.EXT == '1' and MPAMF_IDR.HAS_RIS == '1':

Maximum RIS value supported in MPAMCFG_PART_SEL. Must be 0b0000 if MPAMF_IDR.HAS_RIS

== 0. This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

Otherwise:

Reserved, RES0.

Bits [55:47]

Reserved, RES0.

HAS_DEFAULT_PARTID, bit [46] When MPAMF_IDR.EXT == '1':

Indicates support in an MSC for a default resource control configuration. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_DEFAULT_PARTID Meaning

- 0b0 The default resource control configuration is supported.

- 0b1 The default resource control configuration is not supported.


FEAT_MPAM_MSC_DCTRL implements the functionality identified by the value 0b1. Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_OUT_TL, bit [45] When MPAMF_IDR.EXT == '1':

Has egress translation. Indicates that this MSC supports egress MPAM information bundle manipulation. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_OUT_TL Meaning

- 0b0 MPAM information bundle manipulation for egress translation is not supported.

- 0b1 MPAM information bundle manipulation for egress translation is supported.


Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_IN_TL, bit [44] When MPAMF_IDR.EXT == '1':

Has ingress translation. Indicates that this MSC supports ingress MPAM information bundle manipulation. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_IN_TL Meaning

- 0b0 MPAM information bundle manipulation for ingress translation is not supported.

- 0b1 MPAM information bundle manipulation for ingress translation is supported.


Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_NFU, bit [43] When MPAMF_IDR.EXT == '1':

Has No Future Use field in MPAMCFG_DIS. Indicates that MPAMCFG_DIS.NFU is implemented. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_NFU Meaning

- 0b0 MPAMCFG_DIS.NFU is not implemented. A PARTID disabled through access to MPAMCFG_DIS must preserve the control settings of the disabled PARTID.

- 0b1 Implements MPAMCFG_DIS.NFU. A PARTID disabled with NFU as 1 may have its control settings forgotten.


If MPAMF_IDR.HAS_ENDIS is 0b0, this field must also be 0b0. This field must be the same in each instance of this register and for any value in MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_ENDIS, bit [42] When MPAMF_IDR.EXT == '1':

Has PARTID enable and disable. Indicates that this MSC supports PARTID disable and enable via MPAMCFG_DIS, MPAMCFG_EN and MPAMCFG_EN_FLAGS registers.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_ENDIS Meaning

- 0b0 Does not support PARTID enable and disable functionality, and MPAMCFG_EN, MPAMCFG_DIS and MPAMCFG_EN_FLAGS registers are not implemented.

- 0b1 Supports PARTID enable and disable through the MPAMCFG_EN, MPAMCFG_DIS and MPAMCFG_EN_FLAGS registers.


All three registers must be implemented when this field is 1, MPAMCFG_EN, MPAMCFG_DIS, and MPAMCFG_EN_FLAGS.

This field must be the same in each instance of this register and for any value in MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

SP4, bit [41] When MPAMF_IDR.EXT == '1' and FEAT_RME is implemented:

Indicates whether this MSC supports 4 PARTID spaces. The value of this field is an IMPLEMENTATION DEFINED choice of:

SP4 Meaning

- 0b0 This MSC supports two PARTID spaces.

- 0b1 This MSC supports four PARTID spaces.


This field must read the same in each instance of this register and for any value in MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_ERR_MSI, bit [40] When MPAMF_IDR.EXT == '1':

Has support for MSI writes to signal MPAM error interrupts. These registers are implemented: MPAMF_ERR_MSI_ADDR_L, MPAMF_ERR_MSI_ADDR_H, MPAMF_ERR_MSI_ATTR, MPAMF_ERR_MSI_DATA, and MPAMF_ERR_MSI_MPAM.

The value of this field is an IMPLEMENTATION DEFINED choice of:

###### HAS_ERR_MSI Meaning

- 0b0 MPAMF_ERR_MSI_ADDR_L, MPAMF_ERR_MSI_ADDR_H, MPAMF_ERR_MSI_ATTR, MPAMF_ERR_MSI_DATA, and MPAMF_ERR_MSI_MPAM registers are not implemented.

- 0b1 MPAMF_ERR_MSI_ADDR_L, MPAMF_ERR_MSI_ADDR_H, MPAMF_ERR_MSI_ATTR, MPAMF_ERR_MSI_DATA, and MPAMF_ERR_MSI_MPAM are implemented and can be used to generate writes to signal error interrupts.


If MPAMF_IDR.HAS_ESR is 0, this bit must also be 0. Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_ESR, bit [39] When MPAMF_IDR.EXT == '1':

MPAMF_ESR is implemented. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_ESR Meaning

- 0b0 MPAMF_ESR, MPAMF_ECR, and MPAM error handling are not implemented.

- 0b1 MPAMF_ESR, MPAMF_ECR, and MPAM error handling are implemented.


If an MSC cannot encounter any of the error conditions listed in ‘Errors in MSCs’ in Arm® Architecture Reference Manual Supplement, Memory System Resource Partitioning and Monitoring (MPAM), for Armv8-A (ARM DDI 0598), both the MPAMF_ESR and MPAMF_ECR must be RAZ/WI.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_EXTD_ESR, bit [38] When MPAMF_IDR.EXT == '1':

MPAMF_ESR is 64 bits. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_EXTD_ESR Meaning

- 0b0 MPAMF_ESR is 32 bits.

- 0b1 MPAMF_ESR is 64 bits.


When MPAMF_IDR.HAS_RIS and MPAMF_IDR.HAS_ESR, this field must be 1. Access to this field is RO.

Otherwise:

Reserved, RES0.

NO_IMPL_MSMON, bit [37] When MPAMF_IDR.EXT == '1' and MPAMF_IDR.HAS_IMPL_IDR == '1':

MPAMF_IMPL_IDR defines no IMPLEMENTATION DEFINED resource monitors. The value of this field is an IMPLEMENTATION DEFINED choice of:

NO_IMPL_MSMON Meaning

- 0b0 MPAMF_IMPL_IDR defines at least one IMPLEMENTATION DEFINED resource monitor.

- 0b1 MPAMF_IMPL_IDR does not define any IMPLEMENTATION DEFINED resource monitors.


If MPAMF_IDR.RIS is 1, this field indicates the presence of IMPLEMENTATION DEFINED resource monitors described in MPAMF_IMPL_IDR for the selected resource instance.

Access to this field is RO.

Otherwise:

Reserved, RES0.

NO_IMPL_PART, bit [36] When MPAMF_IDR.EXT == '1' and MPAMF_IDR.HAS_IMPL_IDR == '1':

MPAMF_IMPL_IDR defines no IMPLEMENTATION DEFINED resource controls. The value of this field is an IMPLEMENTATION DEFINED choice of:

NO_IMPL_PART Meaning

- 0b0 MPAMF_IMPL_IDR defines at least one IMPLEMENTATION DEFINED resource control.

- 0b1 MPAMF_IMPL_IDR does not define any IMPLEMENTATION DEFINED resource controls.


If MPAMF_IDR.RIS is 1, this field indicates the presence of IMPLEMENTATION DEFINED resource controls described in MPAMF_IMPL_IDR for the selected resource instance.

Access to this field is RO.

Otherwise:

Reserved, RES0.

Bits [35:33]

Reserved, RES0.

HAS_RIS, bit [32]

When MPAMF_IDR.EXT == '1':

Has resource instance selector. Indicates that MPAMCFG_PART_SEL contains the RIS field that selects a resource instance to control.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_RIS Meaning

- 0b0 MPAMCFG_PART_SEL does not implement the MPAMCFG_PART_SEL.RIS field or multiple resource instance support.

- 0b1 MPAMCFG_PART_SEL implements the MPAMCFG_PART_SEL.RIS field and MPAM resource instance numbers up to and including MPAMF_IDR.RIS_MAX.


Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_PARTID_NRW, bit [31]

Has PARTID Narrowing. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_PARTID_NRW Meaning

- 0b0 Does not have MPAMF_PARTID_NRW_IDR, MPAMCFG_INTPARTID, or intPARTID mapping support.

- 0b1 Supports the MPAMF_PARTID_NRW_IDR, MPAMCFG_INTPARTID registers.


Access to this field is RO.

HAS_MSMON, bit [30]

Has resource Monitors. Indicates whether this MSC has MPAM resource monitors. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_MSMON Meaning

- 0b0 Does not support MPAM resource monitoring by groups or MPAMF_MSMON_IDR.

- 0b1 Supports resource monitoring by matching a combination of PARTID and PMG. See MPAMF_MSMON_IDR.


Access to this field is RO.

HAS_IMPL_IDR, bit [29]

Has MPAMF_IMPL_IDR. Indicates whether this MSC has the IMPLEMENTATION SPECIFIC MPAM features register, MPAMF_IMPL_IDR.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_IMPL_IDR Meaning

0b0 Does not have MPAMF_IMPL_IDR. 0b1 Has MPAMF_IMPL_IDR.

Access to this field is RO.

EXT, bit [28]

Extended MPAMF_IDR. The value of this field is an IMPLEMENTATION DEFINED choice of:

EXT Meaning

0b0 MPAMF_IDR has no defined bits in [63:32]. The register is effectively 32 bits. 0b1 MPAMF_IDR has bits defined in [63:32]. The register is 64 bits.

Access to this field is RO.

HAS_PRI_PART, bit [27]

Has Priority Partitioning. Indicates that MPAM priority partitioning is implemented and MPAMF_PRI_IDR exists.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_PRI_PART Meaning

0b0 Does not support priority partitioning or have MPAMF_PRI_IDR. 0b1 Has priority partitioning and MPAMF_PRI_IDR.

If MPAMF_IDR.RIS is 1, this field indicates the presence of priority partitioning resource controls as described in MPAMF_PRI_IDR for the selected resource instance.

Access to this field is RO.

HAS_MBW_PART, bit [26]

Has Memory Bandwidth Partitioning. Indicates whether this MSC implements MPAM memory bandwidth partitioning and MPAMF_MBW_IDR.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_MBW_PART Meaning

- 0b0 Does not support memory bandwidth partitioning or have MPAMF_MBW_IDR register.

- 0b1 Has MPAMF_MBW_IDR register.


If MPAMF_IDR.RIS is 1, this field indicates the presence of memory bandwidth partitioning resource controls as described in MPAMF_MBW_IDR for the selected resource instance.

Access to this field is RO.

HAS_CPOR_PART, bit [25]

Has Cache Portion Partitioning. Indicates whether this MSC implements MPAM cache portion partitioning and MPAMF_CPOR_IDR.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CPOR_PART Meaning

- 0b0 Does not support cache portion partitioning or have MPAMF_CPOR_IDR or MPAMCFG_CPBM<n> registers.

- 0b1 Has MPAMF_CPOR_IDR and MPAMCFG_CPBM<n> registers.


If MPAMF_IDR.RIS is 1, this field indicates the presence of cache portion partitioning resource controls as described in MPAMF_CPOR_IDR for the selected resource instance.

Access to this field is RO.

HAS_CCAP_PART, bit [24]

Has Cache Capacity Partitioning. Indicates whether this MSC implements MPAM cache capacity partitioning and the MPAMF_CCAP_IDR and MPAMCFG_CMAX registers.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CCAP_PART Meaning

- 0b0 Does not support cache capacity partitioning or have MPAMF_CCAP_IDR and MPAMCFG_CMAX registers.

- 0b1 Has MPAMF_CCAP_IDR and MPAMCFG_CMAX registers.


If MPAMF_IDR.RIS is 1, this field indicates the presence of cache capacity partitioning resource controls as described in MPAMF_CCAP_IDR for the selected resource instance.

Access to this field is RO.

PMG_MAX, bits [23:16]

Maximum supported value of PMG. This field has an IMPLEMENTATION DEFINED value. The value of this field is permitted to vary between the instances of MPAMF_IDR, each reporting the maximum supported PMG value in the PARTID space associated with that instance.

In MPAMF_IDR_s, this field is permitted to report the maximum PMG value for the Non-secure PARTID space or for the Secure PARTID space. The maximum PMG value for the Secure PARTID space can be read from MPAMF_SIDR.PMG_MAX.

Access to this field is RO.

PARTID_MAX, bits [15:0]

Maximum supported value of PARTID. This field has an IMPLEMENTATION DEFINED value. The value of this field is permitted to vary between the instances of MPAMF_IDR, each reporting the maximum supported PARTID value in the PARTID space associated with that instance. In MPAMF_IDR_s, this field is permitted to report the maximum PARTID value for the Non-secure PARTID space or for the Secure PARTID space. The maximum PARTID value for the Secure PARTID space can be read from MPAMF_SIDR.PARTID_MAX. Access to this field is RO.

Otherwise:

31 30 29 28 27 26 25 24 23 16 15 0

| | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | |PMG_MAX|PARTID_MAX|


HAS_PAR TID_NRW

HAS_CCAP_PART HAS_CPOR_PART

HAS_MSMON HAS_IMPL_IDR

HAS_MBW_PART HAS_PRI_PART

RES0

HAS_PARTID_NRW, bit [31]

Has PARTID Narrowing. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_PARTID_NRW Meaning

- 0b0 Does not have MPAMF_PARTID_NRW_IDR, MPAMCFG_INTPARTID, or intPARTID mapping support.

- 0b1 Supports the MPAMF_PARTID_NRW_IDR, MPAMCFG_INTPARTID registers.


Access to this field is RO.

HAS_MSMON, bit [30]

Has resource Monitors. Indicates whether this MSC has MPAM resource monitors. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_MSMON Meaning

- 0b0 Does not support MPAM resource monitoring by groups or MPAMF_MSMON_IDR.


HAS_MSMON Meaning

- 0b1 Supports resource monitoring by matching a combination of PARTID and PMG. See MPAMF_MSMON_IDR.


Access to this field is RO.

HAS_IMPL_IDR, bit [29]

Has MPAMF_IMPL_IDR. Indicates whether this MSC has the IMPLEMENTATION SPECIFIC MPAM features register, MPAMF_IMPL_IDR.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_IMPL_IDR Meaning

- 0b0 Does not have MPAMF_IMPL_IDR.

- 0b1 Has MPAMF_IMPL_IDR.


Access to this field is RO.

Bit [28]

Reserved, RES0.

HAS_PRI_PART, bit [27]

Has Priority Partitioning. Indicates that MPAM priority partitioning is implemented and MPAMF_PRI_IDR exists.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_PRI_PART Meaning

- 0b0 Does not support priority partitioning or have MPAMF_PRI_IDR.

- 0b1 Has priority partitioning and MPAMF_PRI_IDR.


Access to this field is RO.

HAS_MBW_PART, bit [26]

Has Memory Bandwidth Partitioning. Indicates whether this MSC implements MPAM memory bandwidth partitioning and MPAMF_MBW_IDR.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_MBW_PART Meaning

- 0b0 Does not support memory bandwidth partitioning or have MPAMF_MBW_IDR register.


- 0b1 Has MPAMF_MBW_IDR register.


Access to this field is RO.

HAS_CPOR_PART, bit [25]

Has Cache Portion Partitioning. Indicates whether this MSC implements MPAM cache portion partitioning and MPAMF_CPOR_IDR.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CPOR_PART Meaning

- 0b0 Does not support cache portion partitioning or have MPAMF_CPOR_IDR or MPAMCFG_CPBM<n> registers.

- 0b1 Has MPAMF_CPOR_IDR and MPAMCFG_CPBM<n> registers.


Access to this field is RO.

HAS_CCAP_PART, bit [24]

Has Cache Capacity Partitioning. Indicates whether this MSC implements MPAM cache capacity partitioning and the MPAMF_CCAP_IDR and MPAMCFG_CMAX registers.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CCAP_PART Meaning

- 0b0 Does not support cache capacity partitioning or have MPAMF_CCAP_IDR and MPAMCFG_CMAX registers.

- 0b1 Has MPAMF_CCAP_IDR and MPAMCFG_CMAX registers.


Access to this field is RO.

PMG_MAX, bits [23:16]

Maximum supported value of PMG. This field has an IMPLEMENTATION DEFINED value. The value of this field is permitted to vary between the instances of MPAMF_IDR, each reporting the maximum supported PMG value in the PARTID space associated with that instance. In MPAMF_IDR_s, this field is permitted to report the maximum PMG value for the Non-secure PARTID space or for the Secure PARTID space. The maximum PMG value for the Secure PARTID space can be read from MPAMF_SIDR.PMG_MAX. Access to this field is RO.

PARTID_MAX, bits [15:0]

Maximum supported value of PARTID. This field has an IMPLEMENTATION DEFINED value. The value of this field is permitted to vary between the instances of MPAMF_IDR, each reporting the maximum supported PARTID value in the PARTID space associated with that instance.

In MPAMF_IDR_s, this field is permitted to report the maximum PARTID value for the Non-secure PARTID space or for the Secure PARTID space. The maximum PARTID value for the Secure PARTID space can be read from MPAMF_SIDR.PARTID_MAX.

Access to this field is RO.

Accessing MPAMF_IDR

This register is within the MPAM feature page memory frames. If FEAT_MPAM is implemented, the following statements apply:

- • In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.
- • MPAMF_IDR must be readable from the Non-secure and Secure MPAM feature pages.
- • MPAMF_IDR is permitted to have the same contents when read from the Secure or Non-secure MPAM feature pages unless the register contents are different for the different versions, when MPAMF_IDR_s is permitted to have either the same or different contents to MPAMF_IDR_ns.
- • There must be separate registers in the Secure (MPAMF_IDR_s) and Non-secure (MPAMF_IDR_ns) MPAM feature pages.


If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:

- • MPAMF_IDR_s is permitted to have either the same or different contents to MPAMF_IDR_ns, MPAMF_IDR_rt, or MPAMF_IDR_rl.
- • MPAMF_IDR_ns is permitted to have either the same or different contents to MPAMF_IDR_rt or MPAMF_IDR_rl.
- • MPAMF_IDR_rt is permitted to have either the same or different contents to MPAMF_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_IDR_s), Non-secure (MPAMF_IDR_ns), Root (MPAMF_IDR_rt), and Realm (MPAMF_IDR_rl) MPAM feature pages.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• When MPAMF_IDR.HAS_RIS is 1, MPAMF_IDR shows the configuration of MSC MPAM for the resource

instance selected by MPAMCFG_PART_SEL.RIS. Fields that mention RIS in their field descriptions have values that track the implemented properties of the resource instance. Fields that do not mention RIS are constant across all resource instances.

MPAMF_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0000 MPAMF_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0000 MPAMF_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0000 MPAMF_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0000 MPAMF_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.6 MPAMF_IIDR, MPAM Implementation Identification RegisterThe MPAMF_IIDR characteristics are:

Purpose

Uniquely identifies the MSC implementation by the combination of implementer, product ID, variant, and revision.

Configuration

The power domain of MPAMF_IIDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented. Otherwise, direct accesses to MPAMF_IIDR are RES0.

Attributes

MPAMF_IIDR is a 32-bit register.

Field descriptions

31 20 19 16 15 12 11 0

| | | | |
|---|---|---|---|
|ProductID|Variant|Revision|Implementer|


ProductID, bits [31:20]

The MSC implementer as identified in the MPAMF_IIDR.Implementer field must assure each product has a unique ProductID from any other with the same Implementer value.

This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

Variant, bits [19:16]

This field distinguishes product variants or major revisions of the product.

###### Note

Implementations of ProductID with differing software interfaces are expected to have different values in the MPAMF_IIDR.Variant field.

This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

Revision, bits [15:12]

This field distinguishes minor revisions of the product.

###### Note

This field is intended to differentiate product revisions that are minor changes and are largely software compatible with previous revisions.

This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

Implementer, bits [11:0]

Contains the JEP106 manufacturer’s identification code of the designer of the MPAM MSC. The code identifies the designer of the component, which might not be the same as the implementer of the device containing the component. Zero is not a valid JEP106 identification code, meaning a value of zero for MPAMF_IIDR indicates this register is not implemented. For an implementation designed by Arm, this field reads as 0x43B. This field has an IMPLEMENTATION DEFINED value. Bits [11:8] contain the JEP106 bank identifier of the designer minus 1. Bit 7 is RES0. Bits [6:0] contain bits [6:0] of the JEP106 manufacturer’s identification code of the designer. Access to this field is RO.

Accessing MPAMF_IIDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_IIDR must be readable from the Secure, Non-secure, Root, and Realm MPAM feature pages.
- • MPAMF_IIDR must have the same contents in the Secure, Non-secure, Root, and Realm MPAM feature pages.


MPAMF_IIDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0018 MPAMF_IIDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0018 MPAMF_IIDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0018 MPAMF_IIDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0018 MPAMF_IIDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.7 MPAMF_IMPL_IDR, MPAM Implementation-Specific Partitioning Feature Identification RegisterThe MPAMF_IMPL_IDR characteristics are:

Purpose

Indicates the implementation-defined partitioning and monitoring features and parameters of the MSC. MPAMF_IMPL_IDR_s indicates IMPLEMENTATION DEFINED partitioning and monitoring features accessed from the Secure MPAM feature page. MPAMF_IMPL_IDR_ns indicates those accessed from the Non-secure MPAM feature page. MPAMF_IMPL_IDR_rt indicates IMPLEMENTATION DEFINED partitioning and monitoring features accessed from the Root MPAM feature page. MPAMF_IMPL_IDR_rl indicates those accessed from the Realm MPAM feature page. If MPAMF_IDR.HAS_RIS is 1, this register gives the implementation-specific features and parameters of the resource instance selected by MPAMCFG_PART_SEL.RIS for any features that are specific to the resource.

Configuration

The power domain of MPAMF_IMPL_IDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM is implemented and MPAMF_IDR.HAS_IMPL_IDR == ‘1’. Otherwise, direct accesses to MPAMF_IMPL_IDR are RES0.

Attributes

MPAMF_IMPL_IDR is a 32-bit register.

Field descriptions

31 0

Bits [31:0]

IMPLFEAT, bits [31:0] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

All 32 bits of this register are available to be used as the implementer sees fit to indicate the presence of IMPLEMENTATION DEFINED MPAM features in this MSC and to give additional implementation-specific read-only information about the parameters of implementation-specific MPAM features to software. If RIS is implemented, this register indicates the implementation-specific features and parameters of the resource instance selected by MPAMCFG_PART_SEL.RIS. This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

Otherwise:

All 32 bits of this register are available to be used as the implementer sees fit to indicate the presence of IMPLEMENTATION DEFINED MPAM features in this MSC and to give additional implementation-specific read-only information about the parameters of implementation-specific MPAM features to software.

Accessing MPAMF_IMPL_IDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.


- • MPAMF_IMPL_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_IMPL_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:

- • MPAMF_IMPL_IDR_s is permitted to have either the same or different contents to MPAMF_IMPL_IDR_ns, MPAMF_IMPL_IDR_rt, or MPAMF_IMPL_IDR_rl.
- • MPAMF_IMPL_IDR_ns is permitted to have either the same or different contents to MPAMF_IMPL_IDR_rt or MPAMF_IMPL_IDR_rl.
- • MPAMF_IMPL_IDR_rt is permitted to have either the same or different contents to MPAMF_IMPL_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_IMPL_IDR_s), Non-secure (MPAMF_IMPL_IDR_ns), Root (MPAMF_IMPL_IDR_rt), and Realm (MPAMF_IMPL_IDR_rl) MPAM feature pages.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• When MPAMF_IDR.HAS_RIS is 1, MPAMF_IMPL_IDR shows the configuration of implementation-specific features for the resource instance selected by MPAMCFG_PART_SEL.RIS. Fields that mention RIS in their field descriptions have values that track the implemented properties of the resource instance. Fields that do not mention RIS are constant across all resource instances.

MPAMF_IMPL_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0028 MPAMF_IMPL_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0028 MPAMF_IMPL_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0028 MPAMF_IMPL_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0028 MPAMF_IMPL_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.8 MPAMF_MBW_IDR, MPAM Memory Bandwidth Partitioning Identification RegisterThe MPAMF_MBW_IDR characteristics are:

Purpose

Indicates which MPAM bandwidth partitioning features are present on this MSC.

MPAMF_MBW_IDR_s indicates bandwidth partitioning features accessed from the Secure MPAM feature page. MPAMF_MBW_IDR_ns indicates bandwidth partitioning features accessed from the Non-secure MPAM feature page. MPAMF_MBW_IDR_rt indicates bandwidth partitioning features accessed from the Root MPAM feature page. MPAMF_MBW_IDR_rl indicates bandwidth partitioning features accessed from the Realm MPAM feature page.

When MPAMF_IDR.HAS_RIS is 1, some fields in this register give information for the resource instance selected by MPAMCFG_PART_SEL.RIS. The description of every field that is affected by MPAMCFG_PART_SEL.RIS has that information within the field description.

Configuration

The power domain of MPAMF_MBW_IDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM is implemented and MPAMF_IDR.HAS_MBW_PART == ‘1’. Otherwise, direct accesses to MPAMF_MBW_IDR are RES0.

Attributes

MPAMF_MBW_IDR is a 32-bit register.

Field descriptions

31 29 28 16 15 14 13 12 11 10 9 8 7 6 5 0

| | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|
|RES0|BWPBM_WD| | | | | | | |RES0|BWA_WD|


RES0 WINDWR HAS_PROP

MAX_LIM HAS_MIN

HAS_MAX HAS_PBM

Bits [31:29]

Reserved, RES0.

BWPBM_WD, bits [28:16]

Bandwidth portion bitmap width. The number of bandwidth portion bits in the MPAMCFG_MBW_PBM<n> register array. If MPAMF_MBW_IDR.HAS_PBM is 1, this field must contain a value from 1 to 4096, inclusive. Values greater than 32 require a group of 32-bit registers to access the BWPBM, up to 128 if BWPBM_WD is the largest value. If MPAMF_MBW_IDR.HAS_PBM is 0, this field must be ignored by software. This field has an IMPLEMENTATION DEFINED value. If RIS is implemented, this field indicates the width of the memory bandwidth portion bitmap partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Bit [15]

Reserved, RES0.

WINDWR, bit [14]

Indicates the bandwidth accounting period register is writable. The value of this field is an IMPLEMENTATION DEFINED choice of:

WINDWR Meaning

0b0 The bandwidth accounting period is readable from MPAMCFG_MBW_WINWD which might be

fixed or vary due to clock rate reconfiguration of the memory channel or memory controller. 0b1 The bandwidth accounting width is readable and writable per partition in

MPAMCFG_MBW_WINWD.

Access to this field is RO.

HAS_PROP, bit [13]

Indicates that this MSC implements proportional stride bandwidth partitioning and the MPAMCFG_MBW_PROP register can be accessed.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_PROP Meaning

- 0b0 There is no memory bandwidth proportional stride control and the MPAMCFG_MBW_PROP register is RES0.

- 0b1 The proportional stride memory bandwidth partitioning scheme is supported and the MPAMCFG_MBW_PROP register can be accessed.


If RIS is implemented, this field indicates the presence of the memory bandwidth proportional stride partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

HAS_PBM, bit [12]

Indicates that bandwidth portion partitioning is implemented and the MPAMCFG_MBW_PBM<n> register array can be accessed.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_PBM Meaning

0b0 There is no memory bandwidth portion control and the MPAMCFG_MBW_PBM<n> is RES0. 0b1 The memory bandwidth portion allocation scheme exists and the MPAMCFG_MBW_PBM<n>

register can be accessed.

If RIS is implemented, this field indicates the presence of the memory bandwidth portion partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

HAS_MAX, bit [11]

Indicates that this MSC implements maximum bandwidth partitioning and the MPAMCFG_MBW_MAX register can be accessed.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_MAX Meaning

- 0b0 There is no maximum memory bandwidth control and the MPAMCFG_MBW_MAX register is RES0.

- 0b1 The maximum memory bandwidth allocation scheme is supported and the MPAMCFG_MBW_MAX register can be accessed. The MPAMF_MBW_IDR.MAX_LIM and MPAMCFG_MBW_MAX.HARDLIM fields indicate which limit behaviors are implemented and enabled.


If RIS is implemented, this field indicates the presence of the maximum bandwidth partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

HAS_MIN, bit [10]

Indicates that this MSC implements minimum bandwidth partitioning and the MPAMCFG_MBW_MIN register can be accessed.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_MIN Meaning

0b0 There is no minimum memory bandwidth control and the MPAMCFG_MBW_MIN register is

RES0.

0b1 The minimum memory bandwidth allocation scheme is supported and the MPAMCFG_MBW_MIN register can be accessed.

If RIS is implemented, this field indicates the presence of the minimum bandwidth partitioning control for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

MAX_LIM, bits [9:8]

Implemented maximum bandwidth partitioning behaviors. The value of this field is an IMPLEMENTATION DEFINED choice of:

MAX_LIM Meaning

0b00 Both soft limit and hard limit behaviors are implemented. 0b01 Soft limit behavior is implemented, hard limit behavior is not implemented. 0b10 Hard limit behavior is implemented, soft limit behavior is not implemented.

If both soft limit and hard limit behaviors are implemented, MPAMCFG_MBW_MAX.HARDLIM selects which behavior is used.

If RIS is implemented, this field indicates the implemented maximum bandwidth limit partitioning behaviors for the resource instance selected by MPAMCFG_PART_SEL.RIS.

When MPAMF_MBW_IDR.HAS_MAX != ‘1’, access to this field is RES0.

Bits [7:6]

Reserved, RES0.

BWA_WD, bits [5:0]

Number of implemented bits in the bandwidth allocation fields: MIN, MAX, and STRIDE. See MPAMCFG_MBW_MIN, MPAMCFG_MBW_MAX, and MPAMCFG_MBW_PROP.

In any of these bandwidth allocation fields exist, this field must have a value from 1 to 16, inclusive. Otherwise, it is permitted to be 0.

This field has an IMPLEMENTATION DEFINED value. If RIS is implemented, this field indicates the number of implemented bits in the bandwidth allocation control fields for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Accessing MPAMF_MBW_IDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_MBW_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_MBW_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:

- • MPAMF_MBW_IDR_s is permitted to have either the same or different contents to MPAMF_MBW_IDR_ns, MPAMF_MBW_IDR_rt, or MPAMF_MBW_IDR_rl.
- • MPAMF_MBW_IDR_ns is permitted to have either the same or different contents to MPAMF_MBW_IDR_rt or MPAMF_MBW_IDR_rl.
- • MPAMF_MBW_IDR_rt is permitted to have either the same or different contents to MPAMF_MBW_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_MBW_IDR_s), Non-secure (MPAMF_MBW_IDR_ns), Root (MPAMF_MBW_IDR_rt), and Realm (MPAMF_MBW_IDR_rl) MPAM feature pages.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• When MPAMF_IDR.HAS_RIS is 1, MPAMF_MBW_IDR shows the configuration of memory bandwidth partitioning for the bandwidth resource instance selected by MPAMCFG_PART_SEL.RIS. Fields that mention RIS in their field descriptions have values that track the implemented properties of the resource instance. Fields that do not mention RIS are constant across all resource instances.

MPAMF_MBW_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0040 MPAMF_MBW_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0040 MPAMF_MBW_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0040 MPAMF_MBW_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0040 MPAMF_MBW_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.9 MPAMF_MBWUMON_IDR, MPAM Features Memory Bandwidth Usage Monitoring ID registerThe MPAMF_MBWUMON_IDR characteristics are:

Purpose

Indicates the number of memory bandwidth usage monitor instances implemented. This register also indicates several properties of MBWU monitoring, including whether the implementation supports capture, scaling, or long counters.

Configuration

The power domain of MPAMF_MBWUMON_IDR is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MPAMF_MBWUMON_IDR_s indicates the number and properties of Secure memory bandwidth usage monitoring.
- • MPAMF_MBWUMON_IDR_ns indicates the number and properties of Non-secure memory bandwidth usage monitoring.
- • If FEAT_RME is implemented the following statements also apply:
- • MPAMF_MBWUMON_IDR_rt indicates the number and properties of Root memory bandwidth usage monitoring.
- • MPAMF_MBWUMON_IDR_rl indicates the number and properties of Realm memory bandwidth usage monitoring.


If any one of these features is implemented:

- • FEAT_MPAMv0p1
- • FEAT_MPAMv1p1


then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, fields that mention RIS must reflect the properties of the resource instance currently selected by MPAMCFG_PART_SEL.RIS. Fields that do not mention RIS are constant across all resource instances.

The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv1p0) || IsFeatureImplemented(FEAT_MPAMv0p1)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_MBWU == ‘1’). Otherwise, direct accesses to MPAMF_MBWUMON_IDR are RES0.

Attributes

MPAMF_MBWUMON_IDR is a 32-bit register.

Field descriptions

###### When FEAT_MPAMv1p0 is implemented or FEAT_MPAMv0p1 is implemented:

31 30 29 28 27 26 25 24 23 22 21 20 16 15 0

| | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| | |LWD| | | | | | | | |SCALE|NUM_MON|


HAS_CAP TURE

RES0

NO_MATCH_PMG NO_MATCH_PARTID

HAS_LONG

HAS_RWBW HAS_OFLOW_LNKG

HAS_OFLOW_CAPT HAS_CEVNT_OFLW

HAS_OFSR

HAS_CAPTURE, bit [31]

The implementation supports copying an MSMON_MBWU to the corresponding MSMON_MBWU_CAPTURE on a capture event.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CAPTURE Meaning

- 0b0 MSMON_MBWU_CAPTURE is not implemented and there is no support for capture events in the MBWU monitor.

- 0b1 The MSMON_MBWU_CAPTURE register is implemented and the MBWU monitor supports the capture event behavior.


If MPAMF_IDR.HAS_RIS is 1, this field reports that MBWU monitor capture is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

If MPAMF_MBWUMON_IDR.HAS_LONG is 1, this also reports that MSMON_MBWU_L_CAPTURE is implemented.

Access to this field is RO.

HAS_LONG, bit [30] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Reports whether MSMON_MBWU_L is implemented. If HAS_CAPTURE is 1, reports whether MSMON_MBWU_L_CAPTURE is implemented. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_LONG Meaning

0b0 Does not implement MSMON_MBWU_L or MSMON_MBWU_L_CAPTURE. 0b1 Implements MSMON_MBWU_L. If HAS_CAPTURE == 1, MSMON_MBWU_L_CAPTURE

is also implemented.

If MPAMF_IDR.HAS_RIS is 1, this field reports that the long MBWU monitor is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

If MPAMF_MBWUMON_IDR.HAS_CAPTURE is 1, this also reports that MSMON_MBWU_L_CAPTURE is implemented.

Access to this field is RO.

Otherwise:

Reserved, RES0.

LWD, bit [29] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_MBWUMON_IDR.HAS_LONG == '1':

Long register VALUE width. The value of this field is an IMPLEMENTATION DEFINED choice of:

LWD Meaning

- 0b0 MSMON_MBWU_L has 44-bit VALUE field in bits [43:0]. Bits [62:44] are RES0. If HAS_LONG is 1 and MPAMF_MBWUMON_IDR.HAS_CAPTURE is 1, MSMON_MBWU_L_CAPTURE also has 44-bit VALUE field in bits [43:0].

- 0b1 MSMON_MBWU_L has 63-bit VALUE field in bits [62:0]. If MPAMF_MBWUMON_IDR.HAS_CAPTURE == 1, MSMON_MBWU_L_CAPTURE also has 63-bit VALUE field in bits [62:0].


If MPAMF_IDR.HAS_RIS is 1, this field reports the length of the MSMON_MBWU_L.VALUE field implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_RWBW, bit [28] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Read/write bandwidth selection is implemented in MSMON_CFG_MBWU_FLT. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_RWBW Meaning

0b0 Read/write bandwidth selection is not implemented. 0b1 Read/write bandwidth selection is implemented.

If MPAMF_IDR.HAS_RIS is 1, this field reports whether read/write bandwidth collection selection is available in MSMON_CFG_MBWU_FLT for resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_OFLOW_LNKG, bit [27] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Supports MSMON_CFG_MBWU_CTL.OFLOW_LNKG field to control how overflow on an instance affects other monitor instances in this MSC.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_OFLOW_LNKG Meaning

0b0 Does not support MBWU overflow linkage. 0b1 Supports MBWU overflow linkage and the

MSMON_CFG_MBWU_CTL.OFLOW_LNKG field.

If MPAMF_IDR.HAS_RIS is 1, this field reports that MSMON_CFG_MBWU_CTL.OFLOW_LNKG is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_OFSR, bit [26] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

The MBWU monitor overflow status bitmap register, MSMON_MBWU_OFSR, is implemented. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_OFSR Meaning

0b0 MSMON_MBWU_OFSR register is not implemented. 0b1 MSMON_MBWU_OFSR register is implemented.

If MPAMF_IDR.HAS_RIS is 1, this field reports that the MBWU monitor overflow status bitmap register is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_CEVNT_OFLW, bit [25] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Supports MSMON_CFG_MBWU_CTL.CEVNT_OFLW field which can enable the MBWU monitor instance to perform overflow behaviors on a capture event.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_CEVNT_OFLW Meaning

0b0 Does not support MSMON_CFG_MBWU_CTL.CEVNT_OFLW. 0b1 Supports MSMON_CFG_MBWU_CTL.CEVNT_OFLW.

If MPAMF_IDR.HAS_RIS is 1, this field reports that MSMON_CFG_MBWU_CTL.CEVNT_OFLW is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_OFLOW_CAPT, bit [24] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Supports MSMON_CFG_MBWU_CTL.OFLOW_CAPT field which can enable the MBWU monitor instance to capture the monitor on an overflow.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_OFLOW_CAPT Meaning

0b0 Does not support MSMON_CFG_MBWU_CTL.OFLOW_CAPT. 0b1 Supports MSMON_CFG_MBWU_CTL.OFLOW_CAPT.

If MPAMF_IDR.HAS_RIS is 1, this field reports that MSMON_CFG_MBWU_CTL.OFLOW_CAPT is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Otherwise:

Reserved, RES0.

NO_MATCH_PARTID, bit [23]

Indicates support for using PARTID identifiers for monitoring. The value of this field is an IMPLEMENTATION DEFINED choice of:

NO_MATCH_PARTID Meaning

- 0b0 Supports memory bandwidth usage monitoring based on PARTID.

- 0b1 Does not support memory bandwidth usage monitoring based on PARTID.


Access to this field is RO.

NO_MATCH_PMG, bit [22]

Indicates support for using PMG identifiers for monitoring. The value of this field is an IMPLEMENTATION DEFINED choice of:

NO_MATCH_PMG Meaning

- 0b0 Supports memory bandwidth usage monitoring based on PMG.

- 0b1 Does not support memory bandwidth usage monitoring based on PMG.


Access to this field is RO.

Bit [21]

Reserved, RES0.

SCALE, bits [20:16]

Scaling of MSMON_MBWU.VALUE in bits. If scaling is enabled by MSMON_CFG_MBWU_CTL.SCLEN, the byte count in the VALUE field has been shifted by SCALE bits to the right.

This field has an IMPLEMENTATION DEFINED value. If MPAMF_IDR.HAS_RIS is 1, this field reports the scale value for MSMON_MBWU.VALUE field for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

NUM_MON, bits [15:0]

The number of memory bandwidth usage monitor instances implemented. The largest monitor instance selector, MSMON_CFG_MON_SEL.MON_SEL, is NUM_MON minus 1.

This field has an IMPLEMENTATION DEFINED value. If MPAMF_IDR.HAS_RIS is 1, this field reports the number of MBWU monitor instances for MSMON_MBWU.VALUE field for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Accessing MPAMF_MBWUMON_IDR

This register is within the MPAM feature page memory frames. If FEAT_MPAM is implemented, the following statements apply:

- • In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.
- • MPAMF_MBWUMON_IDR must be readable from the Non-secure and Secure MPAM feature pages.
- • MPAMF_MBWUMON_IDR is permitted to have the same contents when read from the Secure or Non-secure MPAM feature pages unless the register contents are different for the different versions, when MPAMF_MBWUMON_IDR_s is permitted to have either the same or different contents to MPAMF_MBWUMON_IDR_ns.
- • There must be separate registers in the Secure (MPAMF_MBWUMON_IDR_s) and Non-secure (MPAMF_MBWUMON_IDR_ns) MPAM feature pages.


If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_MBWUMON_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_MBWUMON_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:

- • MPAMF_MBWUMON_IDR_s is permitted to have either the same or different contents to MPAMF_MBWUMON_IDR_ns, MPAMF_MBWUMON_IDR_rt, or MPAMF_MBWUMON_IDR_rl.
- • MPAMF_MBWUMON_IDR_ns is permitted to have either the same or different contents to MPAMF_MBWUMON_IDR_rt or MPAMF_MBWUMON_IDR_rl.
- • MPAMF_MBWUMON_IDR_rt is permitted to have either the same or different contents to MPAMF_MBWUMON_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_MBWUMON_IDR_s), Non-secure (MPAMF_MBWUMON_IDR_ns), Root (MPAMF_MBWUMON_IDR_rt), and Realm (MPAMF_MBWUMON_IDR_rl) MPAM feature pages.


If any one of these features is implemented:

- • FEAT_MPAMv0p1
- • FEAT_MPAMv1p1


then, the following statements apply:

- • When MPAMF_IDR.HAS_RIS is 1, MPAMF_MBWUMON_IDR shows the configuration of memory bandwidth monitoring for the bandwidth resource instance selected by MPAMCFG_PART_SEL.RIS. Fields that mention RIS in their field descriptions have values that track the implemented properties of the resource instance. Fields that do not mention RIS are constant across all resource instances.
- • Access to MPAMF_MBWUMON_IDR is not affected by MSMON_CFG_MON_SEL.RIS.


MPAMF_MBWUMON_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0090 MPAMF_MBWUMON_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0090 MPAMF_MBWUMON_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0090 MPAMF_MBWUMON_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0090 MPAMF_MBWUMON_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.10 MPAMF_MSMON_IDR, MPAM Resource Monitoring Identification RegisterThe MPAMF_MSMON_IDR characteristics are:

Purpose

Indicates which MPAM monitoring features are present on this MSC. MPAMF_MSMON_IDR_s indicates Secure monitoring features. MPAMF_MSMON_IDR_ns indicates Non-secure monitoring features. MPAMF_MSMON_IDR_rt indicates Root monitoring features. MPAMF_MSMON_IDR_rl indicates Realm monitoring features. If MPAMF_IDR.HAS_RIS is 1, fields that mention RIS must reflect the properties of the resource instance currently selected by MPAMCFG_PART_SEL.RIS. Fields that do not mention RIS are constant across all resource instances.

Configuration

The power domain of MPAMF_MSMON_IDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM is implemented and MPAMF_IDR.HAS_MSMON == ‘1’. Otherwise, direct accesses to MPAMF_MSMON_IDR are RES0.

Attributes

MPAMF_MSMON_IDR is a 32-bit register.

Field descriptions

31 30 29 28 27 26 25 19 18 17 16 15 0

| | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|
| | | | | |RES0| | | |RES0|


HAS_LOC AL_CAPT

MSMON_CSU MSMON_MBWU

_EVNT NO_HW_OFLW

MSMON_CSA HAS_TL_MONITORING

_INTR HAS_OFLW_MSI

HAS_OFLOW_SR

HAS_LOCAL_CAPT_EVNT, bit [31]

Has local capture event generator. Indicates whether this MSC has the MPAM local capture event generator and the MSMON_CAPT_EVNT register.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_LOCAL_CAPT_EVNT Meaning

- 0b0 Does not support MPAM local capture event generator or MSMON_CAPT_EVNT.

- 0b1 Supports the MPAM local capture event generator and the MSMON_CAPT_EVNT register.


Access to this field is RO.

NO_HW_OFLW_INTR, bit [30] When FEAT_MPAMv1p1 is implemented:

Does not have hardwired MPAM monitor overflow interrupt. The value of this field is an IMPLEMENTATION DEFINED choice of:

NO_HW_OFLW_INTR Meaning

- 0b0 Supports generating a hardwired interrupt to signal MPAM monitor overflow.

- 0b1 No support for a hardwired interrupt to signal MPAM monitor overflow.


If this field is 0, the MSC supports generating a hardwired interrupt for monitor overflow events. If this field is 0 and the HAS_OFLW_MSI field in this register is 1, the MSC supports generating both hardwired interrupts and MSI writes to signal interrupts. Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_OFLW_MSI, bit [29] When FEAT_MPAMv1p1 is implemented:

Has support for MSI writes to signal MPAM monitor overflow interrupts. These registers are implemented: MSMON_OFLOW_MSI_ADDR_L, MSMON_OFLOW_MSI_ADDR_H, MSMON_OFLOW_MSI_ATTR, MSMON_OFLOW_MSI_DATA and MSMON_OFLOW_MSI_MPAM.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_OFLW_MSI Meaning

- 0b0 MSMON_OFLOW_MSI_ADDR_L, MSMON_OFLOW_MSI_ADDR_H, MSMON_OFLOW_MSI_ATTR, MSMON_OFLOW_MSI_DATA and MSMON_OFLOW_MSI_MPAM registers are not implemented.

- 0b1 MSMON_OFLOW_MSI_ADDR_L, MSMON_OFLOW_MSI_ADDR_H, MSMON_OFLOW_MSI_ATTR, MSMON_OFLOW_MSI_DATA and MSMON_OFLOW_MSI_ATTR are implemented and can be used to generate writes to signal MPAM monitor overflow interrupts.


If MPAMF_MSMON_IDR.NO_HW_OFLW_INTR is 1 and this bit is 0, this MSC does not support monitor overflow interrupts.

Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_OFLOW_SR, bit [28] When FEAT_MPAMv1p1 is implemented:

Has MPAM monitor overflow status register MSMON_OFLOW_SR. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_OFLOW_SR Meaning

- 0b0 Does not have MSMON_OFLOW_SR.

- 0b1 Supports MSMON_OFLOW_SR.


Access to this field is RO.

Otherwise:

Reserved, RES0.

HAS_TL_MONITORING, bits [27:26] When FEAT_MPAM_MSC_DOMAINS is implemented:

Indicates whether the monitor supports filtering based on ingress unstranslated MPAM information, translated egress MPAM information, or intermediate MPAM information.

The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_TL_MONITORING Meaning Applies when

- 0b00 The monitor filters on the intermediate MPAM information, after ingress translation and before egress translation, if implemented and enabled.

- 0b01 The monitor filters on the incoming untranslated MPAM information of the memory request, before ingress translation.


MPAMF_IDR.HAS_IN_TL == ‘1’

- 0b10 The monitor filters on the outgoing resulting MPAM information of the memory request after egress translation.

MPAMF_IDR.HAS_OUT_TL == ‘1’

- 0b11 Reserved.


Access to this field is RO.

Otherwise:

Reserved, RES0.

Bits [25:19]

Reserved, RES0.

MSMON_CSA, bit [18]

Reports if the cache storage allocation (CSA) monitor is implemented.

The value of this field is an IMPLEMENTATION DEFINED choice of:

MSMON_CSA Meaning

0b0 The cache storage allocation (CSA) monitor is not implemented. 0b1 The cache storage allocation (CSA) monitor is implemented.

If MPAMF_IDR.HAS_RIS is 1, this field reports that the CSA monitor is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

MSMON_MBWU, bit [17]

Memory bandwidth usage monitoring. Indicates whether MPAM monitoring for Memory Bandwidth Usage by PARTID and PMG is implemented and whether the following bandwidth usage registers are accessible:

- • MPAMF_MBWUMON_IDR, MSMON_CFG_MBWU_CTL, MSMON_CFG_MBWU_FLT, MSMON_MBWU.
- • The optional MSMON_MBWU_CAPTURE.
- • If MPAM v0.1 or MPAM v1.1 is implemented, the optional MSMON_MBWU_L and the optional MSMON_MBWU_L_CAPTURE.


The value of this field is an IMPLEMENTATION DEFINED choice of:

MSMON_MBWU Meaning

- 0b0 Does not have monitoring for memory bandwidth usage and does not use the bandwidth usage registers.

- 0b1 Has monitoring of memory bandwidth usage and uses the bandwidth usage registers.


If RIS is implemented, this field indicates that memory bandwidth usage monitoring is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS as described in MPAMF_MBWUMON_IDR.

Access to this field is RO.

MSMON_CSU, bit [16]

Cache storage usage monitoring. Indicates whether MPAM monitoring of cache storage usage by PARTID and PMG is implemented and the following registers are accessible:

- • MPAMF_CSUMON_IDR, MSMON_CFG_CSU_CTL, MSMON_CFG_CSU_FLT, MSMON_CSU.
- • The optional MSMON_CSU_CAPTURE.


The value of this field is an IMPLEMENTATION DEFINED choice of:

MSMON_CSU Meaning

- 0b0 Does not have monitoring for cache storage usage or the MPAMF_CSUMON_IDR, MSMON_CFG_CSU_CTL, MSMON_CFG_CSU_FLT, MSMON_CSU or MSMON_CSU_CAPTURE registers.


###### MSMON_CSU Meaning

- 0b1 Has monitoring of cache storage usage and the MPAMF_CSUMON_IDR, MSMON_CFG_CSU_CTL, MSMON_CFG_CSU_FLT, MSMON_CSU and optional MSMON_CSU_CAPTURE registers.


If RIS is implemented, this field indicates that cache storage usage monitoring is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS as described in MPAMF_CSUMON_IDR.

Access to this field is RO.

Bits [15:0]

Reserved, RES0.

Accessing MPAMF_MSMON_IDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_MSMON_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_MSMON_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:

- • MPAMF_MSMON_IDR_s is permitted to have either the same or different contents to MPAMF_MSMON_IDR_ns, MPAMF_MSMON_IDR_rt, or MPAMF_MSMON_IDR_rl.
- • MPAMF_MSMON_IDR_ns is permitted to have either the same or different contents to MPAMF_MSMON_IDR_rt or MPAMF_MSMON_IDR_rl.
- • MPAMF_MSMON_IDR_rt is permitted to have either the same or different contents to MPAMF_MSMON_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_MSMON_IDR_s), Non-secure (MPAMF_MSMON_IDR_ns), Root (MPAMF_MSMON_IDR_rt), and Realm (MPAMF_MSMON_IDR_rl) MPAM feature pages.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

- • When MPAMF_IDR.HAS_RIS is 1, MPAMF_MSMON_IDR shows the configuration of memory system monitoring for the resource instance selected by MPAMCFG_PART_SEL.RIS. Fields that mention RIS in their field descriptions have values that track the implemented properties of the resource instance. Fields that do not mention RIS are constant across all resource instances.
- • Access to MPAMF_MSMON_IDR is not affected by MSMON_CFG_MON_SEL.RIS.


MPAMF_MSMON_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0080 MPAMF_MSMON_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0080 MPAMF_MSMON_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0080 MPAMF_MSMON_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0080 MPAMF_MSMON_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.11 MPAMF_PARTID_NRW_IDR, MPAM PARTID Narrowing ID registerThe MPAMF_PARTID_NRW_IDR characteristics are:

Purpose

Indicates the largest internal PARTID for this MSC. MPAMF_PARTID_NRW_IDR_s indicates the largest Secure internal PARTID. MPAMF_PARTID_NRW_IDR_ns indicates the largest Non-secure internal PARTID. When FEAT_RME is implemented: MPAMF_PARTID_NRW_rt indicates the largest Root internal PARTID. MPAMF_PARTID_NRW_rl indicates the largest Realm internal PARTID. PARTID narrowing is global to the MSC and does not vary by resource instance.

Configuration

The power domain of MPAMF_PARTID_NRW_IDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented) and MPAMF_IDR.HAS_PARTID_NRW == ‘1’. Otherwise, direct accesses to MPAMF_PARTID_NRW_IDR are RES0.

Attributes

MPAMF_PARTID_NRW_IDR is a 32-bit register.

Field descriptions

31 16 15 0

| | |
|---|---|
|RES0|INTPARTID_MAX|


Bits [31:16]

Reserved, RES0.

INTPARTID_MAX, bits [15:0]

The largest intPARTID supported in this MSC. This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

Accessing MPAMF_PARTID_NRW_IDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_PARTID_NRW_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_PARTID_NRW_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:


- • MPAMF_PARTID_NRW_IDR_s is permitted to have either the same or different contents to MPAMF_PARTID_NRW_IDR_ns, MPAMF_PARTID_NRW_IDR_rt, or MPAMF_PARTID_NRW_IDR_rl.
- • MPAMF_PARTID_NRW_IDR_ns is permitted to have either the same or different contents to MPAMF_PARTID_NRW_IDR_rt or MPAMF_PARTID_NRW_IDR_rl.


- • MPAMF_PARTID_NRW_IDR_rt is permitted to have either the same or different contents to MPAMF_PARTID_NRW_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_PARTID_NRW_IDR_s), Non-secure (MPAMF_PARTID_NRW_IDR_ns), Root (MPAMF_PARTID_NRW_IDR_rt), and Realm (MPAMF_PARTID_NRW_IDR_rl) MPAM feature pages.


MPAMF_PARTID_NRW_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0050 MPAMF_PARTID_NRW_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0050 MPAMF_PARTID_NRW_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0050 MPAMF_PARTID_NRW_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0050 MPAMF_PARTID_NRW_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.12 MPAMF_PRI_IDR, MPAM Priority Partitioning Identification RegisterThe MPAMF_PRI_IDR characteristics are:

Purpose

Indicates which MPAM priority partitioning features are present on this MSC. MPAMF_PRI_IDR_s indicates priority partitioning features accessed from the Secure MPAM feature page. MPAMF_PRI_IDR_ns indicates priority partitioning features accessed from the Non-secure MPAM feature page. MPAMF_PRI_IDR_rt indicates priority partitioning features accessed from the Root MPAM feature page. MPAMF_PRI_IDR_rl indicates priority partitioning features accessed from the Realm MPAM feature page. When MPAMF_IDR.HAS_RIS is 1, some fields in this register give information for the resource instance selected by MPAMCFG_PART_SEL.RIS. The description of every field that is affected by MPAMCFG_PART_SEL.RIS has that information within the field description.

Configuration

The power domain of MPAMF_PRI_IDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented) and MPAMF_IDR.HAS_PRI_PART == ‘1’. Otherwise, direct accesses to MPAMF_PRI_IDR are RES0.

Attributes

MPAMF_PRI_IDR is a 32-bit register.

Field descriptions

31 26 25 20 19 18 17 16 15 10 9 4 3 2 1 0

| | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|
|RES0|DSPRI_WD|RES0| | |RES0|INTPRI_WD|RES0| | |


DSPRI_0_IS_LOW HAS_DSPRI INTPRI_0_IS_LOW HAS_INT

PRI

Bits [31:26]

Reserved, RES0.

DSPRI_WD, bits [25:20]

Number of implemented bits in the downstream priority field (DSPRI) of MPAMCFG_PRI. If HAS_DSPRI == 1, this field must contain a value from 1 to 16, inclusive. If HAS_DSPRI == 0, this field must be 0. This field has an IMPLEMENTATION DEFINED value. If RIS is implemented, this field indicates the number of downstream priority bits for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Bits [19:18]

Reserved, RES0.

DSPRI_0_IS_LOW, bit [17]

Indicates whether 0 in MPAMCFG_PRI.DSPRI is the lowest or the highest downstream priority.

The value of this field is an IMPLEMENTATION DEFINED choice of:

DSPRI_0_IS_LOW Meaning

0b0 In the MPAMCFG_PRI.DSPRI field, a value of 0 means the highest priority. 0b1 In the MPAMCFG_PRI.DSPRI field, a value of 0 means the lowest priority.

If RIS is implemented, this field indicates that 0 is the lowest downstream priority for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

HAS_DSPRI, bit [16]

Indicates that the MPAMCFG_PRI register implements the DSPRI field. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_DSPRI Meaning

- 0b0 This MSC supports priority partitioning, but does not implement a downstream priority (DSPRI) field in the MPAMCFG_PRI register.

- 0b1 This MSC supports downstream priority partitioning and implements the downstream priority (DSPRI) field in the MPAMCFG_PRI register.


If RIS is implemented, this field indicates that downstream priority is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Bits [15:10]

Reserved, RES0.

INTPRI_WD, bits [9:4]

Number of implemented bits in the internal priority field (INTPRI) in the MPAMCFG_PRI register. If HAS_INTPRI == 1, this field must contain a value from 1 to 16, inclusive. If HAS_INTPRI == 0, this field must be 0. This field has an IMPLEMENTATION DEFINED value. If RIS is implemented, this field indicates the number of internal priority bits for the resource instance selected by MPAMCFG_PART_SEL.RIS. Access to this field is RO.

Bits [3:2]

Reserved, RES0.

INTPRI_0_IS_LOW, bit [1]

Indicates whether 0 in MPAMCFG_PRI.INTPRI is the lowest or the highest internal priority. The value of this field is an IMPLEMENTATION DEFINED choice of:

INTPRI_0_IS_LOW Meaning

- 0b0 In the MPAMCFG_PRI.INTPRI field, a value of 0 means the highest priority.

- 0b1 In the MPAMCFG_PRI.INTPRI field, a value of 0 means the lowest priority.


If RIS is implemented, this field indicates that 0 is the lowest internal priority for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

HAS_INTPRI, bit [0]

Indicates that this MSC implements the INTPRI field in the MPAMCFG_PRI register. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_INTPRI Meaning

- 0b0 This MSC supports priority partitioning, but does not implement the internal priority (INTPRI) field in the MPAMCFG_PRI register.

- 0b1 This MSC supports internal priority partitioning and implements the internal priority (INTPRI) field in the MPAMCFG_PRI register.


If RIS is implemented, this field indicates that internal priority is implemented for the resource instance selected by MPAMCFG_PART_SEL.RIS.

Access to this field is RO.

Accessing MPAMF_PRI_IDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_PRI_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_PRI_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:

- • MPAMF_PRI_IDR_s is permitted to have either the same or different contents to MPAMF_PRI_IDR_ns, MPAMF_PRI_IDR_rt, or MPAMF_PRI_IDR_rl.
- • MPAMF_PRI_IDR_ns is permitted to have either the same or different contents to MPAMF_PRI_IDR_rt or MPAMF_PRI_IDR_rl.
- • MPAMF_PRI_IDR_rt is permitted to have either the same or different contents to MPAMF_PRI_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_PRI_IDR_s), Non-secure (MPAMF_PRI_IDR_ns), Root (MPAMF_PRI_IDR_rt), and Realm (MPAMF_PRI_IDR_rl) MPAM feature pages.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• When MPAMF_IDR.HAS_RIS is 1, MPAMF_PRI_IDR shows the configuration of priority partitioning for the resource instance selected by MPAMCFG_PART_SEL.RIS. Fields that mention RIS in their field descriptions have values that track the implemented properties of the resource instance. Fields that do not mention RIS are constant across all resource instances.

MPAMF_PRI_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0048 MPAMF_PRI_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0048 MPAMF_PRI_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0048 MPAMF_PRI_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0048 MPAMF_PRI_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.5.13 MPAMF_SIDR, MPAM Features Secure Identification RegisterThe MPAMF_SIDR characteristics are:

Purpose

The MPAMF_SIDR is a 32-bit read-only register that indicates the maximum Secure PARTID and Secure PMG on this MSC.

Configuration

The power domain of MPAMF_SIDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented. Otherwise, direct accesses to MPAMF_SIDR are RES0.

Attributes

MPAMF_SIDR is a 32-bit register.

Field descriptions

31 24 23 16 15 0

| | | |
|---|---|---|
|RES0|S_PMG_MAX|S_PARTID_MAX|


Bits [31:24]

Reserved, RES0.

S_PMG_MAX, bits [23:16]

Maximum value of Secure PMG supported by this component. This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

S_PARTID_MAX, bits [15:0]

Maximum value of Secure PARTID supported by this component. This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

Accessing MPAMF_SIDR

This register is only within the Secure MPAM feature page memory frame. MPAMF_SIDR must only be readable from the Secure MPAM feature page. If the system or the MSC does not support the Secure address map, this register must not be accessible. MPAMF_SIDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0008 MPAMF_SIDR_s

###### Accesses to this register are RO.

###### 9.6 Memory-mapped partitioning configuration registersThis section lists the external partitioning configuration registers.

###### 9.6.1 MPAMCFG_CASSOC, MPAM Cache Maximum Associativity Partition Configuration RegisterThe MPAMCFG_CASSOC characteristics are:

Purpose

The MPAMCFG_CASSOC is a 32-bit read/write register that controls the maximum fraction of the cache associativity that the PARTID selected by MPAMCFG_PART_SEL is permitted to allocate.

MPAMCFG_CASSOC_s controls the cache maximum associativity for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_CASSOC_ns controls the cache maximum associativity for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_CASSOC_rl controls the cache maximum associativity for the Realm PARTID selected by the Realm instance of MPAMCFG_PART_SEL. MPAMCFG_CASSOC_rt controls the cache maximum associativity for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL.

If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_CASSOC is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when ((MPAMF_IDR.HAS_CCAP_PART == ‘1’) && (IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p1))) && (MPAMF_CCAP_IDR.HAS_CASSOC == ‘1’). Otherwise, direct accesses to MPAMCFG_CASSOC are RES0.

Attributes

MPAMCFG_CASSOC is a 32-bit register.

Field descriptions

31 16 15 0

| | |
|---|---|
|RES0|CASSOC|


Bits [31:16]

Reserved, RES0.

CASSOC, bits [15:0]

Maximum cache associativity usage in fixed-point fraction format by the partition selected by MPAMCFG_PART_SEL. The fraction represents the portion of the cache associativity that the PARTID is permitted to allocate. CASSOC controls the fraction of associativity in each associativity grouping of the cache. In a set associative cache, CASSOC applies to the fraction of the ways in each set.

The implemented width of the fixed-point fraction is given in MPAMF_CCAP_IDR.CASSOC_WD. Unimplemented bits within the field are RAZ/WI. The implemented bits of the CASSOC field are always the most significant bits of the field.

The fixed-point fraction CASSOC is less than 1. The implied binary point is between bits 15 and 16. In an implementation with w implemented bits, the largest fraction of the cache that can be represented is 1- (0.5)w.

Accessing MPAMCFG_CASSOC

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:


- • MPAMCFG_CASSOC_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_CASSOC_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_CASSOC_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_CASSOC_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_CASSOC_s, MPAMCFG_CASSOC_ns, MPAMCFG_CASSOC_rt, and MPAMCFG_CASSOC_rl must be separate registers:
- • The Secure instance (MPAMCFG_CASSOC_s) accesses the cache maximum associativity partitioning used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_CASSOC_ns) accesses the cache maximum associativity partitioning used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_CASSOC_rt) accesses the cache maximum associativity partitioning used for Root PARTIDs.
- • The Realm instance (MPAMCFG_CASSOC_rl) accesses the cache maximum associativity partitioning used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_CASSOC access the cache maximum associativity partitioning configuration settings for the cache resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_CASSOC access the cache maximum associativity partitioning configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_CASSOC access the cache maximum associativity partitioning configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_CASSOC access the cache maximum associativity partitioning configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_CASSOC can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0118 MPAMCFG_CASSOC_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0118 MPAMCFG_CASSOC_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0118 MPAMCFG_CASSOC_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0118 MPAMCFG_CASSOC_rl

When FEAT_RME is implemented, accesses to this register are RW.

- 9.6.2 MPAMCFG_CMAX, MPAM Cache Maximum Capacity Partition Configuration Register The MPAMCFG_CMAX characteristics are:


Purpose

The MPAMCFG_CMAX is a 32-bit read/write register that controls the maximum fraction of the cache capacity that the PARTID selected by MPAMCFG_PART_SEL is permitted to allocate.

MPAMCFG_CMAX_s controls the cache maximum capacity for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_CMAX_ns controls the cache maximum capacity for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_CMAX_rt controls the cache maximum capacity for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. MPAMCFG_CMAX_rl controls the cache maximum capacity for the Realm PARTID selected by the Realm instance of MPAMCFG_PART_SEL. If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_CMAX is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_CCAP_PART == ‘1’)) && (MPAMF_CCAP_IDR.NO_CMAX == ‘0’). Otherwise, direct accesses to MPAMCFG_CMAX are RES0.

Attributes

MPAMCFG_CMAX is a 32-bit register.

Field descriptions

31 30 16 15 0

| | | |
|---|---|---|
| |RES0|CMAX|


SOFTLIM

SOFTLIM, bit [31] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_CCAP_IDR.HAS_CMAX_SOFTLIM == '1':

Soft limiting of CMAX. Soft limiting allows some allocations by a PARTID when its cache use is above the CMAX maximum cache capacity.

SOFTLIM Meaning

- 0b0 When CMAX cache capacity is exceeded, the partition is not allowed to increase its cache capacity usage. It is only permitted to replace a line that was previously occupied by a line allocated by that PARTID.

- 0b1 When CMAX cache capacity is exceeded, the partition is permitted to allocate capacity beyond CMAX, but only from invalid lines or lines belonging to disabled PARTIDs.


Otherwise:

Reserved, RES0.

Bits [30:16]

Reserved, RES0.

CMAX, bits [15:0]

Maximum cache capacity usage in fixed-point fraction format by the partition selected by MPAMCFG_PART_SEL. The fraction represents the portion of the total cache capacity that the PARTID is permitted to allocate. The implemented width of the fixed-point fraction is given in MPAMF_CCAP_IDR.CMAX_WD. Unimplemented bits within the field are RAZ/WI. The implemented bits of the CMAX field are always the most significant bits of the field. The fixed-point fraction CMAX is less than 1. The implied binary point is between bits 15 and 16. In an implementation with w implemented bits, the largest fraction of the cache that can be represented is 1- (0.5)w.

Accessing MPAMCFG_CMAX

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_CMAX_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_CMAX_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_CMAX_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_CMAX_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_CMAX_s, MPAMCFG_CMAX_ns, MPAMCFG_CMAX_rt, and MPAMCFG_CMAX_rl must be separate registers:
- • The Secure instance (MPAMCFG_CMAX_s) accesses the cache capacity partitioning used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_CMAX_ns) accesses the cache capacity partitioning used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_CMAX_rt) accesses the cache capacity partitioning used for Root PARTIDs.
- • The Realm instance (MPAMCFG_CMAX_rl) accesses the cache capacity partitioning used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_CMAX access the cache maximum capacity partitioning configuration settings for the cache resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_CMAX access the cache maximum capacity partitioning configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_CMAX access the cache maximum capacity partitioning configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_CMAX access the cache maximum capacity partitioning configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_CMAX can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0108 MPAMCFG_CMAX_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0108 MPAMCFG_CMAX_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0108 MPAMCFG_CMAX_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0108 MPAMCFG_CMAX_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.6.3 MPAMCFG_CMIN, MPAM Cache Minimum Capacity Partition Configuration RegisterThe MPAMCFG_CMIN characteristics are:

Purpose

The MPAMCFG_CMIN is a 32-bit read/write register that controls the fraction of the cache capacity that the PARTID selected by MPAMCFG_PART_SEL has priority to allocate.

MPAMCFG_CMIN_s controls the cache minimum capacity for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_CMIN_ns controls the cache minimum capacity for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_CMIN_rl controls the cache minimum capacity for the Realm PARTID selected by the Realm instance of MPAMCFG_PART_SEL. MPAMCFG_CMIN_rt controls the cache minimum capacity for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_CMIN is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when ((MPAMF_IDR.HAS_CCAP_PART == ‘1’) && (IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p1))) && (MPAMF_CCAP_IDR.HAS_CMIN == ‘1’). Otherwise, direct accesses to MPAMCFG_CMIN are RES0.

Attributes

MPAMCFG_CMIN is a 32-bit register.

Field descriptions

31 16 15 0

| | |
|---|---|
|RES0|CMIN|


Bits [31:16]

Reserved, RES0.

CMIN, bits [15:0]

Minimum cache capacity usage in fixed-point fraction format by the partition selected by MPAMCFG_PART_SEL. The fraction represents the portion of the total cache capacity that the PARTID has priority to allocate. The implemented width of the fixed-point fraction is the same as the width of MPAMCFG_CMAX.CMAX which is given in MPAMF_CCAP_IDR.CMAX_WD. Unimplemented bits within the field are RAZ/WI. The implemented bits of the CMIN field are always the most significant bits of the field. The fixed-point fraction CMIN is less than 1. The implied binary point is between bits 15 and 16. In an implementation with w implemented bits, the largest fraction of the cache that can be represented is 1- (0.5)w.

Accessing MPAMCFG_CMIN

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:


- • MPAMCFG_CMIN_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_CMIN_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_CMIN_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_CMIN_rl must only be accessible from the Realm MPAM feature page.

- • MPAMCFG_CMIN_s, MPAMCFG_CMIN_ns, MPAMCFG_CMIN_rt, and MPAMCFG_CMIN_rl must be separate registers:


- • The Secure instance (MPAMCFG_CMIN_s) accesses the cache minimum capacity partitioning used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_CMIN_ns) accesses the cache minimum capacity partitioning used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_CMIN_rt) accesses the cache minimum capacity partitioning used for Root PARTIDs.
- • The Realm instance (MPAMCFG_CMIN_rl) accesses the cache minimum capacity partitioning used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_CMIN access the cache minimum capacity partitioning configuration settings for the cache resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_CMIN access the cache minimum capacity partitioning configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_CMIN access the cache minimum capacity partitioning configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_CMIN access the cache minimum capacity partitioning configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_CMIN can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0110 MPAMCFG_CMIN_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0110 MPAMCFG_CMIN_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0110 MPAMCFG_CMIN_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0110 MPAMCFG_CMIN_rl

When FEAT_RME is implemented, accesses to this register are RW.

- 9.6.4 MPAMCFG_CPBM<n>, MPAM Cache Portion Bitmap Partition Configuration Register, n = 0 - 1023 The MPAMCFG_CPBM<n> characteristics are:


Purpose

The MPAMCFG_CPBM<n> register array gives access to the cache portion bitmap. Each register in the array is a read/write register that configures the cache portions numbered from n * 32 to 31 + (n * 32) that a PARTID is allowed to allocate.

After setting MPAMCFG_PART_SEL with a PARTID, software writes to the MPAMCFG_CPBM<n> register to configure which cache portions the PARTID is allowed to allocate.

The MPAMCFG_CPBM<n> register that contains the bitmap bit corresponding to cache portion p has n equal to p[15:5]. The field, P<x>, of that MPAMCFG_CPBM<n> register that contains the bitmap bit corresponding to cache portion p has x equal to p[4:0].

MPAMCFG_CPBM<n>_s controls cache portions for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_CPBM<n>_ns controls the cache portions for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_CPBM<n>_rt controls cache portions for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. MPAMCFG_CPBM<n>_rl controls the cache portions for the Realm PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL.

If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_CPBM<n> is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented) and MPAMF_IDR.HAS_CPOR_PART == ‘1’. Otherwise, direct accesses to MPAMCFG_CPBM<n> are RES0.

Attributes

MPAMCFG_CPBM<n> is a 32-bit register.

Field descriptions

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 0

| | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|P31|P30|P29|P28|P27|P26|P25|P24|P23|P22|P21|P20|P19|P18|P17|P16|P15|P14|P13|P12|P11|P10|P9|P8|P7|P6|P5|P4|P3|P2|P1|P0|


P<x> , bits [x], for x = 31 to 0

Portion allocation control bit. Each cache portion allocation control bit, MPAMCFG_CPBMn.P<x>, grants permission to the PARTID selected by MPAMCFG_PART_SEL to allocate cache lines within cache portion n*32

+ x.

P<x> Meaning

0b0 The PARTID is not permitted to allocate into cache portion n * 32 + x. 0b1 The PARTID is permitted to allocate within cache portion n * 32 + x.

The number of bits in the cache portion partitioning bit map of this component is given in MPAMF_CPOR_IDR.CPBM_WD. MPAMF_CPOR_IDR.CPBM_WD contains a value from 1 to 215, inclusive.

Values of MPAMF_CPOR_IDR.CPBM_WD greater than 32 require an array of 32-bit MPAMCFG_CPBM<n> registers to access the cache portion bitmap, up to 1024 registers.

When ((n * 32) + x) > UInt(MPAMF_CPOR_IDR().CPBM_WD), access to this field is RES0.

Accessing MPAMCFG_CPBM<n>

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_CPBM<n>_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_CPBM<n>_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_CPBM<n>_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_CPBM<n>_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_CPBM<n>_s, MPAMCFG_CPBM<n>_ns, MPAMCFG_CPBM<n>_rt, and MPAMCFG_CPBM<n>_rl must be separate registers:
- • The Secure instance (MPAMCFG_CPBM<n>_s) accesses the cache portion bitmap used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_CPBM<n>_ns) accesses the cache portion bitmap used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_CPBM<n>_rt) accesses the cache portion bitmap used for Root PARTIDs.
- • The Realm instance (MPAMCFG_CPBM<n>_rl) accesses the cache portion bitmap used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_CPBM<n> access the cache portion bitmap configuration settings for the cache resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_CPBM<n> access the cache portion bitmap configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_CPBM<n> access the cache portion bitmap configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_CPBM<n> access the cache portion bitmap configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_CPBM<n> can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x1000 + (4 *

MPAMCFG_CPBM<n>_s

n)

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x1000 + (4 *

MPAMCFG_CPBM<n>_ns

n)

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x1000 + (4 *

MPAMCFG_CPBM<n>_rt

n)

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x1000 + (4 *

MPAMCFG_CPBM<n>_rl

n)

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.6.5 MPAMCFG_DIS, MPAM Partition Configuration Disable RegisterThe MPAMCFG_DIS characteristics are:

Purpose

Disables a PARTID configuration as set in other MPAMCFG registers. MPAMCFG_DIS_s disables a Secure PARTID. MPAMCFG_DIS_ns disables a Non-secure PARTID. MPAMCFG_DIS_rl disables a Realm PARTID. MPAMCFG_DIS_rt disables a Root PARTID.

Configuration

The power domain of MPAMCFG_DIS is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_ENDIS == ‘1’. Otherwise, direct accesses to MPAMCFG_DIS are RES0.

Attributes

MPAMCFG_DIS is a 32-bit register.

Field descriptions

31 30 16 15 0

| | | |
|---|---|---|
|NFU|RES0|PARTID|


NFU, bit [31] When MPAMF_IDR.HAS_NFU == '1':

No Future Use. Software indicates that the PARTID disabled with NFU of 1 will not be used again and will be reconfigured and reenabled before being used again.

NFU Meaning

0b0 Control settings of the disabled PARTID must be retained. 0b1 Control settings of the disabled PARTID may take an UNKNOWN value.

Otherwise:

Reserved, RES0.

Bits [30:16]

Reserved, RES0.

PARTID, bits [15:0]

Selects the PARTID to disable.

Accessing MPAMCFG_DIS

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_DIS_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_DIS_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_DIS_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_DIS_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_DIS_s, MPAMCFG_DIS_ns, MPAMCFG_DIS_rt, and MPAMCFG_DIS_rl must be separate registers:
- • The Secure instance (MPAMCFG_DIS_s) accesses the PARTID disable used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_DIS_ns) accesses the PARTID disable used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_DIS_rt) accesses the PARTID disable used for Root PARTIDs.
- • The Realm instance (MPAMCFG_DIS_rl) accesses the PARTID disable used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_DIS access the PARTID disable configuration settings for the PARTID disable resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_DIS access the PARTID disable configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_DIS access the PARTID disable configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_DIS access the PARTID disable configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_DIS can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0310 MPAMCFG_DIS_s

Accesses to this register are WO/RAZ.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0310 MPAMCFG_DIS_ns

Accesses to this register are WO/RAZ.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0310 MPAMCFG_DIS_rt

When FEAT_RME is implemented, accesses to this register are WO/RAZ.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0310 MPAMCFG_DIS_rl

When FEAT_RME is implemented, accesses to this register are WO/RAZ.

###### 9.6.6 MPAMCFG_EN, MPAM Partition Configuration Enable RegisterThe MPAMCFG_EN characteristics are:

Purpose

Enables a PARTID configuration as set in other MPAMCFG registers. MPAMCFG_EN_s enables a Secure PARTID. MPAMCFG_EN_ns enables a Non-secure PARTID. MPAMCFG_EN_rl enables a Realm PARTID. MPAMCFG_EN_rt enables a Root PARTID.

Configuration

The power domain of MPAMCFG_EN is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_ENDIS == ‘1’. Otherwise, direct accesses to MPAMCFG_EN are RES0.

Attributes

MPAMCFG_EN is a 32-bit register.

Field descriptions

31 16 15 0

| | |
|---|---|
|RES0|PARTID|


Bits [31:16]

Reserved, RES0.

PARTID, bits [15:0]

Selects the PARTID to enable.

Accessing MPAMCFG_EN

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_EN_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_EN_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_EN_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_EN_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_EN_s, MPAMCFG_EN_ns, MPAMCFG_EN_rt, and MPAMCFG_EN_rl must be separate registers:
- • The Secure instance (MPAMCFG_EN_s) accesses the PARTID enable used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_EN_ns) accesses the PARTID enable used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_EN_rt) accesses the PARTID enable used for Root PARTIDs.
- • The Realm instance (MPAMCFG_EN_rl) accesses the PARTID enable used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_EN access the PARTID enable configuration settings for the PARTID enable resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_EN access the PARTID enable configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_EN access the PARTID enable configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_EN access the PARTID enable configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_EN can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0300 MPAMCFG_EN_s

Accesses to this register are WO/RAZ.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0300 MPAMCFG_EN_ns

Accesses to this register are WO/RAZ.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0300 MPAMCFG_EN_rt

When FEAT_RME is implemented, accesses to this register are WO/RAZ.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0300 MPAMCFG_EN_rl

When FEAT_RME is implemented, accesses to this register are WO/RAZ.

###### 9.6.7 MPAMCFG_EN_FLAGS, MPAM Partition Configuration Enable Flags RegisterThe MPAMCFG_EN_FLAGS characteristics are:

Purpose

Enable flags for 32 PARTIDs.

MPAMCFG_EN_FLAGS_s gives read/write access to 32 Secure PARTIDs. MPAMCFG_EN_FLAGS_ns gives read/write access to 32 Non-secure PARTIDs. MPAMCFG_EN_FLAGS_rl gives read/write access to 32 Realm PARTIDs. MPAMCFG_EN_FLAGS_rt gives read/write access to 32 Root PARTIDs.

Configuration

The power domain of MPAMCFG_EN_FLAGS is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_ENDIS == ‘1’. Otherwise, direct accesses to MPAMCFG_EN_FLAGS are RES0.

Attributes

MPAMCFG_EN_FLAGS is a 32-bit register.

Field descriptions

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

| | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | | | | | | | | | | | | | |EN9|EN8|EN7|EN6|EN5|EN4|EN3|EN2|EN1|EN0|


EN31 EN30 EN29 EN28 EN27 EN26 EN25 EN24 EN23 EN22 EN21

EN10 EN11

EN12 EN13

EN14 EN15

EN16 EN17

EN18 EN19

EN20

EN<x> , bits [x], for x = 31 to 0

PARTID Enable flags. The group of flags accessed is selected by MPAMCFG_PART_SEL.PARTID_SEL & 0xFFE0 in bit [0] to (MPAMCFG_PART_SEL.PARTID_SEL & 0xFFE0) + 31 in bit [31].

EN<x> Meaning

- 0b0 The PARTID is disabled.

- 0b1 The PARTID is enabled.


Each bit in MPAMCFG_EN_FLAGS gives access to the same state as controlled by MPAMCFG_EN and MPAMCFG_DIS.

Bits MPAMCFG_EN_FLAGS.EN<x>, where (MPAMCFG_PART_SEL.PARTID_SEL & 0xFFE0) + x is greater than MPAMF_IDR.PARTID_MAX, are not required to be implemented.

As with other partitioning controls, the enable flag for PARTID 0 must be reset to 0b1 (enabled).

Accessing MPAMCFG_EN_FLAGS

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_EN_FLAGS_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_EN_FLAGS_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_EN_FLAGS_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_EN_FLAGS_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_EN_FLAGS_s, MPAMCFG_EN_FLAGS_ns, MPAMCFG_EN_FLAGS_rt, and MPAMCFG_EN_FLAGS_rl must be separate registers:
- • The Secure instance (MPAMCFG_EN_FLAGS_s) accesses the PARTID enable used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_EN_FLAGS_ns) accesses the PARTID enable used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_EN_FLAGS_rt) accesses the PARTID enable used for Root PARTIDs.
- • The Realm instance (MPAMCFG_EN_FLAGS_rl) accesses the PARTID enable used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_EN_FLAGS access the PARTID enable configuration settings for the PARTID enable resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_EN_FLAGS access the PARTID enable configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_EN_FLAGS access the PARTID enable configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_EN_FLAGS access the PARTID enable configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_EN_FLAGS can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0320 MPAMCFG_EN_FLAGS_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0320 MPAMCFG_EN_FLAGS_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0320 MPAMCFG_EN_FLAGS_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0320 MPAMCFG_EN_FLAGS_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.6.8 MPAMCFG_INTPARTID, MPAM Internal PARTID Narrowing Configuration RegisterThe MPAMCFG_INTPARTID characteristics are:

Purpose

MPAMCFG_INTPARTID is a 32-bit read/write register that controls the mapping of the PARTID selected by MPAMCFG_PART_SEL into a narrower internal PARTID (intPARTID).

MPAMCFG_INTPARTID_s controls the mapping for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_INTPARTID_ns controls the mapping for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_INTPARTID_rt controls the mapping for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. MPAMCFG_INTPARTID_rl controls the mapping for the Realm PARTID selected by the Realm instance of MPAMCFG_PART_SEL.

The MPAMCFG_INTPARTID register associates the request PARTID (reqPARTID) in the MPAMCFG_PART_SEL register with an internal PARTID (intPARTID) in this register. To set that association, store reqPARTID into the MPAMCFG_PART_SEL register and then store the intPARTID into the MPAMCFG_INTPARTID register. To read the association, store reqPARTID into the MPAMCFG_PART_SEL register and then read MPAMCFG_INTPARTID.

If the intPARTID stored into MPAMCFG_INTPARTID is out-of-range or does not have the INTERNAL bit set, the association of reqPARTID to intPARTID is not written and MPAMF_ESR is set to indicate an intPARTID_Range error.

If MPAMCFG_PART_SEL.INTERNAL is 1 when MPAMCFG_INTPARTID is read or written, MPAMF_ESR is set to indicate an Unexpected_INTERNAL error.

Configuration

The power domain of MPAMCFG_INTPARTID is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented) and MPAMF_IDR.HAS_PARTID_NRW == ‘1’. Otherwise, direct accesses to MPAMCFG_INTPARTID are RES0.

Attributes

MPAMCFG_INTPARTID is a 32-bit register.

Field descriptions

31 17 16 15 0

| | | |
|---|---|---|
|RES0| |INTPARTID|


INTERNAL

Bits [31:17]

Reserved, RES0.

INTERNAL, bit [16]

Internal PARTID flag. This bit must be 1 when written to the register. If written as 0, the write will not update the reqPARTID to intPARTID association. On a read of this register, the bit will always read the value last written.

INTPARTID, bits [15:0]

This field contains the intPARTID mapped to the reqPARTID in MPAMCFG_PART_SEL. The maximum intPARTID supported is MPAMF_PARTID_NRW_IDR.INTPARTID_MAX.

Accessing MPAMCFG_INTPARTID

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_INTPARTID_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_INTPARTID_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_INTPARTID_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_INTPARTID_rl must only be accessible from the Realm MPAM feature page.


• MPAMCFG_INTPARTID_s, MPAMCFG_INTPARTID_ns, MPAMCFG_INTPARTID_rt, and MPAMCFG_INTPARTID_rl must be separate registers:

- • The Secure instance (MPAMCFG_INTPARTID_s) accesses the PARTID narrowing used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_INTPARTID_ns) accesses the PARTID narrowing used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_INTPARTID_rt) accesses the PARTID narrowing used for Root PARTIDs.
- • The Realm instance (MPAMCFG_INTPARTID_rl) accesses the PARTID narrowing used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_INTPARTID access the PARTID narrowing configuration settings without being affected by MPAMCFG_PART_SEL.RIS.

Loads and stores to MPAMCFG_INTPARTID access the PARTID narrowing configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_INTPARTID can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0600 MPAMCFG_INTPARTID_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0600 MPAMCFG_INTPARTID_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0600 MPAMCFG_INTPARTID_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0600 MPAMCFG_INTPARTID_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.6.9 MPAMCFG_MBW_MAX, MPAM Memory Bandwidth Maximum Partition Configuration RegisterThe MPAMCFG_MBW_MAX characteristics are:

Purpose

MPAMCFG_MBW_MAX is a 32-bit read/write register that controls the maximum fraction of memory bandwidth that the PARTID selected by MPAMCFG_PART_SEL is permitted to use.

MPAMCFG_MBW_MAX_s controls maximum bandwidth for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_MAX_ns controls the maximum bandwidth for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_MAX_rt controls the maximum bandwidth for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_MAX_rl controls the maximum bandwidth for the Realm PARTID selected by the Realm instance of MPAMCFG_PART_SEL.

A PARTID that has used more than MAX is given no access to additional bandwidth if HARDLIM == 1 or is given additional bandwidth only if there are no requests from PARTIDs that have not exceeded their MAX if HARDLIM == 0.

If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_MBW_MAX is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MBW_PART == ‘1’)) && (MPAMF_MBW_IDR.HAS_MAX == ‘1’). Otherwise, direct accesses to MPAMCFG_MBW_MAX are RES0.

Attributes

MPAMCFG_MBW_MAX is a 32-bit register.

Field descriptions

31 30 16 15 0

| | | |
|---|---|---|
| |RES0|MAX|


HARDLIM

HARDLIM, bit [31]

Maximum-bandwidth limit behaviour selection.

HARDLIM Meaning

- 0b0 Soft limit: when MAX bandwidth is exceeded, the partition contends with a low preference for downstream bandwidth beyond MAX.

- 0b1 Hard limit: when MAX bandwidth is exceeded, the partition does not use any more bandwidth until the memory bandwidth measurement for the partition falls below MAX.


Accessing this field has the following behavior:

- • When MPAMF_MBW_IDR.MAX_LIM == ‘00’, access to this field is RW.
- • When MPAMF_MBW_IDR.MAX_LIM == ‘01’, access to this field is RAZ/WI. • When MPAMF_MBW_IDR.MAX_LIM == ‘10’, access to this field is RAO/WI.


Bits [30:16]

Reserved, RES0.

MAX, bits [15:0]

Memory maximum bandwidth allocated to the partition selected by MPAMCFG_PART_SEL. MAX is in fixed-point fraction format. The fraction represents the portion of the total memory bandwidth capacity through the controlled component that the PARTID is permitted to allocate.

The implemented width of the fixed-point fraction is given in MPAMF_MBW_IDR.BWA_WD. Unimplemented bits are RAZ/WI. The implemented bits of the MAX field are always to the left of the field. For example, if BWA_WD = 3, the implemented bits are MPAMCFG_MBW_MAX[15:13] and MPAMCFG_MBW_MAX[12:0] are unimplemented.

The fixed-point fraction MAX is less than 1. The implied binary point is between bits 15 and 16. In an implementation with w implemented bits, the largest fraction of the bandwidth that can be represented is 1- (0.5)w.

Accessing MPAMCFG_MBW_MAX

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_MBW_MAX_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_MBW_MAX_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_MBW_MAX_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_MBW_MAX_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_MBW_MAX_s, MPAMCFG_MBW_MAX_ns, MPAMCFG_MBW_MAX_rt, and MPAMCFG_MBW_MAX_rl must be separate registers:
- • The Secure instance (MPAMCFG_MBW_MAX_s) accesses the memory maximum bandwidth partitioning used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_MBW_MAX_ns) accesses the memory maximum bandwidth partitioning used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_MBW_MAX_rt) accesses the memory maximum bandwidth partitioning used for Root PARTIDs.
- • The Realm instance (MPAMCFG_MBW_MAX_rl) accesses the memory maximum bandwidth partitioning used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_MBW_MAX access the memory maximum bandwidth partitioning configuration settings for the bandwidth resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_MBW_MAX access the memory maximum bandwidth partitioning configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_MBW_MAX access the memory maximum bandwidth partitioning configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_MBW_MAX access the memory maximum bandwidth partitioning configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_MBW_MAX can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0208 MPAMCFG_MBW_MAX_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0208 MPAMCFG_MBW_MAX_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0208 MPAMCFG_MBW_MAX_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0208 MPAMCFG_MBW_MAX_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.6.10 MPAMCFG_MBW_MIN, MPAM Memory Bandwidth Minimum Partition Configuration RegisterThe MPAMCFG_MBW_MIN characteristics are:

Purpose

MPAMCFG_MBW_MIN is a 32-bit read/write register that controls the minimum fraction of memory bandwidth that the PARTID selected by MPAMCFG_PART_SEL is permitted to use.

MPAMCFG_MBW_MIN_s controls the minimum bandwidth for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_MIN_ns controls the minimum bandwidth for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_MIN_rt controls the minimum bandwidth for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_MIN_rl controls the minimum bandwidth for the Realm PARTID selected by the Realm instance of MPAMCFG_PART_SEL.

A PARTID that has used less than MIN is given preferential access to bandwidth. If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_MBW_MIN is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MBW_PART == ‘1’)) && (MPAMF_MBW_IDR.HAS_MIN == ‘1’). Otherwise, direct accesses to MPAMCFG_MBW_MIN are RES0.

Attributes

MPAMCFG_MBW_MIN is a 32-bit register.

Field descriptions

31 16 15 0

| | |
|---|---|
|RES0|MIN|


Bits [31:16]

Reserved, RES0.

MIN, bits [15:0]

Memory minimum bandwidth allocated to the partition selected by MPAMCFG_PART_SEL. MIN is in fixed-point fraction format. The fraction represents the portion of the total memory bandwidth capacity through the controlled component that the PARTID is permitted to allocate.

The implemented width of the fixed-point fraction is given in MPAMF_MBW_IDR.BWA_WD. Unimplemented bits are RAZ/WI. The implemented bits of the MIN field are always to the left of the field. For example, if BWA_WD = 4, the implemented bits are MPAMCFG_MBW_MIN[15:12] and MPAMCFG_MBW_MIN[11:0] are unimplemented.

The fixed-point fraction MIN is less than 1. The implied binary point is between bits 15 and 16. In an implementation with w implemented bits, the largest fraction of the bandwidth that can be represented is 1- (0.5)w.

Accessing MPAMCFG_MBW_MIN

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_MBW_MIN_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_MBW_MIN_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_MBW_MIN_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_MBW_MIN_rl must only be accessible from the Realm MPAM feature page.


• MPAMCFG_MBW_MIN_s, MPAMCFG_MBW_MIN_ns, MPAMCFG_MBW_MIN_rt, and MPAMCFG_MBW_MIN_rl must be separate registers:

- • The Secure instance (MPAMCFG_MBW_MIN_s) accesses the memory minimum bandwidth partitioning used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_MBW_MIN_ns) accesses the memory minimum bandwidth partitioning used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_MBW_MIN_rt) accesses the memory minimum bandwidth partitioning used for Root PARTIDs.
- • The Realm instance (MPAMCFG_MBW_MIN_rl) accesses the memory minimum bandwidth partitioning used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_MBW_MIN access the memory minimum bandwidth partitioning configuration settings for the bandwidth resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_MBW_MIN access the memory minimum bandwidth partitioning configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_MBW_MIN access the memory minimum bandwidth partitioning configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_MBW_MIN access the memory minimum bandwidth partitioning configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_MBW_MIN can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0200 MPAMCFG_MBW_MIN_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0200 MPAMCFG_MBW_MIN_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0200 MPAMCFG_MBW_MIN_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0200 MPAMCFG_MBW_MIN_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.6.11 MPAMCFG_MBW_PBM<n>, MPAM Bandwidth Portion Bitmap Partition Configuration Register, n = 0 - 127The MPAMCFG_MBW_PBM<n> characteristics are:

Purpose

The MPAMCFG_MBW_PBM<n> register array gives access to the memory bandwidth portion bitmap. Each register in the array is a read/write register that configures whether a PARTID is allowed to allocate bandwidth portions within a range.

The range of portions covered in MPAMCFG_MBW_PBM<n> is from portion 32n to portion 32n +31. After setting MPAMCFG_PART_SEL with a PARTID, software writes to one or more of the MPAMCFG_MBW_PBM<n> registers to configure with bandwidth portions the PARTID is allowed to allocate. The MPAMCFG_MBW_PBM<n> register that contains the bitmap bit corresponding to memory bandwidth portion p has n equal to p[11:5]. The field, P<x> of that MPAMCFG_MBW_PBM<n> register that contains the bitmap bit corresponding to memory bandwidth portion p has <x> equal to p[4:0].

The MPAMCFG_MBW_PBM<n>_s registers control the bandwidth portion bitmap for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. The MPAMCFG_MBW_PBM<n>_ns registers control the bandwidth portion bitmap for the Non-secure PARTID selected by the Non-secure instance of

MPAMCFG_PART_SEL. The MPAMCFG_MBW_PBM<n>_rt registers control the bandwidth portion bitmap for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. The MPAMCFG_MBW_PBM<n>_rl registers control the bandwidth portion bitmap for the Realm PARTID selected by the Realm instance of MPAMCFG_PART_SEL.

If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_MBW_PBM<n> is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MBW_PART == ‘1’)) && (MPAMF_MBW_IDR.HAS_PBM == ‘1’). Otherwise, direct accesses to MPAMCFG_MBW_PBM<n> are RES0.

Attributes

MPAMCFG_MBW_PBM<n> is a 32-bit register.

Field descriptions

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 0

| | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|P31|P30|P29|P28|P27|P26|P25|P24|P23|P22|P21|P20|P19|P18|P17|P16|P15|P14|P13|P12|P11|P10|P9|P8|P7|P6|P5|P4|P3|P2|P1|P0|


P<x> , bits [x], for x = 31 to 0

Portion allocation control bit. Each bandwidth portion allocation control bit MPAMCFG_MBW_PBMn.P<x> grants permission to the PARTID selected by MPAMCFG_PART_SEL to allocate bandwidth within bandwidth portion 32*n + <x>.

P<x> Meaning

0b0 The PARTID is not permitted to allocate into bandwidth portion 32*n + <x>. 0b1 The PARTID is permitted to allocate within bandwidth portion 32*n + <x>.

The number of bits in the bandwidth portion partitioning bit map of this component is given in MPAMF_MBW_IDR.BWPBM_WD. BWPBM_WD contains a value from 1 to 212, inclusive. Values of MPAMF_MBW_IDR.BWPBM_WD greater than 32 require a group of 32-bit registers to access the bandwidth portion bitmap, up to 128 32-bit registers. When ((n * 32) + x) > UInt(MPAMF_MBW_IDR().BWPBM_WD), access to this field is RES0.

Accessing MPAMCFG_MBW_PBM<n>

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_MBW_PBM<n>_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_MBW_PBM<n>_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_MBW_PBM<n>_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_MBW_PBM<n>_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_MBW_PBM<n>_s, MPAMCFG_MBW_PBM<n>_ns, MPAMCFG_MBW_PBM<n>_rt, and MPAMCFG_MBW_PBM<n>_rl must be separate registers:
- • The Secure instance (MPAMCFG_MBW_PBM<n>_s) accesses the memory bandwidth portion bitmap used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_MBW_PBM<n>_ns) accesses the memory bandwidth portion bitmap used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_MBW_PBM<n>_rt) accesses the memory bandwidth portion bitmap used for Root PARTIDs.
- • The Realm instance (MPAMCFG_MBW_PBM<n>_rl) accesses the memory bandwidth portion bitmap used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_MBW_PBM<n> access the memory bandwidth portion bitmap configuration settings for the bandwidth resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_MBW_PBM<n> access the memory bandwidth portion bitmap configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_MBW_PBM<n> access the memory bandwidth portion bitmap configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_MBW_PBM<n> access the memory bandwidth portion bitmap configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_MBW_PBM<n> can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x2000 + (4 *

MPAMCFG_MBW_PBM<n>_s

n)

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x2000 + (4 *

MPAMCFG_MBW_PBM<n>_ns

n)

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x2000 + (4 *

MPAMCFG_MBW_PBM<n>_rt

n)

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x2000 + (4 *

MPAMCFG_MBW_PBM<n>_rl

n)

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.6.12 MPAMCFG_MBW_PROP, MPAM Memory Bandwidth Proportional Stride Partition Configuration RegisterThe MPAMCFG_MBW_PROP characteristics are:

Purpose

Controls the proportional stride of memory bandwidth that the PARTID selected by MPAMCFG_PART_SEL uses.

MPAMCFG_MBW_PROP_s controls the bandwidth proportional stride for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_PROP_ns controls the bandwidth proportional stride for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_PROP_rt controls the bandwidth proportional stride for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_PROP_rl controls the bandwidth proportional stride for the Realm PARTID selected by the Realm instance of MPAMCFG_PART_SEL.

Proportional stride is a relative cost of bandwidth requested by one PARTID in relation to the costs of the bandwidths requested by each other PARTID also competing to use the bandwidth.

If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_MBW_PROP is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MBW_PART == ‘1’)) && (MPAMF_MBW_IDR.HAS_PROP == ‘1’). Otherwise, direct accesses to MPAMCFG_MBW_PROP are RES0.

Attributes

MPAMCFG_MBW_PROP is a 32-bit register.

Field descriptions

31 30 16 15 0

| | | |
|---|---|---|
|EN|RES0|STRIDEM1|


EN, bit [31]

Enable proportional stride bandwidth partitioning.

EN Meaning

- 0b0 The selected partition is not regulated by proportional stride bandwidth partitioning.

- 0b1 The selected partition has bandwidth usage regulated by proportional stride bandwidth partitioning as controlled by STRIDEM1.


Bits [30:16]

Reserved, RES0.

STRIDEM1, bits [15:0]

Memory bandwidth stride minus 1 allocated to the partition selected by MPAMCFG_PART_SEL. STRIDEM1 represents the normalized cost of bandwidth consumption by the partition.

The proportional stride partitioning control parameter is an unsigned integer representing the normalized cost to a partition for consuming bandwidth. Larger values have a larger cost and correspond to a lesser allocation of bandwidth while smaller values indicate a lesser cost and therefore a higher allocation of bandwidth.

The implemented width of STRIDEM1 is given in MPAMF_MBW_IDR.BWA_WD.

Accessing MPAMCFG_MBW_PROP

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_MBW_PROP_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_MBW_PROP_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_MBW_PROP_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_MBW_PROP_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_MBW_PROP_s, MPAMCFG_MBW_PROP_ns, MPAMCFG_MBW_PROP_rt, and MPAMCFG_MBW_PROP_rl must be separate registers:
- • The Secure instance (MPAMCFG_MBW_PROP_s) accesses the memory proportional stride bandwidth partitioning used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_MBW_PROP_ns) accesses the memory proportional stride bandwidth partitioning used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_MBW_PROP_rt) accesses the memory proportional stride bandwidth partitioning used for Root PARTIDs.
- • The Realm instance (MPAMCFG_MBW_PROP_rl) accesses the memory proportional stride bandwidth partitioning used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_MBW_PROP access the memory proportional stride bandwidth partitioning configuration settings for the bandwidth resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_MBW_PROP access the memory proportional stride bandwidth partitioning configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_MBW_PROP access the memory proportional stride bandwidth partitioning configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_MBW_PROP access the memory proportional stride bandwidth partitioning configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_MBW_PROP can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0500 MPAMCFG_MBW_PROP_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0500 MPAMCFG_MBW_PROP_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0500 MPAMCFG_MBW_PROP_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0500 MPAMCFG_MBW_PROP_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.6.13 MPAMCFG_MBW_WINWD, MPAM Memory Bandwidth Partitioning Window Width Configuration RegisterThe MPAMCFG_MBW_WINWD characteristics are:

Purpose

MPAMCFG_MBW_WINWD is a 32-bit register that shows and sets the value of the window width for the PARTID in MPAMCFG_PART_SEL.

MPAMCFG_MBW_WINWD_s reads and controls the bandwidth control window width for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_WINWD_ns reads and controls the bandwidth control window width for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_WINWD_rt reads and controls the bandwidth control window width for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. MPAMCFG_MBW_WINWD_rl reads and controls the bandwidth control window width for the Real PARTID selected by the Realm instance of MPAMCFG_PART_SEL.

MPAMCFG_MBW_WINWD is read-only if MPAMF_MBW_IDR.WINDWR == 0, and the window width is set by the hardware, even if variable.

MPAMCFG_MBW_WINWD is read/write if MPAMF_MBW_IDR.WINDWR == 1, permitting configuration of the window width for each PARTID independently on hardware that supports this functionality.

If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_MBW_WINWD is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component.

This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented) and MPAMF_IDR.HAS_MBW_PART == ‘1’. Otherwise, direct accesses to MPAMCFG_MBW_WINWD are RES0.

Attributes

MPAMCFG_MBW_WINWD is a 32-bit register.

Field descriptions

31 24 23 8 7 0

| | | |
|---|---|---|
|RES0|US_INT|US_FRAC|


Bits [31:24]

Reserved, RES0.

US_INT, bits [23:8]

Window width, integer microseconds. This field reads (and sets) the integer part of the window width in microseconds for the PARTID selected by MPAMCFG_PART_SEL.

US_FRAC, bits [7:0]

Window width, fractional microseconds. This field reads (and sets) the fractional part of the window width in microseconds for the PARTID selected by MPAMCFG_PART_SEL.

Accessing MPAMCFG_MBW_WINWD

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_MBW_WINWD_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_MBW_WINWD_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_MBW_WINWD_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_MBW_WINWD_rl must only be accessible from the Realm MPAM feature page.


• MPAMCFG_MBW_WINWD_s, MPAMCFG_MBW_WINWD_ns, MPAMCFG_MBW_WINWD_rt, and MPAMCFG_MBW_WINWD_rl must be separate registers:

- • The Secure instance (MPAMCFG_MBW_WINWD_s) accesses the window width used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_MBW_WINWD_ns) accesses the window width used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_MBW_WINWD_rt) accesses the window width used for Root PARTIDs.
- • The Realm instance (MPAMCFG_MBW_WINWD_rl) accesses the window width used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_MBW_WINWD access the window width configuration settings for the bandwidth resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_MBW_WINWD access the window width configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_MBW_WINWD access the window width configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_MBW_WINWD access the window width configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_MBW_WINWD can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0220 MPAMCFG_MBW_WINWD_s

Accessible as follows:

• When MPAMF_MBW_IDR.WINDWR == ‘0’, accesses to this register are RO. • When MPAMF_MBW_IDR.WINDWR == ‘1’, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0220 MPAMCFG_MBW_WINWD_ns

Accessible as follows:

- • When MPAMF_MBW_IDR.WINDWR == ‘0’, accesses to this register are RO.
- • When MPAMF_MBW_IDR.WINDWR == ‘1’, accesses to this register are RW.


Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0220 MPAMCFG_MBW_WINWD_rt

Accessible as follows:

• When FEAT_RME is implemented and MPAMF_MBW_IDR.WINDWR == ‘0’, accesses to this register are RO. • When FEAT_RME is implemented and MPAMF_MBW_IDR.WINDWR == ‘1’, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0220 MPAMCFG_MBW_WINWD_rl

Accessible as follows:

• When FEAT_RME is implemented and MPAMF_MBW_IDR.WINDWR == ‘0’, accesses to this register are RO. • When FEAT_RME is implemented and MPAMF_MBW_IDR.WINDWR == ‘1’, accesses to this register are RW.

###### 9.6.14 MPAMCFG_PART_SEL, MPAM Partition Configuration Selection RegisterThe MPAMCFG_PART_SEL characteristics are:

Purpose

Selects a partition ID to configure. MPAMCFG_PART_SEL_s selects a Secure PARTID to configure. MPAMCFG_PART_SEL_ns selects a Non-secure PARTID to configure. MPAMCFG_PART_SEL_rt selects a Root PARTID to configure. MPAMCFG_PART_SEL_rl selects a Realm PARTID to configure. After setting this register with a PARTID, software (usually a hypervisor) can perform a series of accesses to MPAMCFG registers to configure parameters for MPAM resource controls to use when requests have that PARTID.

Configuration

The power domain of MPAMCFG_PART_SEL is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (((((((MPAMF_IDR.HAS_CCAP_PART == ‘1’) || (MPAMF_IDR.HAS_CPOR_PART == ‘1’)) || (MPAMF_IDR.HAS_MBW_PART == ‘1’)) || (MPAMF_IDR.HAS_PRI_PART == ‘1’)) || (MPAMF_IDR.HAS_PARTID_NRW == ‘1’)) || ((MPAMF_IDR.EXT == ‘0’) && (MPAMF_IDR.HAS_IMPL_IDR == ‘1’))) || (((MPAMF_IDR.EXT == ‘1’) && (MPAMF_IDR.HAS_IMPL_IDR == ‘1’)) && (MPAMF_IDR.NO_IMPL_PART == ‘0’))). Otherwise, direct accesses to MPAMCFG_PART_SEL are RES0.

Attributes

MPAMCFG_PART_SEL is a 32-bit register.

Field descriptions

31 28 27 24 23 19 18 17 16 15 0

| | | | | | | |
|---|---|---|---|---|---|---|
|RES0|RIS|RES0| | | |PARTID_SEL|


DEFAULT_PARTID INTERNAL INGRESS_TL

Bits [31:28]

Reserved, RES0.

RIS, bits [27:24] When ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p1)) && (MPAMF_IDR.EXT == '1')) && (MPAMF_IDR.HAS_RIS == '1'):

Resource Instance Selector. RIS selects one resource to configure through MPAMCFG registers and describe with MPAMF ID registers.

Otherwise:

Reserved, RES0.

Bits [23:19]

Reserved, RES0.

DEFAULT_PARTID, bit [18]

When FEAT_MPAM_MSC_DCTRL is implemented:

Disables PARTID selection and instead configures the default resource control behavior.

DEFAULT_PARTID Meaning

0b0 PARTID_SEL is interpreted as a request PARTID. 0b1 PARTID_SEL is ignored and any access to MPAMCFG

registers is intended to be for configuring the default resource control behaviour.

Otherwise:

Reserved, RES0.

INGRESS_TL, bit [17] When FEAT_MPAM_MSC_DOMAINS is implemented:

Indicates that PARTID_SEL is used for ingress translation.

INGRESS_TL Meaning

- 0b0 PARTID_SEL is interpreted as a request PARTID for configuring MSC controls.

- 0b1 PARTID_SEL is interpreted as an untranslated ingress request PARTID to be used for configuring direct translations.


When MPAMF_IDR.HAS_IN_TL == ‘0’, access to this field is RAZ/WI.

Otherwise:

Reserved, RES0.

INTERNAL, bit [16]

Internal PARTID. If MPAMF_IDR.HAS_PARTID_NRW == 0b1:

INTERNAL Meaning

- 0b0 PARTID_SEL is interpreted as a request PARTID and ignored except for use with MPAMCFG_INTPARTID register access.

- 0b1 PARTID_SEL is interpreted as an internal PARTID and used for access to MPAMCFG control settings except for MPAMCFG_INTPARTID.


If PARTID narrowing is implemented as indicated by MPAMF_IDR.HAS_PARTID_NRW = 1, when accessing other MPAMCFG registers the value of the MPAMCFG_PART_SEL.INTERNAL bit is checked for these conditions:

- • When the MPAMCFG_INTPARTID register is read or written, if the value of


- MPAMCFG_PART_SEL.INTERNAL is not 0, an Unexpected_INTERNAL error is set in MPAMF_ESR.


- • When an MPAMCFG register other than MPAMCFG_INTPARTID is read or written, if the value of


- MPAMCFG_PART_SEL.INTERNAL is not 1, MPAMF_ESR is set to indicate an intPARTID_Range error.


In either error case listed here, the value returned by a read operation is UNPREDICTABLE, and the control settings are not affected by a write.

When MPAMF_IDR.HAS_PARTID_NRW == ‘0’, access to this field is RAZ/WI.

PARTID_SEL, bits [15:0]

Selects the partition ID to configure. Reads and writes to other MPAMCFG registers are indexed by PARTID_SEL and by the NS bit used to access MPAMCFG_PART_SEL to access the configuration for a single partition.

Accessing MPAMCFG_PART_SEL

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_PART_SEL_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_PART_SEL_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_PART_SEL_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_PART_SEL_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_PART_SEL_s, MPAMCFG_PART_SEL_ns, MPAMCFG_PART_SEL_rt, and MPAMCFG_PART_SEL_rl must be separate registers:
- • The Secure instance (MPAMCFG_PART_SEL_s) accesses the PARTID selector used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_PART_SEL_ns) accesses the PARTID selector used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_PART_SEL_rt) accesses the PARTID selector used for Root PARTIDs.
- • The Realm instance (MPAMCFG_PART_SEL_rl) accesses the PARTID selector used for Realm PARTIDs.


MPAMCFG_PART_SEL can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0100 MPAMCFG_PART_SEL_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0100 MPAMCFG_PART_SEL_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0100 MPAMCFG_PART_SEL_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0100 MPAMCFG_PART_SEL_rl

When FEAT_RME is implemented, accesses to this register are RW.

- 9.6.15 MPAMCFG_PRI, MPAM Priority Partition Configuration Register The MPAMCFG_PRI characteristics are:


Purpose

Controls the internal and downstream priority of requests attributed to the PARTID selected by MPAMCFG_PART_SEL.

MPAMCFG_PRI_s controls the priorities for the Secure PARTID selected by the Secure instance of MPAMCFG_PART_SEL. MPAMCFG_PRI_ns controls the priorities for the Non-secure PARTID selected by the Non-secure instance of MPAMCFG_PART_SEL. MPAMCFG_PRI_rt controls the priorities for the Root PARTID selected by the Root instance of MPAMCFG_PART_SEL. MPAMCFG_PRI_rl controls the priorities for the Realm PARTID selected by the Realm instance of MPAMCFG_PART_SEL.

If MPAMF_IDR.HAS_RIS is 1, the control settings accessed are those of the resource instance currently selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

Configuration

The power domain of MPAMCFG_PRI is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented) and MPAMF_IDR.HAS_PRI_PART == ‘1’. Otherwise, direct accesses to MPAMCFG_PRI are RES0.

Attributes

MPAMCFG_PRI is a 32-bit register.

Field descriptions

31 16 15 0

| | |
|---|---|
|DSPRI|INTPRI|


DSPRI, bits [31:16]

Downstream priority. If MPAMF_PRI_IDR.HAS_DSPRI == 0, bits of this field are RES0 as this field is not used. If MPAMF_PRI_IDR.HAS_DSPRI == 1, this field is a priority value applied to downstream communications from this MSC for transactions of the partition selected by MPAMCFG_PART_SEL. The implemented width of this field is MPAMF_PRI_IDR.DSPRI_WD bits. If the implemented width is less than the width of this field, the least significant bits are used. The encoding of priority is 0-as-lowest or 0-as-highest priority according to the value of MPAMF_PRI_IDR.DSPRI_0_IS_LOW.

INTPRI, bits [15:0]

Internal priority. If MPAMF_PRI_IDR.HAS_INTPRI == 0, bits of this field are RES0 as this field is not used. If MPAMF_PRI_IDR.HAS_INTPRI == 1, this field is a priority value applied internally inside this MSC for transactions of the partition selected by MPAMCFG_PART_SEL. The implemented width of this field is MPAMF_PRI_IDR.INTPRI_WD bits. If the implemented width is less than the width of this field, the least significant bits are used. The encoding of priority is 0-as-lowest or 0-as-highest priority according to the value of MPAMF_PRI_IDR.INTPRI_0_IS_LOW.

Accessing MPAMCFG_PRI

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_PRI_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_PRI_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_PRI_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_PRI_rl must only be accessible from the Realm MPAM feature page.


• MPAMCFG_PRI_s, MPAMCFG_PRI_ns, MPAMCFG_PRI_rt, and MPAMCFG_PRI_rl must be separate registers:

• The Secure instance (MPAMCFG_PRI_s) accesses the priority partitioning used for Secure PARTIDs. • The Non-secure instance (MPAMCFG_PRI_ns) accesses the priority partitioning used for Non-secure

PARTIDs.

- • The Root instance (MPAMCFG_PRI_rt) accesses the priority partitioning used for Root PARTIDs.
- • The Realm instance (MPAMCFG_PRI_rl) accesses the priority partitioning used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_PRI access the priority partitioning configuration settings for the priority resource instance selected by MPAMCFG_PART_SEL.RIS and the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When RIS is not implemented, loads and stores to MPAMCFG_PRI access the priority partitioning configuration settings for the PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL.

When PARTID narrowing is implemented, loads and stores to MPAMCFG_PRI access the priority partitioning configuration settings for the internal PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 1.

When PARTID narrowing is not implemented, loads and stores to MPAMCFG_PRI access the priority partitioning configuration settings for the request PARTID selected by MPAMCFG_PART_SEL.PARTID_SEL, and MPAMCFG_PART_SEL.INTERNAL must be 0.

MPAMCFG_PRI can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0400 MPAMCFG_PRI_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0400 MPAMCFG_PRI_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0400 MPAMCFG_PRI_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0400 MPAMCFG_PRI_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7 Memory-mapped Monitoring_config register descriptionThis section lists the external monitoring configuration registers.

###### 9.7.1 MSMON_CAPT_EVNT, MPAM Capture Event Generation RegisterThe MSMON_CAPT_EVNT characteristics are:

Purpose

Generates a local capture event when written with bit[0] as 1. MSMON_CAPT_EVNT_s generates local capture events for Secure monitor instances only or for Secure and Non-secure monitor instances. MSMON_CAPT_EVNT_ns generates local capture events for Non-secure monitor instances only. MSMON_CAPT_EVNT_rt generates local capture events for Root monitor instances only or for Root, Secure, Realm, and Non-secure monitor instances. MSMON_CAPT_EVNT_rl generates local capture events for Realm monitor instances or for for Realm monitor instances or Realm and Non-secure monitor instances.

Configuration

The power domain of MSMON_CAPT_EVNT is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.HAS_LOCAL_CAPT_EVNT == ‘1’). Otherwise, direct accesses to MSMON_CAPT_EVNT are RES0.

Attributes

MSMON_CAPT_EVNT is a 32-bit register.

Field descriptions

31 2 0

| | | |
|---|---|---|
|RES0|ALL|NOW|


Bits [31:2]

Reserved, RES0.

ALL, bit [1]

In the Secure instance of this register:

- • If ALL is written as 1 and NOW is also written as 1, signal a capture event to Secure and Non-secure monitor instances in this MSC that are configured with CAPT_EVNT = 7.
- • If ALL is written as 0 and NOW is written as 1, signal a capture event to Secure monitor instances in this


MSC that are configured with CAPT_EVNT = 7. In the Non-secure instance of this register, this field is RAZ/WI. In the Root instance of this register:

- • If ALL is written as 1 and NOW is also written as 1, signal a capture event to Root, Realm, Secure, and Non-secure monitor instances in this MSC that are configured with CAPT_EVNT = 7.
- • If ALL is written as 0 and NOW is written as 1, signal a capture event to Root monitor instances within this


MSC that are configured with CAPT_EVNT = 7. In the Realm instance of this register:

- • If ALL is written as 1 and NOW is also written as 1, signal a capture event to Realm and Non-secure monitor instances in this MSC that are configured with CAPT_EVNT = 7.


- • If ALL is written as 0 and NOW is written as 1, signal a capture event to Realm monitor instances within this MSC that are configured with CAPT_EVNT = 7.


This bit always reads as zero.

ALL Meaning

- 0b0 Send capture event only to monitor instances in the same MPAM feature page as this register.

- 0b1 Send capture event to monitor instances in certain MPAM feature pages as described in the ALL field of this register.


NOW, bit [0]

When written as 1, this bit causes an event to those monitor instances described in the ALL field that have CAPT_EVNT set to the value of 7.

When this bit is written as 0, no event is signaled. This bit always reads as zero.

Accessing MSMON_CAPT_EVNT

This register is within the MPAM feature page memory frames. In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.

MSMON_CAPT_EVNT_s must only be accessible from the Secure MPAM feature page. MSMON_CAPT_EVNT_ns must only be accessible from the Non-secure MPAM feature page.

MSMON_CAPT_EVNT_s and MSMON_CAPT_EVNT_ns must be separate registers. The Secure instance (MSMON_CAPT_EVNT_s) can generate local capture events for Secure monitor instances only or for Secure and Non-secure monitor instances, and the Non-secure instance (MSMON_CAPT_EVNT_ns) can generate local capture events for Non-secure monitor instances only.

MSMON_CAPT_EVNT can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0808 MSMON_CAPT_EVNT_s

Accesses to this register are WO/RAZ.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0808 MSMON_CAPT_EVNT_ns

Accesses to this register are WO/RAZ.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0808 MSMON_CAPT_EVNT_rt

When FEAT_RME is implemented, accesses to this register are WO/RAZ.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0808 MSMON_CAPT_EVNT_rl

When FEAT_RME is implemented, accesses to this register are WO/RAZ.

- 9.7.2 MSMON_CFG_CSU_CTL, MPAM Memory System Monitor Configure Cache Storage Usage Monitor Control Register


The MSMON_CFG_CSU_CTL characteristics are:

Purpose

Controls the CSU monitor selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_CFG_CSU_CTL is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_CFG_CSU_CTL_s controls the Secure cache storage usage monitor instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_CFG_CSU_CTL_ns controls Non-secure cache storage usage monitor instance selected by the Non-secure instance of MSMON_CFG_MON_SEL.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_CFG_CSU_CTL_rt controls the monitor configuration for the Root PARTID selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_CFG_CSU_CTL_rl controls the monitor configuration for the Realm PARTID selected by the


Realm instance of MSMON_CFG_MON_SEL. If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance configuration accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_CSU == ‘1’). Otherwise, direct accesses to MSMON_CFG_CSU_CTL are RES0.

Attributes

MSMON_CFG_CSU_CTL is a 32-bit register.

Field descriptions

31 30 28 27 26 25 24 23 22 20 19 18 17 16 15 11 10 8 7 0

| | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|EN|CAPT_EVNT| | | | | |SUBTYPE| | | | |RES0| |0 1 0 0 0 0 1 1|


CAPT_RESET OFLOW_STATUS OFLOW_INTR OFLOW_FRZ OFLOW_CAPT

TYPE OFLOW_LNKG

MATCH_PARTID MATCH_PMG

CEVNT_OFLW RES0

EN, bit [31]

Enabled.

EN Meaning

0b0 The monitor instance is disabled and must not collect any information. 0b1 The monitor instance is enabled to collect information according to the configuration of the instance.

CAPT_EVNT, bits [30:28]

Capture event selector. Select the event that triggers capture from the following:

CAPT_EVNT Meaning

0b000 No capture event is triggered. 0b001 External capture event 1 (optional, but recommended)

- 0b010 External capture event 2 (optional)

- 0b011 External capture event 3 (optional) 0b100 External capture event 4 (optional) 0b101 External capture event 5 (optional) 0b110 External capture event 6 (optional)


0b111 Capture occurs when a MSMON_CAPT_EVNT register in this MSC is written and causes a capture event for the Security state of this monitor. (optional)

The values marked as optional indicate capture event sources that can be omitted in an implementation. Those values representing non-implemented event sources must not trigger a capture event.

When MPAMF_CSUMON_IDR.HAS_CAPTURE == ‘0’, access to this field is RAZ/WI.

CAPT_RESET, bit [27]

Reset after capture. Controls whether the value of MSMON_CSU is reset to zero immediately after being copied to MSMON_CSU_CAPTURE.

CAPT_RESET Meaning

- 0b0 Monitor is not reset on capture.

- 0b1 Monitor is reset on capture.


Because the CSU monitor type produces a measurement rather than a count, it might not make sense to ever reset the value after a capture. If there is no reason to ever reset a CSU monitor, this field is RAZ/WI.

When MPAMF_CSUMON_IDR.HAS_CAPTURE == ‘0’, access to this field is RAZ/WI.

OFLOW_STATUS, bit [26]

Overflow status.

Indicates whether the value of MSMON_CSU has overflowed. If MPAMF_CSUMON_IDR.HAS_CEVNT_OFLW is 1 or MPAMF_CSUMON_IDR.HAS_OFLOW_LNKG is 1, then a store to MSMON_CSU when this field is 1 resets this field to 0.

OFLOW_STATUS Meaning

0b0 No overflow has occurred. 0b1 At least one overflow has occurred since this bit was

last written to zero.

If overflow is not possible for a CSU monitor in the implementation, this field is RAZ/WI.

OFLOW_INTR, bit [25]

Overflow Interrupt. Controls whether an overflow interrupt is generated when the value of MSMON_CSU has overflowed.

OFLOW_INTR Meaning

0b0 No interrupt is signaled on an overflow of MSMON_CSU. 0b1 On overflow, an implementation-specific interrupt is signaled.

When MSMON_CFG_CSU_CTL.OFLOW_INTR == ‘0’, access to this field is RAZ/WI.

OFLOW_FRZ, bit [24]

Freeze Monitor on Overflow. Controls whether the value of MSMON_CSU.VALUE freezes on an overflow.

OFLOW_FRZ Meaning

0b0 Monitor count wraps on overflow. 0b1 Monitor count freezes on overflow. The frozen value might be 0 or

another value if the monitor overflowed with an increment larger than 1.

If overflow is not possible for a CSU monitor in the implementation, this field is RAZ/WI. When a MSMON_CSU.VALUE of a monitor instance is frozen it does not change until MSMON_CSU register for that instance has been written.

OFLOW_CAPT, bit [23] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_CSUMON_IDR.HAS_OFLOW_CAPT == '1':

Capture Monitor on Overflow.

OFLOW_CAPT Meaning

0b0 Monitor is not captured on an overflow or when affected by an overflow linkage event. 0b1 Monitor is captured and the MSMON_CSU.{NRDY, VALUE} fields are copied to the monitor

instance’s capture register on an overflow or when affected by an overflow linkage event. The monitor instance treats an overflow of this monitor instance as a private capture event. If MSMON_CFG_CSU_CTL.CEVNT_OFLW is 1, this monitor instance also treats an overflow linkage event as a capture event. If the OFLOW_FRZ field is 1, the monitor does not continue to count after the overflow or overflow linkage event. If the CAPT_RESET field is 1, the monitor instance resets to 0.

Otherwise:

Reserved, RES0.

SUBTYPE, bits [22:20]

Subtype. Type of cache storage usage counted by this monitor. This field is not currently used for CSU monitors, but reserved for future use. This field is RAZ/WI.

Bit [19]

Reserved, RES0.

CEVNT_OFLW, bit [18] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_CSUMON_IDR.HAS_CEVNT_OFLW == '1':

Capture Event performs overflow behavior.

CEVNT_OFLW Meaning

- 0b0 On a capture event matching the CAPT_EVNT field, the capture behaviors are performed. The MSMON_CSU.{VALUE, NRDY} fields are transferred to the monitor instance’s capture register.

- 0b1 On a capture event matching the CAPT_EVNT field, the monitor instance treats a capture event as an overflow and the overflow behaviors are performed. The behavior is controlled by the MSMON_CFG_CSU_CTL.{OFLOW_FRZ, OFLOW_CAPT, CAPT_RESET} fields. The MSMON_CFG_CSU_CTL.OFLOW_STATUS field is set for this monitor instance.


Otherwise:

Reserved, RES0.

MATCH_PMG, bit [17] When MPAMF_CSUMON_IDR.NO_MATCH_PMG == '0':

Match PMG. Controls whether the monitor measures only storage used with PMG matching MSMON_CFG_CSU_FLT.PMG.

MATCH_PMG Meaning

0b0 The monitor measures storage used with any PMG value. 0b1 The monitor only measures storage used with the PMG value matching MSMON_CFG_CSU_FLT.PMG.

If MATCH_PMG is 1 and MATCH_PARTID is 0, usage is reported for requests with a PMG matching MSMON_CFG_CSU_FLT.PMG regardless of their PARTID value.

Otherwise:

Reserved, RES0.

MATCH_PARTID, bit [16] When MPAMF_CSUMON_IDR.NO_MATCH_PARTID == '0':

Match PARTID. Controls whether the monitor measures only storage used with PARTID matching MSMON_CFG_CSU_FLT.PARTID.

MATCH_PARTID Meaning

0b0 The monitor measures storage used with any PARTID value. 0b1 The monitor only measures storage used with the PARTID value matching

MSMON_CFG_CSU_FLT.PARTID.

Otherwise:

Reserved, RES0.

Bits [15:11]

Reserved, RES0.

OFLOW_LNKG, bits [10:8] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_CSUMON_IDR.HAS_OFLOW_LNKG == '1':

Overflow linkage event. Controls signaling of a capture event on overflow of this monitor instance.

OFLOW_LNKG Meaning

- 0b000 Overflow of the monitor instance only affects this monitor instance.

- 0b001 Overflow of this monitor instance signals Capture Event 1.


- 0b010 Overflow of this monitor instance signals Capture Event 2.

- 0b011 Overflow of this monitor instance signals Capture Event 3.


- 0b100 Overflow of this monitor instance signals Capture Event 4.

- 0b101 Overflow of this monitor instance signals Capture Event 5.


OFLOW_LNKG Meaning

0b110 Overflow of this monitor instance signals Capture Event 6. 0b111 Reserved.

Otherwise:

Reserved, RES0.

TYPE, bits [7:0]

Monitor Type Code. The CSU monitor is TYPE = 0x43. TYPE is a read-only constant indicating the type of the monitor. Reads as 0x43 Access to this field is RO.

Accessing MSMON_CFG_CSU_CTL

This register is within the MPAM feature page memory frames. If FEAT_MPAM is implemented, the following statements apply:

- • In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.
- • MSMON_CFG_CSU_CTL_s must only be accessible from the Secure MPAM feature page. MSMON_CFG_CSU_CTL_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_CSU_CTL_s and MSMON_CFG_CSU_CTL_ns must be separate registers. The Secure instance (MSMON_CFG_CSU_CTL_s) accesses the cache storage usage monitor controls used for Secure PARTIDs, and the Non-secure instance (MSMON_CFG_CSU_CTL_ns) accesses the cache storage usage monitor controls used for Non-secure PARTIDs.


If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_CFG_CSU_CTL_s must only be accessible from the Secure MPAM feature page.
- • MSMON_CFG_CSU_CTL_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_CSU_CTL_rt must only be accessible from the Root MPAM feature page.
- • MSMON_CFG_CSU_CTL_rl must only be accessible from the Realm MPAM feature page.


- • MSMON_CFG_CSU_CTL_s, MSMON_CFG_CSU_CTL_ns, MSMON_CFG_CSU_CTL_rt, and MSMON_CFG_CSU_CTL_rl must be separate registers:
- • The Secure instance (MSMON_CFG_CSU_CTL_s) accesses the cache storage usage monitor controls used for Secure PARTIDs.
- • The Non-secure instance (MSMON_CFG_CSU_CTL_ns) accesses the cache storage usage monitor controls used for Non-secure PARTIDs.
- • The Root instance (MSMON_CFG_CSU_CTL_rt) accesses the cache storage usage monitor controls used for Root PARTIDs.
- • The Realm instance (MSMON_CFG_CSU_CTL_rl) accesses the cache storage usage monitor controls used for Realm PARTIDs.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

- • When RIS is implemented, loads and stores to MSMON_CFG_CSU_CTL access the cache storage usage monitor configuration settings for the cache resource instance selected by MSMON_CFG_MON_SEL.RIS and the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, loads and stores to MSMON_CFG_CSU_CTL access the cache storage usage monitor configuration settings for the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Loads and stores to MSMON_CFG_CSU_CTL access the cache storage usage monitor configuration settings for the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_CFG_CSU_CTL can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0818 MSMON_CFG_CSU_CTL_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0818 MSMON_CFG_CSU_CTL_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0818 MSMON_CFG_CSU_CTL_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0818 MSMON_CFG_CSU_CTL_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.3 MSMON_CFG_CSU_FLT, MPAM Memory System Monitor Configure Cache Storage Usage Monitor Filter RegisterThe MSMON_CFG_CSU_FLT characteristics are:

Purpose

Configures PARTID and PMG to measure or count in the CSU monitor selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_CFG_CSU_FLT is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_CFG_CSU_FLT_s sets filter conditions for the Secure cache storage usage monitor instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_CFG_CSU_CTL_ns sets filter conditions for the Non-secure cache storage usage monitor instance selected by the Non-secure instance of MSMON_CFG_MON_SEL.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_CFG_CSU_FLT_rt sets the filter conditions for the Root PARTID selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_CFG_CSU_FLT_rl sets the filter conditions for the Realm PARTID selected by the Realm


instance of MSMON_CFG_MON_SEL. If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance filter configuration accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_CSU == ‘1’). Otherwise, direct accesses to MSMON_CFG_CSU_FLT are RES0.

Attributes

MSMON_CFG_CSU_FLT is a 32-bit register.

Field descriptions

###### When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

31 30 24 23 16 15 0

| | | | |
|---|---|---|---|
|XCL|RES0|PMG|PARTID|


XCL, bit [31] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_CSUMON_IDR.HAS_XCL == '1':

Exclude Clean. The monitor instance does not count cache storage used by lines in an unmodified cache state.

XCL Meaning

0b0 Monitor instance counts cache storage in modified and unmodified cache lines. 0b1 Monitor instance counts cache storage in modified cache lines only.

Otherwise:

Reserved, RES0.

Bits [30:24]

Reserved, RES0.

PMG, bits [23:16]

Performance monitoring group to filter cache storage usage monitoring. If MSMON_CFG_CSU_CTL.MATCH_PMG is 0, this field is not used to match cache storage to a PMG and the contents of this field is ignored. If MSMON_CFG_CSU_CTL.MATCH_PMG is 1 and MSMON_CFG_CSU_CTL.MATCH_PARTID is 1, the monitor instance selected by MSMON_CFG_MON_SEL measures or counts cache storage labeled with PMG equal to this field and PARTID equal to the PARTID field. If MSMON_CFG_CSU_CTL.MATCH_PMG is 1 and MSMON_CFG_CSU_CTL.MATCH_PARTID is 0, the behavior of the monitor instance selected by MSMON_CFG_MON_SEL is CONSTRAINED UNPREDICTABLE. See MSMON_CFG_CSU_CTL.MATCH_PMG for more information.

PARTID, bits [15:0]

Partition ID to filter cache storage usage monitoring. If MSMON_CFG_CSU_CTL.MATCH_PARTID is 0 and MSMON_CFG_CSU_CTL.MATCH_PMG is 0, the monitor measures all allocated cache storage. If MSMON_CFG_CSU_CTL.MATCH_PARTID is 0 and MSMON_CFG_CSU_CTL.MATCH_PMG is 1, the behavior of the monitor is CONSTRAINED UNPREDICTABLE. See the description of MSMON_CFG_CSU_CTL.MATCH_PMG.

- If MSMON_CFG_CSU_CTL.MATCH_PARTID is 1 and MSMON_CFG_CSU_CTL.MATCH_PMG is 0, the monitor selected by MSMON_CFG_MON_SEL measures or counts cache storage labeled with PARTID equal to this field.
- If MSMON_CFG_CSU_CTL.MATCH_PARTID is 1 and MSMON_CFG_CSU_CTL.MATCH_PMG is 1, the monitor selected by MSMON_CFG_MON_SEL measures or counts cache storage labeled with PARTID equal to this field and PMG equal to the PMG field.


Accessing MSMON_CFG_CSU_FLT

This register is within the MPAM feature page memory frames. If FEAT_MPAM is implemented, the following statements apply:

- • In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.
- • MSMON_CFG_CSU_FLT_s must only be accessible from the Secure MPAM feature page. MSMON_CFG_CSU_FLT_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_CSU_FLT_s and MSMON_CFG_CSU_FLT_ns must be separate registers. The Secure instance (MSMON_CFG_CSU_FLT_s) accesses the PARTID and PMG matching for a cache storage usage monitor used for Secure PARTIDs, and the Non-secure instance (MSMON_CFG_CSU_FLT_ns) accesses the PARTID and PMG matching for a cache storage usage monitor used for Non-secure PARTIDs.


If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:


- • MSMON_CFG_CSU_FLT_s must only be accessible from the Secure MPAM feature page.
- • MSMON_CFG_CSU_FLT_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_CSU_FLT_rt must only be accessible from the Root MPAM feature page.
- • MSMON_CFG_CSU_FLT_rl must only be accessible from the Realm MPAM feature page.

- • MSMON_CFG_CSU_FLT_s, MSMON_CFG_CSU_FLT_ns, MSMON_CFG_CSU_FLT_rt, and MSMON_CFG_CSU_FLT_rl must be separate registers:


- • The Secure instance (MSMON_CFG_CSU_FLT_s) accesses the PARTID and PMG matching for a cache storage usage monitor used for Secure PARTIDs.
- • The Non-secure instance (MSMON_CFG_CSU_FLT_ns) accesses the PARTID and PMG matching for a cache storage usage monitor used for Non-secure PARTIDs.
- • The Root instance (MSMON_CFG_CSU_FLT_rt) accesses the PARTID and PMG matching for a cache storage usage monitor used for Root PARTIDs.
- • The Realm instance (MSMON_CFG_CSU_FLT_rl) accesses the PARTID and PMG matching for a cache storage usage monitor used for Realm PARTIDs.


If any one of these features is implemented:

- • FEAT_MPAMv0p1
- • FEAT_MPAMv1p1


then, the following statements apply:

- • When RIS is implemented, loads and stores to MSMON_CFG_CSU_FLT access the monitor configuration settings for the resource instance selected by MSMON_CFG_MON_SEL.RIS and the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, loads and stores to MSMON_CFG_CSU_FLT access the monitor configuration settings for the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Loads and stores to MSMON_CFG_CSU_FLT access the monitor configuration settings for the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_CFG_CSU_FLT can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0810 MSMON_CFG_CSU_FLT_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0810 MSMON_CFG_CSU_FLT_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0810 MSMON_CFG_CSU_FLT_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0810 MSMON_CFG_CSU_FLT_rl

When FEAT_RME is implemented, accesses to this register are RW.

- 9.7.4 MSMON_CFG_MBWU_CTL, MPAM Memory System Monitor Configure Memory Bandwidth Usage Monitor Control Register


The MSMON_CFG_MBWU_CTL characteristics are:

Purpose

Controls the MBWU monitor selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_CFG_MBWU_CTL is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_CFG_MBWU_CTL_s controls the Secure memory bandwidth usage monitor monitor instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_CFG_MBWU_CTL_ns controls Non-secure memory bandwidth usage monitor monitor instance


selected by the Non-secure instance of MSMON_CFG_MON_SEL. • If FEAT_RME is implemented, the following statements also apply:

- • MSMON_CFG_MBWU_CTL_rt controls the monitor configuration for the Root PARTID selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_CFG_MBWU_CTL_rl controls the monitor configuration for the Realm PARTID selected by


the Realm instance of MSMON_CFG_MON_SEL. If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance configuration accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_MBWU == ‘1’). Otherwise, direct accesses to MSMON_CFG_MBWU_CTL are RES0.

Attributes

MSMON_CFG_MBWU_CTL is a 32-bit register.

Field descriptions

31 30 28 27 26 25 24 23 22 20 19 18 17 16 15 14 13 12 11 10 8 7 0

| | | | | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|EN|CAPT_EVNT| | | | | |SUBTYPE| | | | | | | |RES0| |0 1 0 0 0 0 1 0|


CAPT_RESET OFLOW_STATUS OFLOW_INTR OFLOW_FRZ OFLOW_CAPT

TYPE OFLOW_LNKG

OFLOW_CAPT_L OFLOW_INTR_L

OFLOW_STATUS_L MATCH_PARTID

SCLEN CEVNT_OFLW

MATCH_PMG

###### EN, bit [31]

Enabled.

EN Meaning

0b0 The monitor instance is disabled and must not collect any information. 0b1 The monitor instance is enabled to collect information according to the configuration of the instance.

CAPT_EVNT, bits [30:28]

Capture event selector. When the selected capture event occurs, MSMON_MBWU of the monitor instance is copied to MSMON_MBWU_CAPTURE of the same instance. If the long counter is also implemented, MSMON_MBWU_L is also copied to MSMON_MBWU_L_CAPTURE. Select the event that triggers capture from the following:

CAPT_EVNT Meaning

0b000 No capture event is triggered. 0b001 External capture event 1 (optional, but recommended) 0b010 External capture event 2 (optional) 0b011 External capture event 3 (optional) 0b100 External capture event 4 (optional) 0b101 External capture event 5 (optional) 0b110 External capture event 6 (optional) 0b111 Capture occurs when a MSMON_CAPT_EVNT register in this MSC is written and

causes a capture event for the Security state of this monitor. (optional)

The values marked as optional indicate capture event sources that can be omitted in an implementation. Those values representing non-implemented event sources must not trigger a capture event.

When MPAMF_MBWUMON_IDR.HAS_CAPTURE == ‘0’, access to this field is RAZ/WI.

CAPT_RESET, bit [27]

Reset MSMON_MBWU.VALUE after capture. Controls whether the VALUE field of the monitor instance is reset to zero immediately after being copied to the corresponding capture register.

CAPT_RESET Meaning

- 0b0 MSMON_MBWU.VALUE field of the monitor instance is not reset on capture.

- 0b1 MSMON_MBWU.VALUE field of the monitor instance is reset on capture.


This control bit affects both MSMON_MBWU and MSMON_MBWU_L in implementations that include MSMON_MBWU_L.

When MPAMF_MBWUMON_IDR.HAS_CAPTURE == ‘0’, access to this field is RAZ/WI.

OFLOW_STATUS, bit [26]

Overflow status. Indicates whether the value of MSMON_MBWU has overflowed.

OFLOW_STATUS Meaning

0b0 MSMON_MBWU.VALUE has not overflowed. 0b1 MSMON_MBWU.VALUE has overflowed at least once since this bit was last

written to zero.

Overflow status for MSMON_MBWU_L.VALUE is reported in MSMON_CFG_MBWU_CTL.OFLOW_STATUS_L.

If MPAMF_MBWUMON_IDR.HAS_CEVNT_OFLW is 1 or MPAMF_MBWUMON_IDR.HAS_OFLOW_LNKG is 1, then a store to MSMON_MBWU when this field is 1 resets this field to 0.

OFLOW_INTR, bit [25]

Enable interrupt on overflow of MSMON_MBWU.VALUE.

OFLOW_INTR Meaning

0b0 No interrupt is signaled on an overflow of MSMON_MBWU.VALUE. 0b1 An implementation-specific interrupt is signaled on an overflow of

MSMON_MBWU.VALUE.

Interrupt enable for overflow of MSMON_MBWU_L.VALUE is controlled by MSMON_CFG_MBWU_CTL.OFLOW_INTR_L.

When MSMON_CFG_MBWU_CTL.OFLOW_INTR == ‘0’, access to this field is RAZ/WI.

OFLOW_FRZ, bit [24]

Freeze monitor instance on overflow. Controls whether MSMON_MBWU.VALUE field of the monitor instance freezes on an overflow.

OFLOW_FRZ Meaning

0b0 MSMON_MBWU.VALUE field of the monitor instance wraps on overflow. 0b1 MSMON_MBWU.VALUE field of the monitor instance freezes on overflow. If the

increment that caused the overflow was 1, the frozen value is the post-increment value of

0. If the increment that caused the overflow was larger than 1, the frozen value of the monitor might be 0 or a larger value less than the final increment.

When a MSMON_MBWU.VALUE of a monitor instance is frozen it does not change until MSMON_MBWU register for that instance has been written. If the monitor implements both MSMON_MBWU and MSMON_MBWU_L registers, both are frozen. A write to a frozen register unfreezes the count for just that register.

This control bit affects both MSMON_MBWU and MSMON_MBWU_L in implementations that include MSMON_MBWU_L.

OFLOW_CAPT, bit [23] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_MBWUMON_IDR.HAS_OFLOW_CAPT == '1':

Capture Monitor on Overflow.

OFLOW_CAPT Meaning

- 0b0 Monitor register MSMON_MBWU is not captured on an overflow or when affected by an overflow linkage event.

- 0b1 Monitor register MSMON_MBWU is captured and the MSMON_MBWU.{NRDY, VALUE} fields are copied to the monitor instance’s MSMON_MBWU_CAPTURE register on an overflow or when affected by an overflow linkage event. The monitor instance treats an overflow of this monitor instance as a private capture event. If MSMON_CFG_MBWU_CTL.CEVNT_OFLW is 1, this monitor instance also treats an overflow linkage event as a capture event. If OFLOW_FRZ is 1, the monitor does not continue to count after the overflow or overflow linkage event. If CAPT_RESET is 1, the monitor instance resets to 0.


This bit does not control whether MSMON_MBWU_L is captured on an overflow or overflow linkage event. See MSMON_CFG_MBWU_CTL.OFLOW_CAPT_L.

Otherwise:

Reserved, RES0.

SUBTYPE, bits [22:20]

Subtype. Type of bandwidth counted by this monitor. This field is not currently used for MBWU monitors, but reserved for future use. This field is RAZ/WI.

SCLEN, bit [19]

MSMON_MBWU.VALUE Scaling Enable. Enables scaling of MSMON_MBWU.VALUE by MPAMF_MBWUMON_IDR.SCALE.

SCLEN Meaning

0b0 MSMON_MBWU.VALUE has bytes counted by the monitor instance. 0b1 MSMON_MBWU.VALUE has bytes counted by the monitor instance, shifted right by

MPAMF_MBWUMON_IDR.SCALE.

CEVNT_OFLW, bit [18] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_MBWUMON_IDR.HAS_CEVNT_OFLW == '1':

Capture Event performs overflow behavior.

CEVNT_OFLW Meaning

- 0b0 On a capture event matching the CAPT_EVNT field the capture behaviors are performed. The NRDY and VALUE fields are transferred to the monitor instance’s capture register.

- 0b1 On a capture event matching the CAPT_EVNT field the monitor instance treats a capture event as an overflow and the overflow behaviors are performed. The behavior is controlled by the MSMON_CFG_MBWU_CTL.{OFLOW_FRZ, OFLOW_CAPT, OFLOW_CAPT_L, CAPT_RESET} fields. The MSMON_CFG_MBWU_CTL.{OFLOW_STATUS, OFLOW_STATUS_L} fields are set for this monitor instance.


Otherwise:

Reserved, RES0.

MATCH_PMG, bit [17] When MPAMF_MBWUMON_IDR.NO_MATCH_PMG == '0':

Match PMG. Controls whether the monitor instance only counts data transferred with PMG matching MSMON_CFG_MBWU_FLT.PMG.

MATCH_PMG Meaning

0b0 The monitor instance counts data transferred with any PMG value. 0b1 The monitor instance only counts data transferred with the PMG value matching

MSMON_CFG_MBWU_FLT.PMG.

Otherwise:

Reserved, RES0.

MATCH_PARTID, bit [16] When MPAMF_MBWUMON_IDR.NO_MATCH_PARTID == '0':

Match PARTID. Controls whether the monitor instance counts only data transferred with PARTID matching MSMON_CFG_MBWU_FLT.PARTID.

MATCH_PARTID Meaning

- 0b0 The monitor instance counts data transferred with any PARTID value.

- 0b1 The monitor instance only counts data transferred with the PARTID value matching MSMON_CFG_MBWU_FLT.PARTID.


Otherwise:

Reserved, RES0.

OFLOW_STATUS_L, bit [15] When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

Overflow Status of MSMON_MBWU_L.VALUE of the monitor instance.

Indicates whether MSMON_MBWU_L.VALUE has overflowed.

OFLOW_STATUS_L Meaning

0b0 MSMON_MBWU_L.VALUE has not overflowed. 0b1 MSMON_MBWU_L.VALUE has overflowed at least once since this bit was

last written to zero.

If MPAMF_MBWUMON_IDR.HAS_LONG == 0, this bit is RES0. Overflow status of MSMON_MBWU.VALUE is reported in MSMON_CFG_MBWU_CTL.OFLOW_STATUS. If MPAMF_MBWUMON_IDR.HAS_CEVNT_OFLW is 1 or MPAMF_MBWUMON_IDR.HAS_OFLOW_LNKG is 1, then a store to MSMON_MBWU_L when this field is 1 resets this field to 0.

Otherwise:

Reserved, RES0.

OFLOW_INTR_L, bit [14] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_MBWUMON_IDR.HAS_LONG == '1':

Overflow Interrupt for MSMON_MBWU_L. Controls whether an MPAM overflow interrupt is generated when MSMON_MBWU_L.VALUE overflows.

OFLOW_INTR_L Meaning

0b0 No interrupt is signaled on an overflow of MSMON_MBWU_L.VALUE. 0b1 An implementation-specific interrupt is signaled on overflow of

MSMON_MBWU_L.VALUE.

When MSMON_CFG_MBWU_CTL.OFLOW_INTR_L == ‘0’, access to this field is RAZ/WI.

Otherwise:

Reserved, RES0.

OFLOW_CAPT_L, bit [13]

When ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p1)) && (MPAMF_MBWUMON_IDR.HAS_LONG == '1')) && (MPAMF_MBWUMON_IDR.HAS_OFLOW_CAPT == '1'):

Capture Long Monitor on Overflow. Controls whether MSMON_MBWU_L is copied to MSMON_MBWU_L_CAPTURE on an overflow or an overflow linkage event.

OFLOW_CAPT_L Meaning

- 0b0 Monitor register MSMON_MBWU_L is not captured on an overflow or when affected by an overflow linkage event.

- 0b1 Monitor register MSMON_MBWU_L is captured on an overflow or when affected by an overflow linkage event. If OFLOW_FRZ is 1, the monitor does not continue to count after the overflow or overflow linkage event. If CAPT_RESET is 1, the monitor instance resets to 0.


If this bit is 1, this monitor instance treats an overflow of this monitor instance as a private capture event. If this bit is 1, this monitor instance also treats overflow linkage events for which it qualifies as a private capture event.

Otherwise:

Reserved, RES0.

Bits [12:11]

Reserved, RES0.

OFLOW_LNKG, bits [10:8] When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_MBWUMON_IDR.HAS_OFLOW_LNKG == '1':

Overflow linkage event. Controls signaling of a capture event on overflow of this monitor instance.

OFLOW_LNKG Meaning

- 0b000 Overflow of the monitor instance only affects this monitor instance.

- 0b001 Overflow of this monitor instance signals Capture Event 1.


- 0b010 Overflow of this monitor instance signals Capture Event 2.

- 0b011 Overflow of this monitor instance signals Capture Event 3. 0b100 Overflow of this monitor instance signals Capture Event 4. 0b101 Overflow of this monitor instance signals Capture Event 5. 0b110 Overflow of this monitor instance signals Capture Event 6. 0b111 Reserved.


Otherwise:

Reserved, RES0.

TYPE, bits [7:0]

Monitor Type Code. The MBWU monitor is TYPE = 0x42. TYPE is a read-only constant indicating the type of the monitor. Reads as 0x42 Access to this field is RO.

Accessing MSMON_CFG_MBWU_CTL

This register is within the MPAM feature page memory frames. If FEAT_MPAM is implemented, the following statements apply:

- • In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.
- • MSMON_CFG_MBWU_CTL_s must only be accessible from the Secure MPAM feature page. MSMON_CFG_MBWU_CTL_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_MBWU_CTL_s and MSMON_CFG_MBWU_CTL_ns must be separate registers. The Secure instance (MSMON_CFG_MBWU_CTL_s) accesses the memory bandwidth usage monitor controls used for Secure PARTIDs, and the Non-secure instance (MSMON_CFG_MBWU_CTL_ns) accesses the memory bandwidth usage monitor controls used for Non-secure PARTIDs.


If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_CFG_MBWU_CTL_s must only be accessible from the Secure MPAM feature page.
- • MSMON_CFG_MBWU_CTL_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_MBWU_CTL_rt must only be accessible from the Root MPAM feature page.
- • MSMON_CFG_MBWU_CTL_rl must only be accessible from the Realm MPAM feature page.


- • MSMON_CFG_MBWU_CTL_s, MSMON_CFG_MBWU_CTL_ns, MSMON_CFG_MBWU_CTL_rt, and MSMON_CFG_MBWU_CTL_rl must be separate registers:
- • The Secure instance (MSMON_CFG_MBWU_CTL_s) accesses the memory bandwidth usage monitor controls used for Secure PARTIDs.
- • The Non-secure instance (MSMON_CFG_MBWU_CTL_ns) accesses the memory bandwidth usage monitor controls used for Non-secure PARTIDs.
- • The Root instance (MSMON_CFG_MBWU_CTL_rt) accesses the memory bandwidth usage monitor controls used for Root PARTIDs.
- • The Realm instance (MSMON_CFG_MBWU_CTL_rl) accesses the memory bandwidth usage monitor controls used for Realm PARTIDs.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

- • When RIS is implemented, loads and stores to MSMON_CFG_MBWU_CTL access the monitor configuration settings for the bandwidth resource instance selected by MSMON_CFG_MON_SEL.RIS and the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, loads and stores to MSMON_CFG_MBWU_CTL access the monitor configuration settings for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Loads and stores to MSMON_CFG_MBWU_CTL access the monitor configuration settings for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_CFG_MBWU_CTL can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0828 MSMON_CFG_MBWU_CTL_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0828 MSMON_CFG_MBWU_CTL_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0828 MSMON_CFG_MBWU_CTL_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0828 MSMON_CFG_MBWU_CTL_rl

When FEAT_RME is implemented, accesses to this register are RW.

- 9.7.5 MSMON_CFG_MBWU_FLT, MPAM Memory System Monitor Configure Memory Bandwidth Usage Monitor Filter Register


The MSMON_CFG_MBWU_FLT characteristics are:

Purpose

Controls PARTID and PMG to measure or count in the MBWU monitor selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_CFG_MBWU_FLT is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_CFG_MBWU_FLT_s sets filter conditions for the Secure memory bandwidth usage monitor instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_CFG_MBWU_CTL_ns sets filter conditions for the Non-secure memory bandwidth usage monitor instance selected by the Non-secure instance of MSMON_CFG_MON_SEL.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_CFG_MBWU_FLT_rt sets the filter conditions for the Root PARTID selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_CFG_MBWU_FLT_rl sets the filter conditions for the Realm PARTID selected by the Realm


instance of MSMON_CFG_MON_SEL. If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance filter configuration accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_MBWU == ‘1’). Otherwise, direct accesses to MSMON_CFG_MBWU_FLT are RES0.

Attributes

MSMON_CFG_MBWU_FLT is a 32-bit register.

Field descriptions

###### When FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented:

31 30 29 24 23 16 15 0

| | | | |
|---|---|---|---|
|RWBW|RES0|PMG|PARTID|


RW filtering.

RWBW, bits [31:30]

When MPAMF_MBWUMON_IDR.HAS_RWBW == '1':

Read/write bandwidth filter. Configures the selected monitor instance to count all bandwidth, only read bandwidth or only write bandwidth.

RWBW Meaning

0b00 Monitor instance counts read bandwidth and write bandwidth. 0b01 Monitor instance counts write bandwidth only. 0b10 Monitor instance counts read bandwidth only. 0b11 Reserved.

Otherwise:

Reserved, RES0.

Bits [29:24]

Reserved, RES0.

PMG, bits [23:16]

Performance monitoring group to filter memory bandwidth usage monitoring. If MSMON_CFG_MBWU_CTL.MATCH_PMG == 0, this field is not used to match memory bandwidth to a PMG and the contents of this field is ignored. If MSMON_CFG_MBWU_CTL.MATCH_PMG == 1, the monitor selected by MSMON_CFG_MON_SEL measures or counts memory bandwidth labeled with PMG equal to this field.

PARTID, bits [15:0]

Partition ID to filter memory bandwidth usage monitoring. If MSMON_CFG_MBWU_CTL.MATCH_PARTID == 0, this field is not used to match memory bandwidth to a PARTID and the contents of this field is ignored. If MSMON_CFG_MBWU_CTL.MATCH_PARTID == 1, the monitor selected by MSMON_CFG_MON_SEL measures or counts memory bandwidth labeled with PARTID equal to this field.

Accessing MSMON_CFG_MBWU_FLT

This register is within the MPAM feature page memory frames. If FEAT_MPAM is implemented, the following statements apply:

- • In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.
- • MSMON_CFG_MBWU_FLT_s must only be accessible from the Secure MPAM feature page. MSMON_CFG_MBWU_FLT_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_MBWU_FLT_s and MSMON_CFG_MBWU_FLT_ns must be separate registers. The Secure instance (MSMON_CFG_MBWU_FLT_s) accesses the PARTID and PMG matching for a memory bandwidth usage monitor used for Secure PARTIDs, and the Non-secure instance (MSMON_CFG_MBWU_FLT_ns) accesses the PARTID and PMG matching for a memory bandwidth usage monitor used for Non-secure PARTIDs.


If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:


- • MSMON_CFG_MBWU_FLT_s must only be accessible from the Secure MPAM feature page.


- • MSMON_CFG_MBWU_FLT_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_MBWU_FLT_rt must only be accessible from the Root MPAM feature page.
- • MSMON_CFG_MBWU_FLT_rl must only be accessible from the Realm MPAM feature page.


- • MSMON_CFG_MBWU_FLT_s, MSMON_CFG_MBWU_FLT_ns, MSMON_CFG_MBWU_FLT_rt, and MSMON_CFG_MBWU_FLT_rl must be separate registers:
- • The Secure instance (MSMON_CFG_MBWU_FLT_s) accesses the PARTID and PMG matching for a memory bandwidth usage monitor used for Secure PARTIDs.
- • The Non-secure instance (MSMON_CFG_MBWU_FLT_ns) accesses the PARTID and PMG matching for a memory bandwidth usage monitor used for Non-secure PARTIDs.
- • The Root instance (MSMON_CFG_MBWU_FLT_rt) accesses the PARTID and PMG matching for a memory bandwidth usage monitor used for Root PARTIDs.
- • The Realm instance (MSMON_CFG_MBWU_FLT_rl) accesses the PARTID and PMG matching for a memory bandwidth usage monitor used for Realm PARTIDs.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

- • When RIS is implemented, loads and stores to MSMON_CFG_MBWU_FLT access the monitor configuration settings for the bandwidth resource instance selected by MSMON_CFG_MON_SEL.RIS and the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, loads and stores to MSMON_CFG_MBWU_FLT access the monitor configuration settings for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Loads and stores to MSMON_CFG_MBWU_FLT access the monitor configuration settings for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_CFG_MBWU_FLT can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0820 MSMON_CFG_MBWU_FLT_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0820 MSMON_CFG_MBWU_FLT_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0820 MSMON_CFG_MBWU_FLT_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0820 MSMON_CFG_MBWU_FLT_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.6 MSMON_CFG_MON_SEL, MPAM Monitor Instance Selection RegisterThe MSMON_CFG_MON_SEL characteristics are:

Purpose

Selects a monitor instance to access through the MSMON configuration and counter registers. MSMON_CFG_MON_SEL_s selects a Secure monitor instance to access via the Secure MPAM feature page. MSMON_CFG_MON_SEL_ns selects a Non-secure monitor instance to access via the Non-secure MPAM feature page. MSMON_CFG_MON_SEL_rt selects a Root monitor instance to access via the Root MPAM feature page. MSMON_CFG_MON_SEL_rl selects a Realm monitor instance to access via the Realm MPAM feature page.

###### Note

Different performance monitoring features within an MSC could have different numbers of monitor instances. See the NUM_MON field in the corresponding ID register. This means that a monitor out-of-bounds error might be signaled when an MSMON_CFG register is accessed because the value in MSMON_CFG_MON_SEL.MON_SEL is too large for the particular monitoring feature.

To configure a monitor, set MON_SEL in this register to the index of the monitor instance to configure, then write to the MSMON_CFG_x register to set the configuration of the monitor. At a later time, read the monitor register (for example, MSMON_MBWU) to get the value of the monitor.

Configuration

The power domain of MSMON_CFG_MON_SEL is IMPLEMENTATION DEFINED MSMON_CFG_MON_SEL_s is the instance of MSMON_CFG_MON_SEL in the Secure PA space. MSMON_CFG_MON_SEL_ns is the instance of MSMON_CFG_MON_SEL in the Non-secure PA space. If FEAT_RME is implemented, the following statements apply:

- • MSMON_CFG_MON_SEL_rt is the instance of MSMON_CFG_MON_SEL in the Root PA space.
- • MSMON_CFG_MON_SEL_rl is the instance of MSMON_CFG_MON_SEL in the Realm PA space.


The power and reset domain of each MSC component is specific to that component. This register is present only when (IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (((MPAMF_IDR.HAS_MSMON == ‘1’) || ((MPAMF_IDR.HAS_IMPL_IDR == ‘1’) && (MPAMF_IDR.EXT == ‘0’))) || (((MPAMF_IDR.HAS_IMPL_IDR == ‘1’) && (MPAMF_IDR.EXT == ‘1’)) && (MPAMF_IDR.NO_IMPL_MSMON == ‘0’))). Otherwise, direct accesses to MSMON_CFG_MON_SEL are RES0.

Attributes

MSMON_CFG_MON_SEL is a 32-bit register.

Field descriptions

31 28 27 24 23 16 15 0

| | | | |
|---|---|---|---|
|RES0|RIS|RES0|MON_SEL|


Bits [31:28]

Reserved, RES0.

RIS, bits [27:24]

When ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p1)) && (MPAMF_IDR.EXT == '1')) && (MPAMF_IDR.HAS_RIS == '1'):

Resource Instance Selector. RIS selects one resource to configure through MSMON_CFG registers.

Otherwise:

Reserved, RES0.

Bits [23:16]

Reserved, RES0.

MON_SEL, bits [15:0]

Selects the monitor instance to configure or read. Reads and writes to other MSMON registers are indexed by MON_SEL and by the NS bit used to access MSMON_CFG_MON_SEL to access the configuration for a single monitor.

Accessing MSMON_CFG_MON_SEL

This register is within the MPAM feature page memory frames. If FEAT_MPAM is implemented, the following statements apply:

- • In a system that supports Secure and Non-secure memory maps, there must be both Secure and Non-secure MPAM feature pages.
- • MSMON_CFG_MON_SEL_s must only be accessible from the Secure MPAM feature page. MSMON_CFG_MON_SEL_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_MON_SEL_s and MSMON_CFG_MON_SEL_ns must be separate registers. The Secure instance (MSMON_CFG_MON_SEL_s) accesses the monitor instance selector used for Secure PARTIDs, and the Non-secure instance (MSMON_CFG_MON_SEL_ns) accesses the monitor instance selector used for Non-secure PARTIDs.


If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_CFG_MON_SEL_s must only be accessible from the Secure MPAM feature page.
- • MSMON_CFG_MON_SEL_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CFG_MON_SEL_rt must only be accessible from the Root MPAM feature page.
- • MSMON_CFG_MON_SEL_rl must only be accessible from the Realm MPAM feature page.


• MSMON_CFG_MON_SEL_s, MSMON_CFG_MON_SEL_ns, MSMON_CFG_MON_SEL_rt, and MSMON_CFG_MON_SEL_rl must be separate registers:

- • The Secure instance (MSMON_CFG_MON_SEL_s) accesses the monitor instance selector used for Secure PARTIDs.
- • The Non-secure instance (MSMON_CFG_MON_SEL_ns) accesses the monitor instance selector used for Non-secure PARTIDs.
- • The Root instance (MSMON_CFG_MON_SEL_rt) accesses the monitor instance selector used for Root PARTIDs.
- • The Realm instance (MSMON_CFG_MON_SEL_rl) accesses the monitor instance selector used for Realm PARTIDs.


MSMON_CFG_MON_SEL can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0800 MSMON_CFG_MON_SEL_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0800 MSMON_CFG_MON_SEL_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0800 MSMON_CFG_MON_SEL_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0800 MSMON_CFG_MON_SEL_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.7 MSMON_CSU, MPAM Cache Storage Usage Monitor RegisterThe MSMON_CSU characteristics are:

Purpose

Accesses the CSU monitor instance selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_CSU is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_CSU_s is the Secure cache storage usage monitor instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_CSU_ns is the Non-secure cache storage usage monitor instance selected by the Non-secure instance of MSMON_CFG_MON_SEL.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_CSU_rt is the Root cache storage usage monitor instance selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_CSU_rl is the Realm cache storage usage monitor instance selected by the Realm instance of MSMON_CFG_MON_SEL.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance register accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_CSU == ‘1’). Otherwise, direct accesses to MSMON_CSU are RES0.

Attributes

MSMON_CSU is a 32-bit register.

Field descriptions

31 30 0

| | |
|---|---|
| |VALUE|


NRDY

NRDY, bit [31]

Not Ready. Indicates whether the monitor instance has possibly inaccurate data.

NRDY Meaning

0b0 The monitor instance is ready and the MSMON_CSU.VALUE field is accurate. 0b1 The monitor instance is not ready and the contents of the MSMON_CSU.VALUE field might be

inaccurate or otherwise not represent the actual cache storage usage.

VALUE, bits [30:0]

Cache storage usage measurement value if MSMON_CSU.NRDY is 0. Invalid if MSMON_CSU.NRDY is 1. VALUE is the cache storage usage measured in bytes meeting the criteria set in MSMON_CFG_CSU_FLT and MSMON_CFG_CSU_CTL for the monitor instance selected by MSMON_CFG_MON_SEL.

Accessing MSMON_CSU

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_CSU_s must only be accessible from the Secure MPAM feature page.
- • MSMON_CSU_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CSU_rt must only be accessible from the Root MPAM feature page.
- • MSMON_CSU_rl must only be accessible from the Realm MPAM feature page.


• MSMON_CSU_s, MSMON_CSU_ns, MSMON_CSU_rt, and MSMON_CSU_rl must be separate registers:

- • The Secure instance (MSMON_CSU_s) accesses the cache storage usage monitor used for Secure PARTIDs.
- • The Non-secure instance (MSMON_CSU_ns) accesses the cache storage usage monitor used for Non-secure PARTIDs.
- • The Root instance (MSMON_CSU_rt) accesses the cache storage usage monitor used for Root PARTIDs.
- • The Realm instance (MSMON_CSU_rl) accesses the cache storage usage monitor used for Realm PARTIDs.


If any one of these features is implemented:

- • FEAT_MPAMv0p1
- • FEAT_MPAMv1p1


then, the following statements apply:

- • When RIS is implemented, reads and writes to MSMON_CSU access the cache storage usage monitor monitor instance for the cache resource instance selected by MSMON_CFG_MON_SEL.RIS and the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, reads and writes to MSMON_CSU access the cache storage usage monitor monitor instance for the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Reads and writes to MSMON_CSU access the cache storage usage monitor monitor for the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_CSU can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0840 MSMON_CSU_s

Accessible as follows:

• When MPAMF_CSUMON_IDR.CSU_RO == ‘0’, accesses to this register are RW. • When MPAMF_CSUMON_IDR.CSU_RO == ‘1’, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0840 MSMON_CSU_ns

Accessible as follows:

• When MPAMF_CSUMON_IDR.CSU_RO == ‘0’, accesses to this register are RW. • When MPAMF_CSUMON_IDR.CSU_RO == ‘1’, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0840 MSMON_CSU_rt

Accessible as follows:

- • When FEAT_RME is implemented and MPAMF_CSUMON_IDR.CSU_RO == ‘0’, accesses to this register are RW.
- • When FEAT_RME is implemented and MPAMF_CSUMON_IDR.CSU_RO == ‘1’, accesses to this register are RO.


Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0840 MSMON_CSU_rl

Accessible as follows:

- • When FEAT_RME is implemented and MPAMF_CSUMON_IDR.CSU_RO == ‘0’, accesses to this register are RW.
- • When FEAT_RME is implemented and MPAMF_CSUMON_IDR.CSU_RO == ‘1’, accesses to this register are RO.


###### 9.7.8 MSMON_CSU_CAPTURE, MPAM Cache Storage Usage Monitor Capture RegisterThe MSMON_CSU_CAPTURE characteristics are:

Purpose

MSMON_CSU_CAPTURE is a 32-bit read/write register that accesses the captured MSMON_CSU monitor instance selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_CSU_CAPTURE is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_CSU_CAPTURE_s is the Secure cache storage usage monitor capture instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_CSU_CAPTURE_ns is the Non-secure cache storage usage monitor capture instance selected by the Non-secure instance of MSMON_CFG_MON_SEL.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_CSU_CAPTURE_rt is the Root cache storage usage monitor capture instance selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_CSU_CAPTURE_rl is the Realm cache storage usage monitor capture instance selected by


the Realm instance of MSMON_CFG_MON_SEL. If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance capture register accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when (((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_CSU == ‘1’)) && (MPAMF_CSUMON_IDR.HAS_CAPTURE == ‘1’). Otherwise, direct accesses to MSMON_CSU_CAPTURE are RES0.

Attributes

MSMON_CSU_CAPTURE is a 32-bit register.

Field descriptions

31 30 0

| | |
|---|---|
| |VALUE|


NRDY

NRDY, bit [31]

Not Ready. Indicates whether the captured monitor value has possibly inaccurate data.

NRDY Meaning

0b0 The captured monitor instance was ready and the MSMON_CSU_CAPTURE.VALUE field is accurate. 0b1 The captured monitor instance was not ready and the contents of the

MSMON_CSU_CAPTURE.VALUE field might be inaccurate or otherwise not represent the actual cache storage usage.

VALUE, bits [30:0]

Captured cache storage usage measurement if MSMON_CSU_CAPTURE.NRDY is 0. Invalid if MSMON_CSU_CAPTURE.NRDY is 1.

VALUE is the captured cache storage usage measurement in bytes meeting the criteria set in MSMON_CFG_CSU_FLT and MSMON_CFG_CSU_CTL for the monitor instance selected by MSMON_CFG_MON_SEL.

Accessing MSMON_CSU_CAPTURE

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_CSU_CAPTURE_s must only be accessible from the Secure MPAM feature page.
- • MSMON_CSU_CAPTURE_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CSU_CAPTURE_rt must only be accessible from the Root MPAM feature page.
- • MSMON_CSU_CAPTURE_rl must only be accessible from the Realm MPAM feature page.


• MSMON_CSU_CAPTURE_s, MSMON_CSU_CAPTURE_ns, MSMON_CSU_CAPTURE_rt, and MSMON_CSU_CAPTURE_rl must be separate registers:

- • The Secure instance (MSMON_CSU_CAPTURE_s) accesses the captured cache storage usage monitor used for Secure PARTIDs.
- • The Non-secure instance (MSMON_CSU_CAPTURE_ns) accesses the captured cache storage usage monitor used for Non-secure PARTIDs.
- • The Root instance (MSMON_CSU_CAPTURE_rt) accesses the captured cache storage usage monitor used for Root PARTIDs.
- • The Realm instance (MSMON_CSU_CAPTURE_rl) accesses the captured cache storage usage monitor used for Realm PARTIDs.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

- • When RIS is implemented, reads and writes to MSMON_CSU_CAPTURE access the monitor instance for the cache resource instance selected by MSMON_CFG_MON_SEL.RIS and the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, reads and writes to MSMON_CSU_CAPTURE access the monitor instance for the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Reads and writes to MSMON_CSU_CAPTURE access the monitor for the cache storage usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_CSU_CAPTURE can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0848 MSMON_CSU_CAPTURE_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0848 MSMON_CSU_CAPTURE_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0848 MSMON_CSU_CAPTURE_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0848 MSMON_CSU_CAPTURE_rl

When FEAT_RME is implemented, accesses to this register are RW.

- 9.7.9 MSMON_CSU_OFSR, MPAM CSU Monitor Overflow Status Register The MSMON_CSU_OFSR characteristics are:


Purpose

MSMON_CSU_OFSR is a 32-bit read-only register that shows bitmap of CSU monitor instance overflow status for a contiguous group of 32 monitor instances.

Configuration

The power domain of MSMON_CSU_OFSR is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_CSU_OFSR_s gives a bitmap of pending CSU overflow status for 32 Secure CSU monitor instances.
- • MSMON_CSU_OFSR_ns gives a bitmap of pending CSU overflow status for 32 Non-secure CSU monitor instances.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_CSU_OFSR_rt gives a bitmap of pending CSU overflow status for 32 Root CSU monitor instances.
- • MSMON_CSU_OFSR_rl gives a bitmap of pending CSU overflow status for 32 Realm CSU monitor


instances. The power and reset domain of each MSC component is specific to that component. This register is present only when (((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_CSU == ‘1’)) && (MPAMF_CSUMON_IDR.HAS_OFSR == ‘1’). Otherwise, direct accesses to MSMON_CSU_OFSR are RES0.

Attributes

MSMON_CSU_OFSR is a 32-bit register.

Field descriptions

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 0

| | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | |


OFPND31 OFPND30 OFPND29 OFPND28 OFPND27 OFPND26 OFPND25 OFPND24 OFPND23 OFPND22 OFPND21 OFPND20 OFPND19 OFPND18 OFPND17 OFPND16

OFPND0 OFPND1

OFPND2 OFPND3

OFPND4 OFPND5

OFPND6 OFPND7

OFPND8 OFPND9

OFPND10 OFPND11

OFPND12 OFPND13

OFPND14 OFPND15

OFPND<i> , bits [i], for i = 31 to 0

Overflow status bitmap for CSU monitor instances. The RIS and the contiguous range of CSU monitor instances are set in MSMON_CFG_MON_SEL. i of 0 corresponds to the CSU monitor instance MSMON_CFG_MON_SEL.MON_SEL & 0xFFE0.

OFPND<i> Meaning

0b0 CSU monitor instance (MSMON_CFG_MON_SEL.MON_SEL & 0xFFE0 + i) does not have a

pending overflow.

0b1 CSU monitor instance (MSMON_CFG_MON_SEL.MON_SEL & 0xFFE0 + i) has a pending

overflow.

After reading MSMON_OFLOW_SR to determine that a CSU monitor instance has a pending overflow and which RIS values have pending overflows, an interrupt service routine could poll groups of 32 monitor instances in a RIS for pending monitors by reading this bitmap and incrementing MSMON_CFG_MON_SEL.MON_SEL by 32.

Accessing MSMON_CSU_OFSR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_CSU_OFSR_s must only be accessible from the Secure MPAM feature page.
- • MSMON_CSU_OFSR_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_CSU_OFSR_rt must only be accessible from the Root MPAM feature page.
- • MSMON_CSU_OFSR_rl must only be accessible from the Realm MPAM feature page.


- • MSMON_CSU_OFSR_s, MSMON_CSU_OFSR_ns, MSMON_CSU_OFSR_rt, and MSMON_CSU_OFSR_rl must be separate registers:
- • The Secure instance (MSMON_CSU_OFSR_s) accesses the CSU monitor overflow status bitmap used for Secure PARTIDs.
- • The Non-secure instance (MSMON_CSU_OFSR_ns) accesses the CSU monitor overflow status bitmap used for Non-secure PARTIDs.
- • The Root instance (MSMON_CSU_OFSR_rt) accesses the CSU monitor overflow status bitmap used for Root PARTIDs.
- • The Realm instance (MSMON_CSU_OFSR_rl) accesses the CSU monitor overflow status bitmap used for Realm PARTIDs.


MSMON_CSU_OFSR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0858 MSMON_CSU_OFSR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0858 MSMON_CSU_OFSR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0858 MSMON_CSU_OFSR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0858 MSMON_CSU_OFSR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.7.10 MSMON_MBWU, MPAM Memory Bandwidth Usage Monitor RegisterThe MSMON_MBWU characteristics are:

Purpose

Accesses the monitor instance selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_MBWU is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_MBWU_s is the Secure memory bandwidth usage monitor instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_MBWU_ns is the Non-secure memory bandwidth usage monitor instance selected by the Non-secure instance of MSMON_CFG_MON_SEL.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_MBWU_rt is the Root memory bandwidth usage monitor instance selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_MBWU_rl is the Realm memory bandwidth usage monitor instance selected by the Realm


instance of MSMON_CFG_MON_SEL. If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance register accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_MBWU == ‘1’). Otherwise, direct accesses to MSMON_MBWU are RES0.

Attributes

MSMON_MBWU is a 32-bit register.

Field descriptions

31 30 0

| | |
|---|---|
| |VALUE|


NRDY

NRDY, bit [31]

Not Ready. Indicates whether the monitor has possibly inaccurate data.

NRDY Meaning

0b0 The monitor instance is ready and the MSMON_MBWU.VALUE field is accurate. 0b1 The monitor instance is not ready and the contents of the MSMON_MBWU.VALUE field might be

inaccurate or otherwise not represent the actual memory bandwidth usage.

VALUE, bits [30:0]

Memory bandwidth usage counter value if MSMON_MBWU.NRDY is 0. Invalid if MSMON_MBWU.NRDY is 1.

VALUE is the scaled count of bytes transferred since the monitor was last reset that met the criteria set in MSMON_CFG_MBWU_FLT and MSMON_CFG_MBWU_CTL for the monitor instance selected by MSMON_CFG_MON_SEL.

If MSMON_CFG_MBWU_CTL.SCLEN enables scaling, the count in VALUE is the number of bytes shifted right by MPAMF_MBWUMON_IDR.SCALE bit positions and rounded.

Accessing MSMON_MBWU

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_MBWU_s must only be accessible from the Secure MPAM feature page.
- • MSMON_MBWU_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_MBWU_rt must only be accessible from the Root MPAM feature page.
- • MSMON_MBWU_rl must only be accessible from the Realm MPAM feature page.


- • MSMON_MBWU_s, MSMON_MBWU_ns, MSMON_MBWU_rt, and MSMON_MBWU_rl must be separate registers:
- • The Secure instance (MSMON_MBWU_s) accesses the memory bandwidth usage monitor used for Secure PARTIDs.
- • The Non-secure instance (MSMON_MBWU_ns) accesses the memory bandwidth usage monitor used for Non-secure PARTIDs.
- • The Root instance (MSMON_MBWU_rt) accesses the memory bandwidth usage monitor used for Root PARTIDs.
- • The Realm instance (MSMON_MBWU_rl) accesses the memory bandwidth usage monitor used for Realm PARTIDs.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

- • When RIS is implemented, reads and writes to MSMON_MBWU access the memory bandwidth usage monitor instance for the resource instance selected by MSMON_CFG_MON_SEL.RIS and the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, reads and writes to MSMON_MBWU access the memory bandwidth usage monitor instance for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Reads and writes to MSMON_MBWU access the memory bandwidth usage monitor for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_MBWU can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0860 MSMON_MBWU_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0860 MSMON_MBWU_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0860 MSMON_MBWU_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0860 MSMON_MBWU_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.11 MSMON_MBWU_CAPTURE, MPAM Memory Bandwidth Usage Monitor Capture RegisterThe MSMON_MBWU_CAPTURE characteristics are:

Purpose

Accesses the captured MSMON_MBWU monitor instance selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_MBWU_CAPTURE is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_MBWU_CAPTURE_s is the Secure memory bandwidth usage monitor capture instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_MBWU_CAPTURE_ns is the Non-secure memory bandwidth usage monitor capture instance


selected by the Non-secure instance of MSMON_CFG_MON_SEL. • If FEAT_RME is implemented, the following statements also apply:

- • MSMON_MBWU_CAPTURE_rt is the Root memory bandwidth usage monitor capture instance selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_MBWU_CAPTURE_rl is the Realm memory bandwidth usage monitor capture instance


selected by the Realm instance of MSMON_CFG_MON_SEL. If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance capture register accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when (((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_MBWU == ‘1’)) && (MPAMF_MBWUMON_IDR.HAS_CAPTURE == ‘1’). Otherwise, direct accesses to MSMON_MBWU_CAPTURE are RES0.

Attributes

MSMON_MBWU_CAPTURE is a 32-bit register.

Field descriptions

31 30 0

| | |
|---|---|
| |VALUE|


NRDY

NRDY, bit [31]

Not Ready. The captured NRDY bit from the corresponding instance of MSMON_MBWU. This bit indicates whether the captured monitor value has possibly inaccurate data.

NRDY Meaning

- 0b0 The captured monitor instance was ready and the MSMON_MBWU_CAPTURE.VALUE field is accurate.

- 0b1 The captured monitor instance was not ready and the contents of the MSMON_MBWU_CAPTURE.VALUE field might be inaccurate or otherwise not represent the actual memory bandwidth usage.


VALUE, bits [30:0]

Captured memory bandwidth usage counter value if MSMON_MBWU_CAPTURE.NRDY is 0. Invalid if MSMON_MBWU_CAPTURE.NRDY is 1.

VALUE is the captured VALUE field from the corresponding instance of MSMON_MBWU, the count of bytes transferred since the monitor was last reset that meet the criteria set in MSMON_CFG_MBWU_FLT and MSMON_CFG_MBWU_CTL for the monitor instance selected by MSMON_CFG_MON_SEL.

VALUE captures the MSMON_MBWU.VALUE and preserves any scaling that had been performed on the VALUE field in that register.

Accessing MSMON_MBWU_CAPTURE

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_MBWU_CAPTURE_s must only be accessible from the Secure MPAM feature page.
- • MSMON_MBWU_CAPTURE_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_MBWU_CAPTURE_rt must only be accessible from the Root MPAM feature page.
- • MSMON_MBWU_CAPTURE_rl must only be accessible from the Realm MPAM feature page.


- • MSMON_MBWU_CAPTURE_s, MSMON_MBWU_CAPTURE_ns, MSMON_MBWU_CAPTURE_rt, and MSMON_MBWU_CAPTURE_rl must be separate registers:
- • The Secure instance (MSMON_MBWU_CAPTURE_s) accesses the captured memory bandwidth usage monitor used for Secure PARTIDs.
- • The Non-secure instance (MSMON_MBWU_CAPTURE_ns) accesses the captured memory bandwidth usage monitor used for Non-secure PARTIDs.
- • The Root instance (MSMON_MBWU_CAPTURE_rt) accesses the captured memory bandwidth usage monitor used for Root PARTIDs.
- • The Realm instance (MSMON_MBWU_CAPTURE_rl) accesses the captured memory bandwidth usage monitor used for Realm PARTIDs.


If any one of these features is implemented:

- • FEAT_MPAMv0p1
- • FEAT_MPAMv1p1


then, the following statements apply:

- • When RIS is implemented, reads and writes to MSMON_MBWU_CAPTURE access the monitor instance for the bandwidth resource instance selected by MSMON_CFG_MON_SEL.RIS and the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, reads and writes to MSMON_MBWU_CAPTURE access the monitor instance for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Reads and writes to MSMON_MBWU_CAPTURE access the monitor for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_MBWU_CAPTURE can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0868 MSMON_MBWU_CAPTURE_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0868 MSMON_MBWU_CAPTURE_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0868 MSMON_MBWU_CAPTURE_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0868 MSMON_MBWU_CAPTURE_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.12 MSMON_MBWU_L, MPAM Long Memory Bandwidth Usage Monitor RegisterThe MSMON_MBWU_L characteristics are:

Purpose

Accesses the monitor instance selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_MBWU_L is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_MBWU_L_s is the Secure long memory bandwidth usage monitor instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_MBWU_L_ns is the Non-secure long memory bandwidth usage monitor instance selected by the Non-secure instance of MSMON_CFG_MON_SEL.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_MBWU_L_rt is the Root long memory bandwidth usage monitor instance selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_MBWU_L_rl is the Realm long memory bandwidth usage monitor instance selected by the


Realm instance of MSMON_CFG_MON_SEL. If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance long monitor register accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when (((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_MBWU == ‘1’)) && (MPAMF_MBWUMON_IDR.HAS_LONG == ‘1’). Otherwise, direct accesses to MSMON_MBWU_L are RES0.

Attributes

MSMON_MBWU_L is a 64-bit register.

Field descriptions

###### When MPAMF_MBWUMON_IDR.LWD == '0':

63 62 44 43 32

| | | |
|---|---|---|
| |RES0|VALUE|


NRDY

31 0

VALUE

NRDY, bit [63]

Not Ready. Indicates whether the monitor instance has possibly inaccurate data.

NRDY Meaning

0b0 The monitor instance is ready and the MSMON_MBWU_L.VALUE field is accurate. 0b1 The monitor instance is not ready and the contents of the MSMON_MBWU_L.VALUE field might be inaccurate or otherwise not represent the actual memory bandwidth usage.

Bits [62:44]

Reserved, RES0.

VALUE, bits [43:0]

Long (44-bit) memory bandwidth usage counter value if MSMON_MBWU_L.NRDY is 0. Invalid if MSMON_MBWU_L.NRDY is 1.

VALUE is the long count of bytes transferred since the monitor was last reset that met the criteria set in MSMON_CFG_MBWU_FLT and MSMON_CFG_MBWU_CTL for the monitor instance selected by MSMON_CFG_MON_SEL.

When MPAMF_MBWUMON_IDR.LWD == '1':

63 62 32

| | |
|---|---|
| |VALUE|


NRDY

31 0

VALUE

NRDY, bit [63]

Not Ready. Indicates whether the monitor instance has possibly inaccurate data.

NRDY Meaning

0b0 The monitor instance is ready and the MSMON_MBWU_L.VALUE field is accurate. 0b1 The monitor instance is not ready and the contents of the MSMON_MBWU_L.VALUE field might be inaccurate or otherwise not represent the actual memory bandwidth usage.

VALUE, bits [62:0]

Long (63-bit) memory bandwidth usage counter value if MSMON_MBWU_L.NRDY is 0. Invalid if MSMON_MBWU_L.NRDY is 1.

VALUE is the long count of bytes transferred since the monitor instance was last reset that met the criteria set in MSMON_CFG_MBWU_FLT and MSMON_CFG_MBWU_CTL for the monitor instance selected by MSMON_CFG_MON_SEL.

Accessing MSMON_MBWU_L

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:


- • MSMON_MBWU_L_s must only be accessible from the Secure MPAM feature page.
- • MSMON_MBWU_L_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_MBWU_L_rt must only be accessible from the Root MPAM feature page.
- • MSMON_MBWU_L_rl must only be accessible from the Realm MPAM feature page.

- • MSMON_MBWU_L_s, MSMON_MBWU_L_ns, MSMON_MBWU_L_rt, and MSMON_MBWU_L_rl must be separate registers:


- • The Secure instance (MSMON_MBWU_L_s) accesses the long memory bandwidth usage monitor used for Secure PARTIDs.
- • The Non-secure instance (MSMON_MBWU_L_ns) accesses the long memory bandwidth usage monitor used for Non-secure PARTIDs.
- • The Root instance (MSMON_MBWU_L_rt) accesses the long memory bandwidth usage monitor used for Root PARTIDs.
- • The Realm instance (MSMON_MBWU_L_rl) accesses the long memory bandwidth usage monitor used for Realm PARTIDs.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

- • When RIS is implemented, reads and writes to MSMON_MBWU_L access the long memory bandwidth usage monitor instance for the bandwidth resource instance selected by MSMON_CFG_MON_SEL.RIS and the monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, reads and writes to MSMON_MBWU_L access the long memory bandwidth usage monitor instance for the monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Reads and writes to MSMON_MBWU_L access the long memory bandwidth usage monitor for the monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_MBWU_L can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0880 MSMON_MBWU_L_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0880 MSMON_MBWU_L_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0880 MSMON_MBWU_L_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0880 MSMON_MBWU_L_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.13 MSMON_MBWU_L_CAPTURE, MPAM Long Memory Bandwidth Usage Monitor Capture RegisterThe MSMON_MBWU_L_CAPTURE characteristics are:

Purpose

Accesses the captured MSMON_MBWU_L monitor instance selected by MSMON_CFG_MON_SEL.

Configuration

The power domain of MSMON_MBWU_L_CAPTURE is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_MBWU_L_CAPTURE_s is the Secure long memory bandwidth usage monitor capture instance selected by the Secure instance of MSMON_CFG_MON_SEL.
- • MSMON_MBWU_L_CAPTURE_ns is the Non-secure long memory bandwidth usage monitor capture instance selected by the Non-secure instance of MSMON_CFG_MON_SEL.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_MBWU_L_CAPTURE_rt is the Root long memory bandwidth usage monitor capture instance selected by the Root instance of MSMON_CFG_MON_SEL.
- • MSMON_MBWU_L_CAPTURE_rl is the Realm long memory bandwidth usage monitor capture


instance selected by the Realm instance of MSMON_CFG_MON_SEL. If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

• If MPAMF_IDR.HAS_RIS is 1, the monitor instance long capture register accessed is for the resource instance currently selected by MSMON_CFG_MON_SEL.RIS and the monitor instance of that resource instance selected by MSMON_CFG_MON_SEL.MON_SEL.

The power and reset domain of each MSC component is specific to that component. This register is present only when ((((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_MBWU == ‘1’)) && (MPAMF_MBWUMON_IDR.HAS_CAPTURE == ‘1’)) && (MPAMF_MBWUMON_IDR.HAS_LONG == ‘1’). Otherwise, direct accesses to MSMON_MBWU_L_CAPTURE are RES0.

Attributes

MSMON_MBWU_L_CAPTURE is a 64-bit register.

Field descriptions

###### When MPAMF_MBWUMON_IDR.LWD == '0':

63 62 44 43 32

| | | |
|---|---|---|
| |RES0|VALUE|


NRDY

31 0

VALUE

NRDY, bit [63]

Not Ready. Indicates whether the monitor has possibly inaccurate data.

NRDY Meaning

- 0b0 The captured monitor instance was ready and the MSMON_MBWU_L_CAPTURE.VALUE field is accurate.

- 0b1 The captured monitor instance was not ready and the contents of the MSMON_MBWU_L_CAPTURE.VALUE field might be inaccurate or otherwise not represent the actual memory bandwidth usage.


Bits [62:44]

Reserved, RES0.

VALUE, bits [43:0]

Captured long memory bandwidth usage counter value if MSMON_MBWU_L_CAPTURE.NRDY is 0. Invalid if MSMON_MBWU_L_CAPTURE.NRDY is 1.

VALUE is the captured 44-bit count of bytes transferred since the monitor instance was last reset that met the criteria set in MSMON_CFG_MBWU_FLT and MSMON_CFG_MBWU_CTL for the monitor instance selected by MSMON_CFG_MON_SEL.

When MPAMF_MBWUMON_IDR.LWD == '1':

63 62 32

| | |
|---|---|
| |VALUE|


NRDY

31 0

VALUE

NRDY, bit [63]

Not Ready. Indicates whether the monitor has possibly inaccurate data.

NRDY Meaning

- 0b0 The captured monitor instance was ready and the MSMON_MBWU_L_CAPTURE.VALUE field is accurate.

- 0b1 The captured monitor instance was not ready and the contents of the MSMON_MBWU_L_CAPTURE.VALUE field might be inaccurate or otherwise not represent the actual memory bandwidth usage.


VALUE, bits [62:0]

The captured long memory bandwidth usage counter value if MSMON_MBWU_L_CAPTURE.NRDY is 0. Invalid if MSMON_MBWU_L_CAPTURE.NRDY is 1.

VALUE is the captured 63-bit count of bytes transferred since the monitor instance was last reset that met the criteria set in MSMON_CFG_MBWU_FLT and MSMON_CFG_MBWU_CTL for the monitor instance selected by MSMON_CFG_MON_SEL.

Accessing MSMON_MBWU_L_CAPTURE

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_MBWU_L_CAPTURE_s must only be accessible from the Secure MPAM feature page.
- • MSMON_MBWU_L_CAPTURE_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_MBWU_L_CAPTURE_rt must only be accessible from the Root MPAM feature page.
- • MSMON_MBWU_L_CAPTURE_rl must only be accessible from the Realm MPAM feature page.


• MSMON_MBWU_L_CAPTURE_s, MSMON_MBWU_L_CAPTURE_ns, MSMON_MBWU_L_CAPTURE_rt, and MSMON_MBWU_L_CAPTURE_rl must be separate registers:

- • The Secure instance (MSMON_MBWU_L_CAPTURE_s) accesses the captured long memory bandwidth usage monitor used for Secure PARTIDs.
- • The Non-secure instance (MSMON_MBWU_L_CAPTURE_ns) accesses the captured long memory bandwidth usage monitor used for Non-secure PARTIDs.
- • The Root instance (MSMON_MBWU_L_CAPTURE_rt) accesses the captured long memory bandwidth usage monitor used for Root PARTIDs.
- • The Realm instance (MSMON_MBWU_L_CAPTURE_rl) accesses the captured long memory bandwidth usage monitor used for Realm PARTIDs.


If any one of these features is implemented:

• FEAT_MPAMv0p1 • FEAT_MPAMv1p1

then, the following statements apply:

- • When RIS is implemented, reads and writes to MSMON_MBWU_L_CAPTURE access the monitor instance for the bandwidth resource instance selected by MSMON_CFG_MON_SEL.RIS and the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.
- • When RIS is not implemented, reads and writes to MSMON_MBWU_L_CAPTURE access the monitor instance for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.


Otherwise, the following statements apply:

• Reads and writes to MSMON_MBWU_L_CAPTURE access the monitor for the memory bandwidth usage monitor instance selected by MSMON_CFG_MON_SEL.MON_SEL.

MSMON_MBWU_L_CAPTURE can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0890 MSMON_MBWU_L_CAPTURE_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0890 MSMON_MBWU_L_CAPTURE_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0890 MSMON_MBWU_L_CAPTURE_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0890 MSMON_MBWU_L_CAPTURE_rl

When FEAT_RME is implemented, accesses to this register are RW.

- 9.7.14 MSMON_MBWU_OFSR, MPAM MBWU Monitor Overflow Status Register The MSMON_MBWU_OFSR characteristics are:


Purpose

MSMON_MBWU_OFSR is a 32-bit read-only register that shows bitmap of MBWU monitor instance overflow status for a contiguous group of 32 monitor instances.

Configuration

The power domain of MSMON_MBWU_OFSR is IMPLEMENTATION DEFINED If FEAT_MPAM is implemented, the following statements apply:

- • MSMON_MBWU_OFSR_s gives a bitmap of pending MBWU overflow status for 32 Secure MBWU monitor instances.
- • MSMON_MBWU_OFSR_ns gives a bitmap of pending MBWU overflow status for 32 Non-secure MBWU monitor instances.
- • If FEAT_RME is implemented, the following statements also apply:
- • MSMON_MBWU_OFSR_rt gives a bitmap of pending MBWU overflow status for 32 Root MBWU monitor instances.
- • MSMON_MBWU_OFSR_rl gives a bitmap of pending MBWU overflow status for 32 Realm MBWU


monitor instances. The power and reset domain of each MSC component is specific to that component. This register is present only when (((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.MSMON_MBWU == ‘1’)) && (MPAMF_MBWUMON_IDR.HAS_OFSR == ‘1’). Otherwise, direct accesses to MSMON_MBWU_OFSR are RES0.

Attributes

MSMON_MBWU_OFSR is a 32-bit register.

Field descriptions

31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 0

| | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | |


OFPND31 OFPND30 OFPND29 OFPND28 OFPND27 OFPND26 OFPND25 OFPND24 OFPND23 OFPND22 OFPND21 OFPND20 OFPND19 OFPND18 OFPND17 OFPND16

OFPND0 OFPND1

OFPND2 OFPND3

OFPND4 OFPND5

OFPND6 OFPND7

OFPND8 OFPND9

OFPND10 OFPND11

OFPND12 OFPND13

OFPND14 OFPND15

OFPND<i> , bits [i], for i = 31 to 0

Overflow status bitmap for MBWU monitor instances. The RIS and the contiguous range of MBWU monitor instances are set in MSMON_CFG_MON_SEL. i of 0 corresponds to the MBWU monitor instance MSMON_CFG_MON_SEL.MON_SEL & 0xFFE0.

OFPND<i> Meaning

0b0 MBWU monitor instance (MSMON_CFG_MON_SEL.MON_SEL & 0xFFE0 + i) does not have a

pending overflow.

0b1 MBWU monitor instance (MSMON_CFG_MON_SEL.MON_SEL & 0xFFE0 + i) has a pending

overflow.

After reading MSMON_OFLOW_SR to determine that an MBWU monitor instance has a pending overflow and which RIS values have pending overflows, an interrupt service routine could poll groups of 32 monitor instances in a RIS for pending monitors by reading this bitmap and incrementing MSMON_CFG_MON_SEL.MON_SEL by 32.

A pending overflow may be in either the MSMON_CFG_MBWU_CTL.OFLOW_STATUS or MSMON_CFG_MBWU_CTL.OFLOW_STATUS_L field.

Accessing MSMON_MBWU_OFSR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_MBWU_OFSR_s must only be accessible from the Secure MPAM feature page.
- • MSMON_MBWU_OFSR_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_MBWU_OFSR_rt must only be accessible from the Root MPAM feature page.
- • MSMON_MBWU_OFSR_rl must only be accessible from the Realm MPAM feature page.


• MSMON_MBWU_OFSR_s, MSMON_MBWU_OFSR_ns, MSMON_MBWU_OFSR_rt, and MSMON_MBWU_OFSR_rl must be separate registers:

- • The Secure instance (MSMON_MBWU_OFSR_s) accesses the MBWU monitor overflow status bitmap used for Secure PARTIDs.
- • The Non-secure instance (MSMON_MBWU_OFSR_ns) accesses the MBWU monitor overflow status bitmap used for Non-secure PARTIDs.
- • The Root instance (MSMON_MBWU_OFSR_rt) accesses the MBWU monitor overflow status bitmap used for Root PARTIDs.
- • The Realm instance (MSMON_MBWU_OFSR_rl) accesses the MBWU monitor overflow status bitmap used for Realm PARTIDs.


MSMON_MBWU_OFSR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x0898 MSMON_MBWU_OFSR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x0898 MSMON_MBWU_OFSR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x0898 MSMON_MBWU_OFSR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x0898 MSMON_MBWU_OFSR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.7.15 MSMON_OFLOW_MSI_ADDR_H, MPAM Monitor Overflow MSI Write High-part Address RegisterThe MSMON_OFLOW_MSI_ADDR_H characteristics are:

Purpose

MSMON_OFLOW_MSI_ADDR_H is a 32-bit read/write register for the high part of the MPAM monitor overflow MSI address.

MSMON_OFLOW_MSI_ADDR_H_s is the high part of the MSI write address for monitor overflow interrupts from Secure monitor instances. MSMON_OFLOW_MSI_ADDR_H_ns is the high part of the MSI write address for monitor overflow interrupts from Non-secure monitor instances. MSMON_OFLOW_MSI_ADDR_H_rt is the high part of the MSI write address for monitor overflow interrupts from Root monitor instances. MSMON_OFLOW_MSI_ADDR_H_rl is the high part of the MSI write address for monitor overflow interrupts from Realm monitor instances.

Configuration

The power domain of MSMON_OFLOW_MSI_ADDR_H is IMPLEMENTATION DEFINED MSMON_OFLOW_MSI_ADDR_L, MSMON_OFLOW_MSI_ADDR_H, MSMON_OFLOW_MSI_ATTR, MSMON_OFLOW_MSI_DATA, and MSMON_OFLOW_MSI_MPAM must all be implemented to support MSI writes for monitor overflow interrupts. The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv1p1 is implemented and MPAMF_MSMON_IDR.HAS_OFLW_MSI == ‘1’. Otherwise, direct accesses to MSMON_OFLOW_MSI_ADDR_H are RES0.

Attributes

MSMON_OFLOW_MSI_ADDR_H is a 32-bit register.

Field descriptions

31 20 19 0

| | |
|---|---|
|RES0|MSI_ADDR_H|


Bits [31:20]

Reserved, RES0.

MSI_ADDR_H, bits [19:0]

MSI write address bits[51:32].

Accessing MSMON_OFLOW_MSI_ADDR_H

This register is within the MPAM feature page memory frames. In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_OFLW_MSI_ADDR_H_s must only be accessible from the Secure MPAM feature page.
- • MSMON_OFLW_MSI_ADDR_H_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_OFLW_MSI_ADDR_H_rt must only be accessible from the Root MPAM feature page.
- • MSMON_OFLW_MSI_ADDR_H_rl must only be accessible from the Realm MPAM feature page.


MSMON_OFLW_MSI_ADDR_H_s, MSMON_OFLW_MSI_ADDR_H_ns, MSMON_OFLW_MSI_ADDR_H_rt, and MSMON_OFLW_MSI_ADDR_H_rl must be separate registers:

- • The Secure instance (MSMON_OFLW_MSI_ADDR_H_s) accesses the high part of the monitor overflow MSI write address of Secure monitors.
- • The Non-secure instance (MSMON_OFLW_MSI_ADDR_H_ns) accesses the high part of the monitor overflow MSI write address of Non-secure monitors.
- • The Root instance (MSMON_OFLW_MSI_ADDR_H_rt) accesses the high part of the monitor overflow MSI write address of Root monitors.
- • The Realm instance (MSMON_OFLW_MSI_ADDR_H_rl) accesses the high part of the monitor overflow MSI write address of Realm monitors.


MSMON_OFLOW_MSI_ADDR_H can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x08E4 MSMON_OFLW_MSI_ADDR_H_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x08E4 MSMON_OFLW_MSI_ADDR_H_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x08E4 MSMON_OFLW_MSI_ADDR_H_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x08E4 MSMON_OFLW_MSI_ADDR_H_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.16 MSMON_OFLOW_MSI_ADDR_L, MPAM Monitor Overflow MSI Low-part Address RegisterThe MSMON_OFLOW_MSI_ADDR_L characteristics are:

Purpose

MSMON_OFLOW_MSI_ADDR_L is a 32-bit read/write register for the low part of the MPAM monitor MSI address.

MSMON_OFLOW_MSI_ADDR_L_s is the low part of the MSI write address for overflow interrupts from Secure monitor intances. MSMON_OFLOW_MSI_ADDR_L_ns is the low part of the MSI write address for overflow interrupts from Non-secure monitor instances. MSMON_OFLOW_MSI_ADDR_L_rt is the low part of the MSI write address for overflow interrupts from Root monitor intances. MSMON_OFLOW_MSI_ADDR_L_rl is the low part of the MSI write address for overflow interrupts from Realm monitor instances.

Configuration

The power domain of MSMON_OFLOW_MSI_ADDR_L is IMPLEMENTATION DEFINED MSMON_OFLOW_MSI_ADDR_L, MSMON_OFLOW_MSI_ADDR_H, MSMON_OFLOW_MSI_ATTR, MSMON_OFLOW_MSI_DATA, and MSMON_OFLOW_MSI_MPAM must all be implemented to support MSI writes for monitor overflow interrupts. The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv1p1 is implemented and MPAMF_MSMON_IDR.HAS_OFLW_MSI == ‘1’. Otherwise, direct accesses to MSMON_OFLOW_MSI_ADDR_L are RES0.

Attributes

MSMON_OFLOW_MSI_ADDR_L is a 32-bit register.

Field descriptions

31 2 0

| | |
|---|---|
|MSI_ADDR_L|RES0|


MSI_ADDR_L, bits [31:2]

MSI write address bits[31:2].

Bits [1:0]

Reserved, RES0.

Accessing MSMON_OFLOW_MSI_ADDR_L

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_OFLOW_MSI_ADDR_L_s must only be accessible from the Secure MPAM feature page.
- • MSMON_OFLOW_MSI_ADDR_L_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_OFLOW_MSI_ADDR_L_rt must only be accessible from the Root MPAM feature page.
- • MSMON_OFLOW_MSI_ADDR_L_rl must only be accessible from the Realm MPAM feature page.


- • MSMON_OFLOW_MSI_ADDR_L_s, MSMON_OFLOW_MSI_ADDR_L_ns, MSMON_OFLOW_MSI_ADDR_L_rt, and MSMON_OFLOW_MSI_ADDR_L_rl must be separate registers:
- • The Secure instance (MSMON_OFLOW_MSI_ADDR_L_s) accesses the low part of the overflow MSI write address used for Secure PARTIDs.


- • The Non-secure instance (MSMON_OFLOW_MSI_ADDR_L_ns) accesses the low part of the overflow MSI write address used for Non-secure PARTIDs.
- • The Root instance (MSMON_OFLOW_MSI_ADDR_L_rt) accesses the low part of the overflow MSI write address used for Root PARTIDs.
- • The Realm instance (MSMON_OFLOW_MSI_ADDR_L_rl) accesses the low part of the overflow MSI write address used for Realm PARTIDs.


MSMON_OFLOW_MSI_ADDR_L can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x08E0 MSMON_OFLOW_MSI_ADDR_L_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x08E0 MSMON_OFLOW_MSI_ADDR_L_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x08E0 MSMON_OFLOW_MSI_ADDR_L_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x08E0 MSMON_OFLOW_MSI_ADDR_L_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.17 MSMON_OFLOW_MSI_ATTR, MPAM Monitor Overflow MSI Write Attributes RegisterThe MSMON_OFLOW_MSI_ATTR characteristics are:

Purpose

MSMON_OFLOW_MSI_ATTR is a 32-bit read/write register that controls MPAM monitor overflow MSI write attributes for MPAM monitor overflows in this MSC.

MSMON_OFLOW_MSI_ATTR_s controls Secure MPAM monitor overflow MSI writes. MSMON_OFLOW_MSI_ATTR_ns controls Non-secure MPAM monitor overflow MSI writes. MSMON_OFLOW_MSI_ATTR_rt controls Root MPAM monitor overflow MSI writes. MSMON_OFLOW_MSI_ATTR_rl controls Realm MPAM monitor overflow MSI writes.

Configuration

The power domain of MSMON_OFLOW_MSI_ATTR is IMPLEMENTATION DEFINED MSMON_OFLOW_MSI_ADDR_L, MSMON_OFLOW_MSI_ADDR_H, MSMON_OFLOW_MSI_ATTR, MSMON_OFLOW_MSI_DATA, and MSMON_OFLOW_MSI_MPAM must all be implemented to support MSI writes for monitor overflow interrupts. The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv1p1 is implemented and MPAMF_MSMON_IDR.HAS_OFLW_MSI == ‘1’. Otherwise, direct accesses to MSMON_OFLOW_MSI_ATTR are RES0.

Attributes

MSMON_OFLOW_MSI_ATTR is a 32-bit register.

Field descriptions

31 30 29 28 27 24 23 1 0

| | | | | |
|---|---|---|---|---|
|RES0|MSI_SH|MSI_MEMATTR|RES0| |


MSIEN

Bits [31:30]

Reserved, RES0.

MSI_SH, bits [29:28]

Sharability attribute of MSI writes.

MSI_SH Meaning

0b00 Non-shareable. 0b01 Reserved, CONSTRAINED UNPREDICTABLE.

- 0b10 Outer Shareable.

- 0b11 Inner Shareable.


When MSMON_OFLOW_MSI_ATTR.MSI_MEMATTR specifies a Device memory type, the contents of this field are IGNORED and Shareability is effectively Outer Shareable.

MSI_MEMATTR, bits [27:24]

Memory attributes of MSI writes.

###### Note

This encoding matches the VMSAv8-64 stage 2 MemAttr[3:0] field as described in the Arm ARM, except that the following encodings are Reserved (not UNPREDICTABLE) and behave as DEvice-nGnRnE: 0b0100, 0b1000, and 0b1100.

MSI_MEMATTR Meaning

0b0000 Device-nGnRnE. 0b0001 Device-nGnRE. 0b0010 Device-nGRE. 0b0011 Device-GRE. 0b0100 Reserved. Behave as Device-nGnRnE, 0b0000. 0b0101 Normal Inner Non-cacheable, Outer Non-cacheable. 0b0110 Normal Inner Write-Through Cacheable, Outer Non-cacheable. 0b0111 Normal Inner Write-Back Cacheable, Outer Non-cacheable. 0b1000 Reserved. Behave as Device-nGnRnE, 0b0000. 0b1001 Normal Inner Non-Cachable, Outer Write-Through Cacheable. 0b1010 Normal Inner Write-Through Cacheable, Outer Write-Through

Cachable. 0b1011 Normal Inner Write-Back Cacheable, Outer Write-Through Cachable. 0b1100 Reserved. Behave as Device-nGnRnE, 0b0000. 0b1101 Normal Inner Non-cacheable, Outer Write-Back Cacheable. 0b1110 Normal Inner Write-Through Cacheable, Outer Write-Back

Cacheable. 0b1111 Normal Inner Write-Back Cacheable, Outer Write-Back Cacheable.

When this field specifies a Device memory type, the contents of MSMON_OFLOW_MSI_ATTR.MSI_SH are IGNORED and Shareability is effectively Outer Shareable.

Device types may be implemented as any Device type with more n characters. For example, if this field is set to 0b0010, an implementation may treat the MSI write as the specified type, Device-nGRE, or as Device-nGnRE or as Device-nGnRnE. Reserved encodings 0b0100, 0b1000, and 0b1100 must be implemented to behave the same as the 0b0000 encoding.

Bits [23:1]

Reserved, RES0.

MSIEN, bit [0]

Monitor overflow MSI write enable.

MSIEN Meaning

- 0b0 MPAM monitor overflow MSI writes are not generated to signal enabled MPAM monitor overflow interrupts. When monitor overflow MSI writes are disabled, hardwired monitor overflow interrupt could be generated if hardwired monitor overflow interrupt is implemented.

- 0b1 MPAM monitor overflow MSI writes are generated to signal enabled MPAM monitor overflow interrupts. When monitor overflow MSI writes are enabled, hardwired monitor overflow interrupts are not generated.


This enable affects whether a hardwired overlow interrupt is generated. The reset behavior of this field is:

• On a MSC reset, this field resets to '0'.

Accessing MSMON_OFLOW_MSI_ATTR

This register is within the MPAM feature page memory frames. In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_OFLOW_MSI_ATTR_s must only be accessible from the Secure MPAM feature page.
- • MSMON_OFLOW_MSI_ATTR_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_OFLOW_MSI_ATTR_rt must only be accessible from the Root MPAM feature page.
- • MSMON_OFLOW_MSI_ATTR_rl must only be accessible from the Realm MPAM feature page.


MSMON_OFLOW_MSI_ATTR_s, MSMON_OFLOW_MSI_ATTR_ns, MSMON_OFLOW_MSI_ATTR_rt, and MSMON_OFLOW_MSI_ATTR_rl must be separate registers:

- • The Secure instance (MSMON_OFLOW_MSI_ATTR_s) accesses the monitor overflow MSI write attributes of Secure monitors.
- • The Non-secure instance (MSMON_OFLOW_MSI_ATTR_ns) accesses the monitor overflow MSI write attributes of Non-secure monitors.
- • The Root instance (MSMON_OFLOW_MSI_ATTR_rt) accesses the monitor overflow MSI write attributes of Root monitors.
- • The Realm instance (MSMON_OFLOW_MSI_ATTR_rl) accesses the monitor overflow MSI write attributes of Realm monitors.


MSMON_OFLOW_MSI_ATTR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x08EC MSMON_OFLOW_MSI_ATTR_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x08EC MSMON_OFLOW_MSI_ATTR_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x08EC MSMON_OFLOW_MSI_ATTR_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x08EC MSMON_OFLOW_MSI_ATTR_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.18 MSMON_OFLOW_MSI_DATA, MPAM Monitor Overflow MSI Write Data RegisterThe MSMON_OFLOW_MSI_DATA characteristics are:

Purpose

MSMON_OFLOW_MSI_DATA is a 32-bit read/write register for the MPAM monitor overflow MSI data. MSMON_OFLOW_MSI_DATA_s is the data for the MSI write for monitor overflow from Secure monitor instances. MSMON_OFLOW_MSI_DATA_ns is the data for the MSI writes for monitor overflow interrupts from Non-secure monitor instances. MSMON_OFLOW_MSI_DATA_rt is the data for the MSI write for monitor overflow from Root monitor instances. MSMON_OFLOW_MSI_DATA_rl is the data for the MSI writes for monitor overflow interrupts from Realm monitor instances.

Configuration

The power domain of MSMON_OFLOW_MSI_DATA is IMPLEMENTATION DEFINED MSMON_OFLOW_MSI_ADDR_L, MSMON_OFLOW_MSI_ADDR_H, MSMON_OFLOW_MSI_ATTR, MSMON_OFLOW_MSI_DATA, and MSMON_OFLOW_MSI_MPAM must all be implemented to support MSI writes for monitor overflow interrupts. The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv1p1 is implemented and MPAMF_MSMON_IDR.HAS_OFLW_MSI == ‘1’. Otherwise, direct accesses to MSMON_OFLOW_MSI_DATA are RES0.

Attributes

MSMON_OFLOW_MSI_DATA is a 32-bit register.

Field descriptions

31 0

MSI_DATA

MSI_DATA, bits [31:0]

MSI write data word.

Accessing MSMON_OFLOW_MSI_DATA

This register is within the MPAM feature page memory frames. In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_OFLOW_MSI_DATA_s must only be accessible from the Secure MPAM feature page.
- • MSMON_OFLOW_MSI_DATA_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_OFLOW_MSI_DATA_rt must only be accessible from the Root MPAM feature page.
- • MSMON_OFLOW_MSI_DATA_rl must only be accessible from the Realm MPAM feature page.


MSMON_OFLOW_MSI_DATA_s, MSMON_OFLOW_MSI_DATA_ns, MSMON_OFLOW_MSI_DATA_rt, and MSMON_OFLOW_MSI_DATA_rl must be separate registers:

- • The Secure instance (MSMON_OFLOW_MSI_DATA_s) accesses the monitor overflow MSI write data of Secure monitors.
- • The Non-secure instance (MSMON_OFLOW_MSI_DATA_ns) accesses the monitor overflow MSI write data of Non-secure monitors.


- • The Root instance (MSMON_OFLOW_MSI_DATA_rt) accesses the monitor overflow MSI write data of Root monitors.
- • The Realm instance (MSMON_OFLOW_MSI_DATA_rl) accesses the monitor overflow MSI write data of Realm monitors.


MSMON_OFLOW_MSI_DATA can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x08E8 MSMON_OFLOW_MSI_DATA_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x08E8 MSMON_OFLOW_MSI_DATA_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x08E8 MSMON_OFLOW_MSI_DATA_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x08E8 MSMON_OFLOW_MSI_DATA_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.19 MSMON_OFLOW_MSI_MPAM, MPAM Monitor Overflow MSI Write MPAM Information RegisterThe MSMON_OFLOW_MSI_MPAM characteristics are:

Purpose

MSMON_OFLOW_MSI_MPAM is a 32-bit read/write register that sets the MPAM information for a monitor overflow MSI write.

MSMON_OFLOW_MSI_MPAM_s controls MPAM information labeling of Secure monitor overflow MSI writes. MSMON_OFLOW_MSI_MPAM_ns controls MPAM information labeling of Non-secure monitor overflow MSI writes. MSMON_OFLOW_MSI_MPAM_rt controls MPAM information labeling of Root monitor overflow MSI writes. MSMON_OFLOW_MSI_MPAM_rl controls MPAM information labeling of Realm monitor overflow MSI writes.

Configuration

The power domain of MSMON_OFLOW_MSI_MPAM is IMPLEMENTATION DEFINED MSMON_OFLOW_MSI_ADDR_L, MSMON_OFLOW_MSI_ADDR_H, MSMON_OFLOW_MSI_ATTR, MSMON_OFLOW_MSI_DATA, and MSMON_OFLOW_MSI_MPAM must all be implemented to support MSI writes for monitor overflow interrupts. The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv1p1 is implemented and MPAMF_MSMON_IDR.HAS_OFLW_MSI == ‘1’. Otherwise, direct accesses to MSMON_OFLOW_MSI_MPAM are RES0.

Attributes

MSMON_OFLOW_MSI_MPAM is a 32-bit register.

Field descriptions

31 24 23 16 15 0

| | | |
|---|---|---|
|RES0|PMG|PARTID|


Bits [31:24]

Reserved, RES0.

PMG, bits [23:16]

Performance monitoring group property for an MSC monitor overflow MSI write. The reset behavior of this field is:

• On a MSC reset, this field resets to an architecturally UNKNOWN value.

PARTID, bits [15:0]

Partition ID for an MSC monitor overflow MSI write. The PARTID in this field is in the Secure PARTID space in the MSMON_OFLOW_MSI_MPAM_s instance and in the Non-secure PARTID space in the MSMON_OFLOW_MSI_MPAM_ns instance of this register. The reset behavior of this field is:

• On a MSC reset, this field resets to an architecturally UNKNOWN value.

Accessing MSMON_OFLOW_MSI_MPAM

This register is within the MPAM feature page memory frames. In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_OFLOW_MSI_MPAM_s must only be accessible from the Secure MPAM feature page.
- • MSMON_OFLOW_MSI_MPAM_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_OFLOW_MSI_MPAM_rt must only be accessible from the Root MPAM feature page.
- • MSMON_OFLOW_MSI_MPAM_rl must only be accessible from the Realm MPAM feature page.


MSMON_OFLOW_MSI_MPAM_s, MSMON_OFLOW_MSI_MPAM_ns, MSMON_OFLOW_MSI_MPAM_rt, and MSMON_OFLOW_MSI_MPAM_rl must be separate registers:

- • The Secure instance (MSMON_OFLOW_MSI_MPAM_s) accesses the monitor overflow MSI MPAM information of Secure monitors.
- • The Non-secure instance (MSMON_OFLOW_MSI_MPAM_ns) accesses the monitor overflow MSI MPAM information of Non-secure monitors.
- • The Root instance (MSMON_OFLOW_MSI_MPAM_rt) accesses the monitor overflow MSI MPAM information of Root monitors.
- • The Realm instance (MSMON_OFLOW_MSI_MPAM_rl) accesses the monitor overflow MSI MPAM information of Realm monitors.


MSMON_OFLOW_MSI_MPAM can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x08DC MSMON_OFLOW_MSI_MPAM_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x08DC MSMON_OFLOW_MSI_MPAM_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x08DC MSMON_OFLOW_MSI_MPAM_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x08DC MSMON_OFLOW_MSI_MPAM_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.7.20 MSMON_OFLOW_SR, MPAM Monitor Overflow Status RegisterThe MSMON_OFLOW_SR characteristics are:

Purpose

MSMON_OFLOW_SR is a 32-bit read-only register that shows MPAM monitor overflow status for this MSC. MSMON_OFLOW_SR_s gives the status of overflows of Secure MPAM monitors. MSMON_OFLOW_SR_ns gives the status of overflows of Non-secure MPAM monitors. MSMON_OFLOW_SR_rt gives the status of overflows of Root MPAM monitors. MSMON_OFLOW_SR_rl gives the status of overflows of Realm MPAM monitors.

Configuration

The power domain of MSMON_OFLOW_SR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when ((IsFeatureImplemented(FEAT_MPAMv0p1) || IsFeatureImplemented(FEAT_MPAMv1p0)) && (MPAMF_IDR.HAS_MSMON == ‘1’)) && (MPAMF_MSMON_IDR.HAS_OFLOW_SR == ‘1’). Otherwise, direct accesses to MSMON_OFLOW_SR are RES0.

Attributes

MSMON_OFLOW_SR is a 32-bit register.

Field descriptions

31 30 29 28 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0

| | | | | | | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| | | |RES0| | | | | | | | | | | | | | | | |


CSU_OFL OW_PND

CSA_OFLOW_PND MBWU_OFLOW_PND

RIS_PND15 RIS_PND14 RIS_PND13 RIS_PND12 RIS_PND11 RIS_PND10 RIS_PND9 RIS_PND8

RIS_PND 0 RIS_PND1 RIS_PND2

RIS_PND3 RIS_PND4

RIS_PND5 RIS_PND6

RIS_PND7

CSU_OFLOW_PND, bit [31]

At least one cache storage usage monitor has OFLOW_STATUS of 1 in MSMON_CFG_CSU_CTL.

CSU_OFLOW_PND Meaning

- 0b0 There are no cache storage usage monitor instances where MSMON_CFG_CSU_CTL.OFLOW_STATUS is 1.

- 0b1 MSMON_CFG_CSU_CTL for at least one of the cache storage usage monitor instances has OFLOW_STATUS set to 1.


This field clears when MSMON_CFG_CSU_CTL.OFLOW_STATUS has been reset to 0 for all CSU monitor instances in this MSC.

MBWU_OFLOW_PND, bit [30]

At least one memory bandwidth usage monitor instance has OFLOW_STATUS or OFLOW_STATUS_L of 1 in MSMON_CFG_MBWU_CTL.

MBWU_OFLOW_PND Meaning

- 0b0 There are no memory bandwidth usage monitor instances where MSMON_CFG_MBWU_CTL.OFLOW_STATUS is 1.

- 0b1 MSMON_CFG_MBWU_CTL for at least one of the memory bandwidth usage monitor instances has either OFLOW_STATUS or OFLOW_STATUS_L set to 1.


This field clears when MSMON_CFG_MBWU_CTL.OFLOW_STATUS and MSMON_CFG_MBWU_CTL.OFLOW_STATUS_L have been reset to 0 for all MBWU monitor instances in this MSC.

CSA_OFLOW_PND, bit [29] When MPAMF_MSMON_IDR.MSMON_CSA == '1':

If MPAMF_CSAMON_IDR.HAS_LONG is 1, reports that MSMON_CFG_CSA_CTL.{OFLOW_STATUS,OFLOW_STATUS_L} are {1,x} or {x,1} for at least one cache storage allocation monitor instance. Otherwise, reports that MSMON_CFG_CSA_CTL.OFLOW_STATUS is 1 for at least one cache storage allocation monitor instance.

CSA_OFLOW_PND Meaning

- 0b0 If MPAMF_CSAMON_IDR.HAS_LONG is 1, then MSMON_CFG_CSA_CTL.{OFLOW_STATUS,OFLOW_STATUS_L} are {0,0} for all cache storage allocation monitor instances. Otherwise, MSMON_CFG_CSA_CTL.OFLOW_STATUS is 0 for all cache storage allocation monitor instances.

- 0b1 If MPAMF_CSAMON_IDR.HAS_LONG is 1, then MSMON_CFG_CSA_CTL.{OFLOW_STATUS,OFLOW_STATUS_L} are {1,x} or {x,1} for at least one cache storage allocation monitor instance. Otherwise, MSMON_CFG_CSA_CTL.OFLOW_STATUS is 1 for at least one cache storage allocation monitor instance.


If MPAMF_CSAMON_IDR.HAS_LONG is 1, the value of this field becomes 0 when MSMON_CFG_CSA_CTL.{OFLOW_STATUS,OFLOW_STATUS_L} are reset to {0,0} for all cache storage allocation monitor instances. Otherwise, the value of this field becomes 0 when MSMON_CFG_CSA_CTL.OFLOW_STATUS is reset to 0 for all cache storage allocation monitor instances.

The reset behavior of this field is:

• On a MSC reset, this field resets to an architecturally UNKNOWN value.

Otherwise:

Reserved, RES0.

Bits [28:16]

Reserved, RES0.

RIS_PND<r> , bits [r], for r = 15 to 0

Overflow status by RIS.

RIS_PND<r> Meaning

0b0 RIS r has no unread overflows of any type of monitor. 0b1 RIS r has at least one unread overflow in at least one of the

monitor types.

Combined with the CSU_OFLOW_PND and MBWU_OFLOW_PND flags in this register, an interrupt service routine could poll only the monitor types indicated in monitors for the resource instances flagged in this field.

Bit r is set when any monitor instance of any type in resource instance r has OFLOW_STATUS or OFLOW_STATUS_L set to 1.

Accessing MSMON_OFLOW_SR

This register is within the MPAM feature page memory frames. In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MSMON_OFLOW_SR_s must only be accessible from the Secure MPAM feature page.
- • MSMON_OFLOW_SR_ns must only be accessible from the Non-secure MPAM feature page.
- • MSMON_OFLOW_SR_rt must only be accessible from the Root MPAM feature page.
- • MSMON_OFLOW_SR_rl must only be accessible from the Realm MPAM feature page.


MSMON_OFLOW_SR_s, MSMON_OFLOW_SR_ns, MSMON_OFLOW_SR_rt, and MSMON_OFLOW_SR_rl must be separate registers:

- • The Secure instance (MSMON_OFLOW_SR_s) accesses the monitor overflow status summary of Secure monitors.
- • The Non-secure instance (MSMON_OFLOW_SR_ns) accesses the monitor overflow status summary of Non-secure monitors.
- • The Root instance (MSMON_OFLOW_SR_rt) accesses the monitor overflow status summary of Root monitors.
- • The Realm instance (MSMON_OFLOW_SR_rl) accesses the monitor overflow status summary of Realm monitors.


MSMON_OFLOW_SR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x08F0 MSMON_OFLOW_SR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x08F0 MSMON_OFLOW_SR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x08F0 MSMON_OFLOW_SR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x08F0 MSMON_OFLOW_SR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.8 Memory-mapped error control and status register descriptionThis section lists the external error control and status registers.

###### 9.8.1 MPAMF_ECR, MPAM Error Control Register

The MPAMF_ECR characteristics are:

Purpose

MPAMF_ECR is a 32-bit read/write register that controls MPAM error interrupts for this MSC. MPAMF_ECR_s controls Secure MPAM error handling. MPAMF_ECR_ns controls Non-secure MPAM error handling. MPAMF_ECR_rt controls Root MPAM error handling. MPAMF_ECR_rl controls Realm MPAM error handling.

Configuration

The power domain of MPAMF_ECR is IMPLEMENTATION DEFINED If an MSC cannot encounter any of the error conditions listed in ‘Errors in MSCs’ in Arm® Architecture Reference Manual Supplement, Memory System Resource Partitioning and Monitoring (MPAM), for Armv8-A (ARM DDI 0598), both the MPAMF_ESR and MPAMF_ECR must be RAZ/WI. The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented. Otherwise, direct accesses to MPAMF_ECR are RES0.

Attributes

MPAMF_ECR is a 32-bit register.

Field descriptions

31 1 0

| | |
|---|---|
|RES0| |


INTEN

Bits [31:1]

Reserved, RES0.

INTEN, bit [0]

Interrupt Enable.

INTEN Meaning

0b0 MPAM error interrupts are not signaled. 0b1 MPAM error interrupts are signaled.

Accessing MPAMF_ECR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:


- • MPAMF_ECR_s must only be accessible from the Secure MPAM feature page.
- • MPAMF_ECR_ns must only be accessible from the Non-secure MPAM feature page.


- • MPAMF_ECR_rt must only be accessible from the Root MPAM feature page.
- • MPAMF_ECR_rl must only be accessible from the Realm MPAM feature page.


- • MPAMF_ECR_s, MPAMF_ECR_ns, MPAMF_ECR_rt, and MPAMF_ECR_rl must be separate registers:
- • The Secure instance (MPAMF_ECR_s) accesses the error interrupt controls used for Secure PARTIDs.
- • The Non-secure instance (MPAMF_ECR_ns) accesses the error interrupt controls used for Non-secure PARTIDs.
- • The Root instance (MPAMF_ECR_rt) accesses the error interrupt controls used for Root PARTIDs.
- • The Realm instance (MPAMF_ECR_rl) accesses the error interrupt controls used for Realm PARTIDs.


MPAMF_ECR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x00F0 MPAMF_ECR_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x00F0 MPAMF_ECR_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x00F0 MPAMF_ECR_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x00F0 MPAMF_ECR_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.8.2 MPAMF_ERR_MSI_ADDR_H, MPAM Error MSI High-part Address Register

The MPAMF_ERR_MSI_ADDR_H characteristics are:

Purpose

MPAMF_ERR_MSI_ADDR_H is a 32-bit read/write register for the high part of the MPAM error MSI address. MPAMF_ERR_MSI_ADDR_H_s is the high part of the MSI write address for error interrupts related to Secure PARTIDs. MPAMF_ERR_MSI_ADDR_H_ns is the high part of the MSI write address for error interrupts related to Non-secure PARTIDs. MPAMF_ERR_MSI_ADDR_H_rt is the high part of the MSI write address for error interrupts related to Root PARTIDs. MPAMF_ERR_MSI_ADDR_H_rl is the high part of the MSI write address for error interrupts related to Realm PARTIDs.

Configuration

The power domain of MPAMF_ERR_MSI_ADDR_H is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_ERR_MSI == ‘1’. Otherwise, direct accesses to MPAMF_ERR_MSI_ADDR_H are RES0.

Attributes

MPAMF_ERR_MSI_ADDR_H is a 32-bit register.

Field descriptions

31 20 19 0

| | |
|---|---|
|RES0|MSI_ADDR_H|


Bits [31:20]

Reserved, RES0.

MSI_ADDR_H, bits [19:0]

MSI write address bits[51:32].

Accessing MPAMF_ERR_MSI_ADDR_H

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMF_ERR_MSI_ADDR_H_s must only be accessible from the Secure MPAM feature page.
- • MPAMF_ERR_MSI_ADDR_H_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMF_ERR_MSI_ADDR_H_rt must only be accessible from the Root MPAM feature page.
- • MPAMF_ERR_MSI_ADDR_H_rl must only be accessible from the Realm MPAM feature page.


- • MPAMF_ERR_MSI_ADDR_H_s, MPAMF_ERR_MSI_ADDR_H_ns, MPAMF_ERR_MSI_ADDR_H_rt, and MPAMF_ERR_MSI_ADDR_H_rl must be separate registers:
- • The Secure instance (MPAMF_ERR_MSI_ADDR_H_s) accesses the high part of the memory address for MSI write to signal an MPAM error used for Secure PARTIDs.
- • The Non-secure instance (MPAMF_ERR_MSI_ADDR_H_ns) accesses the high part of the memory address for MSI write to signal an MPAM error used for Non-secure PARTIDs.
- • The Root instance (MPAMF_ERR_MSI_ADDR_H_rt) accesses the high part of the memory address for MSI write to signal an MPAM error used for Root PARTIDs.


- • The Realm instance (MPAMF_ERR_MSI_ADDR_H_rl) accesses the high part of the memory address for MSI write to signal an MPAM error used for Realm PARTIDs.


MPAMF_ERR_MSI_ADDR_H can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x00E4 MPAMF_ERR_MSI_ADDR_H_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x00E4 MPAMF_ERR_MSI_ADDR_H_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x00E4 MPAMF_ERR_MSI_ADDR_H_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x00E4 MPAMF_ERR_MSI_ADDR_H_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.8.3 MPAMF_ERR_MSI_ADDR_L, MPAM Error MSI Low-part Address Register

The MPAMF_ERR_MSI_ADDR_L characteristics are:

Purpose

MPAMF_ERR_MSI_ADDR_L is a 32-bit read/write register for the low part of the MPAM error MSI address. MPAMF_ERR_MSI_ADDR_L_s is the low part of the MSI write address for error interrupts related to Secure PARTIDs. MPAMF_ERR_MSI_ADDR_L_ns is the low part of the MSI write address for error interrupts related to Non-secure PARTIDs. MPAMF_ERR_MSI_ADDR_L_rt is the low part of the MSI write address for error interrupts related to Root PARTIDs. MPAMF_ERR_MSI_ADDR_L_rl is the low part of the MSI write address for error interrupts related to Realm PARTIDs.

Configuration

The power domain of MPAMF_ERR_MSI_ADDR_L is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_ERR_MSI == ‘1’. Otherwise, direct accesses to MPAMF_ERR_MSI_ADDR_L are RES0.

Attributes

MPAMF_ERR_MSI_ADDR_L is a 32-bit register.

Field descriptions

31 2 0

| | |
|---|---|
|MSI_ADDR_L|RES0|


MSI_ADDR_L, bits [31:2]

MSI write address bits[31:2].

Bits [1:0]

Reserved, RES0.

Accessing MPAMF_ERR_MSI_ADDR_L

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMF_ERR_MSI_ADDR_L_s must only be accessible from the Secure MPAM feature page.
- • MPAMF_ERR_MSI_ADDR_L_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMF_ERR_MSI_ADDR_L_rt must only be accessible from the Root MPAM feature page.
- • MPAMF_ERR_MSI_ADDR_L_rl must only be accessible from the Realm MPAM feature page.


• MPAMF_ERR_MSI_ADDR_L_s, MPAMF_ERR_MSI_ADDR_L_ns, MPAMF_ERR_MSI_ADDR_L_rt, and MPAMF_ERR_MSI_ADDR_L_rl must be separate registers:

- • The Secure instance (MPAMF_ERR_MSI_ADDR_L_s) accesses the low part of the memory address for MSI write to signal an MPAM error used for Secure PARTIDs.
- • The Non-secure instance (MPAMF_ERR_MSI_ADDR_L_ns) accesses the low part of the memory address for MSI write to signal an MPAM error used for Non-secure PARTIDs.
- • The Root instance (MPAMF_ERR_MSI_ADDR_L_rt) accesses the low part of the memory address for MSI write to signal an MPAM error used for Root PARTIDs.


• The Realm instance (MPAMF_ERR_MSI_ADDR_L_rl) accesses the low part of the memory address for MSI write to signal an MPAM error used for Realm PARTIDs.

MPAMF_ERR_MSI_ADDR_L can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x00E0 MPAMF_ERR_MSI_ADDR_L_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x00E0 MPAMF_ERR_MSI_ADDR_L_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x00E0 MPAMF_ERR_MSI_ADDR_L_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x00E0 MPAMF_ERR_MSI_ADDR_L_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.8.4 MPAMF_ERR_MSI_ATTR, MPAM Error MSI Write Attributes Register

The MPAMF_ERR_MSI_ATTR characteristics are:

Purpose

MPAMF_ERR_MSI_ATTR is a 32-bit read/write register that controls MPAM error MSI write attributes for MPAM errors in this MSC.

MPAMF_ERR_MSI_ATTR_s controls the attributes of Secure MPAM error MSI writes. MPAMF_ERR_MSI_ATTR_ns controls the attributes of Non-secure MPAM error MSI writes. MPAMF_ERR_MSI_ATTR_rt controls the attributes of Root MPAM error MSI writes. MPAMF_ERR_MSI_ATTR_rl controls the attributes of Realm MPAM error MSI writes.

Configuration

The power domain of MPAMF_ERR_MSI_ATTR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_ERR_MSI == ‘1’. Otherwise, direct accesses to MPAMF_ERR_MSI_ATTR are RES0.

Attributes

MPAMF_ERR_MSI_ATTR is a 32-bit register.

Field descriptions

31 30 29 28 27 24 23 1 0

| | | | | |
|---|---|---|---|---|
|RES0|MSI_SH|MSI_MEMATTR|RES0| |


MSIEN

Bits [31:30]

Reserved, RES0.

MSI_SH, bits [29:28]

Sharability attribute of MSI writes.

MSI_SH Meaning

0b00 Non-shareable. 0b01 Reserved, CONSTRAINED UNPREDICTABLE. 0b10 Outer Shareable. 0b11 Inner Shareable.

When MPAMF_ERR_MSI_ATTR.MSI_MEMATTR specifies a Device memory type, the contents of this field are IGNORED and Shareability is effectively Outer Shareable.

MSI_MEMATTR, bits [27:24]

Memory attributes of MSI writes.

###### Note

This encoding matches the VMSAv8-64 stage 2 MemAttr[3:0] field as described in the Arm ARM, except that the following encodings are Reserved (not UNPREDICTABLE).

MSI_MEMATTR Meaning

0b0000 Device-nGnRnE. 0b0001 Device-nGnRE. 0b0010 Device-nGRE. 0b0011 Device-GRE. 0b0100 Reserved. Behave as Device-nGnRnE, 0b0000. 0b0101 Normal Inner Non-cacheable, Outer Non-cacheable. 0b0110 Normal Inner Write-Through Cacheable, Outer Non-cacheable. 0b0111 Normal Inner Write-Back Cacheable, Outer Non-cacheable. 0b1000 Reserved. Behave as Device-nGnRnE, 0b0000. 0b1001 Normal Inner Non-Cachable, Outer Write-Through Cacheable. 0b1010 Normal Inner Write-Through Cacheable, Outer Write-Through

Cacheable. 0b1011 Normal Inner Write-Back Cacheable, Outer Write-Through Cacheable. 0b1100 Reserved. Behave as Device-nGnRnE, 0b0000. 0b1101 Normal Inner Non-cacheable, Outer Write-Back Cacheable. 0b1110 Normal Inner Write-Through Cacheable, Outer Write-Back Cacheable. 0b1111 Normal Inner Write-Back Cacheable, Outer Write-Back Cacheable.

When this field specifies a Device memory type, the contents of MPAMF_ERR_MSI_ATTR.MSI_SH are IGNORED and Shareability is effectively Outer Shareable.

Device types may be implemented as any Device type with more than ‘n’ characters. For example, if this field is set to 0b0010, an implementation may treat the MSI write as the specified type, Device-nGRE, or as Device-nGnRE or as Device-nGnRnE.

Reserved encodings 0b0100, 0b1000, and 0b1100 must be implemented to behave the same as the 0b0000 encoding.

Bits [23:1]

Reserved, RES0.

MSIEN, bit [0]

Error interrupt MSI Enable.

MSIEN Meaning

- 0b0 MPAM error MSI writes are not generated to signal enabled MPAM error interrupts. When error MSI writes are disabled, hardwired error interrupts could be generated.

- 0b1 MPAM error MSI writes are generated to signal enabled MPAM error interrupts. When error MSI writes are enabled, hardwired error interrupts are not generated.


The value of this field affects whether hardwired error interrupts are generated. The reset behavior of this field is:

• On a MSC reset, this field resets to '0'.

Accessing MPAMF_ERR_MSI_ATTR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMF_ERR_MSI_ATTR_s must only be accessible from the Secure MPAM feature page.
- • MPAMF_ERR_MSI_ATTR_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMF_ERR_MSI_ATTR_rt must only be accessible from the Root MPAM feature page.
- • MPAMF_ERR_MSI_ATTR_rl must only be accessible from the Realm MPAM feature page.


• MPAMF_ERR_MSI_ATTR_s, MPAMF_ERR_MSI_ATTR_ns, MPAMF_ERR_MSI_ATTR_rt, and MPAMF_ERR_MSI_ATTR_rl must be separate registers:

- • The Secure instance (MPAMF_ERR_MSI_ATTR_s) accesses the memory access attributes for MSI write to signal an MPAM error used for Secure PARTIDs.
- • The Non-secure instance (MPAMF_ERR_MSI_ATTR_ns) accesses the memory access attributes for MSI write to signal an MPAM error used for Non-secure PARTIDs.
- • The Root instance (MPAMF_ERR_MSI_ATTR_rt) accesses the memory access attributes for MSI write to signal an MPAM error used for Root PARTIDs.
- • The Realm instance (MPAMF_ERR_MSI_ATTR_rl) accesses the memory access attributes for MSI write to signal an MPAM error used for Realm PARTIDs.


MPAMF_ERR_MSI_ATTR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x00EC MPAMF_ERR_MSI_ATTR_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x00EC MPAMF_ERR_MSI_ATTR_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x00EC MPAMF_ERR_MSI_ATTR_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x00EC MPAMF_ERR_MSI_ATTR_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.8.5 MPAMF_ERR_MSI_DATA, MPAM Error MSI Data Register

The MPAMF_ERR_MSI_DATA characteristics are:

Purpose

MPAMF_ERR_MSI_DATA is a 32-bit read/write register for the MPAM error MSI data. MPAMF_ERR_MSI_DATA_s is the data for the MSI write for error interrupts related to Secure PARTIDs. MPAMF_ERR_MSI_DATA_ns is the data for the MSI write for error interrupts related to Non-secure PARTIDs. MPAMF_ERR_MSI_DATA_rt is the data for the MSI write for error interrupts related to Root PARTIDs. MPAMF_ERR_MSI_DATA_rl is the data for the MSI write for error interrupts related to Realm PARTIDs.

Configuration

The power domain of MPAMF_ERR_MSI_DATA is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_ERR_MSI == ‘1’. Otherwise, direct accesses to MPAMF_ERR_MSI_DATA are RES0.

Attributes

MPAMF_ERR_MSI_DATA is a 32-bit register.

Field descriptions

31 0

MSI_DATA

MSI_DATA, bits [31:0]

MSI data to be written to ITS to signal an MSI.

Accessing MPAMF_ERR_MSI_DATA

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMF_ERR_MSI_DATA_s must only be accessible from the Secure MPAM feature page.
- • MPAMF_ERR_MSI_DATA_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMF_ERR_MSI_DATA_rt must only be accessible from the Root MPAM feature page.
- • MPAMF_ERR_MSI_DATA_rl must only be accessible from the Realm MPAM feature page.


- • MPAMF_ERR_MSI_DATA_s, MPAMF_ERR_MSI_DATA_ns, MPAMF_ERR_MSI_DATA_rt, and MPAMF_ERR_MSI_DATA_rl must be separate registers:
- • The Secure instance (MPAMF_ERR_MSI_DATA_s) accesses the data for MSI write to signal an MPAM error used for Secure PARTIDs.
- • The Non-secure instance (MPAMF_ERR_MSI_DATA_ns) accesses the data for MSI write to signal an MPAM error used for Non-secure PARTIDs.
- • The Root instance (MPAMF_ERR_MSI_DATA_rt) accesses the data for MSI write to signal an MPAM error used for Root PARTIDs.
- • The Realm instance (MPAMF_ERR_MSI_DATA_rl) accesses the data for MSI write to signal an MPAM error used for Realm PARTIDs.


MPAMF_ERR_MSI_DATA can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x00E8 MPAMF_ERR_MSI_DATA_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x00E8 MPAMF_ERR_MSI_DATA_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x00E8 MPAMF_ERR_MSI_DATA_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x00E8 MPAMF_ERR_MSI_DATA_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.8.6 MPAMF_ERR_MSI_MPAM, MPAM Error MSI Write MPAM Information Register

The MPAMF_ERR_MSI_MPAM characteristics are:

Purpose

MPAMF_ERR_MSI_MPAM is a 32-bit read/write register that sets the MPAM information for error MSI write attributes for MPAM errors in this MSC.

MPAMF_ERR_MSI_MPAM_s controls MPAM information labeling of Secure MPAM error MSI writes. MPAMF_ERR_MSI_MPAM_ns controls MPAM information labeling of Non-secure MPAM error MSI writes. MPAMF_ERR_MSI_MPAM_rt controls MPAM information labeling of Root MPAM error MSI writes. MPAMF_ERR_MSI_MPAM_rl controls MPAM information labeling of Realm MPAM error MSI writes.

Configuration

The power domain of MPAMF_ERR_MSI_MPAM is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_ERR_MSI == ‘1’. Otherwise, direct accesses to MPAMF_ERR_MSI_MPAM are RES0.

Attributes

MPAMF_ERR_MSI_MPAM is a 32-bit register.

Field descriptions

31 24 23 16 15 0

| | | |
|---|---|---|
|RES0|PMG|PARTID|


Bits [31:24]

Reserved, RES0.

PMG, bits [23:16]

Performance monitoring group property for PARTID MSC error interrupt write. The reset behavior of this field is:

• On a MSC reset, this field resets to an architecturally UNKNOWN value.

PARTID, bits [15:0]

Partition ID for MSC error interrupt write. The PARTID in this register is in the Secure PARTID space in the MPAMF_ERR_MSI_MPAM_s instance and in the Non-secure PARTID space in the MPAMF_ERR_MSI_MPAM_ns instance of this register. The reset behavior of this field is:

• On a MSC reset, this field resets to an architecturally UNKNOWN value.

Accessing MPAMF_ERR_MSI_MPAM

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:


- • MPAMF_ERR_MSI_MPAM_s must only be accessible from the Secure MPAM feature page.


- • MPAMF_ERR_MSI_MPAM_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMF_ERR_MSI_MPAM_rt must only be accessible from the Root MPAM feature page.
- • MPAMF_ERR_MSI_MPAM_rl must only be accessible from the Realm MPAM feature page.


- • MPAMF_ERR_MSI_MPAM_s, MPAMF_ERR_MSI_MPAM_ns, MPAMF_ERR_MSI_MPAM_rt, and MPAMF_ERR_MSI_MPAM_rl must be separate registers:
- • The Secure instance (MPAMF_ERR_MSI_MPAM_s) accesses the MPAM information for MSI write request to signal an MPAM error used for Secure PARTIDs.
- • The Non-secure instance (MPAMF_ERR_MSI_MPAM_ns) accesses the MPAM information for MSI write request to signal an MPAM error used for Non-secure PARTIDs.
- • The Root instance (MPAMF_ERR_MSI_MPAM_rt) accesses the MPAM information for MSI write request to signal an MPAM error used for Root PARTIDs.
- • The Realm instance (MPAMF_ERR_MSI_MPAM_rl) accesses the MPAM information for MSI write request to signal an MPAM error used for Realm PARTIDs.


MPAMF_ERR_MSI_MPAM can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x00DC MPAMF_ERR_MSI_MPAM_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x00DC MPAMF_ERR_MSI_MPAM_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x00DC MPAMF_ERR_MSI_MPAM_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x00DC MPAMF_ERR_MSI_MPAM_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.8.7 MPAMF_ESR, MPAM Error Status Register

The MPAMF_ESR characteristics are:

Purpose

Indicates MPAM error status for this MSC. MPAMF_ESR_s reports Secure MPAM errors. MPAMF_ESR_ns reports Non-secure MPAM errors. MPAMF_ESR_rt reports Root MPAM errors. MPAMF_ESR_rl reports Realm MPAM errors. Software should write this register after reading the status of an error to reset ERRCODE to 0x0000 and OVRWR to 0 so that future errors are not reported with OVRWR set.

Configuration

The power domain of MPAMF_ESR is IMPLEMENTATION DEFINED MPAMF_ESR is 64-bit register when MPAM v0.1 or v1.1 is implemented and MPAMF_IDR.HAS_EXTD_ESR

== 1. Otherwise, MPAMF_ESR is a 32-bit register. If an MSC cannot encounter any of the error conditions listed in ‘Errors in MSCs’ in Arm® Architecture Reference Manual Supplement, Memory System Resource Partitioning and Monitoring (MPAM), for Armv8-A (ARM DDI 0598), both the MPAMF_ESR and MPAMF_ECR must be RAZ/WI. The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p0 is implemented. Otherwise, direct accesses to MPAMF_ESR are RES0.

Attributes

MPAMF_ESR is a:

- • 64-bit register when (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_EXTD_ESR == ‘1’.
- • 32-bit register otherwise.


Field descriptions

When (FEAT_MPAMv0p1 is implemented or FEAT_MPAMv1p1 is implemented) and MPAMF_IDR.HAS_EXTD_ESR == '1':

63 36 35 32

| | |
|---|---|
|RES0|RIS|


31 30 28 27 24 23 16 15 0

| | | | | |
|---|---|---|---|---|
| |RES0|ERRCODE|PMG|PARTID_MON|


OVRWR

Bits [63:36]

Reserved, RES0.

RIS, bits [35:32] When MPAMF_IDR.HAS_RIS == '1':

Resource Instance Selector. Where applicable to the ERRCODE, captures the RIS value for the error.

Otherwise:

Reserved, RES0.

OVRWR, bit [31]

Overwritten. If 0 and ERRCODE == 0b0000, no errors have occurred. If 0 and ERRCODE is nonzero, a single error has occurred and is recorded in this register. If 1 and ERRCODE is nonzero, multiple errors have occurred and this register records the most recent error. The state where this bit is 1 and ERRCODE is zero must not be produced by hardware and is only reached when software writes this combination into this register.

Bits [30:28]

Reserved, RES0.

ERRCODE, bits [27:24]

Error code.

ERRCODE Meaning

0b0000 No error. 0b0001 PARTID_SEL_Range. 0b0010 Req_PARTID_Range. 0b0011 MSMONCFG_ID_RANGE. 0b0100 Req_PMG_Range. 0b0101 Monitor_Range. 0b0110 intPARTID_Range. 0b0111 Unexpected_INTERNAL. 0b1000 Undefined_RIS_PART_SEL. 0b1001 RIS_No_Control. 0b1010 Undefined_RIS_MON_SEL. 0b1011 RIS_No_Monitor. 0b1100 Reserved. 0b1101 Reserved. 0b1110 Reserved. 0b1111 Reserved.

PMG, bits [23:16]

Program monitoring group. Set to the PMG on an error that captures PMG. Otherwise, set to 0x00 on an error that does not capture PMG.

PARTID_MON, bits [15:0]

PARTID or monitor. Set to the PARTID on an error that captures PARTID. Set to the monitor index on an error that captures MON. On an error that captures neither PARTID nor MON, this field is set to 0.

###### Otherwise:

31 30 28 27 24 23 16 15 0

| | | | | |
|---|---|---|---|---|
| |RES0|ERRCODE|PMG|PARTID_MON|


OVRWR

OVRWR, bit [31]

Overwritten. If 0 and ERRCODE == 0b0000, no errors have occurred. If 0 and ERRCODE is nonzero, a single error has occurred and is recorded in this register. If 1 and ERRCODE is nonzero, multiple errors have occurred and this register records the most recent error. The state where this bit is 1 and ERRCODE is 0 must not be produced by hardware and is only reached when software writes this combination into this register.

Bits [30:28]

Reserved, RES0.

ERRCODE, bits [27:24]

Error code.

ERRCODE Meaning

0b0000 No error. 0b0001 PARTID_SEL_Range. 0b0010 Req_PARTID_Range. 0b0011 MSMONCFG_ID_RANGE. 0b0100 Req_PMG_Range. 0b0101 Monitor_Range. 0b0110 intPARTID_Range. 0b0111 Unexpected_INTERNAL. 0b1000 Reserved. 0b1001 Reserved. 0b1010 Reserved. 0b1011 Reserved.

- 0b1100 Reserved.

- 0b1101 Reserved.


- 0b1110 Reserved.

- 0b1111 Reserved.


PMG, bits [23:16]

Program monitoring group. Set to the PMG on an error that captures PMG. Otherwise, set to 0x00 on an error that does not capture PMG.

PARTID_MON, bits [15:0]

PARTID or monitor. Set to the PARTID on an error that captures PARTID. Set to the monitor index on an error that captures MON. On an error that captures neither PARTID nor MON, this field is set to 0x0000.

Accessing MPAMF_ESR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMF_ESR_s must only be accessible from the Secure MPAM feature page.
- • MPAMF_ESR_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMF_ESR_rt must only be accessible from the Root MPAM feature page.
- • MPAMF_ESR_rl must only be accessible from the Realm MPAM feature page.


- • MPAMF_ESR_s, MPAMF_ESR_ns, MPAMF_ESR_rt, and MPAMF_ESR_rl must be separate registers:
- • The Secure instance (MPAMF_ESR_s) accesses the error status used for Secure PARTIDs.
- • The Non-secure instance (MPAMF_ESR_ns) accesses the error status used for Non-secure PARTIDs.
- • The Root instance (MPAMF_ESR_rt) accesses the error status used for Root PARTIDs.
- • The Realm instance (MPAMF_ESR_rl) accesses the error status used for Realm PARTIDs.


MPAMF_ESR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x00F8 MPAMF_ESR_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x00F8 MPAMF_ESR_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x00F8 MPAMF_ESR_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x00F8 MPAMF_ESR_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.9 Memory-mapped translation configuration registersThis section lists the external translation configuration registers.

###### 9.9.1 MPAMCFG_IN_TL, MPAM Ingress PARTID Translation Configuration RegisterThe MPAMCFG_IN_TL characteristics are:

Purpose

Enables ingress PARTID translation and configures direct ingress PARTID translations.

Configuration

The power domain of MPAMCFG_IN_TL is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM_MSC_DOMAINS is implemented and MPAMF_IDR.HAS_IN_TL == ‘1’. Otherwise, direct accesses to MPAMCFG_IN_TL are RES0.

Attributes

MPAMCFG_IN_TL is a 32-bit register.

Field descriptions

31 30 16 15 0

| | | |
|---|---|---|
| |RES0|PARTID_TL|


ENABLE

ENABLE, bit [31]

Enables ingress PARTID translation in the MSC.

ENABLE Meaning

0b0 Ingress PARTID translation is disabled. 0b1 Ingress PARTID translation is enabled.

Bits [30:16]

Reserved, RES0.

PARTID_TL, bits [15:0] When MPAMF_IN_TL_IDR.HAS_DIRECT_TL == '1':

PARTID to be used as direct translation of the ingress PARTID configured in MPAMCFG_PART_SEL.PARTID_SEL when MPAMCFG_PART_SEL.INGRESS_TL == 1.

Otherwise:

Reserved, RES0.

Accessing MPAMCFG_IN_TL

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:


- • MPAMCFG_IN_TL_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_IN_TL_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_IN_TL_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_IN_TL_rl must only be accessible from the Realm MPAM feature page.

- • MPAMCFG_IN_TL_s, MPAMCFG_IN_TL_ns, MPAMCFG_IN_TL_rt, and MPAMCFG_IN_TL_rl must be separate registers:


- • The Secure instance (MPAMCFG_IN_TL_s) accesses the ingress PARTID translation used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_IN_TL_ns) accesses the ingress PARTID translation used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_IN_TL_rt) accesses the ingress PARTID translation used for Root PARTIDs.
- • The Realm instance (MPAMCFG_IN_TL_rl) accesses the ingress PARTID translation used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_IN_TL access the ingress PARTID translation configuration settings without being affected by MPAMCFG_PART_SEL.RIS.

MPAMCFG_IN_TL can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x3008 MPAMCFG_IN_TL_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x3008 MPAMCFG_IN_TL_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x3008 MPAMCFG_IN_TL_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x3008 MPAMCFG_IN_TL_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.9.2 MPAMCFG_IN_TL_BASE, MPAM Ingress PARTID Translation Base Configuration RegisterThe MPAMCFG_IN_TL_BASE characteristics are:

Purpose

Configures the base value used to compute translation PARTIDs for ingress PARTIDs that do not have direct translation.

Configuration

The power domain of MPAMCFG_IN_TL_BASE is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM_MSC_DOMAINS is implemented, MPAMF_IDR.HAS_IN_TL

== ‘1’, and MPAMF_IN_TL_IDR.HAS_BASE_MASK == ‘1’. Otherwise, direct accesses to MPAMCFG_IN_TL_BASE are RES0.

Attributes

MPAMCFG_IN_TL_BASE is a 32-bit register.

Field descriptions

31 16 15 0

| | |
|---|---|
|RES0|BASE|


Bits [31:16]

Reserved, RES0.

BASE, bits [15:0]

Base value used to compute the ingress translation of PARTIDs that do not have a direct translation configured.

Accessing MPAMCFG_IN_TL_BASE

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_IN_TL_BASE_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_IN_TL_BASE_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_IN_TL_BASE_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_IN_TL_BASE_rl must only be accessible from the Realm MPAM feature page.


• MPAMCFG_IN_TL_BASE_s, MPAMCFG_IN_TL_BASE_ns, MPAMCFG_IN_TL_BASE_rt, and MPAMCFG_IN_TL_BASE_rl must be separate registers:

- • The Secure instance (MPAMCFG_IN_TL_BASE_s) accesses the ingress PARTID translation base used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_IN_TL_BASE_ns) accesses the ingress PARTID translation base used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_IN_TL_BASE_rt) accesses the ingress PARTID translation base used for Root PARTIDs.
- • The Realm instance (MPAMCFG_IN_TL_BASE_rl) accesses the ingress PARTID translation base used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_IN_TL_BASE access the ingress PARTID translation base configuration settings without being affected by MPAMCFG_PART_SEL.RIS.

MPAMCFG_IN_TL_BASE can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x3010 MPAMCFG_IN_TL_BASE_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x3010 MPAMCFG_IN_TL_BASE_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x3010 MPAMCFG_IN_TL_BASE_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x3010 MPAMCFG_IN_TL_BASE_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.9.3 MPAMCFG_IN_TL_MASK, MPAM Ingress PARTID Translation Mask Configuration RegisterThe MPAMCFG_IN_TL_MASK characteristics are:

Purpose

Configures the mask value used to compute translation PARTIDs for ingress PARTIDs that do not have direct translation.

Configuration

The power domain of MPAMCFG_IN_TL_MASK is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM_MSC_DOMAINS is implemented, MPAMF_IDR.HAS_IN_TL

== ‘1’, and MPAMF_IN_TL_IDR.HAS_BASE_MASK == ‘1’. Otherwise, direct accesses to MPAMCFG_IN_TL_MASK are RES0.

Attributes

MPAMCFG_IN_TL_MASK is a 32-bit register.

Field descriptions

31 5 4 0

| | |
|---|---|
|RES0|MASK_WD|


Bits [31:5]

Reserved, RES0.

MASK_WD, bits [4:0]

Value used to calculate the mask as 2MASK_WD-1. The mask is then used to compute the ingress translation of PARTIDs that do not have a direct translation configured.

Accessing MPAMCFG_IN_TL_MASK

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_IN_TL_MASK_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_IN_TL_MASK_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_IN_TL_MASK_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_IN_TL_MASK_rl must only be accessible from the Realm MPAM feature page.


- • MPAMCFG_IN_TL_MASK_s, MPAMCFG_IN_TL_MASK_ns, MPAMCFG_IN_TL_MASK_rt, and MPAMCFG_IN_TL_MASK_rl must be separate registers:
- • The Secure instance (MPAMCFG_IN_TL_MASK_s) accesses the ingress PARTID translation mask used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_IN_TL_MASK_ns) accesses the ingress PARTID translation mask used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_IN_TL_MASK_rt) accesses the ingress PARTID translation mask used for Root PARTIDs.
- • The Realm instance (MPAMCFG_IN_TL_MASK_rl) accesses the ingress PARTID translation mask used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_IN_TL_MASK access the ingress PARTID translation mask configuration settings without being affected by MPAMCFG_PART_SEL.RIS.

MPAMCFG_IN_TL_MASK can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x3018 MPAMCFG_IN_TL_MASK_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x3018 MPAMCFG_IN_TL_MASK_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x3018 MPAMCFG_IN_TL_MASK_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x3018 MPAMCFG_IN_TL_MASK_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.9.4 MPAMCFG_OUT_TL, MPAM Egress PARTID Translation Configuration RegisterThe MPAMCFG_OUT_TL characteristics are:

Purpose

Enables egress PARTID translation and configures direct egress PARTID translations.

Configuration

The power domain of MPAMCFG_OUT_TL is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM_MSC_DOMAINS is implemented and MPAMF_IDR.HAS_OUT_TL == ‘1’. Otherwise, direct accesses to MPAMCFG_OUT_TL are RES0.

Attributes

MPAMCFG_OUT_TL is a 32-bit register.

Field descriptions

31 30 16 15 0

| | | |
|---|---|---|
| |RES0|PARTID_TL|


ENABLE

ENABLE, bit [31]

Enables egress PARTID translation in the MSC.

ENABLE Meaning

0b0 Egress PARTID translation is disabled. 0b1 Egress PARTID translation is enabled.

Bits [30:16]

Reserved, RES0.

PARTID_TL, bits [15:0] When MPAMF_OUT_TL_IDR.HAS_DIRECT_TL == '1':

PARTID to be used as direct translation of the egress PARTID configured in MPAMCFG_PART_SEL.PARTID_SEL when MPAMCFG_PART_SEL.INGRESS_TL == 0.

Otherwise:

Reserved, RES0.

Accessing MPAMCFG_OUT_TL

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_OUT_TL_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_OUT_TL_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_OUT_TL_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_OUT_TL_rl must only be accessible from the Realm MPAM feature page.


• MPAMCFG_OUT_TL_s, MPAMCFG_OUT_TL_ns, MPAMCFG_OUT_TL_rt, and MPAMCFG_OUT_TL_rl must be separate registers:

- • The Secure instance (MPAMCFG_OUT_TL_s) accesses the egress PARTID translation used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_OUT_TL_ns) accesses the egress PARTID translation used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_OUT_TL_rt) accesses the egress PARTID translation used for Root PARTIDs.
- • The Realm instance (MPAMCFG_OUT_TL_rl) accesses the egress PARTID translation used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_OUT_TL access the egress PARTID translation configuration settings without being affected by MPAMCFG_PART_SEL.RIS. MPAMCFG_OUT_TL can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x3208 MPAMCFG_OUT_TL_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x3208 MPAMCFG_OUT_TL_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x3208 MPAMCFG_OUT_TL_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x3208 MPAMCFG_OUT_TL_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.9.5 MPAMCFG_OUT_TL_BASE, MPAM Egress PARTID Translation Base Configuration RegisterThe MPAMCFG_OUT_TL_BASE characteristics are:

Purpose

Configures the base value used to compute translation PARTIDs for egress PARTIDs that do not have direct translation.

Configuration

The power domain of MPAMCFG_OUT_TL_BASE is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM_MSC_DOMAINS is implemented, MPAMF_IDR.HAS_OUT_TL == ‘1’, and MPAMF_OUT_TL_IDR.HAS_BASE_MASK == ‘1’. Otherwise, direct accesses to MPAMCFG_OUT_TL_BASE are RES0.

Attributes

MPAMCFG_OUT_TL_BASE is a 32-bit register.

Field descriptions

31 16 15 0

| | |
|---|---|
|RES0|BASE|


Bits [31:16]

Reserved, RES0.

BASE, bits [15:0]

Base value used to compute the egress translation of PARTIDs that do not have a direct translation configured.

Accessing MPAMCFG_OUT_TL_BASE

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_OUT_TL_BASE_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_OUT_TL_BASE_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_OUT_TL_BASE_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_OUT_TL_BASE_rl must only be accessible from the Realm MPAM feature page.


• MPAMCFG_OUT_TL_BASE_s, MPAMCFG_OUT_TL_BASE_ns, MPAMCFG_OUT_TL_BASE_rt, and MPAMCFG_OUT_TL_BASE_rl must be separate registers:

- • The Secure instance (MPAMCFG_OUT_TL_BASE_s) accesses the egress PARTID translation base used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_OUT_TL_BASE_ns) accesses the egress PARTID translation base used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_OUT_TL_BASE_rt) accesses the egress PARTID translation base used for Root PARTIDs.
- • The Realm instance (MPAMCFG_OUT_TL_BASE_rl) accesses the egress PARTID translation base used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_OUT_TL_BASE access the egress PARTID translation base configuration settings without being affected by MPAMCFG_PART_SEL.RIS.

MPAMCFG_OUT_TL_BASE can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x3210 MPAMCFG_OUT_TL_BASE_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x3210 MPAMCFG_OUT_TL_BASE_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x3210 MPAMCFG_OUT_TL_BASE_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x3210 MPAMCFG_OUT_TL_BASE_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.9.6 MPAMCFG_OUT_TL_MASK, MPAM Egress PARTID Translation Mask Configuration RegisterThe MPAMCFG_OUT_TL_MASK characteristics are:

Purpose

Configures the mask value used to compute translation PARTIDs for egress PARTIDs that do not have direct translation.

Configuration

The power domain of MPAMCFG_OUT_TL_MASK is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM_MSC_DOMAINS is implemented, MPAMF_IDR.HAS_OUT_TL == ‘1’, and MPAMF_OUT_TL_IDR.HAS_BASE_MASK == ‘1’. Otherwise, direct accesses to MPAMCFG_OUT_TL_MASK are RES0.

Attributes

MPAMCFG_OUT_TL_MASK is a 32-bit register.

Field descriptions

31 5 4 0

| | |
|---|---|
|RES0|MASK_WD|


Bits [31:5]

Reserved, RES0.

MASK_WD, bits [4:0]

Value used to calculate the mask as 2MASK_WD-1. The mask is then used to compute the egress translation of PARTIDs that do not have a direct translation configured.

Accessing MPAMCFG_OUT_TL_MASK

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

• In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps:

- • MPAMCFG_OUT_TL_MASK_s must only be accessible from the Secure MPAM feature page.
- • MPAMCFG_OUT_TL_MASK_ns must only be accessible from the Non-secure MPAM feature page.
- • MPAMCFG_OUT_TL_MASK_rt must only be accessible from the Root MPAM feature page.
- • MPAMCFG_OUT_TL_MASK_rl must only be accessible from the Realm MPAM feature page.


• MPAMCFG_OUT_TL_MASK_s, MPAMCFG_OUT_TL_MASK_ns, MPAMCFG_OUT_TL_MASK_rt, and MPAMCFG_OUT_TL_MASK_rl must be separate registers:

- • The Secure instance (MPAMCFG_OUT_TL_MASK_s) accesses the egress PARTID translation mask used for Secure PARTIDs.
- • The Non-secure instance (MPAMCFG_OUT_TL_MASK_ns) accesses the egress PARTID translation mask used for Non-secure PARTIDs.
- • The Root instance (MPAMCFG_OUT_TL_MASK_rt) accesses the egress PARTID translation mask used for Root PARTIDs.
- • The Realm instance (MPAMCFG_OUT_TL_MASK_rl) accesses the egress PARTID translation mask used for Realm PARTIDs.


When RIS is implemented, loads and stores to MPAMCFG_OUT_TL_MASK access the egress PARTID translation mask configuration settings without being affected by MPAMCFG_PART_SEL.RIS.

MPAMCFG_OUT_TL_MASK can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x3218 MPAMCFG_OUT_TL_MASK_s

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x3218 MPAMCFG_OUT_TL_MASK_ns

Accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x3218 MPAMCFG_OUT_TL_MASK_rt

When FEAT_RME is implemented, accesses to this register are RW.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x3218 MPAMCFG_OUT_TL_MASK_rl

When FEAT_RME is implemented, accesses to this register are RW.

###### 9.9.7 MPAMF_IN_TL_IDR, MPAM Ingress PARTID Translation ID RegisterThe MPAMF_IN_TL_IDR characteristics are:

Purpose

Indicates the ingress PARTID translation capabilities of the MSC.

Configuration

The power domain of MPAMF_IN_TL_IDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM_MSC_DOMAINS is implemented and MPAMF_IDR.HAS_IN_TL == ‘1’. Otherwise, direct accesses to MPAMF_IN_TL_IDR are RES0.

Attributes

MPAMF_IN_TL_IDR is a 32-bit register.

Field descriptions

31 30 29 16 15 0

| | | | |
|---|---|---|---|
| | |RES0|IN_PARTID_MAX|


HAS_DIR ECT_TL

HAS_BASE_MASK

HAS_DIRECT_TL, bit [31]

Indicates support for direct ingress translation of PARTIDs. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_DIRECT_TL Meaning

0b0 Direct ingress translation of PARTIDs is not supported. 0b1 Direct ingress translation of PARTIDs is supported for those

PARTIDs with an explicitly set translation configuration.

Access to this field is RO.

HAS_BASE_MASK, bit [30]

Indicates support for computed ingress translation of PARTIDs using a configurable mask and base. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_BASE_MASK Meaning

- 0b0 Computed ingress translation of PARTIDs using a configurable mask and base is not supported.


HAS_BASE_MASK Meaning

- 0b1 Computed ingress translation of PARTIDs using a configurable mask and base is supported for those PARTIDs without an explicitly set translation configuration.


Access to this field is RO.

Bits [29:16]

Reserved, RES0.

IN_PARTID_MAX, bits [15:0] When MPAMF_IN_TL_IDR.HAS_DIRECT_TL == '1':

Maximum value for PARTIDs to be used as direct ingress translations, as configured in MPAMCFG_IN_TL.PARTID_TL.

This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

Otherwise:

Reserved, RES0.

Accessing MPAMF_IN_TL_IDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_IN_TL_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_IN_TL_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:

- • MPAMF_IN_TL_IDR_s is permitted to have either the same or different contents to MPAMF_IN_TL_IDR_ns, MPAMF_IN_TL_IDR_rt, or MPAMF_IN_TL_IDR_rl.
- • MPAMF_IN_TL_IDR_ns is permitted to have either the same or different contents to MPAMF_IN_TL_IDR_rt or MPAMF_IN_TL_IDR_rl.
- • MPAMF_IN_TL_IDR_rt is permitted to have either the same or different contents to MPAMF_IN_TL_IDR_rl.


- • There must be separate registers in the Secure (MPAMF_IN_TL_IDR_s), Non-secure (MPAMF_IN_TL_IDR_ns), Root (MPAMF_IN_TL_IDR_rt), and Realm (MPAMF_IN_TL_IDR_rl) MPAM feature pages.


MPAMF_IN_TL_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x3000 MPAMF_IN_TL_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x3000 MPAMF_IN_TL_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x3000 MPAMF_IN_TL_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x3000 MPAMF_IN_TL_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

###### 9.9.8 MPAMF_OUT_TL_IDR, MPAM Egress PARTID Translation ID RegisterThe MPAMF_OUT_TL_IDR characteristics are:

Purpose

Indicates the egress PARTID translation capabilities of the MSC.

Configuration

The power domain of MPAMF_OUT_TL_IDR is IMPLEMENTATION DEFINED The power and reset domain of each MSC component is specific to that component. This register is present only when FEAT_MPAM_MSC_DOMAINS is implemented and MPAMF_IDR.HAS_OUT_TL == ‘1’. Otherwise, direct accesses to MPAMF_OUT_TL_IDR are RES0.

Attributes

MPAMF_OUT_TL_IDR is a 32-bit register.

Field descriptions

31 30 29 16 15 0

| | | | |
|---|---|---|---|
| | |RES0|OUT_PARTID_MAX|


HAS_DIR ECT_TL

HAS_BASE_MASK

HAS_DIRECT_TL, bit [31]

Indicates support for direct egress translation of PARTIDs. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_DIRECT_TL Meaning

0b0 Direct egress translation of PARTIDs is not supported. 0b1 Direct egress translation of PARTIDs is supported for those

PARTIDs with an explicitly set translation configuration.

Access to this field is RO.

HAS_BASE_MASK, bit [30]

Indicates support for computed egress translation of PARTIDs using a configurable mask and base. The value of this field is an IMPLEMENTATION DEFINED choice of:

HAS_BASE_MASK Meaning

0b0 Computed egress translation of PARTIDs using a configurable mask and base is not supported.

HAS_BASE_MASK Meaning

0b1 Computed egress translation of PARTIDs using a configurable mask and base is supported for those PARTIDs without an explicitly set translation configuration.

Access to this field is RO.

Bits [29:16]

Reserved, RES0.

OUT_PARTID_MAX, bits [15:0] When MPAMF_OUT_TL_IDR.HAS_DIRECT_TL == '1':

Maximum value for PARTIDs to be used as direct egress translations, as configured in MPAMCFG_OUT_TL.PARTID_TL.

This field has an IMPLEMENTATION DEFINED value. Access to this field is RO.

Otherwise:

Reserved, RES0.

Accessing MPAMF_OUT_TL_IDR

If both FEAT_MPAM and FEAT_RME are implemented, the following statements apply:

- • In a system that supports Secure, Non-secure, Root, and Realm memory maps, there must be MPAM feature pages in all four address maps.
- • MPAMF_OUT_TL_IDR must be readable from the Non-secure, Secure, Root, and Realm MPAM feature pages.
- • MPAMF_OUT_TL_IDR is permitted to have the same contents when read from the Secure, Non-secure, Root, and Realm MPAM feature pages unless the register contents are different for the different versions:
- • MPAMF_OUT_TL_IDR_s is permitted to have either the same or different contents to MPAMF_OUT_TL_IDR_ns, MPAMF_OUT_TL_IDR_rt, or MPAMF_OUT_TL_IDR_rl.
- • MPAMF_OUT_TL_IDR_ns is permitted to have either the same or different contents to MPAMF_OUT_TL_IDR_rt or MPAMF_OUT_TL_IDR_rl.
- • MPAMF_OUT_TL_IDR_rt is permitted to have either the same or different contents to MPAMF_OUT_TL_IDR_rl.


• There must be separate registers in the Secure (MPAMF_OUT_TL_IDR_s), Non-secure (MPAMF_OUT_TL_IDR_ns), Root (MPAMF_OUT_TL_IDR_rt), and Realm (MPAMF_OUT_TL_IDR_rl) MPAM feature pages.

MPAMF_OUT_TL_IDR can be accessed through the memory-mapped interface:

Component Frame Offset Instance

MPAM MPAMF_BASE_s 0x3200 MPAMF_OUT_TL_IDR_s

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_ns 0x3200 MPAMF_OUT_TL_IDR_ns

Accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rt 0x3200 MPAMF_OUT_TL_IDR_rt

When FEAT_RME is implemented, accesses to this register are RO.

Component Frame Offset Instance

MPAM MPAMF_BASE_rl 0x3200 MPAMF_OUT_TL_IDR_rl

When FEAT_RME is implemented, accesses to this register are RO.

## Part A Appendixes

### Appendix A1Generic Resource Controls

This chapter contains the following sections:

- • Portion resource controls.
- • Maximum-usage resource controls.
- • Proportional resource allocation facilities.
- • Combining resource controls.


###### A1.1 Portion resource controlsA1.1 Portion resource controls

IBNMSM Some resources can be divided into portions that can be allocated exclusively to a partition (private) or to two or more partitions (shared).

IHDVYW Resource portions are allocated to a partition using a portion bit map (PBM).

RYCBKB A PBM is an MMR containing a vector of single-bit elements with all of the following properties:

- • The PBM bit width (PBM_WD) can be up to 215 bits.
- • The PBM_WD is a multiple of 32 bits.
- • Element n is PBM[n] at the address (MPAMF_BASE + PBM_offset), where PBM_offset is the offset of the particular PBM register.


###### Note

To access the PBM[n], software accesses the appropriate 32-bit PBM register word at the address MPAMF_BASE + PBM_offset + ((n 3) & 3̃), and then accesses the bit (n & 31).

RTVYHS If the 32-bit word containing the highest byte of the PBM (MPAMF_BASE + PBM_offset + (PBM_WD 3)) has

unused bits, those bits are RES0.

IBKCQH Figure A1-1 shows an example of a PBM. In this example, if the value of PBM[23] is 1, then resource portion 23 of 32 is allocated to the partition. If the value of PBM[23] is 0, then that resource is not allocated to the partition.

63

31 23 0

| | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|


0: May not allocate portion 23 of 32 1: May allocate portion 23 of 32

###### Figure A1-1 Portion bit map.

IVTMPP Figure A1-2 shows how the PBM can be used to allocate resources between private and shared partitions.

- Partition 1 (PBM = 0b0011)
- Partition 2 (PBM = 0b0110)


Portion Allocation

Portion 0 Portion 1 Portion 2 Portion 3

| | | | |
|---|---|---|---|
| | | | |
| | | | |
| | | | |
| | | | |
|Exclusively PARTID = 1|Shared by both|Exclusively PARTID = 2|Unallocated|


- Uses portions 0 and 1
- Uses portions 1 and 2


###### Figure A1-2 Portion shared and exclusive allocations.

A1.2 Maximum-usage resource controls

###### A1.2 Maximum-usage resource controls

ITZGCP For a shared resource, a Maximum-usage resource control sets a maximum fraction of the resource capacity that a partition is permitted to use. When a partition is not using its maximum permitted fraction, more of the resource can be allocated to that partition. When a partition is using its maximum permitted fraction, one or more of the following apply:

- • More allocation to that partition is prevented.
- • Allocation to that partition is deferred.
- • Allocation priority for that partition is lowered.
- • A prior allocation to that partition is reclaimed.
- • A prior allocation to that partition is replaced.


ICPYYR Software writes a maximum fraction to a maximum-usage resource control as a 16-bit unsigned fixed-point value. See The fixed-point fractional format.

RRJFKG The effective fraction value is the programmed value plus one lsb. For example:

1) Software is using 16 fraction bits to target 50%. The closest hexadecimal value to 50% is 0x8000. Software offsets

0x8000 by one lsb and writes 0x7FFF, and: 2) If the fractional control field is implemented as:

- • 16-bits, the programmed value is 0x7FFF. The effective value is 0x8000.
- • 12-bits, the programmed value is 0x7FF. The effective value is 0x800.
- • 8-bits, the programmed value is 0x7F. The effective value is 0x80.


RFKNKB An effective fraction value of:

- • 100% is enforced by programming the fractional control field with all ones.
- • 0% is enforced by disabling the PARTID by writing the PARTID value to MPAMCFG_DIS.PARTID. See Disabling a PARTID.


###### A1.3 Proportional resource allocation facilities

IRCCNS MPAM proportional stride partitioning is related to two software resource management interfaces:

- • The Linux cgroup weights interface uses integer weights to allocate a fraction of a resource to a process.
- • The VMware® shares interface uses an integer share to allocate a fraction of a resource to a virtual machine.


IPQGXF For partition, p, the weight or share is used to compute the resource fraction, f, as follows:

Weight Σ Weight

p

f(p) =

|w|
|---|


all w

IGYWFZ The partition stride is a scaled reciprocal of the weight, as follows:

S f(p)

Stride of p =

IGFZGN To normalize stride values and set the smallest system stride to 1, software should choose a scaling factor, S, equal to the

largest f(p). Software should scale all strides by the same S.

IMKXZZ Stride-based proportional allocation is useful for temporal or rate-of-occurrence resources, such as bandwidth.

IBCJJN If a value STRIDEM1 is used by software to do stride-based proportional allocation, all of the following apply:

- • STRIDEM1 is a positive unsigned integer with an IMPLEMENTATION DEFINED field width of w.
- • STRIDEM1 has the bit range [0 . . . 2w-1].
- • STRIDEM1 represents stride values in the range [1 . . . 2w].


IYPFQC If, after normalization, the stride value is greater than 2w, software should program STRIDEM1 using the largest representable value, 2w - 1.

INTJKQ All of the following apply to proportional resource allocation:

- • Resource proportion allocation shrinks and grows as the number of partitions fluctuate.
- • Allocation is subdividable. For example, if a virtual machine is allocated half of a whole resource, and a child application of that virtual machine is allocated 2/3 of the VM resource allocation, then that child application is allocated 1/2 x 2/3 = 1/3 of the whole resource.
- • Proportional resource allocation only needs to consider current resource contenders for a temporal resource, such as memory bandwidth.
- • Proportional resource allocation is work-conserving if it does not idle a resource when only low-proportion requests exist, but instead uses as much resource as is requested. Proportional resource allocation can allocate the resource to those lower-proportion requests, in proportion to their relative weights.


###### A1.3.1 Model of stride-based memory bandwidth scheduling

IMHDQH In the stride-based memory bandwidth scheduling model, each partition has an offset(p) that tracks the time since the partition, p, consumed bandwidth and is bound to an upper limit offset_limit. When a request, r, arrives it is given a deadline equal to the current_time plus stride(p) minus offset(p). The offset(p) is set to current_time minus deadline, and the offset(p) is incremented in event-time units until it reaches offset_limit.

IWHLVJ In the stride-based memory bandwidth scheduling model, requests are serviced in increasing deadline order. New requests with small strides (highest access to bandwidth) may be scheduled ahead of requests with large strides.

###### IXKZQD If there are unprocessed requests, the stride-based memory bandwidth scheduling model does not prevent servicing a request with a later deadline if there are no requests waiting with earlier deadlines. This is considered work-conserving.

###### A1.4 Combining resource controlsA1.4 Combining resource controls

INWGDZ A resource can be controlled using multiple methods, and the result of combining those controls should produce a combined effect. For example, when allocating a resource using both portion control and maximum-usage control, allocation should satisfy both controls.

IZGWVY All resource controls should have at least one setting that does not limit resource access.

IVKPWB When an implementation contains multiple controls for the same resource, the limits imposed by each control on partition use are all applied. By selecting controls that limit partition use and controls that do not, software can dynamically adapt regulation controls in a single system.

IXNMQH If the effective maximum limit for a partition is lower than its effective minimum limit, the control behaviour is

CONSTRAINED UNPREDICTABLE.

### Appendix A2MSC Firmware Data

This chapter contains the following sections:

- • Introduction.
- • Partitioning-control parameters.
- • Performance-monitoring parameters.
- • Discovery of resource to RIS mapping.
- • Discovery of wired interrupts.


###### A2.1 Introduction

IJZCXC In a system implementing MPAM, firmware data is used by software to discover the memory system topology and implemented MPAM controls and monitors. The interface used by software to access the firmware data is beyond the scope of this document.

###### Note

Examples of firmware data interfaces include:

- • ACPI.
- • Device Tree.


IWMBFB For static devices in an implementation, discovery of firmware data can be done by reading pre-configured information from firmware storage, dynamically discovered using device probing and testing, or some combination of both approaches.

###### A2.1.1 Partitioning-control parameters

IJVBPL Table A2-1 shows the partitioning control parameters discoverable in firmware. For each supported address space, every MPAM MSC has the MPAMF_IDR MMR at offset 0 from the corresponding MPAMF_BASE address. Other MPAM memory-mapped registers are at fixed offsets from that address. See Memory-mapped Registers.

Table A2-1 Partitioning control parameters

Control Parameter Data Format Description

MPAM MPAMF_BASE_ns Address MPAMF_BASE address register for the Non-secure address space. MPAM MPAMF_BASE_s Address MPAMF_BASE address register for the Secure address space. MPAM MPAMF_BASE_rl Address MPAMF_BASE address register for the Realm address space. MPAM MPAMF_BASE_rt Address MPAMF_BASE address register for the Root address space.

- A2.1.2 Performance-monitoring parameters IXXLKJ Table A2-2 shows the performance monitoring parameters discoverable in firmware.

Table A2-2 Performance-monitoring parameters

Monitor Parameter Data Format Description

CSU MAX_NRDY_USEC Uint32 Maximum number of microseconds that the NRDY signal can remain 1 in the absence of monitor reconfiguration or writes to the MSMON_CSU register. MSMON_CSU.VALUE must be accurate and MSMON_CSU.NRDY set to zero before MAX_NRDY_USEC microseconds have elapsed since the monitor was configured, reconfigured, or written.

- A2.1.3 Discovery of resource to RIS mapping


IJTGKX The RIS value used to control an MSC resource instance is not provided by MPAM MMR ID registers. The method used to discover the mapping of a RIS value to an MSC resource instance is IMPLEMENTATION DEFINED, such as a firmware data table or some other means.

###### A2.1.4 Discovery of wired interrupts

IVYPCC Information on the connection and grouping of MPAM MSC interrupt sources to GIC inputs is not provided by the MPAM MSC ID registers. Software must discover this information firmware.

### Appendix A3Examples of partial MPAM implementations

This chapter contains the following sections:

• Examples.

Examples of partial MPAM implementations

###### A3.1 ExamplesA3.1 Examples

IGKZSY The following are examples of partial MPAM implementations.

###### A3.1.1 An MSC that has no MPAM partitioning or monitoring, only MPAM propagation

IGSHTX All of the following apply to an MSC that propagates MPAM information but does not implement MPAM partitioning or monitoring:

- • The minimum required MMRs must be implemented, and the MPAMF_IDR.{PARTID_MAX, PMG_MAX} fields must indicate the maximum PARTID that can be propagated. See Minimum required memory-mapped registers.
- • All HAS_* and NO_* bits in MPAMF_IDR must be zero.
- • MPAMF_AIDR must indicate MPAM v1.0 or MPAM v1.1.
- • MPAMF_IIDR must identify the implementation.
- • MPAMF_SIDR.{PARTID_MAX, PMG_MAX} fields must indicate the maximum Secure PARTID that can be propagated.


No other registers are required.

###### A3.1.2 An MSC when hardware configuration removes a partitioning control or resource usage monitor

IBQMBS If an MSC has a hardware configuration option that removes partitioning controls or resource usage monitors, then when

the feature is removed, the HAS_* bits in the corresponding MPAMF_*IDR registers must be RES0.

###### A3.1.3 An MSC when hardware configuration has removed all MPAM functionality

ISCTFX If an MSC has a hardware configuration option that removes all MPAM functionality, then all of the following apply:

- • The minimum required MPAM registers must be present. See Minimum required memory-mapped registers.
- • MPAMF_IDR, MPAMF_AIDR, and MPAMF_SIDR must be RAZ/WI.
- • MPAMF_IIDR is permitted to either identify the implementation information or be RAZ/WI.


Note

Because software might attempt to discover MPAM on the hardware configuration, the minimum MPAM registers must be present to allow the lack of MPAM function to be discovered.

###### A3.1.4 An MSC when hardware configuration removes a resource instance

IRJHQG If an MSC has a hardware configuration option that removes a resource instance, then when the resource is removed the MPAMF_*IDR registers corresponding to that RIS value have each of the RIS-specific fields set to zero. For more information on RIS-specific fields, see Effects of MPAMCFG_PART_SEL.RIS on values read from other registers.

### Glossary

This glossary describes some of the terms that are used in this document. Some of these terms are unique to MPAM and are introduced in this document while others are standard terms that can be found in the Glossary of the Arm® Architecture Reference Manual for A-profile architecture.

Abort An exception caused by an illegal memory access. Aborts can be caused by the external memory

system or the MMU.

Aligned A data item stored at an address that is exactly divisible by the highest power of 2 that divides exactly into its size in bytes. Aligned halfwords, words and doublewords therefore have addresses that are divisible by 2, 4, and 8, respectively.

ALTSP Alternative PARTID space. AMBA Advanced Microcontroller Bus Architecture. The AMBA family of protocol specifications is the Arm

open standard for on-chip buses. AMBA provides solutions for the interconnection and management of the functional blocks that make up a System-on-Chip (SoC). Applications include the development of embedded systems with one or more processors or signal processors and multiple peripherals.

Banked register A register that has multiple instances, with the instance that is in use depending on the PE mode,

Security state, or other PE state.

Burst A group of transfers that form a single transaction. With AMBA protocols, only the first transfer of the burst includes address information, and the transfer type determines the addresses used for subsequent transfers.

BWA BandWidth Allocation. BWPBM BandWidth Portion Bit Map. CONSTRAINED UNPREDICTABLE

Where an instruction can result in UNPREDICTABLE behavior, the Armv8 architecture specifies a narrow range of permitted behaviors. This range is the range of CONSTRAINED UNPREDICTABLE behavior. All implementations that are compliant with the architecture must follow the CONSTRAINED UNPREDICTABLE behavior.

Execution at Non-secure EL1 or EL0 of an instruction that is CONSTRAINED UNPREDICTABLE can be implemented as generating a trap exception that is taken to EL2, provided that at least one instruction that is not UNPREDICTABLE and is not CONSTRAINED UNPREDICTABLE causes a trap exception that is taken to EL2.

In body text, the term CONSTRAINED UNPREDICTABLE is shown in SMALL CAPITALS. See also UNPREDICTABLE.

Core See Processing element (PE). CPBM Cache-Portion Bit Map. CSU Cache-Storage Usage. Downstream Information propagating in the direction from Requesters towards terminating Completer components. DSB Data Synchronization Barrier. E2H EL2 Host. A bit field in the HCR_EL2 register. This configuration executes a type-2 hypervisor and its

host operating system in EL2 rather than EL1, for better performance. Type-2 hypervisors run on a host operating system rather then running as a small, standalone OS-like program. For example, kvm is a type-2 hypervisor.

HCR An abbreviated reference to the Hypervisor Configuration Registers in AArch64 HCR_EL2 and in

AArch32 HCR and HCR2. ICN InterConnect Network. ID An identifier or label. Intermediate physical address (IPA)

An implementation of virtualization, the address to which a Guest OS maps a VA. A hypervisor might then map the IPA to a PA. Typically, the Guest OS is unaware of the translation from IPA to PA.

See also Physical address (PA), Virtual address (VA). IPA See Intermediate physical address (IPA). kvm Kernel-based Virtual Machine, an open-source software package that implements a type-2 hypervisor

within Linux. LPI Locality-specific Peripheral Interrupt. MBWU Memory BandWidth Usage. Memory-system component

MSC. A function, unit, or design block in a memory system that can have partitionable resources. MSCs consist of all units that handle load or store requests issued by any MPAM Requester. These include cache memories, interconnects, memory management units, memory channel controllers, queues, buffers, rate adaptors, etc. An MSC may contain one or more resources that each may have zero or more resource partitioning controls. For example, a PE may contain several caches, each of which might have zero or more resource partitioning controls.

Memory-system resource

A resource that affects the performance of software's use of the memory system and is either local to an MSC (such as cache-memory capacity) or non-local (such as memory bandwidth, which is present over an entire path, from Requester to Completer, that may pass through multiple MSCs).

MMR Memory-mapped Register. MPAM Memory system resource Partitioning and Monitoring. MPAM information

The MPAM information bundle, comprising PARTID, PMG, and MPAM_NS.

MPAM_NS MPAM security-space bit. It is not stored in a PE register; it comes from the current Security state of a PE and is communicated to MSCs as part of the MPAM information bundle. In non-PE Requesters, the Security state can be determined in other ways.

MPAM resource partition

See Resource partition. MPAM_SP In MPAM for RME the MPAM PARTID space indication. MSC Memory-system Component. See Memory-system component. MSI Message signaled interrupts. Signaled using a memory write that is usually directed at an interrupt

translation service.

NRDY Not-Ready bit. MPAM resource monitors set this bit to indicate that the monitor register does not

currently have an accurate value. NS Non-Secure. A bit indicating that an address space is not Secure. PA See Physical address (PA). PARTID The partition number component of an MPAM resource partition ID. See Resource partition Partition A division of resources. A partition is manifest in a PARTID and MPAM_NS. In an MSC, the PARTID

and MPAM_NS select partitioning control settings that affect the partitioning by regulating the allocation of the resource to requests using that PARTID and MPAM_NS.

PE See Processing element (PE). Physical address (PA)

An address that identifies a location in the physical memory map. See also Intermediate physical address (IPA), Virtual address (VA).

Physical PARTID A partition ID that is transmitted with memory requests and can be used by MSCs to control resources usage. A physical PARTID is in either the Non-secure or Secure PARTID space. If MPAM for RME is implemented, there are two additional PARTID spaces, Realm PARTID space and Root PARTID space.

PMG Performance Monitoring Group, a property of a partition used in MSCs by MPAM performance

monitors that can be programmed to be sensitive to the particular PARTID and PMG combination.

Portion A uniquely identifiable part of the resource. It is of fixed size or capacity. A particular resource has a constant number of portions. Portions are distinct. Portion n is the same part of the resource for every partition. Thus, every partition that is given access to a portion n shares access to portion n.

PPI Private Peripheral Interrupt. Processing element (PE)

The abstract machine defined in the Arm architecture, as documented in an Arm Architecture Reference Manual. A PE implementation compliant with the Arm architecture must conform with the behaviors described in the corresponding Arm Architecture Reference Manual.

RAZ See Read-As-Zero (RAZ). RAZ/WI Read-As-Zero, Writes Ignored.

Hardware must implement the field as Read-as-Zero, and must ignore writes to the field. Software can rely on the field reading as all 0s, and on writes being ignored. This description can apply to a single bit that reads as 0, or to a field that reads as all 0s. See also Read-As-Zero (RAZ).

Read-As-Zero (RAZ)

Hardware must implement the field as reading as all 0s. Software:

• Can rely on the field reading as all 0s.

• Must use a SBZP policy to write to the field. This description can apply to a single bit that reads as 0, or to a field that reads as all 0s. See also RAZ/WI, RES0.

- RES0 A reserved bit. Used for fields in register descriptions, and for fields in architecturally-defined data structures that are held in memory, for example in translation table descriptors. Within the architecture, there are some cases where a register bit or field:


- • Is RES0 in some defined architectural context.
- • Has different defined behavior in a different architectural context. Note
- • RES0 is not used in descriptions of instruction encodings.
- • Where an AArch32 System register is Architecturally mapped to an AArch64 System register, and a bit or field in that register is RES0 in one Execution state and has defined behavior in the other Execution state, this is an example of a bit or field with behavior that depends on the architectural context.


This means the definition of RES0 for fields in read/write registers is:

If a bit is RES0 in all contexts

For a bit in a read/write register, it is IMPLEMENTATION DEFINED whether:

1. The bit is hardwired to 0. In this case:

- • Reads of the bit always return 0.
- • Writes to the bit are ignored.


2. The bit can be written. In this case:

- • An indirect write to the register sets the bit to 0.
- • A read of the bit returns the last value successfully written, by either a direct or an indirect write, to the bit.

If the bit has not been successfully written since reset, then the read of the bit returns the reset value if there is one, or otherwise returns an UNKNOWN value.

- • A direct write to the bit must update a storage location associated with the bit.
- • The value of the bit must have no effect on the operation of the PE, other than determining the value read back from the bit, unless this Manual explicitly defines additional properties for the bit.


Whether RES0 bits or fields follow behavior 1 or behavior 2 is IMPLEMENTATION DEFINED on a field-by-field basis.

If a bit is RES0 only in some contexts

For a bit in a read/write register, when the bit is described as RES0:

- • An indirect write to the register sets the bit to 0.
- • A read of the bit must return the value last successfully written to the bit, by either a direct or an indirect write, regardless of the use of the register when the bit was written.


If the bit has not been successfully written since reset, then the read of the bit returns the reset value if there is one, or otherwise returns an UNKNOWN value.

- • A direct write to the bit must update a storage location associated with the bit.
- • While the use of the register is such that the bit is described as RES0, the value of the bit must have no effect on the operation of the PE, other than determining the value read back from that bit, unless this Manual explicitly defines additional properties for the bit.


Considering only contexts that apply to a particular implementation, if there is a context in which a bit is defined as RES0, another context in which the same bit is defined as RES1, and no context in which the bit is defined as a functional bit, then it is IMPLEMENTATION DEFINED whether:

• Writes to the bit are ignored, and reads of the bit return an UNKNOWN value. • The value of the bit can be written, and a read returns the last value written to

the bit.

The RES0 description can apply to bits or fields that are read-only, or are write-only:

- • For a read-only bit, RES0 indicates that the bit reads as 0, but software must treat the bit as UNKNOWN.
- • For a write-only bit, RES0 indicates that software must treat the bit as SBZ.

A bit that is RES0 in a context is reserved for possible future use in that context. To preserve forward compatibility, software:

- • Must not rely on the bit reading as 0.
- • Must use an SBZP policy to write to the bit.


This RES0 description can apply to a single bit, or to a field for which each bit of the field must be treated as RES0.

In body text, the term RES0 is shown in SMALL CAPITALS. See also Read-As-Zero (RAZ), RES1, UNKNOWN.

- RES1 A reserved bit. Used for fields in register descriptions, and for fields in architecturally-defined data structures that are held in memory, for example in translation table descriptors. Within the architecture, there are some cases where a register bit or field:


- • Is RES1 in some defined architectural context.
- • Has different defined behavior in a different architectural context. Note
- • RES1 is not used in descriptions of instruction encodings.
- • Where an AArch32 System register is Architecturally mapped to an AArch64 System register, and a bit or field in that register is RES1 in one Execution state and has defined behavior in the other Execution state, this is an example of a bit or field with behavior that depends on the architectural context.


This means the definition of RES1 for fields in read/write registers is:

If a bit is RES1 in all contexts

For a bit in a read/write register, it is IMPLEMENTATION DEFINED whether:

1. The bit is hardwired to 1. In this case:

- • Reads of the bit always return 1.
- • Writes to the bit are ignored.


2. The bit can be written. In this case:

- • An indirect write to the register sets the bit to 1.


- • A read of the bit returns the last value successfully written, by either a direct or an indirect write, to the bit.

If the bit has not been successfully written since reset, then the read of the bit returns the reset value if there is one, or otherwise returns an UNKNOWN value.

- • A direct write to the bit must update a storage location associated with the bit.
- • The value of the bit must have no effect on the operation of the PE, other than determining the value read back from the bit, unless this Manual explicitly defines additional properties for the bit.


Whether RES1 bits or fields follow behavior 1 or behavior 2 is IMPLEMENTATION DEFINED on a field-by-field basis.

If a bit is RES1 only in some contexts

For a bit in a read/write register, when the bit is described as RES1:

- • An indirect write to the register sets the bit to 1.
- • A read of the bit must return the value last successfully written to the bit, regardless of the use of the register when the bit was written.

Note

As indicated in this list, this value might be written by an indirect write to the register.

If the bit has not been successfully written since reset, then the read of the bit returns the reset value if there is one, or otherwise returns an UNKNOWN value.

- • A direct write to the bit must update a storage location associated with the bit.
- • While the use of the register is such that the bit is described as RES1, the value of the bit must have no effect on the operation of the PE, other than determining the value read back from that bit, unless this Manual explicitly defines additional properties for the bit.


Considering only contexts that apply to a particular implementation, if there is a context in which a bit is defined as RES0, another context in which the same bit is defined as RES1, and no context in which the bit is defined as a functional bit, then it is IMPLEMENTATION DEFINED whether:

• Writes to the bit are ignored, and reads of the bit return an UNKNOWN value. • The value of the bit can be written, and a read returns the last value written to

the bit.

The RES1 description can apply to bits or fields that are read-only, or are write-only:

- • For a read-only bit, RES1 indicates that the bit reads as 1, but software must treat the bit as UNKNOWN.
- • For a write-only bit, RES1 indicates that software must treat the bit as SBO.

A bit that is RES1 in a context is reserved for possible future use in that context. To preserve forward compatibility, software:

- • Must not rely on the bit reading as 1.
- • Must use an SBOP policy to write to the bit.


This RES1 description can apply to a single bit, or to a field for which each bit of the field must be treated as RES1.

In body text, the term RES1 is shown in SMALL CAPITALS.

See also RES0, UNKNOWN. Reserved Unless otherwise stated:

- • Instructions that are reserved or that access reserved registers have UNPREDICTABLE or CONSTRAINED UNPREDICTABLE behavior.
- • Bit positions described as reserved are:


— In an RW or WO register, RES0.

— In an RO register, UNK.

Resource partition

The collection of MPAM resource control settings associated with a software environment and identified by the combination of a physical PARTID space and a partition number.

RIS Resource instance selection. The value in MPAMCFG_PART_SEL.RIS selects the resource instance that is configured through MPAMCFG_* registers and described by the MPAMF ID registers. See RIS values.

RME Realm Management Extension. RME specifies how PE execution context is mapped to Security states. SCR An abbreviated reference to the Secure Configuration Registers such as SCTLR_ELx or AArch32

SCTLR. SMMU System Memory Management Unit. SPE Statistical Profiling Extension. SPI Shared Peripheral Interrupt. TGE Trap General Exception. A field in the HCR_EL2 register. It causes EL0 exceptions, that would

normally trap to EL1, to instead trap to EL2. This function can be used to run an EL2 host’s applications at EL0, so that any exceptions in the application trap to the host OS at EL2.

UNDEFINED Indicates cases where an attempt to execute a particular encoding bit pattern generates an exception, that is taken to the current Exception level, or to the default Exception level for taking exceptions if the UNDEFINED encoding was executed at EL0. This applies to:

- • Any encoding that is not allocated to any instruction.
- • Any encoding that is defined as never accessible at the current Exception level.
- • Some cases where an enable, disable, or trap control means an encoding is not accessible at the current Exception level.


If the generated exception is taken to an Exception level that is using AArch32 then it is taken as an Undefined Instruction exception.

###### Note

On reset, the default Exception level for taking exceptions from EL0 is EL1. However, an implementation might include controls that can change this, effectively making EL1 inactive. See the description of the Exception model for more information.

In body text, the term UNDEFINED is shown in SMALL CAPITALS.

UNKNOWN An UNKNOWN value does not contain valid data, and can vary from moment to moment, instruction to instruction, and implementation to implementation. An UNKNOWN value must not return information that cannot be accessed at the current or a lower level of privilege using instructions that are not UNPREDICTABLE, are not CONSTRAINED UNPREDICTABLE, and do not return UNKNOWN values.

An UNKNOWN value must not be documented or promoted as having a defined value or effect. In body text, the term UNKNOWN is shown in SMALL CAPITALS. See also CONSTRAINED UNPREDICTABLE, UNDEFINED , UNPREDICTABLE.

###### UNPREDICTABLE

Means the behavior cannot be relied upon. UNPREDICTABLE behavior must not perform any function that cannot be performed at the current or a lower level of privilege using instructions that are not UNPREDICTABLE. UNPREDICTABLE behavior must not be documented or promoted as having a defined effect. An instruction that is UNPREDICTABLE can be implemented as UNDEFINED. Execution at Non-secure EL1 or EL0 of an instruction that is UNPREDICTABLE can be implemented as generating a trap exception that is taken to EL2, provided that at least one instruction that is not UNPREDICTABLE and is not CONSTRAINED UNPREDICTABLE causes a trap exception that is taken to EL2. In body text, the term UNPREDICTABLE is shown in SMALL CAPITALS. See also CONSTRAINED UNPREDICTABLE, UNDEFINED.

Upstream Information propagating in the direction from terminating Completer components towards Requesters. VA See Virtual address (VA). Virtual address (VA)

An address generated by an Arm PE. This means it is an address that might be held in the program counter of the PE. For a PMSA implementation, the virtual address is identical to the physical address.

See also Intermediate physical address (IPA), Physical address (PA).

Virtual PARTID One of a small range of PARTIDs that can be used by a virtual machine (VM). Virtual PARTIDs are mapped into physical PARTIDs using the virtual partition mapping entries in the MPAMVPM0 MPAMVPM7 registers.

VM Virtual Machine. VMM Virtual Machine Monitor. An alias for “hypervisor”. Word A 32-bit data item. Words are normally word-aligned in Arm systems. Word-aligned Means that the address is divisible by 4.

