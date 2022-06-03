<div class="innerCell">

# **<span id="DeltaXSEDEDocumentation-top" class="confluence-anchor-link conf-macro output-inline" hasbody="false" macro-name="anchor"> </span>Delta User Guide**

*Last update: April 7, 2022*

# **Status Updates and Notices**

*Delta* is tentatively scheduled to enter production in Q2 2022.

<div
class="confluence-information-macro confluence-information-macro-information conf-macro output-block"
hasbody="true" macro-name="info">

key: light grey font is work in progress

<span
class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon">
</span>

<div class="confluence-information-macro-body">

Items in light grey font are in progress and coming soon.  <span
style="color: rgb(165,173,186);">Example software or feature not yet
implemented.</span>

</div>

</div>

# **Introduction**

*Delta* is a dedicated,
<a href="http://www.xsede.org" class="external-link" rel="nofollow"
target="_blank">eXtreme Science and Engineering Science Discovery
Environment (XSEDE)</a> allocated resource designed by HPE and NCSA,
delivering a highly capable GPU-focused compute environment for GPU and
CPU workloads.  Besides offering a mix of standard and reduced precision
GPU resources, *Delta* also offers GPU-dense nodes with both NVIDIA and
AMD GPUs.  *Delta* provides high performance node-local SSD scratch
filesystems, as well as both standard lustre and <span
style="color: rgb(165,173,186);">relaxed-POSIX</span> parallel
filesystems spanning the entire resource.

*Delta's* CPU nodes are each powered by two 64-core AMD EPYC 7763
("Milan") processors, with 256 GB of DDR4 memory.  The *Delta* GPU
resource has four node types: one with 4 NVIDIA A100 GPUs (40 GB HBM2
RAM each) connected via NVLINK and 1 64-core AMD EPYC 7763 ("Milan")
processor, the second with 4 NVIDIA A40 GPUs (48 GB GDDR6 RAM) connected
via PCIe 4.0 and 1 64-core AMD EPYC 7763 ("Milan") processor, the third
with 8 NVIDIA A100 GPUs in a dual socket AMD EPYC 7763 (128-cores per
node) node with 2 TB of DDR4 RAM and NVLINK,  and the fourth with 8 AMD
MI100 GPUs (32GB HBM2 RAM each) in a dual socket AMD EPYC 7763
(128-cores per node) node with 2 TB of DDR4 RAM and PCIe 4.0. 

*Delta* has 124 standard CPU nodes, 100 4-way A100-based GPU nodes, 100
4-way A40-based GPU nodes, 5 8-way A100-based GPU nodes, and 1 8-way
MI100-based GPU node.  Every *Delta* node has high-performance
node-local SSD storage (740 GB for CPU nodes, 1.5 TB for GPU nodes), and
is connected to the 7 PB Lustre parallel filesystem via the high-speed
interconnect.  The *Delta* resource uses the SLURM workload manager for
job scheduling.  

Delta supports the
<a href="https://www.xsede.org/software" class="external-link"
rel="nofollow" target="_blank">XSEDE core software stack</a>, including
remote login, remote computation, data movement, science workflow
support, and science gateway support toolkits.

  

**<span
class="confluence-embedded-file-wrapper confluence-embedded-manual-size"><img
src="/download/attachments/144016994/image2021-3-12_14-23-11.png?version=1&amp;modificationDate=1615925873000&amp;api=v2"
class="confluence-embedded-image"
data-image-src="/download/attachments/144016994/image2021-3-12_14-23-11.png?version=1&amp;modificationDate=1615925873000&amp;api=v2"
data-unresolved-comment-count="0" data-linked-resource-id="144016995"
data-linked-resource-version="1" data-linked-resource-type="attachment"
data-linked-resource-default-alias="image2021-3-12_14-23-11.png"
data-base-url="https://wiki.ncsa.illinois.edu"
data-linked-resource-content-type="image/png"
data-linked-resource-container-id="144016994"
data-linked-resource-container-version="177" height="250" /></span>**

**Figure 1. Delta System**

Delta is supported by the National Science Foundation under Grant No.
OAC-<span style="color: rgb(0,0,0);">2005572</span>.

Any opinions, findings, and conclusions or recommendations expressed in
this material are those of the author(s) and do not necessarily reflect
the views of the National Science Foundation.

<div class="table-wrap">

|                                     |
|:-----------------------------------:|
| *Delta* is now accepting proposals. |

</div>

