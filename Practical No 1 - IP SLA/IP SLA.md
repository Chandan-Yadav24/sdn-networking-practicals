### IP SLA



209.165.201.0

Step 1: Configure loopbacks and assign addresses.



**R1:**

conf t

hostname R1

int l0

desc R1 LAN

ip add 192.168.1.1 255.255.255.0

int s1/0

desc r1-->r2

ip add 209.165.201.2 255.255.255.252

clock rate 128000

bandwidth 128

no sh

exit

int s1/1

desc r1-->r3

ip add 209.165.202.130 255.255.255.252

clock rate 128000

bandwidth 128

no sh

exit





**R2:**

conf t

hostname ISP1

int l0

desc sim web serv

ip add 209.165.200.254 255.255.255.252

int l1

desc DNS serv

ip add 209.165.201.30 255.255.255.252

int s1/0

desc r2-->r1

ip add 209.165.201.1 255.255.255.252

clock rate 128000

bandwidth 128

no sh

exit

int s1/1

desc r2-->r3

ip add 209.165.200.225 255.255.255.252

clock rate 128000

bandwidth 128

no sh

exit





**R3:**

conf t

hostname ISP2

int l0

desc sim web serv

ip add 209.165.200.254 255.255.255.252

int l1

desc DNS serv

ip add 209.165.202.158 255.255.255.252

int s1/0

desc r3-->r1

ip add 209.165.202.129 255.255.255.252

clock rate 128000

bandwidth 128

no sh

exit

int s1/1

desc r3-->r2

ip add 209.165.200.226 255.255.255.252

clock rate 128000

bandwidth 128

no sh

exit





**Step 2**: Configure static routing



**R1:**

conf t

ip route 0.0.0.0 0.0.0.0 209.165.201.1



**R2:**

conf t

router eigrp 1

network 209.165.200.224 0.0.0.3

network 209.165.201.0 0.0.0.31

no auto-summary

exit



ip route 192.168.1.0 255.255.255.0 209.165.201.2



**R3:**

conf t

router eigrp 1

network 209.165.200.224 0.0.0.3

network 209.165.202.128 0.0.0.31

no auto-summary

exit



ip route 192.168.1.0 255.255.255.0 209.165.202.130



**Testing**

R1

tclsh

foreach address {

209.165.200.254

209.165.201.30

209.165.202.158

} {

ping $address source 192.168.1.1

}



trace

tclsh

foreach address {

209.165.200.254

209.165.201.30

209.165.202.158

} {

trace $address source 192.168.1.1

}





**Step 3: Configure IP SLA probes.**  



**R1**:

conf t

ip sla 11

icmp-echo  209.165.201.30

frequency 10

exit

ip sla schedule 11 life forever start-time now

end



**verify command**

**R1**:

show ip sla configuration 11

show ip sla statistics





**R1:**

conf t

ip sla 22

icmp-echo  209.165.202.158

frequency 10

exit

ip sla schedule 22 life forever start-time now

end





show ip sla configuration 22 

show ip sla statistics 22



**Step 4: Configure tracking options.** 

**R1:**

conf t

no ip route 0.0.0.0 0.0.0.0 209.165.201.1

ip route 0.0.0.0 0.0.0.0 209.165.201.1 5



&nbsp;Verify the routing table. 

show ip route | begin Gateway 



**config-track subconfiguration mode.** 

conf t

track 1 ip sla 11 reachability

delay down 10 up 1 

exit



**Debug**

R1# debug ip routing 



 **floating static route t**

conf t

ip route 0.0.0.0 0.0.0.0 209.165.201.1 2 track 1 



**for 2nd track**

conf t

track 2 ip sla 22 reachability 

delay down 10 up 1 

exit 

ip route 0.0.0.0 0.0.0.0 209.165.202.129 3 track 2 



**Verify the routing table again.** 

R1#show ip route | begin Gateway





**Step 5 verify SLA OPeration**

**R2:**

int lo1

shutdown



R1:see output



show ip route | begin Gateway

show ip sla statistics



**trace web server**

trace 209.165.200.254 source 192.168.1.1



**R2:**

ISP1(config-if)# no shutdown 





**R1:**

show ip sla statistics 

show ip route | begin Gateway 

