----------------------------------------------------------------------------------------------------------------------
1. Topology description:
----------------------------------------------------------------------------------------------------------------------

This topology is a simplified 3 tier architecture, which connects 4 separate zones (Floor1, Floor2, Floor3,Internet(host 10.0.0.86, for better testing) and Basement).
Each of the separate zone has its own ranges of IP addresses, VLANs, Access Policy, etc.

Note: The full documentation you can read in Full_Document_PKT1.txt

----------------------------------------------------------------------------------------------------------------------



				        |10.0.0.86|
					(INTERNET)
					    |
					    |
     |VLAN45||VLAN40||VLAN30|(Floor2) --- (CORE) --- (Basement | VLAN1 | Servers)
		         	          /   \
   				         /     \
   				  (Floor1)     (Floor3)
				  |VLAN10|     |VLAN50|
				  |VLAN20|     |VLAN60|



----------------------------------------------------------------------------------------------------------------------
============================================================================================================================
WARNING: 

In this lab environment, SVIs are treated as trusted infrastructure interfaces.
Permitting all traffic sourced from SVI is a simplification to avoid over-complicating ACLs for control-plane and management protocols such as HSRP, DHCP relay, NTP, Syslog and SSH

----------------------------------------------------------------------------------------------------------------------------

HSRP control traffic is not affected by the applied VLAN ACLs due to its link-local multicast nature (TTL=1).
Additional permit statements for SVI-sourced traffic are included to avoid potential issues with management-plane traffic in Packet Tracer.

----------------------------------------------------------------------------------------------------------------------------

DNS access model :

End hosts are allowed to perform DNS resolution (UDP/TCP port 53) everywhere.
Direct access to DNS servers (ICMP, TCP other than 53) MAY be blocked by ACLs and policy requirements if needed.

For example, if the policy will be about denying access to the remote servers means that UDP/TCP 53 kinds of messages still 
won`t be discarded. 
----------------------------------------------------------------------------------------------------------------------------

Special Devices meaning:

Special device means device which IP address statically assigned and should be considered as trusted. Usually it is Layer 2 
Switches of Access and Distribution. They use VLAN default gateway as their own and belong to specific VLAN.
To simplify, it is the devices with SVI, but aren't used in routing the packets. It allows to provide
SSH, NTP, FTP, Syslog and other useful features even on Layer 2 devices

----------------------------------------------------------------------------------------------------------------------------
SVI and MLS meanings:

SVI means Switch Virtual Interface,which is assigned to Access, Distribution and Multilayer Switches. 

MLS means MultiLayer Switch
----------------------------------------------------------------------------------------------------------------------------
Banners:

Each device has its message of the day.
----------------------------------------------------------------------------------------------------------------------------
Remote and local server meaning:

Remote server means that server is located in the other network.
Local server means that server is located in the same network

============================================================================================================================
----------------------------------------------------------------------------------------------------------------------------



	|
	|
	V


-----------------------------------------------------------------------------------------------------------------------
========
SERVICES
========
-----------------------------------------------------------------------------------------------------------------------

The basement provides services such as NTP, DNS, DHCP, 
FTP, TFTP, Syslog and SNMP(Partly, due to Cisco Packet Tracer simplification).

DNS Server IP Address: 10.0.0.226

DHCP Server IP Address: 10.0.0.227

NTP Server IP Address: 10.0.0.228

PC_ADMIN (SSH Client) IP Address: 10.0.0.229

Syslog Server IP Address: 10.0.0.230

(T)FTP Server IP Address: 10.0.0.231

SW1 (One of the SSH Servers) IP Address: 10.0.0.232

------------------------------------------------------------------------------------------------------------------------



	|
	|
	V

------------------------------------------------------------------------------------------------------------------------
===============================================
Brief Tests for each VLAN(1,10-60) and services
===============================================
------------------------------------------------------------------------------------------------------------------------

----------------------------------------Testing of VLAN1---------------------------------------------------------------

1) CASE 1: Link status
Command: In privileged mode: show ip interface brief / show spanning-tree

Expected result: All needed interfaces should be UP/UP
------------------------------------------------------------------------------------------------------------------------
2) CASE 2: Clarifying if CDP/LLDP enabled:

Command: In Privileged mode: show cdp neighbors / show lldp neighbors

Expected result:
(SW1)
 	1) Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
R_B          Gig 0/1          147            R       C2900       Gig 0/1

 	2) Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
Device ID           Local Intf     Hold-time  Capability      Port ID
R_B                 Gig0/1         120        R               Gig0/1

Total entries displayed: 1
------------------------------------------------------------------------------------------------------------------------
3) CASE 3: Spanning-tree testing

Command: In Privileged mode: show spanning-tree.
 
The expected result: The output depends on the device, but expected to see a SW1 as a Root Bridge.
------------------------------------------------------------------------------------------------------------------------
4) CASE 4: Default Gateway and VLAN connectivity:
Command: 1) From PC try to ping another host in the same VLAN
 	 2) From any device in VLAN1 try to ping 10.0.0.225 .

Expected result: Successful ping in both cases.
------------------------------------------------------------------------------------------------------------------------
5) CASE 5: Test for Internet connection, DHCP and DNS responses.

Command: 1)ipconfig /all or ipconfig /renew from any PC to check if DHCP messages is working.
         2)Ping internet.com (10.0.0.86) from any PC.
 
Expected results: 1)Unsuccessful DHCP responses because of static config.
 		  2)Successful ping  and DNS responses is expected.
------------------------------------------------------------------------------------------------------------------------
6) CASE 6: Test for connection between VLANs:

Command: To test connection from any PC to the any other VLAN. ping pc1.vlanX.com , X - vlan number.

Expected results: Successful ping to the VLAN20,30,40,50,60
 		  Unsuccessful ping to the VLAN10,45

------------------------------------------------------------------------------------------------------------------------
7)  CASE 7: Test to ping any remote server

Command: Ping 10.0.0.226 (domain server. Can be any other Server) from PC_ADMIN.

Expected result: Successful ping.
------------------------------------------------------------------------------------------------------------------------
8) CASE 8: SSH Connection from PC_ADMIN

Command: From the PC_ADMIN try 'SSH -l *login* *IP or domain name of the host* ' command in command prompt to check SSH connectivity to the SW1, R_B.
Expected result: Successful connection with logging.
 		 If the brute force attempt detected(5 tries within 45 seconds) on R_B - block for a 2 minutes.
 		 SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.
------------------------------------------------------------------------------------------------------------------------
9)  CASE 9: NTP testing:

Command: From the either SW1 or R_B try to check NTP status. In Privileged mode: show ntp associations /show ntp status.

Expected example of output:
(R_B)

address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
------------------------------------------------------------------------------------------------------------------------
10) CASE 10: File Transferring test

Command: From the either SW1 or R_B, try to download any file by TFTP or FTP. The (T)FTP server is 10.0.0.31.
In Privileged mode: copy flash: ftp: ; *filename*.

Expected result: Successful file transfer. (Need to wait a few minutes)


------------------------------------------------------------------------------------------------------------------------
11) Case 11: Syslog testing
Command: From SW1 or R_B try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).
 	 In Global configuration mode
 	 interface *any unused interface*
         no shutdown !enabling it
 	 shutdown !disabling the interface to the default state

Expected result: The new trap would be created and syslog message will appear on the remote server.
------------------------------------------------------------------------------------------------------------------------








----------------------------------------Testing of VLAN10---------------------------------------------------------------

1) CASE 1: Etherchannel and link status
Command: In privileged mode: Show etherchannel summary / show spanning-tree (to see if there are no fastEthernets) / show ip interface brief

Expected result: All needed interfaces should be UP/UP
(DSW3)
Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/3(P)       
2      Po3(SU)           LACP   Fa0/2(P) Fa0/4(P) 

Note: 
If Po2 appears on the DSW3 with status (SD); - , it may be an bug of old configuration. So, don`t pay attention to it
However, on other Switches it is not okay if some unknown port-channels are shown.
------------------------------------------------------------------------------------------------------------------------
2) CASE 2: Clarifying if CDP/LLDP is disabled:

