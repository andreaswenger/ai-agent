## Advanced NUMA Configuration Options in AHV 

Article # KB-9795 Last modified on May 15th 2025 Visibility Public 

Summary: Advanced configuration options of NUMA optimizationin AHV. 

## Versions affected: ALL AHV Versions 

Performance AHV 

## Description: 

## Key NUMA Concepts 

NUMA, or non-uniform memory access, is a method of sharing memory to improve performance in a multiprocessing system. 

SMP, or Symmetric Multiprocessing, is a tightly-coupled, share-everything system in which multiple processors act under a single operating system with the ability to access each other's memory over a common bus system. 

QPI, or Intel QuickPath Interconnect, is a point-to-point processor interconnect developed by Intel which replaced the front-side bus in Xeon, Itanium, and certain desktop platforms starting in 2008. NUMA adds the functionality of memory being shared only within the same CPU, eliminating the need for memory access to occur across the QPI. 

## The AHV Scheduler and NUMA 

To keep CPU and memory together as much as possible, the Linux process scheduler under AHV takes several factors into account when choosing where to run a process. The two most important ones are: 

1. Where the memory of the process is physically located 

2. Where the process ran the last time it was on-proc (actively working on a processor core) 

Memory is allocated by the host when the UVM first accesses the memory. The scheduler knows that running on a different core or different NUMA node has a significant cost, so it tries to avoid doing either. It is important to understand that the scheduler is not trying to spread the load evenly across NUMA nodes-- It is trying to make sure each process receives a fair allotment of processor time. 

## Additional Terminology 

CPU – the CPU (Central Processing Unit), the processor, is the component of a computer system that performs all the tasks the OS or Applications on that given system run. CPUs in SMP (Symmetric Multiprocessing) contain many cores. 

CPU Socket – a CPU socket or CPU slot contains one or more mechanical components providing mechanical and electrical connections between a microprocessor and a printed circuit board (PCB). 

The typical setup is a dual-socket system containing two CPUs but also four-socket systems are available. 

CPU Core – a CPU core contains a unit containing an L1 cache and functional units needed to run applications. Cores can independently run applications or threads. One or more cores can exist on a single CPU. 

In AHV there are two options to configure CPU resources: 

num_vcpus - vCPU(s) in AHV define a single socket in the OS. The reason why there is an option to define multiple vCPU(s) per VM is simply for licensing reasons as some software products bind the license to the number of cores vs the number of sockets. 

num_cores_per_vcpus - number Of Cores Per vCPU defines the number of cores within a vCPU defined in the step above. The default is to define a single vCPU + a number of cores. 

## CVM Configuration 

The CVM is pinned to the socket of the first SCSI controller in AHV, ESXi and Hyper-V. The SCSI controller can be bound to NUMA Node 0 or 1 so depending on the platform the CVM runs on the first CPU or seconds CPU. If the node uses two SCSI controllers or runs All-NVMe, configure the CVM to the first NUMA node in the system. The number of cores that is pinned to the CVM depends on the number of cores per physical CPU, as well as on the node's function, e.g. Storage Only node or Compute/Storage Node. Information about CPU and memory configuration can be found in the Field Installation Guide. 

## CPU 

The below table gives an idea of how the configuration during imaging works on different hardware configurations for the CPU configuration of the CVM: 

