![image 1](arm_mpam_architecture_compliance_test_scenario_images/imageFile1.png)

![image 2](arm_mpam_architecture_compliance_test_scenario_images/imageFile2.png)

## Arm® MPAM Architecture Compliance

Revision: r1p0

## Test Scenario

###### Non-Confidential Issue 0001-00

Copyright © 2025 Arm Limited (or its affiliates). All rights reserved.

###### ARM040-1254092399-18836

###### Arm MPAM Test Scenario Document

Copyright © 2025 Arm Limited (or its affiliates). All rights reserved. Release information Document history

|Issue|Date|Confidentiality|Change|
|---|---|---|---|
|01|31 Mar 2025|Non-Confidential|0.5.0 Alpha|


##### Non-Confidential Proprietary Notice

This document is protected by copyright and other related rights and the practice or implementation of the information contained in this document may be protected by one or more patents or pending patent applications. No part of this document may be reproduced in any form by any means without the express prior written permission of Arm. No license, express or implied, by estoppel or otherwise to any intellectual property rights is granted by this document unless specifically stated.

Your access to the information in this document is conditional upon your acceptance that you will not use or permit others to use the information for the purposes of determining whether implementations infringe any third-party patents.

THIS DOCUMENT IS PROVIDED “AS IS”. ARM PROVIDES NO REPRESENTATIONS AND NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, INCLUDING, WITHOUT LIMITATION, THE IMPLIED WARRANTIES OF MERCHANTABILITY, SATISFACTORY QUALITY, NON-INFRINGEMENT OR FITNESS FOR A PARTICULAR PURPOSE WITH RESPECT TO THE DOCUMENT. For the avoidance of doubt, Arm makes no representation with respect to, has undertaken no analysis to identify or understand the scope and content of, patents, copyrights, trade secrets, or other rights.

This document may include technical inaccuracies or typographical errors. TO THE EXTENT NOT PROHIBITED BY LAW, IN NO EVENT WILL ARM BE LIABLE FOR ANY DAMAGES, INCLUDING WITHOUT LIMITATION ANY DIRECT, INDIRECT, SPECIAL, INCIDENTAL, PUNITIVE, OR CONSEQUENTIAL DAMAGES, HOWEVER CAUSED AND REGARDLESS OF THE THEORY OF LIABILITY, ARISING OUT OF ANY USE OF THIS DOCUMENT, EVEN IF ARM HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES. This document consists solely of commercial items. You shall be responsible for ensuring that any use, duplication or disclosure of this document complies fully with any relevant export laws and regulations to assure that this document or any portion thereof is not exported, directly or indirectly, in violation of such export laws. Use of the word “partner” in reference to Arm's customers is not intended to create or refer to any partnership relationship with any other company. Arm may make changes to this document at any time and without notice. This document may be translated into other languages for convenience, and you agree that if there is any conflict between the English version of this document and any translation, the terms of the English version of the Agreement shall prevail. The Arm corporate logo and words marked with ® or ™ are registered trademarks or trademarks of Arm Limited (or its affiliates) in the US and/or elsewhere. All rights reserved. Other brands and names mentioned in this document may be the trademarks of their respective owners. Please follow Arm's trademark usage guidelines at https://www.arm.com/company/policies/trademarks. Copyright © 2025 Arm Limited (or its affiliates). All rights reserved. Arm Limited. Company 02557590 registered in England.

110 Fulbourn Road, Cambridge, England CB1 9NJ. (LES-PRE-20349)

##### Confidentiality Status

This document is Non-Confidential. The right to use, copy and disclose this document may be subject to license restrictions in accordance with the terms of the agreement entered into by Arm and the party that Arm delivered this document to.

Unrestricted Access is an Arm internal classification.

##### Product Status

The information in this document is for an Alpha product, that is a product under development.

###### Progressive terminology commitment

Arm values inclusive communities. Arm recognizes that we and our industry have used terms that can be offensive. Arm strives to lead the industry and create change.

This document includes terms that can be offensive. We will replace these terms in a future issue of this document. If you find offensive terms in this document, please email terms@arm.com.

##### Web Address

www.arm.com

# Contents

###### 1 Introduction ............................................................................................................................................................5

- 1.1 Product revision status.....................................................................................................................................5
- 1.2 Intended audience .............................................................................................................................................5
- 1.3 Conventions.........................................................................................................................................................5

- 1.3.1 Glossary.............................................................................................................................................................5
- 1.3.2 Typographical Conventions.........................................................................................................................6


