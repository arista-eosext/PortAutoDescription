PortAutoDescription
===================
Port auto-description tool automatically updates the port
description of an interface based on the lldp neighbor
information of the attached device.

## INSTALLATION
In order to install this extension:
- copy 'portAuto' to /mnt/flash
- enable the Command API interface:

```
management api http-commands
    no shutdown
```

change SWITCH_IP, USERNAME and PASSWORD at the top of the
script to the ones appropriate for your installation. If
running locallty, use '127.0.0.1' for the IP.

portAuto can then be started using any of the following methods:
    
1 - Execute directly from bash (from the switch, or a remote
    switch/server running Python):

```
(bash)# /mnt/flash/portAuto
```
   
2 - Configure an alias on the switch:

```
(config)# alias portAuto bash /mnt/flash/portAuto
```
   
3 - Schedule a job on the switch:
    e.g.: In order to run portAuto every 12 hours, use:
```

(config)# schedule portAuto interval 720 max-log-files 0
         command bash sudo /mnt/flash/portAuto
```

4 - Run at switch boot time by adding the following startup
    config:
```
(config)# event-handler portAutoDescription
(config)# trigger on-boot
(config)# action bash /mnt/flash/portAuto
(config)# asynchronous
(config)# exit
```

Note that in order for this to work, you will also have to
enable the Command API interface in the startup-config (see
above).

## COMPATIBILITY
Version 2.0 has been developed and tested against EOS-4.12.0 and
is using the Command API interface. Hence, it should maintain
backward compatibility with future EOS releases.

## LIMITATIONS
None known.

## LICENSE
BSD-3, See LICENSE file