|Terminology|Description|
|---|---|
|Standard|Min: 8 vCPUs|
|hybrid<br>platforms|Max: 12 vCPUs (14-core or higher processor with HT)|
|Allfash|Min: 12 vCPUs|
|platforms|Max: 12 vCPUs|
|NVMe|Min: 12 vCPUs|
|platforms|Max: 16 vCPUs|
|Standard|Min: Varies depending on specifc processor and platform confgurations|
|hybrid and|(dual-socket vs. single-socket platform)|
|all-fash|Max: 12 vCPUs|
|platforms||
||6-core processor with HT (12 logical cores per socket, 24 logical cores total|
||per node) -> Assign CVM 8 vCPUs|
||6-core processor no HT (6 logical cores per socket, 12 logical cores total|



|||
|---|---|
|Terminology|Description|
||per node) -> Assign CVM 8 vCPUs<br>6-core processor no HT single-socket platform (6 logical cores per socket, 6<br>logical cores total per node) -> Assign CVM 4 vCPUs<br>8-core processor with HT (16 logical cores per socket, 32 logical cores total<br>per node) -> Assign CVM 8 vCPUs<br>8-core processor no HT (8 logical cores per socket, 16 logical cores total<br>per node) -> Assign CVM 8 vCPUs<br>8-core processor no HT single-socket platform (8 logical cores per socket, 8<br>logical cores total per node ) -> Assign CVM 6 vCPUs<br>10-core processor with HT (20 logical cores per socket, 40 logical cores<br>total per node) -> Assign CVM 10 vCPUs<br>10-core processor no HT (10 logical cores per socket, 20 logical cores total<br>per node) -> Assign CVM 10 vCPUs<br>10-core processor no HT single-socket platform (10 logical cores per socket,<br>10 logical cores total per node) -> Assign CVM 7 vCPUs<br>12-core processor with HT (24 logical cores per socket, 48 logical cores<br>total per node) -> Assign CVM 12 vCPUs<br>12-core processor no HT (12 logical cores per socket, 24 logical cores total<br>per node) -> Assign CVM 12 vCPUs<br>12-core processor no HT single-socket platform (12 logical cores per socket,<br>12 logical cores total per node) -> Assign CVM 9 vCPUs<br>16-core or higher processor with HT (32+ logical cores per socket, 64+<br>logical cores total per node) -> Assign CVM 12 vCPUs|
|NVMe<br>platforms|8-core processor with HT (16 logical cores per socket, 32 logical cores total<br>per node) -> Assign CVM 12 vCPUs<br>14-core processor with HT (28 logical cores per socket, 56 logical cores<br>total per node) -> Assign CVM 14 vCPUs<br>16-core or higher processor with HT (32+ logical cores per socket, 64+<br>logical cores total per node) -> Assign CVM 16 vCPUs<br></pre>""",<br>"AOS 4.x and AOS 5.0.x:"<br>"""<pre>|
|||



## Memory 

For the memory configuration of the CVM, it is possible to choose this during Foundation. There are some additional configuration defaults as follows: 

|||
|---|---|
|Type of<br>environment|Description|
|VDI (Virtual<br>Desktop<br>Infrastructure)|16 GB|
|Storage<br>Heavy Nodes<br>(+60 TB)|24 GB|
|Minimal<br>Compute<br>Node<br>(Storage<br>Only)|24 GB|
|High<br>Performance|32 GB|
|Dense Nodes<br>(+120 TB)|40 GB|
|||



## CVM configuration example 

To understand how an existing CVM is sized and where the CVM is running under AHV the below steps are helpful to understand how NUMA and the CPU configuration work in AHV. 

Note: The following commands are to be run on the AHV host that is running the CVM under review. 

- To determine the number of sockets and physical cores, use command lscpu, for example, running on a host with 2 cores and 6 cores per socket (6 physical CPUs X2 =12) - (note "Thread(s) per core" to indicate hyperthreading): 

```
[root@AHV ~]# lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                24
On-line CPU(s) list:   0-23
Thread(s) per core:    2
Core(s) per socket:    6
Socket(s):             2
NUMA node(s):          2
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 63
Model name:            Intel(R) Xeon(R) CPU E5-2620 v3 @ 2.40GHz
```

```
Stepping:              2
CPU MHz:               2600.042
CPU max MHz:           3200.0000
CPU min MHz:           1200.0000
BogoMIPS:              4800.08
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              15360K
NUMA node0 CPU(s):     0-5,12-17
NUMA node1 CPU(s):     6-11,18-23
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge
mca cmov pat ...
```

The following commands should be run on another host with 14 physical CPUs per socket: 

- Numa Nodes: 0,1 Node 0 CPUs: 0 - 13 (Cores) and 28 - 41 (hyperthreads) Node 1 CPUs: 14 - 27 (Cores) and 42 - 55 (hyperthreads) 

1. Numa configuration and the number of cores available in the hardware of a given CVM can be displayed by using the command below: 

```
root@AHV:~$  numactl --hardware
available: 2 nodes (0-1)
node 0 cpus: 0 1 2 3 4 5 6 7 8 9 10 11 12 13 28 29 30 31 32 33 34 35 36 37 38
39 40 41
node 0 size: 192019 MB
node 0 free: 428 MB
node 1 cpus: 14 15 16 17 18 19 20 21 22 23 24 25 26 27 42 43 44 45 46 47 48 49
50 51 52 53 54 55
node 1 size: 193519 MB
node 1 free: 2321 MB
node distances:
node   0   1
  0:  10  21
  1:  21  10
```

With virsh list it is possible to get the ID and Name of the CVM as well as local running User VMs: 

