# How to build a Dynamically Loadable EPICS Module (Few Examples)

![Linter Run](https://github.com/jeonghanlee/DynSharedLibs/workflows/Linter%20Run/badge.svg)

To add an EPICS module into the prebuilt / running IOC is impossible without the whole new recompiling technically. And in many situations, it may be impossible to recompile an entire IOC from scratch due to several reasons. However, in specific situation, we would like to add an EPICS module into an exist IOC.

There is a minimal hackable method to do if the EPICS module has only EPICS base dependency. (We can do this in more complicated case, but I do not want to cover this situation.) This method is based on PSI/ESS dynamically loadable module [1,2], which works brilliant, but it uses the standard EPICS building system as much as it can.

This repository shows one of simplest example how to do this without extra makefile, but with few more extra makefile rules. And this repository is just an example, which gives an insight to help EPICS control system engineers to resolve this issue if system is urgently needed to be integrated with an EPICS module.

Michael Davidsaver now developing the brand-new approach to embrace PSI/ESS method in the latest EPICS base [3]. I hope we can have this feature soon.

Note that this approach may not work the exist / prebuilt IOC due to unknown reasons, which I cannot test them all.

## Requirements

* EPICS Base : one MUST download the exact version of EPICS base which was used to build the exist IOC

## Examples

Each unique branch contains each module example. Current the following modules can be tested, and are planned.

* iocStats : <https://github.com/jeonghanlee/DynSharedLibs/releases/tag/iocStats_v01>
* recsync (aka RecCaster) : <https://github.com/jeonghanlee/DynSharedLibs/releases/tag/recsync_v01>
* retools : <https://github.com/jeonghanlee/DynSharedLibs/releases/tag/retools_v01>
* autosave (TODO)
* caPutLog (TODO)

## Build Procedure

```bash
make init
make build
make install
make exist
```

## Run

```bash
jhlee@parity: DynSharedLibs (rescync)$ softIoc cmd/st.cmd
dbLoadDatabase("/home/jhlee/epics_env/epics-base/bin/linux-x86_64/../../dbd/softIoc.dbd")
softIoc_registerRecordDeviceDriver(pdbbase)
# Begin cmd/st.cmd
dlload "/home/jhlee/gitsrc/DynSharedLibs/cmd/librecsync.so"
dbLoadDatabase "/home/jhlee/gitsrc/DynSharedLibs/cmd/reccaster.dbd"
recsync_registerRecordDeviceDriver
epicsEnvSet("IOCNAME", "recsync")
var(reccastTimeout, 5.0)
var(reccastMaxHoldoff, 5.0)
dbLoadRecords("/home/jhlee/gitsrc/DynSharedLibs/cmd/reccaster.db", "P=recsync-RecSync:")
# End cmd/st.cmd
iocInit()
Starting iocInit
############################################################################
## EPICS R7.0.3.2-DEV
## Rev. R7.0.3.1-94-g02a24a144d0c06231121
############################################################################
iocRun: All initialization complete
iocRun: All initialization complete
epics> dbl
recsync-RecSync:Msg-I
recsync-RecSync:State-Sts
epics> dbpr recsync-RecSync:State-Sts
ASG :               DESC:               DISA: 0             DISP: 0
DISV: 1             NAME: recsync-RecSync:State-Sts         RVAL: 0
SEVR: NO_ALARM      STAT: NO_ALARM      SVAL: 0             TPRO: 0
VAL : 1
epics> dbpr recsync-RecSync:Msg-I
ASG :               DESC:               DISA: 0             DISP: 0
DISV: 1             NAME: recsync-RecSync:Msg-I             SEVR: NO_ALARM
STAT: NO_ALARM      SVAL:               TPRO: 0             VAL : Searching  
```

## Cleaning

* Clean all files that are created.

```bash
make clean
```

* Clean all files that are created and remove the download source

```bash
make distclean
```

## References

[1] <https://github.com/paulscherrerinstitute/require>

[2] <https://github.com/icshwi/e3>

[3] <https://github.com/epics-base/epics-base/pull/76>
