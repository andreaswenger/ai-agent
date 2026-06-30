# **Nutanix Physical Memory Configuration** 

## Legal 

© 2026 Nutanix, Inc. All rights reserved. Nutanix, the Nutanix logo and all Nutanix product and service names mentioned are registered trademarks or trademarks of Nutanix, Inc. in the United States and other countries. All other brand names mentioned are for identification purposes only and may be the trademarks of their respective holder(s). 

Certain information contained in this content may link or refer to, or be based on, studies, publications, surveys, and other data obtained from third-party sources and our own internal estimates and research. While we believe these third-party studies, publications, surveys, and other data are reliable as of the date of publication, they have not independently verified unless specifically stated, and we make no representation as to the adequacy, fairness, accuracy, or completeness of any information obtained from a third-party. Our decision to publish, link to or reference third-party data should not be considered an endorsement of any such content. 

Nutanix, Inc. 1740 Technology Drive San Jose, CA 95110 

Nutanix Physical Memory Configuration 

## **Contents** 

**1. Executive Summary.................................................................................4** 

**2. Nutanix Physical Memory Configuration...............................................6** 

Interleaved Memory.............................................................................................................................6 Intel Emerald Rapids...........................................................................................................................6 Intel Sapphire Rapids..........................................................................................................................7 Ice Lake...............................................................................................................................................9 Intel Cascade Lake........................................................................................................................... 11 

## **3. Nutanix Physical Memory Considerations and** 

**Recommendations..................................................................................14 About Nutanix.............................................................................................16 List of Figures.............................................................................................................................................17** 

Nutanix Physical Memory Configuration 

## 1. Executive Summary 

This document addresses the following frequently asked questions about physical memory on Nutanix appliances or nodes: 

- When should I use a given number of memory modules per Nutanix node? 

- How should I populate my Nutanix node with memory modules to get the best possible performance, prepare for future expansion, or achieve optimal total cost of ownership (TCO)? 

- When is a Nutanix node's memory mode balanced, nearly balanced, or unbalanced? 

We also answer each of these questions in relation to the significant differences between Intel CPU families. 

In this document, we cover the following topics: 

- Introduction to Nutanix physical memory configuration 

- Intel Ice Lake architecture 

- Intel Cascade Lake architecture 

- Configuration examples 

- Considerations and recommendations 

_Table: Document Version History_ 

|**Version Number**|**Published**|**Notes**|
|---|---|---|
|1.0|August 2019|Original publication.|
|1.1|September 2020|Updated Nutanix overview.|



© 2026 Nutanix, Inc. All rights reserved  | **4** 

Nutanix Physical Memory Configuration 

|**Version Number**|**Published**<br>**Notes**|
|---|---|
|2.0<br>2.1<br>3.0<br>3.1<br>3.2|October 2021<br>Updated with Ice Lake<br>processors. Added Memory<br>Interleaving and Ice Lake<br>sections, updated the<br>Configuration Example with<br>Skylake or Cascade Lake<br>and the Considerations and<br>Recommendations sections,<br>and updated the Skylake and<br>Cascade Lake Components<br>and the Skylake and Cascade<br>Lake Configuration Example<br>tables.<br>November 2022<br>Updated the Considerations<br>and Recommendations<br>section.<br>March 2024<br>Updated for Nutanix NX G9<br>nodes.<br>March 2025<br>Added the Intel Sapphire<br>Rapids section and updated<br>the Nutanix Physical<br>Memory Considerations and<br>Recommendations section.<br>March 2026<br>Removed references to NX G5<br>and G6 nodes, Intel Broadwell,<br>and Intel Skylake.|



© 2026 Nutanix, Inc. All rights reserved  | **5** 

Nutanix Physical Memory Configuration 

## 2. Nutanix Physical Memory Configuration 

Maximum memory frequency or speed and the number of available memory slots depend on the exact CPU model in the different CPU families, the kind of memory used, and type of motherboard. Here, we describe the maximum values that NX nodes can currently support. OEM-supported platforms can have different maximum values. Check with the hardware vendor for the maximum values for your specific system. 