```
root@AHV:~$ virsh list
```

```
 Id    Name                                 State
```

```
----------------------------------------------------
```

```
 1     NTNX-TestCluster-2-CVM               running
```

```
 3     8552e38f-efa4-477b-8e97-1eb03fe1bb0d running
```

The below command illustrates the configuration of the CVM and shows the CVM is pinned to NUMA Node 1 with 14 configured vCPUs, has 32 GB memory configured, and is using hostpassthrough CPU mode: 

```
root@AHV:~$ virsh dumpxml NTNX-TestCluster-2-CVM | grep cpu
  <memory unit='KiB'>33554432</memory>
  <vcpu placement='static' cpuset='14-27,42-55'>14</vcpu>
```

```
  <cpu mode='host-passthrough'/>
```

Use virsh vcpuinfo to see the pinning to the given NUMA node: 

```
root@AHV:~$ virsh vcpuinfo NTNX-TestCluster-2-CVM
VCPU:           0
CPU:            22
State:          running
CPU time:       11903.2s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           1
CPU:            34
State:          running
CPU time:       27084.5s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           2
CPU:            21
State:          running
CPU time:       22038.2s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           3
CPU:            36
State:          running
CPU time:       20936.2s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           4
CPU:            37
State:          running
CPU time:       17738.4s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

```
VCPU:           5
CPU:            31
State:          running
CPU time:       16173.2s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           6
CPU:            20
State:          running
CPU time:       13982.2s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           7
CPU:            33
State:          running
CPU time:       12610.5s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           8
CPU:            30
State:          running
CPU time:       11254.0s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           9
CPU:            38
State:          running
CPU time:       10274.8s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           10
CPU:            28
State:          running
CPU time:       8278.3s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           11
CPU:            25
State:          running
CPU time:       8350.9s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           12
CPU:            32
State:          running
CPU time:       9260.8s
```

```
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           13
CPU:            26
State:          running
CPU time:       8748.4s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

To find the process ID of the CVM top in batch mode can be used with the below command (from the CVM SSH caps output so it is recommended to use the COLUMNS command as shown below): 

```
 root@AHV:~$ top -n 1 -bc | grep CVM | grep -v grep | awk '{print $1}'
25726
```

```
nutanix@CVM:~$ ssh -q  root@192.168.5.1 COLUMNS=200 top -n 1 -bc | grep CVM |
awk '{print $1}'
```

```
25726
```

With numastat qemu-kvm, it is possible to show all running VMs on this AHV host. While it can be identified that all memory is bound to the CVM, there is always a small amount active on the other NUMA node. 

```
root@AHV:~$ numastat qemu-kvm
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
25726 (qemu-kvm)              1.63        32816.6332818.26
202023 (qemu-kvm)          6151.24         2072.02         8223.27
-----------------  --------------- --------------- ---------------
Total                      6152.88        34888.65        41041.53
```

## Solution: 

## Advanced Configuration Options for Optimizing NUMA Access By User VMs. 

Table: Different options exist under AHV, and the options illustrate the possible options both from a memory and CPU perspective. 

|Terminology|Description|
|---|---|
|vcpu_hard_pin=true/false||
||NOTE:|
||Prevent using vcpu_hard_pin and contact Nutanix support before|
||using it (exception  are SAP HANA deployments).|
||Never use more than one VM per node with vCPU hard pinning|



|Terminology|Description|
|---|---|
||and confgure memory close to the max of the NUMA node so the<br>VM will never land on the node with the CVM.<br>Thisfag pins the VM to a given socket and thefrst physical cores<br>(dependent on the confgured vCPU in the VM) and should be<br>used very carefully. If the VM is confgured for example with 4<br>vCPUs (or 1 vCPU with 4 cores) thefrst 4 x physical cores will be<br>pinned to this VM. By default, the Linux Scheduler in AHV will<br>schedule all cores wherever they are best available. Thisfag<br>ensures they are pinned on either CPU1 or CPU2 (randomly when<br>hosting the VM). While the VM stays on this CPU, we cannot defne<br>which CPU. Thisfag should be used only forone User VM per<br>nodeas it pins cores to the same physical cores. On top it should<br>be made sure to give this User VM close to the max of a single<br>NUMA node's memory, so we won't start this VM on the same<br>NUMA node as the CVM. If this VM runs on the same NUMA node<br>as the CVM both the CVM and UVM potentially see high<br>scheduling contention. The same is true for multiple User VMs on<br>the opposite NUMA node to the node where the CVM is pinned.|
|num_threads_per_core=<br><number_of_thread>|AHV controls CPU cores as afat map, so the VM itself is unaware<br>of whether a CPU is a "real" core or a hyperthreaded core. If the<br>work cannot be scheduled on this core, the core must wait for free<br>resources to reschedule on a different core. If the application is<br>aware of the underlying architecture and which core is a "real" vs.<br>hyperthreaded core, it could help to schedule the work. This<br>awareness only applies to very few workloads, one of these being<br>SAP.<br>Note:Setting this option to 0 does not hide the HT featurefag on<br>the CPU when using multiple cores per vCPU. This is because the<br>fag signifes both Hyper-Threading and or Core Multi-Processing.|



