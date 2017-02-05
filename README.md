# pylight-fuse
Fuse filesystem to control wireless lighting systems
----
This is a python 2 module used to provide access to wireless lighting systems through the linux filesystem using FUSE (File system in User Space).
Each bulb/zone will be exposed as a file within the linux file system. 
Commands written to the files will be parsed and sent to the light.

Reads of the file will report: 
*The commands available
*The last state (current) state of the bulb (milight bulbs do not store state).

The system is being developed using the Milight wifi bridge, but should be extendable to any lighting system....
Initial commands will be simply transmitted to the bulb, later we can add composite commands

