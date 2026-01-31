# **IPSec Site-to-Site VPNs** 



**Step 1: Design the network topology.** 

&nbsp;

**Step 2: Configure the network.** 



**Router 1 (R1):** 

conf t 

no ip domain-lookup 

line console 0 

&nbsp;logging synchronous 

&nbsp;exec-timeout 0 0 

&nbsp;exit 

interface FastEthernet0/0 

&nbsp;description Connection to R2 

&nbsp;ip address 64.100.0.2 255.255.255.252 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface FastEthernet0/1 

&nbsp;description Connection to D1 

&nbsp;ip address 10.10.0.1 255.255.255.252 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

&nbsp;

router ospf 123 

&nbsp;router-id 1.1.1.1 

&nbsp;auto-cost reference-bandwidth 1000 

&nbsp;network 10.10.0.0 0.0.0.3 area 0 

&nbsp;default-information originate 

&nbsp;exit 

ip route 0.0.0.0 0.0.0.0 64.100.0.1 

end 

wr mem 

&nbsp;

**Router 2 (R2):** 

conf t 

no ip domain-lookup 

line console 0 

&nbsp;logging synchronous 

&nbsp;exec-timeout 0 0 

&nbsp;exit 

! This is R2 - Implement GRE over IPSec Site-to-Site VPN 

interface FastEthernet0/0 

&nbsp;description Connection to R1 

&nbsp;ip address 64.100.0.1 255.255.255.252 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface FastEthernet0/1 

&nbsp;description Connection to R3 

&nbsp;ip address 64.100.1.1 255.255.255.252 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback0 

&nbsp;description Internet simulated address 

&nbsp;ip address 209.165.200.225 255.255.255.224 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

ip route 0.0.0.0 0.0.0.0 Loopback0 

ip route 10.10.0.0 255.255.252.0 64.100.0.2 

ip route 10.10.4.0 255.255.252.0 64.100.1.2 

ip route 10.10.16.0 255.255.248.0 64.100.1.2 

end 

wr mem 

&nbsp;

**Router 3 (R3):**  

conf t 

no ip domain-lookup 

line console 0 

&nbsp;logging synchronous 

&nbsp;exec-timeout 0 0 

&nbsp;exit 

! This is R3 - Implement GRE over IPSec Site-to-Site VPN 

interface FastEthernet0/0 

&nbsp;description Connection to R2 

&nbsp;ip address 64.100.1.2 255.255.255.252 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface FastEthernet0/1 

&nbsp;description Connection to D2 

&nbsp;ip address 10.10.4.1 255.255.255.252 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

ip route 0.0.0.0 0.0.0.0 64.100.1.1 

router ospf 123 

&nbsp;router-id 3.3.3.1 

&nbsp;auto-cost reference-bandwidth 1000 

&nbsp;network 10.10.4.0 0.0.0.3 area 0 

&nbsp;default-information originate 

&nbsp;exit 

end 

wr mem 

&nbsp;

**• Router 4 (D1):** 

conf t 

no ip domain-lookup 

line console 0 

&nbsp;logging synchronous 

&nbsp;exec-timeout 0 0 

&nbsp;exit 

! This is D1 - Implement GRE over IPSec Site-to-Site VPN 

interface FastEthernet0/0 

&nbsp;description Connection to R1 

&nbsp;ip address 10.10.0.2 255.255.255.252 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface FastEthernet0/1 

&nbsp;description Connection to PC1 

&nbsp;ip address 10.10.1.1 255.255.255.0 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback2 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.2.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

&nbsp;

interface Loopback3 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.3.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

router ospf 123 

&nbsp;router-id 1.1.1.2 

&nbsp;auto-cost reference-bandwidth 1000 

&nbsp;network 10.10.0.0 0.0.3.255 area 0 

&nbsp;exit 

end 

wr mem 

&nbsp;

**• Router 5 (D2):** 

conf t 

no ip domain-lookup 

line console 0 

&nbsp;logging synchronous 

&nbsp;exec-timeout 0 0 

&nbsp;exit 

! This is D2 - Implement GRE over IPSec Site-to-Site VPN 

interface FastEthernet0/0 