|||
|---|---|
|Terminology|Description|
||(ex: set this parameter to 1).<br>For physical hardware with dual-sockets, thisfag should be used<br>with 1 or 2 while for physical hardware with 4 sockets it can be<br>used with 1 - 4 while 0 means default and won't be effective. For<br>more information please refer to the<br>AHV Best Practices.|
|extra_fags=numa_pinning=<br><0/1>|Thisfag pins both memory and vCPUs to a single NUMA node.<br>While this is a very good situation from the performance<br>perspective, it has some caveats regarding migration to other AHV<br>Hosts. It may work well in a homogeneous environment where the<br>CVM is always pinned to the same NUMA nodes, e.g. NUMA node<br>0. However, thisfag may do more harm than good with a<br>heterogeneous cluster, as the VMs may be pinned to the same<br>NUMA node as the CVM. The extrafag is only effective on single<br>or dual-socket systems and not quad-socket hosts.|
|||



## Finding Information About a Given User VM 

Get the UUID information about the VM: 

```
nutanix@CVM:~$ acli vm.list | grep -w Windows2019 | awk '{print $2}'
170d3af9-9978-4653-96aa-3e9b344ff68f
```

The command below will output the PID (Process ID) of the VM in question. The PID is changing each time when the VM is powered off and on: 

```
root@AHV:~$ top -n 1 -bc | grep 170d3af9-9978-4653-96aa-3e9b344ff68f | grep -v
'grep\|frodo' | awk '{print $1}'
```

```
141236
```

With numastat qemu-kvm, it is possible to show all running VMs on this AHV host. The output below shows the default behavior using no advanced options: 

```
root@AHV$ numastat qemu-kvm
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
71428 (qemu-kvm)           8202.62           20.23         8222.86
108154 (qemu-kvm)          7809.41          413.66         8223.07
```

```
139665 (qemu-kvm)          8215.55            7.44         8222.99
225588 (qemu-kvm)             2.51        32813.76        32816.27
-----------------  --------------- --------------- ---------------
Total                     24230.09        33255.09        57485.18
```

As there is no scheduling competition on this host at this point the CPU scheduler mostly schedules the VM on the physical CPUs of NUMA node 0. Be aware that the vCPUs can be scheduled on all available pCPUs on both NUMA nodes: 

```
root@AHV:~$ virsh vcpuinfo 170d3af9-9978-4653-96aa-3e9b344ff68f
VCPU:           0
CPU:            4
State:          running
CPU time:       198.4s
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           1
CPU:            5
State:          running
CPU time:       140.6s
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           2
CPU:            7
State:          running
CPU time:       215.5s
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           3
CPU:            6
State:          running
CPU time:       129.3s
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

## Eight Advanced NUMA Configurations Options 

The configuration options below should be seen as ways to optimize NUMA behaviour by pinning User VMs to specific cores. These options are not needed in the majority of AHV environments. The examples from the output of virsh dumpxml <vm_uuid> show topology sockets number. This number reflects the maximum number of CPUs to be configured and might differ between AHV releases. The high number is used in order to support hot-add functionality. 

Option 1: vcpu_hard_pin=true 

The below acli vm.get <vm_name> output snippet shows the configuration of the VM: 

```
num_cores_per_vcpu: 4
num_threads_per_core: 1
num_vcpus: 1
num_vnuma_nodes: 0
vcpu_hard_pin: True
```

The snippet from virsh dumpxml <vm_uuid>  below shows the configuration being pinned the VM to these physical cores: 

Note: pinning eventually puts all pinned VMs on the same physical cores, so the feature should be used only when absolutely necessary. 

```
<vcpu placement='static' current='4'>240</vcpu>
  <cputune>
    <vcpupin vcpu='0' cpuset='0'/>
    <vcpupin vcpu='1' cpuset='1'/>
    <vcpupin vcpu='2' cpuset='2'/>
    <vcpupin vcpu='3' cpuset='3'/>
  </cputune>
