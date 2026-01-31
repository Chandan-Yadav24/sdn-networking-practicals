4\. Implement Multiarea OSPFv3 

Step 1: Build the topology 

&nbsp;

**Step 2: Configure IP's address and Loopback in all the router according to** 

**the topology** 

**We will use IPv6 for OSPF version 3** 

**There's a different command for IPv6 configuration. Follow as below.** 

**R1**: 

**R1 – IPv6 Config** 

en 

conf t 

&nbsp;

int s0/0 

&nbsp;ipv6 address 2001:DB8:ACAD:12::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int lo0 

&nbsp;ipv6 address 2001:DB8:ACAD::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int lo1 

&nbsp;ipv6 address 2001:DB8:ACAD:1::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int lo2 

&nbsp;ipv6 address 2001:DB8:ACAD:2::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int lo3 

&nbsp;ipv6 address 2001:DB8:ACAD:3::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

end 

wr 

&nbsp;

&nbsp;

**R2 – IPv6 Config** 

en 

conf t 

&nbsp;

int s0/0 

&nbsp;ipv6 address 2001:DB8:ACAD:12::2/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int s0/1 

&nbsp;ipv6 address 2001:DB8:ACAD:23::2/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int lo8 

&nbsp;ipv6 address 2001:DB8:ACAD:8::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

end 

wr 

&nbsp;

&nbsp;

**R3 – IPv6 Config** 

en 

conf t 

&nbsp;

int s0/1 

&nbsp;ipv6 address 2001:DB8:ACAD:23::3/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int lo4 

&nbsp;ipv6 address 2001:DB8:ACAD:4::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int lo5 

&nbsp;ipv6 address 2001:DB8:ACAD:5::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int lo6 

&nbsp;ipv6 address 2001:DB8:ACAD:6::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int lo7 

&nbsp;ipv6 address 2001:DB8:ACAD:7::1/64 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

end 

wr 

&nbsp;

**Step 3: Once IP is assigned to all. We have to do IPv6 unicast. And we have** 

**to assign router ID to the routers.** 

**R1:** 

**R1** 

en 

conf t 

ipv6 unicast-routing 

&nbsp;

ipv6 router ospf 1 

&nbsp;router-id 1.1.1.1 

do sh ipv6 ospf 

&nbsp;

exit 

&nbsp;

int s0/0 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

int lo0 

&nbsp;ipv6 ospf 1 area 0 

exit 

int lo1 

&nbsp;ipv6 ospf 1 area 0 

exit 

int lo2 

&nbsp;ipv6 ospf 1 area 0 

exit 

int lo3 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

end 

wr 

&nbsp;

&nbsp;

**R2** 

en 

conf t 

ipv6 unicast-routing 

&nbsp;

ipv6 router ospf 1 

&nbsp;router-id 2.2.2.2 

do sh ipv6 ospf 

&nbsp;

exit 

&nbsp;

int s0/0 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

int s0/1 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

int lo8 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

end 

wr 

&nbsp;

&nbsp;

**R3** 

en 

conf t 

ipv6 unicast-routing 

&nbsp;

ipv6 router ospf 1 

&nbsp;router-id 3.3.3.3 

do sh ipv6 ospf 

&nbsp;

exit 

&nbsp;

int s0/1 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

int lo4 

&nbsp;ipv6 ospf 1 area 0 

exit 

int lo5 

&nbsp;ipv6 ospf 1 area 0 

exit 

int lo6 

&nbsp;ipv6 ospf 1 area 0 

exit 

int lo7 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

end 

wr 

&nbsp;

**Step 4: Now we will configure multi-area OSPFv3 in all the router** 

**R1** 

en 

conf t 

&nbsp;

ipv6 unicast-routing 

ipv6 router ospf 1 

&nbsp;router-id 1.1.1.1 

exit 

&nbsp;

int lo0 

&nbsp;ipv6 ospf 1 area 1 

&nbsp;ipv6 ospf network point-to-point 

exit 

&nbsp;

int lo1 

&nbsp;ipv6 ospf 1 area 1 

&nbsp;ipv6 ospf network point-to-point 

exit 

&nbsp;

int lo2 

&nbsp;ipv6 ospf 1 area 1 

&nbsp;ipv6 ospf network point-to-point 

exit 

&nbsp;

int lo3 

&nbsp;ipv6 ospf 1 area 1 