- 1.4 Useful resources.................................................................................................................................................6
- 1.5 Feedback...............................................................................................................................................................7


- 1.5.1 Feedback on this product.............................................................................................................................7
- 1.5.2 Feedback on content.....................................................................................................................................7


###### 2 Arm MPAM MSC Architecture .........................................................................................................................8

- 2.1 MPAM ACS..........................................................................................................................................................8
- 2.2 Register Tests......................................................................................................................................................9
- 2.3 Cache Tests........................................................................................................................................................11
- 2.4 Error Tests..........................................................................................................................................................18
- 2.5 Memory Bandwidth Tests .............................................................................................................................23


###### Appendix A Revisions............................................................................................................................................25

# 1 Introduction

### 1.1 Product revision status

The rmpn identifier indicates the revision status of the product described in this book, for example, r1p2, where:

rm Identifies the major revision of the product, for example, r1. pn

Identifies the minor revision or modification status of the product, for example, p2.

### 1.2 Intended audience

This document is for engineers who are verifying an implementation of Arm® Memory System Resource Partitioning and Monitoring (MPAM) System Component Architecture.

### 1.3 Conventions

The following subsections describe conventions used in Arm documents.

#### 1.3.1 Glossary

The Arm Glossary is a list of terms used in Arm documentation, together with definitions for those terms. The Arm Glossary does not contain terms that are industry standard unless the Arm meaning differs from the generally accepted meaning.

See the Arm Glossary for more information: https://developer.arm.com/glossary.

- 1.3.2 Typographical Conventions


|Convention|Use|
|---|---|
|italic|Introduces citations.|
|bold|Highlights interface elements, such as menu names. Denotes signal names. Also used for terms in descriptive lists, where appropriate.<br><br>|
|monospace|Denotes text that you can enter at the keyboard, such as commands, file and program names, and source code.<br><br>|
|monospace bold|Denotes language keywords when used outside example code.|
|monospace underline<br><br>|Denotes a permitted abbreviation for a command or option. You can enter the underlined text instead of the full command or option name.<br><br>|
|<and>|Encloses replaceable terms for assembler syntax where they appear in code or code fragments. For example:<br><br>MRC p15, 0, <Rd>, <CRn>, <CRm>, <Opcode_2>|
|SMALL CAPITALS|Used in body text for a few terms that have specific technical meanings, that are defined in the Arm® Glossary. For example, IMPLEMENTATION DEFINED, IMPLEMENTATION SPECIFIC, UNKNOWN, and UNPREDICTABLE.<br><br>|


### 1.4 Useful resources

This document contains information that is specific to this product. See the following resources for other relevant information.

- • Arm Non-Confidential documents are available on developer.arm.com/documentation. Each document link in the tables below provides direct access to the online version of the document.
- • Arm Confidential documents are available to licensees only through the product package.


|Arm products|Document ID|Confidentiality|
|---|---|---|
|MPAM Memory System Component Specification|ARM IHI 0099A.a|Non-Confidential|
|Arm® Architecture Reference Manual Armv8, for Armv8-A Architecture<br><br>|ARM DDI 0487L.a|Non-Confidential|


### 1.5 Feedback

Arm welcomes feedback on this product and its documentation.

- 1.5.1 Feedback on this product If you have any comments or suggestions about this product, contact your supplier and give:

- • The product name.
- • The product revision or version.
- • An explanation with as much information as you can provide. Include symptoms and diagnostic procedures if appropriate.


- 1.5.2 Feedback on content If you have comments on content, send an email to support-systemready-acs@arm.com and give:


- • The title Arm MPAM Test Scenario.
- • The number ARM040-1254092399-18836
- • If applicable, the page number(s) to which your comments refer.
- • A concise explanation of your comments.
- • Arm also welcomes general suggestions for additions and improvements.


# 2 Arm MPAM MSC Architecture

The MPAM (Memory System Resource Partitioning and Monitoring) architecture is designed for systems running multiple applications or virtual machines on shared memory, particularly in enterprise networking and server environments. It addresses key requirements like controlling the performance impact of non-conforming software, bounding resource usage, and minimizing interference between software components.

MPAM achieves this through two main mechanisms: memory-system resource partitioning and resource usage monitoring. It propagates Partition IDs (PARTID) and Performance Monitoring Groups (PMG) across the memory system, enabling fine-grained control and monitoring of memory resources.

