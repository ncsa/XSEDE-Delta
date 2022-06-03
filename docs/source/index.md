::: innerCell
# **[ ]{#DeltaXSEDEDocumentation-top .confluence-anchor-link .conf-macro .output-inline hasbody="false" macro-name="anchor"}Delta User Guide** {#DeltaXSEDEDocumentation-topDeltaUserGuide}

*Last update: April 7, 2022*

# **Status Updates and Notices** {#DeltaXSEDEDocumentation-StatusUpdatesandNotices}

*Delta* is tentatively scheduled to enter production in Q2 2022.

::: {.confluence-information-macro .confluence-information-macro-information .conf-macro .output-block hasbody="true" macro-name="info"}
key: light grey font is work in progress

[ ]{.aui-icon .aui-icon-small .aui-iconfont-info
.confluence-information-macro-icon}

::: confluence-information-macro-body
Items in light grey font are in progress and coming soon.  [Example
software or feature not yet
implemented.]{style="color: rgb(165,173,186);"}
:::
:::

# **Introduction** {#DeltaXSEDEDocumentation-Introduction}

*Delta* is a dedicated, [eXtreme Science and Engineering Science
Discovery Environment (XSEDE)](http://www.xsede.org){.external-link
rel="nofollow" target="_blank"} allocated resource designed by HPE and
NCSA, delivering a highly capable GPU-focused compute environment for
GPU and CPU workloads.  Besides offering a mix of standard and reduced
precision GPU resources, *Delta* also offers GPU-dense nodes with both
NVIDIA and AMD GPUs.  *Delta* provides high performance node-local SSD
scratch filesystems, as well as both standard lustre and
[relaxed-POSIX]{style="color: rgb(165,173,186);"} parallel filesystems
spanning the entire resource.

