Implement eBGP for Ipv4. 



**Part 1: Build the Network and Configure Basic Device Settings and Interface Addressing** 

**Step 1:Design the Topology** 

                         **R2**

        **Loopback0: 192.168.2.1**

        **Loopback1: 192.168.2.65**

                 **f0/0 10.1.2.2**

                    **|**

                    **|  (Ethernet link)**

                    **|**

**f0/0 10.1.2.1     R1 +-----------------------------+ R3     f0/0 10.2.3.3**

                 **/  |                             |  \\**

                **/   |                             |   \\**

               **/    |                             |    \\**

      **(Serial link) |                             | (Serial link)**

     **s1/0 10.1.3.1  |                             |  s1/0 10.1.3.3**

                    **|                             |**

      **(Serial link) |                             | (Serial link)**

   **s1/1 10.1.3.129  |                             |  s1/1 10.1.3.130**

                    **|                             |**

                    **+---------- (Ethernet link) ---+**

                               **R2 f0/1 10.2.3.2**





**R1 Loopbacks:**

  **Loopback0: 192.168.1.1**

  **Loopback1: 192.168.1.65**



**R3 Loopbacks:**

  **Loopback0: 192.168.3.1**

  **Loopback1: 192.168.3.65**

**Step 2: Configure all 3 Routers.** 

**• Router R1** 

no ip domain lookup 

line console 0 

logging synchronous 

exec-timeout 0 0 

exit 

interface Loopback0 

ip address 192.168.1.1 255.255.255.224 

no shutdown 

exit 

interface Loopback1 

ip address 192.168.1.65 255.255.255.192 

no shutdown 

exit 

interface FastEthernet0/0 

ip address 10.1.2.1 255.255.255.0 

speed auto 

duplex auto 

no shutdown 

exit 

interface Serial1/0 

ip address 10.1.3.1 255.255.255.128 

clock rate 64000 

encapsulation ppp 

no shutdown 

exit 

interface Serial1/1 

ip address 10.1.3.129 255.255.255.128 

clock rate 64000 

encapsulation ppp 

no shutdown 

exit 



**Router 2:** 

no ip domain lookup 

line console 0 

logging synchronous 

exec-timeout 0 0 

exit 

interface Loopback0 

ip address 192.168.2.1 255.255.255.224 

no shutdown 

exit 

interface Loopback1 

ip address 192.168.2.65 255.255.255.192 

no shutdown 

exit 

interface FastEthernet0/0 

ip address 10.1.2.2 255.255.255.0 

speed auto 

duplex auto 

no shutdown 

exit 

interface FastEthernet0/1 

ip address 10.2.3.2 255.255.255.0 

speed auto 

duplex auto 

no shutdown 

exit 

&nbsp;

&nbsp;

&nbsp;

**Router 3:** 

&nbsp;

conf t 

no ip domain lookup 

&nbsp;

line console 0 

logging synchronous 

exec-timeout 0 0 

exit 

&nbsp;

interface Loopback0 

ip address 192.168.3.1 255.255.255.224 

no shutdown 

exit 

interface Loopback1 

ip address 192.168.3.65 255.255.255.192 

no shutdown 

exit 

interface FastEthernet0/0 

ip address 10.2.3.3 255.255.255.0 

speed auto 

duplex auto 

no shutdown 

exit 

interface Serial1/0 

ip address 10.1.3.3 255.255.255.128 

clock rate 64000 

encapsulation ppp 

no shutdown 

exit 

interface Serial1/1 

ip address 10.1.3.131 255.255.255.128 

clock rate 64000 

encapsulation ppp 

no shutdown 

exit 

&nbsp;

&nbsp;

&nbsp;

**Part 2: Configure and Verify eBGP for IPv4 on all Routers** 

**Step 1: Implement BGP and neighbor relationships on R1.** 

&nbsp;

**Router 1:** 

router bgp 1000 

bgp router-id 1.1.1.1 

neighbor 10.1.2.2 remote-as 500 

neighbor 10.1.3.3 remote-as 300 

network 192.168.1.0 mask 255.255.255.224 

network 192.168.1.64 mask 255.255.255.192 

&nbsp;

**Step 2: Implement BGP and neighbor relationships on R2.** 

**R2:** 

router bgp 500 

bgp router-id 2.2.2.2 

neighbor 10.1.2.1 remote-as 1000 

neighbor 10.2.3.3 remote-as 300 

network 192.168.2.0 mask 255.255.255.224 

network 192.168.2.64 mask 255.255.255.192 



**Step 3: Implement BGP and neighbor relationships on R3** 

**R3:** 

router bgp 300 

bgp router-id 3.3.3.3 

no bgp default ipv4-unicast 

neighbor 10.2.3.2 remote-as 500 

neighbor 10.1.3.1 remote-as 1000 

neighbor 10.1.3.129 remote-as 1000 

Step 4: Verifying BGP neighbor relationships. 

R1#show ip route bgp 

R2#show ip route bgp 

R2#show ip bgp neighbors 

&nbsp;

&nbsp;

&nbsp;

**The interfaces on R3 need to be activated in IPv4 AF configuration mode** 

**Router 3:** 

router bgp 300 

bgp router-id 3.3.3.3 

no bgp default ipv4-unicast 

&nbsp;

neighbor 10.2.3.2 remote-as 500 

neighbor 10.1.3.1 remote-as 1000 

neighbor 10.1.3.129 remote-as 1000 

address-family ipv4 

neighbor 10.1.3.1 activate 

neighbor 10.1.3.129 activate 

neighbor 10.2.3.2 activate 

network 192.168.3.0 mask 255.255.255.224 

network 192.168.3.64 mask 255.255.255.192 

exit-address-family 

Verify that the BGP state between R2 and R3 has now been established 

R2#show ip bgp neighbors | begin BGP neighbor is 10.2.3.3 

Step 5: Examining the running-configs. 

R1#show running-config | section bgp 

&nbsp;

&nbsp;

&nbsp;

&nbsp;

**Step 6: Verifying BGP operations.** 

&nbsp;

&nbsp;

sh ip bgp 192.168.1.0 

&nbsp;

&nbsp;

show ip bgp neighbors 