MPAM defines a framework for memory-system components (MSCs) to manage and monitor resource usage, providing standardized, implementation-independent memory-mapped interfaces. These features help align system performance with higher-level software goals and ensure consistent behaviour in complex, multi-tenant environments.

The goal of MPAM ACS is to test the Errors in MSCs, Partitioning & Monitoring related features for compliance against MPAM MSC Specification.

### 2.1 MPAM ACS

The tests are classified as:

- • Register Tests
- • Cache Tests
- • Error Tests
- • Memory Bandwidth Tests


### 2.2 Register Tests

|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|1|Check MPAM Version EXT Bit Check|• Get the total number of MSC nodes.<br><br>• Loop through each MSC node by index.<br><br>• If version is MPAM v1.0, check if EXT field is 0.<br><br>• If version is MPAM v1.1, check if EXT field is 1.<br><br>• If EXT field doesn’t match the expected value, print error and fail the test.<br><br>• If version is not 1.0 or 1.1, print error and fail the test.<br><br>• If all MSCs pass the check, mark the test as passed.<br>|
|2|Check Expansion of MPAMF_ESR|Get the total number of MSC nodes.<br><br>• Loop through each MSC node by index.<br><br>• Read the MPAMF_IDR register value.<br><br>• If both HAS_ESR and HAS_RIS are set, and EXT is set:<br><br>• Check if HAS_EXTD_ESR is also set.<br><br>• If HAS_EXTD_ESR is not set, print error and fail the test.<br><br>• If all MSCs pass the check, mark the test as passed.<br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|3|Check MPAM MSC Feature Test|Get the total number of MSC nodes.<br><br>• Loop through each MSC node by index.<br><br>• Read ID registers: MPAMF_IDR, CCAP_IDR, CSUMON_IDR, MBWUMON_IDR, MSMON_IDR.<br><br>• If version is MPAM v1.0:<br><br>• Check that all prohibited fields (e.g., HAS_CMIN, NO_CMAX, HAS_CASSOC, HAS_ENDIS, etc.) are not set.<br><br>• If any prohibited field is set, print error and increment test failure counter.<br><br>• If version is MPAM v1.1 or v0.1:<br><br>• If HAS_IMPL_IDR is set:<br><br>• Ensure required fields (NO_IMPL_PART, NO_IMPL_MSMON) are also set.<br><br>• If not, print error and increment failure counter.<br><br>• Check that EXT field is set.<br><br>• If not, print error and increment failure counter.<br><br>• If version is unsupported, print error and fail the test.<br><br>• After all nodes are checked, if any failure was recorded, mark the test as failed. Otherwise, mark the test as passed.<br><br><br>|


### 2.3 Cache Tests

