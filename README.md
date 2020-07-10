# Dynamically Loading EPICS Module Builder Example

![Linter Run](https://github.com/jeonghanlee/DynSharedLibs/workflows/Linter%20Run/badge.svg)

To add an EPICS module into the prebuilt / running IOC is impossible without the whole new recompiling technically. And in many situations, it may be impossible to recompile an entire IOC from scratch due to several reasons. However, in specific situation, we would like to add an EPICS module into an exist IOC.

There is a minimal hackable method to do if the EPICS module has only EPICS base dependency. (We can do this in more complicated case, but I do not want to cover this situation.) This method is based on PSI/ESS dynamically loadable module [1,2], which works brilliant, but it uses the standard EPICS building system as much as it can.

This repository shows one of simplest example how to do this without extra makefile, but with few more extra makefile rules. And this repository is just an example, which gives an insight to help EPICS control system engineers to resolve this issue if system is urgently needed to be integrated with an EPICS module.

Michael Davidsaver now developing the brand-new approach to embrace PSI/ESS method in the latest EPICS base [3]. I hope we can have this feature soon.

Note that this approach may not work the exist / prebuilt IOC due to unknown reasons, which I cannot test them all.

## Requirements

* EPICS Base : one MUST download the exact version of EPICS base which was used to build the exist IOC

## Build Procedure

```bash
make init
make build
make install
make exist
```

## Run

```bash
softIoc cmd/st.cmd
epics> dbl
epics> dbpr TEST-IocStats:HEARTBEAT
epics> dbpr TEST-IocStats:HEARTBEAT
A   : 68            ASG :               B   : 0             C   : 0
CALC: (A<2147483647)?A+1:1              D   : 0
DESC: 1 Hz counter since startup        DISA: 0             DISP: 0
DISV: 1             DLYA: 0             E   : 0             F   : 0
G   : 0             H   : 0             I   : 0             J   : 0
K   : 0             L   : 0             NAME: TEST-IocStats:HEARTBEAT
OCAL: 0             OEVT:               OVAL: 69            POVL: 69
PVAL: 69            SEVR: NO_ALARM      STAT: NO_ALARM      TPRO: 0
VAL : 69
epics> dbpr TEST-IocStats:HEARTBEAT
A   : 69            ASG :               B   : 0             C   : 0
CALC: (A<2147483647)?A+1:1              D   : 0
DESC: 1 Hz counter since startup        DISA: 0             DISP: 0
DISV: 1             DLYA: 0             E   : 0             F   : 0
G   : 0             H   : 0             I   : 0             J   : 0
K   : 0             L   : 0             NAME: TEST-IocStats:HEARTBEAT
OCAL: 0             OEVT:               OVAL: 70            POVL: 70
PVAL: 70            SEVR: NO_ALARM      STAT: NO_ALARM      TPRO: 0
VAL : 70  
```

## References

[1] <https://github.com/paulscherrerinstitute/require>

[2] <https://github.com/icshwi/e3>

[3] <https://github.com/epics-base/epics-base/pull/76>
