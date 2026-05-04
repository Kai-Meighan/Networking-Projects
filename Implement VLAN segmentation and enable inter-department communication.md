# Weekly Project - Implement VLAN segmentation and enable inter-department communication

## Background

The company is restructuring its internal network to improve security and traffic management. Currently, all devices exist on a single flat network, which creates unnecessary broadcast traffic and poses a security risk.

You have been assigned to segment the network by department while ensuring that communication between departments is still possible where required.

## Topology

**Devices**:
- 1x Router
- 1x Switch (for ROAS phase)
- 1x L3 Switch (for later phase)
- 3x PCs (one per VLAN)

**Physical Layout**
PC1 ──┐
      ├── Switch ─── Router   (ROAS phase)
PC2 ──┤
PC3 ──┘

## **Requirements**

### 1. **Network Segmentation**

You must divide the network into the following VLANs:

- **VLAN 10** → Admin
- **VLAN 20** → Sales
- **VLAN 30** → IT

Each department must be logically isolated at Layer 2.

---

### 2. **Device Assignment**

- Each department will have **one workstation**
- Each workstation must be placed in the correct VLAN
- End devices must be able to communicate within their own VLAN

---

### 3. **Inter-VLAN Communication (Phase 1)**

The business requires all departments to communicate with each other.

You must:

- Enable communication between VLANs using a **router-based solution**
- Ensure all devices can successfully reach devices in other VLANs

---

### 4. **Infrastructure Constraint**

- Only a **single physical link** is available between the switch and the routing device
- The solution must support traffic from all VLANs across this link

---

### 5. **Inter-VLAN Communication (Phase 2 Upgrade)**

As part of a performance improvement initiative:

- Replace the router-based solution with a **Layer 3 switching solution**
- Maintain full inter-VLAN communication
- Remove any unnecessary devices from the topology

---

### 6. **IP Addressing**

- Each VLAN must use a **separate IP subnet**
- All devices must be correctly addressed
- Each device must be able to reach its default gateway


**Step 1**
Let's divide the network into three destinct VLANs; Admin, Sales and IT. 

VLAN 10
<img width="820" height="103" alt="image" src="https://github.com/user-attachments/assets/26bbdebf-c449-42eb-8806-381b88f08356" />

_Also will change the hostname on each_

<img width="561" height="102" alt="image" src="https://github.com/user-attachments/assets/0893586d-3a39-465f-8e00-98cb119eab69" />

VLAN 20

<img width="695" height="222" alt="image" src="https://github.com/user-attachments/assets/9d94461d-d0f5-49c7-a3db-bca4cd233a9f" />

VLAN 30

<img width="670" height="222" alt="image" src="https://github.com/user-attachments/assets/b8923b9b-d725-45c0-96b5-7ec6674a84e2" />

Verfication: 

<img width="1366" height="543" alt="image" src="https://github.com/user-attachments/assets/2f5a756b-e32f-40a0-a1b7-a65c4a30d14d" />

2. For step two, this was largely taken care of when building out the topology and as there is only one device per VLAN, intra-VLAN communication is not necessary

3. Ok now the devices need to be able to talk to eachother, to do this we will first start by adding in two more links from SW1 to R1, to support our three VLANs.

<img width="886" height="630" alt="image" src="https://github.com/user-attachments/assets/a13f870c-0711-44c9-82b4-d396d2f720d8" />

To start, lets configure the switch side of things and have all interfaces as access ports to their dedicated VLANs

<img width="956" height="650" alt="image" src="https://github.com/user-attachments/assets/400690ad-7a68-461a-9573-6b06eeb6fe20" />


For the router side of things, we will configure the default gateway on each of the respective router interfaces, for all inter-vlan traffic to pass through

<img width="1310" height="205" alt="image" src="https://github.com/user-attachments/assets/1e3eab5a-c010-4c31-ad0b-b724c2db6c11" />


A ping from PC1 destinned for PC2 travels via SW2 to R1.

<img width="1015" height="701" alt="image" src="https://github.com/user-attachments/assets/d6fd95f9-f3c7-4ad7-bfaa-81f2fa74a5dd" />

Then via R1 to PC2

<img width="920" height="677" alt="image" src="https://github.com/user-attachments/assets/bf8e8fe7-7d08-4179-8ae5-aa8157482971" />

So now what we have done is set up VLANs so broadcast traffic remains in the respective VLANs and we have also configured inter-VLAN routing to ensure that devices have to pass through the router to communciate with one another.


4. So now, only one physical link is permitted between SW1 and R1, thankfully we can still make this work via a Router-on-a-Stick configuration.

To do this we have to first get rid of the three connections from SW1 to R1 and replace with a singular connection. set up a trunk link on SW2. 

<img width="854" height="616" alt="image" src="https://github.com/user-attachments/assets/c26dbc3c-517b-40d3-afdc-753b6c95ea33" />

_I also removed the IP addresses from the other two interfaces that were being used before._

Now we need to set a trunk link of f0/5 to allow traffic from all three VLANs to pass. 

<img width="665" height="55" alt="image" src="https://github.com/user-attachments/assets/593ed621-0e84-41ad-9ebd-84fd364942f8" />

<img width="1037" height="129" alt="image" src="https://github.com/user-attachments/assets/df26f822-690c-4fb7-909c-c792be85e4e5" />

Now for R1, we will need to config sub interfaces on g0/1 to the default gateway of each of our three VLANs and also specify from what VLAN each device's traffic is destined fromn.

<img width="1232" height="254" alt="image" src="https://github.com/user-attachments/assets/693246d7-3b1a-4bd9-8b4e-dbb8d4da128a" />

<img width="1310" height="451" alt="image" src="https://github.com/user-attachments/assets/5d384fb0-f354-4ff6-985a-87abd6583329" />

For each sub-interface, we had to ensure we used the 'encapsulation dot1q <vlan-id>' command to specify for this sub-interface here is the VLAN dedicated to it. 

