 # SdvBench: Microsoft SDV Benchmarks

[Download here](https://www.microsoft.com/en-us/download/details.aspx?id=55656) (Approx. 15000 files, 1.3GB compressed)

This repository contains a subset of the internal tests used by Microsoft's [Static Driver Verifier](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/static-driver-verifier) tool. These tests are generated from Window's Device Drivers while checking for one of the various [properties](https://msdn.microsoft.com/en-us/library/windows/hardware/ff551714) that WDM drivers must satisfy. 

SDV is a cornerstone in the [successful application of automated program verification in the industry](http://dl.acm.org/citation.cfm?id=1965743). It has given rise to verifiers like SLAM, Yogi and Corral, and indirectly influenced the design of many other verifiers. These programs represent one of the most comprehensive and challenging collection of industrial-strength verification benchmarks. 

Each program is a [Boogie](https://github.com/boogie-org/boogie) file and represents checking of one property on a single driver. The file names are loosely derived from the signature `DriverName_PropertyName_InstanceNumber.bpl`. A single property can give rise to many verification checks, hence the use of `InstanceNumber`. The property is already instrumented into the code as assertions. There is a well-defined entrypoint, which is the unique procedure tagged with `{:entrypoint}` annotation. The goal of the verifier is to find an execution of the program, starting at this entrypoint, that ends in an assertion failure. That's it! The entire process of how the file is generated is explained in our [FSE 2014](https://www.microsoft.com/en-us/research/publication/powering-the-static-driver-verifier-using-corral/) paper. 

The programs are readily analyzed by [Corral](https://www.microsoft.com/en-us/research/project/q-program-verifier/), a whole-program verifier for Boogie programs. The flags used to run Corral are mentioned alongside each file. Note that while running with a timeout of 2000 seconds, the entire suite takes over a month of CPU time to finish (when running sequentially) with Corral. 

These program carry some preculiarities specific of the use of Corral inside SDV. For instance, all loops have been converted to recursive procedures. Predicates that are used as Houdini candidates (for generating program invariants) are detailed alongside the procedure tagged with the annotation `{:template}`. 

We invite contributions from the community to help navigate these benchmarks and make them accessible to other verification tools. As an example, we have added `BoogieFileStats` utility. To use it, build `BoogieFileStats\BoogieFileStats.sln` using `VS 2015`. Then execute it as follows to obtain some simple statistics about a Boogie program.

```
sdvbench\BoogieFileStats>BoogieFileStats\bin\Debug\BoogieFileStats.exe SdvBench\ITP_WDM\fail_driver1_cancelroutine_0.bpl

fail_driver1_cancelroutine_0.bpl
  Procedures: 61
  Implementations: 54
  Global variables: 23
  Map-typed Global variables: 10
  Basic blocks: 368
  Commands: 710
  Call Commands: 147
```

## Contact

Please use github issues for questions and comments. We are looking forward to hearing from you!
