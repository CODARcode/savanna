# Savana 1.0 Release (June 2018)

The Savanna runtime provides the infrastructure to orchestrate and run complex science workflows consisting of multiple components (simulation, analysis codes, coupling frameworks, profiling libraries etc.).

Savanna is designed to work with the Cheetah Experiment Harness that is used to run parameter sweep experiments for exascale applications.
For the current release 1.0 of Savanna, Savanna is integrated into the source code of [Cheetah](https://github.com/CODARcode/cheetah).
Users are not required to launch Savanna explicitly. The workflow engine in Cheetah, along with the specification format for defining parameter sweeps, implement the full functionality of Savanna.



## Installation

Instructions for installing Savanna on different supercomputers such as ORNL's Titan, ALCF's Theta, and NERSC's Cori can be found [here](https://github.com/CODARcode/Software_Stack_QA).
Installing Savanna installs the full stack of CODAR applications and libraries:
- ADIOS
- Flexpath
- Dataspaces
- Compression libraries:
  - bzip2
  - zlib
  - zfp
  - sz
  - lz4
  - blosc
  - mgard

A benchmark running CODAR technologies has been provided at [Heat-Transfer](https://github.com/CODARcode/Example-Heat_Transfer/tree/codar)(branch 'codar'). It is a simple benchmark that implements Newton's law of cooling.
Accompanying Cheetah configuration to run parameter sweeps on the benchmark can be found in the examples section of the [Cheetah repository](https://github.com/CODARcode/cheetah/tree/master/examples).