Command: In Privileged mode: show cdp neighbors / show lldp neighbors

Expected result: % CDP is not enabled
		 % LLDP is not enabled
------------------------------------------------------------------------------------------------------------------------
3) CASE 3: Spanning-tree testing

Command: In Privileged mode: show spanning-tree.
 
The expected result: The output depends on the device, but expected to see a DSW3 as the Root Bridge.
------------------------------------------------------------------------------------------------------------------------
4) CASE 4: Default Gateway and VLAN connectivity:
Command: 1) From PC try to ping another host in the same VLAN 
	 2) From any device in VLAN10 try to ping 10.0.1.126 (VIP), 10.0.1.124 and 10.0.1.125.

Expected result: Successful ping in both cases.
------------------------------------------------------------------------------------------------------------------------
5) CASE 5: Test for Internet connection, DHCP and DNS responses. 

Command: 1)ipconfig /all or ipconfig /renew from any PC to check if DHCP messages is working.
         2)Ping internet.com (10.0.0.86) from any PC.
 
Expected results: 1)Successful DHCP responses and DNS responses. 
		  2)Ping successful responses is expected.
------------------------------------------------------------------------------------------------------------------------
6) CASE 6: Test for connection between VLANs:

Command: To test connection from any PC to the any other VLAN. ping pc1.vlanX.com , X - vlan number.

Expected results: Successful ping to the VLAN10
		  Unsuccessful ping to the VLAN20, 30, 40, 50, 60

------------------------------------------------------------------------------------------------------------------------
7)  CASE 7: Test to ping any remote server

Command: Ping 10.0.0.226 (domain server. Can be any other Server) from any PC. 

Expected result: Unsuccessful ping.
------------------------------------------------------------------------------------------------------------------------
8) CASE 8: SSH Connection from PC_ADMIN

Command: From the PC_ADMIN try 'SSH -l *login* *IP or domain name of the host* ' command in command prompt to check SSH connectivity to the ASW3,DSW3 or MLS3/4. 

Expected result: Successful connection with logging.
		 If the brute force attempt detected(5 tries within 45 seconds) on MLS3/4 - block for a 2 minutes.
		 SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.
------------------------------------------------------------------------------------------------------------------------
9)  CASE 9: NTP testing:

Command: From the either ASW3,DSW3 or MLS3/4 try to check NTP status. In Privileged mode: show ntp associations /show ntp status.

Expected example of output:
(ASW3)

address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
------------------------------------------------------------------------------------------------------------------------
10) CASE 10: File Transferring test

Command: From the either ASW3,DSW3 or MLS3/4, try to download any file by TFTP or FTP. The (T)FTP server is 10.0.0.31. 
In Privileged mode: copy flash: ftp: ; *filename*. 

Expected result: Successful file transfer. (Need to wait a few minutes)