<topology sockets='60' cores='4' threads='1'/>
```

The output of virsh vcpuinfo <vm_uuid> shows the scheduling of a given VM on the physical cores 0- 3: 

```
root@AHV:~$ virsh vcpuinfo 170d3af9-9978-4653-96aa-3e9b344ff68f
VCPU:           0
CPU:            0
State:          running
CPU time:       7.3s
-------------------------------------------------------
CPU Affinity:   y
VCPU:           1
CPU:            1
State:          running
CPU time:       4.3s
------------------------------------------------------
CPU Affinity:   -y
VCPU:           2
CPU:            2
```

```
State:          running
CPU time:       3.8s
-----------------------------------------------------
CPU Affinity:   --y
VCPU:           3
CPU:            3
State:          running
CPU time:       4.8s
----------------------------------------------------
CPU Affinity:   ---y
```

The output of numastat qemu-kvm indicates that the memory is split between NUMA nodes 0 and 1: 

```
root@AHV:~$ numastat qemu-kvm
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
108154 (qemu-kvm)          7809.39          413.66         8223.05
139665 (qemu-kvm)          8215.54            7.44         8222.97
141236 (qemu-kvm)          6230.00         1993.12         8223.12
225588 (qemu-kvm)             2.51        32813.60        32816.11
-----------------  --------------- --------------- ---------------
Total                     22257.43        35227.82        57485.2
```

Option 2: num_threads_per_core=2 

The snippet from acli vm.get <vm_name> below shows the configuration of the VM: 

```
num_cores_per_vcpu: 2
num_threads_per_core: 2
num_vcpus: 1
num_vnuma_nodes: 0
vcpu_hard_pin: False
```

The snippet from virsh dumpxml <vm_id>  below shows the configuration of  2 cores and 2 threads: 

```
<vcpu placement='static' current='4'>240</vcpu>
<topology sockets='30' cores='2' threads='2'/>
```

The output of virsh vcpuinfo <vm_uuid> below shows the scheduling of a given VM on the physical and hyperthreaded cores. The CPU Affinity illustrates the VM being able to be scheduled on all 

available cores as well as hyperthreads across the available physical CPUs 0 and 1. With this setting the thread configuration does not make a lot of sense: 

```
root@AHV:~$ virsh vcpuinfo 170d3af9-9978-4653-96aa-3e9b344ff68f
VCPU:           0
CPU:            3
State:          running
CPU time:       14.7s
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           1
CPU:            7
State:          running
CPU time:       6.7s
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           2
CPU:            4
State:          running
CPU time:       8.9s
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           3
CPU:            6
State:          running
CPU time:       6.9s
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

## numastat qemu-kvm indicates that the memory is mostly bound to NUMA 0 at this point. 

```
root@AHV:~$ numastat qemu-kvm
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
108154 (qemu-kvm)          7809.39          413.66         8223.05
139665 (qemu-kvm)          8215.54            7.44         8222.97
195825 (qemu-kvm)          8208.71           14.29         8222.99
225588 (qemu-kvm)             2.51        32813.60        32816.11
-----------------  --------------- --------------- ---------------
Total                     24236.14        33248.99        57485.12
```

Option 3: num_threads_per_core=2 and vcpu_hard_pin=true 

The below acli vm.get <vm_name> output snippet shows the configuration of the VM: 

```
num_cores_per_vcpu: 2
num_threads_per_core: 2
num_vcpus: 1
num_vnuma_nodes: 0
vcpu_hard_pin: True
```

Below snippet from virsh dumpxml <vm_id>  shows the configuration of 2 threads and vcpu_hard_pin being enabled: 

```
<vcpu placement='static' current='4'>240</vcpu>
  <cputune>
    <vcpupin vcpu='0' cpuset='0'/>    << -- real core
    <vcpupin vcpu='1' cpuset='28'/>   << -- hyperthread
    <vcpupin vcpu='2' cpuset='1'/>    << -- real core
    <vcpupin vcpu='3' cpuset='29'/>   << -- hyperthread
  </cputune>
<topology sockets='30' cores='2' threads='2'/>
```

Using virsh vcpuinfo <vm_uuid> illustrates the configuration is now being pinned to real cores and hyperthreads. 

