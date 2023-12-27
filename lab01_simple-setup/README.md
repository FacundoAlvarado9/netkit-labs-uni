# Lab 01 - Simple setup
This lab showcases the simplest of examples: the connection of two computers via a single collision domain.

## How was it achieved
A simple collision domain named "DCa" was declared in the *lab.conf* file, and on each *startup file* the following IP addresses are asigned:

- 192.168.1.1/24 for pc1
- 192.168.1.2/24 for pc2

Resulting in the following topology

![Topology for lab 01](/readme_img/lab01_topology.jpg)

## Testing
Testing can be achieved simply with the use of the *ping* command. For example, from pc1:

```
ping 192.168.1.1
```