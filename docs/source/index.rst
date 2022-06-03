.. container:: innerCell

   .. rubric:: **Delta User Guide**
      :name: DeltaXSEDEDocumentation-topDeltaUserGuide

   *Last update: April 7, 2022*

   .. rubric:: **Status Updates and Notices**
      :name: DeltaXSEDEDocumentation-StatusUpdatesandNotices

   *Delta* is tentatively scheduled to enter production in Q2 2022.

   .. container::
   confluence-information-macro confluence-information-macro-information conf-macro output-block

      key: light grey font is work in progress

      .. container:: confluence-information-macro-body

         Items in light grey font are in progress and coming soon. 
         Example software or feature not yet implemented.

   .. rubric:: **Introduction**
      :name: DeltaXSEDEDocumentation-Introduction

   *Delta* is a dedicated, `eXtreme Science and Engineering Science
   Discovery Environment (XSEDE) <http://www.xsede.org>`__ allocated
   resource designed by HPE and NCSA, delivering a highly capable
   GPU-focused compute environment for GPU and CPU workloads.  Besides
   offering a mix of standard and reduced precision GPU resources,
   *Delta* also offers GPU-dense nodes with both NVIDIA and AMD GPUs. 
   *Delta* provides high performance node-local SSD scratch filesystems,
   as well as both standard lustre and relaxed-POSIX parallel
   filesystems spanning the entire resource.

   *Delta's* CPU nodes are each powered by two 64-core AMD EPYC 7763
   ("Milan") processors, with 256 GB of DDR4 memory.  The *Delta* GPU
   resource has four node types: one with 4 NVIDIA A100 GPUs (40 GB HBM2
   RAM each) connected via NVLINK and 1 64-core AMD EPYC 7763 ("Milan")
   processor, the second with 4 NVIDIA A40 GPUs (48 GB GDDR6 RAM)
   connected via PCIe 4.0 and 1 64-core AMD EPYC 7763 ("Milan")
   processor, the third with 8 NVIDIA A100 GPUs in a dual socket AMD
   EPYC 7763 (128-cores per node) node with 2 TB of DDR4 RAM and
   NVLINK,  and the fourth with 8 AMD MI100 GPUs (32GB HBM2 RAM each) in
   a dual socket AMD EPYC 7763 (128-cores per node) node with 2 TB of
   DDR4 RAM and PCIe 4.0. 

   *Delta* has 124 standard CPU nodes, 100 4-way A100-based GPU nodes,
   100 4-way A40-based GPU nodes, 5 8-way A100-based GPU nodes, and 1
   8-way MI100-based GPU node.  Every *Delta* node has high-performance
   node-local SSD storage (740 GB for CPU nodes, 1.5 TB for GPU nodes),
   and is connected to the 7 PB Lustre parallel filesystem via the
   high-speed interconnect.  The *Delta* resource uses the SLURM
   workload manager for job scheduling.  

   Delta supports the `XSEDE core software
   stack <https://www.xsede.org/software>`__, including remote login,
   remote computation, data movement, science workflow support, and
   science gateway support toolkits.

   | 

   **|image1|**

   **Figure 1. Delta System**

   Delta is supported by the National Science Foundation under Grant No.
   OAC-2005572.

   Any opinions, findings, and conclusions or recommendations expressed
   in this material are those of the author(s) and do not necessarily
   reflect the views of the National Science Foundation.

   .. container:: table-wrap

      +-------------------------------------+
      | *Delta* is now accepting proposals. |
      +-------------------------------------+

   `Top of Page <#DeltaXSEDEDocumentation-top>`__

   .. rubric:: **Account Administration**
      :name: DeltaXSEDEDocumentation-AccountAdministration

   -  For XSEDE projects please use the `XSEDE user
      portal <https://portal.xsede.org/my-xsede>`__ for project and
      account management.
   -  Non-XSEDE Account and Project administration is handled by NCSA
      Identity and NCSA group management tools. For more information
      please see the `NCSA Allocation and Account
      Management </display/USSPPRT/User+Services+NCSA+Allocation+and+Account+Management>`__
      documentation page. 

   .. rubric:: **Configuring Your Account**
      :name: DeltaXSEDEDocumentation-ConfiguringYourAccount

   -  bash is the default shell, submit a support request to change your
      default shell
   -  environment variables: XSEDE CUE, `SLURM
      batch <#DeltaXSEDEDocumentation-slurm_environment_batch>`__
   -  using `Modules <#DeltaXSEDEDocumentation-lmod>`__ 

   .. rubric:: **System Architecture**
      :name: DeltaXSEDEDocumentation-SystemArchitecture

   Delta is designed to help applications transition from CPU-only to
   GPU or hybrid CPU-GPU codes. Delta has some important architectural
   features to facilitate new discovery and insight:

   -  a single processor architecture (AMD) across all node types: CPU
      and GPU
   -  support for NVIDIA A100 MIG GPU partitioning allowing for
      fractional use of the A100s if your workload isn't able to exploit
      an entire A100 efficiently
   -  ray tracing hardware support from the NVIDIA A40 GPUs
   -  9 large memory (2 TB) nodes 
   -  a low latency and high bandwidth HPE/Cray Slingshot interconnect
      between compute nodes
   -  lustre for home, projects and scratch file systems
   -  support for relaxed and non-posix IO
   -  shared-node jobs and the single core and single MIG GPU slice
   -  Resources for persistent services in support of Gateways, Open
      OnDemand, Data Transport nodes..., 
   -  Unique AMD MI-100 resource  

   .. rubric:: **Model Compute Nodes**
      :name: DeltaXSEDEDocumentation-ModelComputeNodes

   The Delta compute ecosystem is composed of 5 node types: dual-socket
   CPU-only compute nodes, single socket 4-way NVIDIA A100 GPU compute
   nodes, single socket 4-way NVIDIA A40 GPU compute nodes, dual-socket
   8-way NVIDIA A100 GPU compute nodes, and a single socket 8-way AMD
   MI100 GPU compute nodes. The CPU-only and 4-way GPU nodes have 256 GB
   of RAM per node while the 8-way GPU nodes have 2 TB of RAM. The
   CPU-only node has 0.74 TB of local storage while all GPU nodes have
   1.5 TB of local storage.

   .. rubric:: Table. CPU Compute Node Specifications
      :name: DeltaXSEDEDocumentation-Table.CPUComputeNodeSpecifications

   .. container:: table-wrap

      +----------------------------------+----------------------------------+
      | .. container::                   | .. container::                   |
      | tablesorter-header-inner         | tablesorter-header-inner         |
      |                                  |                                  |
      |    Specification                 |    Value                         |
      +==================================+==================================+
      | Number of nodes                  | 124                              |
      +----------------------------------+----------------------------------+
      | CPU                              | AMD EPYC 7763                    |
      |                                  | "Milan" (PCIe Gen4)              |
      +----------------------------------+----------------------------------+
      | Sockets per node                 | 2                                |
      +----------------------------------+----------------------------------+
      | Cores per socket                 | 64                               |
      +----------------------------------+----------------------------------+
      | Cores per node                   | 128                              |
      +----------------------------------+----------------------------------+
      | Hardware threads per core        | 1                                |
      +----------------------------------+----------------------------------+
      | Hardware threads per node        | 128                              |
      +----------------------------------+----------------------------------+
      | Clock rate (GHz)                 | ~ 2.45                           |
      +----------------------------------+----------------------------------+
      | RAM (GB)                         | 256                              |
      +----------------------------------+----------------------------------+
      | Cache (KB) L1/L2/L3              |  64/512/32768                    |
      +----------------------------------+----------------------------------+
      | Local storage (TB)               | 0.74 TB                          |
      +----------------------------------+----------------------------------+

   The AMD CPUs are set for 4 NUMA domains per socket (NPS=4). 

   .. rubric:: Table. 4-way NVIDIA A40 GPU Compute Node Specifications 
      :name: DeltaXSEDEDocumentation-Table.4-wayNVIDIAA40GPUComputeNodeSpecifications

   .. container:: table-wrap

      .. container:: tablesorter-header-inner

         Specification