[Top of Page](#DeltaXSEDEDocumentation-top)

# **Account Administration**

-   For XSEDE projects please use the
    <a href="https://portal.xsede.org/my-xsede" class="external-link"
    rel="nofollow" target="_blank">XSEDE user portal</a> for project and
    account management.
-   Non-XSEDE Account and Project administration is handled by NCSA
    Identity and NCSA group management tools. For more information
    please see the [NCSA Allocation and Account
    Management](/display/USSPPRT/User+Services+NCSA+Allocation+and+Account+Management)
    documentation page. 

## **Configuring Your Account**

-   bash is the default shell, submit a support request to change your
    default shell
-   environment variables: <span style="color: rgb(165,173,186);">XSEDE
    CUE</span>, [SLURM
    batch](#DeltaXSEDEDocumentation-slurm_environment_batch)  
-   using [Modules](#DeltaXSEDEDocumentation-lmod) 

# **System Architecture**

<span style="color: rgb(0,0,0);">Delta is designed to help applications
transition from CPU-only to GPU or hybrid CPU-GPU codes. Delta has some
important architectural features to facilitate new discovery and
insight:</span>

-   <span style="color: rgb(0,0,0);">a single processor architecture
    (AMD) across all node types: CPU and GPU</span>
-   <span style="color: rgb(17,17,17);">support for NVIDIA A100 MIG GPU
    partitioning allowing for fractional use of the A100s if your
    workload isn't able to exploit an entire A100 efficiently</span>
-   <span style="color: rgb(0,0,0);">ray tracing hardware support from
    the NVIDIA A40 GPUs</span>
-   <span style="color: rgb(0,0,0);">9 large memory (2 TB) nodes </span>
-   <span style="color: rgb(0,0,0);">a low latency and high bandwidth
    HPE/Cray Slingshot interconnect between compute nodes</span>
-   <span style="color: rgb(0,0,0);">lustre for home, projects and
    scratch file systems</span>
-   <span style="color: rgb(165,173,186);">support for relaxed and
    non-posix IO</span>
-   <span style="color: rgb(17,17,17);">shared-node jobs and the single
    core and single MIG GPU slice</span>
-   <span style="color: rgb(17,17,17);">Resources for persistent
    services in support of Gateways, Open OnDemand, Data Transport
    nodes..., </span>
-   <span style="color: rgb(0,0,0);">Unique AMD MI-100 resource</span>  

## **Model Compute Nodes**

<span style="color: rgb(0,0,0);">The Delta compute ecosystem is composed
of 5 node types: dual-socket CPU-only compute nodes, single socket 4-way
NVIDIA A100 GPU compute nodes, single socket 4-way NVIDIA A40 GPU
compute nodes, dual-socket 8-way NVIDIA A100 GPU compute nodes, and a
single socket 8-way AMD MI100 GPU compute nodes. The CPU-only and 4-way
GPU nodes have 256 GB of RAM per node while the 8-way GPU nodes have 2
TB of RAM. The CPU-only node has 0.74 TB of local storage while all GPU
nodes have 1.5 TB of local storage.</span>

### Table. CPU Compute Node Specifications

<div class="table-wrap">

<table class="wrapped confluenceTable tablesorter tablesorter-default"
role="grid" data-resolved="">
<thead>
<tr class="header tablesorter-headerRow" role="row">
<th
class="confluenceTh tablesorter-header sortableHeader tablesorter-headerUnSorted"
data-column="0" tabindex="0" scope="col" role="columnheader"
aria-disabled="false" data-unselectable="on" aria-sort="none"
aria-label="Specification: No sort applied, activate to apply an ascending sort"
style="user-select: none"><div class="tablesorter-header-inner">
Specification
</div></th>
<th
class="confluenceTh tablesorter-header sortableHeader tablesorter-headerUnSorted"
data-column="1" tabindex="0" scope="col" role="columnheader"
aria-disabled="false" data-unselectable="on" aria-sort="none"
aria-label="Value: No sort applied, activate to apply an ascending sort"
style="user-select: none"><div class="tablesorter-header-inner">
Value
</div></th>
</tr>
</thead>
<tbody aria-live="polite" aria-relevant="all">
<tr class="odd" role="row">
<td class="confluenceTd"><p><span>Number of nodes</span></p></td>
<td class="confluenceTd"><p><span>124</span></p></td>
</tr>
<tr class="even" role="row">
<td class="confluenceTd"><span>CPU</span></td>
<td class="confluenceTd"><span><span>AMD</span></span> EPYC
7763<span><span><br />
</span></span><span>"Milan" (PCIe Gen4)</span></td>
</tr>
<tr class="odd" role="row">
<td class="confluenceTd">Sockets per node</td>
<td class="confluenceTd">2</td>
</tr>
<tr class="even" role="row">
<td class="confluenceTd"><p><span>Cores per socket</span></p></td>
<td class="confluenceTd"><p><span>64</span></p></td>
</tr>
<tr class="odd" role="row">
<td class="confluenceTd">Cores per node</td>
<td class="confluenceTd">128</td>
</tr>
<tr class="even" role="row">
<td class="confluenceTd"><p><span>Hardware threads per
core</span></p></td>
<td class="confluenceTd">1</td>
</tr>
<tr class="odd" role="row">
<td class="confluenceTd"><p><span>Hardware threads per
node</span></p></td>
<td class="confluenceTd"><p><span>128</span></p></td>
</tr>
<tr class="even" role="row">
<td class="confluenceTd"><p><span>Clock rate (GHz)</span></p></td>
<td class="confluenceTd"><p><span>~ 2.45</span></p></td>
</tr>
<tr class="odd" role="row">
<td class="confluenceTd"><p><span>RAM (GB)</span></p></td>
<td class="confluenceTd"><p><span>256</span></p></td>
</tr>
<tr class="even" role="row">
<td class="confluenceTd"><p><span>Cache (KB) L1/L2/L3</span></p></td>
<td class="confluenceTd"><p><span> 64/512/32768</span></p></td>
</tr>
<tr class="odd" role="row">
<td class="confluenceTd"><p><span>Local storage (TB)</span></p></td>
<td class="confluenceTd"><p><span>0.74 TB</span></p></td>
</tr>
</tbody>
</table>

</div>

The AMD CPUs are set for 4 NUMA domains per socket (NPS=4). 

### Table. 4-way NVIDIA A40 GPU Compute Node Specifications 

<div class="table-wrap">

<div class="tablesorter-header-inner">

Specification

</div>

</div>

</div>

<div class="tablesorter-header-inner">

Value

</div>

Number of nodes

100

GPU

NVIDIA A40 

(<a href="https://www.nvidia.com/en-us/data-center/a40/#specs"
class="external-link" rel="nofollow" target="_blank">Vendor page</a>)

GPUs per node

4

GPU Memory (GB)

48 DDR6 with ECC

CPU

AMD Milan

CPU sockets per node

1

Cores per socket

64

Cores per node

64

Hardware threads per core

1

Hardware threads per node

64

Clock rate (GHz)

\~ 2.45

RAM (GB)

256

Cache (KB) L1/L2/L3

&nbsng affinitization to NUMA nodes on the CPU. Note that the
relationship between GPU index and NUMA domain are inverse.

Table. 4-way NVIDIA A40 Mapping and GPU-CPU AffeTd"\>GPU1

SYS

X

SYS

SYS

SYS

32-47

  

Table Legend

<span class="s1">X    = Self  
</span><span class="s1">SYS  = ConneaderRow"\></span>

NVIDIA A100

(<a href="https://www.nvidia.com/en-us/data-center/a100/#specifications"
class="external-link" rel="nofollow" target="_blank">Vendor page</a>)

fluenceTd"\>1

Hardware threads per node

64

Clock rate (GHz)

\~ 2.45

  

GPU0

GPU1

GPU2

GPU3

HSN

CPU Affinity

NUMA Affinity

GPU0

X

NV4

X

NV4

SYS

16-31

1

GPU3

NV4

NV4

NV4

X

PHB

0-15

PHB  = Connection traversing PCIe as well as a PCIe Host Bridge
(typically the CPU)  

Specification

GPU Memory (GB)

40 

CPU

AMD Milan

RAM (GB)

2,048

Cache (KB) L1/L2/L3

 64/512/32768

GPU5

GPU6

GPU7

HSN

3

GPU2

NV12

NV12

X

X

NV12

NV12

NV12

5

GPU3

NV12

NODE = Connection traversing Pication: No sort applied, activate to
apply an ascending sort" style="user-select: none;"\>

<div class="confluenceTd" 1"="">

GPUs per node

</div>

8

GPU Memory (GB)

32

CPU

\~ 2.45

RAM (GB)

**Network**

Delta is connected to the nd it has 6PB of usable space.  These file
systems run Lustre via DDN's ExaScaler 6 stack (Lustre 2.14 based).

<u>Hardware:  
</u>DDN SFA7990XE (Quantity: 3), each unit contains

-   One additional SS9012 enclosure
-   168 x 16TB SAS Drives
-   7 x 1.92TB SAS SSDs

The $HOME file system has ort applied, activate to apply an ascending
sort" style="user-select: none;"\>

<div class="tablesorter-header-inner">

File size

</div>

<u>Hardware:  
</u>DDN SFA400NVXE (Quanup\>

<div class="tables" aria-relevant="all">

$HOME

</div>

**25GB. **400,000 files per u="1" class="confluenceTd"\>No

Yes; files older than 30-days (access time)

Area for computation, largest allocEDocumentation-login_nodes"
data-hasbody="false" data-macro-name="anchor"\>

Direct access to the Delta login nodene" aria-label="example usage with
ssh: No sort applied, activate to apply an ascending sort"
style="user-select: none;"\>

<div class="tablesorter-header-inner">

eedu

</div>

<span style="letter-spacing: 0.0px;">If needed, XSEDE users can lookup
their local username at
</span><a href="https://portal.xsede.org/group/xup/accounts"
class="external-link" style="letter-spacing: 0.0px;" data-re"true"=""
data-macro-name="info"></a>

maintaining persistent sessions: tmux

<span
class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon">
</span>

<div class="confluence-information-macro-body">

tmux is available on the login nodes to maintain persistent sessse
execute the gsissh command with the “-vvv” option and include the
verbose output in your problem description.

Once on the XSEDE SSO hub:

<div class="coder sh-confluence nogutter java">

<div class="toolbar">

<a href="#"
class="toolbar_item cthe system affects others. Exercise good citizenship to ensure that your activity does not advee the following entries in their &lt;code&gt;$HOME&lt;/code&gt; and entries in the project and scratch file systems. T&lt;/span&gt;o depan&gt;&lt;/div&gt;&lt;table border="
data-0"="" data-cellpadding="0" data-cellspacing="0"></a>

<div class="container" title="Hint: double-click to select code">

</div>

<div class="line number6 index5 alt1">

 

</div>

<div class="line number7 index6 alt2">

`$ `nbsp;  delta_abcd      4096 Feb 21 11:54 `/scratch/abcd`

</div>

<div class="line number15 i </p><p><br></p><div class="
confluence-information-macro=""
confluence-information-macro-information="" conf-macro="" outpu=""
usability="" of="" the="" delta="" login="" node.&nbsp;<="" span="">

rsync - to be used for small to modest transfers to avoid impacting the
usability of the Delta login node. 

-   Sharing Files with Collaborators

    Building Software. 

    <span
    class="confluence-embedded-file-wrapper conf-macrD EPYC 7xx3 Series Processors.pdf">![](/rest/documentConversion/latest/conversion/thumbnail/179671355%20apply%20an%20ascending%20sort)</span>

    <div class="tablesorter-header-inner">

    gcc

    </div>

-   <span style="color: rgb(165,173,186);">Launching One Hybrid
    (MPI+Threads) Application</span>

-   More Than One Serial Applicati\>Keep in mind that your charges are
    based on the resources that are reserved de

## **Accessing the Compute Nodes**

Deltamitting<span style="letter-spacing: 0.0px;"> for detail number2
index1 alt1"\>`             `</span>

<div class="codeContent panelContent pdl">

<div>

`252` `GB`

</div>

<div class="tablesorter-headerRow" linked-resource-id="179671305"
linked-resource-type="attachment"
linked-resource-container-id="1p&gt;&lt;thead&gt;&lt;tr role=" row"="">

<span style="color: rgb(165,173,186);">TBD</span>

</div>

</div>

</div>

</div>

</div>

</div>

30 min<span style="color: rgb(165,173,186);">30 min</span>

TBD<span style="color: rgb(165,173,186);">TBD</span>

<span style="color: rgb(165,173,186);">2.0</span>

1.n-sviewviewofslurmpartitions"\>sview view of slurm partitions

<a href="#" class="toolbar_item command_help help">?</a>

single core class="java plain"\>--tasks=`1` `\`

<div class="line number3 index2 alt2">

`  ``--tasks-per-node=``1` `--cpus-per-task=``1` applications from
within them.  Use <u>*mpirun*</u> to launch mpi jobs from within an
interactive job.  Within standard batch jobs submitted via sbatch, use
<u>srun</u> to launch MPI codes.

</div>

### Interactive X11 Support

To run an X11 based application on a compute node in an interactive
session, the use of the `--x11` switch with `srun` is needed. For
example, to run a single core job that uses 1g of memory with X11 (in
this" title="Hint: double-click to select code"\>

<div class="line number1 index0 alt2">

`srun -A abcd-delta-cpu  --partition=cpu \`

</div>

<div class="line number2 index1 alt1">

`  ``--nodes=``1` `--tasks=``1` `--tasks-per-node=`` `

-   Serial jobs

    <div class="code panel pdl conf-macro output-block"
    style="border-width: 1px;" hasbody="true" macro-name="code">

    <div class="codeHeader panelHeader pdl"
    style="border-bottom-width: 1px;">

    **serial example script**`#!/bin/bash`

    </div>

    <div class="line number3 index2 alt2">

    `#SBATCH --mem=16g`

    </div>

    <div class="line number4 index3 alt1">

    `#SBATCH --nodes=1`

    </div>

    <div class="line number5 index4 alt2">

    `#SBATCH --ntasks-per-node=1`

    </div>

    <div class="line number6 index5 alt1">

    `#SBATCH --cpus-per-task=1    # <- match to OMP_NUM_THREADS`

    </div>

    <div class="line number7 index6 alt2">

    `#SBA      # hh:mm:ss for the job`

    </div>

    <div class="line number11 index10 alt2">

    `### GPU options ###`

    </div>

    <div class="line number12 index11 alt1">

    `##SBATCH --gpus-per-node=2`

    </div>

    <div class="line number13 index12 alt2">

    `##SBATCH --gpu-bind=none     # <- or closest`

    </div>

    <div class="line number14 index13 alt1">

    `##SBATCH --mail-user=you@yourinstitution.edu `

    </div>

    <div class="line number15 index14 alt2">

    `##SBATCH --mail-type="BEGIN,END" See sbatch or srun man pages for more email options  `

    </div>

    <div class="line number16 index15 alt1">

     

    </div>

    <div class="line number17 index16 alt2">

     

    </div>

    <div class="line index18 alt2">

    `             ``# (good job metadata and reproducibility)`

    </div>

    <div class="line number20 index19 alt1">

    `             ``# $WORK and $SCRATCH are notd>`
    MPI  

    <div class="code panel pdl conf-macro output-block"
    style="border-width: 1px;" hasbody="true" macro-name="code">

    <div class="codeHeader panelHeader pdl hide-border-bottom"
    style="border-bottom-width: 1px;">

    **mpi example script** <span clat:="" double-click="" to=""
    select="" code"=""></span>
    <div class="line number1 index0 alt2">

    `#!/bin/bash`

    </div>

    <div class="line number2 index1 alt1">

    `#SBATCH --mem=16g`

    </div>

    `#SBATCH --time=00:10:00      # hh:mm:ss for the job`

    </div>

    <div class="line number10 index9 alt1">

    `### GPU options ###`

    </div>

    <div class="line number11 index10 alt2">

    `##SBATCH --gpus-per-node=2`

    </div>

    <div class="line number12 index11 alt1">

    `##SBATCH --gpu-bind=none     # <- or closest ##SBATCH --mp;      ``# (good job metadata and reproducibility)`

    </div>

    <div class="line number17 index16 alt2">

    `             ``# $WORK and $SCRATCH are now set`

    </div>

    <div class="line number18 index17 alt1">

    `module load gcc``/11``.2.0 openmpi  ``# ... or any appropriate modules`

    </div>

    <div
    class="line numbody></table></div></div> </div> </div></li><li><p class="
    auto-cursor-target"="">

    OpenMP  <span style="color: rgb(255,204,153);"> </span>
    <div class="code panel pdl conf-macro output-block"
    style="border-width: 1px;" hasbody="true" macro-name="code">

    <div class="codeHeader panelHeader pdl hide-border-bottom"
    style="border-bottom-width: 1px;">

    **openmp example script** <span
    class="collapse-source expand-control" style=""><span
    class="expand-control-icon icon"> </span><span
    class="expand-control-text">Expand sourcetd
    class="code"\></span></span>
    <div class="container" title="Hint: double-click to select code">

    <div class="line number1 index0 alt2">

    `#!/bin/bash`

    </div>

    <div class="line number2 index1 alt1">

    `#SBATCH --mem=16g`

    </div>

    <div class="line number3 index2 alt2">

    `#SBATCH --nodes=1`

    </div>

    <div class="line number4 index3 alt1">

    `#SBATCH --ntasks-per-node=1`
    <div class="line number9 index8 alt2">

    `#SBATCH --time=00:10:00      # hh:mm:ss for the job`

    </div>

    <div class="line number10 index9 alt1">

    `### GPU options ###`

    </div>

    <div class="line number11 index10 alt2">

    `##SBATCH --gpus-per-node=2`

    </div>

    <div class="line number12 index11 alt1">

    `##SBATCH --gpu-bind=none     # <- o needed`

    </div>

    <div class="line number17 index16 alt2">

    `             ``# (good job metadata and reproducibility)`

    </div>

    <div class="line number18 index17 alt1">

    `             ``# $WORK and $SCRATCH are now set`

    </div>

    <div class="line number19 index18 alt2">

    `mo/div>`
    <div class="line number22 index21 alt1">

    `export` `OMP_NUM_THREADS=32`

    </div>

    <div class="line number23 index22 alt2">

    `srun stream_gcc`

    </div>

    </div>

    </div>

    </div>

    </div>

    </div>

    </div>

    </div>

    </div>

    </div>

-   <span style="color: rgb(165,173,186);">Parametric / Array / HTC
    jobs</span>

# **Job Management **

Batch jobs are submitted through a *job script*  (as in the examples
above) using the sbatch command. Job scripts generally start with a
series of SLURM *directives* that describe requirements of the job such
as number of nodes, wall time required, etc… to the batch
system/scheduler (SLURM directives can also be specified as options on
the sbatch command line; command line options take precedence over those
in the script). The rest of the batch script consists of user commands.

The syntax for sbatch is:

**sbatch** \[list of sbatch options\] script_name

Refer to the sbatch man page for detailed information on the options.

#### squeue/scontrol/sinfo

Commands that display batch job and partition information .

<div class="table-wrap">

<table class="wrapped confluenceTable tablesorter tablesorter-default"
role="grid" data-resolved="">
<thead>
<tr class="header tablesorter-headerRow" role="row">
<th style="text-align: center;"
class="confluenceTh tablesorter-header sortableHeader tablesorter-headerUnSorted"
style="user-select: none" data-column="0" tabindex="0" scope="col"
role="columnheader" aria-disabled="false" data-unselectable="on"
aria-sort="none"
aria-label="SLURM EXAMPLE COMMAND: No sort applied, activate to apply an ascending sort"><div
class="tablesorter-header-inner">
SLURM EXAMPLE COMMAND
</div></th>
<th style="text-align: center;"
class="confluenceTh tablesorter-header sortableHeader tablesorter-headerUnSorted"
style="user-select: none" data-column="1" tabindex="0" scope="col"
role="columnheader" aria-disabled="false" data-unselectable="on"
aria-sort="none"
aria-label="DESCRIPTION: No sort applied, activate to apply an ascending sort"><div
class="tablesorter-header-inner">
DESCRIPTION
</div></th>
</tr>
</thead>
<tbody aria-live="polite" aria-relevant="all">
<tr class="odd" role="row">
<td class="confluenceTd"><span style="color: rgb(0,0,0);">squeue
-a</span></td>
<td class="confluenceTd">List the status of all jobs on the system.</td>
</tr>
<tr class="even" role="row">
<td class="confluenceTd"><span style="color: rgb(0,0,0);">squeue -u
$USER</span></td>
<td class="confluenceTd">List the status of all your jobs in the batch
system.</td>
</tr>
<tr class="odd" role="row">
<td class="confluenceTd"><span style="color: rgb(0,0,0);">squeue -j
JobID</span></td>
<td class="confluenceTd">List nodes allocated to a running job in
addition to basic information..</td>
</tr>
<tr class="even" role="row">
<td class="confluenceTd"><span style="color: rgb(0,0,0);">scontrol show
job JobID</span></td>
<td class="confluenceTd">List detailed information on a particular
job.</td>
</tr>
<tr class="odd" role="row">
<td class="confluenceTd"><span style="color: rgb(0,0,0);">sinfo
-a</span></td>
<td class="confluenceTd">List summary information on all the
partition.</td>
</tr>
</tbody>
</table>

</div>

See the manual (man) pages for other available options.

  

Useful Batch Job Environment Variables<span
id="DeltaXSEDEDocumentation-slurm_environment_variables"
class="confluence-anchor-link conf-macro output-inline" hasbody="false"
macro-name="anchor"> </span>

<div class="table-wrap">

<table
class="relative-table wrapped confluenceTable tablesorter tablesorter-default"
style="width: 100.0%;" role="grid" data-resolved="">
<colgroup>
<col style="width: 12%" />
<col style="width: 16%" />
<col style="width: 53%" />
</colgroup>
<thead>
<tr class="header tablesorter-headerRow" role="row">
<th style="text-align: left;"
class="confluenceTh tablesorter-header sortableHeader tablesorter-headerUnSorted"
style="user-select: none" data-column="0" tabindex="0" scope="col"
role="columnheader" aria-disabled="false" data-unselectable="on"
aria-sort="none"
aria-label="DESCRIPTION: No sort applied, activate to apply an ascending sort"><div
class="tablesorter-header-inner">
<p>DESCRIPTION</p>
</div></th>
<th style="text-align: left;"
class="confluenceTh tablesorter-header sortableHeader tablesorter-headerUnSorted"
style="user-select: none" data-column="1" tabindex="0" scope="col"
role="columnheader" aria-disabled="false" data-unselectable="on"
aria-sort="none"
aria-label="SLURM ENVIRONMENT VARIABLE: No sort applied, activate to apply an ascending sort"><div
class="tablesorter-header-inner">
<p>SLURM ENVIRONMENT VARIABLE</p>
</div></th>
<th style="text-align: left;"
class="confluenceTh tablesorter-header sortableHeader tablesorter-headerUnSorted"
style="user-select: none" data-column="2" tabindex="0" scope="col"
role="columnheader" aria-disabled="false" data-unselectable="on"
aria-sort="none"
aria-label="DETAIL DESCRIPTION: No sort applied, activate to apply an ascending sort"><div
class="tablesorter-header-inner">
<p>DETAIL DESCRIPTION</p>
</div></th>
</tr>
</thead>
<tbody aria-live="polite" aria-relevant="all">
<tr class="odd" role="row">
<td style="text-align: left;" class="confluenceTd">JobID</td>
<td style="text-align: left;" class="confluenceTd">$SLURM_JOB_ID</td>
<td style="text-align: left;" class="confluenceTd">Job identifier
assigned to the job</td>
</tr>
<tr class="even" role="row">
<td style="text-align: left;" class="confluenceTd">Job Submission
Directory</td>
<td style="text-align: left;"
class="confluenceTd">$SLURM_SUBMIT_DIR</td>
<td style="text-align: left;" class="confluenceTd">By default, jobs
start in the directory that the job was submitted from. So the "cd
$SLURM_SUBMIT_DIR" command is not needed.</td>
</tr>
<tr class="odd" role="row">
<td style="text-align: left;" class="confluenceTd">Machine(node)
list</td>
<td style="text-align: left;" class="confluenceTd">$SLURM_NODELIST</td>
<td style="text-align: left;" class="confluenceTd">variable name that
contains the list of nodes assigned to the batch job</td>
</tr>
<tr class="even" role="row">
<td style="text-align: left;" class="confluenceTd">Array JobID</td>
<td style="text-align: left;"
class="confluenceTd">$SLURM_ARRAY_JOB_ID<br />
$SLURM_ARRAY_TASK_ID</td>
<td style="text-align: left;" class="confluenceTd">each member of a job
array is assigned a unique identifier</td>
</tr>
</tbody>
</table>

</div>

See the sbatch man page for additional environment variables available.

**srun**

The srun command initiates an interactive job on the compute nodes.

For example, the following command:

`srun -A account_name --time=00:30:00 --nodes=1 --ntasks-per-node=64 \`

`--mem=16g --pty /bin/bash`

will run an interactive job in the default queue with a wall clock limit
of 30 minutes, using one node and 16 cores per node. You can also use
other sbatch options such as those documented above.

After you enter the command, you will have to wait for SLURM to start
the job. As with any job, your interactive job will wait in the queue
until the specified number of nodes is available. If you specify a small
number of nodes for smaller amounts of time, the wait should be shorter
because your job will backfill among larger jobs. You will see something
like this:

`srun: job 123456 queued and waiting for resources`

Once the job starts, you will see:

`srun: job 123456 has been allocated resources`

and will be presented with an interactive shell prompt on the launch
node. At this point, you can use the appropriate command to start your
program.

When you are done with your work, you can use the exit command to end
the job.

**scancel**

The scancel command deletes a queued job or terminates a running job.

-   scancel JobID deletes/terminates a job.

# **Refunds**

Refunds are considered, when appropriate, for jobs that failed due to
circumstances beyond user control.

XSEDE users and project that wish to request a refund should see the
XSEDE Refund Policy section located <a
href="https://portal.xsede.org/su-converter#:~:text=RESET-,XSEDE%20Refund%20Policy,-(v1.2)"
class="external-link" rel="nofollow" target="_blank">here</a>.

Other allocated users and projects wishing to request a refund
should email <a href="mailto:help@ncsa.illinois.edu" class="external-link"
rel="nofollow" target="_blank">help@ncsa.illinois.edu</a>. Please
include the batch job ids and the standard error and output files
produced by the job(s). 

# **Visualization**

Delta A40 nodes support NVIDIA raytracing hardware.

-   <span style="color: rgb(165,173,186);">describe visualization
    capabilities & software.</span>
-   <span style="color: rgb(165,173,186);">how to establish
    VNC/DVC/remote desktop</span>

# **Containers**

## Singularity

Container support on Delta is provided by Singularity. 

Docker images can be converted to Singularity sif format via the
`singularity pull ` command. Commands can be run from within a container
using `singularity run` command.

If you encounter quota issues with Singularity caching in
`~/.singularity` , the environment variable `SINGULARITY_CACHEDIR` can
be used to use a different location such as a scratch space. 

Your $HOME is automatically available from containers run via
Singularity.  You can "pip3 install --user" against a container's
python, setup virtualenv's or similar while useing a containerized
application.  Just run the container's /bin/bash to get a Singularity\>
prompt.  Here's an srun example of that with tensorflow:

<div class="code panel pdl conf-macro output-block"
style="border-width: 1px;" hasbody="true" macro-name="code">

<div class="codeHeader panelHeader pdl hide-border-bottom"
style="border-bottom-width: 1px;">

**srun the bash from a container to interact with programs inside it**
<span class="collapse-source expand-control" style=""><span
class="expand-control-icon icon"> </span><span
class="expand-control-text">Expand source</span></span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

<div>

<div id="highlighter_213490"
class="syntaxhighlighter collapsed sh-confluence nogutter java">

<div class="toolbar">

<a href="#"
class="toolbar_item command_expandSource expandSource">expand source</a><a href="#" class="toolbar_item command_help help">?</a>

</div>

<table data-border="0" data-cellpadding="0" data-cellspacing="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td class="code"><div class="container"
title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<code class="sourceCode java">$ srun \</code>
</div>
<div class="line number2 index1 alt1">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>mem<span
class="op">=</span><span class="er">32</span>g \</code>
</div>
<div class="line number3 index2 alt2">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>nodes<span
class="op">=</span></code><code class="sourceCode java"><span
class="dv">1</span></code> <code class="sourceCode java">\</code>
</div>
<div class="line number4 index3 alt1">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>ntasks<span
class="op">-</span>per<span class="op">-</span>node<span
class="op">=</span></code><code class="sourceCode java"><span
class="dv">1</span></code> <code class="sourceCode java">\</code>
</div>
<div class="line number5 index4 alt2">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>cpus<span
class="op">-</span>per<span class="op">-</span>task<span
class="op">=</span></code><code class="sourceCode java"><span
class="dv">1</span></code> <code class="sourceCode java">\</code>
</div>
<div class="line number6 index5 alt1">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>partition<span
class="op">=</span>gpuA100x4 \</code>
</div>
<div class="line number7 index6 alt2">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>account<span
class="op">=</span>bbka<span class="op">-</span>delta<span
class="op">-</span>gpu \</code>
</div>
<div class="line number8 index7 alt1">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>gpus<span
class="op">-</span>per<span class="op">-</span>node<span
class="op">=</span></code><code class="sourceCode java"><span
class="dv">1</span></code> <code class="sourceCode java">\</code>
</div>
<div class="line number9 index8 alt2">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>gpus<span
class="op">-</span>per<span class="op">-</span>task<span
class="op">=</span></code><code class="sourceCode java"><span
class="dv">1</span></code> <code class="sourceCode java">\</code>
</div>
<div class="line number10 index9 alt1">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>gpu<span
class="op">-</span>bind<span class="op">=</span>verbose<span
class="op">,</span>per_task<span class="op">:</span></code><code
class="sourceCode java"><span class="dv">1</span></code> <code
class="sourceCode java">\</code>
</div>
<div class="line number11 index10 alt2">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">--</span>pty \</code>
</div>
<div class="line number12 index11 alt1">
<code class="sourceCode java"> </code><code
class="sourceCode java">singularity run <span class="op">--</span>nv
\</code>
</div>
<div class="line number13 index12 alt2">
<code class="sourceCode java"> </code><code
class="sourceCode java"><span class="op">/</span>sw<span
class="op">/</span>external<span class="op">/</span>NGC<span
class="op">/</span>tensorflow<span class="op">:</span></code><code
class="sourceCode java"><span class="fl">22.02</span></code><code
class="sourceCode java"><span class="op">-</span>tf2<span
class="op">-</span>py3 <span class="op">/</span>bin<span
class="op">/</span>bash </code>
</div>
<div class="line number14 index13 alt1">
<code class="sourceCode java"># job starts <span
class="kw">...</span></code>
</div>
<div class="line number15 index14 alt2">
<code class="sourceCode java">Singularity<span class="op">&gt;</span>
hostname</code>
</div>
<div class="line number16 index15 alt1">
<code class="sourceCode java">gpua068<span class="op">.</span><span
class="fu">delta</span><span class="op">.</span><span
class="fu">internal</span><span class="op">.</span><span
class="fu">ncsa</span><span class="op">.</span><span
class="fu">edu</span></code>
</div>
<div class="line number17 index16 alt2">
<code class="sourceCode java">Singularity<span class="op">&gt;</span>
which python  # the python in the container</code>
</div>
<div class="line number18 index17 alt1">
<code class="sourceCode java"><span class="op">/</span>usr<span
class="op">/</span>bin<span class="op">/</span>python</code>
</div>
<div class="line number19 index18 alt2">
<code class="sourceCode java">Singularity<span class="op">&gt;</span>
python <span class="op">--</span>version</code>
</div>
<div class="line number20 index19 alt1">
<code class="sourceCode java">Python </code><code
class="sourceCode java"><span class="fl">3.8</span></code><code
class="sourceCode java"><span class="op">.</span></code><code
class="sourceCode java"><span class="dv">10</span></code>
</div>
<div class="line number21 index20 alt2">
<code class="sourceCode java">Singularity<span
class="op">&gt;</span></code>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

  

## NVIDIA NGC Containers

Delta provides NVIDIA NGC Docker containers that we have pre-built with
Singularity.  Look for the latest binary containers in
**/sw/external/NGC/** . The containers are used as shown in the sample
scripts below:

<div class="code panel pdl conf-macro output-block"
style="border-width: 1px;" hasbody="true" macro-name="code">

<div class="codeHeader panelHeader pdl"
style="border-bottom-width: 1px;">

**PyTorch example script**

</div>

<div class="codeContent panelContent pdl">

<div>

<div id="highlighter_512656"
class="syntaxhighlighter sh-confluence nogutter bash">

<div class="toolbar">

<a href="#" class="toolbar_item command_help help">?</a>

</div>

<table data-border="0" data-cellpadding="0" data-cellspacing="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td class="code"><div class="container"
title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<code class="sourceCode bash"><span class="co">#!/bin/bash</span></code>
</div>
<div class="line number2 index1 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--mem=64g</span></code>
</div>
<div class="line number3 index2 alt2">
<code class="sourceCode bash"><span class="co">#SBATCH
--nodes=1</span></code>
</div>
<div class="line number4 index3 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--ntasks-per-node=1</span></code>
</div>
<div class="line number5 index4 alt2">
<code class="sourceCode bash"><span class="co">#SBATCH
--cpus-per-task=64     # &lt;- match to OMP_NUM_THREADS, 64 requests
whole node</span></code>
</div>
<div class="line number6 index5 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--partition=gpuA100x4 # &lt;- one of: gpuA100x4 gpuA40x4 gpuA100x8
gpuMI100x8</span></code>
</div>
<div class="line number7 index6 alt2">
<code class="sourceCode bash"><span class="co">#SBATCH
--account=bbka-delta-gpu</span></code>
</div>
<div class="line number8 index7 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--job-name=pytorchNGC</span></code>
</div>
<div class="line number9 index8 alt2">
<code class="sourceCode bash"><span class="co">### GPU options
</span><span class="al">###</span></code>
</div>
<div class="line number10 index9 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--gpus-per-node=1</span></code>
</div>
<div class="line number11 index10 alt2">
<code class="sourceCode bash"><span class="co">#SBATCH
--gpus-per-task=1</span></code>
</div>
<div class="line number12 index11 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--gpu-bind=verbose,per_task:1 </span></code>
</div>
<div class="line number13 index12 alt2">
<code class="sourceCode bash"><span class="ex"> </span></code> 
</div>
<div class="line number14 index13 alt1">
<code class="sourceCode bash"><span class="ex">module</span> reset
</code><code class="sourceCode bash"><span class="co"># drop modules and
explicitly load the ones needed</span></code>
</div>
<div class="line number15 index14 alt2">
<code class="sourceCode bash"><span
class="ex">             </span></code><code
class="sourceCode bash"><span class="co"># (good job metadata and
reproducibility)</span></code>
</div>
<div class="line number16 index15 alt1">
<code class="sourceCode bash"><span
class="ex">             </span></code><code
class="sourceCode bash"><span class="co"># $WORK and $SCRATCH are now
set</span></code>
</div>
<div class="line number17 index16 alt2">
<code class="sourceCode bash"><span class="ex">module</span> list 
</code><code class="sourceCode bash"><span class="co"># job
documentation and metadata</span></code>
</div>
<div class="line number18 index17 alt1">
 
</div>
<div class="line number19 index18 alt2">
<code class="sourceCode bash"><span class="bu">echo</span></code> <code
class="sourceCode bash"><span class="st">&quot;job is starting on
</span><span class="kw">`</span><span class="fu">hostname</span><span
class="kw">`</span><span class="st">&quot;</span></code>
</div>
<div class="line number20 index19 alt1">
 
</div>
<div class="line number21 index20 alt2">
<code class="sourceCode bash"><span class="co"># run the container
binary with arguments: python3 &lt;program.py&gt;</span></code>
</div>
<div class="line number22 index21 alt1">
<code class="sourceCode bash"><span class="ex">singularity</span> run
<span class="at">--nv</span> <span class="dt">\</span></code>
</div>
<div class="line number23 index22 alt2">
<code class="sourceCode bash"><span class="ex"> </span></code><code
class="sourceCode bash"><span
class="ex">/sw/external/NGC/pytorch</span></code><code
class="sourceCode bash"><span class="ex">:22.02-py3</span> python3
tensor_gpu.py</code>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div class="code panel pdl conf-macro output-block"
style="border-width: 1px;" hasbody="true" macro-name="code">

<div class="codeHeader panelHeader pdl hide-border-bottom"
style="border-bottom-width: 1px;">

**Tensorflow example script** <span
class="collapse-source expand-control" style=""><span
class="expand-control-icon icon"> </span><span
class="expand-control-text">Expand source</span></span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

<div>

<div id="highlighter_589831"
class="syntaxhighlighter collapsed sh-confluence nogutter bash">

<div class="toolbar">

<a href="#"
class="toolbar_item command_expandSource expandSource">expand source</a><a href="#" class="toolbar_item command_help help">?</a>

</div>

<table data-border="0" data-cellpadding="0" data-cellspacing="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td class="code"><div class="container"
title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<code class="sourceCode bash"><span class="co">#!/bin/bash</span></code>
</div>
<div class="line number2 index1 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--mem=64g</span></code>
</div>
<div class="line number3 index2 alt2">
<code class="sourceCode bash"><span class="co">#SBATCH
--nodes=1</span></code>
</div>
<div class="line number4 index3 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--ntasks-per-node=1</span></code>
</div>
<div class="line number5 index4 alt2">
<code class="sourceCode bash"><span class="co">#SBATCH
--cpus-per-task=64     # &lt;- match to OMP_NUM_THREADS</span></code>
</div>
<div class="line number6 index5 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--partition=gpuA100x4 # &lt;- one of: gpuA100x4 gpuA40x4 gpuA100x8
gpuMI100x8</span></code>
</div>
<div class="line number7 index6 alt2">
<code class="sourceCode bash"><span class="co">#SBATCH
--account=bbka-delta-gpu</span></code>
</div>
<div class="line number8 index7 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--job-name=tfNGC</span></code>
</div>
<div class="line number9 index8 alt2">
<code class="sourceCode bash"><span class="co">### GPU options
</span><span class="al">###</span></code>
</div>
<div class="line number10 index9 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--gpus-per-node=1</span></code>
</div>
<div class="line number11 index10 alt2">
<code class="sourceCode bash"><span class="co">#SBATCH
--gpus-per-task=1</span></code>
</div>
<div class="line number12 index11 alt1">
<code class="sourceCode bash"><span class="co">#SBATCH
--gpu-bind=verbose,per_task:1</span></code>
</div>
<div class="line number13 index12 alt2">
<code class="sourceCode bash"><span class="ex"> </span></code> 
</div>
<div class="line number14 index13 alt1">
<code class="sourceCode bash"><span class="ex">module</span> reset
</code><code class="sourceCode bash"><span class="co"># drop modules and
explicitly load the ones needed</span></code>
</div>
<div class="line number15 index14 alt2">
<code class="sourceCode bash"><span
class="ex">             </span></code><code
class="sourceCode bash"><span class="co"># (good job metadata and
reproducibility)</span></code>
</div>
<div class="line number16 index15 alt1">
<code class="sourceCode bash"><span
class="ex">             </span></code><code
class="sourceCode bash"><span class="co"># $WORK and $SCRATCH are now
set</span></code>
</div>
<div class="line number17 index16 alt2">
<code class="sourceCode bash"><span class="ex">module</span> list 
</code><code class="sourceCode bash"><span class="co"># job
documentation and metadata</span></code>
</div>
<div class="line number18 index17 alt1">
 
</div>
<div class="line number19 index18 alt2">
<code class="sourceCode bash"><span class="bu">echo</span></code> <code
class="sourceCode bash"><span class="st">&quot;job is starting on
</span><span class="kw">`</span><span class="fu">hostname</span><span
class="kw">`</span><span class="st">&quot;</span></code>
</div>
<div class="line number20 index19 alt1">
 
</div>
<div class="line number21 index20 alt2">
<code class="sourceCode bash"><span class="co"># run the container
binary with arguments: python3 &lt;program.py&gt;</span></code>
</div>
<div class="line number22 index21 alt1">
<code class="sourceCode bash"><span class="ex">singularity</span> run
<span class="at">--nv</span> <span class="dt">\</span></code>
</div>
<div class="line number23 index22 alt2">
<code class="sourceCode bash"><span class="ex"> </span></code><code
class="sourceCode bash"><span
class="ex">/sw/external/NGC/tensorflow</span></code><code
class="sourceCode bash"><span class="ex">:22.02-tf2-py3</span> python3
<span class="dt">\</span></code>
</div>
<div class="line number24 index23 alt1">
<code class="sourceCode bash"><span class="ex"> </span></code><code
class="sourceCode bash"><span class="ex">tf_matmul.py</span></code>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

## Container list (as of March, 2022)

<div class="code panel pdl conf-macro output-block"
style="border-width: 1px;" hasbody="true" macro-name="code">

<div class="codeHeader panelHeader pdl"
style="border-bottom-width: 1px;">

**catalog.txt**

</div>

<div class="codeContent panelContent pdl">

<div>

<div id="highlighter_134830"
class="syntaxhighlighter sh-confluence nogutter java">

<div class="toolbar">

<a href="#" class="toolbar_item command_help help">?</a>

</div>

<table data-border="0" data-cellpadding="0" data-cellspacing="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td class="code"><div class="container"
title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<code class="sourceCode java">caffe<span class="op">:</span></code><code
class="sourceCode java"><span class="fl">20.03</span></code><code
class="sourceCode java"><span class="op">-</span>py3</code>
</div>
<div class="line number2 index1 alt1">
<code class="sourceCode java">caffe2<span
class="op">:</span></code><code class="sourceCode java"><span
class="fl">18.08</span></code><code class="sourceCode java"><span
class="op">-</span>py3</code>
</div>
<div class="line number3 index2 alt2">
<code class="sourceCode java">cntk<span class="op">:</span></code><code
class="sourceCode java"><span class="fl">18.08</span></code><code
class="sourceCode java"><span class="op">-</span>py3 <span
class="op">,</span> Microsoft Cognitive <span
class="bu">Toolkit</span></code>
</div>
<div class="line number4 index3 alt1">
<code class="sourceCode java">digits<span
class="op">:</span></code><code class="sourceCode java"><span
class="fl">21.09</span></code><code class="sourceCode java"><span
class="op">-</span>tensorflow<span class="op">-</span>py3</code>
</div>
<div class="line number5 index4 alt2">
<code class="sourceCode java">lammps<span
class="op">:</span>patch_4May2022</code>
</div>
<div class="line number6 index5 alt1">
<code class="sourceCode java">matlab<span
class="op">:</span>r2021b</code>
</div>
<div class="line number7 index6 alt2">
<code class="sourceCode java">mxnet<span class="op">:</span></code><code
class="sourceCode java"><span class="fl">21.09</span></code><code
class="sourceCode java"><span class="op">-</span>py3</code>
</div>
<div class="line number8 index7 alt1">
<code class="sourceCode java">namd<span class="op">:</span></code><code
class="sourceCode java"><span class="fl">2.13</span></code><code
class="sourceCode java"><span class="op">-</span>multinode</code>
</div>
<div class="line number9 index8 alt2">
<code class="sourceCode java">pytorch<span
class="op">:</span></code><code class="sourceCode java"><span
class="fl">22.02</span></code><code class="sourceCode java"><span
class="op">-</span>py3</code>
</div>
<div class="line number10 index9 alt1">
<code class="sourceCode java">tensorflow<span
class="op">:</span></code><code class="sourceCode java"><span
class="fl">22.02</span></code><code class="sourceCode java"><span
class="op">-</span>tf1<span class="op">-</span>py3</code>
</div>
<div class="line number11 index10 alt2">
<code class="sourceCode java">tensorflow<span
class="op">:</span></code><code class="sourceCode java"><span
class="fl">22.02</span></code><code class="sourceCode java"><span
class="op">-</span>tf2<span class="op">-</span>py3</code>
</div>
<div class="line number12 index11 alt1">
<code class="sourceCode java">tensorrt<span
class="op">:</span></code><code class="sourceCode java"><span
class="fl">22.02</span></code><code class="sourceCode java"><span
class="op">-</span>py3</code>
</div>
<div class="line number13 index12 alt2">
<code class="sourceCode java">theano<span
class="op">:</span></code><code class="sourceCode java"><span
class="fl">18.08</span></code>
</div>
<div class="line number14 index13 alt1">
<code class="sourceCode java">torch<span class="op">:</span></code><code
class="sourceCode java"><span class="fl">18.08</span></code><code
class="sourceCode java"><span class="op">-</span>py2</code>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

see
also: <a href="https://catalog.ngc.nvidia.com/orgs/nvidia/containers"
class="external-link" rel="nofollow"
target="_blank">https://catalog.ngc.nvidia.com/orgs/nvidia/containers</a>

## Other Containers

### Extreme-scale Scientific Software Stack (E4S)

The E4S container with GPU (cuda and rocm) support is provided for users
of specific ECP packages made available by the E4S project
(<a href="https://e4s-project.github.io/" class="external-link"
rel="nofollow" target="_blank">https://e4s-project.github.io/</a>). The
singularity image is available as :

    /sw/external/E4S/e4s-gpu-x86_64.sif

    To use E4S with NVIDIA GPUs

<div class="code panel pdl conf-macro output-block"
style="border-width: 1px;" hasbody="true" macro-name="code">

<div class="codeContent panelContent pdl">

<div>

<div id="highlighter_120219"
class="syntaxhighlighter sh-confluence nogutter java">

<div class="toolbar">

<a href="#" class="toolbar_item command_help help">?</a>

</div>

<table data-border="0" data-cellpadding="0" data-cellspacing="0">
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td class="code"><div class="container"
title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<code class="sourceCode java">$ srun <span
class="op">--</span>account<span class="op">=</span>account_name <span
class="op">--</span>partition<span class="op">=</span>gpuA100<span
class="op">-</span>interactive \</code>
</div>
<div class="line number2 index1 alt1">
<code class="sourceCode java">  </code><code
class="sourceCode java"><span class="op">--</span>nodes<span
class="op">=</span></code><code class="sourceCode java"><span
class="dv">1</span></code> <code class="sourceCode java"><span
class="op">--</span>gpus<span class="op">-</span>per<span
class="op">-</span>node<span class="op">=</span></code><code
class="sourceCode java"><span class="dv">1</span></code> <code
class="sourceCode java"><span class="op">--</span>tasks<span
class="op">=</span></code><code class="sourceCode java"><span
class="dv">1</span></code> <code class="sourceCode java"><span
class="op">--</span>tasks<span class="op">-</span>per<span
class="op">-</span>node<span class="op">=</span></code><code
class="sourceCode java"><span class="dv">1</span></code> <code
class="sourceCode java">\ </code>
</div>
<div class="line number3 index2 alt2">
<code class="sourceCode java">  </code><code
class="sourceCode java"><span class="op">--</span>cpus<span
class="op">-</span>per<span class="op">-</span>task<span
class="op">=</span></code><code class="sourceCode java"><span
class="dv">1</span></code> <code class="sourceCode java"><span
class="op">--</span>mem<span class="op">=</span><span
class="er">20</span>g \</code>
</div>
<div class="line number4 index3 alt1">
<code class="sourceCode java">  </code><code
class="sourceCode java"><span class="op">--</span>pty bash</code>
</div>
<div class="line number5 index4 alt2">
<code class="sourceCode java">$ singularity exec <span
class="op">--</span>cleanenv <span class="op">/</span>sw<span
class="op">/</span>external<span class="op">/</span>E4S<span
class="op">/</span>e4s<span class="op">-</span>gpu<span
class="op">-</span>x86_64<span class="op">.</span><span
class="fu">sif</span> \</code>
</div>
<div class="line number6 index5 alt1">
<code class="sourceCode java">  </code><code
class="sourceCode java"><span class="op">/</span>bin<span
class="op">/</span>bash <span class="op">--</span>rcfile <span
class="op">/</span>etc<span class="op">/</span>bash<span
class="op">.</span><span class="fu">bashrc</span></code>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

The spack package inside of the image will interact with a local spack
installation. If  \~/.spack directory exists, it might need to be
renamed. 

More information can be found at
<a href="https://e4s-project.github.io/download.html"
class="external-link" rel="nofollow"
target="_blank">https://e4s-project.github.io/download.html</a>

# <span style="color: rgb(165,173,186);">**Protected Data (N/A)**</span>

...

# **Help**

For assistance with the use of Delta

-   XSEDE users can create a ticket via the user portal at
    <a href="https://portal.xsede.org/web/xup/help-desk"
    class="external-link" rel="nofollow"
    target="_blank">https://portal.xsede.org/web/xup/help-desk</a>
-   All other users (Illinois allocations, Diversity Allocations, etc)
    please send email to help@ncsa.illinois.edu.

# **Acknowledge**

To acknowledge the NCSA Delta system in particular, please include the
following

This research is part of the Delta research computing project, which is
supported by the National Science Foundation (award OCI 2005572), and
the State of Illinois. Delta is a joint effort of the University of
Illinois at Urbana-Champaign and its National Center for Supercomputing
Applications.

To include acknowledgement of XSEDE contributions to a publication or
presentation please see
<a href="https://portal.xsede.org/acknowledge" class="external-link"
rel="nofollow" target="_blank">https://portal.xsede.org/acknowledge</a>
and <a href="https://www.xsede.org/for-users/acknowledgement"
class="external-link" rel="nofollow"
target="_blank">https://www.xsede.org/for-users/acknowledgement</a>.

# **References**

Supporting documentation resources:

<a href="https://www.rcac.purdue.edu/knowledge/anvil"
class="external-link" rel="nofollow"
target="_blank">https://www.rcac.purdue.edu/knowledge/anvil</a>

<a href="https://nero-docs.stanford.edu/jupyter-slurm.html"
class="external-link" rel="nofollow"
target="_blank">https://nero-docs.stanford.edu/jupyter-slurm.html</a>

  

</div>