------------------------------------------------------------------------------------------------------------------------
11) Case 11: Syslog testing
Command: From ASW3, DSW3 or MLS3/4 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).
	 In Global configuration mode
	 interface *any unused interface*  
         no shutdown !enabling it 
	 shutdown !disabling the interface to the default state

Expected result: The new trap would be created and syslog message will appear on the remote server.
------------------------------------------------------------------------------------------------------------------------






----------------------------------------Testing of VLAN20---------------------------------------------------------------

1) CASE 1: Etherchannel and link status
Command: In privileged mode: Show etherchannel summary / show spanning-tree (to see if there are no fastEthernets) / show ip interface brief

Expected result: 
(DSW4)
Group  Port-channel  Protocol    Ports
------+-------------+-----------+------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/3(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/4(P) 

------------------------------------------------------------------------------------------------------------------------
2) CASE 2: Clarifying if CDP/LLDP is disabled:

Command: In Privileged mode: show cdp neighbors / show lldp neighbors

Expected result: % CDP is not enabled
		 % LLDP is not enabled

------------------------------------------------------------------------------------------------------------------------
3) CASE 3: Spanning-tree testing

Command: In Privileged mode: show spanning-tree.
 
The expected result: The output depends on the device, but expected to see a DSW4 as a Root Bridge.

------------------------------------------------------------------------------------------------------------------------
4) CASE 4: Default Gateway and VLAN connectivity:

Command: 1) From PC try to ping another host in the same VLAN 
	 2) From any device in VLAN20 try to ping 10.0.1.254 (VIP), 10.0.1.253 and 10.0.1.252.

Expected result: Successful ping in both cases.

------------------------------------------------------------------------------------------------------------------------
5) CASE 5: Test for Internet connection, DHCP and DNS responses. 

Command: 1)ipconfig /all or ipconfig /renew from any PC to check if DHCP messages is working.
         2)Ping internet.com (10.0.0.86) from any PC.
 
Expected results: 1)Successful DHCP responses and DNS responses. 
		  2)Ping unsuccessful responses is expected.

------------------------------------------------------------------------------------------------------------------------
6) CASE 6: Test for connection between VLANs:

Command: To test connection from any PC to the any other VLAN. ping pc1.vlanX.com.

Expected results: Successful ping to the VLAN20, 40, 50, 60
		  Unsuccessful ping to the VLAN10, 30

------------------------------------------------------------------------------------------------------------------------
7)  CASE 7: Test to ping any remote server

Command: Ping 10.0.0.226 (domain server. Can be any other Server) from any PC. 

Expected result: Successful ping.

------------------------------------------------------------------------------------------------------------------------
8) CASE 8: SSH Connection from PC_ADMIN

Command: From the PC_ADMIN try 'SSH -l *login* *IP or domain name of the host* ' command in command prompt to check SSH connectivity to the ASW4,DSW4 or MLS3/4. 

Expected result: Successful connection with logging.
		 If the brute force attempt detected(5 tries within 45 seconds) on MLS3/4 - block for a 2 minutes.
		 SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.

------------------------------------------------------------------------------------------------------------------------
9)  CASE 9: NTP testing:

Command: From the either ASW4,DSW4 or MLS3/4 try to check NTP status. In Privileged mode: show ntp associations /show ntp status.

Expected example of output:

address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12

------------------------------------------------------------------------------------------------------------------------
10) CASE 10: File Transferring test

Command: From the either ASW4,DSW4 or MLS3/4, try to download any file by TFTP or FTP. The (T)FTP server is 10.0.0.31. 
In Privileged mode: copy flash: ftp: ; *filename*. 

Expected result: Successful file transfer. (Need to wait a few minutes)


------------------------------------------------------------------------------------------------------------------------
11) Case 11: Syslog testing
Command: From ASW4, DSW4 or MLS3/4 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).
	 In Global configuration mode
	 interface *any unused interface* 
         no shutdown !enabling it 
	 shutdown !disabling the interface to the default state

Expected result: The new trap would be created and syslog message will appear on the remote server.

------------------------------------------------------------------------------------------------------------------------





----------------------------------------Testing of VLAN30---------------------------------------------------------------

1) CASE 1: Etherchannel and link status
Command: In privileged mode: Show etherchannel summary / show spanning-tree (to see if there are no fastEthernets) / show ip interface brief

Expected result: All needed interfaces should be UP/UP
(ASW5)
Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P) 

------------------------------------------------------------------------------------------------------------------------
2) CASE 2: Clarifying if CDP/LLDP is enabled:

Command: In Privileged mode: show cdp neighbors / show lldp neighbors

Expected result: 
(ASW5)
	1)Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
         	           S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
  Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
  ASW6         Por 3            148            S       2960        Fas 0/3
  ASW6         Por 3            148            S       2960        Fas 0/6
  ASW6         Por 3            148            S       2960        Por 3
  DSW6         Por 2            148            S       2960        Fas 0/2
  DSW6         Por 2            148            S       2960        Fas 0/5
  DSW6         Por 2            148            S       2960        Por 2
  DSW5         Por 1            149            S       2960        Fas 0/1
  DSW5         Por 1            149            S       2960        Fas 0/4
  DSW5         Por 1            149            S       2960        Por 1
	2) % LLDP is not enabled
------------------------------------------------------------------------------------------------------------------------
3) CASE 3: Spanning-tree testing

Command: In Privileged mode: show spanning-tree.
 
The expected result: The output depends on the device, but expected to see a DSW5 as a Root Bridge.
------------------------------------------------------------------------------------------------------------------------
4) CASE 4: Default Gateway and VLAN connectivity:
Command: 1) From PC try to ping another host in the same VLAN 
	 2) From any device in VLAN30 try to ping 10.0.2.62 (VIP), 10.0.2.61 and 10.0.2.60.