|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|101|Check CPOR Partitioning|• Get the index of the LLC and its cache identifier, Skip the test if LLC info is invalid or not found.<br><br>• For each MSC node: Find matching cache resource nodes of type PE_CACHE. Check if CPOR and CSU monitor are supported. Track number of CPORcapable nodes and CSU monitor count. Determine max cache size and min/max PARTID.<br>• Skip the test if CPOR nodes or CSU monitors are not available.<br><br>• Allocate memory for storing CSU monitor counters for each CPOR scenario. Read and modify MPAM2_EL2 to set PARTID and PMG.<br><br>• For each MSC node with CPOR support:<br><br><br>Configure CPOR Settings and disable CCAP partition settings. Allocate source and destination buffers. Configure and enable<br><br>CSU monitor. Wait for NRDY timeout to stabilize configuration Capture CSU monitor count before and after memory copy. Store the count difference for analysis. Free allocated buffers.<br><br>• Restore original MPAM2_EL2 value.<br><br>• Compare CSU monitor counters across scenarios. If counter increases when it should decrease, mark test as failed.<br><br>• Set test result to pass if all comparisons are valid, else fail.<br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|102|Check CCAP Partitioning|• Get the index of the LLC and its cache identifier, Skip the test if LLC info is invalid or not found.<br><br>• For each MSC node: Find matching cache resource nodes of type PE_CACHE. Check if CCAP and CSU monitor are supported. Track number of CCAP-capable nodes and CSU monitor count.<br><br>• Determine max cache size and min/max PARTID. Skip the test if CCAP nodes or CSU monitors are not available.<br><br>• Allocate memory for storing CSU monitor counters for each CCAP scenario. Read and modify MPAM2_EL2 to set PARTID and PMG.<br><br>• For each MSC node with CCAP support: Configure CCAP Settings and disable CPOR partition settings.<br><br>• Allocate source and destination buffers.<br><br>• Configure and enable CSU monitor. Wait for NRDY timeout to stabilize configuration.<br><br>• Capture CSU monitor count before and after memory copy. Store the count difference for analysis.<br><br>• Free allocated buffers. Restore original MPAM2_EL2 value.<br><br>• Compare CSU monitor counters across scenarios. If counter increases when it should decrease, mark test as failed.<br><br>• Set test result to pass if all comparisons are valid, else fail.<br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|103|Check CPOR with CCAP Partitioning|• Get the LLC index and cache identifier; skip the test if either is missing or invalid.<br><br>• Loop through all MSC nodes and their resources to find PE_CACHE resources that match the LLC identifier and support both CCAP and CPOR; count valid nodes, update cache max size, track CSU monitor count if supported, and update minimum max PARTID value.<br><br>• Skip the test if there are no CCAP+CPOR nodes or no CSU monitors available.<br><br>• Allocate a 2D buffer to store CSU monitor counter values for each test scenario and MSC node.<br><br>• Read and store the current MPAM2_EL2 value, clear the PARTID_D and PMG_D fields, set the desired PARTID and PMG values, and write the updated value back to MPAM2_EL2.<br><br>• For each enabled test scenario, loop through all MSC nodes, and for each matching PE_CACHE resource, select the resource instance if RIS is supported, configure CCAP and CPOR partitioning according to the scenario (or fallback to 100% if only one is supported), allocate source and destination memory buffers, configure and enable the CSU monitor, wait for NRDY timeout to complete, record the start CSU count, perform memory copy, record the end count, disable the monitor, store the count difference, and free the allocated memory.<br><br>• Restore the original MPAM2_EL2 register value.<br><br>• Compare CSU monitor count differences across scenarios for each MSC node; if a later scenario shows more activity than the previous one, log the failure.<br><br>• Set the final test status as pass if all comparisons succeed or fail if any mismatch is found.<br><br><br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|104|Check PMG Storage by CPOR Nodes|• Get the LLC index and cache identifier; skip the test if either is missing or invalid.<br><br>• Loop through all MSC nodes and their resources to find PE_CACHE resources matching the LLC identifier that support CPOR; for each valid node, configure RIS if supported, update the max cache size, get the max PMG value, count CSU monitors if supported, and track the minimum max PARTID value.<br><br>• Skip the test if no CPOR-capable nodes or CSU monitors are available.<br><br>• Loop through MSC nodes again and for each valid CPORcapable PE_CACHE resource, configure CPOR partitioning to 75% using the selected PARTID.<br><br>• Set up two PMG groups: PMG1 as default and PMG2 as the highest supported PMG; read and store the current MPAM2_EL2 value, clear PARTID_D and PMG_D fields, set PARTID and PMG1, and write back the modified MPAM2_EL2.<br><br>• For each matching PE_CACHE resource, allocate source and destination memory buffers sized to 50% of the cache size, log the buffer size, and skip if allocation fails.<br><br>• Configure CSU monitor with PMG1, enable monitoring, wait for NRDY timeout, perform a memory copy transaction, and record the updated CSU monitor count (storage_value1).<br><br>• Reconfigure CSU monitor with PMG2, enable monitoring, wait for NRDY timeout again, perform a second memory copy transaction, and record the updated CSU monitor count (storage_value2).<br><br>• Disable CSU monitor, restore original MPAM2_EL2 value, and free the memory buffers.<br><br>• Fail the test if storage_value1 is zero (no activity recorded for<br><br><br>PMG1) or if storage_value2 is non-zero (unexpected activity for<br><br>PMG2); otherwise, mark the test as passed.<br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|105|Check PMG Storage by CCAP Nodes|• Get the LLC index and cache identifier; skip the test if either is missing or invalid.<br><br>• Loop through all MSC nodes and their resources to find PE_CACHE resources matching the LLC identifier that support CCAP; for each valid node, configure RIS if supported, update the max cache size, get the max PMG value, count CSU monitors if supported, and track the minimum max PARTID value.<br><br>• Skip the test if no CCAP-capable nodes or CSU monitors are available.<br><br>• Loop through MSC nodes again and for each valid CCAPcapable PE_CACHE resource, configure CCAP partitioning to 75% using the selected PARTID.<br><br>• Set up two PMG groups: PMG1 as default and PMG2 as the highest supported PMG; read and store the current MPAM2_EL2 value, clear PARTID_D and PMG_D fields, set PARTID and PMG1, and write back the modified MPAM2_EL2.<br><br>• For each matching PE_CACHE resource, allocate source and destination memory buffers sized to 50% of the cache size, log the buffer size, and skip if allocation fails.<br><br>• Configure CSU monitor with PMG1, enable monitoring, wait for NRDY timeout, perform a memory copy transaction, and record the updated CSU monitor count (storage_value1).<br><br>• Reconfigure CSU monitor with PMG2, enable monitoring, wait for NRDY timeout again, perform a second memory copy transaction, and record the updated CSU monitor count (storage_value2).<br><br>• Disable CSU monitor, restore original MPAM2_EL2 value, and free the memory buffers.<br><br>• Fail the test if storage_value1 is zero (no activity recorded for<br><br><br>PMG1) or if storage_value2 is non-zero (unexpected activity for<br><br>PMG2); otherwise, mark the test as passed.<br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|106|Check PARTID Storage by CPOR Nodes<br><br>|• Get the LLC index and cache identifier; skip the test if either is missing or invalid.<br><br>• Loop through all MSC nodes and their resources to find PE_CACHE resources matching the LLC identifier that support CPOR; for each valid node, configure RIS if supported, update the max cache size, count CSU monitors if supported, and track the number of CPOR-capable nodes.<br><br>• Skip the test if no CPOR nodes or CSU monitors are found.<br><br>• Loop through MSC nodes again and for each valid CPORcapable PE_CACHE resource, configure CPOR partitioning to 75% using the selected test PARTID.<br><br>• Create two PARTIDs (partid1 and partid2) for generating PE traffic; read and store the current MPAM2_EL2 value, clear PARTID_D and PMG_D fields, set PARTID to partid1 and PMG to default, and write the updated MPAM2_EL2 value.<br><br>• For each matching PE_CACHE resource, allocate source and destination memory buffers sized to 50% of cache size, log buffer size, and skip if allocation fails.<br><br>• Configure CSU monitor with partid1, enable monitoring, wait for NRDY timeout, perform a memory transaction, and record updated CSU monitor count as storage_value1.<br><br>• Reconfigure CSU monitor with partid2, enable monitoring again, wait for NRDY timeout, perform a second memory transaction, and record updated CSU monitor count as storage_value2.<br><br>• Disable the monitor, restore MPAM2_EL2 to its original value, and free the memory buffers.<br><br>• Fail the test if storage_value1 is zero (no activity for partid1) or if storage_value2 is non-zero (unexpected activity for partid2); otherwise, mark the test as passed.<br><br><br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|107|Check PARTID Storage by CCAP Nodes<br><br>|• Get the LLC index and cache identifier; skip the test if either is missing or invalid.<br><br>• Loop through all MSC nodes and their resources to find PE_CACHE resources matching the LLC identifier that support CCAP; for each valid node, configure RIS if supported, update the max cache size, count CSU monitors if supported, and track the number of CCAP-capable nodes.<br><br>• Skip the test if no CCAP nodes or CSU monitors are found.<br><br>• Loop through MSC nodes again and for each valid CCAPcapable PE_CACHE resource, configure CCAP partitioning to 75% using the selected test PARTID.<br><br>• Create two PARTIDs (partid1 and partid2) for generating PE traffic; read and store the current MPAM2_EL2 value, clear PARTID_D and PMG_D fields, set PARTID to partid1 and PMG to default, and write the updated MPAM2_EL2 value.<br><br>• For each matching PE_CACHE resource, allocate source and destination memory buffers sized to 50% of cache size, log buffer size, and skip if allocation fails.<br><br>• Configure CSU monitor with partid1, enable monitoring, wait for NRDY timeout, perform a memory transaction, and record updated CSU monitor count as storage_value1.<br><br>• Reconfigure CSU monitor with partid2, enable monitoring again, wait for NRDY timeout, perform a second memory transaction, and record updated CSU monitor count as storage_value2.<br><br>• Disable the monitor, restore MPAM2_EL2 to its original value, and free the memory buffers.<br><br>• Fail the test if storage_value1 is zero (no activity for partid1) or if storage_value2 is non-zero (unexpected activity for partid2); otherwise, mark the test as passed.<br><br><br>|