Maximum physical memory capacity varies between the different Nutanix node types because of form factor differences and the number of Nutanix nodes per Nutanix block. 

This document considers CPU configuration and performance at a very high level. For more information, see Intel processor documentation. 

## **Interleaved Memory** 

Interleaved memory increases the memory bandwidth available for applications and lets the CPU spread memory access across DIMMs instead of limiting it to one DIMM at a time. Interleaved memory enables processes, transactions, and applications to access contiguous memory more quickly because the system doesn't need to finish one memory transaction before it starts the next. 

An interleave set is the logical construct that holds the DIMMs. A balanced memory configuration only has one interleave set per CPU, and nearly balanced and unbalanced memory configurations have at least two interleave sets per CPU. 

## **Intel Emerald Rapids** 

The following figure depicts one CPU socket in the Emerald Rapids CPU architecture. It includes the components described in this document. 

© 2026 Nutanix, Inc. All rights reserved  | **6** 

Nutanix Physical Memory Configuration 

Figure 1: Emerald Rapids CPU Architecture 

The following table provides memory information for both a single–CPU socket system and a dual–CPU socket system. The dual–CPU socket system is the most common configuration for Nutanix nodes, but specific models come with a single CPU socket. 

_Table: Emerald Rapids Components_ 

|**Component**|**Single CPU**|**Dual CPU Quantity or Speed**|
|---|---|---|
||**Quantity or Speed**||
|Memory controllers|4|8|
|Memory channels|8|16|
|Memory slots per channel|2|2|
|Total memory slots|16|32|
|Memory banks|2|2|
|Maximum memory speed|5,600 MHz|5,600 MHz|
|using memory bank 1|||
|Maximum memory speed|4,400 MHz|4,400 MHz|
|using memory banks 1 and 2|||



Intel processor type and number of memory banks populated determine the maximum memory speed, which can be 5,600 MHz, 5,200 MHz, 4,800 MHz, or 4,400 MHz. 

## **Intel Sapphire Rapids** 

The NX G9 series uses Intel Sapphire Rapids processors. The following figure depicts one CPU socket in the Sapphire Rapids CPU architecture. It includes the components described in this document. 

© 2026 Nutanix, Inc. All rights reserved  | **7** 

Nutanix Physical Memory Configuration 

Figure 2: Sapphire Rapids CPU Architecture 

The following table provides memory information for both a single–CPU socket system and a dual–CPU socket system. The dual–CPU socket system is the most common configuration for Nutanix nodes, but specific models come with a single CPU socket. 

_Table: Sapphire Rapids Components_ 

|**Component**|**Single CPU**|**Dual CPU Quantity or Speed**|
|---|---|---|
||**Quantity or Speed**||
|Memory controllers|4|8|
|Memory channels|8|16|
|Memory slots per channel|2|2|
|Total memory slots|16|32|
|Memory banks|2|2|
|Maximum memory speed|4,800 MHz|4,800 MHz|
|using memory bank 1|||
|Maximum memory speed|4,400 MHz|4,400 MHz|
|using memory banks 1 and 2|||



**Note:** Intel processor type and number of memory banks populated determine the maximum memory speed, which can be 4,800 MHz, 4,400 MHz, or 4,000 MHz. 

## **Configuration Example with Sapphire and Emerald Rapids** 

As an example, consider a dual–CPU socket system where you need 2,048 GB of memory. For memory performance and scalability, use 16 × 128 GB memory modules, as this design populates all 16 memory slots in the first memory bank. 

Based on memory module price per gigabyte, the TCO is potentially lower when you use both memory banks—32 memory slots total—than when you use one memory bank, or 

© 2026 Nutanix, Inc. All rights reserved  | **8** 

Nutanix Physical Memory Configuration 

16 memory slots. Use 32 × 64 GB memory modules if the system must be optimized for performance and TCO. 

