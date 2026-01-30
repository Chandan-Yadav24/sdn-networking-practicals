# **VLAN**



**Create the following VLANS on switch Bobcat:** 

• VLAN 10: name Tigers 

• VLAN 20: name Lions 

• VLAN 30: name Panthers 

• Configure the interfaces between the switches as trunks. 

• Configure switch Bobcat to be the VTP server. 

• Configure switch Panther to be a VTP client. 

• Configure switch Tiger so it does not synchronize itself to the latest VTP information, it 

should 

• forward advertisements to switch Panther though. 

• Change the VTP domain name to "MSCCS". 

• Use the password "MSCCS123" for VTP. 

• Make sure there is no unnecessary vlan traffic flooded on the trunk links. 

&nbsp;

&nbsp;

Create the following VLANS on switch Bobcat: 

VLAN 10: name Tigers 

VLAN 20: name Lions 

VLAN 30: name Panthers 



1\) Physical Connection / Trunk Link Table



+--------+----------+--------+----------+--------+-----------+---------------+----------------+

| Link # | Switch A | Port A | Switch B | Port B | Link Type | Encapsulation | VLANs Carried |

+--------+----------+--------+----------+--------+-----------+---------------+----------------+

|   1    | Bobcat   | e1/0   | Tiger    | e1/0   | Trunk     | 802.1Q (dot1q)| 10, 20, 30    |

|   2    | Tiger    | e1/1   | Panther  | e1/0   | Trunk     | 802.1Q (dot1q)| 10, 20, 30    |

+--------+----------+--------+----------+--------+-----------+---------------+----------------+





**Bobcat:**

conf t 

vlan 10 

vlan 20 

vlan 30 

exit 

end 

show vlan brief 

&nbsp;

&nbsp;

**Configure the interfaces between the switches as trunks.** 

**Bobcat:**

conf t

int range e1/0 

switchport trunk encapsulation dot1q 

&nbsp;

&nbsp;

**Tiger:**

conf t 

int range e1/0 

switchport trunk encapsulation dot1q 

exit



int range e1/1 

switchport trunk encapsulation dot1q 

exit 



&nbsp;

&nbsp;

&nbsp;

**Panther**:

conf t 

int range e1/0 

&nbsp;

switchport trunk encapsulation dot1q 

exit 

&nbsp;

**Configure switch Bobcat to be the VTP server.** 

int e1/0 

vtp mode server 

&nbsp;

**Configure switch Panther to be a VTP client.** 

int e1/0 

vtp mode client 

&nbsp;

**Configure switch Tiger so it does not synchronise itself to the lastest VTP information, it should** 

**forward advertisements to switch Panther though.** 

**Tiger:**

int e1/1 

vtp mode transparent 

&nbsp;

**Change the VTP domain name to "MSCCS".** 

**Tiger:**

conf t 

vtp domain MSCCS 

&nbsp;

**Bobcat** :

conf t

vtp domain MSCCS 

&nbsp;

**Panther:** 

conf t

vtp domain MSCCS 

&nbsp;

**Use the password "MSCCS123" for VTP.** 

**Bobcat** 

&nbsp;vtp password MSCCS123 

&nbsp;

**Tiger**  

vtp password MSCCS123 

&nbsp;

**Panther**  

vtp password MSCCS123 

&nbsp;

&nbsp;

**Check and verify the VLANs** 

Bobcat#show vlan 

&nbsp;

Tiger#show vlan 

&nbsp;

Panther#show vlan 

&nbsp;

Panther#show vtp status 

&nbsp;

**Make sure there is no unnecessary vlan traffic flooded on the trunk links.** 

&nbsp;

**Bobcat:** 

conf t 

vtp pruning 

&nbsp;

Panther#show vtp status 

&nbsp;