### 2.4 Error Tests

|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|201|Check MSC PARTID selection range error<br><br>|• Get PE index and total MSC node count.<br><br>• Loop through each MSC; skip if MPAMF_ESR is not supported.<br><br>• Save current MPAMF_ECR value for later restore. Reset error code; fail test if reset fails.<br><br>• Program MPAMCFG_PART_SEL with out-of-range PARTID. Wait briefly for error to propagate.<br><br>• Read MPAMF_ESR and check for expected error code. If error code mismatch, log and mark test as failed.<br><br>• Restore original MPAMF_ECR value.<br><br>• After loop, skip test if no ESR-capable nodes were found; fail if any mismatch; else pass.<br><br><br>|
|202|Check request PARTID out-of-range error<br><br>|• Get PE index, total MSC count, and save MPAM2_EL2.<br><br>• Loop through MSCs; skip if MPAMF_ESR not supported. Save MPAMF_ECR and reset error code; fail if reset fails.<br><br>• Program MPAM2_EL2 with PARTID that is beyond the range of MSC's maximum PARTID; if skipped, restore MPAM2_EL2 and continue. Mark test as executed.<br><br>• Wait briefly, read MPAMF_ESR, and check for expected error code; fail if mismatched.<br><br>• Restore MPAM2_EL2 and MPAMF_ECR. Mark test as skipped, failed, or passed based on results.<br><br><br>|
|203|Check MSMON config ID out-ofrange error<br><br>|• Get PE index and total number of MSC nodes.<br><br>• Loop through MSCs; skip if MPAMF_ESR is not supported. Save MPAMF_ECR and reset any existing error code; fail if reset fails.<br><br>• Check if the MSC supports monitors and get the total count of CSU and MBWU monitors.<br><br>• Skip if no monitors are present in the MSC. Mark that at least one MSC ran the test.<br><br>• Generate a monitor config ID out-of-range error using the monitor count. Wait briefly, read MPAMF_ESR, and verify the error code; fail if it doesn’t match expected.<br><br>• Restore original MPAMF_ECR value. Mark test as skipped if no eligible MSCs, failed if any mismatch, otherwise passed.<br><br><br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|204|Check request PMG out-of-range error<br><br>|• Get PE index and total MSC count; save current MPAM2_EL2.<br><br>• Loop through MSCs; skip if MPAMF_ESR not supported. Save MPAMF_ECR and reset error code; fail if reset fails.<br><br>• Program MPAM2_EL2 with PMG that is beyond the range of MSC's maximum PMG; if skipped, restore MPAM2_EL2 and continue.<br><br>• Mark test as executed; wait, read MPAMF_ESR, and compare with expected error code.<br><br>• Fail if error code mismatches; restore MPAM2_EL2 and MPAMF_ECR. Set final status as skipped, failed, or passed based on test outcome.<br><br><br>|
|205|Check MSC MONITOR selection range error<br><br>|• Get PE index and total MSC count. Loop through MSCs; skip if ESR not supported or no monitors are implemented.<br><br>• Count CSU and MBWU monitors; skip MSC if none found.<br><br>• Save MPAMF_ECR and reset error code; fail if reset fails. Mark test as executed, generate Monitor selection range error using total monitor count.<br><br>• Wait, read MPAMF_ESR, and check for expected error code; fail if mismatched.<br><br>• Restore MPAMF_ECR and update final test status as skipped, failed, or passed.<br><br><br>|
|206|Check intPARTID out-of-range error|• Get PE index and total MSC node count. Loop through MSCs; skip if MPAMF_ESR or PARTID narrowing not supported.<br><br>• Save MPAMF_ECR and reset error code; fail if reset fails. Mark test as executed, get max PARTID and max internal PARTID.<br><br>• Write a valid reqPARTID and map it to an out-of-range intPARTID (max intPARTID + 1).<br><br>• Wait, read MPAMF_ESR, and verify expected error code; fail if mismatched.<br><br>• Restore MPAMF_ECR and set final test result as skipped, failed, or passed.<br><br><br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|207|Check Unexpected internal error|• Get PE index and total number of MSCs.<br><br>• Loop through MSCs; skip if MPAMF_ESR or PARTID narrowing is not supported. Save MPAMF_ECR and reset error code; fail if reset fails.<br><br>• Mark test as executed, get max PARTID and max internal PARTID.<br><br>• Configure MPAMCFG_PART_SEL to select internal mapping using a valid PARTID.<br><br>• Configure MPAMCFG_INTPARTID with the max internal PARTID to trigger an unexpected internal error.<br><br>• Wait, read MPAMF_ESR, and check for expected error code; fail if mismatched. Restore MPAMF_ECR and set final status as skipped, failed, or passed.<br><br><br>|
|208|Check undefined RIS in MPAMCFG_PART_SEL error<br><br>|• Get PE index and total number of MSCs.<br><br>• Loop through MSCs; skip if MPAMF_ESR or RIS is not supported. Save MPAMF_ECR and reset error code; fail if reset fails.<br><br>• Get max PARTID and max RIS index, then write an out-of-range RIS to MPAMCFG_PART_SEL.<br><br>• Trigger error by accessing the same register; wait, then read MPAMF_ESR.<br><br>• If expected RIS error code is not seen, check if MPAMCFG_PART_SEL behaves as RAZ/WI; fail if not.<br><br>• Restore MPAMF_ECR, and mark the test as skipped, failed, or passed based on result.<br><br><br>|
|209|Check RIS no control error|• Get PE index and total MSC node count. Loop through MSCs; skip if MPAMF_ESR or RIS is not supported.<br><br>• For each resource in the MSC, save MPAMF_ECR and reset error code; fail if reset fails. Select the current RIS index using MPAMCFG_PART_SEL.RIS.<br><br>• Check if CCAP, CPOR, or MBW partitioning is unimplemented; if so, write to the corresponding partition register to trigger an error.<br><br>• Read MPAMF_ESR and verify if the RIS_NO_CTRL error code is reported.<br><br>• If error code mismatches, check if the register behaves as RAZ/WI; fail if not.<br><br>• Restore MPAMF_ECR and update final test status as skipped, failed, or passed.<br><br><br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|210|Check undefined RIS in MSMON_CFG_MON_SEL error<br><br>|• Get PE index and total MSC count. Loop through MSCs; skip if MPAMF_ESR, RIS, or MSMON is not supported.<br><br>• Save MPAMF_ECR and reset error code; fail if reset fails. Get max RIS index and write out-of-range RIS to MSMON_CFG_MON_SEL.<br><br>• Access the same register to trigger the error; wait and read MPAMF_ESR.<br><br>• If expected error code is not reported, verify if register behaves as RAZ/WI; fail if not.<br><br>• Restore MPAMF_ECR and mark test as skipped, failed, or passed.<br><br><br>|
|211|Check RIS no monitor error|• Get PE index and total number of MSCs. Loop through MSCs; skip if MPAMF_ESR or RIS is not supported.<br><br>• For each resource in the MSC, save MPAMF_ECR and reset error code; fail if reset fails. Configure MSMON_CFG_MON_SEL.RIS to select current resource.<br><br>• If CSU or MBWU monitoring is not implemented, write to the corresponding unimplemented MSMON_CFG register to trigger an error.<br><br>• Read MPAMF_ESR and check for RIS_NO_MON error; if missing, check if register behaves as RAZ/WI and fail if not.<br><br>• Restore MPAMF_ECR; mark the test as skipped, failed, or passed based on results.<br><br><br>|
|212|Check MSC Level-Sensitive Error Interrupt Behaviour<br><br>|• Get PE index and total MSC count. Loop through MSCs; skip if MPAMF_ESR is not supported.<br><br>• Get error interrupt number and type; skip if interrupt is not level-triggered or not implemented.<br><br>• Save MPAMF_ECR and reset error code; fail if reset fails. Register the interrupt handler and route the interrupt to the PE.<br><br>• Trigger an MSC error interrupt by writing nonzero value to MPAMF_ESR. Poll until interrupt is received or timeout occurs.<br><br>• Restore MPAMF_ECR; fail if interrupt not received in time.<br><br>• Mark test as skipped if no MSC supports error interrupt; otherwise, pass or fail based on result.<br><br><br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|213|Check MSC Edge-Trigger Error Interrupt Behaviour<br><br>|• Get PE index and total MSC count. Loop through MSCs; skip if MPAMF_ESR not supported.<br><br>• Get error interrupt number and flags; skip if not edge-triggered or not implemented.<br><br>• Save MPAMF_ECR and reset error code; fail if reset fails. Register interrupt handler and route the interrupt to the PE.<br><br>• Try to trigger an MSC error interrupt via MPAMF_ESR. Poll for interrupt completion; if handler catches interrupt in time, fail the test.<br><br>• Restore MPAMF_ECR and mark test as passed, failed, or skipped based on result.<br><br><br>|
|214|Check MBWU monitor overflow interrupt functionality<br><br>|• Get PE index and total MSC count, save and update MPAM2_EL2 with default PARTID and PMG.<br><br>• Loop through MSCs and their resources; check for RIS, MBWU monitor, and overflow interrupt support.<br><br>• Skip if monitor or interrupt not supported; otherwise, register interrupt handler and route it to PE. Trigger MBWU overflow by enabling monitor, writing the maximum supported counter value to MBWU monitor and performing large memory copy using 10MB buffers.<br><br>• Wait for NRDY timeout, perform memory copy, and poll for interrupt completion.<br><br>• If interrupt not received in time, fail the test; otherwise, reset status and free buffers. Restore MPAM2_EL2, disable and reset monitor after test.<br><br>• Mark test as skipped if no MSC supports overflow interrupt; else pass or fail based on result.<br><br><br>|