&nbsp;description Connection to R3 

&nbsp;ip address 10.10.4.2 255.255.255.252 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface FastEthernet0/1 

&nbsp;description Connection to PC2 

&nbsp;ip address 10.10.5.1 255.255.255.0 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback16 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.16.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback17 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.17.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback18 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.18.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback19 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.19.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback20 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.20.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback21 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.21.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback22 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.22.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

interface Loopback23 

&nbsp;description Loopback to simulate an OSPF network 

&nbsp;ip address 10.10.23.1 255.255.255.0 

&nbsp;ip ospf network point-to-point 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

router ospf 123 

&nbsp;router-id 3.3.3.2 

&nbsp;auto-cost reference-bandwidth 1000 

&nbsp;network 10.10.4.0 0.0.1.255 area 0 

&nbsp;network 10.10.16.0 0.0.7.255 area 0 

&nbsp;exit 

end 

wr mem 

&nbsp;

&nbsp;

**PC 1:** 

&nbsp;

PC1> ip 10.10.1.10/24 10.10.1.1 

Checking for duplicate address... 

PC1: 10.10.1.10 255.255.255.0 gateway 10.10.1.1 

PC1> sh ip 

&nbsp;

PC2: 

PC2> ip 10.10.5.10/24 10.10.5.1 

PC1: 10.10.5.10 255.255.255.0 gateway 10.10.5.1 

PC2> sh ip 

&nbsp;

**Step 3: On PC1, verify end-to-end connectivity.** 

**From PC1, ping the first loopback on D3 (10.10.16.1).** 

&nbsp;

PC1> ping 10.10.16.1 

&nbsp;

Finally, from PC1, ping the default gateway loopback on R2 (209.165.200.225). 

&nbsp;

PC1> ping 209.165.200.225 

&nbsp;

**Step 4: Verify the routing table of R1 and R3.** 

**Verify the OSPF routing table of R1.** 

R1#sh ip route ospf 

&nbsp;

Verify the routing table of R3. 

R3#sh ip route ospf 

&nbsp;

**Step 5: Configure GRE over IPsec using a Crypto Map on R1.** 

**• On R1, configure the ISAKMP policy and pre-shared key.** 

**Like site-to-site VPNs using crypto maps, GRE over IPsec also requires an ISAKMP policy** 

**configuration and pre-shared key configured.** 

**In this lab, we will use the following parameters for the ISAKMP policy 10 on R1:** 

**o Encryption: aes 256** 

**o Hash: sha256** 

**o Authentication method: pre-share key** 

**o Diffie-Hellman group: 14** 

**o Lifetime: 3600 seconds (60 minutes / 1 hour)** 

**Configure ISAKMP policy 10 on R1:** 

&nbsp;

conf t 

! Configure ISAKMP (Phase 1) Policy for VPN 

crypto isakmp policy 10 

&nbsp;encryption aes 256 

&nbsp;hash sha 

&nbsp;authentication pre-share 

&nbsp;group 1 

&nbsp;lifetime 36000 

&nbsp;exit 

end 

wr mem 

&nbsp;

Configure the pre-shared key of cisco123 on R1. This command points to the remote peer R3 G0/0/0 

IP address. 

&nbsp;

R1(config)# 

R1(config)#crypto isakmp key cisco123 address 64.100.1.2 

R1(config)# 

 

**On R1, configure the transform set and VPN ACL.** 

**Create a transform set called GRE-VPN using AES 256 cipher with ESP and the SHA 256 hash** 

**function.** 

&nbsp;

R1(config)#crypto ipsec transform-set GRE-VPN esp-aes 256 esp-sha-hmac 

R1(cfg-crypto-trans)# 

&nbsp;

**Unlike a site-to-site IPsec VPN, the transform must use transport mode. The mode command is used** 

**to identify the type of tunnel that will be established. The default is mode tunnel mode. However,** 

**GRE over IPsec should be configured using the mode transport command.** 

R1(cfg-crypto-trans)#mode transport 

R1(cfg-crypto-trans)#exit 

R1(config)# 

&nbsp;