Expected result: Successful ping in both cases.
------------------------------------------------------------------------------------------------------------------------
5) CASE 5: Test for Internet connection, DHCP and DNS responses. 

Command: 1)ipconfig /all or ipconfig /renew from any PC to check if DHCP messages is working.
         2)Ping internet.com (10.0.0.86) from any PC.
 
Expected results: 1)Successful DHCP responses and DNS responses. 
		  2)Ping successful responses is expected.
------------------------------------------------------------------------------------------------------------------------
6) CASE 6: Test for connection between VLANs:

Command: To test connection from any PC to the any other VLAN. ping pc1.vlanX.com , X - vlan number.

Expected results: Successful ping to the VLAN30,40,50,60
		  Unsuccessful ping to the VLAN10,20

------------------------------------------------------------------------------------------------------------------------
7)  CASE 7: Test to ping any remote server

Command: Ping 10.0.0.226 (domain server. Can be any other Server) from any PC. 

Expected result: Successful ping.
------------------------------------------------------------------------------------------------------------------------
8) CASE 8: SSH Connection from PC_ADMIN

Command: From the PC_ADMIN try 'SSH -l *login* *IP or domain name of the host* ' command in command prompt to check SSH connectivity to the ASW5 or MLS5/6. 

Expected result: Successful connection with logging.
		 If the brute force attempt detected(5 tries within 45 seconds) on MLS5/6 - block for a 2 minutes.
		 SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.
------------------------------------------------------------------------------------------------------------------------
9)  CASE 9: NTP testing:

Command: From the either ASW5 or MLS5/6 try to check NTP status. In Privileged mode: show ntp associations /show ntp status.

Expected example of output:
(ASW5)

address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
------------------------------------------------------------------------------------------------------------------------
10) CASE 10: File Transferring test

Command: From the either ASW5 or MLS5/6, try to download any file by TFTP or FTP. The (T)FTP server is 10.0.0.31. 
In Privileged mode: copy flash: ftp: ; *filename*. 

Expected result: Successful file transfer. (Need to wait a few minutes)


------------------------------------------------------------------------------------------------------------------------
11) Case 11: Syslog testing
Command: From ASW5 or MLS5/6 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).
	 In Global configuration mode
	 interface *any unused interface*  
         no shutdown !enabling it 
	 shutdown !disabling the interface to the default state

Expected result: The new trap would be created and syslog message will appear on the remote server.
------------------------------------------------------------------------------------------------------------------------







----------------------------------------------------------------------------------------------------------------------
VLAN40 (Other VLANs are not covered in this documentation section)
----------------------------------------------------------------------------------------------------------------------
1)VLAN40 uses the following IP range of addresses: 10.0.2.128/25, addresses 10.0.2.250, .251, .252, .253 and .254 are reserved and cannot be assigned to the default clients. 
IP 10.0.2.250 is given to ASW5, 10.0.2.251 is given to DSW5. The addresses 10.0.2.252 - MLS6, 10.0.2.253 - MLS5. 
The address 10.0.2.254 is the default gateway.

| VLAN | Subnet       | Gateway    | DHCP Range             | Reserved IPs |
| ---- | -----------  | ---------- | -------------------    | ------------ |
| 40   | 10.0.2.128/25| 10.0.2.254 | 10.0.2.129-10.0.2.249  | 250-254      |


2)The VLAN40 uses ports on both ASW5 and ASW6.

ASW5:    
| Interface Type      | Ports                                                  |
| ------------------- | -------------------------------------------------------|
| **Trunk (Po1)**     | Fa0/1, Fa0/4                                           |
| **Trunk (Po2)**     | Fa0/2, Fa0/5                                           |
| **Trunk (Po3)**     | Fa0/3, Fa0/6                                           |
| **Access (VLAN40)** | Fa0/13-14 (active), Fa0/15–24 (inactive)               |
| **Unused / VLAN1**  | Fa0/7-9(down), Gi0/1(down), Gi0/2 (down)               |
| **SVI**             | Vlan40 (10.0.2.250), Vlan1 (down)                      |

--------------------------------------------------------------------------------
                                                                         
ASW6:
| Interface Type      | Ports                                                  |
| ------------------- | -------------------------------------------------------|
| **Trunk (Po1)**     | Fa0/1, Fa0/4                                           |
| **Trunk (Po2)**     | Fa0/2, Fa0/5                                           |
| **Trunk (Po3)**     | Fa0/3, Fa0/6                                           |
| **Access (VLAN40)** | Fa0/13-14 (active), Fa0/15–24 (inactive)               |
| **Unused / VLAN1**  | Fa0/7-9(down), Gi0/1(down), Gi0/2 (down)               |
| **SVI**             | Vlan40 (Unused), Vlan1 (down)                          |


3)VLAN40 default gateway: 10.0.2.254.

| Role            | Address           |
| --------------- | ----------------- |
| Virtual IP      | 10.0.2.254        |
| Active HSRP     | MLS6 - 10.0.2.252 |
| Standby HSRP    | MLS5 - 10.0.2.253 |
| HSRP Preemption | Enabled           |



4)VLAN40 Clients use remote DNS and DHCP servers (host 10.0.0.226 and 10.0.0.227).

5)VLAN40 Clients can't access VLAN10.