If budget or a specific memory capacity are more important than performance, you can also achieve a nearly balanced configuration for a dual–CPU socket system with 4, 8, 12, and 24 DIMM slots populated. 

The following table provides examples of memory configurations and their relation to scalability, TCO, and performance. The figures in the Number of Memory Slots Used column apply to a dual–CPU socket system. 

_Table: Sapphire and Emerald Rapids Configuration Example_ 

|**Memory**|**DIMM or**|**Number of**|**Scalable**|**TCO Optimal**|**Performance**|
|---|---|---|---|---|---|
|**Capacity**|**Memory Size**|**Memory Slots**|||**Optimal**|
|1,024 GB|32 GB|32|No|Yes|Yes|
|1,024 GB|64 GB|16|Yes|No|Yes|
|1,024 GB|128 GB|8|Yes|No|No|
|2,048 GB|64 GB|32|No|Yes|Yes|
|2,048 GB|128 GB|16|Yes|No|Yes|
|4,096 GB|128 GB|32|No|Yes|Yes|



## **Ice Lake** 

The NX G8 series uses Intel Ice Lake processors. One CPU socket in the Ice Lake CPU architecture looks the same as one Sapphire Rapids CPU socket; see the Sapphire Rapids CPU Architecture image in the previous section. 

The following table provides memory information for both a single–CPU socket system and a dual–CPU socket system. The dual–CPU socket system is the most common configuration for Nutanix nodes, but specific models come with a single CPU socket. 

_Table: Ice Lake Components_ 

|**Component**|**Single CPU**|**Dual CPU Quantity or Speed**|
|---|---|---|
||**Quantity or Speed**||
|Memory controllers|4|8|
|Memory channels|8|16|



© 2026 Nutanix, Inc. All rights reserved  | **9** 

Nutanix Physical Memory Configuration 

|**Component**|**Single CPU**<br>**Quantity or Speed**<br>**Dual CPU Quantity or Speed**|
|---|---|
|Memory slots per channel<br>Total memory slots<br>Memory banks<br>Maximum memory speed<br>using memory bank 1<br>Maximum memory speed<br>using memory banks 1 and 2|2<br>2<br>16<br>32<br>2<br>2<br>3,200 MHz<br>3,200 MHz<br>2,933 MHz<br>2,933 MHz|



**Note:** Intel processor type and number of memory banks populated determine the maximum memory speed, which can be 3,200 MHz, 2,933 MHz, or 2,667 MHz. 

## **Configuration Example with Ice Lake** 

As an example, consider a dual–CPU socket system where you need 2,048 GB of memory. For memory performance and scalability, use 16 × 128 GB memory modules, as this design populates all 16 memory slots in the first memory bank. 

Based on memory module price per gigabyte, the TCO is potentially lower when you use both memory banks—32 memory slots total—than when you use one memory bank, or 16 memory slots. Use 32 × 64 GB memory modules if the system must be optimized for performance and TCO. 

If budget or a specific memory capacity are more important than performance, you can also achieve a nearly balanced configuration for a dual–CPU socket system with 4, 8, 12, and 24 DIMM slots populated. 

The following table provides examples of memory configurations and their relation to scalability, TCO, and performance. The figures in the Number of Memory Slots Used column apply to a dual–CPU socket system. 

_Table: Ice Lake Configuration Example_ 

|**Memory**|**DIMM or**|**Number of**|**Scalable**|**TCO Optimal**|**Performance**|
|---|---|---|---|---|---|
|**Capacity**|**Memory Size**|**Memory Slots**|||**Optimal**|
|1,024 GB|32 GB|32|No|Yes|Yes|
|1,024 GB|64 GB|16|Yes|No|Yes|



© 2026 Nutanix, Inc. All rights reserved  | **10** 

Nutanix Physical Memory Configuration 

