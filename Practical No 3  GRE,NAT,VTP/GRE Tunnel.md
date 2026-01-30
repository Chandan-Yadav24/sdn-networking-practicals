# GRE Tunnel



**R1**:

conf t 

int 

interface fastEthernet 0/0 

ip address 1.1.1.1 255.0.0.0 

no shut 

exit 

int 

interface loopback 0 

ip address 10.1.1.1 255.255.255.255 

no shut 

end



**R2:** 

conf t 

hostname ISP 

interface fastEthernet 0/0 

ip address 1.1.1.2 255.0.0.0 

no shut 

exit 

interface fastEthernet 0/1 

ip address 2.2.2.1 255.0.0.0 

no shut 

exit 

interface loopback 0 

ip address 20.1.1.1 255.255.255.255 

no shut 

end 

&nbsp;

**R3:** 

conf t 

int 

interface fa 

interface fastEthernet 0/1 

ip address 2.2.2.2 255.0.0.0 

no shut 

exit 

interface loopback 0 

ip address 30.1.1.1 255.255.255.255 

no shut 

end





**Step 3:** Check the connection between R1, ISP, R3. 

ISP#ping 1.1.1.1 

&nbsp;

**Step 4: Create the GRE Tunnel.** 

**R1:** 

conf t 

interface tunnel 1 

tunnel source fastEthernet 0/0 

tunnel destination 2.2.2.2 

ip address 192.168.13.1 255.255.255.0 

no shut 

end 

sh ip int bri





**R3:** 

conf t 

interface tunnel 1 

tunnel source fastEthernet 0/1 

tunnel destination 1.1.1.1 

ip address 192.168.13.2 255.255.255.0 

no shut 

end 

sh ip int bri 

&nbsp;

**Step 5: Assign a Static Route for R1 \& R3.** 

&nbsp;

**R1:** 

conf t 

ip route 2.0.0.0 255.0.0.0 1.1.1.2 

end 

&nbsp;

**R3:** 

conf t 

ip route 1.0.0.0 255.0.0.0 2.2.2.1 

end 

&nbsp;

**Step 6: Check whether the tunnel works.** 

R1#ping 192.168.13.2 

&nbsp;

**Step 7: Configure EIGRP for R1 \& R3.** 

**R1:** 

configure terminal 

router eigrp 1 

network 10.0.0.0 

network 192.168.13.0 

no auto-summary 

end 

&nbsp;

**R3:** 

conf t

router eigrp 1 

network 30.0.0.0 

network 192.168.13.0 

no auto-summary 

end 

&nbsp;

**Step 8: Check whether EIGRP is configured for R1 \& R3.** 

**• R1:** 

R1#sh ip route 

&nbsp;

**R3:** 

R3#sh ip route 

&nbsp;

**Step 9: To set MTU as the GRE Head.** 

**R1:** 

conf t

interface tunnel 1 

ip mtu 1300 

ip tcp adjust-mss 1360 

end





