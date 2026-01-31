Implement Multi-Area OSPFv2



**Step 1: Take 4 router and make a network as below.** 

&nbsp;

Step 2: Configure all the network as below: 

**R1:** 

en 

conf t 

int f0/0 

ip add 192.168.12.1 255.255.255.0 

no shut 

exit 

int f0/1 

ip add 192.168.13.1 255.255.255.0 

no shut 

exit 

&nbsp;

end 

wr 

&nbsp;

&nbsp;

**R2** 

en 

conf t 

&nbsp;

int f0/0 

ip add 192.168.12.2 255.255.255.0 

no shut 

exit 

&nbsp;

int f0/1 

ip add 192.168.24.2 255.255.255.0 

no shut 

exit 

&nbsp;

end 

wr 

&nbsp;

&nbsp;

**R3** 

en 

conf t 

&nbsp;

int f0/1 

ip add 192.168.13.3 255.255.255.0 

no shut 

exit 

&nbsp;

int loopback0 

ip add 3.3.3.3 255.255.255.255 

no shut 

exit 

&nbsp;

end 

wr 

&nbsp;

&nbsp;

**R4** 

en 

conf t 

&nbsp;

int f0/1 

&nbsp;ip add 192.168.24.4 255.255.255.0 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int loopback0 

&nbsp;ip add 4.4.4.4 255.255.255.255 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

end 

wr 

&nbsp;

**Step 3: Now try to ping any router. It won't work because there is no** 

**Protocol applied.** 

**So now we will apply Multi - Area OSPFv2(Area 0, 1, 2).** 

**Configure the system for Multi - Area OSPFv2 as below:** 

**R1 – OSPF Config** 

en 

conf t 

router ospf 1 

network 192.168.12.0 0.0.0.255 area 0 

network 192.168.13.0 0.0.0.255 area 1 

exit 

end 

wr 

&nbsp;

&nbsp;

**R2 – OSPF Config** 

en 

conf t 

router ospf 1 

&nbsp;network 192.168.12.0 0.0.0.255 area 0 

&nbsp;network 192.168.24.0 0.0.0.255 area 2 

exit 

end 

wr 

&nbsp;

&nbsp;

**R3 – OSPF Config** 

en 

conf t 

router ospf 1 

network 192.168.13.0 0.0.0.255 area 1 

network 3.3.3.3 0.0.0.0 area 1 

exit 

end 

wr 

&nbsp;

&nbsp;

**R4 – OSPF Config** 

en 

conf t 

router ospf 1 

network 192.168.24.0 0.0.0.255 area 2 

network 4.4.4.4 0.0.0.0 area 2 

exit 

end 

wr 

&nbsp;

**Step 4: Enter the command 'show ip route ospf' to check whether OSPF is** 

**successfully configured.** 

R1#show ip route ospf 

&nbsp;

R2#show ip route ospf 

&nbsp;

R3#show ip route ospf 

&nbsp;

R4#show ip route ospf 

&nbsp;

&nbsp;

**Step 5: To check the neighbor enter 'show ip ospf neighbor' and check the** 

**neighbor:** 

R1#show ip ospf neighbor 

&nbsp;

R2#show ip ospf neighbor 

&nbsp;

R3#show ip ospf neighbor 

&nbsp;

R4#show ip ospf neighbor 

&nbsp;

&nbsp;

**As now we have successfully configured and checked that OSPF multi-Area** 

**is there in our network. Try pinging any router or loopback from any** 

**router.** 

**Step 6:** 

R1: 

tclsh 

foreach address {192.168.13.3 192.168.24.4 3.3.3.3 4.4.4.4} { 

&nbsp;ping $address 

} 

Tclquit 

&nbsp;

R2: 

tclsh 

foreach address {192.168.13.3 3.3.3.3 4.4.4.4} { 

&nbsp;ping $address 

} 

tclquit 

&nbsp;

R3: 

&nbsp;

R4:

