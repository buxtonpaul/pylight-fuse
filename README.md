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
------ | -----------
| State   | Controls the on off state of the light. |
| Brightness | Value 0 to 100 Controls the brightness of the bulb |
| Colour-RGB | Colour of the bulb as RGB |
| Colour-HUE | Colour of the bulb using a HUE value. |
| Colour-SAT | Saturation of the bulb (if supported) |
| Mode | Mode of the light (discomodes) |
| White | White Mode of the light |
| Temperature | Colour temperature of the light |


Read | Usage
-----|------
State | Last written or current on/off sate
Brightness | Current brightness...

###
Configuration

For the MiLight case we have bridges and each bridge may have n zones where zone 0 is a special zone which 
Refers to all zones on that bridge for that light type.

Configuration can be done as

List of bridges

Bridge = Bridge Config (IP address etc) + list of bulb nodes

Bulb node = Zone name, Zone number, Zone types (for milight = RGB, RGBWW, White, Bridge)

----
# Thoughts
Bridge should allow multiple names for the same zone.
Zone status should be pushed back to all nodes that connect to that zone.
Should be able to produce Graphviz representation of the attached lights.



~~~~
                            |----- Zone 1, RGBW+-----> Hall Light
     Bridge_milight001------|                  |-----> Landing Light
                            |
                            |
                            |--------->Zone 2, RGBW ----> Bedroom Light
                            |
                            |---------->Zone 1, White----->Spare Room




Is actually more like



        Bridge -----------> RGBW 
                        |
                        |
                        |--> White ------> Zone 0--------------------|
                                    |                                |
                                    |                                |
                                    |----------------> Zone 1<-------|
                                    |                                |
                                    |                                |
                                    |----------------> Zone 2<-------+

~~~~


![Alt text](https://g.gravizo.com/g?
  digraph G {
    aize ="4,4";
    bridge1 -> rgbw;
    rgb1 -> zoner1;
    rgb1 -> zoner2;
    bridge1 -> white;
    white -> zonew1;
    zonew1 -> spareroom;
    zoner1 -> bedroom;
    zoner2 -> hallway;
    zoner2 -> landing;
  }
)