Practical No 6



**Implement Single-Area OSPFv2**

**Step 1:** To create a network take 3 routers and 3 PC's

 

**Step 2: Configure PC:**

**PC1**:

PC1> ip 192.168.1.1 255.255.255.0 gateway 192.168.1.50

PC1> sh ip

 

**PC2**:

PC2> ip 192.168.2.1 255.255.255.0 gateway 192.168.2.50

PC2> sh ip

 

**PC3**:

PC3> ip 192.168.3.1 255.255.255.0 gateway 192.168.3.50

PC3> sh ip

 

**Step 3: Configure IP Address in Router:**

**R1:**

enable

configure terminal

int  f0/0

ip address 192.168.1.50 255.255.255.0

no shutdown

exit

int  s1/0

ip address 10.1.1.1 255.255.255.0

keepalive 10

encapsulation hdlc

no shutdown

exit

end

write memory

 

no keepalive will make IOS stop waiting for L2 keepalive packets, so Protocol may stay up.

 

 

 

**R2**

en

conf t

int f0/0

ip add 192.168.2.50 255.255.255.0

no shut

exit

int s1/0

ip add 10.1.1.2 255.255.255.0

no shut

exit

int s1/1

ip add 11.1.1.1 255.255.255.0

no keepalive

clock rate 128000

keepalive 10

encapsulation hdlc

no shut

exit

end

wr

 

**R3**

en

conf t

int f0/0

ip add 192.168.3.50 255.255.255.0

no shut

exit

int s1/0

ip add 11.1.1.2 255.255.255.0

no shut

exit

end

wr

 

**Step 4: Check whether the IP Address assigned is correct or not by using**

'do sh ip int br'

**R1**:

R1(config)#do sh ip int br

 

**R2**:

R2(config)#do sh ip int br

 

**R3**:

R3(config)#do sh ip int br

 

 

**Step 5: Check whether direct connection ping is working in all the routers**

**and PCs:**

**PC1**:

PC1> ping 192.168.1.50

 

**PC2**:

PC2> ping 192.168.2.50

 

PC3:

PC3> ping 192.168.3.50

 

**R1**:

R1(config)#do ping 10.1.1.2

 

R2:

R2(config)#do ping 10.1.1.1

P2(config)#exit

 

R3:

R3(config)#do ping 11.1.1.2

 

**Direct Connection ping is working successfully. But indirect won't work**

**because we haven't done any protocol.**

**So, we will do OSPF in single area.**

**Step 6: Configure OSPF protocol in all the routers.**

**R1**:

router ospf 1

network 192.168.1.0 0.0.0.255 area 0

network 10.1.1.0 0.0.0.255 area 0

 

**R2**:

R2 OSPF Config

en

conf t

router ospf 1

 network 192.168.2.0 0.0.0.255 area 0

 network 10.1.1.0 0.0.0.255 area 0

 network 11.1.1.0 0.0.0.255 area 0

exit

end

wr

 

**R3 OSPF Config**

en

conf t

router ospf 1

 network 192.168.3.0 0.0.0.255 area 0

 network 11.1.1.0 0.0.0.255 area 0

exit

end

wr

 

**Step 7: Once OSPF is done enter command 'sh ip route' in all router to**

**check whether OSPF is done properly.**

R1:

R1#sh ip route

 

R2:

R2#sh ip route

 

R3:

R3#sh ip route

 

**Step 8: Enter command 'sh ip protocols' to check which all protocols are**

**applied in our network:**

R1:

R1#sh ip protocols

 

R2:

R2#sh ip protocols

 

R3:

R3#sh ip protocols

 

Step 9: Enter command 'sh ip ospf neigbor' to check OSPF Neigbor:

R1:

R1#sh ip ospf neighbor

 

R2:

R2#sh ip ospf neighbor

 

R3:

R3#sh ip ospf neighbor

 

**Step 10: Now you can ping any indirect connection because we have**

**doneOSPF on the router**

PC1:

PC1> ping 192.168.2.1

 

PC2> ping 192.168.1.1

 

PC3> ping 192.168.1.1

