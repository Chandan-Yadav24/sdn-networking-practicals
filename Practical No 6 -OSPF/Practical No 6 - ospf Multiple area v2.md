**3. OSPFv2 Route Summarization and Filtering**



**Step 1: Follow the same Topology as the Multi - Area OSPFv2.** 

 

**Step 2: Add more loopbacks to Router 3 and configure the OSPF** 

**accordingly.** 

**R3:**

en 

conf t 

&nbsp;

int loopback0 

ip add 172.17.0.1 255.255.255.255 

no shut 

exit 

&nbsp;

int loopback1 

ip add 172.17.1.1 255.255.255.255 

no shut 

exit 

&nbsp;

int loopback2 

&nbsp;ip add 172.17.2.1 255.255.255.255 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int loopback3 

&nbsp;ip add 172.17.3.1 255.255.255.255 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

int loopback4 

&nbsp;ip add 172.17.4.1 255.255.255.255 

&nbsp;no shut 

&nbsp;exit 

&nbsp;

end 

wr 

&nbsp;

&nbsp;

router ospf 1 

&nbsp;network 172.17.0.1 0.0.0.0 area 1 

&nbsp;network 172.17.1.1 0.0.0.0 area 1 

&nbsp;network 172.17.2.1 0.0.0.0 area 1 

&nbsp;network 172.17.3.1 0.0.0.0 area 1 

&nbsp;network 172.17.4.1 0.0.0.0 area 1 

&nbsp;

**Step 3: Enter 'show ip route' on R2 and you will see all the loopback of R3.** 

**Because till now we haven't performed any summarization on R1.** 

R2#show ip route 

&nbsp;

**Step 4: So now we will perform summarization on** R1 

router ospf 1 

area 1 range 172.17.0.0 255.255.252.0 

end 

show ip route 

&nbsp;

**Step 5: Once again we will go to R2 and enter the command 'show ip route'. Now we have done** 

**summarization on R1 so we will see only 2 loopbacks of R3.** 

&nbsp;

R2#show ip route 

&nbsp;

That's how we do summarization. 

&nbsp;

**Step 6: And now you can ping any loopback of R3 from any router.** 

**Just to confirm I have pinged the loopback of R3 via R4.** 

R4#ping 172.17.3.1 

R4#ping 172.17.4.1 

R4#ping 172.17.0.1 

&nbsp;

I have pinged the loopback of R3 via R1. 

R1# ping 172.17.0.1 

R1# ping 172.17.1.1 

R1# ping 172.17.2.1 

R1# ping 172.17.3.1 

R1# ping 172.17.4.1 

&nbsp;

I have pinged the loopback of R3 via R2. 

R2# ping 172.17.0.1 

R2# ping 172.17.1.1 

R2# ping 172.17.2.1 

R2# ping 172.17.3.1 

R2# ping 172.17.4.1