&nbsp;ipv6 ospf network point-to-point 

exit 

&nbsp;

int s0/0 

&nbsp;ipv6 ospf 1 area 0 

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

ipv6 unicast-routing 

ipv6 router ospf 1 

&nbsp;router-id 2.2.2.2 

exit 

&nbsp;

int s0/0 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

int s0/1 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

int lo8 

&nbsp;ipv6 ospf 1 area 0 

&nbsp;ipv6 ospf network point-to-point 

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

ipv6 unicast-routing 

ipv6 router ospf 1 

&nbsp;router-id 3.3.3.3 

exit 

&nbsp;

int lo4 

&nbsp;ipv6 ospf 1 area 2 

&nbsp;ipv6 ospf network point-to-point 

exit 

&nbsp;

int lo5 

&nbsp;ipv6 ospf 1 area 2 

&nbsp;ipv6 ospf network point-to-point 

exit 

&nbsp;

int lo6 

&nbsp;ipv6 ospf 1 area 2 

&nbsp;ipv6 ospf network point-to-point 

exit 

&nbsp;

int lo7 

&nbsp;ipv6 ospf 1 area 2 

&nbsp;ipv6 ospf network point-to-point 

exit 

&nbsp;

int s0/1 

&nbsp;ipv6 ospf 1 area 0 

exit 

&nbsp;

end 

wr 

&nbsp; 

&nbsp;

**Step 5: Use the show ipv6 protocols command to verify multi-area OSPFv3** 

**status.** 

do sh ipv6 protocols 

&nbsp;

do sh ipv6 protocols 

&nbsp;

do sh ipv6 protocols 

&nbsp;

**Step 6: Use the 'show ipv6 ospf' command to verify configurations.** 

R1#show ipv6 ospf 

&nbsp;

R2#show ipv6 ospf 

&nbsp;

R3#show ipv6 ospf 

&nbsp;

**Step 7: Verify OSPFv3 neighbors and routing information.** 

**R1:** 

R1#sh ipv6 ospf neighbor 

&nbsp;

R2#sh ipv6 ospf neighbor 

&nbsp;

R3#sh ipv6 ospf neighbor 

&nbsp;

**Step 8: Check 'show ipv6 route ospf' to see the OSPF configuration** 

&nbsp;

R1#show ipv6 route ospf 

&nbsp;

R2#show ipv6 route ospf 

&nbsp;

R3#show ipv6 route ospf 

&nbsp;

**Step 9: Issue the 'show ipv6 ospf database' command on all routers to** 

**check the IPv6 OSPF Database** 

&nbsp;

R1#show ipv6 ospf database 

&nbsp;

R2#show ipv6 ospf database 

&nbsp;

R3#show ipv6 ospf database 

&nbsp;

**Reachability Tests** 

**On R1** 

&nbsp;

ping ipv6 2001:DB8:ACAD:12::2     ! R2's Serial0/0 

ping ipv6 2001:DB8:ACAD:23::3     ! R3's Serial0/1 via R2 

ping ipv6 2001:DB8:ACAD:4::1      ! R3 Loopback4 

ping ipv6 2001:DB8:ACAD:5::1      ! R3 Loopback5 

ping ipv6 2001:DB8:ACAD:8::1      ! R2 Loopback8 

&nbsp;

**On R2** 

Neighbor and remote tests: 

ping ipv6 2001:DB8:ACAD:12::1     ! R1’s Serial0/0 

ping ipv6 2001:DB8:ACAD:23::3     ! R3’s Serial0/1 

ping ipv6 2001:DB8:ACAD::1        ! R1 Loopback0 

ping ipv6 2001:DB8:ACAD:3::1      ! R1 Loopback3 

ping ipv6 2001:DB8:ACAD:7::1      ! R3 Loopback7 

 

**On R3** 

Neighbor and remote loopback tests: 

ping ipv6 2001:DB8:ACAD:23::2     ! R2 Serial0/1 

ping ipv6 2001:DB8:ACAD:12::1     ! R1 Serial0/0 

ping ipv6 2001:DB8:ACAD::1        ! R1 Loopback0 

ping ipv6 2001:DB8:ACAD:1::1      ! R1 Loopback1 

ping ipv6 2001:DB8:ACAD:8::1      ! R2 Loopback8 

&nbsp;

Now you have successfully configured multi-area OSPF v3 using IPv6 