.. container:: tablesorter-header-inner

   Value

Number of nodes

100

GPU

NVIDIA A40 

(`Vendor page <https://www.nvidia.com/en-us/data-center/a40/#specs>`__)

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

~ 2.45

RAM (GB)

256

Cache (KB) L1/L2/L3

&nbsng affinitization to NUMA nodes on the CPU. Note that the
relationship between GPU index and NUMA domain are inverse.

Table. 4-way NVIDIA A40 Mapping and GPU-CPU AffeTd">GPU1

SYS

X

SYS

SYS

SYS

32-47

| 

Table Legend

X    = Self
SYS  = ConneaderRow">

NVIDIA A100

(`Vendor
page <https://www.nvidia.com/en-us/data-center/a100/#specifications>`__)

fluenceTd">1

Hardware threads per node

64

Clock rate (GHz)

~ 2.45

| 

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

| PHB  = Connection traversing PCIe as well as a PCIe Host Bridge
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
apply an ascending sort" style="user-select: none;">

.. container:: confluenceTd

   GPUs per node

8

GPU Memory (GB)

32

CPU

~ 2.45

RAM (GB)

**Network**

Delta is connected to the nd it has 6PB of usable space.  These file
systems run Lustre via DDN's ExaScaler 6 stack (Lustre 2.14 based).

*Hardware:
*\ DDN SFA7990XE (Quantity: 3), each unit contains

-  One additional SS9012 enclosure
-  168 x 16TB SAS Drives
-  7 x 1.92TB SAS SSDs

The $HOME file system has ort applied, activate to apply an ascending
sort" style="user-select: none;">

.. container:: tablesorter-header-inner

   File size