6)VLAN40 Special devices (10.0.2.250 (ASW5.floor2.com), 10.0.2.251 (DSW5.floor2.com) , 10.0.2.252 (MLS6's SVI) and 10.0.2.253 (MLS5's SVI) ) can access services such as (T)FTP, SSH, Syslog, DNS and NTP. 

7)The root bridge for the VLAN40 is DSW5 and configured by lowering STP priority for this VLAN.

8)The extended ACL is applied directly on the VLAN's interface on the MLS5/6. 
The direction is IN.

Extended IP access list VL40
    10 deny ip 10.0.2.128 0.0.0.127 10.0.1.0 0.0.0.127                       !denying VLAN10
    20 permit ip any any                                                     !permitting other traffic



----------------------------------------Testing of VLAN40---------------------------------------------------------------

1) CASE 1: Etherchannel and link status
Command: In privileged mode: Show etherchannel summary / show spanning-tree (to see if there are no fastEthernets) / show ip interface brief

Expected result: All needed interfaces should be UP/UP
(ASW5)
Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P) 

------------------------------------------------------------------------------------------------------------------------
2) CASE 2: Clarifying if CDP/LLDP is enabled:

Command: In Privileged mode: show cdp neighbors / show lldp neighbors

Expected result: 
(ASW5)
	1)Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
         	           S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
  Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
  ASW6         Por 3            148            S       2960        Fas 0/3
  ASW6         Por 3            148            S       2960        Fas 0/6
  ASW6         Por 3            148            S       2960        Por 3
  DSW6         Por 2            148            S       2960        Fas 0/2
  DSW6         Por 2            148            S       2960        Fas 0/5
  DSW6         Por 2            148            S       2960        Por 2
  DSW5         Por 1            149            S       2960        Fas 0/1
  DSW5         Por 1            149            S       2960        Fas 0/4
  DSW5         Por 1            149            S       2960        Por 1
	2) % LLDP is not enabled
------------------------------------------------------------------------------------------------------------------------
3) CASE 3: Spanning-tree testing

Command: In Privileged mode: show spanning-tree.
 
The expected result: The output depends on the device, but expected to see a DSW5 as a Root Bridge.
------------------------------------------------------------------------------------------------------------------------
4) CASE 4: Default Gateway and VLAN connectivity:
Command: 1) From PC try to ping another host in the same VLAN 
	 2) From any device in VLAN40 try to ping 10.0.2.250 , 10.0.2.251, .252, .253, .254 (VIP)

Expected result: Successful ping in each case.
------------------------------------------------------------------------------------------------------------------------
5) CASE 5: Test for Internet connection, DHCP and DNS responses. 

Command: 1)ipconfig /all or ipconfig /renew from any PC to check if DHCP messages is working.
         2)Ping internet.com (10.0.0.86) from any PC.
 
Expected results: 1)Successful DHCP responses and DNS responses. 
		  2)Ping successful responses is expected.
------------------------------------------------------------------------------------------------------------------------
6) CASE 6: Test for connection between VLANs:

Command: To test connection from any PC to the any other VLAN. ping pc1.vlanX.com , X - vlan number.

Expected results: Successful ping to the VLAN20,30,40,50,60
		  Unsuccessful ping to the VLAN10

------------------------------------------------------------------------------------------------------------------------
7)  CASE 7: Test to ping any remote server

Command: Ping 10.0.0.226 (domain server. Can be any other Server) from any PC. 

Expected result: Successful ping.
------------------------------------------------------------------------------------------------------------------------
8) CASE 8: SSH Connection from PC_ADMIN

Command: From the PC_ADMIN try 'SSH -l *login* *IP or domain name of the host* ' command in command prompt to check SSH connectivity to the ASW5,DSW5 or MLS5/6. 

Expected result: Successful connection with logging.
		 If the brute force attempt detected(5 tries within 45 seconds) on MLS5/6 - block for a 2 minutes.
		 SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.
------------------------------------------------------------------------------------------------------------------------
9)  CASE 9: NTP testing:

Command: From the either ASW5,DSW5 or MLS5/6 try to check NTP status. In Privileged mode: show ntp associations /show ntp status.

Expected example of output:
(ASW5)

address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
------------------------------------------------------------------------------------------------------------------------
10) CASE 10: File Transferring test

Command: From the either ASW5,DSW5 or MLS5/6, try to download any file by TFTP or FTP. The (T)FTP server is 10.0.0.31. 
In Privileged mode: copy flash: ftp: ; *filename*. 

Expected result: Successful file transfer. (Need to wait a few minutes)


------------------------------------------------------------------------------------------------------------------------
11) Case 11: Syslog testing
Command: From ASW5, DSW5 or MLS5/6 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).
	 In Global configuration mode
	 interface *any unused interface*  
         no shutdown !enabling it 
	 shutdown !disabling the interface to the default state

Expected result: The new trap would be created and syslog message will appear on the remote server.
------------------------------------------------------------------------------------------------------------------------






----------------------------------------Testing of VLAN45---------------------------------------------------------------

1) CASE 1: Etherchannel and link status

Command: In privileged mode: Show etherchannel summary / show spanning-tree (to see if there are no fastEthernets) / show ip interface brief

Expected result: All needed interfaces should be UP/UP
(ASW6)
Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P)

2) CASE 2: Clarifying if CDP/LLDP is enabled:

Command: In Privileged mode: show cdp neighbors / show lldp neighbors

Expected result: 
(DSW6)
	1)Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