Real physical cores: 0, 1 

- Hyperthreads: 28, 29 

```
root@AHV:~$ virsh vcpuinfo 170d3af9-9978-4653-96aa-3e9b344ff68f
VCPU:           0
CPU:            0
State:          running
CPU time:       4.2s
-------------------------------------------------------
CPU Affinity:   y
VCPU:           1
CPU:            28
State:          running
CPU time:       1.0s
---------------------------
CPU Affinity:   ----------------------------y
```

```
VCPU:           2
```

```
CPU:            1
State:          running
CPU time:       1.7s
------------------------------------------------------
CPU Affinity:   -y
VCPU:           3
CPU:            29
State:          running
CPU time:       1.8s
--------------------------
CPU Affinity:   -----------------------------y
```

## numastat qemu-kvm indicates that the memory is split between both NUMA nodes. 

```
root@AHV:~$ numastat qemu-kvm
```

```
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
108154 (qemu-kvm)          7809.39          413.66         8223.05
139665 (qemu-kvm)          8215.54            7.44         8222.97
214831 (qemu-kvm)          4129.12         4093.73         8222.85
225588 (qemu-kvm)             2.51        32813.60        32816.11
-----------------  --------------- --------------- ---------------
Total                     24236.14        33248.99        57485.12
```

## Option 4: num_vnuma_nodes=1 

The below acli vm.get <vm_name> output snippet shows the configuration of the VM: 

```
num_cores_per_vcpu: 4
num_threads_per_core: 1
num_vcpus: 1
num_vnuma_nodes: 1
vcpu_hard_pin: False
```

The placement is set to a given NUMA node (NUMA Node 1 in this case and both real cores + hyperthreads). The VM is not hard pinned to this NUMA node and could be placed on NUMA Node 0 once migrated to another host. 

```
<vcpu placement='static' current='4'>240</vcpu>
  <cputune>
```

```
    <vcpupin vcpu='0' cpuset='14-27,42-55'/>    << -- real core &
hyperthreaded cores on NUMA Node 1
    <vcpupin vcpu='1' cpuset='14-27,42-55'/>    << -- real core &
hyperthreaded cores on NUMA Node 1
    <vcpupin vcpu='2' cpuset='14-27,42-55'/>    << -- real core &
hyperthreaded cores on NUMA Node 1
    <vcpupin vcpu='3' cpuset='14-27,42-55'/>    << -- real core &
hyperthreaded cores on NUMA Node 1
  </cputune>
<topology sockets='60' cores='4' threads='1'/>
```

Using virsh vcpuinfo <vm_uuid> illustrates the configuration is now being pinned real cores and hyperthreads on NUMA node 1. 

```
root@AHV:~$ virsh vcpuinfo 170d3af9-9978-4653-96aa-3e9b344ff68f
VCPU:           0
CPU:            23
State:          running
CPU time:       7.0s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           1
CPU:            27
State:          running
CPU time:       3.9s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           2
CPU:            24
State:          running
CPU time:       3.4s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           3
CPU:            26
State:          running
CPU time:       3.5s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

numastat qemu-kvm indicates that the memory is pinned to a given NUMA node. 

```
root@AHV:~$ numastat qemu-kvm
```

```
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
108154 (qemu-kvm)          7809.39          413.66         8223.05
139665 (qemu-kvm)          8215.54            7.44         8222.97
219943 (qemu-kvm)             9.52         8213.38         8222.89
225588 (qemu-kvm)             2.51        32813.60        32816.11
-----------------  --------------- --------------- ---------------
Total                     24236.14        33248.99        57485.12
```

## Option 5: num_vnuma_nodes=2 

The below acli vm.get <vm_name> output snippet shows the configuration of the VM: 

```
num_cores_per_vcpu: 4
num_threads_per_core: 1
num_vcpus: 1
num_vnuma_nodes: 2
vcpu_hard_pin: False
```

The placement is set to a given NUMA node (NUMA node 0 and 1 in this case and both real cores + hyperthreads). The VM is not hard pinned to these NUMA nodes. 

```
<vcpu placement='static' current='4'>240</vcpu>
  <cputune>
    <vcpupin vcpu='0' cpuset='0-13,28-41'/>    << -- real core & hyperthreaded
cores on NUMA Node 0
```

```
    <vcpupin vcpu='1' cpuset='0-13,28-41'/>    << -- real core & hyperthreaded
cores on NUMA Node 0
```

```
    <vcpupin vcpu='2' cpuset='14-27,42-55'/>   << -- real core & hyperthreaded
