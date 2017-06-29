# Savana 0.5 Release (June 2017)

Overview:
------------
At its core, the Savanna runtime is a combination of two distinct in situ/online workflow management technologies:  Swift/T and ADIOS.  By combining them in a new larger infrastructure, Savanna offers a more comprehensive environment for constructing online data analysis and reduction frameworks that captures both Swift/T’s asynchronous workflow construction capabilities and ADIOS’s staged communication abilities for long-running online analytics.

There are three high-level goals for the Savanna in situ runtime.
* To provide a tested deployment framework for any ECP application
  (or software technology) project to utilize for online data analysis
  and reduction.
* To provide the infrastructure needed to create a testing
  framework (Cheetah) to evaluate reduction and analysis functions for
  performance on a variety of levels (application and platform)
* To provide a reference approach for ECP applications/teams that
  have specialized needs that exceed the infrastructure design
  constraints.


The name Savanna represents all three of these points -- The savanna
is an ecosystem that supports some of the largest animals on the
earth, as well as some of the fastest (like Cheetahs).  We want the
Savanna runtime to play a similar role for the exascale computing
project.  Savanna is not intended to be the only possible way of
deploying CODAR-developed or vetted analytics and reduction functions;
multiple cooperating ecosystems are needed to make the total system
thrive.  However, it should be a convenient and straight-forward
approach, making it easier for applications to focus on the science,
rather than the details of advanced scheduler settings, rdma network
transfers, and other technical minutia that tend to interfere with the
deployment of online techniques.

Installation
------------

The simplest form of installation is through spack.  ([https://github.com/LLNL/spack]())  Savanna is a package that depends on a specific set of configured installations of Swift/T and ADIOS, which are also available within spack.  Spack will automatically download and build all of the dependencies, so installation is as simple as the following:
`Spack install savanna`

A fork of the official spack repository is maintained at [https://github.com/CODARcode/spack]().  During the release cycle, the most recent versions of spack packages for ADIOS, Swift/T, Savanna, and Cheetah (a related CODAR product) can be found there, until those packages have been accepted by the spack maintainers.

What’s in Savanna?
------------

The Savanna installation includes the dependencies for correct
configuration and build of Swift/T and ADIOS, as well as some example
code to demonstrate the capability.  For the 0.5 beta release, we
introduce a self-contained mini-app, as well as a 3rd party process
(already built in the ADIOS release) that will allow you to transfer
data and compress or reduce the data in a variety of ways.  These are
intended to be simple enough examples that users can use them for
self-guided exploration of the interfaces.  More details on the heat
transfer mini-app are included below.

Heat Transfer Example
------------

The heat transfer example has been prepared to demonstrate CODAR Savana capability in which users can compose and execute multiple applications in an orchestrated environment. 
The code consists of two components; "simulator" and "stager". "Simulator" performs numeric calculations of heat transfers and outputs data during the calculation to "stager". "Stager" takes outputs from "simulator" and performs extra steps, such as saving data as files, compressions, transforms, etc. Executions are orchestrated and managed by CoDAR's key infrastructure software, Swift/T and Adios.

The heat transfer example is publicly available in CoDAR's git repository: https://github.com/CODARcode/Example-Heat_Transfer
Build
Heat transfer example depends on multiple key software, Swift, EVPath, and Adios. The details of building instructions are included in "README.adoc" in the release. 
Run
Swift provides run-time environment. It launches "simulator" and "stager" as a workflow defined in "workflow.swift" file. For the convenience, users can execute the example by a s script (run-workflow.sh)

By modifying workflow.swift, users can change the number of processes, arguments, and compressions options which will be described in the next. A few key options are as follows:
@par=N (program1, argument1): use N processes to launch program1 with argument1
Line 7: arguments1 = split("heat  4 3  40 50  6 500", " ");
Parameters for heat transfer execution. Details are as follows:
`$ ./heat_transfer_adios2`
 `Usage: heat_transfer  output  N  M   nx  ny   steps iterations
 output: name of output file`
 `N:      number of processes in X dimension`
 `M:      number of processes in Y dimension`
 `nx:     local array size in X dimension per processor`
 `ny:     local array size in Y dimension per processor`
 `steps:  the total number of steps to output`
 `iterations: one step consist of this many iterations`
` ensenble_float: A parameter we can vary to vary the results`
`Line 17: arguments2 = split("heat.bp staged.bp FLEXPATH \"\" MPI\"\"", " ");`

Parameters for stager.

`$ stage_write/stage_write `
`Usage: stage_write/stage_write input output rmethod "params" wmethod "params" <decomposition>`
 `   input   Input stream path`
 `   output  Output file path`
 `   rmethod ADIOS method to read with`
 `           Supported read methods: BP, DATASPACES, DIMES, FLEXPATH`
`    params  Read method parameters (in quotes; comma-separated list)`
 `   wmethod ADIOS method to write with`
 `   params  Write method parameters (in quotes; comma-separated list)
    names   List of variables to apply transforms(compression) (in quotes; comma-separated list)`
 `   params  Transform parameters (in quotes)`
  `  <decomposition>    list of numbers e.g. 32 8 4`
   `         Decomposition values in each dimension of an array`
  `          The product of these number must be less then the number`
`            of processes. Processes whose rank is higher than the`
`            product, will not write anything.`
 `              Arrays with less dimensions than the number of values,`
`            will be decomposed with using the appropriate number of`
`            values.`



Compression
------------

To enable compression through Adios, we need to provide additional options for "stager". The options are the list of variables to compress and the Adios' compression definition. For an example, the following options can used in "workflow.swift"
arguments2 = split("heat.bp staged.bp FLEXPATH \"\" MPI \"\" \"T,dt\" \"ZFP:accuracy=0.001\"", " ");
Then, stager will compress "T" and "dT" variables with ZFP method maintaining errors lower than 0.001 (This is ZFP's specific parameters. More details can be found in Adios's manual). 