**Next, create a named extended ACL called GRE-VPN-ACL that makes the tunnel interface traffic** 

**interesting.** 

conf t 

ip access-list extended GRE-VPN-ACL 

&nbsp;permit gre host 64.100.0.2 host 64.100.1.2 

&nbsp;exit 

end 

wr mem 

&nbsp;

**On R1, configure the crypto map and apply it to the interface.** 

**Create a crypto map called GRE-CMAP that associates the new GRE-VPN-ACL, transform set, and** 

**peer.** 

&nbsp;

conf t 

crypto map GRE-CMAP 10 ipsec-isakmp 

&nbsp;match address GRE-VPN-ACL 

&nbsp;set transform-set GRE-VPN 

&nbsp;set peer 64.100.1.2 

&nbsp;exit 

end 

wr mem 

&nbsp;

**Finally, assign a crypto map called GRE-MAP on G0/0/0** 

conf t 

! Apply the crypto map to the outbound interface 

interface FastEthernet0/0 

&nbsp;crypto map GRE-CMAP 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

end 

wr mem 

&nbsp;

**On R1, configure the GRE tunnel interface.** 

**Configure a GRE tunnel interface as shown. To enable GRE on the tunnel interface, the tunnel** 

**gre ipv4 command is required. However, this command is enabled by default and will therefore** 

**be configured in our example.** 

conf t 

! Configure GRE Tunnel interface 

interface Tunnel1 

&nbsp;bandwidth 4000 

&nbsp;ip address 172.16.1.1 255.255.255.252 

&nbsp;ip mtu 1400 

&nbsp;tunnel source 64.100.0.2 

&nbsp;tunnel destination 64.100.1.2 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

end 

wr mem 

&nbsp;

**Step 6: Configure GRE over IPsec using a Tunnel IPsec Profile on R3.** 

**In this part, we will configure GRE over IPsec using tunnel IPsec profiles on R3.** 

**On R3, configure the ISAKMP policy, pre-shared key, and transform set.** 

**In this step, we will configure the same parameters for the ISAKMP policy 10 that we configured on** 

. 

**Configure ISAKMP policy 10 on R3:** 

conf t 

**! Configure ISAKMP (Phase 1) Policy for GRE over IPSec VPN** 

crypto isakmp policy 10 

&nbsp;encryption aes 256 

&nbsp;hash sha 

&nbsp;authentication pre-share 

&nbsp;group 1 

&nbsp;lifetime 36000 

&nbsp;exit 

end 

wr mem 

&nbsp;

**Configure the pre-shared key of cisco123 on R1. This command points to the remote peer R3 G0/0/0** 

**IP address.** 

R3(config)# 

R3 (config)#crypto isakmp key cisco123 address 64.100.0.2 

R3(config)# 

&nbsp;

**Create a new transform set called GRE-VPN using the same security parameters and transport mode** 

**that we configured on R1. Also configure the mode transport command.** 

conf t 

crypto ipsec transform-set GRE-VPN esp-aes 256 esp-sha-hmac 

&nbsp;mode transport 

&nbsp;exit 

end 

wr mem 

&nbsp;

O**n R3, configure the IPsec profile.** 

**Instead of a crypto map, we will configure an IPsec profile called GRE-PROFILE using the crypto** 

**ipsec profile ipsec-profile-name global configuration command.** 

R3 (config)#crypto ipsec profile GRE-profile 

R3(ipsec-profile) # 

&nbsp;

**In IPsec profile configuration mode, specify the transform set to be negotiated using the set** 

**transform-set transform-set-name command. Multiple transform sets can be specified in order of** 

**priority. The fist transform-set-name specified is the highest priority.** R3( ipsec-profile)# 

R3( ipsec-profile) #set transform-set GRE-VPN 

R3( ipsec-profile)#exit 

R3(config)# 

&nbsp;

**On R3, configure the tunnel interface.** 

**On R3, configure a GRE tunnel interface.** 

conf t 

! Configure GRE Tunnel interface 

interface Tunnel1 

&nbsp;bandwidth 4000 

&nbsp;ip address 172.16.1.2 255.255.255.252 

&nbsp;ip mtu 1400 