cores on NUMA Node 1
```

```
    <vcpupin vcpu='3' cpuset='14-27,42-55'/>   << -- real core & hyperthreaded
cores on NUMA Node 1
```

```
  </cputune>
 <numatune>
  <memnode cellid='0' mode='strict' nodeset='0'/>
  <memnode cellid='1' mode='strict' nodeset='1'/>
</numatune>
```

```
<topology sockets='60' cores='4' threads='1'/>
```

Using virsh vcpuinfo <vm_uuid> illustrates the configuration is now using both real cores and hyperthreads on NUMA Node 0 and 1. 

```
root@AHV:~$ virsh vcpuinfo 170d3af9-9978-4653-96aa-3e9b344ff68f
VCPU:           0
CPU:            10
State:          running
CPU time:       7.0s
----------------------------
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           1
CPU:            11
State:          running
CPU time:       3.9s
----------------------------
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           2
CPU:            17
State:          running
CPU time:       3.5s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           3
CPU:            22
State:          running
CPU time:       3.7s
--------------
CPU Affinity:   --------------yyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

## numastat qemu-kvm indicates that the memory is pinned to a given NUMA node. 

```
root@AHV:~$ numastat qemu-kvm
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
108154 (qemu-kvm)          7809.39          413.66         8223.05
139665 (qemu-kvm)          8215.54            7.44         8222.97
225588 (qemu-kvm)             2.51        32813.60        32816.11
229399 (qemu-kvm)          4106.07         4116.65         8222.72
-----------------  --------------- --------------- ---------------
Total                     24236.14        33248.99        57485.12
```

## Option 6: num_vnuma_nodes=2 & vcpu_hard_pin=true 

The below acli vm.get <vm_name> output snippet shows the configuration of the VM: 

```
num_cores_per_vcpu: 4
num_threads_per_core: 1
num_vcpus: 1
num_vnuma_nodes: 2
vcpu_hard_pin: True
```

The placement is set to a given NUMA node (NUMA node 0 and 1 in this case and only real cores due to the hard pinning): 

```
<vcpu placement='static' current='4'>240</vcpu>
  <cputune>
```

```
    <vcpupin vcpu='0' cpuset='0'/>    << -- hard pinned real core on NUMA Node
0
    <vcpupin vcpu='1' cpuset='1'/>    << -- hard pinned real core on NUMA Node
0
    <vcpupin vcpu='2' cpuset='14'/>   << -- hard pinned real core on NUMA Node
1
    <vcpupin vcpu='3' cpuset='15'/>   << -- hard pinned real core on NUMA Node
1
  </cputune>
  <numatune>
    <memnode cellid='0' mode='strict' nodeset='0'/>
    <memnode cellid='1' mode='strict' nodeset='1'/>
  </numatune>
<topology sockets='60' cores='4' threads='1'/>
```

virsh vcpuinfo <vm_uuid> illustrates the configuration using real cores pinned on NUMA node 0 and 1. 

## `root@AHV:~$ virsh vcpuinfo 170d3af9-9978-4653-96aa-3e9b344ff68f` 

```
VCPU:           0
CPU:            0
State:          running
CPU time:       9.8s
-------------------------------------------------------
CPU Affinity:   y
VCPU:           1
CPU:            1
```

```
State:          running
CPU time:       2.3s
------------------------------------------------------
CPU Affinity:   -y
VCPU:           2
CPU:            14
State:          running
CPU time:       2.7s
-----------------------------------------
CPU Affinity:   --------------y
VCPU:           3
CPU:            15
State:          running
CPU time:       3.3s
----------------------------------------
CPU Affinity:   ---------------y
```

numastat qemu-kvm indicates that the memory is pinned to a given NUMA node. 

```
root@AHV:~$ numastat qemu-kvm
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
108154 (qemu-kvm)          7809.39          413.66         8223.05
139665 (qemu-kvm)          8215.54            7.44         8222.97
225588 (qemu-kvm)             2.51        32813.60        32816.11
233918 (qemu-kvm)          4114.39         4108.62         8223.02
-----------------  --------------- --------------- ---------------
Total                     24236.14        33248.99        57485.12
```

## Option 7: extra_flags=numa_pinning=0/1 

The below acli vm.get <vm_name> output snippet shows the configuration of the VM: 

```
num_cores_per_vcpu: 4
num_threads_per_core: 1
num_vcpus: 1
num_vnuma_nodes: 0
vcpu_hard_pin: False
extra_flags {
  key: "numa_pinning"
```