### 2.5 Memory Bandwidth Tests

|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|301|Check MBWPBM Partitioning|• Get PE index and total MSC count; identify memory resources supporting MBWPBM and track minmax PARTID.<br><br>• Skip test if no MBWPBM-supported memory nodes exist. Disable CPOR and CCAP partitioning across all MSCs for minmax PARTID.<br><br>• Update MPAM2_EL2 with minmax PARTID and default PMG and prepare a counter array to log MBWU activity.<br><br>• For each test scenario (e.g., 20% and 5% partition): Loop through memory MSC nodes, if MBWPBM is supported:<br><br>• Configure MBWPBM for the scenario and disable MBWMIN/MBWMAX if present. Allocate source and destination buffers based on available address range and required size.<br><br>• Skip if SRAT info is missing or memory cannot be allocated. If MBWU monitor is not found, skip the test.<br><br>• Configure and enable the MBWU monitor, then perform memory copy. Record start and end MBWU monitor counts to measure bandwidth usage.<br><br>• Reset and disable MBWU monitor and free allocated memory. Restore original MPAM2_EL2 settings.<br><br>• Compare recorded monitor counts across scenarios; if higher bandwidth usage is recorded in more restricted settings, mark test as failed. Otherwise, mark test as passed.<br><br><br>|


