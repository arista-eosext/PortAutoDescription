PortAutoDescription
===================
Port auto-description tool automatically updates the port
description of an interface based on the lldp neighbor
information of the attached device.

## INSTALLATION
In order to install this extension:
- copy 'portAuto' to /mnt/flash
- enable the Command API interface:

By default, the script is set to use unix-sockets. Since the script
runs on the local switch, by using unix-sockets you do not need to keep
credentials in the script.

```
conf
management api http-commands
    protocol unix-socket
    no shutdown
```

(This will enable eAPI for HTTPS if you do not want to use unix-sockets)
```
management api http-commands
    no shutdown
```

change SWITCH_IP, USERNAME and PASSWORD at the top of the
script to the ones appropriate for your installation. If
running locallty, use '127.0.0.1' for the IP. If using unix-sockets
you do not need to worry about USERNAME and PASSWORD.

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

## CUSTOMIZATION
Version 3 of this script provides more information by leveraging the
``show lldp neighbors detail`` command. Therefore, you can adjust the
description string by using any of the keys in the following object:

```
{
  'age': 3,
  'neighborDevice': u'veos-dc1-pod1-spine2',
  'neighborEnabledCapabilities': [u'Bridge'],
  'neighborMac': u'0011.2233.4456',
  'neighborMaxFrameSize': u'9236',
  'neighborMgmtAddr': u'192.168.0.10',
  'neighborMgmtAddrType': u'IPv4',
  'neighborMgmtOid': '',
  'neighborPort': u'Ethernet3',
  'neighborPortDescription': u'My wonderful Et3',
  'neighborSystemCapabilities': [u'Bridge', u'Router'],
  'neighborSystemDescription': u'Arista Networks EOS version 4.14.2F running on an Arista Networks vEOS',
  'neighborVlanId': u'1',
  'port': u'Ethernet3',
  'ttl': 120
}
  ```

Armed with this object, you can now make the interface description anything
you like. EG:

```
intfDesc = '*** Link to %s[%s] | VLAN%s | MGMT %s' % (i['neighborDevice'],
                                                      i['neighborPort'],
                                                      i['neighborVlanId'],
                                                      i['neighborMgmtAddr'])
```


## COMPATIBILITY
Version 2.0 has been developed and tested against EOS-4.12.0 and
is using the Command API interface. Hence, it should maintain
backward compatibility with future EOS releases.

## LIMITATIONS
None known.

## LICENSE
BSD-3, See LICENSE file