```
  value: "0"
}
```

The placement is hard pinned to a given NUMA node (NUMA node 0 in this case) and all its real cores and hyperthreads: 

```
<vcpu placement='static' cpuset='0-13,28-41' current='4'>240</vcpu>
```

```
<topology sockets='60' cores='4' threads='1'/>
```

virsh vcpuinfo <vm_uuid> illustrates the configuration using all cores on NUMA node 0. If VMs will get moved to other hosts the VM will always stay on the configured NUMA node. 

```
root@AHV:~$ virsh vcpuinfo 170d3af9-9978-4653-96aa-3e9b344ff68f
VCPU:           0
CPU:            11
State:          running
CPU time:       8.0s
----------------------------
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           1
CPU:            12
State:          running
CPU time:       6.1s
----------------------------
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           2
CPU:            1
State:          running
CPU time:       5.6s
----------------------------
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyy
VCPU:           3
CPU:            13
State:          running
CPU time:       5.5s
----------------------------
CPU Affinity:   yyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

numastat qemu-kvm indicates that the memory is pinned to a given NUMA node. 

```
root@AHV:~$ numastat qemu-kvm
```

```
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
23161 (qemu-kvm)           8215.82            7.42         8223.24
108154 (qemu-kvm)          7809.39          413.66         8223.05
139665 (qemu-kvm)          8215.54            7.44         8222.97
225588 (qemu-kvm)             2.51        32813.60        32816.11
-----------------  --------------- --------------- ---------------
Total                     24236.14        33248.99        57485.12
```

## Option 8: extra_flags=numa_pinning=0 and vcpu_hard_pin: True 

Note: This option is only working when the VM is pinned to NUMA Node 0 so only in cases where the CVM is pinned to NUMA Node 1. It is not possible to pin the User VM on NUMA Node 1 with vcpu_hard_pin. 

The below acli vm.get <vm_name> output snippet shows the configuration of the VM. 

```
num_cores_per_vcpu: 4
num_threads_per_core: 1
num_vcpus: 1
num_vnuma_nodes: 0
vcpu_hard_pin: True
extra_flags {
  key: "numa_pinning"
  value: "0"
}
```

The placement is hard pinned to a given NUMA node (NUMA node 0 in this case) and first hard pinned 4 cores. 

```
<vcpu placement='static' cpuset='0-13,28-41' current='4'>240</vcpu>
 <cputune>
   <vcpupin vcpu='0' cpuset='0'/>
   <vcpupin vcpu='1' cpuset='1'/>
   <vcpupin vcpu='2' cpuset='2'/>
   <vcpupin vcpu='3' cpuset='3'/>
 </cputune>
```

```
<topology sockets='60' cores='4' threads='1'/>
```

virsh vcpuinfo <vm_uuid> illustrates the configuration using hard pinned cores on NUMA node 0. Also if the VMs will get moved across other hosts the VM will always stay on the configured NUMA node. It should be kept in mind that multiple hard pinned VMs will be placed on the same static physical cores. 

```
root@AHV:~$ virsh vcpuinfo 170d3af9-9978-4653-96aa-3e9b344ff68f
VCPU:           0
CPU:            0
State:          running
CPU time:       8.3s
-------------------------------------------------------
CPU Affinity:   y
VCPU:           1
CPU:            1
State:          running
CPU time:       5.4s
------------------------------------------------------
CPU Affinity:   -y
VCPU:           2
CPU:            2
State:          running
CPU time:       4.8s
-----------------------------------------------------
CPU Affinity:   --y
VCPU:           3
CPU:            3
State:          running
CPU time:       5.6s
----------------------------------------------------
CPU Affinity:   ---y
```

## numastat qemu-kvm indicates that the memory is pinned to a given NUMA node. 

```
root@AHV:~$ numastat qemu-kvm
Per-node process memory usage (in MBs)
PID                         Node 0          Node 1           Total
-----------------  --------------- --------------- ---------------
35476 (qemu-kvm)           8215.56            7.52         8223.07
108154 (qemu-kvm)          7809.39          413.66         8223.05
139665 (qemu-kvm)          8215.54            7.44         8222.97
225588 (qemu-kvm)             2.51        32813.60        32816.11
-----------------  --------------- --------------- ---------------
Total                     24236.14        33248.99        57485.12
```

**==> picture [595 x 57] intentionally omitted <==**

