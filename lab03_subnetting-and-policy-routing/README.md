# Lab 03 - Subnetting and Policy-based routing

Due to the complexity of the topology diagram, for now I will not generate a new translated version. Take into acount that:
- "Red" means Network in Spanish. That means that, for example, "Red 2" means "Network 2".
- "Centro de cómputos" = Computing center

The following is a diagramm representing the topology of the given network, defined in the *lab.config* file.

![Topology for lab03](/readme_img/lab03_topology.png)

## Subnetting

Making use of the Variable Length Subnet Mask (VLSM) strategy.

### Available addresses and masks

- 172.16.138.0/24
- 172.16.139.0/24
- 172.16.140.0/24
- 172.16.141.0/24
- 172.16.142.0/24
- 172.16.143.0/24

### Count of devices per network

- Network 1: 100 devices.
- Network 2: 96 devices.
- Network 3: 29 devices.
- Network 4: 50 devices.
- Network 5: 2 devices.
- Network 6: 2 devices.
- Network 7: 509 devices.
- Network 8: 360 devices.

### Our subnetting procedure

We started with the network with the most devices, i.e. network 7. Continuing in descending order according to device count, taking the smallest possible mask to cover the device count.

- Network 7: For all pre-defined addreses and masks (all /24), there is place for 256 IPs (254 devices, considering the network and broadcast addresses). Therefore, we decided to combine two /24 networks into one /23 network, therefore having "place" for 512 IPs (510 hosts), just enough for the 509 devices.

    ```
    Network 7
    Network address: 172.16.138.0/23
    Broadcast: 172.16.139.255
    Usable Host IP Range: 172.16.138.1 - 172.16.139.254
    ```

- Network 8: Same situation. 
  
    ```
    Network 8
    Network address: 172.16.140.0/23
    Broadcast: 172.16.141.255
    Usable Host IP Range: 172.16.140.1 - 172.16.141.254
    ```

- Network 1: Even though a mask of /24 gives us enough "place" for our devices, we decided to take it down a notch and use a mask of /25. That is, we get a count of 126 usable IPs for hosts.

    ```
    Network 1
    Network address: 172.16.142.0/25
    Broadcast: 172.16.142.127
    Usable Host IP Range: 172.16.142.1 - 172.16.142.126
    ```

- Network 2: Same situation.

    ```
    Network 2
    Network address: 172.16.142.128/25
    Broadcast: 172.16.142.127
    Usable Host IP Range: 172.16.142.1 - 172.16.142.126
    ```

- Network 4: Since we need only 50 addreses for hosts, we shrank the mask to a /26. That means 62 usable IPs for hosts.

    ```
    Network 4
    Network address: 172.16.143.0/26
    Broadcast: 172.16.142.63
    Usable Host IP Range: 172.16.143.1 - 172.16.142.62
    ```

- Network 3: 29 IPs for hosts needed, taking the smallest mask possible means taking a /27 mask, with 30 usable IPs for hosts.

    ```
    Network 3
    Network address: 172.16.143.64/27
    Broadcast: 172.16.143.95
    Usable Host IP Range: 172.16.143.65 - 172.16.143.94
    ```

- Network 5 and Network 6: Since they are point-to-point networks, the smallest possible mask is /30 with 2 usable IPs for hosts.

    ```
    Network 5
    Network address: 172.16.143.96/30
    Broadcast: 172.16.143.99
    Usable Host IP Range: 172.16.143.97 - 172.16.143.98
    ```

    ```
    Network 6
    Network address: 172.16.143.100/30
    Broadcast: 172.16.143.103
    Usable Host IP Range: 172.16.143.101 - 172.16.143.102
    ```

## Policy-based routing

### Policy to implement

1. "The default route for each of the networks should direct traffic to the ISP1 provider. The gateway address for that is 172.31.0.254"

2. "A routing policy must be defined in the router R3 so that traffic coming from the Network 8 is directed towards the provider ISP2. Other traffic must not be affected. The gateway address for this is 172.31.0.253"

3. "Aggregation must be used in order to minimize the count of entries in the routing tables."

