# IPv4 ACLs



**Step 1**: conf ip all router and pc



**R1:**

conf t

int f0/0

ip add 192.168.1.50 255.255.255.0

no sh

ex

int s1/0

ip add 10.1.1.1 255.255.255.0

no sh

ex



**R2:**

conf t

int f0/0

ip add 192.168.3.50 255.255.255.0

no sh

ex

int s1/0

ip add 10.1.1.2 255.255.255.0

no sh

ex

int s1/1

ip add 11.1.1.2 255.255.255.0

no sh

ex



**R3:**

conf t

int f0/0

ip add 192.168.2.50 255.255.255.0

no sh

ex

int s1/0

ip add 11.1.1.1 255.255.255.0

no sh

ex



**PC 1:**

ip 192.168.1.1 255.255.255.0 192.168.1.50

sh ip



**PC 2:**

ip 192.168.3.1 255.255.255.0 192.168.3.50

sh ip



**PC 3:**

ip 192.168.2.1 255.255.255.0 192.168.2.50

sh ip





**R1:** 

ping 192.168.1.1 

&nbsp;

**R2:** 

ping 192.168.3.1 

&nbsp;

**R3:** 

ping 192.168.2.1



**Step 5: Follow below to configure RIP in routers.** 

**R1:**

conf t

router rip

network 192.168.1.0

network 10.1.1.0

ex



**R2:**

conf t

router rip

network 192.168.3.0

network 10.1.1.0

network 11.1.1.0

ex



**R3:**

conf t

router rip

network 192.168.2.0

network 11.1.1.0

ex



**Step 6: Now we will apply ACL.** 

Standard ACL now



**R1:**

conf t

access-list 10 deny host 192.168.2.1

exit

show access-list

conf t

access-list 10 permit any

int s1/0

ip access-group 10 in

exit



**PC3:** 

ping 192.168.1.1 (it will not work)



**R3:**

conf t

access-list 40 deny host 192.168.3.1

exit

show access-list

conf t

access-list 40 permit any

int s1/0

ip access-group 40 in

exit



**PC2:**

ping 192.168.2.1 (it will not work)





**R2:**

conf t 

access-list 10 deny 192.168.1.1 

exit 

sh access-list

conf t 

access-list 10 permit any 

int s1/0 

ip access-group 10 in 

exit 

access-list 20 deny icmp host 192.168.1.1 host 192.168.2.1



PC1> ping 192.168.3.1 







**Step 7: We will now apply Extended ACL**  



**R3:**

conf t

access-list 121 deny icmp host 192.168.3.1 host 192.168.2.1 

do sh access-list 121 

access-list 121 permit icmp any any 

do sh access-list 121 

int s1/0 

ip access-group 121 out 

do sh access-list 121 

exit 



Here PC3 have accept R2 but deny PC2.  

PC2> ping 192.168.2.1 



But if any other device ping PC 3 it will permit it  

R2#ping 192.168.2.1 





