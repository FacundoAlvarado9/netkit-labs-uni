# netkit-labs-uni
## What is this?

Firstly, this is a backup repo for the Netkit labs I (and another classmate) worked on in the lecture *Redes de Computadoras* (_Computer Networks_) at Universidad Nacional del Sur, Bahía Blanca, Argentina.

It is also a showcase of simple examples of Netkit labs.

## What is Netkit?

According to its website, Netkit is "the poor man's way to experiment with computer networking". It is basically an environment that lets you set up virtual network devices (computers, routers, switches, etc.) that can be connected to one another, emulating a network from the real world.

## About the labs showcased here

Each lab touches on a different concept and grows in complexity. From the set-up of a simple connection between two computers, to static routing, to setting up simple DNS and DHCP servers, and finally dynamic-routing using Quagga. The last lab was a form of practical exam, done without help and combining most of the topics already mentioned here.

## Structure of the labs

(Little explanation based on this [Netkit introductory guide by G. Di Battista et al. at the Università degli Studi Roma Tre](https://igm.univ-mlv.fr/ens/Licence/L3/2009-2010/Reseau/netkit/netkit-introduction.pdf))

In each lab you will find:

- The **lab.conf** file:
	It contains settings to the VMs that make up the lab, as well as the topology of the network that interconnects these. For example, for the definition of a collision domain

	```
	machine[arg] = value
	```
	Being *arg* an integer i, it defines for the *machine* a collision domain named as the variable *value* to which the interface *ethi* is attached. For example, the following definition

	```
	pc1[0] = A
	
	pc2[0] = A
	pc2[1] = B

	pc3[0] = B
	```
	Would result in the following topology:
	![Topology for the previous definition](/readme_img/labconf_topology_example.png)