### Implementation

Routing is statically defined.

All traffic from the networks to the computing center (*Centro de cómputos*) is directed towards the router R3.

For example, let's follow the routing tables from R4 to the *Centro de cómputos*. Starting at the R4 router the routing table looks like this:

| Destination | Gateway | Genmask | Flags | Metric | Ref | Use | Iface | //Comment |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0.0.0.0 | 172.16.143.101 | 0.0.0.0 | UG | 0 | 0 | 0 | eth2 | default-gateway to R1 |
| 172.16.138.0 | 0.0.0.0 | 255.255.254.0 | U | 0 | 0 | 0 | eth0 | to network 7 |
| 172.16.140.0 | 0.0.0.0 | 255.255.254.0 | U | 0 | 0 | 0 | eth1 | to network 8 |
| 172.16.143.100 | 0.0.0.0 | 255.255.254.252 | U | 0 | 0 | 0 | eth2 | to network 6 |

Following with R1:

| Destination | Gateway | Genmask | Flags | Metric | Ref | Use | Iface | //Comment |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0.0.0.0 | 172.16.143.66 | 0.0.0.0 | UG | 0 | 0 | 0 | eth2 | default-gateway to R2 |
| 172.16.138.0 | 172.16.143.102 | 255.255.254.0 | UG | 0 | 0 | 0 | eth3 | to network 7 via R4 |
| 172.16.140.0 | 172.16.143.102 | 255.255.254.0 | UG | 0 | 0 | 0 | eth3 | to network 8 via R4|
| 172.16.142.0 | 0.0.0.0 | 255.255.254.128 | U | 0 | 0 | 0 | eth0 | to network 1 |
| 172.16.142.128 | 0.0.0.0 | 255.255.254.128 | U | 0 | 0 | 0 | eth1 | to network 2 |
| 172.16.143.64 | 0.0.0.0 | 255.255.254.224 | U | 0 | 0 | 0 | eth2 | to network 3 |
| 172.16.143.100 | 0.0.0.0 | 255.255.254.252 | U | 0 | 0 | 0 | eth3 | to network 6 |

And R2:

| Destination | Gateway | Genmask | Flags | Metric | Ref | Use | Iface | //Comment |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0.0.0.0 | 172.16.143.98 | 0.0.0.0 | UG | 0 | 0 | 0 | eth2 | default-gateway to R3 |
| 172.16.138.0 | 172.16.143.65 | 255.255.254.0 | UG | 0 | 0 | 0 | eth1 | to network 7 via R1 |
| 172.16.140.0 | 172.16.143.65 | 255.255.254.0 | UG | 0 | 0 | 0 | eth1 | to network 8 via R1 |
| 172.16.142.0 | 172.16.143.65 | 255.255.255.0 | UG | 0 | 0 | 0 | eth1 | to network 1 via R1 |
| 172.16.143.0 | 0.0.0.0 | 255.255.254.192 | U | 0 | 0 | 0 | eth0 | to network 4 |
| 172.16.143.64 | 0.0.0.0 | 255.255.254.224 | U | 0 | 0 | 0 | eth1 | to network 3 |
| 172.16.143.96 | 0.0.0.0 | 255.255.254.252 | U | 0 | 0 | 0 | eth2 | to network 5 |
| 172.16.143.100 | 172.16.143.65 | 255.255.254.252 | UG | 0 | 0 | 0 | eth1 | to network 6 via R1 |

For implementing the policy of directing traffic from Network 8 towards ISP2 and the rest to ISP1, we defined two tables, as the following execution of *ip rule* shows

```
> ip rule
0: from all lookup local
32765: from 172.16.140.0/23 lookup 99
(...)
```

Which means that traffic coming from the network 8 will be routed following the table 99, and not the main routing table. This table looks like the following: (directing traffic to ISP2)

```
> ip route list table 99
default via 172.31.0.253 dev eth1
```

Whereas for the general table:

```
> ip route list
default via 172.31.0.254 dev eth1
(...)
```

Directing the rest of the traffic to the ISP1.