|**Memory**|**DIMM or**|**Number of**|**Scalable**|**TCO Optimal**|**Performance**|
|---|---|---|---|---|---|
|**Capacity**|**Memory Size**|**Memory Slots**|||**Optimal**|
|1,024 GB|128 GB|8|Yes|No|No|
|2,048 GB|64 GB|32|No|Yes|Yes|
|2,048 GB|128 GB|16|Yes|No|Yes|
|4,096 GB|128 GB|32|No|Yes|Yes|



## **Intel Cascade Lake** 

Nutanix uses Intel Cascade Lake CPUs in the NX G7 Nutanix nodes. 

The following figure depicts one CPU socket in the Cascade Lake CPU architecture. It includes the components described in this document. 

Figure 3: Cascade Lake CPU Architecture 

The following table provides memory information for both a single–CPU socket system and a dual–CPU socket system. The dual–CPU socket system is the most common configuration for Nutanix nodes, but specific models come with a single CPU socket. 

## _Table: Cascade Lake Components_ 

|**Component**|**Single CPU**|**Dual CPU Quantity or Speed**|
|---|---|---|
||**Quantity or Speed**||
|Memory controllers|2|4|
|Memory channels|6|12|
|Memory slots per channel|2|2|
|Total memory slots|12|24|



© 2026 Nutanix, Inc. All rights reserved  | **11** 

Nutanix Physical Memory Configuration 

|**Component**|**Single CPU**<br>**Quantity or Speed**<br>**Dual CPU Quantity or Speed**|
|---|---|
|Memory banks<br>Maximum memory speed<br>using memory bank 1<br>Maximum memory speed<br>using memory banks 1 and 2|2<br>2<br>2,933 MHz or 2,667 MHz<br>2,933 MHz or 2,667 MHz<br>2,933 MHz or 2,666 MHz<br>2,933 MHz or 2,667 MHz|



**Note:** Intel processor type and number of memory banks populated determine the maximum memory speed. Cascade Lake maximum memory speed is 2,933 MHz. 

## **Configuration Example with Cascade Lake** 

As an example, for a dual–CPU socket system where you need 512 GB of memory, you can use 8 × 64 GB memory modules (which provide 512 GB of memory), but this configuration doesn't deliver optimal performance. For better memory performance and future scalability, use 12 × 64 GB memory modules, as this design populates all 12 memory slots in the first memory bank and offers 768 GB of memory. 

Based on memory module price per gigabyte, the TCO is potentially lower when you use both memory banks—24 memory slots total—than when you use one memory bank, or 12 memory slots. In addition to potentially better TCO, we see a slightly better performance predictability during testing when we use all memory slots. In this example, the configuration is 24 × 32 GB memory modules. 

If budget or a specific memory capacity is more important than performance, you can also achieve a nearly balanced configuration for a dual–CPU socket system with 6, 8, and 16 DIMM slots populated. 

The following table provides examples of memory configurations and their relation to scalability, TCO, and performance. The figures in the Number of Memory Slots Used column apply to a dual–CPU socket system. 

_Table: Cascade Lake Configuration Example_ 

|**Memory**|**DIMM or**|**Number of**|**Scalable**|**TCO Optimal**|**Performance**|
|---|---|---|---|---|---|
|**Capacity**|**Memory Size**|**Memory Slots**|||**Optimal**|
|512 GB|64 GB|8|Yes|No|No|
|768 GB|32 GB|24|No|Yes|Yes|



© 2026 Nutanix, Inc. All rights reserved  | **12** 

Nutanix Physical Memory Configuration 

|**Memory**<br>**Capacity**|**DIMM or**<br>**Memory Size**<br>**Number of**<br>**Memory Slots**<br>**Scalable**<br>**TCO Optimal**<br>**Performance**<br>**Optimal**|
|---|---|
|768 GB<br>1,024 GB<br>1,536 GB<br>1,536 GB|64 GB<br>12<br>Yes<br>No<br>Yes<br>64 GB<br>16<br>Yes<br>No<br>No<br>64 GB<br>24<br>No<br>Yes<br>Yes<br>128 GB<br>12<br>Yes<br>No<br>Yes|



