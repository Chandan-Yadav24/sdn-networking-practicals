**Observe STP Topology Changes and Implement RSTP :** 

 **a) Implement Advanced STP Modifications and**  

       **Mechanisms.** 

 **b) Implement MST** 







**Physical Connection Table (from your interface descriptions)**



+------+-----------+--------------------+------+-----------+------------------------------+

| DevA | PortA     | Link Type          | DevB | PortB     | Notes                        |

+------+-----------+--------------------+------+-----------+------------------------------+

| D1   | e0/0      | TRUNK (802.1Q)     | D2   | e0/0      | D1<->D2 uplink               |

| D1   | e0/1      | TRUNK (802.1Q)     | A1   | e0/2      | D1<->A1 uplink (2nd link)    |

| D1   | e0/2      | TRUNK (802.1Q)     | A1   | e0/0      | D1<->A1 uplink (1st link)    |

| D2   | e0/1      | TRUNK (802.1Q)     | A1   | e0/1      | D2<->A1 uplink (1st link)    |

| D2   | e0/2      | TRUNK (802.1Q)     | A1   | e0/3      | D2<->A1 uplink (2nd link)    |

+------+-----------+--------------------+------+-----------+------------------------------+



IP Address Table (Management SVI on VLAN 1)

+--------+-------------------+-------------+------------+------------------------------+

| Device | Interface         | VLAN        | IP/Mask    | Purpose                      |

+--------+-------------------+-------------+------------+------------------------------+

| D1     | Vlan1 (SVI)       | VLAN 1      | 10.0.0.1/8 | Management IP + STP root     |

| D2     | Vlan1 (SVI)       | VLAN 1      | 10.0.0.2/8 | Management IP                |

| A1     | Vlan1 (SVI)       | VLAN 1      | 10.0.0.3/8 | Management IP                |

+--------+-------------------+-------------+------------+------------------------------+





**Switch D1** 

enable 

configure terminal 

hostname D1 

line console 0 

&nbsp;logging synchronous 

&nbsp;exec-timeout 0 0 

spanning-tree mode pvst 

interface vlan1 

&nbsp;ip address 10.0.0.1 255.0.0.0 

&nbsp;no shutdown 

! ---- Uplinks ---- 

interface ethernet0/0 

&nbsp;description to D2 e0/0 

&nbsp;switchport 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

interface ethernet0/1 

&nbsp;description to A1 e0/2 

&nbsp;switchport 	

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

interface ethernet0/2 

&nbsp;description to A1 e0/0 

&nbsp;switchport 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

end 

write memory 

&nbsp;

&nbsp;

**Make D1 a deterministic root** 

conf t 

spanning-tree vlan 1 root primary 



&nbsp;

**Switch D2** 

enable 

configure terminal 

hostname D2 

! 

line console 0 

&nbsp;logging synchronous 

&nbsp;exec-timeout 0 0 

! 

spanning-tree mode pvst 

! 

Vlan 1 

name management 

interface vlan1 

&nbsp;ip address 10.0.0.2 255.0.0.0 

&nbsp;no shutdown 

! 

! ---- Uplinks ---- 

interface ethernet0/0 

&nbsp;description to D1 e0/0 

&nbsp;switchport 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

! 

interface ethernet0/1 

&nbsp;description to A1 e0/1 

&nbsp;switchport 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

! 

interface ethernet0/2 

&nbsp;description to A1 e0/3 

&nbsp;switchport 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

! 

end 

write memory 

&nbsp;

&nbsp;

**Switch A1** 

enable 

configure terminal 

hostname A1 

no ip domain-lookup 

service password-encryption 

banner motd # STP/RSTP Lab - A1 # 

! 

line console 0 

&nbsp;logging synchronous 

&nbsp;exec-timeout 0 0 

! 

spanning-tree mode pvst 

! 

interface vlan1 

&nbsp;ip address 10.0.0.3 255.0.0.0 

&nbsp;no shutdown 

! 

! ---- Uplinks ---- 

interface ethernet0/0 

&nbsp;description to D1 e0/2 

