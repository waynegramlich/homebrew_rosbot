# Homebrew ROSBot

The goal of Homebrew ROSBot is to provide a relatively low cost
robot platform that can ROS (Robot Operating System.)  Central
to the robot design of the robot is a bus architecture that is
expandable.

All of the printed circuit boards (PCB's) for this project are
Open Source Hardware.  In addition, they are through hole (as
opposed to surface mount) designs to simplify assembly.

## Bus Architecture

For this project, it is an article of faith that there will
eventually be a robot bus that people/organizations engineer
modules for. Until there is a clear marketplace winner, the
following robot bus architecture is being used:

    http://gramlich.net/projects/robus/specifications.html

Alas, these files have not really been pushed up into a git
repository yet.

The bus has had several names over the years and it is now just
called "bus".  The intention is that somebody who is better at
marketing will give it the final name ("RoBus", "MakerBus",
"HackerBus", "UbiquiBus", "BlastBus", "FunBus", etc.)

## Printed Circuit Boards

Each PCB is stored in a separate repository.  If you go into the
repository (or clone it), the basic directory structure is:

* `./README.md`  The PCB documentation.
* `./docs/*` The various electronic component .pdf specification sheets.
* `./rev_a/*` Revision A of the PCB.

     ...

* `./rev_N/*` # Revision N of the PCB.

For a given PCB revision directory, the following files tend to be found:

* *pcb_name*`.pdf`.  The PCB schematic in `.pdf` format.
* *pcb_name*`.svg`.  The PCB schematic in `.svg` format (1 `.svg` per page).
* *pcb_name*`.drl`.  The Excellon drill file.
* *pcb_name*`.g??`.  The various Gerber (i.e. RS274X) files.
* *pcb_name*`.pro`.  The KiCAD project file.
* *pcb_name*`.sch`.  The KiCAD schematic file.
* *pcb_name*`.lib`.  The KiCAD schematic symbol library.
* *pcb_name*`.cmp`.  The KiCAD reference name to footprint mapping.
* *pcb_name*`.pretty/*`.  The KiCAD footprint directory.
* `fp-lib_table`.  The KiCAD footprint library table.
* *pcb_name*`.kicad_pcb`.  The KiCAD PCB file.
* *pcb_name*`.net`.  The KiCAD network list.
* *pcb_iname`.ino`.   The optional Arduino compatible source file.
* `Makefile`.  An optional Makefile to build the .ino file.

There may be other random stuff in the directory as well.

### Busino

The "Busino" concept with mini-shields concept is not a keeper in its
current format.  Busino takes up too much board space with connectors,
alignment screws, etc..  Busino is going to morph into something called
the bus_arduino shield, which is a designed primarily to interface to
Arduino shields.

* [Busino](https://github.com/waynegramlich/busino).
  The Busino is an Arduino compatible PCB that can be connected
  to a bus.  It has attach points for up to 4 mini-shields.

  * [BeagleBone Black Mini-Shield](https://github.com/waynegramlich/mini_beaglebone_black).
    The BeagleBone Black mini-shield provides a Busino connection to
    a beaglebone black processor.

  * [Bridge Encoders Sonar Mini-Shield](https://github.com/waynegramlich/mini_bridge_encoders_sonar).
    The Bridge Encoders Sonar mini-shield provides a dual H-bride
    to drive two small motors, a couple of quadrature shaft encoder
    inputs for doing dead reckoning, and a couple of sonar inputs
    for inexpensive HC-SR04 sonars.

  * [Power Mini-Shield](https://github.com/waynegramlich/mini_power).
    The Power mini-shield provides the ability to attach two separate
    batteries to Busino.  There is polarity protection and fuse
    protection.  Finally, there is an On/Off switch.

  * [Raspberry Pi Mini-Shield](https://github.com/waynegramlich/mini_raspberry_pi)
    The Raspberry Pi mini-shield provides a Busino connection to
    a Raspberry Pi processor.


### Dual Slot Encoders

The dual slot encoders are basically a keeper.  They are based on the
Sharp GP1S094HCZ0F, which is a 3mm gap slot interrupter.

* [Dual Slot Encoders](https://github.com/waynegramlich/dual_slot_encoders).
    The dual slot encoders PCB provides a quadrature encoder using
    a couple of optical interrupter slot sensors.  An encoder disk
    is needed to complete the system.

### Bus based PCBS:

* [BeagleBone Black Bus Module](https://github.com/waynegramlich/bus_beaglebone).
  This module simple provides an ATmega324P as a bridging processor
  between a Beaglebone Black and the bus.

* [Bridge Encoders Sonar Bus Module](https://github.com/waynegramlich/bus_bridge_encoders_sonar).
  This module is optimized to privide just provide a 1 Ampere dual
  H-bridge, 2 quadrature encoders, and 2 sonars that connect to the
  bus.

* [Power Bus Module](https://github.com/waynegramlich/bus_power).
  This module strictly provides power to the bus with polarity
  protection, over current protection and an on-off switch.

* [Raspberry Pi Bus Module](https://github.com/waynegramlich/bus_raspberry_pi).
  This module simple provides an ATmega324P as a bridging processor
  between a Raspberry Pi Modle B+ and the bus.  (This one is not getting
  much work right now due to the difficulty of getting ROS up and running
  on a Raspberry Pi.)

* [Sonar10 Bus Module](https://github.com/waynegramlich/bus_sonar10).
  This module provides a bus interface to drive up to 10 HC-SR04
  sonar modules.

* [Grove12 Bus Module](https://github.com/waynegramlich/bus_grove10).
  This module provides 12 Grove system connectors (4 dedicated to
  I<Sup>2</Sup>C communication.)  The
  [Grove System](http://www.seeedstudio.com/wiki/GROVE_System) was
  developed by [SeeedStudio](http://www.seeedstudio.com/) and
  has 100+ modules that can be plugged into the connectors.

There are a number of PCB's that are still in the gestation phase:

* ServoN Bus Module.  This is a board optimized to drive N hobby servos.
  N is the largest number of servos that still fit on one board.

* ClickN Bus Module.  This is a board for interfacing to the
  MikroElectronica MikroBus Click modules.  Again, N is selected
  to be a value that fits.

* Dyna Bus Module.  This is a board optimized for talking to the
  Dynamixel robot servo line (e.g. ath AX-12A) by Robotis.




