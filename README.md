# Ex. No: 8   Packet Tracer: Connect a Router to a LAN
# Date: 26-09-2025
<br>

## Objective
Configure and verify router LAN/WAN interfaces in Cisco Packet Tracer and test end-to-end connectivity.<br>
•	Display key router information (interfaces, status, routing table).<br>
•	Configure IPv4 addresses and descriptions on R1 and R2 interfaces.<br>
•	Bring interfaces up and verify with show commands and pings. <br>
<br>

## Apparatus / Tools Required
•	Cisco Packet Tracer<br>
•	2 Routers (R1, R2 — 2911 or equivalent)<br>
•	2 Switches (S1, S2)<br>
•	4 PCs (PC1–PC4) with NICs<br>
•	Copper straight-through cables for LAN links; Serial DCE/DTE cable for WAN link <br>
<br>

## Network Topology Diagram
(Insert your Packet Tracer screenshot showing R1—S1—PC1/PC2 and R2—S2—PC3/PC4, plus R1–R2 Serial link S0/0/0.)<br>
<br>
Addressing Table (from activity)<br>
Device	Interface	IP Address	Subnet Mask	Default Gateway<br>
R1	G0/0	192.168.10.1	255.255.255.0	—<br>
R1	G0/1	192.168.11.1	255.255.255.0	—<br>
R1	S0/0/0 (DCE)	209.165.200.225	255.255.255.252	—<br>
R2	G0/0	10.1.1.1	255.255.255.0	—<br>
R2	G0/1	10.1.2.1	255.255.255.0	—<br>
R2	S0/0/0	209.165.200.226	255.255.255.252	—<br>
PC1	NIC	192.168.10.10	255.255.255.0	192.168.10.1<br>
PC2	NIC	192.168.11.10	255.255.255.0	192.168.11.1<br>
PC3	NIC	10.1.1.10	255.255.255.0	10.1.1.1<br>
PC4	NIC	10.1.2.10	255.255.255.0	10.1.2.1<br>
Note (per activity): Console password = cisco; Privileged EXEC password = class. <br>
<br>

## Procedure
### Part 1: Display Router Information (R1 shown; repeat on R2)
1.	Click R1 → CLI. If prompted, use console password cisco, then enable and password class. <br>
2.	View interface statistics (all):<br>
3.	R1# show interfaces<br>
4.	View a specific interface (example, Serial 0/0/0):<br>
5.	R1# show interfaces serial 0/0/0<br>
o	Note IP address and bandwidth from the output. <br>
6.	View G0/0 details:<br>
7.	R1# show interfaces gigabitEthernet 0/0<br>
o	Note IP, MAC address, and BW. <br>
8.	Get a brief summary of all interfaces:<br>
9.	R1# show ip interface brief<br>
o	Count Serial and Ethernet ports, and check their status (up/down). <br>
10.	Display routing table:<br>
11.	R1# show ip route<br>
o	Identify C (connected) and any dynamic routes; note how unknown networks are handled. <br>
<br>

### Part 2: Configure Router Interfaces
R1 – LAN Interfaces<br>
1.	Enter global config: R1# configure terminal<br>
2.	G0/0 (LAN to S1 / VLAN with PC1)<br>
3.	R1(config)# interface g0/0<br>
4.	R1(config-if)# ip address 192.168.10.1 255.255.255.0<br>
5.	R1(config-if)# description LAN connection to S1<br>
6.	R1(config-if)# no shutdown<br>
7.	G0/1 (LAN to S1 / VLAN with PC2)<br>
8.	R1(config)# interface g0/1<br>
9.	R1(config-if)# ip address 192.168.11.1 255.255.255.0<br>
10.	R1(config-if)# description LAN connection to S1 (PC2 VLAN)<br>
11.	R1(config-if)# no shutdown<br>
R1 – WAN Serial (to R2)<br>
R1(config)# interface s0/0/0<br>
R1(config-if)# ip address 209.165.200.225 255.255.255.252<br>
R1(config-if)# clock rate 64000   ← (DCE end)<br>
R1(config-if)# description WAN link to R2<br>
R1(config-if)# no shutdown<br>
(Use show controllers serial 0/0/0 if you need to confirm DCE/DTE in PT.) <br>
R2 – LAN Interfaces<br>
R2# conf t<br>
R2(config)# interface g0/0<br>
R2(config-if)# ip address 10.1.1.1 255.255.255.0<br>
R2(config-if)# description LAN connection to S2 (PC3)<br>
R2(config-if)# no shutdown<br>
R2(config)# interface g0/1<br>
R2(config-if)# ip address 10.1.2.1 255.255.255.0<br>
R2(config-if)# description LAN connection to S2 (PC4)<br>
R2(config-if)# no shutdown<br>
R2 – WAN Serial (to R1)<br>
R2(config)# interface s0/0/0<br>
R2(config-if)# ip address 209.165.200.226 255.255.255.252<br>
R2(config-if)# description WAN link to R1<br>
R2(config-if)# no shutdown<br>
Save to NVRAM (both routers)<br>
R1# copy running-config startup-config<br>
R2# copy running-config startup-config<br>

