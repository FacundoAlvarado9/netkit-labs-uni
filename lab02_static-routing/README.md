# Lab 02 - Static Routing
## Topology
From both the *lab.conf* file (provided by the lab tutor) and the startup files, resulted the following topology

![Topology for lab 02](/readme_img/lab02_topology.jpg)

## Static routing
The routing was done by setting the default gateways. 
- For the workstations, pointing to the routers directly connected to them.
- For the routers at the outer margins (R1, R2 and R4) the default gateway was set to the interface of R3 connected to one of their networks (Network2, Network5 and Network3 respectively). Since R3 "knows" them all, any package going from any device to another will reach its destination with no major trouble.