|Test number<br><br>|Scenario|Algorithm|
|---|---|---|
|302|Check MBWMIN Partitioning|• Get PE index and total MSC count; identify memory nodes supporting MBWMIN and find the minimum valid PARTID. Skip the test if no MBWMIN-capable memory nodes exist.<br><br>• Set up MPAM2_EL2 with default PMG and minmax PARTID, and configure all cache nodes to disable CPOR, CCAP, MBWPBM, and MBWMAX. Install exception handlers for sync and async faults.<br><br>• For each MBWMIN-capable memory node: Configure RIS, disable MBWPBM and MBWMAX.<br><br>• Allocate shared buffers and determine buffer size. Launch contention by spawning memory-copy threads on secondary PEs.<br><br>• Scenario 1: Configure MBWMIN with BW1 percentage and record MBWU monitor count. Wait for all secondary PEs to complete; if timeout, fail the test.<br><br>• Scenario 2: Reconfigure MBWMIN with BW2 percentage and repeat measurement. Compare bandwidth usage across scenarios; fail if BW usage increases when a stricter limit is set.<br><br>• Restore MPAM2_EL2 and clean up memory allocations. Mark test as passed if all conditions are met.<br><br><br>|
|303|Check MBWMAX Partitioning|• Get PE index and total MSC count; identify memory resources supporting MBWMAX and track minmax PARTID.<br><br>• Skip test if no MBWMAX-supported memory nodes exist. Disable CPOR and CCAP partitioning across all MSCs for minmax PARTID.<br><br>• Update MPAM2_EL2 with minmax PARTID and default PMG and prepare a counter array to log MBWU activity.<br><br>• For each test scenario (e.g., 20% and 5% partition): Loop through memory MSC nodes; if MBWMAX is supported: Configure MBWMAX for the scenario and disable MBWMIN/MBWPBM if present.<br><br>• Allocate source and destination buffers based on available memory and required size. Skip if SRAT info is invalid or buffer allocation fails.<br><br>• If MBWU monitor is not found, skip the test. Configure and enable the MBWU monitor, then perform memory copy.<br><br>• Record start and end MBWU monitor counts to measure bandwidth usage. Reset and disable MBWU monitor and free allocated memory. Restore original MPAM2_EL2 settings.<br><br>• Compare recorded monitor counts across scenarios; if usage is higher at a more restrictive setting, mark test as failed. Otherwise, mark test as passed.<br><br><br>|


# Appendix A Revisions

###### This appendix describes the technical changes between released issues of this book.

Table A-1 Issue 01

|Change|Location|
|---|---|
|First release| |