<br>

### Part 3: Verification & Testing<br>
1.	Quick interface check (both routers)<br>
2.	R1# show ip interface brief<br>
3.	R2# show ip interface brief<br>
o	Ensure all configured ports show correct IPs and Status/Protocol: up/up. <br>
4.	Routing table review<br>
5.	R1# show ip route<br>
6.	R2# show ip route<br>
o	Count C (connected) and any dynamic routes; total should align with the number of LANs/WANs in the topology. <br>
7.	End-to-end pings (examples from the activity):<br>
o	From PC1, ping PC4.<br>
o	From R2, ping PC2.<br>
o	You should also be able to ping all active router interfaces from the PCs. (Switches are not configured for management; you won’t ping them.) <br>
<br>

## Commands Used (summary)
•	Mode/navigation: enable, configure terminal, end<br>
•	Interface config: interface g0/0 | g0/1 | s0/0/0, ip address A.B.C.D M.M.M.M, description ..., no shutdown<br>
•	DCE timing (if applicable): clock rate 64000<br>
•	Show/verify: show interfaces, show interfaces serial 0/0/0, show interfaces g0/0, show ip interface brief, show ip route<br>
•	Save: copy running-config startup-config / wr <br>
<br>
## Output (Attach Screenshots)
### •	show ip interface brief on R1 and R2 (after configuration)<br>

#### R1 - Interface Brief

<img width="877" height="889" alt="Screenshot 2025-09-26 085156" src="https://github.com/user-attachments/assets/4f9d104f-560f-4773-bdf5-6043f6ce4d90" />

#### R2 - Interface Brief

<img width="877" height="889" alt="Screenshot 2025-09-26 085335" src="https://github.com/user-attachments/assets/46dbd980-4eb6-4e8d-b32f-0eeb536fa3f3" />

### •	show ip route on R1 and R2<br>

#### R1 - IP Route

<img width="877" height="889" alt="Screenshot 2025-09-26 085441" src="https://github.com/user-attachments/assets/736eca46-5120-4e1d-9f81-35064230f969" />

#### R2 - IP Route

<img width="877" height="889" alt="Screenshot 2025-09-26 085504" src="https://github.com/user-attachments/assets/6a417fd9-1556-46e1-9dc2-659f570796d7" />


### •	Successful ping PC2 → PC4 and R2 → PC2<br>
#### PC2 → PC4(Ping)
<img width="877" height="889" alt="Screenshot 2025-09-26 085603" src="https://github.com/user-attachments/assets/2ca75c27-a6cb-400f-961e-84ae6e003006" />

#### R2 → PC2(Ping)
<img width="877" height="889" alt="Screenshot 2025-09-26 085726" src="https://github.com/user-attachments/assets/7243c291-20bb-4881-ba3d-5efd55a0a278" />

### •	Interface up messages after no shutdown on each link <br>
#### R1 - Interface Up Status

<img width="877" height="483" alt="Screenshot 2025-09-26 085917" src="https://github.com/user-attachments/assets/22586493-4069-454b-bd44-410c048259f6" />

#### R2 - Interface Up Status

<img width="877" height="509" alt="Screenshot 2025-09-26 090021" src="https://github.com/user-attachments/assets/1c5d96d0-d6a8-40c3-8602-a6b8cb586e29" />



<br>

## Result
R1 and R2 were configured with correct IPv4 addresses and interface descriptions, links were brought up, routing tables showed connected networks, and end-to-end connectivity between PCs across the WAN link was verified using pings. The configurations were saved to NVRAM for persistence.<br>
