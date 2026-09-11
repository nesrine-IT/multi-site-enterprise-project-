# multi-site-enterprise-project
## Description 
This project simulates a highly-available multi-site enterprise network consisting of headquarter and branch using cisco technologies in cisco packet tracer . it focuses on :
* Network segmentation
* Redudancy of links and gateways
* Dynamic and static routing
* Network security
* centralized services
* secure communication between sites "VPN"
## Network architecture 
The HQ network is divided into four offices . Each one has its own VLAN . There are additional subnets and Vlans that are used for company's severs and management traffic as follows:

| VLAN | Office     | Network       |
| ---- | ---------- | -----------   |
| 93   | IT         | `172.16.1.0/25` |
| 94   | Sales      | `172.16.1.128/25`|
| 95   | Accounting | `172.16.2.0/25`|
| 96   | HR         | `172.16.2.128/25`|
| 97   | Servers    | `172.16.3.0/28`|
| 99  | Management   | `172.16.99.0/24`|

The Branch contains three Vlans for offices "HR,Accounting and Marketing" and another vlan for Wirless LAN (WLAN)

| Office | Network |
|---|---|
| HR | `192.168.0.0/27` |
| Accounting | `192.168.0.32/27` |
| Marketing | `192.168.0.64/27` |
| WLAN| `192.168.99.0/24` |

# Topology
HeadQuarter Site : it consists of two access switches for and two Multi-Layer Switches (MLS) for inter-vlan routing .DHCP server  as a centralized service for automatically assigning IP configuration to devices, and a an edge router for external internet connexion.

Branch Site : it consists of one switch to attach all vlans , one router that performs both inter-vlan and external internet routing and Access Point (AP) for wireless connection of laptops corresponding to each vlan to the network.

<img width="1540" height="607" alt="cis" src="https://github.com/user-attachments/assets/0e222d76-0168-40fa-95d9-f831449c58f3" />

# Switching

### VLAN Segmentation

VLANs are used to logically separate offices and reduce unnecessary Layer 2 broadcast traffic.
### EtherChannel

EtherChannel is used to combine multiple physical links into a single logical connection  in order to provide link redundancy ,increase bandwidth  and avoid STP port blocking.
### PVST

instead of blocking ports by STP permanently to avoid layer 2 loops and wasting the use of the redundant switches,PVST is applied on MLS1 and MLS2 to determine which path should be forwarding traffic and which one remains blocked . In case of this project vlan 93,94,99 will be forwarded by MLS1 and blocked by MLS2 and vice versa for vlan 95,96,97
### HSRP

HSRP provides first-hop redundancy for the internal VLANs. Instead of hosts relying directly on a single physical Layer 3 switch as their default gateway, they use a **virtual IP address** shared between the redundant distribution switches. Where the active MLS use higher priority and standby use lower priority.
in this project vlan 93,94 are gathered in group 1 with priority 200 in Multi-Layer Switch 1 which makes it the main gateway and priority 100 in MLS2 to take over when MLS1 falls down .While vlan 95,96,97 are gathered into group numbered 2 with priority 200 in MLS2 and priority 100 in MLS1.

###  Wireless LAN:
in branch site , instead of using wired connection between end devices and access switch , access points are used for wireless connection and they provide vlan segementation as a normal switch. the link between AP and SW is a trunk.

# Routing 

### Switch Virtual Interface (SVI)
In HQ site , Multi-Layer Switches are used for inter-vlan routing ,so SVIs are used to provide layer-gateways for  VLANs.
### Router On a Stick (ROaS)
In the branch site , the access switch is directly connected to edge router on a trunk interface. on the router we configure sub-interfaces tagged with 801.1Q to perform routing between vlans.
### Static routing 
In HQ ,default routing was configured on MLS1,2 to forward any packet going to external network to next hop which is the router interfaces 10.0.255.1/5. and static routing was configured on the router to foward back the internet packet to MLS1, through 10.0.255.2/5 interfaces.
### OSPF routing
OSPF was configured between routers in order to exchange routing tables and calculating the shortest path without needing to configure each one statically.
### NAT/PAT
NAT is configured on internal and external interfaces of router to translate private IP addresses to the public address 20.0.0.2using different ports.
# Network security 
### Access-lists 
Access-lists are filter traffic ,to determine which vlans are allowed to use external networks , and which are allowed to access network devices through **ssh** , in this lab there are 3 necessary ACLs:
* NAT access-list to allow users access internet using public IP for network security.
* VPN access-list to allow users access site A and B ressources as they are in the same place using private IPs
* SSH access-list to allow only management vlan access network devices.
### Port-security
port security are configured on access ports to allow only one device to be connected and its mac address is detected automatically while plugging the device.
  
 
 