© 2026 Nutanix, Inc. All rights reserved  | **13** 

Nutanix Physical Memory Configuration 

## 3. Nutanix Physical Memory Considerations and Recommendations 

Nutanix recommends balanced memory configurations: 

- Use identical CPUs. 

- Keep the memory configuration identical across CPUs. 

- Keep memory channel configuration identical. 

- Use identical DIMMs. 

The following table explains the relationship between number of DIMM slots populated, predictable performance, and maximum memory bandwidth or throughput capability for these definitions: 

- P-1.0: balanced configuration; predictable performance and maximum throughput for the given CPU 

- P-0.n: nearly balanced configuration; predictable performance with nearly n percent throughput possible for the given CPU 

- U: unbalanced configuration 

_Table: Populated DIMM Slots, Performance, and Bandwidth or Throughput by Processor_ 

|**Number of DIMMs**|**Sapphire and**|**Icelake (ICX)**|**Cascade Lake**|
|---|---|---|---|
||**Emerald Rapids**|**Configuration**|**(CLX) Configuration**|
|2|P-0.125|P-0.125|P-0.167|
|4|P-0.25|P-0.25|P-0.33|
|6|U|P-0.33|P-0.5|
|8|P-0.50|P-0.50|P-0.66|
|12|P-0.75|P-0.75|P-1.0|
|16|P-1.0|P-1.0|P-0.66|
|18|U|U|U|



© 2026 Nutanix, Inc. All rights reserved  | **14** 

Nutanix Physical Memory Configuration 

|**Number of DIMMs**|**Sapphire and**|**Icelake (ICX)**|**Cascade Lake**|
|---|---|---|---|
||**Emerald Rapids**|**Configuration**|**(CLX) Configuration**|
|24|P-0.66|P-0.66|P-1.0|
|32|P-1.0|P-1.0|N/A|



Use the following formulas to determine the number of memory modules required for different balanced memory configurations: 

- Ice Lake, optimal performance and scalability: Amount of memory required / 16 For example: 2,048 GB / 16 = 128 GB memory modules 

- Ice Lake, optimal performance and TCO: Amount of memory required / 32 For example: 2,048 GB / 32 = 64 GB memory modules 

- Cascade Lake, optimal performance and scalability: Amount of memory required / 12 For example: 768 GB / 12 = 64 GB memory modules 

- Cascade Lake, optimal performance and TCO: Amount of memory required / 24 

For example: 768 GB / 24 = 32 GB memory modules 

When you populate all available memory banks, memory speed drops for all CPUs: 

- Emerald Rapids from 5,600 MHz to 4,400 MHz 

- Sapphire Rapids from 4,800 MHz to 4,400 MHz 

- Ice Lake from 3,200 MHz to 2,933 MHz 

- Cascade Lake from 2,933 to 2,666 MHz 

The memory speed drop described doesn't necessarily mean lower performance for Ice Lake or Cascade Lake processors because using all memory slots in these CPUs provides higher bandwidth. Application characteristics and memory type factor in. 

When you need larger memory module sizes (for example, 128 GB memory modules or support for up to 4.5 TB of memory per socket), choose a model with a compatible CPU suffix (M, L) during your selection process. 

© 2026 Nutanix, Inc. All rights reserved  | **15** 

Nutanix Physical Memory Configuration 

## About Nutanix 

Nutanix offers a single platform to run all your apps and data across multiple clouds while simplifying operations and reducing complexity. Trusted by companies worldwide, Nutanix powers hybrid multicloud environments efficiently and cost effectively. This enables companies to focus on successful business outcomes and new innovations. Learn more at Nutanix.com. 

© 2026 Nutanix, Inc. All rights reserved  | **16** 

Nutanix Physical Memory Configuration 

## **List of Figures** 

Figure 1: Emerald Rapids CPU Architecture................................................................................................. 7 Figure 2: Sapphire Rapids CPU Architecture................................................................................................ 8 Figure 3: Cascade Lake CPU Architecture..................................................................................................11 

