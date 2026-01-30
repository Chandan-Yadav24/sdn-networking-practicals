# NAT



**Step 1: Topology**



**Step 2: Configure the network.**

**R1(gateway):**

configure terminal

hostname gateway

 

interface GigabitEthernet 1/0

ip address 200.2.2.18 255.255.255.252

no shutdown

 

interface fastEthernet 0/0

ip address 10.10.10.1 255.255.255.0

no shutdown

 

end

 

**R2(ISP):**

configure terminal

hostname ISP

 

interface loopback0

ip address 172.16.1.1 255.255.255.255

no shutdown

 

interface GigabitEthernet1/0

ip address 200.2.2.17 255.255.255.252

no shutdown

 

end

 

**PC1:**

PC1> ip 10.10.10.2 255.255.255.0 10.10.10.1

PC1> sh ip

 

PC2:

PC2> ip 10.10.10.3 255.255.255.0 10.10.10.1

PC2> sh ip

 

**Step 3: Create a Static Route.**

**R2:**

conf t

ip route 199.99.9.32 255.255.255.224 200.2.2.18

end

 

**Step 4: Create a Default Route.**

**R1:**

conf t

ip route 0.0.0.0 0.0.0.0 200.2.2.17

 

**Step 5: Make a pool of IP Address which can be used as Public IP Address:**

**Run the command: ip nat pool public-access 199.99.9.32 199.99.9.35 netmask**

**255.255.255.252**

**R1:**

configure terminal

ip nat pool public-access 199.99.9.32 199.99.9.46 netmask 255.255.255.240

end

 



**Step 6: Make an access list that will map the public IP addresses to the inside private IP**

**addresses and define the NAT translation from inside list to outside pool.**

 

**R1(gateway)**

conf t

access-list 1 permit 10.10.10.0 0.0.0.255

 

**Step 7: Now we will define which interface is inside and which one is outside.**

**Gateway:**

**conf t**

ip nat inside source list 1 pool public-access overload

 

interface fastEthernet 0/0

ip nat inside

 

interface g 1/0

ip nat outside

 

end

 

**Step 8: Now ping from the PC to the loopback of ISP.**

**PC1:**

PC1> ping 172.16.1.1

 

**PC2:**

PC2> ping 172.16.1.1

 

**Step 9: Verify NAT \& PAT Translations.**

**R1:**

\#show run | include nat

 

gateway#sh ip nat translations

 

**Step 10: Verify NAT \& PAT Statistics.**

gateway#sh ip nat statistics

