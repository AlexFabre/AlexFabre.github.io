# Project Organisation
07 October 2020

_A unified way to organize code directories and files of embedded projects alongside ST Cube IDE's files and folders_ 

## Structure
```
PCB_ID 
|
|
└───Inc
|   |   main.h
|   |   ...
|   |
|   └───com                     /* Communication headers */
|   |       com_process.h
|   |       com_command.h
|   |
|   └───generic_peripheral      /* Drivers headers with no specific modification for the project */
|   |       l647x.h
|   |       l6470.h
|   |       l6472.h
|   |       mcp23sxx.h
|   |       ...
|   |
|   └───motor                   /* Motors headers accessible to non-coders */
|   |       motor_generic_cmd.h
|   |       motor_FIX.h
|   |       motor_GRABBER.h
|   |       ...
|   |
|   └───peripheral              /* Physical parts functions */
|   |       pump.h
|   |       valve.h
|   |       ...
|   └───sequence
|           sequence.h
|           ...
|
└───Src
│   │   main.c
│   │   ...
│   |
|   └───com
|   |       com_process.c
|   |
|   └───generic_peripheral      /* Drivers generic functions */
|   |       l647x.c
|   |       mcp23s17.c
|   |
|   └───motor
|   |       motor_generic_cmd.c
|   |       motor.c
|   |
|   └───peripheral              /* Drivers and parts headers specific to the project */
|   |       pump.c
|   |       valve.c
|   |       ...
|   |
│   └───sequence                /* Integrators sequences */
│   |       _shadow_sequence.c
│   |       integrator_sequence.c
|   |       ...
|   |
|   └───task                    /* All the RTOS tasks */
|   |   └───motor
|   |   |       motor_init_task.c
|   |   |       FIX_task.c
|   |   |       GRABBER_task.c
|   |   |       ...
|   |   |
|   |   └───com
|   |   |       com_manager_task.c
|   |   |
|   |   └───sequence 
|   |   |       work_task.c
|   |   |
|   |   └───peripheral
|   |           pump_task.c
|   |           ...
```

## Rules
* __header__ files (*.h) go into __Inc/__ directory
* __source__ files (*.c) go into __Src/__ directory
* __One file per object and one task per file__

## Versioning

_Only repositories needed to generate the project must be versioned._

All output directories listed below must be left unversioned:

* __Debug__ Contains the compilation output files when debugging project (-DDEBUG).
* __Release__ Contains the same output files but when compiled in Release.
* __.settings__ Contains Eclipse local language settings.

_ _ _

Alex Fabre