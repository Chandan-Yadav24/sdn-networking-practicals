### EIGPR 



**R2**

conf t

int f0/0

ip add 192.168.3.1 255.255.255.0

no sh

exit

int s1/0

ip add 192.168.0.2 255.255.255.0

no sh

exit



**R1**

conf t

int s1/0

ip add 192.168.0.1 255.255.255.0

no sh

exit

int f0/0

ip add 192.168.4.1 255.255.255.0

no sh

exit

int s1/1

ip add 192.168.1.1 255.255.255.0

no sh

exit



**R3**

conf t

int s1/0

ip add 192.168.1.2 255.255.255.0

no sh

exit

int f0/0

ip add 192.168.2.1 255.255.255.0

no sh

exit





**PC 3**

&nbsp;ip 192.168.2.2 192.168.2.1



**PC2**



&nbsp;ip 192.168.4.2 192.168.4.1



**PC1**

&nbsp;ip 192.168.3.2 192.168.3.1







Eigrp part



**R1**

conf t

router eigrp 2

network 192.168.3.0

network 192.168.0.0

network 192.168.1.0

network 192.168.4.0 

**R2**

conf t

router eigrp 2

network 192.168.4.0

network 192.168.0.0

network 192.168.1.0

network 192.168.3.0 


**R3**

conf t

router eigrp 2

network 192.168.2.0

network 192.168.1.0







ping



ping 192.168.4.2

ping 192.168.3.2

ping 192.168.2.2





