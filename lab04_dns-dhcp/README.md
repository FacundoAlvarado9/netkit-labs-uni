# Lab 03 - Subnetting and Policy-based routing
The following is a diagramm representing the topology of the given network, defined in the *lab.config* file.

![Topology for lab03](/readme_img/lab04_topology.jpg)

## Subnetting

Making use of the Variable Length Subnet Mask (VLSM) strategy.

### Available addresses and masks

- 192.168.148.0/24
- 192.168.149.0/24
- 192.168.150.0/24
- 192.168.151.0/24
- 192.168.152.0/24

### Network sizes

- Network 1: 356 devices.
- Network 2: 230 devices.
- Network 3: 61 devices.
- Network 4: 110 devices.
- Network 5: 55 devices.
- Network 6: 50 devices.
- Network 7: 2 devices.
- Network 8: 2 devices.

### Asignment
Network by network from the ones with the most devices to the ones with the lowest.

|Network 1 (356 devices) |
| ---- |
| 192.168.148.0/23 |
| Network Address: 192.168.148.0 |
| Broadcast: 192.168.149.255 |
| Usable Host IP Range: 192.168.148.1 - 192.168.149.254 (510 hosts) |


| Network 2 (230 devices) |
| ---- |
| 192.168.150.0/24 |
| Network Address: 192.168.150.0 |
| Broadcast: 192.168.150.255 |
| Usable Host IP Range: 192.168.150.1 - 192.168.150.254 (254 hosts) |

| Network 4  (110 devices )|
| ---- |
| 192.168.151.0/25 |
| Network Address: 192.168.151.0 |
| Broadcast: 192.168.151.127 |
| Usable Host IP Range: 192.168.151.1 - 192.168.151.126 (126 hosts) |

| Network 3 (61 devices) |
| ---- |
| 192.168.151.128/26 |
| Network Address: 192.168.151.128 |
| Broadcast: 192.168.151.191 |
| Usable Host IP Range: 192.168.151.129 - 192.168.151.190 (62 hosts) |

| Network 5 (55 devices) |
| ---- |
| 192.168.151.192/26 |
| Network Address: 192.168.151.192 |
| Broadcast: 192.168.151.255 |
| Usable Host IP Range: 192.168.151.193 - 192.168.151.254 (62 hosts) |

| Network 6 (50 devices) |
| ---- |
| 192.168.152.0/26 |
| Network Address: 192.168.152.0 |
| Broadcast: 192.168.152.63 |
| Usable Host IP Range: 192.168.152.1 - 192.168.152.62 (62 hosts) |

| Network 7 (2 devices) |
| ---- |
| 192.168.152.64/30 |
| Network Address: 192.168.152.64 |
| Broadcast: 192.168.152.67 |
| Usable Host IP Range: 192.168.152.65 - 192.168.152.66 (2 hosts) |

| Network 8 (2 devices) |
| ---- |
| 192.168.152.68/30 |
| Network Address: 192.168.152.68 |
| Broadcast: 192.168.152.71 |
| Usable Host IP Range: 192.168.152.69 - 192.168.152.70 (2 hosts)

## Static Routing
The main policy for routing was: Traffic from each of the networks should be directed to the ISP router.

Following that, we defined the routing tables as follows:

![Static routing tables](/readme_img/lab04_routing.jpg)