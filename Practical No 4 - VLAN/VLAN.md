# VLAN



**1) Physical Connection / Port Mapping Table**

\[PC1 e0]  ----  \[L2 Switch1 (IOU1) e0/1]   (ACCESS VLAN 5)



\[PC2 e0]  ----  \[L2 Switch1 (IOU1) e0/2]   (ACCESS VLAN 10)



\[L2 Switch1 (IOU1) e0/3]  ----  \[L2 Switch2 (IOU2) e0/0]   (TRUNK dot1q, VLAN 5 \& 10)



\[L2 Switch1 (IOU1) e0/0]  ----  \[Router R1 Gi0/0]          (TRUNK dot1q, VLAN 5 \& 10)



\[PC3 e0]  ----  \[L2 Switch2 (IOU2) e0/1]   (ACCESS VLAN 5)



**2) VLAN Table (Logical Segmentation)**

**VLAN 5   = IT**

**VLAN 10  = SALES**



**3) End Device IP Addressing Table**

**+------+-----------+------------------+----------------+**

**| Host | VLAN      | IP Address       | Default GW     |**

**+------+-----------+------------------+----------------+**

**| PC1  | VLAN 5    | 192.168.5.5/24   | 192.168.5.1    |**

**| PC2  | VLAN 10   | 192.168.10.10/24 | 192.168.10.1   |**

**| PC3  | VLAN 5    | 192.168.5.10/24  | 192.168.5.1    |**

**+------+-----------+------------------+----------------+**



**Configuration :** 

**PC1:** ip 192.168.5.5/24 192.168.5.1 

Sh ip



**PC2:** 

PC2> ip 192.168.10.10/24 192.168.10.1 

PC2> sh ip



**PC3:** 

PC3> ip 192.168.5.10/24 192.168.5.1 

PC3> sh ip





**Layer2 Switch 1:** 

conf t 

vlan 5 

name IT 

exit 

vlan 10 

name SALES 

exit 

end





**Layer 2 Switch 2:** 

conf t 

vlan 5 

&nbsp;name IT 

exit 

end 

write memory



&nbsp;

**Configuring the trunk and access interface for L2 Switch 1:** 



conf t 

interface ethernet0/1 

&nbsp;switchport mode access 

&nbsp;switchport access vlan 5 

exit 

interface ethernet0/2 

&nbsp;switchport mode access 

&nbsp;switchport access vlan 10 

exit 

interface ethernet0/3 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

exit 

interface ethernet0/0 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

end 

write memory



**Configuring the trunk and access interface for L2 Switch 2:** 



conf t 

interface ethernet0/1 

&nbsp;switchport mode access 

&nbsp;switchport access vlan 5 

exit 

interface ethernet0/0 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

exit 

end 

write memory





**Configuring Router R1:** 



conf t 

interface Gi 0/0 

&nbsp;no shutdown 

exit 

interface Gi 0/0.5 

&nbsp;encapsulation dot1q 5 

&nbsp;ip address 192.168.5.1 255.255.255.0 

&nbsp;no shutdown 

exit 

interface Gi 0/0.10 

&nbsp;encapsulation dot1q 10 

&nbsp;ip address 192.168.10.1 255.255.255.0 

&nbsp;no shutdown 

exit 

end 

write memory





**Testing the Network:** 

1\) Ping PC1 to V2 member PC2 to test the connection. 

PC1> ping 192.168.5.1 

PC1> ping 192.168.10.1 

PC1> ping 192.168.10.10 

PC1> ping 192.168.5.10 



**2) Ping from PC2 to PCs on VLAN 5:** 

Ping 192.168.10.1 

Ping 192.168.5.5 

Ping 192.168.5.10 



**3) Similarly, ping from PC3 to the other PCs on VLAN10:** 

PC3> ping 192.168.5.1 

PC3> ping 192.168.10.1 

PC3> ping 192.168.10.10 

PC3> ping 192.168.5.10 



**Checking if PC1 has been correctly configured:** 

**PC1:**

sh ip 



**Checking if PC2 has been correctly configured:** 

sh ip 

&nbsp;

**Checking if PC3 has been correctly configured:** 

sh ip 

&nbsp;

**Checking VLAN on Layer2 Switch 1:** 

IOU1#show vlan bri 





**Checking VLAN on Layer2 Switch 2:** 

IOU2#sh vlan bri 

&nbsp;

**Checking the running configuration of L2 Switch 1:** 

IOU1#sh running-config 

&nbsp;

&nbsp;

**Checking the running configuration of L2 Switch 2:** 

IOU2#sh running-config 

&nbsp;

&nbsp;

**❖ The interfaces the have been set to the trunk mode in L2 Switch 1:** 

IOU1#sh int trunk 

&nbsp;

**The interfaces the have been set to the trunk mode in L2 Switch 2:** 

IOU2#sh int trunk 

&nbsp;

**The overall running configuration of Router 1 (R1):** 

R1#sh running-config