ASW5         Por 2            169            S       2960        Fas 0/2
ASW5         Por 2            169            S       2960        Fas 0/5
ASW5         Por 2            169            S       2960        Por 2
MLS5         Gig 0/2          169                    3650        Gig 1/0/2
MLS6         Gig 0/1          169                    3650        Gig 1/0/1
ASW6         Por 1            169            S       2960        Fas 0/4
ASW6         Por 1            169            S       2960        Por 1
ASW6         Por 1            169            S       2960        Fas 0/1
	2) % LLDP is not enabled
------------------------------------------------------------------------------------------------------------------------
3) CASE 3: Spanning-tree testing

Command: In Privileged mode: show spanning-tree.
 
The expected result: The output depends on the device, but expected to see a DSW6 as a Root Bridge.
------------------------------------------------------------------------------------------------------------------------
4) CASE 4: Default Gateway and VLAN connectivity:
Command: 1) From PC try to ping another host in the same VLAN 
	 2) From any device in VLAN45 try to ping 10.0.2.122 , 10.0.2.123, .124, .125, .126 (VIP)

Expected result: Successful ping in each cases.
------------------------------------------------------------------------------------------------------------------------
5) CASE 5: Test for Internet connection, DHCP and DNS responses. 

Command: 1)ipconfig /all or ipconfig /renew from any PC to check if DHCP messages is working.
         2)Ping internet.com (10.0.0.86) from any PC.
 
Expected results: 1)Successful DHCP and DNS responses. 
		  2)Unsuccessful ping responses is expected.
------------------------------------------------------------------------------------------------------------------------
6) CASE 6: Test for connection between VLANs:

Command: To test connection from any PC to the any other VLAN. ping pc1.vlanX.com , X - vlan number.

Expected results: Successful ping to the VLAN20,40
		  Unsuccessful ping to the VLAN10,30,50,60

------------------------------------------------------------------------------------------------------------------------
7)  CASE 7: Test to ping any remote server

Command: Ping 10.0.0.226 (domain server. Can be any other Server) from any PC. 

Expected result: Unsuccessful ping.
------------------------------------------------------------------------------------------------------------------------
8) CASE 8: SSH Connection from PC_ADMIN

Command: From the PC_ADMIN try 'SSH -l *login* *IP or domain name of the host* ' command in command prompt to check SSH connectivity to the ASW6,DSW6 or MLS5/6. 

Expected result: Successful connection with logging.
		 If the brute force attempt detected(5 tries within 45 seconds) on MLS5/6 - block for a 2 minutes.
		 SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.
------------------------------------------------------------------------------------------------------------------------
9)  CASE 9: NTP testing:

Command: From the either ASW6,DSW6 or MLS5/6 try to check NTP status. In Privileged mode: show ntp associations /show ntp status.

Expected example of output:
(ASW5)

address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
------------------------------------------------------------------------------------------------------------------------
10) CASE 10: File Transferring test

Command: From the either ASW6,DSW6 or MLS5/6, try to download any file by TFTP or FTP. The (T)FTP server is 10.0.0.31. 
In Privileged mode: copy flash: ftp: ; *filename*. 

Expected result: Successful file transfer. (Need to wait a few minutes)


------------------------------------------------------------------------------------------------------------------------
11) Case 11: Syslog testing
Command: From ASW6, DSW6 or MLS5/6 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).
	 In Global configuration mode
	 interface *any unused interface*  
         no shutdown !enabling it 
	 shutdown !disabling the interface to the default state

Expected result: The new trap would be created and syslog message will appear on the remote server.
------------------------------------------------------------------------------------------------------------------------






----------------------------------------Testing of VLAN50------------------------------------------------------

1) CASE 1: Etherchannel and link status
Command: In privileged mode: Show etherchannel summary / show spanning-tree (to see if there are no fastEthernets) / show ip interface brief

Expected result: All needed interfaces should be UP/UP
(ASW1)
Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P) 

-------------------------------------------------------------------------------------------------------------------
2) CASE 2: Clarifying if CDP/LLDP is enabled:

Command: In Privileged mode: show cdp neighbors / show lldp neighbors

Expected result: 
(ASW2)
	1) Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
ASW1         Por 3            172            S       2960        Fas 0/3
ASW1         Por 3            172            S       2960        Fas 0/6
ASW1         Por 3            172            S       2960        Por 3
DSW1         Por 2            172            S       2960        Fas 0/2
DSW1         Por 2            172            S       2960        Fas 0/5
DSW1         Por 2            172            S       2960        Por 2
DSW2         Por 1            172            S       2960        Fas 0/4
DSW2         Por 1            172            S       2960        Fas 0/1
DSW2         Por 1            172            S       2960        Por 1
	2) Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
Device ID           Local Intf     Hold-time  Capability      Port ID
DSW1                Po2            120        B               Fa0/2
DSW1                Po2            120        B               Fa0/5
ASW1                Po3            120        B               Fa0/3
ASW1                Po3            120        B               Fa0/6
DSW2                Po1            120        B               Fa0/1
DSW2                Po1            120        B               Fa0/4

-------------------------------------------------------------------------------------------------------------------

3) CASE 3: Spanning-tree testing

Command: In Privileged mode: show spanning-tree.
 
The expected result: The output depends on the device, but expected to see a DSW2 as a Root Bridge.
-------------------------------------------------------------------------------------------------------------------

4) CASE 4: Default Gateway and VLAN connectivity:
Command: 1) From PC try to ping another host in the same VLAN 
	 2) From any device in VLAN50 try to ping 10.0.3.14 (VIP), 10.0.3.13, .12, .11 and 10.0.3.10.

Expected result: Successful ping in both cases.
-------------------------------------------------------------------------------------------------------------------

