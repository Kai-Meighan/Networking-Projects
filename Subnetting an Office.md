# Subnetting an Office

## Project Brief

You’ve just joined TechCorp as a junior network engineer.

The company is expanding quickly and needs a structured internal network. Right now, everything is flat and unmanaged—your job is to design and implement a scalable IP addressing scheme and ensure communication between all departments.

You’ve been given one IP block and a list of requirements.

No further guidance.

## 🎯 Objective

Design and deploy a working network in Packet Tracer that:

- Efficiently uses IP space
- Supports all departments
- Enables full communication across the organisation
- Is logically structured and scalable

## 📦 Available Address Space
192.168.10.0/24

You may not request more.

## 🏢 Department Requirements
Engineering - 60 Hosts Required
HR - 15 Hosts Required
Management - 7 Hosts Required
IT - 5 Hosts Required

## 🧠 Design Constraints
Each department must be placed in its own subnet
IP space must be allocated efficiently (no excessive waste)
The network must allow for future growth where possible
All departments must be able to communicate with each other


## 🏗️ Infrastructure Constraints

You are provided:
- 2 Routers
- 5 Switches (1 for each subnet)
- End devices (PCs)

You may not add additional routers.

You must:

1. Physically separate parts of the network across both routers
2. Establish connectivity between routers

## 🔗 Connectivity Requirements
Routers must communicate with each other
All subnets must be reachable across the network
Inter-department communication must function reliably

## 🧪 Validation Requirements

At minimum, your network must successfully:

Transmit traffic between all departments
Pass end-to-end connectivity tests (e.g. Engineering → IT)

Failure to achieve full connectivity = failed deployment


# Project 


First I'll do up a quick topology with all of the components out within Packet Tracer:

<img width="1072" height="716" alt="image" src="https://github.com/user-attachments/assets/03f2a319-f596-4701-ac7c-351df2ca3100" />


Since there are a different number of hosts per planned subnet and one of our goals is to reduce as much waste per subnet as possible, I will use Variable-Length Subnet Masks in order to appropriately subnet this network. 

So following the VLSM process, let's start by assigning the start of the IP address block to the biggest host, which in our case is **Engineering** with 60 hosts. So Engineering's Network Address will be 192.168.10.0.

Now to find the prexfix length, we need to work backwards from the number of hosts we have. We have 60 hosts so 2^Host Bits -2, must be 60 or greater. 

Doing the maths we can see the 2^6 is 64, minus 2 for the network and broadcast address, I can see we have a total of 62 usable addresses, which is ideal for our subnet. Therefore the engineering subnet will use a /26 prefix length. 

Since there are 64 total addresses in a /26 prefix length, we can also determine the Broadcast address for the Engineering subnet will be 192.168.1.63 /26. Below is a screenshot of the 1st subnet we have drawn out on our topology:

<img width="1472" height="653" alt="image" src="https://github.com/user-attachments/assets/ef0e7ab2-03ab-4ccd-8b37-8a105c52c630" />


Now we have to configure the IP addresses on the relevant devices. First R1's g0/0/0 interface. We will configure this with the last usable address of the subnet block which is 192.168.10.62.

<img width="934" height="115" alt="image" src="https://github.com/user-attachments/assets/0f079938-7380-4c66-b4e7-9dc7411bf054" />

Next we will do PC3 and PC4's IP addresses. For this, I will just use the first and second usable addresses of 192.168.10.1 and 192.168.10.2

_PC3_
<img width="1820" height="661" alt="image" src="https://github.com/user-attachments/assets/034a2972-8881-4965-8baf-533d08c310e7" />

_PC4_
<img width="2195" height="699" alt="image" src="https://github.com/user-attachments/assets/bf28d387-b316-4750-9ebf-349f3de0db4a" />

Next we need to move to the second largest subnet, which will be the **HR subnet** with 15 hosts. We can add one on from the broadcast address of subnet 1 to get the network address of this subnet, which will be 192.168.10.64.

In order to find the broadcast address, we will have to look for the prefix length first. So originally, I was thinking that a /28 prefix length would work as that has 16 usable IP addresses but taking away the broadcast and network addresses I can see that this would only result in 14 usable addresses, so not enough. A /27 prefix length gives us 30 total usable addresses so that can work a bit better. 

So the prefix lenth for the HR subnet will be /27 and therefore the subnet mask will be 255.255.255.224. We can deduce the broadcast address from this will be, 192.168.10.95. Now we have the network address and the broadcast address, we can assign PC1, PC2 and R1's g0/0/1 interface with the correct IPs. 

_R1 - 192.168.10.94_

<img width="970" height="112" alt="image" src="https://github.com/user-attachments/assets/276fe3b8-b426-45d9-b82e-a5092fc5666c" />

_PC1 - 192.168.10.65_

<img width="2127" height="568" alt="image" src="https://github.com/user-attachments/assets/849914e6-a1ef-43f7-8c40-34b02087b10f" />


_PC2 - 192.168.10.66_

<img width="2196" height="543" alt="image" src="https://github.com/user-attachments/assets/bea59ece-92ea-4dde-823b-d541d26f251f" />