&nbsp;tunnel source 64.100.1.2 

&nbsp;tunnel destination 64.100.0.2 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

end 

wr mem 

&nbsp;

Apply the IPsec profile GRE-PROFILE to the Tunnel 1 interface using the tunnel protection ipsec 

profile profile-name command. 

conf t 

interface Tunnel1 

&nbsp;tunnel protection ipsec profile GRE-profile 

&nbsp;shutdown 

&nbsp;no shutdown 

&nbsp;exit 

end 

wr mem 

&nbsp;

**On R1 and R3, enable OSPF routing on the tunnel interface.** 

**Verify that the GRE over IPsec VPN is operational.** 

**On R1, perform an extended ping to the R3 10.10.16.1 interface** 

 

**R3#** 

&nbsp;

R1#ping 10.10.16.1 source 10.10.0.1 

&nbsp;

**The pings are successful, and it appears that the VPN is operational. On R1, verify the IPsec SA** 

**encrypted and decrypted statistics.** 

&nbsp;

show crypto ipsec sa | include encrypt|decrypt 

&nbsp;

**From D1, trace the path taken to the R3 10.10.16.1 interface**. 

&nbsp;

D1#trace 10.10.16.1 

&nbsp;

**On R1, configure OSPF to advertise the tunnel interfaces.** 

&nbsp;

conf t 

router ospf 123 

&nbsp;network 172.16.1.0 0.0.0.3 area 0 

&nbsp;exit 

end 

wr mem 

&nbsp;

**On R3, configure OSPF to advertise the tunnel interfaces.** 

&nbsp;

conf t 

! Enable OSPF on the GRE tunnel network 

router ospf 123 

&nbsp;network 172.16.1.0 0.0.0.3 area 0 

&nbsp;exit 

end 

wr mem 

&nbsp;

**Step 7: Verify the GRE over IPsec Tunnel on R1 and R3** 

**Now that the GRE over IPsec has been configured, we must verify that the tunnel interfaces** 

**are correctly enabled, that the crypto session is active, and then generate traffic to confirm it** 

**is traversing securely over the IPsec tunnel.** 

**On R1 and R3, verify the tunnel interfaces.** 

**Use the show interfaces tunnel 1 command to verify the interface settings** 

R1#sh interfaces tunnel 1 

&nbsp;

**On R3, use the show interfaces tunnel 1 command to verify the interface settings.** 

**show interface tunnel1 | include is up | Internet address | Enc | Tunnel protocol** 

&nbsp;

&nbsp;

**On R1 and R3, verify the crypto settings.** 

**On R1, use the show crypto session command to verify the operation of the VPN tunnel.** 

&nbsp;

R1#sh crypto session 

 

**On R3, use the show crypto session command to verify the operation of the VPN tunnel. R3#** 

R3#sh crypto session 

&nbsp;

**On R1 and R3, verify OSPF routing.** 

**On R1 and R3, verify which interfaces are configured for OSPF using the show ip ospf interface** 

**brief command.** 

R1# 

R1#sh ip ospf int bri 

&nbsp;

R3#sh ip ospf int bri 

&nbsp;

**On R1 and R3, verify the OSPF neighbours using the show ip ospf interface brief command.** 

R3#sh ip ospf neighbor 

&nbsp;

R1#sh ip route ospf 

&nbsp;

R3#sh ip route ospf 

&nbsp;

**Verify that there is an operational logical point-to-point link between R1 and R3 using the GRE** 

**tunnel interface.** 

&nbsp;

R1#sh ip route 172.16.0.0 

&nbsp;

R3#sh ip route 172.16.0.0 

&nbsp;

**Test the GRE over IPsec VPN tunnel.** 

**From D1, trace the path taken to the R3 10.10.16.1 interface.** 

&nbsp;

D1#trace 10.10.16.1 

&nbsp;

**On R1, verify the IPsec SA encrypted and decrypted statistics**. 

R1#sh crypto ipsec sa | include encrypt | decrypt 

&nbsp;

**The output verifies that the GRE over IPsec VPN tunnel is properly encrypting traffic between both** 

**sites. The packets encrypted include the trace packets along with OSPF packet**