&nbsp;switchport 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

! 

interface ethernet0/1 

&nbsp;description to D2 e0/1 

&nbsp;switchport 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

! 

interface ethernet0/2 

&nbsp;description to D1 e0/1 

&nbsp;switchport 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

! 

interface ethernet0/3 

&nbsp;description to D2 e0/2 

&nbsp;switchport 

&nbsp;switchport trunk encapsulation dot1q 

&nbsp;switchport mode trunk 

&nbsp;no shutdown 

! 

end 

write memory 

&nbsp;

**4) Sanity Checks (what/why/how)** 

D1#show interfaces trunk ! verify trunks are up and VLAN 1 is allowed/active 

&nbsp;

D2#show interfaces trunk 

&nbsp;

A1#show interfaces trunk 

&nbsp;

D1#show spanning-tree summary 

&nbsp;

D2#show spanning-tree summary 

&nbsp;

A1#show spanning-tree summary 

&nbsp;

**(From D1 for example PING):** 

D1#ping 10.0.0.2 source vlan1 ! reach D2 SVI 

&nbsp;

**5) Part 2 -Discover the Default Spanning Tree (PVST+) (Per-VLAN Spanning Tree Plus)** 

&nbsp;

**A) Identify the Root Bridge** 

D1#show spanning-tree root 

&nbsp;

Record: Bridge ID of the root, Root Port (local), and Root Path Cost. If you set root primary on 

D1 should be the root. 

&nbsp;

**B) Inspect Port Roles/States** 

**• Root Bridge (likely D1):** all participating ports should be Designated Forwarding. 

**• Non-root (A1, D2):** each has exactly one Root port; redundant paths become Alternate 

(Blocking/Discarding). Which one blocks depends on root path cost and tie-breakers 

(bridge/port IDs). 

&nbsp;

D1#show spanning-tree vlan 1 

&nbsp;

D2#show spanning-tree vlan 1 

&nbsp;

A1#show spanning-tree vlan 1 

&nbsp;

**6) Show It Adjusts -Simulate a Failure (PVST+)** 

**Turn on event logs (D2) and** 

**Fail a link (shut D2 A1 on D2 e0/1):** 

&nbsp;

**D2:**

debug spanning-tree events 

conf t 

interface ethernet0/1 

&nbsp;shutdown 

end 

&nbsp;

**Observe:** 

**• STP re-computes; a previously Alternate link should unblock to maintain a single loop-free** 

**path.** 

**• PVST+ takes several seconds (listening/learning) before forwarding resumes.** 

**Restore link \& stop logs:** 



**D2:** 

conf t 

interface ethernet0/1 

&nbsp;no shutdown 

end 

undebug all 

&nbsp;

**Re-check roles:** 

D2#show spanning-tree vlan 1 

&nbsp;

**7) Migrate to RSTP (Rapid-PVST) and Compare** 

**Enable on all switches:** 

**D1:** 

conf t 

spanning-tree mode rapid-pvst 

end 

write memory 

&nbsp;

D1#show spanning-tree summary ! should indicate rapid-pvst 

&nbsp;

**D2:** 

conf t 

spanning-tree mode rapid-pvst 

end 

write memory 

&nbsp;

D2#show spanning-tree summary ! should indicate rapid-pvst 

&nbsp;

**A1:**

configure terminal 

spanning-tree mode rapid-pvst 

end 

&nbsp;

A1#show spanning-tree summary 

&nbsp;

**Repeat the same failure test (again D2 e0/1):** 



**D2:**

debug spanning-tree events 

configure terminal 

interface e0/1 

shutdown 

end 

&nbsp; 

**Expected: Noticeably faster reconvergence (often sub-second to a couple of seconds on** 

**point-to-point links), thanks to RSTP's proposal/agreement and sync processes.** 



**D2:**

configure terminal 

interface e0/1 

no shutdown 

end 

undebug all 

&nbsp;

&nbsp;

Results Checked 

PVST+ took approx. 16 microseconds 

RSTP took approx. 7 microseconds