Now onto the third-largest subnet, which will be the **management subnet** with 7 hosts. With 7 hosts a /28 prefix length will give us 16 total addresses, with 14 of them usable - which is perfect. So the subnet mask will be 255.255.255.240.
The network address will be subnet 2's broadcast address plus 1, so it will be 192.168.10.96. 
With the network address and prefix length deduced, we can work out the broadcast address which will be 192.168.10.111.

With these addresses and prefix length's in mind, we can now configure the Management subnet.

_R2 g0/1 Interface - 192.168.10.111_

<img width="1058" height="162" alt="image" src="https://github.com/user-attachments/assets/5b000c7b-1d1f-44da-a7fd-73bb789f61d4" />

_PC7 - 192.168.10.98_

<img width="1923" height="529" alt="image" src="https://github.com/user-attachments/assets/1aed4a85-381a-4f72-bbdd-ada1d81a19da" />

_PC8 - 192.168.10.99_

<img width="1817" height="584" alt="image" src="https://github.com/user-attachments/assets/e682d8a0-1971-4cbd-8f61-358045c2570a" />


Finally, the last subnet, **the IT subnet** with 5 hosts. 5 hosts allows us to use a /29 prefix length as this gives us 8 total addresses, 6 of them being usable, thus perfect for our requirements. This gives us a subnet mask of 255.255.255.248.

Adding 1 onto the broadcast address of the management subnet, we get the network address of IT subnet, which will be 192.168.10.112. We also know that a /29 prefix length will give us 8 total addresses, so we can deduce the broadcast address of the subnet, being 192.168.10.119. 

Let's configure the relevant devices IP addresses. 

_R2 G0/2 Interface - 102.168.10.118_

<img width="1026" height="108" alt="image" src="https://github.com/user-attachments/assets/5a74f9ab-eb3b-4d03-b098-17d032c80079" />

_PC9 - 192.168.10.113_

<img width="1623" height="545" alt="image" src="https://github.com/user-attachments/assets/2f23b654-f9e2-49d2-9b06-cc1b34077771" />

_PC10 - 192.168.10.114_

<img width="1709" height="571" alt="image" src="https://github.com/user-attachments/assets/e1569c34-e24c-4c10-8ab2-5e14bc80d5f1" />


The last thing we have to do is configure a P2P link between the two routers. For this we will use a /30 prefix length, as is normal for P2P links. The network address can be 192.168.10.120 and with only 4 available addresses the broadcast address will be 192.168.10.123.

That leaves R1's g0/2 interface with the IP of 192.168.10.121 and R2's g0/0 interface with the IP of 192.168.10.122

R1 interface configuration

<img width="920" height="75" alt="image" src="https://github.com/user-attachments/assets/f1d43e2c-608e-45f1-95a7-7b8d2934d9a6" />


R2 interface configruation

<img width="1053" height="39" alt="image" src="https://github.com/user-attachments/assets/fccc6baa-e4b2-4c21-a5b4-318bb0791942" />



Now in order to test everything works, I will set up two static routes on each router to test connectivity. 

R1 Static Routes to Management and IT Subnet

<img width="1082" height="42" alt="image" src="https://github.com/user-attachments/assets/11fb59f1-edce-45da-b695-ddebad0f01ee" />

<img width="1139" height="307" alt="image" src="https://github.com/user-attachments/assets/4de14ecd-b032-4842-8d30-19dbf2b2f50d" />

_Verification_

<img width="1056" height="43" alt="image" src="https://github.com/user-attachments/assets/7cd461c2-8e77-4e83-8740-6a2ac248799b" />


R2 Static Routes to Engineering and HR Subnets

<img width="1058" height="36" alt="image" src="https://github.com/user-attachments/assets/750fa764-f8c7-43c5-8011-3d930921fa76" />

<img width="1123" height="371" alt="image" src="https://github.com/user-attachments/assets/9910eef2-962a-4e45-aed1-668a8a699ccd" />


_Verification_

<img width="922" height="426" alt="image" src="https://github.com/user-attachments/assets/2e8661ab-6dae-4a5e-b0e8-2b0875f445ec" />


**Connectivity Test**

PC1 to PC7 - Works as expected

<img width="934" height="436" alt="image" src="https://github.com/user-attachments/assets/3c7e07f5-cf0c-450b-8e3c-781c1dc07626" />


PC4 to PC10 - Works as expected

<img width="911" height="435" alt="image" src="https://github.com/user-attachments/assets/020212e2-1d46-4c39-94dc-333a16e0c66b" />



So now we have a fully functioning network, all subnetted to their appropriate sizes and prefix lengths. For each subnet, I went through the calculations of how each prefix length was determined, what the first and last usable IP addresses were and then what the network and broacast addresses for. We did only get to a max address of 192.168.1.123, which means we still have 131 addresses still remaining in our network block, allowing for the scalability of more subnets for the organisation. 

**Here is the finalised topology:**
<img width="1461" height="546" alt="image" src="https://github.com/user-attachments/assets/3ce89c57-5ea6-447f-ab73-5111ac2d40c2" />


