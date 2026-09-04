# multi-site-enterprise-project-
## Description 
This project simulates a highly-available multi-site enterprise network consisting of headquarter and branch using cisco technologies in cisco packet tracer . it focuses on :
* Network segmentation
* Redudancy of links and gateways
* Dynamic and static routing
* Network security
* centralized services
* secure communication between sites "VPN"
## Network architecture 
The HQ network is divided into four offices . Each one has its own VLAN and subnet and other subnet is used for company's severs as follows:

| VLAN | Office     | Network       |
| ---- | ---------- | -----------   |
| 93   | IT         | 172.16.1.0/25 |
| 94   | Sales      | 172.16.1.128/25|
| 95   | Accounting | 172.16.2.0/25|
| 96   | HR         | 172.16.2.128/25|
| 97   | Servers    | 172.16.3.0/28|