5) CASE 5: Test for Internet connection, DHCP and DNS responses. 

Command: 1)ipconfig /all or ipconfig /renew from any PC to check if DHCP messages is working.
         2)Ping internet.com (10.0.0.86) from any PC.
 
Expected results: 1)Successful DHCP responses and DNS responses. 
		  2)Ping successful responses is expected.
-------------------------------------------------------------------------------------------------------------------

6) CASE 6: Test for connection between VLANs:

Command: To test connection from any PC to the any other VLAN. ping pc1.vlanX.com , X - vlan number.

Expected results: Successful ping to the VLAN20,30,40,50,60 and to the host 10.0.0.229
		  Unsuccessful ping to the VLAN10,45 and network 10.0.0.224/27

-------------------------------------------------------------------------------------------------------------------

7)  CASE 7: Test to ping any remote server

Command: Ping 10.0.0.226 (domain server. Can be any other Server) from any PC. 

Expected result: Unsuccessful ping.
-------------------------------------------------------------------------------------------------------------------

8) CASE 8: SSH Connection from PC_ADMIN

Command: From the PC_ADMIN try 'SSH -l *login* *IP or domain name of the host* ' command in command prompt to check SSH connectivity to the ASW1,ASW2, DSW1  or MLS1/2. 

Expected result: Successful connection with logging.
		 If the brute force attempt detected(5 tries within 45 seconds) on MLS5/6 - block for a 2 minutes.
		 SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.
-------------------------------------------------------------------------------------------------------------------

9)  CASE 9: NTP testing:

Command: From the either ASW1,ASW2,DSW1  or MLS1/2 try to check NTP status. In Privileged mode: show ntp associations /show ntp status.

Expected example of output:
(ASW1)

address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
-------------------------------------------------------------------------------------------------------------------

10) CASE 10: File Transferring test

Command: From the either ASW1,ASW2, DSW1 or MLS1/2, try to download any file by TFTP or FTP. The (T)FTP server is 10.0.0.31. 
In Privileged mode: copy flash: ftp: ; *filename*. 

Expected result: Successful file transfer. (Need to wait a few minutes)


-------------------------------------------------------------------------------------------------------------------

11) Case 11: Syslog testing
Command: From ASW1, ASW2, DSW1 or MLS1/2 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).
	 In Global configuration mode
	 interface *any unused interface*  
         no shutdown !enabling it 
	 shutdown !disabling the interface to the default state

Expected result: The new trap would be created and syslog message will appear on the remote server.
-------------------------------------------------------------------------------------------------------------------






----------------------------------------Testing of VLAN60---------------------------------------------------------------

1) CASE 1: Etherchannel and link status
Command: In privileged mode: Show etherchannel summary / show spanning-tree (to see if there are no fastEthernets) / show ip interface brief

Expected result: All needed interfaces should be UP/UP
(ASW1)
Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P) 

------------------------------------------------------------------------------------------------------------------------
2) CASE 2: Clarifying if CDP/LLDP is enabled:

Command: In Privileged mode: show cdp neighbors / show lldp neighbors

Expected result: 
(DSW2)
	1) Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
MLS2         Gig 0/1          148                    3650        Gig 1/0/1
ASW1         Por 2            149            S       2960        Fas 0/5
ASW1         Por 2            149            S       2960        Por 2
ASW1         Por 2            149            S       2960        Fas 0/2
MLS1         Gig 0/2          148                    3650        Gig 1/0/2
ASW2         Por 1            149            S       2960        Fas 0/4
ASW2         Por 1            149            S       2960        Fas 0/1
ASW2         Por 1            149            S       2960        Por 1
	2) Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
Device ID           Local Intf     Hold-time  Capability      Port ID
ASW2                Po1            120        B               Fa0/1
ASW2                Po1            120        B               Fa0/4
ASW1                Po2            120        B               Fa0/2
ASW1                Po2            120        B               Fa0/5
MLS1                Gig0/2         120        R               Gig1/0/2
------------------------------------------------------------------------------------------------------------------------
3) CASE 3: Spanning-tree testing

Command: In Privileged mode: show spanning-tree.
 
The expected result: The output depends on the device, but expected to see a DSW2 as a Root Bridge.
------------------------------------------------------------------------------------------------------------------------
4) CASE 4: Default Gateway and VLAN connectivity:
Command: 1) From PC try to ping another host in the same VLAN 
	 2) From any device in VLAN60 try to ping 10.0.3.254 (VIP), 10.0.3.253, .252, .251 and 10.0.3.250.

Expected result: Successful ping in both cases.
------------------------------------------------------------------------------------------------------------------------
5) CASE 5: Test for Internet connection, DHCP and DNS responses. 

Command: 1)ipconfig /all or ipconfig /renew from any PC to check if DHCP messages is working.
         2)Ping internet.com (10.0.0.86) from any PC.
 
Expected results: 1)Successful DHCP responses and DNS responses. 
		  2)Unsuccessful ping responses is expected.
------------------------------------------------------------------------------------------------------------------------
6) CASE 6: Test for connection between VLANs:

Command: To test connection from any PC to the any other VLAN. ping pc1.vlanX.com , X - vlan number.

Expected results: Successful ping to the VLAN20,30,40,50,60 
		  Unsuccessful ping to the VLAN10,45

------------------------------------------------------------------------------------------------------------------------
7)  CASE 7: Test to ping any remote server

Command: Ping 10.0.0.226 (domain server. Can be any other Server) from any PC. 