*Delta\'s* CPU nodes are each powered by two 64-core AMD EPYC 7763
(\"Milan\") processors, with 256 GB of DDR4 memory.  The *Delta* GPU
resource has four node types: one with 4 NVIDIA A100 GPUs (40 GB HBM2
RAM each) connected via NVLINK and 1 64-core AMD EPYC 7763 (\"Milan\")
processor, the second with 4 NVIDIA A40 GPUs (48 GB GDDR6 RAM) connected
via PCIe 4.0 and 1 64-core AMD EPYC 7763 (\"Milan\") processor, the
third with 8 NVIDIA A100 GPUs in a dual socket AMD EPYC 7763 (128-cores
per node) node with 2 TB of DDR4 RAM and NVLINK,  and the fourth with 8
AMD MI100 GPUs (32GB HBM2 RAM each) in a dual socket AMD EPYC 7763
(128-cores per node) node with 2 TB of DDR4 RAM and PCIe 4.0. 

*Delta* has 124 standard CPU nodes, 100 4-way A100-based GPU nodes, 100
4-way A40-based GPU nodes, 5 8-way A100-based GPU nodes, and 1 8-way
MI100-based GPU node.  Every *Delta* node has high-performance
node-local SSD storage (740 GB for CPU nodes, 1.5 TB for GPU nodes), and
is connected to the 7 PB Lustre parallel filesystem via the high-speed
interconnect.  The *Delta* resource uses the SLURM workload manager for
job scheduling.  

Delta supports the [XSEDE core software
stack](https://www.xsede.org/software){.external-link rel="nofollow"
target="_blank"}, including remote login, remote computation, data
movement, science workflow support, and science gateway support
toolkits.

\

**[![](/download/attachments/144016994/image2021-3-12_14-23-11.png?version=1&modificationDate=1615925873000&api=v2){.confluence-embedded-image
height="250"
image-src="/download/attachments/144016994/image2021-3-12_14-23-11.png?version=1&modificationDate=1615925873000&api=v2"
unresolved-comment-count="0" linked-resource-id="144016995"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="image2021-3-12_14-23-11.png"
base-url="https://wiki.ncsa.illinois.edu"
linked-resource-content-type="image/png"
linked-resource-container-id="144016994"
linked-resource-container-version="177"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}**

**Figure 1. Delta System**

Delta is supported by the National Science Foundation under Grant No.
OAC-[2005572]{style="color: rgb(0,0,0);"}.

Any opinions, findings, and conclusions or recommendations expressed in
this material are those of the author(s) and do not necessarily reflect
the views of the National Science Foundation.

::: table-wrap
  -------------------------------------
   *Delta* is now accepting proposals.
  -------------------------------------
:::

[Top of Page](#DeltaXSEDEDocumentation-top)

# **Account Administration** {#DeltaXSEDEDocumentation-AccountAdministration}

-   For XSEDE projects please use the [XSEDE user
    portal](https://portal.xsede.org/my-xsede){.external-link
    rel="nofollow" target="_blank"} for project and account management.
-   Non-XSEDE Account and Project administration is handled by NCSA
    Identity and NCSA group management tools. For more information
    please see the [NCSA Allocation and Account
    Management](/display/USSPPRT/User+Services+NCSA+Allocation+and+Account+Management)
    documentation page. 

## **Configuring Your Account** {#DeltaXSEDEDocumentation-ConfiguringYourAccount}

-   bash is the default shell, submit a support request to change your
    default shell
-   environment variables: [XSEDE
    CUE]{style="color: rgb(165,173,186);"}, [SLURM
    batch](#DeltaXSEDEDocumentation-slurm_environment_batch)\
-   using [Modules](#DeltaXSEDEDocumentation-lmod) 

# **System Architecture** {#DeltaXSEDEDocumentation-SystemArchitecture}

[Delta is designed to help applications transition from CPU-only to GPU
or hybrid CPU-GPU codes. Delta has some important architectural features
to facilitate new discovery and insight:]{style="color: rgb(0,0,0);"}

-   [a single processor architecture (AMD) across all node types: CPU
    and GPU]{style="color: rgb(0,0,0);"}
-   [support for NVIDIA A100 MIG GPU partitioning allowing for
    fractional use of the A100s if your workload isn\'t able to exploit
    an entire A100 efficiently]{style="color: rgb(17,17,17);"}
-   [ray tracing hardware support from the NVIDIA A40
    GPUs]{style="color: rgb(0,0,0);"}
-   [9 large memory (2 TB) nodes ]{style="color: rgb(0,0,0);"}
-   [a low latency and high bandwidth HPE/Cray Slingshot interconnect
    between compute nodes]{style="color: rgb(0,0,0);"}
-   [lustre for home, projects and scratch file
    systems]{style="color: rgb(0,0,0);"}
-   [support for relaxed and non-posix
    IO]{style="color: rgb(165,173,186);"}
-   [shared-node jobs and the single core and single MIG GPU
    slice]{style="color: rgb(17,17,17);"}
-   [Resources for persistent services in support of Gateways, Open
    OnDemand, Data Transport nodes\..., ]{style="color: rgb(17,17,17);"}
-   [Unique AMD MI-100 resource]{style="color: rgb(0,0,0);"}  

## **Model Compute Nodes** {#DeltaXSEDEDocumentation-ModelComputeNodes}

[The Delta compute ecosystem is composed of 5 node types: dual-socket
CPU-only compute nodes, single socket 4-way NVIDIA A100 GPU compute
nodes, single socket 4-way NVIDIA A40 GPU compute nodes, dual-socket
8-way NVIDIA A100 GPU compute nodes, and a single socket 8-way AMD MI100
GPU compute nodes. The CPU-only and 4-way GPU nodes have 256 GB of RAM
per node while the 8-way GPU nodes have 2 TB of RAM. The CPU-only node
has 0.74 TB of local storage while all GPU nodes have 1.5 TB of local
storage.]{style="color: rgb(0,0,0);"}

### Table. CPU Compute Node Specifications {#DeltaXSEDEDocumentation-Table.CPUComputeNodeSpecifications}

::: table-wrap
+------------------------------+------------------------------+
| ::: tablesorter-header-inner | ::: tablesorter-header-inner |
| Specification                | Value                        |
| :::                          | :::                          |
+==============================+==============================+
| Number of nodes              | 124                          |
+------------------------------+------------------------------+
| CPU                          | AMD EPYC 7763\               |
|                              | \"Milan\" (PCIe Gen4)        |
+------------------------------+------------------------------+
| Sockets per node             | 2                            |
+------------------------------+------------------------------+
| Cores per socket             | 64                           |
+------------------------------+------------------------------+
| Cores per node               | 128                          |
+------------------------------+------------------------------+
| Hardware threads per core    | 1                            |
+------------------------------+------------------------------+
| Hardware threads per node    | 128                          |
+------------------------------+------------------------------+
| Clock rate (GHz)             | \~ 2.45                      |
+------------------------------+------------------------------+
| RAM (GB)                     | 256                          |
+------------------------------+------------------------------+
| Cache (KB) L1/L2/L3          |  64/512/32768                |
+------------------------------+------------------------------+
| Local storage (TB)           | 0.74 TB                      |
+------------------------------+------------------------------+
:::

The AMD CPUs are set for 4 NUMA domains per socket (NPS=4). 

### Table. 4-way NVIDIA A40 GPU Compute Node Specifications  {#DeltaXSEDEDocumentation-Table.4-wayNVIDIAA40GPUComputeNodeSpecifications}

::: table-wrap
::: tablesorter-header-inner
Specification
:::
:::
:::

::: tablesorter-header-inner
Value
:::

Number of nodes

100

GPU

NVIDIA A40 

([Vendor
page](https://www.nvidia.com/en-us/data-center/a40/#specs){.external-link
rel="nofollow" target="_blank"})

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

Table. 4-way NVIDIA A40 Mapping and GPU-CPU AffeTd\"\>GPU1

SYS

X

SYS

SYS

SYS

32-47

\

Table Legend

[X    = Self\
]{.s1}[SYS  = ConneaderRow\"\>]{.s1}

NVIDIA A100

([Vendor
page](https://www.nvidia.com/en-us/data-center/a100/#specifications){.external-link
rel="nofollow" target="_blank"})

fluenceTd\"\>1

Hardware threads per node

64

Clock rate (GHz)

\~ 2.45

\

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
(typically the CPU)\

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
apply an ascending sort\" style=\"user-select: none;\"\>

::: {.confluenceTd 1\"=""}
GPUs per node
:::

8

GPU Memory (GB)

32

CPU

\~ 2.45

RAM (GB)

**Network**

Delta is connected to the nd it has 6PB of usable space.  These file
systems run Lustre via DDN\'s ExaScaler 6 stack (Lustre 2.14 based).

[Hardware:\
]{.underline}DDN SFA7990XE (Quantity: 3), each unit contains

-   One additional SS9012 enclosure
-   168 x 16TB SAS Drives
-   7 x 1.92TB SAS SSDs

The \$HOME file system has ort applied, activate to apply an ascending
sort\" style=\"user-select: none;\"\>

::: tablesorter-header-inner
File size
:::

[Hardware:\
]{.underline}DDN SFA400NVXE (Quanup\>

::: {.tables aria-relevant="all"}
\$HOME
:::

**25GB. **400,000 files per u=\"1\" class=\"confluenceTd\"\>No

Yes; files older than 30-days (access time)

Area for computation, largest allocEDocumentation-login_nodes\"
data-hasbody=\"false\" data-macro-name=\"anchor\"\>

Direct access to the Delta login nodene\" aria-label=\"example usage
with ssh: No sort applied, activate to apply an ascending sort\"
style=\"user-select: none;\"\>

::: tablesorter-header-inner
eedu
:::

[If needed, XSEDE users can lookup their local username at
]{style="letter-spacing: 0.0px;"}[](https://portal.xsede.org/group/xup/accounts){.external-link
style="letter-spacing: 0.0px;" re\"true\"="" macro-name="info"}

maintaining persistent sessions: tmux

[ ]{.aui-icon .aui-icon-small .aui-iconfont-info
.confluence-information-macro-icon}

::: confluence-information-macro-body
tmux is available on the login nodes to maintain persistent sessse
execute the gsissh command with the "-vvv" option and include the
verbose output in your problem description.

Once on the XSEDE SSO hub:

::: {.coder .sh-confluence .nogutter .java}
::: toolbar
[](#){.toolbar_item .cthe .system .affects .others. .Exercise .good
.citizenship .to .ensure .that .your .activity .does .not .advee .the
.following .entries .in .their .<code>$HOME</code> .and .entries .in
.the .project .and .scratch .file .systems. .T</span>o
.depan></div><table .border= 0\"="" cellpadding="0" cellspacing="0"}

::: {.container title="Hint: double-click to select code"}
:::

::: {.line .number6 .index5 .alt1}
 
:::

::: {.line .number7 .index6 .alt2}
`$ `{.bash .plain}nbsp;  delta_abcd      4096 Feb 21 11:54
`/scratch/abcd`{.bash .plain}
:::

::: {.line .number15 .i .</p><p><br></p><div .class= confluence-information-macro="" confluence-information-macro-information="" conf-macro="" outpu="" usability="" of="" the="" delta="" login="" node.&nbsp;<="" span=""}
rsync - to be used for small to modest transfers to avoid impacting the
usability of the Delta login node. 

-   Sharing Files with Collaborators

    Building Software. 

    [![](/rest/documentConversion/latest/conversion/thumbnail/179671355%20apply%20an%20ascending%20sort)]{.confluence-embedded-file-wrapper
    .conf-macrD .EPYC .7xx3 .Series .Processors.pdf}

    ::: tablesorter-header-inner
    gcc
    :::

-   [Launching One Hybrid (MPI+Threads)
    Application]{style="color: rgb(165,173,186);"}

-   More Than One Serial Applicati\>Keep in mind that your charges are
    based on the resources that are reserved de

## **Accessing the Compute Nodes** {#DeltaXSEDEDocumentation-AccessingtheComputeNodes}

Deltamitting[ for detail number2 index1 alt1\"\>`             `{.java
.spaces}]{style="letter-spacing: 0.0px;"}

::: {.codeContent .panelContent .pdl}
<div>

`252`{.java .value} `GB`{.java .plain}

</div>

::: {.tablesorter-headerRow linked-resource-id="179671305" linked-resource-type="attachment" linked-resource-container-id="1p><thead><tr role=" row\"=""}
[TBD]{style="color: rgb(165,173,186);"}
:::
:::
:::
:::
:::
:::

30 min[30 min]{style="color: rgb(165,173,186);"}

TBD[TBD]{style="color: rgb(165,173,186);"}

[2.0]{style="color: rgb(165,173,186);"}

1.n-sviewviewofslurmpartitions\"\>sview view of slurm partitions

[?](#){.toolbar_item .command_help .help}

single core class=\"java plain\"\>\--tasks=`1`{.java .value} `\`{.java
.plain}

::: {.line .number3 .index2 .alt2}
`  `{.java .spaces}`--tasks-per-node=`{.java .plain}`1`{.java .value}
`--cpus-per-task=`{.java .plain}`1`{.java .value} applications from
within them.  Use [*mpirun*]{.underline} to launch mpi jobs from within
an interactive job.  Within standard batch jobs submitted via sbatch,
use [srun]{.underline} to launch MPI codes.
:::

### Interactive X11 Support {#DeltaXSEDEDocumentation-InteractiveX11Support}

To run an X11 based application on a compute node in an interactive
session, the use of the `--x11` switch with `srun` is needed. For
example, to run a single core job that uses 1g of memory with X11 (in
this\" title=\"Hint: double-click to select code\"\>

::: {.line .number1 .index0 .alt2}
`srun -A abcd-delta-cpu  --partition=cpu \`{.java .plain}
:::

::: {.line .number2 .index1 .alt1}
`  `{.java .spaces}`--nodes=`{.java .plain}`1`{.java .value}
`--tasks=`{.java .plain}`1`{.java .value} `--tasks-per-node=`{.java
.plain}` `{#DeltaXSEDEDocumentation-JobScripts .confluence-anchor-link
.conf-macro .output-inline clats="" <span="" hasbody="false"
macro-name="anchor"}

-   Serial jobs

    ::: {.code .panel .pdl .conf-macro .output-block style="border-width: 1px;" hasbody="true" macro-name="code"}
    ::: {.codeHeader .panelHeader .pdl style="border-bottom-width: 1px;"}
    **serial example script**`#!/bin/bash`{.bash .preprocessor .bold}
    :::

    ::: {.line .number3 .index2 .alt2}
    `#SBATCH --mem=16g`{.bash .comments}
    :::

    ::: {.line .number4 .index3 .alt1}
    `#SBATCH --nodes=1`{.bash .comments}
    :::

    ::: {.line .number5 .index4 .alt2}
    `#SBATCH --ntasks-per-node=1`{.bash .comments}
    :::

    ::: {.line .number6 .index5 .alt1}
    `#SBATCH --cpus-per-task=1    # <- match to OMP_NUM_THREADS`{.bash
    .comments}
    :::

    ::: {.line .number7 .index6 .alt2}
    `#SBA      # hh:mm:ss for the job`{.bash .comments}
    :::

    ::: {.line .number11 .index10 .alt2}
    `### GPU options ###`{.bash .comments}
    :::

    ::: {.line .number12 .index11 .alt1}
    `##SBATCH --gpus-per-node=2`{.bash .comments}
    :::

    ::: {.line .number13 .index12 .alt2}
    `##SBATCH --gpu-bind=none     # <- or closest`{.bash .comments}
    :::

    ::: {.line .number14 .index13 .alt1}
    `##SBATCH --mail-user=you@yourinstitution.edu `{.bash .comments}
    :::

    ::: {.line .number15 .index14 .alt2}
    `##SBATCH --mail-type="BEGIN,END" See sbatch or srun man pages for more email options  `{.bash
    .comments}
    :::

    ::: {.line .number16 .index15 .alt1}
     
    :::

    ::: {.line .number17 .index16 .alt2}
     
    :::

    ::: {.line .index18 .alt2}
    `             `{.bash
    .spaces}`# (good job metadata and reproducibility)`{.bash .comments}
    :::

    ::: {.line .number20 .index19 .alt1}
    `             `{.bash .spaces}`# $WORK and $SCRATCH are notd>`{.bash
    .comments}
    MPI  

    ::: {.code .panel .pdl .conf-macro .output-block style="border-width: 1px;" hasbody="true" macro-name="code"}
    ::: {.codeHeader .panelHeader .pdl .hide-border-bottom style="border-bottom-width: 1px;"}
    **mpi example script** []{clat:="" double-click="" to="" select=""
    code\"=""}

    ::: {.line .number1 .index0 .alt2}
    `#!/bin/bash`{.bash .preprocessor .bold}
    :::

    ::: {.line .number2 .index1 .alt1}
    `#SBATCH --mem=16g`{.bash .comments}
    :::

    `#SBATCH --time=00:10:00      # hh:mm:ss for the job`{.bash
    .comments}
    :::

    ::: {.line .number10 .index9 .alt1}
    `### GPU options ###`{.bash .comments}
    :::

    ::: {.line .number11 .index10 .alt2}
    `##SBATCH --gpus-per-node=2`{.bash .comments}
    :::

    ::: {.line .number12 .index11 .alt1}
    `##SBATCH --gpu-bind=none     # <- or closest ##SBATCH --mp;      `{.bash
    .comments}`# (good job metadata and reproducibility)`{.bash
    .comments}
    :::

    ::: {.line .number17 .index16 .alt2}
    `             `{.bash
    .spaces}`# $WORK and $SCRATCH are now set`{.bash .comments}
    :::

    ::: {.line .number18 .index17 .alt1}
    `module load gcc`{.bash .plain}`/11`{.bash
    .plain}`.2.0 openmpi  `{.bash
    .plain}`# ... or any appropriate modules`{.bash .comments}
    :::

    ::: {.line .numbody></table></div></div> .</div> .</div></li><li><p .class= auto-cursor-target\"=""}
    OpenMP  [ ]{style="color: rgb(255,204,153);"}

    ::: {.code .panel .pdl .conf-macro .output-block style="border-width: 1px;" hasbody="true" macro-name="code"}
    ::: {.codeHeader .panelHeader .pdl .hide-border-bottom style="border-bottom-width: 1px;"}
    **openmp example script** [[ ]{.expand-control-icon .icon}[Expand
    sourcetd class=\"code\"\>]{.expand-control-text}]{.collapse-source
    .expand-control style=""}

    ::: {.container title="Hint: double-click to select code"}
    ::: {.line .number1 .index0 .alt2}
    `#!/bin/bash`{.bash .preprocessor .bold}
    :::

    ::: {.line .number2 .index1 .alt1}
    `#SBATCH --mem=16g`{.bash .comments}
    :::

    ::: {.line .number3 .index2 .alt2}
    `#SBATCH --nodes=1`{.bash .comments}
    :::

    ::: {.line .number4 .index3 .alt1}
    `#SBATCH --ntasks-per-node=1`{.bash .comments}

    ::: {.line .number9 .index8 .alt2}
    `#SBATCH --time=00:10:00      # hh:mm:ss for the job`{.bash
    .comments}
    :::

    ::: {.line .number10 .index9 .alt1}
    `### GPU options ###`{.bash .comments}
    :::

    ::: {.line .number11 .index10 .alt2}
    `##SBATCH --gpus-per-node=2`{.bash .comments}
    :::

    ::: {.line .number12 .index11 .alt1}
    `##SBATCH --gpu-bind=none     # <- o needed`{.bash .comments}
    :::

    ::: {.line .number17 .index16 .alt2}
    `             `{.bash
    .spaces}`# (good job metadata and reproducibility)`{.bash .comments}
    :::

    ::: {.line .number18 .index17 .alt1}
    `             `{.bash
    .spaces}`# $WORK and $SCRATCH are now set`{.bash .comments}
    :::

    ::: {.line .number19 .index18 .alt2}
    `mo/div>`{.bash .plain}

    ::: {.line .number22 .index21 .alt1}
    `export`{.bash .functions} `OMP_NUM_THREADS=32`{.bash .plain}
    :::

    ::: {.line .number23 .index22 .alt2}
    `srun stream_gcc`{.bash .plain}
    :::
    :::
    :::
    :::
    :::
    :::
    :::
    :::
    :::
    :::

-   [Parametric / Array / HTC jobs]{style="color: rgb(165,173,186);"}

# **Job Management ** {#DeltaXSEDEDocumentation-JobManagement}

Batch jobs are submitted through a *job script*  (as in the examples
above) using the sbatch command. Job scripts generally start with a
series of SLURM *directives* that describe requirements of the job such
as number of nodes, wall time required, etc... to the batch
system/scheduler (SLURM directives can also be specified as options on
the sbatch command line; command line options take precedence over those
in the script). The rest of the batch script consists of user commands.

The syntax for sbatch is:

**sbatch** \[list of sbatch options\] script_name

Refer to the sbatch man page for detailed information on the options.

#### squeue/scontrol/sinfo {#DeltaXSEDEDocumentation-squeue/scontrol/sinfo style="text-align: center;"}

Commands that display batch job and partition information .

::: table-wrap
+----------------------------------+----------------------------------+
| ::: tablesorter-header-inner     | ::: tablesorter-header-inner     |
| SLURM EXAMPLE COMMAND            | DESCRIPTION                      |
| :::                              | :::                              |
+==================================+==================================+
| [squeue                          | List the status of all jobs on   |
| -a]{style="color: rgb(0,0,0);"}  | the system.                      |
+----------------------------------+----------------------------------+
| [squeue -u                       | List the status of all your jobs |
| \$U                              | in the batch system.             |
| SER]{style="color: rgb(0,0,0);"} |                                  |
+----------------------------------+----------------------------------+
| [squeue -j                       | List nodes allocated to a        |
| Jo                               | running job in addition to basic |
| bID]{style="color: rgb(0,0,0);"} | information..                    |
+----------------------------------+----------------------------------+
| [scontrol show job               | List detailed information on a   |
| Jo                               | particular job.                  |
| bID]{style="color: rgb(0,0,0);"} |                                  |
+----------------------------------+----------------------------------+
| [sinfo                           | List summary information on all  |
| -a]{style="color: rgb(0,0,0);"}  | the partition.                   |
+----------------------------------+----------------------------------+
:::

See the manual (man) pages for other available options.

\

Useful Batch Job Environment Variables[
]{#DeltaXSEDEDocumentation-slurm_environment_variables
.confluence-anchor-link .conf-macro .output-inline hasbody="false"
macro-name="anchor"}

::: table-wrap
+-------+-----------+-------------------------------------+
| :::   | :         | ::: tablesorter-header-inner        |
|  tabl | :: tables | DETAIL DESCRIPTION                  |
| esort | orter-hea | :::                                 |
| er-he | der-inner |                                     |
| ader- | SLURM     |                                     |
| inner | EN        |                                     |
| D     | VIRONMENT |                                     |
| ESCRI | VARIABLE  |                                     |
| PTION | :::       |                                     |
| :::   |           |                                     |
+:======+:==========+:====================================+
| JobID | \$SLU     | Job identifier assigned to the job  |
|       | RM_JOB_ID |                                     |
+-------+-----------+-------------------------------------+
| Job   | \$SLURM_S | By default, jobs start in the       |
| Submi | UBMIT_DIR | directory that the job was          |
| ssion |           | submitted from. So the \"cd         |
| Dire  |           | \$SLURM_SUBMIT_DIR\" command is not |
| ctory |           | needed.                             |
+-------+-----------+-------------------------------------+
| Mac   | \$SLURM   | variable name that contains the     |
| hine( | _NODELIST | list of nodes assigned to the batch |
| node) |           | job                                 |
| list  |           |                                     |
+-------+-----------+-------------------------------------+
| Array | \$S       | each member of a job array is       |
| JobID | LURM_ARRA | assigned a unique identifier        |
|       | Y_JOB_ID\ |                                     |
|       | \$S       |                                     |
|       | LURM_ARRA |                                     |
|       | Y_TASK_ID |                                     |
+-------+-----------+-------------------------------------+
:::

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

# **Refunds** {#DeltaXSEDEDocumentation-Refunds}

Refunds are considered, when appropriate, for jobs that failed due to
circumstances beyond user control.

XSEDE users and project that wish to request a refund should see the
XSEDE Refund Policy section located
[here](https://portal.xsede.org/su-converter#:~:text=RESET-,XSEDE%20Refund%20Policy,-(v1.2)){.external-link
rel="nofollow" target="_blank"}.

Other allocated users and projects wishing to request a refund
should email [help@ncsa.illinois.edu](mailto:help@ncsa.illinois.edu){.external-link
rel="nofollow" target="_blank"}. Please include the batch job ids and
the standard error and output files produced by the job(s). 

# **Visualization** {#DeltaXSEDEDocumentation-Visualization}

Delta A40 nodes support NVIDIA raytracing hardware.

-   [describe visualization capabilities &
    software.]{style="color: rgb(165,173,186);"}
-   [how to establish VNC/DVC/remote
    desktop]{style="color: rgb(165,173,186);"}

# **Containers** {#DeltaXSEDEDocumentation-Containers}

## Singularity {#DeltaXSEDEDocumentation-Singularity}

Container support on Delta is provided by Singularity. 

Docker images can be converted to Singularity sif format via the
`singularity pull ` command. Commands can be run from within a container
using `singularity run` command.

If you encounter quota issues with Singularity caching in
`~/.singularity` , the environment variable `SINGULARITY_CACHEDIR` can
be used to use a different location such as a scratch space. 

Your \$HOME is automatically available from containers run via
Singularity.  You can \"pip3 install \--user\" against a container\'s
python, setup virtualenv\'s or similar while useing a containerized
application.  Just run the container\'s /bin/bash to get a Singularity\>
prompt.  Here\'s an srun example of that with tensorflow:

::: {.code .panel .pdl .conf-macro .output-block style="border-width: 1px;" hasbody="true" macro-name="code"}
::: {.codeHeader .panelHeader .pdl .hide-border-bottom style="border-bottom-width: 1px;"}
**srun the bash from a container to interact with programs inside it**
[[ ]{.expand-control-icon .icon}[Expand
source]{.expand-control-text}]{.collapse-source .expand-control
style=""}
:::

::: {.codeContent .panelContent .pdl .hide-toolbar}
<div>

::: {#highlighter_213490 .syntaxhighlighter .collapsed .sh-confluence .nogutter .java}
::: toolbar
[expand source](#){.toolbar_item .command_expandSource
.expandSource}[?](#){.toolbar_item .command_help .help}
:::

+-----------------------------------------------------------------------+
| ::: {.container title="Hint: double-click to select code"}            |
| ::: {.line .number1 .index0 .alt2}                                    |
| `$ srun \`{.java .plain}                                              |
| :::                                                                   |
|                                                                       |
| ::: {.line .number2 .index1 .alt1}                                    |
| ` `{.java .spaces}`--mem=32g \`{.java .plain}                         |
| :::                                                                   |
|                                                                       |
| ::: {.line .number3 .index2 .alt2}                                    |
| ` `{.java .spaces}`--nodes=`{.java .plain}`1`{.java .value} `\`{.java |
| .plain}                                                               |
| :::                                                                   |
|                                                                       |
| ::: {.line .number4 .index3 .alt1}                                    |
| ` `{.java .spaces}`--ntasks-per-node=`{.java .plain}`1`{.java .value} |
| `\`{.java .plain}                                                     |
| :::                                                                   |
|                                                                       |
| ::: {.line .number5 .index4 .alt2}                                    |
| ` `{.java .spaces}`--cpus-per-task=`{.java .plain}`1`{.java .value}   |
| `\`{.java .plain}                                                     |
| :::                                                                   |
|                                                                       |
| ::: {.line .number6 .index5 .alt1}                                    |
| ` `{.java .spaces}`--partition=gpuA100x4 \`{.java .plain}             |
| :::                                                                   |
|                                                                       |
| ::: {.line .number7 .index6 .alt2}                                    |
| ` `{.java .spaces}`--account=bbka-delta-gpu \`{.java .plain}          |
| :::                                                                   |
|                                                                       |
| ::: {.line .number8 .index7 .alt1}                                    |
| ` `{.java .spaces}`--gpus-per-node=`{.java .plain}`1`{.java .value}   |
| `\`{.java .plain}                                                     |
| :::                                                                   |
|                                                                       |
| ::: {.line .number9 .index8 .alt2}                                    |
| ` `{.java .spaces}`--gpus-per-task=`{.java .plain}`1`{.java .value}   |
| `\`{.java .plain}                                                     |
| :::                                                                   |
|                                                                       |
| ::: {.line .number10 .index9 .alt1}                                   |
| ` `{.java .spaces}`--gpu-bind=verbose,per_task:`{.java                |
| .plain}`1`{.java .value} `\`{.java .plain}                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number11 .index10 .alt2}                                  |
| ` `{.java .spaces}`--pty \`{.java .plain}                             |
| :::                                                                   |
|                                                                       |
| ::: {.line .number12 .index11 .alt1}                                  |
| ` `{.java .spaces}`singularity run --nv \`{.java .plain}              |
| :::                                                                   |
|                                                                       |
| ::: {.line .number13 .index12 .alt2}                                  |
| ` `{.java .spaces}`/sw/external/NGC/tensorflow:`{.java                |
| .plain}`22.02`{.java .value}`-tf2-py3 /bin/bash `{.java .plain}       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number14 .index13 .alt1}                                  |
| `# job starts ...`{.java .plain}                                      |
| :::                                                                   |
|                                                                       |
| ::: {.line .number15 .index14 .alt2}                                  |
| `Singularity> hostname`{.java .plain}                                 |
| :::                                                                   |
|                                                                       |
| ::: {.line .number16 .index15 .alt1}                                  |
| `gpua068.delta.internal.ncsa.edu`{.java .plain}                       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number17 .index16 .alt2}                                  |
| `Singularity> which python  # the python in the container`{.java      |
| .plain}                                                               |
| :::                                                                   |
|                                                                       |
| ::: {.line .number18 .index17 .alt1}                                  |
| `/usr/bin/python`{.java .plain}                                       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number19 .index18 .alt2}                                  |
| `Singularity> python --version`{.java .plain}                         |
| :::                                                                   |
|                                                                       |
| ::: {.line .number20 .index19 .alt1}                                  |
| `Python `{.java .plain}`3.8`{.java .value}`.`{.java .plain}`10`{.java |
| .value}                                                               |
| :::                                                                   |
|                                                                       |
| ::: {.line .number21 .index20 .alt2}                                  |
| `Singularity>`{.java .plain}                                          |
| :::                                                                   |
| :::                                                                   |
+-----------------------------------------------------------------------+
:::

</div>
:::
:::

\

## NVIDIA NGC Containers {#DeltaXSEDEDocumentation-NVIDIANGCContainers}

Delta provides NVIDIA NGC Docker containers that we have pre-built with
Singularity.  Look for the latest binary containers in
**/sw/external/NGC/** . The containers are used as shown in the sample
scripts below:

::: {.code .panel .pdl .conf-macro .output-block style="border-width: 1px;" hasbody="true" macro-name="code"}
::: {.codeHeader .panelHeader .pdl style="border-bottom-width: 1px;"}
**PyTorch example script**
:::

::: {.codeContent .panelContent .pdl}
<div>

::: {#highlighter_512656 .syntaxhighlighter .sh-confluence .nogutter .bash}
::: toolbar
[?](#){.toolbar_item .command_help .help}
:::

+-----------------------------------------------------------------------+
| ::: {.container title="Hint: double-click to select code"}            |
| ::: {.line .number1 .index0 .alt2}                                    |
| `#!/bin/bash`{.bash .preprocessor .bold}                              |
| :::                                                                   |
|                                                                       |
| ::: {.line .number2 .index1 .alt1}                                    |
| `#SBATCH --mem=64g`{.bash .comments}                                  |
| :::                                                                   |
|                                                                       |
| ::: {.line .number3 .index2 .alt2}                                    |
| `#SBATCH --nodes=1`{.bash .comments}                                  |
| :::                                                                   |
|                                                                       |
| ::: {.line .number4 .index3 .alt1}                                    |
| `#SBATCH --ntasks-per-node=1`{.bash .comments}                        |
| :::                                                                   |
|                                                                       |
| ::: {.line .number5 .index4 .alt2}                                    |
| `#SBATCH --cpus-per-tas                                               |
| k=64     # <- match to OMP_NUM_THREADS, 64 requests whole node`{.bash |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number6 .index5 .alt1}                                    |
| `#SBATCH --partition=                                                 |
| gpuA100x4 # <- one of: gpuA100x4 gpuA40x4 gpuA100x8 gpuMI100x8`{.bash |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number7 .index6 .alt2}                                    |
| `#SBATCH --account=bbka-delta-gpu`{.bash .comments}                   |
| :::                                                                   |
|                                                                       |
| ::: {.line .number8 .index7 .alt1}                                    |
| `#SBATCH --job-name=pytorchNGC`{.bash .comments}                      |
| :::                                                                   |
|                                                                       |
| ::: {.line .number9 .index8 .alt2}                                    |
| `### GPU options ###`{.bash .comments}                                |
| :::                                                                   |
|                                                                       |
| ::: {.line .number10 .index9 .alt1}                                   |
| `#SBATCH --gpus-per-node=1`{.bash .comments}                          |
| :::                                                                   |
|                                                                       |
| ::: {.line .number11 .index10 .alt2}                                  |
| `#SBATCH --gpus-per-task=1`{.bash .comments}                          |
| :::                                                                   |
|                                                                       |
| ::: {.line .number12 .index11 .alt1}                                  |
| `#SBATCH --gpu-bind=verbose,per_task:1 `{.bash .comments}             |
| :::                                                                   |
|                                                                       |
| ::: {.line .number13 .index12 .alt2}                                  |
| ` `{.bash .spaces}                                                    |
| :::                                                                   |
|                                                                       |
| ::: {.line .number14 .index13 .alt1}                                  |
| `module reset `{.bash                                                 |
| .plain}`# drop modules and explicitly load the ones needed`{.bash     |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number15 .index14 .alt2}                                  |
| `             `{.bash                                                 |
| .spaces}`# (good job metadata and reproducibility)`{.bash .comments}  |
| :::                                                                   |
|                                                                       |
| ::: {.line .number16 .index15 .alt1}                                  |
| `             `{.bash                                                 |
| .spaces}`# $WORK and $SCRATCH are now set`{.bash .comments}           |
| :::                                                                   |
|                                                                       |
| ::: {.line .number17 .index16 .alt2}                                  |
| `module list  `{.bash .plain}`# job documentation and metadata`{.bash |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number18 .index17 .alt1}                                  |
|                                                                       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number19 .index18 .alt2}                                  |
| `echo`{.bash .functions} `` "job is starting on `hostname`" ``{.bash  |
| .string}                                                              |
| :::                                                                   |
|                                                                       |
| ::: {.line .number20 .index19 .alt1}                                  |
|                                                                       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number21 .index20 .alt2}                                  |
| `#                                                                    |
|  run the container binary with arguments: python3 <program.py>`{.bash |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number22 .index21 .alt1}                                  |
| `singularity run --nv \`{.bash .plain}                                |
| :::                                                                   |
|                                                                       |
| ::: {.line .number23 .index22 .alt2}                                  |
| ` `{.bash .spaces}`/sw/external/NGC/pytorch`{.bash                    |
| .plain}`:22.02-py3 python3 tensor_gpu.py`{.bash .plain}               |
| :::                                                                   |
| :::                                                                   |
+-----------------------------------------------------------------------+
:::

</div>
:::
:::

::: {.code .panel .pdl .conf-macro .output-block style="border-width: 1px;" hasbody="true" macro-name="code"}
::: {.codeHeader .panelHeader .pdl .hide-border-bottom style="border-bottom-width: 1px;"}
**Tensorflow example script** [[ ]{.expand-control-icon .icon}[Expand
source]{.expand-control-text}]{.collapse-source .expand-control
style=""}
:::

::: {.codeContent .panelContent .pdl .hide-toolbar}
<div>

::: {#highlighter_589831 .syntaxhighlighter .collapsed .sh-confluence .nogutter .bash}
::: toolbar
[expand source](#){.toolbar_item .command_expandSource
.expandSource}[?](#){.toolbar_item .command_help .help}
:::

+-----------------------------------------------------------------------+
| ::: {.container title="Hint: double-click to select code"}            |
| ::: {.line .number1 .index0 .alt2}                                    |
| `#!/bin/bash`{.bash .preprocessor .bold}                              |
| :::                                                                   |
|                                                                       |
| ::: {.line .number2 .index1 .alt1}                                    |
| `#SBATCH --mem=64g`{.bash .comments}                                  |
| :::                                                                   |
|                                                                       |
| ::: {.line .number3 .index2 .alt2}                                    |
| `#SBATCH --nodes=1`{.bash .comments}                                  |
| :::                                                                   |
|                                                                       |
| ::: {.line .number4 .index3 .alt1}                                    |
| `#SBATCH --ntasks-per-node=1`{.bash .comments}                        |
| :::                                                                   |
|                                                                       |
| ::: {.line .number5 .index4 .alt2}                                    |
| `#SBATCH --cpus-per-task=64     # <- match to OMP_NUM_THREADS`{.bash  |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number6 .index5 .alt1}                                    |
| `#SBATCH --partition=                                                 |
| gpuA100x4 # <- one of: gpuA100x4 gpuA40x4 gpuA100x8 gpuMI100x8`{.bash |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number7 .index6 .alt2}                                    |
| `#SBATCH --account=bbka-delta-gpu`{.bash .comments}                   |
| :::                                                                   |
|                                                                       |
| ::: {.line .number8 .index7 .alt1}                                    |
| `#SBATCH --job-name=tfNGC`{.bash .comments}                           |
| :::                                                                   |
|                                                                       |
| ::: {.line .number9 .index8 .alt2}                                    |
| `### GPU options ###`{.bash .comments}                                |
| :::                                                                   |
|                                                                       |
| ::: {.line .number10 .index9 .alt1}                                   |
| `#SBATCH --gpus-per-node=1`{.bash .comments}                          |
| :::                                                                   |
|                                                                       |
| ::: {.line .number11 .index10 .alt2}                                  |
| `#SBATCH --gpus-per-task=1`{.bash .comments}                          |
| :::                                                                   |
|                                                                       |
| ::: {.line .number12 .index11 .alt1}                                  |
| `#SBATCH --gpu-bind=verbose,per_task:1`{.bash .comments}              |
| :::                                                                   |
|                                                                       |
| ::: {.line .number13 .index12 .alt2}                                  |
| ` `{.bash .spaces}                                                    |
| :::                                                                   |
|                                                                       |
| ::: {.line .number14 .index13 .alt1}                                  |
| `module reset `{.bash                                                 |
| .plain}`# drop modules and explicitly load the ones needed`{.bash     |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number15 .index14 .alt2}                                  |
| `             `{.bash                                                 |
| .spaces}`# (good job metadata and reproducibility)`{.bash .comments}  |
| :::                                                                   |
|                                                                       |
| ::: {.line .number16 .index15 .alt1}                                  |
| `             `{.bash                                                 |
| .spaces}`# $WORK and $SCRATCH are now set`{.bash .comments}           |
| :::                                                                   |
|                                                                       |
| ::: {.line .number17 .index16 .alt2}                                  |
| `module list  `{.bash .plain}`# job documentation and metadata`{.bash |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number18 .index17 .alt1}                                  |
|                                                                       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number19 .index18 .alt2}                                  |
| `echo`{.bash .functions} `` "job is starting on `hostname`" ``{.bash  |
| .string}                                                              |
| :::                                                                   |
|                                                                       |
| ::: {.line .number20 .index19 .alt1}                                  |
|                                                                       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number21 .index20 .alt2}                                  |
| `#                                                                    |
|  run the container binary with arguments: python3 <program.py>`{.bash |
| .comments}                                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number22 .index21 .alt1}                                  |
| `singularity run --nv \`{.bash .plain}                                |
| :::                                                                   |
|                                                                       |
| ::: {.line .number23 .index22 .alt2}                                  |
| ` `{.bash .spaces}`/sw/external/NGC/tensorflow`{.bash                 |
| .plain}`:22.02-tf2-py3 python3 \`{.bash .plain}                       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number24 .index23 .alt1}                                  |
| ` `{.bash .spaces}`tf_matmul.py`{.bash .plain}                        |
| :::                                                                   |
| :::                                                                   |
+-----------------------------------------------------------------------+
:::

</div>
:::
:::

## Container list (as of March, 2022) {#DeltaXSEDEDocumentation-Containerlist(asofMarch,2022)}

::: {.code .panel .pdl .conf-macro .output-block style="border-width: 1px;" hasbody="true" macro-name="code"}
::: {.codeHeader .panelHeader .pdl style="border-bottom-width: 1px;"}
**catalog.txt**
:::

::: {.codeContent .panelContent .pdl}
<div>

::: {#highlighter_134830 .syntaxhighlighter .sh-confluence .nogutter .java}
::: toolbar
[?](#){.toolbar_item .command_help .help}
:::

+-----------------------------------------------------------------------+
| ::: {.container title="Hint: double-click to select code"}            |
| ::: {.line .number1 .index0 .alt2}                                    |
| `caffe:`{.java .plain}`20.03`{.java .value}`-py3`{.java .plain}       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number2 .index1 .alt1}                                    |
| `caffe2:`{.java .plain}`18.08`{.java .value}`-py3`{.java .plain}      |
| :::                                                                   |
|                                                                       |
| ::: {.line .number3 .index2 .alt2}                                    |
| `cntk:`{.java .plain}`18.08`{.java                                    |
| .value}`-py3 , Microsoft Cognitive Toolkit`{.java .plain}             |
| :::                                                                   |
|                                                                       |
| ::: {.line .number4 .index3 .alt1}                                    |
| `digits:`{.java .plain}`21.09`{.java .value}`-tensorflow-py3`{.java   |
| .plain}                                                               |
| :::                                                                   |
|                                                                       |
| ::: {.line .number5 .index4 .alt2}                                    |
| `lammps:patch_4May2022`{.java .plain}                                 |
| :::                                                                   |
|                                                                       |
| ::: {.line .number6 .index5 .alt1}                                    |
| `matlab:r2021b`{.java .plain}                                         |
| :::                                                                   |
|                                                                       |
| ::: {.line .number7 .index6 .alt2}                                    |
| `mxnet:`{.java .plain}`21.09`{.java .value}`-py3`{.java .plain}       |
| :::                                                                   |
|                                                                       |
| ::: {.line .number8 .index7 .alt1}                                    |
| `namd:`{.java .plain}`2.13`{.java .value}`-multinode`{.java .plain}   |
| :::                                                                   |
|                                                                       |
| ::: {.line .number9 .index8 .alt2}                                    |
| `pytorch:`{.java .plain}`22.02`{.java .value}`-py3`{.java .plain}     |
| :::                                                                   |
|                                                                       |
| ::: {.line .number10 .index9 .alt1}                                   |
| `tensorflow:`{.java .plain}`22.02`{.java .value}`-tf1-py3`{.java      |
| .plain}                                                               |
| :::                                                                   |
|                                                                       |
| ::: {.line .number11 .index10 .alt2}                                  |
| `tensorflow:`{.java .plain}`22.02`{.java .value}`-tf2-py3`{.java      |
| .plain}                                                               |
| :::                                                                   |
|                                                                       |
| ::: {.line .number12 .index11 .alt1}                                  |
| `tensorrt:`{.java .plain}`22.02`{.java .value}`-py3`{.java .plain}    |
| :::                                                                   |
|                                                                       |
| ::: {.line .number13 .index12 .alt2}                                  |
| `theano:`{.java .plain}`18.08`{.java .value}                          |
| :::                                                                   |
|                                                                       |
| ::: {.line .number14 .index13 .alt1}                                  |
| `torch:`{.java .plain}`18.08`{.java .value}`-py2`{.java .plain}       |
| :::                                                                   |
| :::                                                                   |
+-----------------------------------------------------------------------+
:::

</div>
:::
:::

see
also: [https://catalog.ngc.nvidia.com/orgs/nvidia/containers](https://catalog.ngc.nvidia.com/orgs/nvidia/containers){.external-link
rel="nofollow" target="_blank"}

## Other Containers {#DeltaXSEDEDocumentation-OtherContainers}

### Extreme-scale Scientific Software Stack (E4S) {#DeltaXSEDEDocumentation-Extreme-scaleScientificSoftwareStack(E4S)}

The E4S container with GPU (cuda and rocm) support is provided for users
of specific ECP packages made available by the E4S project
([https://e4s-project.github.io/](https://e4s-project.github.io/){.external-link
rel="nofollow" target="_blank"}). The singularity image is available as
:

    /sw/external/E4S/e4s-gpu-x86_64.sif

    To use E4S with NVIDIA GPUs

::: {.code .panel .pdl .conf-macro .output-block style="border-width: 1px;" hasbody="true" macro-name="code"}
::: {.codeContent .panelContent .pdl}
<div>

::: {#highlighter_120219 .syntaxhighlighter .sh-confluence .nogutter .java}
::: toolbar
[?](#){.toolbar_item .command_help .help}
:::

+-----------------------------------------------------------------------+
| ::: {.container title="Hint: double-click to select code"}            |
| ::: {.line .number1 .index0 .alt2}                                    |
| `$                                                                    |
|  srun --account=account_name --partition=gpuA100-interactive \`{.java |
| .plain}                                                               |
| :::                                                                   |
|                                                                       |
| ::: {.line .number2 .index1 .alt1}                                    |
| `  `{.java .spaces}`--nodes=`{.java .plain}`1`{.java .value}          |
| `--gpus-per-node=`{.java .plain}`1`{.java .value} `--tasks=`{.java    |
| .plain}`1`{.java .value} `--tasks-per-node=`{.java .plain}`1`{.java   |
| .value} `\ `{.java .plain}                                            |
| :::                                                                   |
|                                                                       |
| ::: {.line .number3 .index2 .alt2}                                    |
| `  `{.java .spaces}`--cpus-per-task=`{.java .plain}`1`{.java .value}  |
| `--mem=20g \`{.java .plain}                                           |
| :::                                                                   |
|                                                                       |
| ::: {.line .number4 .index3 .alt1}                                    |
| `  `{.java .spaces}`--pty bash`{.java .plain}                         |
| :::                                                                   |
|                                                                       |
| ::: {.line .number5 .index4 .alt2}                                    |
| `$ sin                                                                |
| gularity exec --cleanenv /sw/external/E4S/e4s-gpu-x86_64.sif \`{.java |
| .plain}                                                               |
| :::                                                                   |
|                                                                       |
| ::: {.line .number6 .index5 .alt1}                                    |
| `  `{.java .spaces}`/bin/bash --rcfile /etc/bash.bashrc`{.java        |
| .plain}                                                               |
| :::                                                                   |
| :::                                                                   |
+-----------------------------------------------------------------------+
:::

</div>
:::
:::

The spack package inside of the image will interact with a local spack
installation. If  \~/.spack directory exists, it might need to be
renamed. 

More information can be found at
[https://e4s-project.github.io/download.html](https://e4s-project.github.io/download.html){.external-link
rel="nofollow" target="_blank"}

# [**Protected Data (N/A)**]{style="color: rgb(165,173,186);"} {#DeltaXSEDEDocumentation-ProtectedData(N/A)}

\...

# **Help** {#DeltaXSEDEDocumentation-Help}

For assistance with the use of Delta

-   XSEDE users can create a ticket via the user portal at
    [https://portal.xsede.org/web/xup/help-desk](https://portal.xsede.org/web/xup/help-desk){.external-link
    rel="nofollow" target="_blank"}
-   All other users (Illinois allocations, Diversity Allocations, etc)
    please send email to help@ncsa.illinois.edu.

# **Acknowledge** {#DeltaXSEDEDocumentation-Acknowledge}

To acknowledge the NCSA Delta system in particular, please include the
following

This research is part of the Delta research computing project, which is
supported by the National Science Foundation (award OCI 2005572), and
the State of Illinois. Delta is a joint effort of the University of
Illinois at Urbana-Champaign and its National Center for Supercomputing
Applications.

To include acknowledgement of XSEDE contributions to a publication or
presentation please see
[https://portal.xsede.org/acknowledge](https://portal.xsede.org/acknowledge){.external-link
rel="nofollow" target="_blank"} and
[https://www.xsede.org/for-users/acknowledgement](https://www.xsede.org/for-users/acknowledgement){.external-link
rel="nofollow" target="_blank"}.

# **References** {#DeltaXSEDEDocumentation-References}

Supporting documentation resources:

[https://www.rcac.purdue.edu/knowledge/anvil](https://www.rcac.purdue.edu/knowledge/anvil){.external-link
rel="nofollow" target="_blank"}

[https://nero-docs.stanford.edu/jupyter-slurm.html](https://nero-docs.stanford.edu/jupyter-slurm.html){.external-link
rel="nofollow" target="_blank"}

\
:::
