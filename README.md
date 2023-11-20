## OVERVIEW

The `show fabric` command shows you statistics and the status of the uplinks and BGP peerings. 
It is programmed to be used for leaf routers in IP fabrics that are based on eBGP (between Spine/Leaf as IGP) and iBGP EVPN. 

```
A:leaf1# show fabric summary
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Summary: Fabric Connectivity Report
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
+--------------+-------------+-------------------+---------+------------+---------------------+
| Chassis Type |     S/N     |      Uptime       | CPU (%) | Memory (%) | Disk Partitions (%) |
+==============+=============+===================+=========+============+=====================+
| 7220 IXR-D3  | NS2113T0411 | 73 days, 23:37:42 | 36      | 41         | 0/21/1/49/8/6/7     |
+--------------+-------------+-------------------+---------+------------+---------------------+
+-----------------+--------------+-------------+-------------+---------------+------------------+
| Local Interface | Local Router | Link Status | eBGP Status | Remote Router | Remote Interface |
+=================+==============+=============+=============+===============+==================+
| ethernet-1/33   | leaf1        |     up      | established | spine1        | ethernet-1/19    |
| ethernet-1/34   | leaf1        |     up      | established | spine2        | ethernet-1/19    |
+-----------------+--------------+-------------+-------------+---------------+------------------+
+-------------------------+-------------+----------------------+--------------+-------------------+
| Route Reflector Address | iBGP Status | Neighbor Description | Rx/Active/Tx | Uptime (hh:mm:ss) |
+=========================+=============+======================+==============+===================+
| 100.101.100.224         | established | RR-Spine1            | 91/91/29     | 49 days, 1:39:53  |
| 100.101.100.225         | connect     | RR-Spine2            | 0/0/0        | 0:00:00           |
+-------------------------+-------------+----------------------+--------------+-------------------+
+-----------------+--------------------+---------------------+----------------+---------+---------+------------------+
| Local Interface | Traffic Bps In/Out |   Packets In/Out    | Errored In/Out | FCS Err | CRC Err | Transceiver Volt |
+=================+====================+=====================+================+=========+=========+==================+
| ethernet-1/33   | 2303/2303          | 22559953/6716965612 | 0/0            | 0       | 0       | 3.2777           |
| ethernet-1/34   | 2711/2711          | 23189180/20854400   | 0/0            | 0       | 0       | 3.2419           |
+-----------------+--------------------+---------------------+----------------+---------+---------+------------------+
--{ + running }--[  ]--

```

It requires some inputs to discover your uplinks and BGP peerings. 

## HOW TO GET IT INTO YOUR SR LINUX

Get this CLI Plugin by simply installing the rpm from the bash of your SR Linux node.

1. Connect to your node and get to the bash:
```
A:leaf1# bash
```

3. Download or copy the rpm file into the linux system:

```
[admin@leaf1 ~]$ wget https://github.com/aaakpinar/show-fabric-pack/raw/main/show-fabric-0.1.1.rpm
```
>Look for the latest release...

3. Check your uplink descriptions and BGP group names and configure the `showfabric.conf` file:
```
[admin@leaf1 ~]$ sudo vi /etc/showfabric.conf
############################ INPUTs here... ################################
# set a description pattern that is common in the uplink descriptions
# alternatively, set it directly: interfaces=ethernet-1/33,ethernet-1/34
# if both given: interfaces list has precedence over the description pattern
[DEFAULT]
description = spine
# or
interfaces =

# e/iBGP group names
uplink_peer_group = eBGP-underlay
rr_peer_group = iBGP-EVPN

# network instance is typically default
uplink_network_instance = default
############################################################################
```

4.  Once you're done, open a new ssh or just connect to the SR CLI from the linux CLI:
```
[admin@leaf1 ~]$ sr_cli
A:leaf1# show fabric summary
```

Enjoy!

-Alperen