Expected result: Successful ping.
------------------------------------------------------------------------------------------------------------------------
8) CASE 8: SSH Connection from PC_ADMIN

Command: From the PC_ADMIN try 'SSH -l *login* *IP or domain name of the host* ' command in command prompt to check SSH connectivity to the ASW1,ASW2, DSW2  or MLS1/2. 

Expected result: Successful connection with logging.
		 If the brute force attempt detected(5 tries within 45 seconds) on MLS5/6 - block for a 2 minutes.
		 SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.
------------------------------------------------------------------------------------------------------------------------
9)  CASE 9: NTP testing:

Command: From the either ASW1,ASW2,DSW2  or MLS1/2 try to check NTP status. In Privileged mode: show ntp associations /show ntp status.

Expected example of output:
(ASW2)

address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
------------------------------------------------------------------------------------------------------------------------
10) CASE 10: File Transferring test

Command: From the either ASW1,ASW2, DSW2 or MLS1/2, try to download any file by TFTP or FTP. The (T)FTP server is 10.0.0.31. 
In Privileged mode: copy flash: ftp: ; *filename*. 

Expected result: Successful file transfer. (Need to wait a few minutes)


------------------------------------------------------------------------------------------------------------------------
11) Case 11: Syslog testing
Command: From ASW1, ASW2, DSW2 or MLS1/2 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).
	 In Global configuration mode
	 interface *any unused interface*  
         no shutdown !enabling it 
	 shutdown !disabling the interface to the default state

Expected result: The new trap would be created and syslog message will appear on the remote server.
------------------------------------------------------------------------------------------------------------------------








----------------------------------------------------------CORE Testing--------------------------------------------------------

1) CASE 1: Interface and link status:

   Command: on the any device of the core, from Privileged exec mode: show ip interface brief
								      show cdp neighbors 
   Expected result of the second command: 
	(R1)
	Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
	                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
	Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
	R3           Gig 0/2          147            R       C2900       Gig 0/2
	MLS_L3_2     Gig 0/1          147                    3650        Gig 1/0/4
	R_B          Gig 0/1/0        147            R       C2900       Gig 0/1/0
	R2           Gig 0/0/0        147            R       C2900       Gig 0/0/0
	R_INT        Gig 0/0          148            R       C2900       Gig 0/0

------------------------------------------------------------------------------------------------------------------------------	

2) CASE 2: IP address and routing status:

   Command: on the any device of the core, or any device who can send ICMP ping requests without policy issues, try:
									ping *IP address*
									show ip route !from the device to check which route does it has								

   Expected results: Successful ping

------------------------------------------------------------------------------------------------------------------------------

3) CASE 3: OSPF clarifying:

   Commands: to clarify if OSPF works properly, run these commands only on the CORE devices or from SSH: 
	show ip protocol
	show ip ospf 
	show ip ospf neighbor 
	show ip ospf database
	show ip ospf interface
   Expected result of one of the commands:
	(R1)#show ip ospf neighbor

	Neighbor ID     Pri   State           Dead Time   Address         Interface
	3.0.0.0           1   FULL/BDR        00:00:39    10.0.0.78       GigabitEthernet0/1/0
	3.4.4.4           1   FULL/DR         00:00:38    10.0.0.62       GigabitEthernet0/1
	3.3.3.3           1   FULL/DR         00:00:39    10.0.0.69       GigabitEthernet0/2
	3.2.2.2           1   FULL/DR         00:00:39    10.0.0.73       GigabitEthernet0/0/0

--------------------------------------------------------------------------------------------------------------------------------

4) CASE 4: NTP status
	
   Commands: To check NTP status on CORE device,from Privileged exec mode use: show ntp status ,  show ntp associations
	
   Expected output: (R1)

   address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    5        16      375    1.00           0.00              0.24
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured

---------------------------------------------------------------------------------------------------------------------------------

5) CASE 5: Check SSH and Brute Force method:

   Commands: 1) From any non-PC_ADMIN, try: SSH -l *Username of the device* *IP Address*. 
	     2) From PC_ADMIN in Basement, try: SSH -l *Username of the device* *IP Address*
             3) From PC_ADMIN after valid logging, try to finish SSH connection by *exit*, and after try to login 5 times, 
             but with either wrong Username or Password

   Expected results: 1)(VLAN50 PC) SSH -l MLS3_ADMIN 10.0.0.33 

			% Connection refused by remote host

		     2) C:\>SSH -l MLS3_ADMIN 10.0.0.17

			Password: 


			----------------------------------------------------------------------------------
			ATTENTION! EACH ACTION IS LOGGED AND UNAUTHORIZED USAGE IS FORBIDDEN
			----------------------------------------------------------------------------------


			MLS3>en
			Password: 
			MLS3#
			
		     3)SSH -l MLS3_ADM 10.0.0.17

		       % Connection refused by remote host

--------------------------------------------------------------------------------------------------------------------------------

6) CASE 6: Check TFTP / FTP Status: From the any device, try to download any file by TFTP or FTP. The (T)FTP server is 10.0.0.31.
In Privileged mode: *copy flash: ftp:* / *copy flash: tftp:*
		    *filename*.

Expected result: Successful file transfer. (Need to wait a few minutes)

--------------------------------------------------------------------------------------------------------------------------------

7) CASE 7: Syslog checking:

   Command: From any device of the CORE, try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).
	    1) To create trap message, try to login successfully, with correct Username and Password.
	    2) To create trap message, try to login with wrong Username or Password.
   
   Expected results: In both cases, trap message will appear in Syslog server.



