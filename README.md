# pylight-fuse
Fuse filesystem to control wireless lighting systems
----
This is a python 2 module used to provide access to wireless lighting systems through the linux filesystem using FUSE (File system in User Space).
Each bulb/zone will be exposed as a file within the linux file system. 
Commands written to the files will be parsed and sent to the light.

Reads of the file will report: 
* The commands available
* The last state (current) state of the bulb (milight bulbs do not store state).

The system is being developed using the Milight wifi bridge, but should be extendable to any lighting system....
Initial commands will be simply transmitted to the bulb, later we can add composite commands

Static vs Dynamic configuration?
Parsed commands or control endpoints.

E.g.
~~~~
|
|
|--Light 1 -> ON/Off/Bright commands
|
|--Light 2 -|
            |
            - control
            |
            |- Bright
            |
            |- Colour
            |
            |- Mode
~~~~
Parsed commands:-
* Single point of contact per bulb.
* Interface becomes a message build/parse mechanism
* Enforces single command at a time (assuming file access is limited)
* Potential to build flows of commands with implicit ordering

Control Endpoints:
* Controls supported by a light are explicitly exported no need to handle errors (although parameters do need parsing)
* Still needs some form of parsing to at a minimum handle boolean params and variables

---
## Proposed Controls

Write | Usage 
-------------------
| State   | Controls the on off state of the light. |
| Brightness | Value 0 to 100 Controls the brightness of the bulb |
| Colour-RGB | Colour of the bulb as RGB |
| Colour-HUE | Colour of the bulb using a HUE value. |
| Colour-SAT | Saturation of the bulb (if supported) |
| Mode | Mode of the light (discomodes) |
| White | White Mode of the light |
| Temperatre | Colour temperature of the light |


Read | Usage
------------
State | Last written or current on/off sate
Brightness | Current brightness...