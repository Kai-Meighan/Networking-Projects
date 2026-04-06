# Small Office LAN Challenge

## Scenario

You are a junior network engineer tasked with deploying the network infrastructure for a small office with three employees.

The company requires that all workstations on the office floor be able to communicate over a local Ethernet network.

Your job is to design, configure, and verify a basic switched LAN that supports communication between all hosts.

## Objectives

Build and configure a functional Ethernet LAN.

Demonstrate understanding of:

- Ethernet switching
- IPv4 addressing
- Switch interface configuration
- Packet forwarding within a LAN

## Requirements

Your network must include:

1. One switch
2. Three PCs
3. Ethernet connectivity between devices

All hosts must:

- Be placed within the same IPv4 subnet
- Have unique addresses
- Successfully communicate with each other
- Verification Requirements

You must demonstrate:

Successful host-to-host communication
That the switch interfaces are operational


## Lab

First things first, lets get all of our devices out onto a blank packet tracer canvas


<img width="679" height="455" alt="image" src="https://github.com/user-attachments/assets/b33f73e1-fd50-4512-9027-f21ededcbce6" />

I will give all devices the following hostnames; PC1, PC2, PC3, SW1 


_PC2_


<img width="699" height="714" alt="image" src="https://github.com/user-attachments/assets/a60e4f9a-a62f-4437-ae03-654a77e5c73a" />


_SW1_


<img width="610" height="156" alt="image" src="https://github.com/user-attachments/assets/501ba315-5ef0-4d09-ad55-54a18ce2326a" />


Next we need to get the devices connected to the main switch. For that a simple UTP copper straight-through cable can be used. A straight-through cable will work here are we are going from PC to switch. A crossover cable would be needed if going switch-to-switch.


<img width="548" height="367" alt="image" src="https://github.com/user-attachments/assets/5fbab681-8a4e-4cf2-a60d-bb86a7823e1e" />


We are now all connected to the switch interfaces. There is no configuration needed on the switch interfaces themselves as Packet Tracer auto-negotiates duplex and speed, so for simplicities sake we can just allow that to occur.


<img width="1346" height="175" alt="image" src="https://github.com/user-attachments/assets/4ac2ee4b-01fa-4fd3-9c4d-a920c30e761b" />


Now in order for host-to-host communication, the devices need a way to identify one another. For this we can assign IP addresses to each of the devices. 


_PC1_


<img width="698" height="705" alt="image" src="https://github.com/user-attachments/assets/fadedf93-19c5-49b0-b3a5-b195be174a13" />


_Updated Topology with addressing_


<img width="700" height="497" alt="image" src="https://github.com/user-attachments/assets/6e07ea34-bdc7-478a-baf7-bf854c8e75a9" />


Now that we are connected to the relevant interfaces on the switch and each device has a L3 way of uniquely identifying eachother, we can use Packet Tracer's simulation mode in order to trace the initial ARP traffic and then visualise the ICMP request and reply


**PC1 Ping to PC2**


1. Initial ARP Request goes out to find the MAC address for PC2. The switch, operating at Layer 2, has no knowledge of layer 3 addressing therefore it has to operate based off of MAC addresses and ARP is the initial protocol used to find a devices MAC address from its IP address. In our topology, PC1 sends an ARP request looking for who has PC2's IP address. PC3 will drop the packet as it reads the identified IP and realises it is not them. 

<img width="641" height="441" alt="image" src="https://github.com/user-attachments/assets/37071789-94d1-471b-8d30-efe8d041c429" />


2. PC2 responds to the ARP request with an ARP reply, containing its MAC address


<img width="872" height="611" alt="image" src="https://github.com/user-attachments/assets/59e11de7-07e2-4df2-b46d-7fc8aea9dcd7" />


3. Now that PC1 knows PC2's MAC address, ICMP traffic can now flow between the devices with the switch brokering the traffic


<img width="697" height="683" alt="image" src="https://github.com/user-attachments/assets/ba92cc98-f0a1-4c2f-a2ec-6a41a84ecc98" />


 <img width="786" height="419" alt="image" src="https://github.com/user-attachments/assets/0fab2e9b-5d39-4fea-8cf9-187ec567207a" />


 _Verification of PC1 now knowing PC2's MAC address within its ARP table_

 
 <img width="909" height="130" alt="image" src="https://github.com/user-attachments/assets/cdb7c00a-1003-4fd9-91d6-c41448987078" />

