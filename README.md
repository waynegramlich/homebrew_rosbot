# homebrew_rosbot

## Installing ROS Hydro Onto a Micro-SD Card

1. Create a ros-bbb directory:

        cd ...
        mkdir ros-bbb
        cd ros-bbb

2. Download the following three files from ros-training.com:

        wget http://www.ros-training.com/static/media/ros-bbb/ubuntu_ros_partitions.sfdisk
        wget http://www.ros-training.com/static/media/ros-bbb/ubuntu_ros_boot.partimg.gz.000
        wget http://www.ros-training.com/static/media/ros-bbb/ubuntu_ros_rootfs.partimg.gz.000

3. Verify that you got files that are the right size:

        ls -la
        -rw-rw-r-- 1 wayne wayne   6916451 Aug 22 08:01 ubuntu_ros_boot.partimg.gz.000
        -rw-rw-r-- 1 wayne wayne       259 Aug 22 08:01 ubuntu_ros_partitions.sfdisk
        -rw-rw-r-- 1 wayne wayne 345502689 Aug 22 08:09 ubuntu_ros_rootfs.partimg.gz.000

4. *Before* put your micro-SD card in, see what /dev/sd* says:

        ls -l /dev/sd*
        brw-rw---- 1 root disk 8,  0 Aug 22 10:33 /dev/sda
        brw-rw---- 1 root disk 8,  1 Aug 22 10:33 /dev/sda1
        brw-rw---- 1 root disk 8,  2 Aug 22 10:33 /dev/sda2
        brw-rw---- 1 root disk 8,  3 Aug 22 10:33 /dev/sda3
        brw-rw---- 1 root disk 8,  4 Aug 22 10:33 /dev/sda4
        brw-rw---- 1 root disk 8,  5 Aug 22 10:33 /dev/sda5
        brw-rw---- 1 root disk 8,  6 Aug 22 10:33 /dev/sda6
        brw-rw---- 1 root disk 8,  7 Aug 22 10:33 /dev/sda7

5. Now insert your 8GB (or larger) micro-SD card into the slot and
   do another ls command:

        ls -l /dev/sd*
        brw-rw---- 1 root disk 8,  0 Aug 22 10:33 /dev/sda
        brw-rw---- 1 root disk 8,  1 Aug 22 10:33 /dev/sda1
        brw-rw---- 1 root disk 8,  2 Aug 22 10:33 /dev/sda2
        brw-rw---- 1 root disk 8,  3 Aug 22 10:33 /dev/sda3
        brw-rw---- 1 root disk 8,  4 Aug 22 10:33 /dev/sda4
        brw-rw---- 1 root disk 8,  5 Aug 22 10:33 /dev/sda5
        brw-rw---- 1 root disk 8,  6 Aug 22 10:33 /dev/sda6
        brw-rw---- 1 root disk 8,  7 Aug 22 10:33 /dev/sda7
        brw-rw---- 1 root disk 8, 16 Aug 22 18:50 /dev/sdb
        brw-rw---- 1 root disk 8, 17 Aug 22 18:50 /dev/sdb1

   Notice that /dev/sdb and /dev/sdb1 showed up.  If something
   else shows up, you are going to need to manually edit
   ubuntu_ros_partitions.sfdisk file.  In addition, everywhere in
   the instructions below, you are going to have to change
   "/dev/sdb..." into /dev/sdX..." where "X" is the correct
   micro-SD drive letter.  If this does not make any sense,
   *get some help*.

6. I do the next commands as root:

        sudo bash
        Password: {your root password here}

7. Make sure that the partimage command is installed:

        apt-get install partimage
        {Respond positively to all prompts}

8. Make darn sure that /dev/sdb* is not installed:

        umount /dev/sdb*
        {It should say everything is unmounted}

9. Install a partition table on /dev/sdb:

        sfdisk --force /dev/sdb < ubuntu_ros_partitions.sfdisk
        partprobe /dev/sdb
        partprobe -s /dev/sdb

   The partprobe -s command may mention "msdos", just ignore that.

10. Now install boot image on micro-SD card:

        partimage restore /dev/sdb1 ubuntu_ros_boot.partimg.gz.000

    This will bring up one of the fake ASCII window system.
    You can use [Tab] to advance from one option to the next
    You can use [space] to toggle an option.
    You can type [enter] to trigger everything.
    For this screen I only toggle the option that writes 0's
    to empty blocks.  It is probably unnecessary, but I like
    to be sure.  Type [enter] at the continue option.  Keep
    typing [Tab]'s and [returns] until it actually write the
    partition bits.  The program will finish and then pause
    for a while as it finishes flushing the bits out to the
    micro-SD card.  Just be patient.

11. Now install rootfs partition onto the micro-SD card:

        partimage restore /dev/sdb2 ubuntu_ros_rootfs.partimg.gz.000
                
    Go through the same steps as in step 10.

12. Make darn sure that /dev/sdb* is unmounted:

        umount /dev/sdb*
        {It should say everything is no longer mounted}

14. Make sure that minicom is installed:

        apt-get install minicom

14. Exit from root:

        exit

15. Yank the micro-SD card out of your machine.

16. Grab your Beaglebone Black (henecforth called BBB.)  Flip it
    over and install the micro-SD card.

17. Find two open USB ports on your laptop/desktop.  Plug your
    FTDI 3.3V USB to serial cable into one of the USB ports.
    Make *darn* sure that you have a 3.3V FTDI cable.  If you
    are using one of Wayne's FTDI cables with tape wrapped
    arround the pin, peel the tape back and make *sure* it
    says "3V3" on the USB connector.  Plug the short USB to
    mini-USB cable that came with your BBB into the other USB
    port on laptop/desktop.  Do *not* plug the other end of
    this cable into the BBB yet.

18. On the top of the BBB, there is a 1x6 pin header labeled J1.
    One of the pins has a white dot next to the pin.  Plug in
    the 1x6 female receptical on the FTDI cable such that the
    black wire goes into the next the white dot.

19. Fire up the minicom terminal emulator:

        minicom

    Make sure the baud rate is "115200 8N1".  It should specify
    that it is using a serail cable like /dev/ttyUSB0.

20. Locate the little black button on the top surface of the BBB
    on that is near the micro-SD card (which is on the back surface).
    This button is very close to one of the mounting holes.  Also
    locate the micro-USB connector on the back side of the BBB
    near the Ethernet Jack (which is on the front side.)

21. Now we will carefully power up the BBB.  This is done by:

    A. Press down the black button and *hold* it down.
    B. While leaving the black button held down, plug in
       the power micro-SD.
    C. The blue LED's flash on the BBB.
    D. Boot up text spews out on the minicom window.

22. In fairly short order, Linux 13.04 will boot up and prompt
    you to login.  You can login as either user "ubuntu" with
    password "temppwd" or as user "ros" with password "training".

23. After you have logged in, you can poke around.  Verity that
    the directory "/opt/ros/hydro" exists.

24. Now you can shut down the BBB:

        sudo halt
        Password: {Use "temppwd" or "training")

    Some lights flash and some text spews.  Eventually the BBB
    stops.

25. Remove power cable and USB-to Serial cable.

## Printed Circuit Boards

The following PCB's are available:

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

* [Dual Slot Encoders](https://github.com/waynegramlich/dual_slot_encoders).
    The dual slot encoders PCB provides a quadrature encoder using
    a couple of optical interrupter slot sensors.  An encoder disk
    is needed to complete the system.

* Bus based PCBS:

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