*Hardware:
*\ DDN SFA400NVXE (Quanup>

.. container:: tables

   $HOME

**25GB. **\ 400,000 files per u="1" class="confluenceTd">No

Yes; files older than 30-days (access time)

Area for computation, largest allocEDocumentation-login_nodes"
data-hasbody="false" data-macro-name="anchor">

Direct access to the Delta login nodene" aria-label="example usage with
ssh: No sort applied, activate to apply an ascending sort"
style="user-select: none;">

.. container:: tablesorter-header-inner

   eedu

If needed, XSEDE users can lookup their local username
at\ ` <https://portal.xsede.org/group/xup/accounts>`__

maintaining persistent sessions: tmux

.. container:: confluence-information-macro-body

   tmux is available on the login nodes to maintain persistent sessse
   execute the gsissh command with the “-vvv” option and include the
   verbose output in your problem description.

   Once on the XSEDE SSO hub:

   .. container:: coder sh-confluence nogutter java

      .. container:: toolbar

         ` <#>`__

         .. container::

         .. container:: line number6 index5 alt1

             

         .. container:: line number7 index6 alt2

            ``$``\ nbsp;  delta_abcd      4096 Feb 21 11:54
            ``/scratch/abcd``

         .. container:: line number15 i </p><p><br></p><div class=

            rsync - to be used for small to modest transfers to avoid
            impacting the usability of the Delta login node. 

            -  Sharing Files with Collaborators

               Building Software. 

               |image2|

               .. container:: tablesorter-header-inner

                  gcc

            -  Launching One Hybrid (MPI+Threads) Application

            -  More Than One Serial Applicati>Keep in mind that your
               charges are based on the resources that are reserved de

            .. rubric:: **Accessing the Compute Nodes**
               :name: DeltaXSEDEDocumentation-AccessingtheComputeNodes

            Deltamittingfor detail number2 index1
            alt1">\ ``             ``

            .. container:: codeContent panelContent pdl

               .. container::

                  ``252`` ``GB``

               .. container:: tablesorter-headerRow

                  TBD

30 min30 min

TBDTBD

2.0

1.n-sviewviewofslurmpartitions">sview view of slurm partitions

`? <#>`__

single core class="java plain">--tasks=\ ``1`` ``\``

.. container:: line number3 index2 alt2

   ``  ``\ ``--tasks-per-node=``\ ``1`` ``--cpus-per-task=``\ ``1``
   applications from within them.  Use *mpirun* to launch mpi jobs from
   within an interactive job.  Within standard batch jobs submitted via
   sbatch, use *srun* to launch MPI codes.

.. _DeltaXSEDEDocumentation-InteractiveX11Support:

Interactive X11 Support
~~~~~~~~~~~~~~~~~~~~~~~

To run an X11 based application on a compute node in an interactive
session, the use of the ``--x11`` switch with ``srun`` is needed. For
example, to run a single core job that uses 1g of memory with X11 (in
this" title="Hint: double-click to select code">

.. container:: line number1 index0 alt2

   ``srun -A abcd-delta-cpu  --partition=cpu \``

.. container:: line number2 index1 alt1

   ``  ``\ ``--nodes=``\ ``1`` ``--tasks=``\ ``1``
   ``--tasks-per-node=``\ ````

   -  Serial jobs

      .. container:: code panel pdl conf-macro output-block

         .. container:: codeHeader panelHeader pdl

            **serial example script**\ ``#!/bin/bash``

         .. container:: line number3 index2 alt2

            ``#SBATCH --mem=16g``

         .. container:: line number4 index3 alt1

            ``#SBATCH --nodes=1``

         .. container:: line number5 index4 alt2

            ``#SBATCH --ntasks-per-node=1``

         .. container:: line number6 index5 alt1

            ``#SBATCH --cpus-per-task=1    # <- match to OMP_NUM_THREADS``

         .. container:: line number7 index6 alt2

            ``#SBA      # hh:mm:ss for the job``

         .. container:: line number11 index10 alt2

            ``### GPU options ###``

         .. container:: line number12 index11 alt1

            ``##SBATCH --gpus-per-node=2``

         .. container:: line number13 index12 alt2

            ``##SBATCH --gpu-bind=none     # <- or closest``

         .. container:: line number14 index13 alt1

            ``##SBATCH --mail-user=you@yourinstitution.edu``

         .. container:: line number15 index14 alt2

            ``##SBATCH --mail-type="BEGIN,END" See sbatch or srun man pages for more email options ``

         .. container:: line number16 index15 alt1

             

         .. container:: line number17 index16 alt2

             

         .. container:: line index18 alt2

            ``             ``\ ``# (good job metadata and reproducibility)``

         .. container:: line number20 index19 alt1

            ``             ``\ ``# $WORK and $SCRATCH are notd>``
            MPI  

            .. container:: code panel pdl conf-macro output-block

               .. container::
               codeHeader panelHeader pdl hide-border-bottom

                  **mpi example script**

                  .. container:: line number1 index0 alt2

                     ``#!/bin/bash``

                  .. container:: line number2 index1 alt1

                     ``#SBATCH --mem=16g``

                  ``#SBATCH --time=00:10:00      # hh:mm:ss for the job``

               .. container:: line number10 index9 alt1

                  ``### GPU options ###``

               .. container:: line number11 index10 alt2

                  ``##SBATCH --gpus-per-node=2``

               .. container:: line number12 index11 alt1

                  ``##SBATCH --gpu-bind=none     # <- or closest ##SBATCH --mp;      ``\ ``# (good job metadata and reproducibility)``

               .. container:: line number17 index16 alt2

                  ``             ``\ ``# $WORK and $SCRATCH are now set``

               .. container:: line number18 index17 alt1

                  ``module load gcc``\ ``/11``\ ``.2.0 openmpi ``\ ``# ... or any appropriate modules``

               .. container::
               line numbody></table></div></div> </div> </div></li><li><p class=

                  OpenMP   

                  .. container:: code panel pdl conf-macro output-block

                     .. container::
                     codeHeader panelHeader pdl hide-border-bottom

                        **openmp example script**  Expand sourcetd
                        class="code">

                        .. container::

                           .. container:: line number1 index0 alt2

                              ``#!/bin/bash``

                           .. container:: line number2 index1 alt1

                              ``#SBATCH --mem=16g``

                           .. container:: line number3 index2 alt2

                              ``#SBATCH --nodes=1``

                           .. container:: line number4 index3 alt1

                              ``#SBATCH --ntasks-per-node=1``

                              .. container:: line number9 index8 alt2

                                 ``#SBATCH --time=00:10:00      # hh:mm:ss for the job``

                              .. container:: line number10 index9 alt1

                                 ``### GPU options ###``

                              .. container:: line number11 index10 alt2

                                 ``##SBATCH --gpus-per-node=2``

                              .. container:: line number12 index11 alt1

                                 ``##SBATCH --gpu-bind=none     # <- o needed``

                              .. container:: line number17 index16 alt2

                                 ``             ``\ ``# (good job metadata and reproducibility)``

                              .. container:: line number18 index17 alt1

                                 ``             ``\ ``# $WORK and $SCRATCH are now set``

                              .. container:: line number19 index18 alt2

                                 ``mo/div>``

                                 .. container::
                                 line number22 index21 alt1

                                    ``export`` ``OMP_NUM_THREADS=32``

                                 .. container::
                                 line number23 index22 alt2

                                    ``srun stream_gcc``

   -  Parametric / Array / HTC jobs

   .. rubric:: **Job Management **
      :name: DeltaXSEDEDocumentation-JobManagement

   Batch jobs are submitted through a *job script*  (as in the examples
   above) using the sbatch command. Job scripts generally start with a
   series of SLURM *directives* that describe requirements of the job
   such as number of nodes, wall time required, etc… to the batch
   system/scheduler (SLURM directives can also be specified as options
   on the sbatch command line; command line options take precedence over
   those in the script). The rest of the batch script consists of user
   commands.

   The syntax for sbatch is:

   **sbatch** [list of sbatch options] script_name

   Refer to the sbatch man page for detailed information on the options.

   .. rubric:: squeue/scontrol/sinfo
      :name: DeltaXSEDEDocumentation-squeue/scontrol/sinfo

   Commands that display batch job and partition information .

   .. container:: table-wrap

      +----------------------------------+----------------------------------+
      | .. container::                   | .. container::                   |
      | tablesorter-header-inner         | tablesorter-header-inner         |
      |                                  |                                  |
      |    SLURM EXAMPLE COMMAND         |    DESCRIPTION                   |
      +==================================+==================================+
      | squeue -a                        | List the status of all jobs on   |
      |                                  | the system.                      |
      +----------------------------------+----------------------------------+
      | squeue -u $USER                  | List the status of all your jobs |
      |                                  | in the batch system.             |
      +----------------------------------+----------------------------------+
      | squeue -j JobID                  | List nodes allocated to a        |
      |                                  | running job in addition to basic |
      |                                  | information..                    |
      +----------------------------------+----------------------------------+
      | scontrol show job JobID          | List detailed information on a   |
      |                                  | particular job.                  |
      +----------------------------------+----------------------------------+
      | sinfo -a                         | List summary information on all  |
      |                                  | the partition.                   |
      +----------------------------------+----------------------------------+

   See the manual (man) pages for other available options.

   | 

   Useful Batch Job Environment Variables

   .. container:: table-wrap

      +-------+-----------+-------------------------------------+
      | .. c  | .. co     | .. container::                      |
      | ontai | ntainer:: | tablesorter-header-inner            |
      | ner:: | tables    |                                     |
      | tabl  | orter-hea |    DETAIL DESCRIPTION               |
      | esort | der-inner |                                     |
      | er-he |           |                                     |
      | ader- |    SLURM  |                                     |
      | inner |    EN     |                                     |
      |       | VIRONMENT |                                     |
      |    D  |           |                                     |
      | ESCRI |  VARIABLE |                                     |
      | PTION |           |                                     |
      +=======+===========+=====================================+
      | JobID | $SLU      | Job identifier assigned to the job  |
      |       | RM_JOB_ID |                                     |
      +-------+-----------+-------------------------------------+
      | Job   | $SLURM_S  | By default, jobs start in the       |
      | Submi | UBMIT_DIR | directory that the job was          |
      | ssion |           | submitted from. So the "cd          |
      | Dire  |           | $SLURM_SUBMIT_DIR" command is not   |
      | ctory |           | needed.                             |
      +-------+-----------+-------------------------------------+
      | Mac   | $SLURM    | variable name that contains the     |
      | hine( | _NODELIST | list of nodes assigned to the batch |
      | node) |           | job                                 |
      | list  |           |                                     |
      +-------+-----------+-------------------------------------+
      | Array | $         | each member of a job array is       |
      | JobID | SLURM_ARR | assigned a unique identifier        |
      |       | AY_JOB_ID |                                     |
      |       | $S        |                                     |
      |       | LURM_ARRA |                                     |
      |       | Y_TASK_ID |                                     |
      +-------+-----------+-------------------------------------+

   See the sbatch man page for additional environment variables
   available.

   **srun**

   The srun command initiates an interactive job on the compute nodes.

   For example, the following command:

   ``srun -A account_name --time=00:30:00 --nodes=1 --ntasks-per-node=64 \``

   ``--mem=16g --pty /bin/bash``

   will run an interactive job in the default queue with a wall clock
   limit of 30 minutes, using one node and 16 cores per node. You can
   also use other sbatch options such as those documented above.

   After you enter the command, you will have to wait for SLURM to start
   the job. As with any job, your interactive job will wait in the queue
   until the specified number of nodes is available. If you specify a
   small number of nodes for smaller amounts of time, the wait should be
   shorter because your job will backfill among larger jobs. You will
   see something like this:

   ``srun: job 123456 queued and waiting for resources``

   Once the job starts, you will see:

   ``srun: job 123456 has been allocated resources``

   and will be presented with an interactive shell prompt on the launch
   node. At this point, you can use the appropriate command to start
   your program.

   When you are done with your work, you can use the exit command to end
   the job.

   **scancel**

   The scancel command deletes a queued job or terminates a running job.

   -  scancel JobID deletes/terminates a job.

   .. rubric:: **Refunds**
      :name: DeltaXSEDEDocumentation-Refunds

   Refunds are considered, when appropriate, for jobs that failed due to
   circumstances beyond user control.

   XSEDE users and project that wish to request a refund should see the
   XSEDE Refund Policy section located
   `here <https://portal.xsede.org/su-converter#:~:text=RESET-,XSEDE%20Refund%20Policy,-(v1.2)>`__.

   Other allocated users and projects wishing to request a refund
   should email \ help@ncsa.illinois.edu. Please include the batch job
   ids and the standard error and output files produced by the job(s). 

   .. rubric:: **Visualization**
      :name: DeltaXSEDEDocumentation-Visualization

   Delta A40 nodes support NVIDIA raytracing hardware.

   -  describe visualization capabilities & software.
   -  how to establish VNC/DVC/remote desktop

   .. rubric:: **Containers**
      :name: DeltaXSEDEDocumentation-Containers

   .. rubric:: Singularity
      :name: DeltaXSEDEDocumentation-Singularity

   Container support on Delta is provided by Singularity. 

   Docker images can be converted to Singularity sif format via the
   ``singularity pull`` command. Commands can be run from within a
   container using ``singularity run``\  command.

   If you encounter quota issues with Singularity caching in
   ``~/.singularity`` , the environment variable
   ``SINGULARITY_CACHEDIR`` can be used to use a different location such
   as a scratch space. 

   Your $HOME is automatically available from containers run via
   Singularity.  You can "pip3 install --user" against a container's
   python, setup virtualenv's or similar while useing a containerized
   application.  Just run the container's /bin/bash to get a
   Singularity> prompt.  Here's an srun example of that with tensorflow:

   .. container:: code panel pdl conf-macro output-block

      .. container:: codeHeader panelHeader pdl hide-border-bottom

         **srun the bash from a container to interact with programs
         inside it**  Expand source

      .. container:: codeContent panelContent pdl hide-toolbar

         .. container::

            .. container::
            syntaxhighlighter collapsed sh-confluence nogutter java
               :name: highlighter_213490

               .. container:: toolbar

                  `expand source <#>`__\ `? <#>`__

               +-----------------------------------------------------------------------+
               | .. container::                                                        |
               |                                                                       |
               |    .. container:: line number1 index0 alt2                            |
               |                                                                       |
               |       ``$ srun \``                                                    |
               |                                                                       |
               |    .. container:: line number2 index1 alt1                            |
               |                                                                       |
               |       `` ``\ ``--mem=32g \``                                          |
               |                                                                       |
               |    .. container:: line number3 index2 alt2                            |
               |                                                                       |
               |       `` ``\ ``--nodes=``\ ``1`` ``\``                                |
               |                                                                       |
               |    .. container:: line number4 index3 alt1                            |
               |                                                                       |
               |       `` ``\ ``--ntasks-per-node=``\ ``1`` ``\``                      |
               |                                                                       |
               |    .. container:: line number5 index4 alt2                            |
               |                                                                       |
               |       `` ``\ ``--cpus-per-task=``\ ``1`` ``\``                        |
               |                                                                       |
               |    .. container:: line number6 index5 alt1                            |
               |                                                                       |
               |       `` ``\ ``--partition=gpuA100x4 \``                              |
               |                                                                       |
               |    .. container:: line number7 index6 alt2                            |
               |                                                                       |
               |       `` ``\ ``--account=bbka-delta-gpu \``                           |
               |                                                                       |
               |    .. container:: line number8 index7 alt1                            |
               |                                                                       |
               |       `` ``\ ``--gpus-per-node=``\ ``1`` ``\``                        |
               |                                                                       |
               |    .. container:: line number9 index8 alt2                            |
               |                                                                       |
               |       `` ``\ ``--gpus-per-task=``\ ``1`` ``\``                        |
               |                                                                       |
               |    .. container:: line number10 index9 alt1                           |
               |                                                                       |
               |       `` ``\ ``--gpu-bind=verbose,per_task:``\ ``1`` ``\``            |
               |                                                                       |
               |    .. container:: line number11 index10 alt2                          |
               |                                                                       |
               |       `` ``\ ``--pty \``                                              |
               |                                                                       |
               |    .. container:: line number12 index11 alt1                          |
               |                                                                       |
               |       `` ``\ ``singularity run --nv \``                               |
               |                                                                       |
               |    .. container:: line number13 index12 alt2                          |
               |                                                                       |
               |       `` ``                                                           |
               | \ ``/sw/external/NGC/tensorflow:``\ ``22.02``\ ``-tf2-py3 /bin/bash`` |
               |                                                                       |
               |    .. container:: line number14 index13 alt1                          |
               |                                                                       |
               |       ``# job starts ...``                                            |
               |                                                                       |
               |    .. container:: line number15 index14 alt2                          |
               |                                                                       |
               |       ``Singularity> hostname``                                       |
               |                                                                       |
               |    .. container:: line number16 index15 alt1                          |
               |                                                                       |
               |       ``gpua068.delta.internal.ncsa.edu``                             |
               |                                                                       |
               |    .. container:: line number17 index16 alt2                          |
               |                                                                       |
               |       ``Singularity> which python  # the python in the container``    |
               |                                                                       |
               |    .. container:: line number18 index17 alt1                          |
               |                                                                       |
               |       ``/usr/bin/python``                                             |
               |                                                                       |
               |    .. container:: line number19 index18 alt2                          |
               |                                                                       |
               |       ``Singularity> python --version``                               |
               |                                                                       |
               |    .. container:: line number20 index19 alt1                          |
               |                                                                       |
               |       ``Python``\ ``3.8``\ ``.``\ ``10``                              |
               |                                                                       |
               |    .. container:: line number21 index20 alt2                          |
               |                                                                       |
               |       ``Singularity>``                                                |
               +-----------------------------------------------------------------------+

   | 

   .. rubric:: NVIDIA NGC Containers
      :name: DeltaXSEDEDocumentation-NVIDIANGCContainers

   Delta provides NVIDIA NGC Docker containers that we have pre-built
   with Singularity.  Look for the latest binary containers in
   **/sw/external/NGC/** . The containers are used as shown in the
   sample scripts below:

   .. container:: code panel pdl conf-macro output-block

      .. container:: codeHeader panelHeader pdl

         **PyTorch example script**

      .. container:: codeContent panelContent pdl

         .. container::

            .. container:: syntaxhighlighter sh-confluence nogutter bash
               :name: highlighter_512656

               .. container:: toolbar

                  `? <#>`__

               +-----------------------------------------------------------------------+
               | .. container::                                                        |
               |                                                                       |
               |    .. container:: line number1 index0 alt2                            |
               |                                                                       |
               |       ``#!/bin/bash``                                                 |
               |                                                                       |
               |    .. container:: line number2 index1 alt1                            |
               |                                                                       |
               |       ``#SBATCH --mem=64g``                                           |
               |                                                                       |
               |    .. container:: line number3 index2 alt2                            |
               |                                                                       |
               |       ``#SBATCH --nodes=1``                                           |
               |                                                                       |
               |    .. container:: line number4 index3 alt1                            |
               |                                                                       |
               |       ``#SBATCH --ntasks-per-node=1``                                 |
               |                                                                       |
               |    .. container:: line number5 index4 alt2                            |
               |                                                                       |
               |       ``#SBATCH --cpus-pe                                             |
               | r-task=64     # <- match to OMP_NUM_THREADS, 64 requests whole node`` |
               |                                                                       |
               |    .. container:: line number6 index5 alt1                            |
               |                                                                       |
               |       ``#SBATCH --parti                                               |
               | tion=gpuA100x4 # <- one of: gpuA100x4 gpuA40x4 gpuA100x8 gpuMI100x8`` |
               |                                                                       |
               |    .. container:: line number7 index6 alt2                            |
               |                                                                       |
               |       ``#SBATCH --account=bbka-delta-gpu``                            |
               |                                                                       |
               |    .. container:: line number8 index7 alt1                            |
               |                                                                       |
               |       ``#SBATCH --job-name=pytorchNGC``                               |
               |                                                                       |
               |    .. container:: line number9 index8 alt2                            |
               |                                                                       |
               |       ``### GPU options ###``                                         |
               |                                                                       |
               |    .. container:: line number10 index9 alt1                           |
               |                                                                       |
               |       ``#SBATCH --gpus-per-node=1``                                   |
               |                                                                       |
               |    .. container:: line number11 index10 alt2                          |
               |                                                                       |
               |       ``#SBATCH --gpus-per-task=1``                                   |
               |                                                                       |
               |    .. container:: line number12 index11 alt1                          |
               |                                                                       |
               |       ``#SBATCH --gpu-bind=verbose,per_task:1``                       |
               |                                                                       |
               |    .. container:: line number13 index12 alt2                          |
               |                                                                       |
               |       `` ``                                                           |
               |                                                                       |
               |    .. container:: line number14 index13 alt1                          |
               |                                                                       |
               |       ``m                                                             |
               | odule reset``\ ``# drop modules and explicitly load the ones needed`` |
               |                                                                       |
               |    .. container:: line number15 index14 alt2                          |
               |                                                                       |
               |                                                                       |
               |      ``             ``\ ``# (good job metadata and reproducibility)`` |
               |                                                                       |
               |    .. container:: line number16 index15 alt1                          |
               |                                                                       |
               |       ``             ``\ ``# $WORK and $SCRATCH are now set``         |
               |                                                                       |
               |    .. container:: line number17 index16 alt2                          |
               |                                                                       |
               |       ``module list ``\ ``# job documentation and metadata``          |
               |                                                                       |
               |    .. container:: line number18 index17 alt1                          |
               |                                                                       |
               |                                                                       |
               |                                                                       |
               |    .. container:: line number19 index18 alt2                          |
               |                                                                       |
               |       ``echo`` :literal:`"job is starting on `hostname`"`             |
               |                                                                       |
               |    .. container:: line number20 index19 alt1                          |
               |                                                                       |
               |                                                                       |
               |                                                                       |
               |    .. container:: line number21 index20 alt2                          |
               |                                                                       |
               |                                                                       |
               |   ``# run the container binary with arguments: python3 <program.py>`` |
               |                                                                       |
               |    .. container:: line number22 index21 alt1                          |
               |                                                                       |
               |       ``singularity run --nv \``                                      |
               |                                                                       |
               |    .. container:: line number23 index22 alt2                          |
               |                                                                       |
               |       `` `                                                            |
               | `\ ``/sw/external/NGC/pytorch``\ ``:22.02-py3 python3 tensor_gpu.py`` |
               +-----------------------------------------------------------------------+

   .. container:: code panel pdl conf-macro output-block

      .. container:: codeHeader panelHeader pdl hide-border-bottom

         **Tensorflow example script**  Expand source

      .. container:: codeContent panelContent pdl hide-toolbar

         .. container::

            .. container::
            syntaxhighlighter collapsed sh-confluence nogutter bash
               :name: highlighter_589831

               .. container:: toolbar

                  `expand source <#>`__\ `? <#>`__

               +-----------------------------------------------------------------------+
               | .. container::                                                        |
               |                                                                       |
               |    .. container:: line number1 index0 alt2                            |
               |                                                                       |
               |       ``#!/bin/bash``                                                 |
               |                                                                       |
               |    .. container:: line number2 index1 alt1                            |
               |                                                                       |
               |       ``#SBATCH --mem=64g``                                           |
               |                                                                       |
               |    .. container:: line number3 index2 alt2                            |
               |                                                                       |
               |       ``#SBATCH --nodes=1``                                           |
               |                                                                       |
               |    .. container:: line number4 index3 alt1                            |
               |                                                                       |
               |       ``#SBATCH --ntasks-per-node=1``                                 |
               |                                                                       |
               |    .. container:: line number5 index4 alt2                            |
               |                                                                       |
               |                                                                       |
               |      ``#SBATCH --cpus-per-task=64     # <- match to OMP_NUM_THREADS`` |
               |                                                                       |
               |    .. container:: line number6 index5 alt1                            |
               |                                                                       |
               |       ``#SBATCH --parti                                               |
               | tion=gpuA100x4 # <- one of: gpuA100x4 gpuA40x4 gpuA100x8 gpuMI100x8`` |
               |                                                                       |
               |    .. container:: line number7 index6 alt2                            |
               |                                                                       |
               |       ``#SBATCH --account=bbka-delta-gpu``                            |
               |                                                                       |
               |    .. container:: line number8 index7 alt1                            |
               |                                                                       |
               |       ``#SBATCH --job-name=tfNGC``                                    |
               |                                                                       |
               |    .. container:: line number9 index8 alt2                            |
               |                                                                       |
               |       ``### GPU options ###``                                         |
               |                                                                       |
               |    .. container:: line number10 index9 alt1                           |
               |                                                                       |
               |       ``#SBATCH --gpus-per-node=1``                                   |
               |                                                                       |
               |    .. container:: line number11 index10 alt2                          |
               |                                                                       |
               |       ``#SBATCH --gpus-per-task=1``                                   |
               |                                                                       |
               |    .. container:: line number12 index11 alt1                          |
               |                                                                       |
               |       ``#SBATCH --gpu-bind=verbose,per_task:1``                       |
               |                                                                       |
               |    .. container:: line number13 index12 alt2                          |
               |                                                                       |
               |       `` ``                                                           |
               |                                                                       |
               |    .. container:: line number14 index13 alt1                          |
               |                                                                       |
               |       ``m                                                             |
               | odule reset``\ ``# drop modules and explicitly load the ones needed`` |
               |                                                                       |
               |    .. container:: line number15 index14 alt2                          |
               |                                                                       |
               |                                                                       |
               |      ``             ``\ ``# (good job metadata and reproducibility)`` |
               |                                                                       |
               |    .. container:: line number16 index15 alt1                          |
               |                                                                       |
               |       ``             ``\ ``# $WORK and $SCRATCH are now set``         |
               |                                                                       |
               |    .. container:: line number17 index16 alt2                          |
               |                                                                       |
               |       ``module list ``\ ``# job documentation and metadata``          |
               |                                                                       |
               |    .. container:: line number18 index17 alt1                          |
               |                                                                       |
               |                                                                       |
               |                                                                       |
               |    .. container:: line number19 index18 alt2                          |
               |                                                                       |
               |       ``echo`` :literal:`"job is starting on `hostname`"`             |
               |                                                                       |
               |    .. container:: line number20 index19 alt1                          |
               |                                                                       |
               |                                                                       |
               |                                                                       |
               |    .. container:: line number21 index20 alt2                          |
               |                                                                       |
               |                                                                       |
               |   ``# run the container binary with arguments: python3 <program.py>`` |
               |                                                                       |
               |    .. container:: line number22 index21 alt1                          |
               |                                                                       |
               |       ``singularity run --nv \``                                      |
               |                                                                       |
               |    .. container:: line number23 index22 alt2                          |
               |                                                                       |
               |                                                                       |
               |  `` ``\ ``/sw/external/NGC/tensorflow``\ ``:22.02-tf2-py3 python3 \`` |
               |                                                                       |
               |    .. container:: line number24 index23 alt1                          |
               |                                                                       |
               |       `` ``\ ``tf_matmul.py``                                         |
               +-----------------------------------------------------------------------+

   .. rubric:: Container list (as of March, 2022)
      :name: DeltaXSEDEDocumentation-Containerlist(asofMarch,2022)

   .. container:: code panel pdl conf-macro output-block

      .. container:: codeHeader panelHeader pdl

         **catalog.txt**

      .. container:: codeContent panelContent pdl

         .. container::

            .. container:: syntaxhighlighter sh-confluence nogutter java
               :name: highlighter_134830

               .. container:: toolbar

                  `? <#>`__

               +-----------------------------------------------------------------------+
               | .. container::                                                        |
               |                                                                       |
               |    .. container:: line number1 index0 alt2                            |
               |                                                                       |
               |       ``caffe:``\ ``20.03``\ ``-py3``                                 |
               |                                                                       |
               |    .. container:: line number2 index1 alt1                            |
               |                                                                       |
               |       ``caffe2:``\ ``18.08``\ ``-py3``                                |
               |                                                                       |
               |    .. container:: line number3 index2 alt2                            |
               |                                                                       |
               |       ``cntk:``\ ``18.08``\ ``-py3 , Microsoft Cognitive Toolkit``    |
               |                                                                       |
               |    .. container:: line number4 index3 alt1                            |
               |                                                                       |
               |       ``digits:``\ ``21.09``\ ``-tensorflow-py3``                     |
               |                                                                       |
               |    .. container:: line number5 index4 alt2                            |
               |                                                                       |
               |       ``lammps:patch_4May2022``                                       |
               |                                                                       |
               |    .. container:: line number6 index5 alt1                            |
               |                                                                       |
               |       ``matlab:r2021b``                                               |
               |                                                                       |
               |    .. container:: line number7 index6 alt2                            |
               |                                                                       |
               |       ``mxnet:``\ ``21.09``\ ``-py3``                                 |
               |                                                                       |
               |    .. container:: line number8 index7 alt1                            |
               |                                                                       |
               |       ``namd:``\ ``2.13``\ ``-multinode``                             |
               |                                                                       |
               |    .. container:: line number9 index8 alt2                            |
               |                                                                       |
               |       ``pytorch:``\ ``22.02``\ ``-py3``                               |
               |                                                                       |
               |    .. container:: line number10 index9 alt1                           |
               |                                                                       |
               |       ``tensorflow:``\ ``22.02``\ ``-tf1-py3``                        |
               |                                                                       |
               |    .. container:: line number11 index10 alt2                          |
               |                                                                       |
               |       ``tensorflow:``\ ``22.02``\ ``-tf2-py3``                        |
               |                                                                       |
               |    .. container:: line number12 index11 alt1                          |
               |                                                                       |
               |       ``tensorrt:``\ ``22.02``\ ``-py3``                              |
               |                                                                       |
               |    .. container:: line number13 index12 alt2                          |
               |                                                                       |
               |       ``theano:``\ ``18.08``                                          |
               |                                                                       |
               |    .. container:: line number14 index13 alt1                          |
               |                                                                       |
               |       ``torch:``\ ``18.08``\ ``-py2``                                 |
               +-----------------------------------------------------------------------+

   see also: https://catalog.ngc.nvidia.com/orgs/nvidia/containers

   .. rubric:: Other Containers
      :name: DeltaXSEDEDocumentation-OtherContainers

   .. rubric:: Extreme-scale Scientific Software Stack (E4S)
      :name: DeltaXSEDEDocumentation-Extreme-scaleScientificSoftwareStack(E4S)

   The E4S container with GPU (cuda and rocm) support is provided for
   users of specific ECP packages made available by the E4S project
   (https://e4s-project.github.io/). The singularity image is available
   as :

   ::

      /sw/external/E4S/e4s-gpu-x86_64.sif

   ::

      To use E4S with NVIDIA GPUs

   .. container:: code panel pdl conf-macro output-block

      .. container:: codeContent panelContent pdl

         .. container::

            .. container:: syntaxhighlighter sh-confluence nogutter java
               :name: highlighter_120219

               .. container:: toolbar

                  `? <#>`__

               +-----------------------------------------------------------------------+
               | .. container::                                                        |
               |                                                                       |
               |    .. container:: line number1 index0 alt2                            |
               |                                                                       |
               |                                                                       |
               |   ``$ srun --account=account_name --partition=gpuA100-interactive \`` |
               |                                                                       |
               |    .. container:: line number2 index1 alt1                            |
               |                                                                       |
               |       ``  ``\ ``--nodes=``\ ``1`` ``--gpus-per-node=``\ ``1``         |
               |       ``--tasks=``\ ``1`` ``--tasks-per-node=``\ ``1`` ``\``          |
               |                                                                       |
               |    .. container:: line number3 index2 alt2                            |
               |                                                                       |
               |       ``  ``\ ``--cpus-per-task=``\ ``1`` ``--mem=20g \``             |
               |                                                                       |
               |    .. container:: line number4 index3 alt1                            |
               |                                                                       |
               |       ``  ``\ ``--pty bash``                                          |
               |                                                                       |
               |    .. container:: line number5 index4 alt2                            |
               |                                                                       |
               |       ``                                                              |
               | $ singularity exec --cleanenv /sw/external/E4S/e4s-gpu-x86_64.sif \`` |
               |                                                                       |
               |    .. container:: line number6 index5 alt1                            |
               |                                                                       |
               |       ``  ``\ ``/bin/bash --rcfile /etc/bash.bashrc``                 |
               +-----------------------------------------------------------------------+

   The spack package inside of the image will interact with a local
   spack installation. If  ~/.spack directory exists, it might need to
   be renamed. 

   More information can be found at
   https://e4s-project.github.io/download.html

   .. rubric:: Protected Data (N/A)
      :name: DeltaXSEDEDocumentation-ProtectedData(N/A)

   ...

   .. rubric:: **Help**
      :name: DeltaXSEDEDocumentation-Help

   For assistance with the use of Delta

   -  XSEDE users can create a ticket via the user portal at
      https://portal.xsede.org/web/xup/help-desk
   -  All other users (Illinois allocations, Diversity Allocations, etc)
      please send email to help@ncsa.illinois.edu.

   .. rubric:: **Acknowledge**
      :name: DeltaXSEDEDocumentation-Acknowledge

   To acknowledge the NCSA Delta system in particular, please include
   the following

   This research is part of the Delta research computing project, which
   is supported by the National Science Foundation (award OCI 2005572),
   and the State of Illinois. Delta is a joint effort of the University
   of Illinois at Urbana-Champaign and its National Center for
   Supercomputing Applications.

   To include acknowledgement of XSEDE contributions to a publication or
   presentation please see https://portal.xsede.org/acknowledge and
   https://www.xsede.org/for-users/acknowledgement.

   .. rubric:: **References**
      :name: DeltaXSEDEDocumentation-References

   Supporting documentation resources:

   https://www.rcac.purdue.edu/knowledge/anvil

   https://nero-docs.stanford.edu/jupyter-slurm.html

   | 

.. |image1| image:: /download/attachments/144016994/image2021-3-12_14-23-11.png?version=1&modificationDate=1615925873000&api=v2
   :class: confluence-embedded-image
   :height: 250px
.. |image2| image:: /rest/documentConversion/latest/conversion/thumbnail/179671355%20apply%20an%20ascending%20sort
