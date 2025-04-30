# Basic-Data-and-Voice-VLAN-Setup-Homelab
  Objective
To set up an office network with four VLANs: one for regular office staff, one for HR,
one for IT and one for VoIP (Voice over IP) devices like IP phones. A core switch will
connect two access switches, and you will test connectivity between them.

Software Requirements:
- CISCO Packet Tracer

Step-by-Step Instructions
Step 1: Add Devices to the Workspace
1. Open Cisco Packet Tracer.
2. From the End Devices section, drag and drop the following devices onto the
workspace:
○ 5 PCs (PC-0, PC-1, PC-2, PC-3, PC-5)
○ 2 IP Phones (IP Phone 0, IP Phone 1)
3. From the Network Devices section, drag and drop:
○ 3 Switches (Switch-0, Switch-1, Switch-2)
Step 2: Physically Connect Devices
Wired Connections
1. Click on the cable icon and select a copper straight-through cable.
2. Connect the following devices using the copper straight-through cable.
○ PC-0 to Switch-0 (e.g., FastEthernet 0/1).
○ IP Phone 0 to Switch 0 (e.g., Switch to FastEthernet 0/2)
○ PC-1 to IP Phone 0. (e.g., PC to FastEthernet 0)
○ IP Phone 1 to Switch 0 (e.g., Switch to FastEthernet 0/6)
○ PC-2 to IP Phone 1. (e.g., PC to FastEthernet 0)
○ PC-3 to Switch-1 (e.g., FastEthernet 0/1).
○ PC-2 to Switch-1 (e.g., FastEthernet 0/2)
3. Connect the Access Switches to the Core Switch .
○ Switch-0 to Switch-2 (e.g., GigabitEthernet 0/1 to GigabitEthernet 0/1).
○ Switch-1 to Switch-2 (e.g., GigabitEthernet 0/1 to GigabitEthernet 0/2).


Step 3: Configure the Core Switch
1. Click on Switch-2, go to the CLI tab.
2. Configure the hostname for the core switch
Switch-2> enable
Switch-2# configure terminal

Switch-2(config)# hostname CoreSwitch
CoreSwitch(config)#
3. Create VLANs for the following: 10 - Office, 20 - HR, 30 - IT , 40 - Voice
CoreSwitch(config)# vlan 10
CoreSwitch(config-vlan)# name Office
CoreSwitch(config-vlan)# vlan 20
CoreSwitch(config-vlan)# name HR
CoreSwitch(config-vlan)# vlan 30
CoreSwitch(config-vlan)# name IT
CoreSwitch(config-vlan)# vlan 40
CoreSwitch(config-vlan)# name Voice
CoreSwitch(config-vlan)# exit


you can command as " sh vlan br" to see the list for changes.


4. Configure Trunk ports on Core Switch
Trunk ports allow multiple VLANs to pass through a single link between
switches.
Setup Trunk port for Access Switch 1
CoreSwitch(config)# int gi0/1
CoreSwitch(config-if)#switchport mode trunk
CoreSwitch(config-if)#switchport trunk allowed vlan


Setup Trunk port for Access Switch 2
CoreSwitch(config)# int gi0/2
CoreSwitch(config-if)#switchport mode trunk
CoreSwitch(config-if)#switchport trunk allowed vlan
all
5. Save the configuration:
CoreSwitch# write memory


Step 4: Configure VLANs on Access Switch 1
1. Click on Switch-0, go to the CLI tab.
2. Configure the hostname for the core switch
Switch-0> enable
Switch-0# configure terminal
Switch-0(config)# hostname Access1
3. Create VLANs for the following: 10 - Office, 20 - HR , 40 - Voice
CoreSwitch(config)# vlan 10
CoreSwitch(config-vlan)# name Office
CoreSwitch(config-vlan)# vlan 20
CoreSwitch(config-vlan)# name HR
CoreSwitch(config-vlan)# vlan 40
CoreSwitch(config-vlan)# name Voice
CoreSwitch(config-vlan)# exit

4. Configure the uplink to the core switch as a trunk:
Access1(config)# int gi0/1
Access1(config-if)#switchport mode trunk
Access1(config-if)#switchport trunk allowed vlan
10,20,40
5. Assign access and voice ports to VLAN 10 for office computers:
Access1(config)# int range fa0/1-5
Access1(config-if)#switchport mode access
Access1(config-if)#switchport access vlan 10
Access1(config-if)#switchport voice vlan 40
6. Assign access and voice ports to VLAN 20 for HR computers:
Access1(config)# int range fa0/6-10
Access1(config-if)#switchport mode access
Access1(config-if)#switchport access vlan 20
Access1(config-if)#switchport voice vlan 40


7. Save the configuration:
Access1# write memory
