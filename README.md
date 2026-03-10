
# Topology description:

<p>
    <ul>
        <li>This topology is a simplified 3 tier architecture, which connects 4 separate zones (Floor1, Floor2, Floor3,Internet(host 10.0.0.86, for better testing) and Basement).</li>
        <li>Each of the separate zone has its own ranges of IP addresses, VLANs, Access Policy, etc.</li>
        <li>Each Special device has its own IP and VLAN interface, which allows to connect to Layer 2 switches.</li>
    </ul>


</p>



### To run and test the topology, use the next steps:

<ol>
    <li>Download the files project_pkt1.pkt , Credentials.txt (optional, because CPT file already has logins and password for each device.Hovewer, if you want to have more readable and separate file with credentials, it is recommended to download it)</li> 
    <li> Launch the file with before downloaded Cisco Packet Tracer Application</li>
    <li> Perform testing by default commands. To test any device, click on its icon and choose the option you need.
    </li>  
</ol>


> Note: <p>The full documentation you can read in Full_Document_PKT1.txt and other related files in this repository. </p>

[Network Topology](Simple_Topology.png)



 

# WARNING: 

<ul>
<li>In this lab environment, SVIs are treated as trusted infrastructure interfaces.</li>
<li>Permitting all traffic sourced from SVI is a simplification to avoid over-complicating ACLs for control-plane and management protocols such as HSRP, DHCP relay, NTP, Syslog and SSH</li>
<li>Additional permit statements for SVI-sourced traffic are included to avoid potential issues with management-plane traffic in Packet Tracer. </li>
<li>The security section about DHCP Snooping, DAI and Port-Security is not covered in this document. To see more details about it, download Security_Features.txt </li>
</ul>


### DNS access model :

<ul> 
    <li>End hosts are allowed to perform DNS resolution (UDP/TCP port 53) everywhere.</li>
    <li>Direct access to DNS servers (ICMP, TCP other than 53) MAY be blocked by ACLs and policy requirements if needed.</li>
    <li>For example, if the policy will be about denying access to the remote servers means that UDP/TCP 53 kinds of messages still 
won`t be discarded. </li>
</ul>

### Special Devices meaning:

<ul> 
    <li>Special device means device which IP address statically assigned and should be considered as trusted. Usually it is Layer 2 Switches of Access and Distribution. They use VLAN default gateway as their own and belong to specific VLAN.</li>
    <li>To simplify, it is the devices with SVI, but aren't used in routing the packets. It allows to provide
SSH, NTP, FTP, Syslog and other useful features even on Layer 2 devices</li>

</ul>



### SVI and devices meanings:

<ul> 
    <li>SVI means Switch Virtual Interface,which is assigned to Access, Distribution and Multilayer Switches. </li>
    <li>MLS means MultiLayer Switch , ASW means Access Switch, DSW means Distribution Switch.</li>
    <li>Distribution Switches don't perform Layer 2 routing and was created for security features and to allow extension of the network in the future</li>
</ul>

### Banners and interface description:
<ul> 
    <li>Each device has its message of the day.</li>
    <li>Each interface has its brief description. </li>
</ul>

### Remote and local server meaning:


<ul> 
    <li>Remote server means that server is located in the other network.
Local server means that server is located in the same network</li>

</ul>




### Services and assigned IP Addresses:
<ul>
    <li>The basement provides services such as NTP, DNS, DHCP, 
FTP, TFTP, Syslog and SNMP(Partly, due to Cisco Packet Tracer simplification).<ul>
            <li>DNS Server IP Address: 10.0.0.226</li>
            <li>DHCP Server IP Address: 10.0.0.227</li>
            <li>NTP Server IP Address: 10.0.0.228</li>
            <li>PC_ADMIN (SSH Client) IP Address: 10.0.0.229</li>
            <li>Syslog Server IP Address: 10.0.0.230</li>
            <li>(T)FTP Server IP Address: 10.0.0.231</li>
            <li>SW1 (One of the SSH Servers) IP Address: 10.0.0.232</li>
                </ul>
    </li>

</ul>



|Device | IP Address | VLAN Interface|
| :-----| :--------: | :-------------|
| ASW1 | 10.0.3.10 | 50|
|***----***|***------***|**---**|
|ASW2 | 10.0.3.250 | 60|
|***----***|***------***|**---**|
|ASW3 | 10.0.1.122| 10|
|***----***|***------***|**---**|
|ASW4| 10.0.1.250| 20|
|***----***|***------***|**---**|
|ASW5| 10.0.2.250| 40|
|***----***|***------***|**---**|
|ASW6|10.0.2.122| 45|
|***----***|***------***|**---**|
| ***DSW1*** | 10.0.3.11 | 50|
|***----***|***------***|**---**|
| ***DSW2*** | 10.0.3.251 | 60|
|***----***|***------***|**---**|
|***DSW3*** | 10.0.1.123| 10|
|***----***|***------***|**---**|
|***DSW4***| 10.0.1.251| 20|
|***----***|***------***|**---**|
|***DSW5***| 10.0.2.251| 40|
|***----***|***------***|**---**|
|***DSW6***|10.0.2.123| 45|
|***----***|***------***|**---**|
|MLS1| 10.0.3.13|50|
|      |10.0.3.253|60|
|***----***|***------***|**---**|
|MLS2|10.0.3.12|50|
|    |10.0.3.252|60|
|***----***|***------***|**---**|
|MLS3|10.0.1.125|10 |
| |10.0.1.252| 20|
|***----***|***------***|**---**|
|MLS4| 10.0.1.124 |10|
| |10.0.1.253 | 20|
|***----***|***------***|**---**|
|MLS5 | 10.0.2.253 | 40|
| | 10.0.2.60 | 30 |
| | 10.0.2.125| 45|
|***----***|***------***|**---**|
| MLS6| 10.0.2.61|30|
| | 10.0.2.252| 40|
| | 10.0.2.124| 45|
|***----***|***------***|**---**|
|***SW1***| 10.0.0.232| 1|
<p><pre>








</pre></p>




|Device     | Interfaces  | IP Address | Network Addr., /30|     
|:----------|:-----------:|:----------:|:------------------|
|           |   G0/0/0    | 10.0.0.74  |  10.0.0.72        |
|           |   G0/1/0    | 10.0.0.77  |  10.0.0.76        |
| ***R1***  |   G0/0      | 10.0.0.85  |  10.0.0.84        |
|           |   G0/1      | 10.0.0.61  |  10.0.0.60        |
|           |   G0/2      | 10.0.0.70  |  10.0.0.68        |
| --------  |-------------|------------|-------------------|
|           |   G0/0/0    | 10.0.0.73  |  10.0.0.72        |
|           |   G0/1/0    | 10.0.0.53  |  10.0.0.52        |
| ***R2***  |   G0/0      | 10.0.0.65  |  10.0.0.64        |
|           |   G0/1      | 10.0.0.58  |  10.0.0.56        | 
|-----------|-------------|------------|-------------------|
|           |   G0/0/0    | 10.0.0.81  |  10.0.0.80        |
|           |   G0/1/0    | 10.0.0.50  |  10.0.0.48        |
| ***R3***  |   G0/0      | 10.0.0.66  |  10.0.0.64        |
|           |   G0/2      | 10.0.0.69  |  10.0.0.68        | 
|-----------|-------------|------------|-------------------|
|           |   G0/0/0    | 10.0.0.82  |  10.0.0.80        |
| ***R_B*** |   G0/1/0    | 10.0.0.78  |  10.0.0.76        |      
|-----------|-------------|------------|-------------------|
|***R_INT***|   G0/0      | 10.0.0.86  |  10.0.0.84        |
|-----------|-------------|------------|-------------------|
|           |   G1/0/3    | 10.0.0.10  |  10.0.0.8         |
| ***MLS1***|   G1/1/4    | 10.0.0.41  |  10.0.0.40        |
|-----------|-------------|------------|-------------------|
|           |   G1/0/3    | 10.0.0.6   |  10.0.0.4         |
| ***MLS2***|   G1/1/4    | 10.0.0.46  |  10.0.0.44        |
|-----------|-------------|------------|-------------------|
|           |   G1/0/3    | 10.0.0.33  |  10.0.0.32        |
| ***MLS3***|   G1/1/4    | 10.0.0.17  |  10.0.0.16        |
|-----------|-------------|------------|-------------------|
|           |   G1/0/3    | 10.0.0.37  |  10.0.0.36        |
| ***MLS4***|   G1/1/4    | 10.0.0.14  |  10.0.0.12        |
|-----------|-------------|------------|-------------------|
|           |   G1/0/3    | 10.0.0.25  |  10.0.0.24        |
| ***MLS5***|   G1/1/4    | 10.0.0.1   |  10.0.0.0         |
|-----------|-------------|------------|-------------------|
|           |   G1/0/3    | 10.0.0.29  |  10.0.0.28        |
| ***MLS6***|   G1/1/4    | 10.0.0.21  |  10.0.0.20        |
|-----------|-------------|------------|-------------------|
|           |   G1/0/1    | 10.0.0.34  |  10.0.0.32        |
|           |   G1/0/2    | 10.0.0.38  |  10.0.0.36        |
|           |   G1/0/3    | 10.0.0.30  |  10.0.0.28        |
|           |   G1/0/4    | 10.0.0.26  |  10.0.0.24        |
|***MLS_L3_1***|   G1/1/1   | 10.0.0.42 |  10.0.0.40       |
|           |   G1/1/2    | 10.0.0.45  |  10.0.0.44        |
|           |   G1/1/3    | 10.0.0.49  |  10.0.0.48        |
|           |   G1/1/4    | 10.0.0.54  |  10.0.0.52        |
|-----------|-------------|------------|-------------------|
|           |   G1/0/1    | 10.0.0.9   |  10.0.0.8         |
|           |   G1/0/2    | 10.0.0.5   |  10.0.0.4         |
|           |   G1/0/3    | 10.0.0.57  |  10.0.0.56        |
|           |   G1/0/4    | 10.0.0.62  |  10.0.0.60        |
|***MLS_L3_2***|   G1/1/1    | 10.0.0.2   |  10.0.0.0         |
|           |   G1/1/2    | 10.0.0.22  |  10.0.0.20        |
|           |   G1/1/3    | 10.0.0.18  |  10.0.0.16        |
|           |   G1/1/4    | 10.0.0.13  |  10.0.0.12        |
|-----------|-------------|------------|-------------------|





# Brief Tests for each VLAN(1,10-60) and services

### Testing of VLAN1

<ol>
<li> <p>CASE: LINK STATUS</p>
        <p>Command:
        <p> <ul>
                <li>In privileged mode: <strong>show ip interface brief</strong>/ <strong>show spanning-tree</strong></li>
            </ul>
            </p>
            </p>
        <p>Expected result: 
            <p>
            <ul>
                  <li>All needed interfaces should be UP/UP</li>
            </ul>
            </p>
        </p>
    </li>

<li> <p>CASE: Clarifying if CDP/LLDP enabled:</p>
    <P>Command: In Privileged mode:
        <p>
            <ol>
                <li><strong> show cdp neighbors </strong></li>
                <li><strong>show lldp neighbors</strong></li>
            </ol>
        </p>
    </p>
    <p>Expected result: (SW1)
    <p>1.</p>
 	<pre>Capability Codes: 
R - Router, T - Trans Bridge, B - Source Route Bridge

S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
R_B          Gig 0/1          147            R       C2900       Gig 0/1
</pre>
<p>2.</p>
<pre>Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
Device ID           Local Intf     Hold-time  Capability      Port ID
R_B                 Gig0/1         120        R               Gig0/1

Total entries displayed: 1</pre> 
</p>
    </li>

<li> <p>CASE: Spanning-tree testing</p>
    <p>Command: In Privileged mode: 
        <p>
            <ul>
                <li><strong> show spanning-tree</strong></li>
            </ul>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The output depends on the device, but expected to see a SW1 as a Root Bridge.</li>
            </ul>
        </p>
    </p>
</li>

<li> <p>CASE: Default Gateway and VLAN connectivity.</p>
    <P>Command: <ol>
    <li>From PC try to ping another host in the same VLAN.</li>
    <li>From any device in VLAN1 try: <strong>ping 10.0.0.225</strong> .</li>
    </ol>
    </p>
    <p>Expected result: 
       <p>
            <ul>
                <li>Successful ping in both cases</li>
            </ul>
       </p>                 
   </p>
</li>

<li> <p>CASE: Test for Internet connection, DHCP and DNS responses.</p>
    <P>Command: 
        <p> 
            <ol> 
    <li> ipconfig /renew from any PC to check if DHCP messages is working.
    </li>
    <li> nslookup internet.com </li>
    <li>Ping internet.com (10.0.0.86) from any PC.</li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li> Unsuccessful DHCP responses because of static config.</li>
                <li> Successful DNS responses with IP of 10.0.0.86 ;</li>
                <li> Successful ping  and DNS responses is expected.</li>
            </ol>
        </p> 
    </p>
</li>

<li> <p>CASE: Test for connection between VLANs:</p>
    <P>Command: To test connection from any PC to the any other VLAN: <strong> ping pc1.vlanX.com </strong>, X - vlan number.</p>
    <p>Expected result: 
        <ol>
            <li>Successful ping to the VLAN20,30,40,50,60;</li>
            <li>Unsuccessful ping to the VLAN10,45;</li>
        </ol>
    </p>
</li>


<li> <p>CASE: Test to ping any remote server</p>
    <P>Command: In command prompt: 
        <p>
            <ul>
                <li><strong>ping 10.0.0.226 </strong> (This is domain server. Can be any other Server) from PC_ADMIN</li>
            </ul>
        </p> 
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li>Successful ping.</li>
            </ol>
        </p>
    </p>
</li>


<li> <p>CASE: SSH Connection from PC_ADMIN </p>
    <P>Command: From the PC_ADMIN try 'SSH -l *login* *IP or domain name of the host* ' command in command prompt to check SSH connectivity to the SW1, R_B.</p>
    <p>Expected result: 
        <ul>
             <li>Successful connection with logging.</li>
             <li>If the brute force attempt detected(5 tries within 45 seconds) on R_B - block for a 2 minutes.</li>
             <li>SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.</li>
        </ul>
     </p>
</li>


<li> <p>CASE: NTP testing</p>
    <P>Command: From the either SW1 or R_B try to check NTP status. In Privileged mode: <strong>show ntp associations</strong> /<strong>show ntp status</strong>.</p>
    <p>Expected result: 
        <p>(R_B)
            <p>
                <pre>address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
                </pre>
            </p>
        </p>
    </p>
    </li>


<li> <p>CASE: File Transferring test</p>
    <P>Command: From the either SW1 or R_B, try to download any file by TFTP or FTP.    <p>The (T)FTP server is 10.0.0.31.</p>
            <p>In Privileged mode:<strong> copy flash: ftp: </strong> ; *filename*.</p>
    </p>
    <p>Expected result: 
        <p>
             <ul>
                <li>Successful file transfer. (Need to wait a few minutes)</li>
             </ul>
        </p>
    </p>
    </li>


<li> <p>CASE: Syslog testing </p>
<P>Command: 
    <p>
        <ol>
            <li>From SW1 or R_B try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).</li>
            <li>Go to the Global configuration mode, using following commands:
                <ol>
                    <li>interface *any unused interface*</li>
                    <li>no shutdown !enabling it</li>
                    <li>shutdown !disabling the interface to the default state</li>
                </ol>
            </li>
        </ol>
    </p> 
</p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The new trap would be created and syslog message will appear on the remote server</li>
            </ul>
        </p>
    </p>
    </li>

</ol>









### Testing of VLAN10

<ol>
<li> <p>CASE:  Etherchannel and link status</p>
        <p>Command: (DSW3)
        <p> <ul>
                <li>In privileged mode, use following commands:</li>
                <li><strong>Show etherchannel summary</strong></li>
                <li><strong>show ip interface brief</strong></li>
            </ul>
            </p>
            </p>
        <p>Expected result: 
            <p>
            <ul>
                  <li>All needed interfaces should be UP/UP</li>
                  <li><p>
                        <pre>Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/3(P)       
2      Po3(SU)           LACP   Fa0/2(P) Fa0/4(P) 
                        </pre>
                        <p>
> note: 
            <p>If Po2 appears on the DSW3 with status (SD); - , it may be an bug of old configuration. So, don`t pay attention to it
However, on other Switches it is not okay if some unknown port-channels are shown</p> 
                        </p>
                  </p></li>
            </ul>
            </p>
        </p>
    </li>

<li> <p>CASE: Clarifying if CDP/LLDP is disabled:
 </p>
    <P>Command: 
        <p>
            <ol>
                <li>In Privileged mode:</li>
                <li><strong> show cdp neighbors </strong></li>
                <li><strong>show lldp neighbors</strong></li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <ol>
            <li>% CDP is not enabled</li>
            <li>% LLDP is not enabled</li>
        </ol>
    </p>
</li>

<li> <p>CASE: Spanning-tree testing
</p>
    <p>Command:  
        <p>
            <ul>
                <li>In Privileged mode:</li>
                <li><strong> show spanning-tree</strong></li>
            </ul>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The output depends on the device, but expected to see a DSW3 as the Root Bridge.</li>
            </ul>
        </p>
    </p>
</li>

<li> <p>CASE: Default Gateway and VLAN connectivity.</p>
    <P>Command: <ol>
    <li>From PC try to ping another host in the same VLAN.</li>
    <li>From any device in VLAN10 try: </li>
    <li><strong>ping 10.0.0.126 (VIP)</strong></li>
    <li><strong>ping 10.0.0.125 </strong></li>
    <li><strong>ping 10.0.0.124</strong></li>
    </ol>
    </p>
    <p>Expected result: 
       <p>
            <ul>
                <li>Successful ping in each cases</li>
            </ul>
       </p>                 
   </p>
</li>

<li> <p>CASE: Test for Internet connection, DHCP and DNS responses.</p>
    <P>Command: 
        <p> 
            <ol> 
    <li> <strong> ipconfig /renew </strong> from any PC to check if DHCP messages is working.
    </li>
    <li> <strong> nslookup internet.com </strong> </li>
    <li><strong>Ping internet.com</strong>(10.0.0.86) from any PC.</li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li> Successful DHCP responses</li>
                <li> Successful DNS responses with IP of 10.0.0.86 ;</li>
                <li> Successful ping  and DNS responses is expected.</li>
            </ol>
        </p> 
    </p>
</li>

<li> <p>CASE: Test for connection between VLANs:</p>
    <P>Command: To test connection from any PC to the any other VLAN: <strong> ping pc1.vlanX.com </strong>, X - vlan number.</p>
    <p>Expected result: 
        <ol>
            <li>Successful ping to the VLAN10;</li>
            <li>Unsuccessful ping to the VLAN20, 30, 40, 50, 60;</li>
        </ol>
    </p>
</li>


<li> <p>CASE: Test to ping any remote server</p>
    <P>Command:  
        <p>
            <ul>
                <li>In command prompt use the following command:</li>
                <li>Use <strong>ping 10.0.0.226 </strong> (This is domain server. Can be any other Server) from any PC.</li>
            </ul>
        </p> 
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li>Unccessful ping.</li>
            </ol>
        </p>
    </p>
</li>


<li> <p>CASE: SSH Connection from PC_ADMIN </p>
    <P>Command: 
        <p>
          <ul>
              <li>From the PC_ADMIN in command prompt try following commands:</li>
               <li><strong>SSH -l *login* *IP or domain name of the host*  </strong> ,to check SSH connectivity to the ASW3,DSW3, MLS3 or MLS4.</li>
         </ul>
        </p>
    </p>
    <p>Expected result: 
        <ul>
             <li>Successful connection with logging.</li>
             <li>If the brute force attempt detected(5 tries within 45 seconds) on MLS3/4 - block for a 2 minutes.</li>
             <li>SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.</li>
        </ul>
     </p>
</li>


<li> <p>CASE: NTP testing</p>
    <P>Command:
        <ol>
            <li> From the either SW1 or R_B try to check NTP status. In Privileged mode:</li>
            <li><strong>show ntp associations</strong></li>
            <li><strong>show ntp status</strong></li>
        </ol>
    </p>
    <p>Expected result: 
        <p>(ASW3)
            <p>
                <pre>address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
                </pre>
            </p>
        </p>
    </p>
    </li>


<li> <p>CASE: File Transferring test</p>
    <P>Command: From the either ASW3,DSW3 or MLS3/4, try to download any file by TFTP or FTP.   <p>The (T)FTP server is 10.0.0.31.</p>
            <p>In Privileged mode:<strong> copy flash: ftp: </strong> ; *filename*.</p>
    </p>
    <p>Expected result: 
        <p>
             <ul>
                <li>Successful file transfer. (Need to wait a few minutes)</li>
             </ul>
        </p>
    </p>
    </li>


<li> <p>CASE: Syslog testing </p>
<P>Command: 
    <p>
        <ol>
            <li>From ASW3, DSW3 or MLS3/4 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).</li>
            <li>Go to the Global configuration mode, using following commands:
                <ol>
                    <li>interface *any unused interface*</li>
                    <li>no shutdown !enabling it</li>
                    <li>shutdown !disabling the interface to the default state</li>
                </ol>
            </li>
        </ol>
    </p> 
</p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The new trap would be created and syslog message will appear on the remote server</li>
            </ul>
        </p>
    </p>
    </li>

</ol>


### Testing of VLAN20


<ol>
<li> <p>CASE:  Etherchannel and link status</p>
        <p>Command:
            <p>
                <ul>
                    <li>In privileged mode, use following commands:</li>
                    <li><strong>Show etherchannel summary</strong></li>
                    <li><strong></strong>show ip interface brief</li>
                </ul>
            </p>
        </p>
        <p>Expected result: (DSW4)
            <p>
            <ul>
                  <li>All needed interfaces should be UP/UP</li>
            </ul>
<p>
                        <pre>Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/3(P)       
2      Po3(SU)           LACP   Fa0/2(P) Fa0/4(P) 
                        </pre>
</p>
            </p>
        </p>
    </li>

<li> <p>CASE: Clarifying if CDP/LLDP is disabled:
 </p>
    <P>Command: 
        <p>
            <ol>
                <li>In Privileged mode:</li>
                <li><strong> show cdp neighbors </strong></li>
                <li><strong>show lldp neighbors</strong></li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <ol>
            <li>% CDP is not enabled</li>
            <li>% LLDP is not enabled</li>
        </ol>
    </p>
</li>

<li> <p>CASE: Spanning-tree testing
</p>
    <p>Command:  
        <p>
            <ul>
                <li>In Privileged mode use the following command:</li>
                <li><strong> show spanning-tree</strong></li>
            </ul>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ul>
                <li> The output depends on the device, but expected to see a DSW4 as a Root Bridge.</li>
            </ul>
        </p>
    </p>
</li>

<li> <p>CASE: Default Gateway and VLAN connectivity.</p>
    <P>Command: <ol>
    <li>From PC try to ping another host in the same VLAN.</li>
    <li>From any device in VLAN20 try: </li>
    <li><strong>ping 10.0.1.254 (VIP)</strong></li>
    <li><strong>ping 10.0.1.253 </strong></li>
    <li><strong>ping 10.0.1.252</strong></li>
    </ol>
    </p>
    <p>Expected result: 
       <p>
            <ul>
                <li>Successful ping in each cases</li>
            </ul>
       </p>                 
   </p>
</li>

<li> <p>CASE: Test for Internet connection, DHCP and DNS responses.</p>
    <P>Command: 
        <p> 
            <ol> 
    <li><strong> ipconfig /renew </strong>from any PC to check if DHCP messages is working.
    </li>
    <li><strong>nslookup internet.com</strong></li>
    <li><strong>Ping internet.com</strong> (10.0.0.86) from any PC.</li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li> Successful DHCP responses</li>
                <li> Successful DNS responses with IP of 10.0.0.86 ;</li>
                <li> Unsuccessful ping is expected.</li>
            </ol>
        </p> 
    </p>
</li>

<li> <p>CASE: Test for connection between VLANs:</p>
    <P>Command: To test connection from any PC to the any other VLAN: <strong> ping pc1.vlanX.com </strong>, X - vlan number.</p>
    <p>Expected result: 
        <ol>
            <li>Successful ping to the VLAN20, 40, 50, 60;</li>
            <li>Unsuccessful ping to the VLAN10, 30;</li>
        </ol>
    </p>
</li>


<li> <p>CASE: Test to ping any remote server</p>
    <P>Command: 
        <p>
            <ul>
                <li>In command prompt use the following command:</li>
                <li>Use <strong>ping 10.0.0.226 </strong> (This is domain server. Can be any other Server) from any PC.</li>
            </ul>
        </p> 
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li>Successful ping.</li>
            </ol>
        </p>
    </p>
</li>


<li> <p>CASE: SSH Connection from PC_ADMIN </p>
    <P>Command: 
        <p>
          <ul>
              <li>From the PC_ADMIN in command prompt try following commands:</li>
               <li><strong>SSH -l *login* *IP or domain name of the host*  </strong> ,to check SSH connectivity to the ASW4,DSW4, MLS3 or MLS4.</li>
         </ul>
        </p>
    </p>
    <p>Expected result: 
        <ul>
             <li>Successful connection with logging.</li>
             <li>If the brute force attempt detected(5 tries within 45 seconds) on MLS3/4 - block for a 2 minutes.</li>
             <li>SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.</li>
        </ul>
     </p>
</li>


<li> <p>CASE: NTP testing</p>
    <P>Command:
        <ol>
            <li> From the either ASW4,DSW4 or MLS3/4 try to check NTP status. In Privileged mode:</li>
            <li><strong>show ntp associations</strong></li>
            <li><strong>show ntp status</strong></li>
        </ol>
    </p>
    <p>Expected result: 
        <p>(ASW4)
            <p>
                <pre>address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
                </pre>
            </p>
        </p>
    </p>
    </li>


<li> <p>CASE: File Transferring test</p>
    <P>Command: From the either ASW4,DSW4 or MLS3/4, try to download any file by TFTP or FTP.   <p>The (T)FTP server is 10.0.0.31.</p>
            <p>In Privileged mode:<strong> copy flash: ftp: </strong> ; *filename*.</p>
    </p>
    <p>Expected result: 
        <p>
             <ul>
                <li>Successful file transfer. (Need to wait a few minutes)</li>
             </ul>
        </p>
    </p>
    </li>


<li> <p>CASE: Syslog testing </p>
<P>Command: 
    <p>
        <ol>
            <li>From ASW4, DSW4 or MLS3/4 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).</li>
            <li>Go to the Global configuration mode, using following commands:
                <ol>
                    <li>interface *any unused interface*</li>
                    <li>no shutdown !enabling it</li>
                    <li>shutdown !disabling the interface to the default state</li>
                </ol>
            </li>
        </ol>
    </p> 
</p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The new trap would be created and syslog message will appear on the remote server</li>
            </ul>
        </p>
    </p>
    </li>

</ol>


### Testing of VLAN30


<ol>
<li> <p>CASE:  Etherchannel and link status</p>
        <p>Command:
            <p>
                <ul>
                    <li>In privileged mode, use following commands:</li>
                    <li><strong>Show etherchannel summary</strong></li>
                    <li><strong></strong>show ip interface brief</li>
                </ul>
            </p>
        </p>
        <p>Expected result: (ASW5)
            <p>
            <ul>
                  <li>All needed interfaces should be UP/UP</li>
            </ul>
<p>
                        <pre>Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P) 
                        </pre>
</p>
            </p>
        </p>
    </li>

<li> <p>CASE: Clarifying if CDP/LLDP is disabled:
 </p>
    <P>Command: 
        <p>
            <ol>
                <li>In Privileged mode:</li>
                <li><strong> show cdp neighbors </strong></li>
                <li><strong>show lldp neighbors</strong></li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <P>(ASW6)</P>
        <ol>
            <li><pre>Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
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
            </pre></li>
            <li>% LLDP is not enabled</li>
        </ol>
    </p>
</li>

<li> <p>CASE: Spanning-tree testing
</p>
    <p>Command:  
        <p>
            <ul>
                <li>In Privileged mode use the following command:</li>
                <li><strong> show spanning-tree</strong></li>
            </ul>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ul>
                <li> The output depends on the device, but expected to see a DSW5 as a Root Bridge.</li>
            </ul>
        </p>
    </p>
</li>

<li> <p>CASE: Default Gateway and VLAN connectivity.</p>
    <P>Command: <ol>
    <li>From PC try to ping another host in the same VLAN.</li>
    <li>From any device in VLAN30 try: </li>
    <li><strong>ping 10.0.2.62 (VIP)</strong></li>
    <li><strong>ping 10.0.2.61 </strong></li>
    <li><strong>ping 10.0.2.60</strong></li>
    </ol>
    </p>
    <p>Expected result: 
       <p>
            <ul>
                <li>Successful ping in each cases</li>
            </ul>
       </p>                 
   </p>
</li>

<li> <p>CASE: Test for Internet connection, DHCP and DNS responses.</p>
    <P>Command: 
        <p> 
            <ol> 
    <li><strong> ipconfig /renew </strong>from any PC to check if DHCP messages is working.
    </li>
    <li><strong>nslookup internet.com</strong></li>
    <li><strong>Ping internet.com</strong> (10.0.0.86) from any PC.</li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li> Successful DHCP responses</li>
                <li> Successful DNS responses with IP of 10.0.0.86 ;</li>
                <li> Successful ping is expected.</li>
            </ol>
        </p> 
    </p>
</li>

<li> <p>CASE: Test for connection between VLANs:</p>
    <P>Command: To test connection from any PC to the any other VLAN: <strong> ping pc1.vlanX.com </strong>, X - vlan number.</p>
    <p>Expected result: 
        <ol>
            <li>Successful ping to the VLAN30,40,50,60;</li>
            <li>Unsuccessful ping to the VLAN10,20;</li>
        </ol>
    </p>
</li>


<li> <p>CASE: Test to ping any remote server</p>
    <P>Command: 
        <p>
            <ul>
                <li>In command prompt use the following command:</li>
                <li>Use <strong>ping 10.0.0.226 </strong> (This is domain server. Can be any other Server) from any PC.</li>
            </ul>
        </p> 
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li>Successful ping.</li>
            </ol>
        </p>
    </p>
</li>


<li> <p>CASE: SSH Connection from PC_ADMIN </p>
    <P>Command: 
        <p>
          <ul>
              <li>From the PC_ADMIN in command prompt try following commands:</li>
               <li><strong>SSH -l *login* *IP or domain name of the host*  </strong> ,to check SSH connectivity to the ASW5, MLS5 or MLS6.</li>
         </ul>
        </p>
    </p>
    <p>Expected result: 
        <ul>
             <li>Successful connection with logging.</li>
             <li>If the brute force attempt detected(5 tries within 45 seconds) on MLS5/6 - block for a 2 minutes.</li>
             <li>SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.</li>
        </ul>
     </p>
</li>


<li> <p>CASE: NTP testing</p>
    <P>Command:
        <ol>
            <li> From the either ASW5 or MLS5/6 try to check NTP status. In Privileged mode:</li>
            <li><strong>show ntp associations</strong></li>
            <li><strong>show ntp status</strong></li>
        </ol>
    </p>
    <p>Expected result: 
        <p>(ASW5)
            <p>
                <pre>address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
                </pre>
            </p>
        </p>
    </p>
    </li>


<li> <p>CASE: File Transferring test</p>
    <P>Command: From the either ASW5 or MLS5/6, try to download any file by TFTP or FTP.
    <p>The (T)FTP server is 10.0.0.31.</p>
            <p>In Privileged mode:<strong> copy flash: ftp: </strong> ; *filename*.</p>
    </p>
    <p>Expected result: 
        <p>
             <ul>
                <li>Successful file transfer. (Need to wait a few minutes)</li>
             </ul>
        </p>
    </p>
    </li>


<li> <p>CASE: Syslog testing </p>
<P>Command: 
    <p>
        <ol>
            <li>From  ASW5 or MLS5/6 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).</li>
            <li>Go to the Global configuration mode, using following commands:
                <ol>
                    <li>interface *any unused interface*</li>
                    <li>no shutdown !enabling it</li>
                    <li>shutdown !disabling the interface to the default state</li>
                </ol>
            </li>
        </ol>
    </p> 
</p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The new trap would be created and syslog message will appear on the remote server</li>
            </ul>
        </p>
    </p>
    </li>




</ol>

### Testing of VLAN40



<ol>
<li> <p>CASE:  Etherchannel and link status</p>
        <p>Command:
            <p>
                <ul>
                    <li>In privileged mode, use following commands:</li>
                    <li><strong>Show etherchannel summary</strong></li>
                    <li><strong></strong>show ip interface brief</li>
                </ul>
            </p>
        </p>
        <p>Expected result: (ASW5)
            <p>
            <ul>
                  <li>All needed interfaces should be UP/UP</li>
            </ul>
<p>
                        <pre>Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P) 
                        </pre>
</p>
            </p>
        </p>
    </li>

<li> <p>CASE: Clarifying if CDP/LLDP is disabled:
 </p>
    <P>Command: 
        <p>
            <ol>
                <li>In Privileged mode:</li>
                <li><strong> show cdp neighbors </strong></li>
                <li><strong>show lldp neighbors</strong></li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <P>(ASW6)</P>
        <ol>
            <li><pre>Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
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
            </pre></li>
            <li>% LLDP is not enabled</li>
        </ol>
    </p>
</li>

<li> <p>CASE: Spanning-tree testing
</p>
    <p>Command:  
        <p>
            <ul>
                <li>In Privileged mode use the following command:</li>
                <li><strong> show spanning-tree</strong></li>
            </ul>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ul>
                <li> The output depends on the device, but expected to see a DSW5 as a Root Bridge.</li>
            </ul>
        </p>
    </p>
</li>

<li> <p>CASE: Default Gateway and VLAN connectivity.</p>
    <P>Command: <ol>
    <li>From PC try to ping another host in the same VLAN.</li>
    <li>From any device in VLAN40 try: </li>
    <li><strong>ping 10.0.2.254 (VIP)</strong></li>
    <li><strong>ping 10.0.2.253 </strong></li>
    <li><strong>ping 10.0.2.252</strong></li>
    <li><strong>ping 10.0.2.251 </strong></li>
    <li><strong>ping 10.0.2.250</strong></li>
    </ol>
    </p>
    <p>Expected result: 
       <p>
            <ul>
                <li>Successful ping in each case</li>
            </ul>
       </p>                 
   </p>
</li>

<li> <p>CASE: Test for Internet connection, DHCP and DNS responses.</p>
    <P>Command: 
        <p> 
            <ol> 
    <li><strong> ipconfig /renew </strong>from any PC to check if DHCP messages is working.
    </li>
    <li><strong>nslookup internet.com</strong></li>
    <li><strong>Ping internet.com</strong> (10.0.0.86) from any PC.</li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li> Successful DHCP responses</li>
                <li> Successful DNS responses with IP of 10.0.0.86 ;</li>
                <li> Successful ping is expected.</li>
            </ol>
        </p> 
    </p>
</li>

<li> <p>CASE: Test for connection between VLANs:</p>
    <P>Command: To test connection from any PC to the any other VLAN: <strong> ping pc1.vlanX.com </strong>, X - vlan number.</p>
    <p>Expected result: 
        <ol>
            <li>Successful ping to the VLAN20,30,40,50,60;</li>
            <li>Unsuccessful ping to the VLAN10;</li>
        </ol>
    </p>
</li>


<li> <p>CASE: Test to ping any remote server</p>
    <P>Command: 
        <p>
            <ul>
                <li>In command prompt use the following command:</li>
                <li>Use <strong>ping 10.0.0.226 </strong> (This is domain server. Can be any other Server) from any PC.</li>
            </ul>
        </p> 
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li>Successful ping.</li>
            </ol>
        </p>
    </p>
</li>


<li> <p>CASE: SSH Connection from PC_ADMIN </p>
    <P>Command: 
        <p>
          <ul>
              <li>From the PC_ADMIN in command prompt try following commands:</li>
               <li><strong>SSH -l *login* *IP or domain name of the host*  </strong> ,to check SSH connectivity to the ASW5,DSW5, MLS5 or MLS6.</li>
         </ul>
        </p>
    </p>
    <p>Expected result: 
        <ul>
             <li>Successful connection with logging.</li>
             <li>If the brute force attempt detected(5 tries within 45 seconds) on MLS5/6 - block for a 2 minutes.</li>
             <li>SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.</li>
        </ul>
     </p>
</li>


<li> <p>CASE: NTP testing</p>
    <P>Command:
        <ol>
            <li> From the either ASW5,DSW5 or MLS5/6 try to check NTP status. In Privileged mode:</li>
            <li><strong>show ntp associations</strong></li>
            <li><strong>show ntp status</strong></li>
        </ol>
    </p>
    <p>Expected result: 
        <p>(ASW5)
            <p>
                <pre>address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
                </pre>
            </p>
        </p>
    </p>
    </li>


<li> <p>CASE: File Transferring test</p>
    <P>Command: From the either ASW5,DSW5 or MLS5/6, try to download any file by TFTP or FTP.
    <p>The (T)FTP server is 10.0.0.31.</p>
            <p>In Privileged mode:<strong> copy flash: ftp: </strong> ; *filename*.</p>
    </p>
    <p>Expected result: 
        <p>
             <ul>
                <li>Successful file transfer. (Need to wait a few minutes)</li>
             </ul>
        </p>
    </p>
    </li>


<li> <p>CASE: Syslog testing </p>
<P>Command: 
    <p>
        <ol>
            <li>From  ASW5, DSW5 or MLS5/6 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).</li>
            <li>Go to the Global configuration mode, using following commands:
                <ol>
                    <li>interface *any unused interface*</li>
                    <li>no shutdown !enabling it</li>
                    <li>shutdown !disabling the interface to the default state</li>
                </ol>
            </li>
        </ol>
    </p> 
</p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The new trap would be created and syslog message will appear on the remote server</li>
            </ul>
        </p>
    </p>
    </li>

</ol>

### Testing of VLAN45



<ol>
<li> <p>CASE:  Etherchannel and link status</p>
        <p>Command:
            <p>
                <ul>
                    <li>In privileged mode, use following commands:</li>
                    <li><strong>Show etherchannel summary</strong></li>
                    <li><strong></strong>show ip interface brief</li>
                </ul>
            </p>
        </p>
        <p>Expected result: (ASW6)
            <p>
            <ul>
                  <li>All needed interfaces should be UP/UP</li>
            </ul>
<p>
                        <pre>Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P) 
                        </pre>
</p>
            </p>
        </p>
    </li>

<li> <p>CASE: Clarifying if CDP/LLDP is disabled:
 </p>
    <P>Command: 
        <p>
            <ol>
                <li>In Privileged mode:</li>
                <li><strong> show cdp neighbors </strong></li>
                <li><strong>show lldp neighbors</strong></li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <P>(DSW6)</P>
        <ol>
            <li><pre>Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
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
            </pre></li>
            <li>% LLDP is not enabled</li>
        </ol>
    </p>
</li>

<li> <p>CASE: Spanning-tree testing
</p>
    <p>Command:  
        <p>
            <ul>
                <li>In Privileged mode use the following command:</li>
                <li><strong> show spanning-tree</strong></li>
            </ul>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ul>
                <li> The output depends on the device, but expected to see a DSW6 as a Root Bridge.</li>
            </ul>
        </p>
    </p>
</li>

<li> <p>CASE: Default Gateway and VLAN connectivity.</p>
    <P>Command: <ol>
    <li>From PC try to ping another host in the same VLAN.</li>
    <li>From any device in VLAN40 try: </li>
    <li><strong>ping 10.0.2.126 (VIP)</strong></li>
    <li><strong>ping 10.0.2.125 </strong></li>
    <li><strong>ping 10.0.2.124 </strong></li>
    <li><strong>ping 10.0.2.123 </strong></li>
    <li><strong>ping 10.0.2.122 </strong></li>
    </ol>
    </p>
    <p>Expected result: 
       <p>
            <ul>
                <li>Successful ping in each case</li>
            </ul>
       </p>                 
   </p>
</li>

<li> <p>CASE: Test for Internet connection, DHCP and DNS responses.</p>
    <P>Command: 
        <p> 
            <ol> 
    <li><strong> ipconfig /renew </strong>from any PC to check if DHCP messages is working.
    </li>
    <li><strong>nslookup internet.com</strong></li>
    <li><strong>Ping internet.com</strong> (10.0.0.86) from any PC.</li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li> Successful DHCP responses</li>
                <li> Successful DNS responses with IP of 10.0.0.86 ;</li>
                <li> Unsuccessful ping is expected.</li>
            </ol>
        </p> 
    </p>
</li>

<li> <p>CASE: Test for connection between VLANs:</p>
    <P>Command: To test connection from any PC to the any other VLAN: <strong> ping pc1.vlanX.com </strong>, X - vlan number.</p>
    <p>Expected result: 
        <ol>
            <li>Successful ping to the VLAN20,40;</li>
            <li>Unsuccessful ping to the VLAN10,30,50,60;</li>
        </ol>
    </p>
</li>


<li> <p>CASE: Test to ping any remote server</p>
    <P>Command: 
        <p>
            <ul>
                <li>In command prompt use the following command:</li>
                <li>Use <strong>ping 10.0.0.226 </strong> (This is domain server. Can be any other Server) from any PC.</li>
            </ul>
        </p> 
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li>Unsuccessful ping.</li>
            </ol>
        </p>
    </p>
</li>


<li> <p>CASE: SSH Connection from PC_ADMIN </p>
    <P>Command: 
        <p>
          <ul>
              <li>From the PC_ADMIN in command prompt try following commands:</li>
               <li><strong>SSH -l *login* *IP or domain name of the host*  </strong> ,to check SSH connectivity to the ASW6,DSW6, MLS5 or MLS6.</li>
         </ul>
        </p>
    </p>
    <p>Expected result: 
        <ul>
             <li>Successful connection with logging.</li>
             <li>If the brute force attempt detected(5 tries within 45 seconds) on MLS5/6 - block for a 2 minutes.</li>
             <li>SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.</li>
        </ul>
     </p>
</li>


<li> <p>CASE: NTP testing</p>
    <P>Command:
        <ol>
            <li> From the either ASW6,DSW6 or MLS5/6 try to check NTP status. In Privileged mode:</li>
            <li><strong>show ntp associations</strong></li>
            <li><strong>show ntp status</strong></li>
        </ol>
    </p>
    <p>Expected result: 
        <p>(ASW6)
            <p>
                <pre>address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
                </pre>
            </p>
        </p>
    </p>
    </li>


<li> <p>CASE: File Transferring test</p>
    <P>Command: From the either ASW6,DSW6 or MLS5/6, try to download any file by TFTP or FTP.
    <p>The (T)FTP server is 10.0.0.31.</p>
            <p>In Privileged mode:<strong> copy flash: ftp: </strong> ; *filename*.</p>
    </p>
    <p>Expected result: 
        <p>
             <ul>
                <li>Successful file transfer. (Need to wait a few minutes)</li>
             </ul>
        </p>
    </p>
    </li>


<li> <p>CASE: Syslog testing </p>
<P>Command: 
    <p>
        <ol>
            <li>From  ASW6, DSW6 or MLS5/6 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).</li>
            <li>Go to the Global configuration mode, using following commands:
                <ol>
                    <li>interface *any unused interface*</li>
                    <li>no shutdown !enabling it</li>
                    <li>shutdown !disabling the interface to the default state</li>
                </ol>
            </li>
        </ol>
    </p> 
</p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The new trap would be created and syslog message will appear on the remote server</li>
            </ul>
        </p>
    </p>
    </li>




</ol>





### Testing of VLAN50


<ol>
<li> <p>CASE:  Etherchannel and link status</p>
        <p>Command:
            <p>
                <ul>
                    <li>In privileged mode, use following commands:</li>
                    <li><strong>Show etherchannel summary</strong></li>
                    <li><strong></strong>show ip interface brief</li>
                </ul>
            </p>
        </p>
        <p>Expected result: (ASW1)
            <p>
            <ul>
                  <li>All needed interfaces should be UP/UP</li>
            </ul>
<p>
                        <pre>Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P) 
                        </pre>
</p>
            </p>
        </p>
    </li>

<li> <p>CASE: Clarifying if CDP/LLDP is disabled:
 </p>
    <P>Command: 
        <p>
            <ol>
                <li>In Privileged mode:</li>
                <li><strong> show cdp neighbors </strong></li>
                <li><strong>show lldp neighbors</strong></li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <P>(ASW2)</P>
        <ol>
            <li><pre>Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
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
            </pre></li>
            <li><pre>Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
Device ID           Local Intf     Hold-time  Capability      Port ID
DSW1                Po2            120        B               Fa0/2
DSW1                Po2            120        B               Fa0/5
ASW1                Po3            120        B               Fa0/3
ASW1                Po3            120        B               Fa0/6
DSW2                Po1            120        B               Fa0/1
DSW2                Po1            120        B               Fa0/4

</pre></li>
        </ol>
    </p>
</li>

<li> <p>CASE: Spanning-tree testing
</p>
    <p>Command:  
        <p>
            <ul>
                <li>In Privileged mode use the following command:</li>
                <li><strong> show spanning-tree</strong></li>
            </ul>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ul>
                <li> The output depends on the device, but expected to see a DSW2 as a Root Bridge.</li>
            </ul>
        </p>
    </p>
</li>

<li> <p>CASE: Default Gateway and VLAN connectivity.</p>
    <P>Command: <ol>
    <li>From PC try to ping another host in the same VLAN.</li>
    <li>From any device in VLAN45 try: </li>
    <li><strong>ping 10.0.3.14 (VIP)</strong></li>
    <li><strong>ping 10.0.3.13 </strong></li>
    <li><strong>ping 10.0.3.12 </strong></li>
    <li><strong>ping 10.0.3.11 </strong></li>
    <li><strong>ping 10.0.3.10 </strong></li>
    </ol>
    </p>
    <p>Expected result: 
       <p>
            <ul>
                <li>Successful ping in each case</li>
            </ul>
       </p>                 
   </p>
</li>

<li> <p>CASE: Test for Internet connection, DHCP and DNS responses.</p>
    <P>Command: 
        <p> 
            <ol> 
    <li><strong> ipconfig /renew </strong>from any PC to check if DHCP messages is working.
    </li>
    <li><strong>nslookup internet.com</strong></li>
    <li><strong>Ping internet.com</strong> (10.0.0.86) from any PC.</li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li> Successful DHCP responses</li>
                <li> Successful DNS responses with IP of 10.0.0.86 ;</li>
                <li> Successful ping is expected.</li>
            </ol>
        </p> 
    </p>
</li>

<li> <p>CASE: Test for connection between VLANs:</p>
    <P>Command: To test connection from any PC to the any other VLAN: <strong> ping pc1.vlanX.com </strong>, X - vlan number.</p>
    <p>Expected result: 
        <ol>
            <li>Successful ping to the VLAN20,30,40,50,60 and to the host 10.0.0.229;</li>
            <li>Unsuccessful ping to the VLAN10,45 and network 10.0.0.224/27;</li>
        </ol>
    </p>
</li>


<li> <p>CASE: Test to ping any remote server</p>
    <P>Command: 
        <p>
            <ul>
                <li>In command prompt use the following command:</li>
                <li>Use <strong>ping 10.0.0.226 </strong> (This is domain server. Can be any other Server) from any PC.</li>
            </ul>
        </p> 
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li>Unsuccessful ping.</li>
            </ol>
        </p>
    </p>
</li>


<li> <p>CASE: SSH Connection from PC_ADMIN </p>
    <P>Command: 
        <p>
          <ul>
              <li>From the PC_ADMIN in command prompt try following commands:</li>
               <li><strong>SSH -l *login* *IP or domain name of the host*  </strong> ,to check SSH connectivity to the ASW1,ASW2, DSW1, MLS1 or MLS2.</li>
         </ul>
        </p>
    </p>
    <p>Expected result: 
        <ul>
             <li>Successful connection with logging.</li>
             <li>If the brute force attempt detected(5 tries within 45 seconds) on MLS1/2 - block for a 2 minutes.</li>
             <li>SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.</li>
        </ul>
     </p>
</li>


<li> <p>CASE: NTP testing</p>
    <P>Command:
        <ol>
            <li> From the either ASW1,ASW2, DSW1 or MLS1/2 try to check NTP status. In Privileged mode:</li>
            <li><strong>show ntp associations</strong></li>
            <li><strong>show ntp status</strong></li>
        </ol>
    </p>
    <p>Expected result: 
        <p>(ASW1)
            <p>
                <pre>address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
                </pre>
            </p>
        </p>
    </p>
    </li>


<li> <p>CASE: File Transferring test</p>
    <P>Command: From the either ASW1, ASW2, DSW1 or MLS1/2, try to download any file by TFTP or FTP.
    <p>The (T)FTP server is 10.0.0.31.</p>
            <p>In Privileged mode:<strong> copy flash: ftp: </strong> ; *filename*.</p>
    </p>
    <p>Expected result: 
        <p>
             <ul>
                <li>Successful file transfer. (Need to wait a few minutes)</li>
             </ul>
        </p>
    </p>
    </li>


<li> <p>CASE: Syslog testing </p>
<P>Command: 
    <p>
        <ol>
            <li>From  ASW1, ASW2, DSW1 or MLS1/2 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).</li>
            <li>Go to the Global configuration mode, using following commands:
                <ol>
                    <li>interface *any unused interface*</li>
                    <li>no shutdown !enabling it</li>
                    <li>shutdown !disabling the interface to the default state</li>
                </ol>
            </li>
        </ol>
    </p> 
</p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The new trap would be created and syslog message will appear on the remote server</li>
            </ul>
        </p>
    </p>
    </li>


</ol>

### Testing of VLAN60

<ol>
<li> <p>CASE:  Etherchannel and link status</p>
        <p>Command:
            <p>
                <ul>
                    <li>In privileged mode, use following commands:</li>
                    <li><strong>Show etherchannel summary</strong></li>
                    <li><strong></strong>show ip interface brief</li>
                </ul>
            </p>
        </p>
        <p>Expected result: (ASW1)
            <p>
            <ul>
                  <li>All needed interfaces should be UP/UP</li>
            </ul>
<p>
                        <pre>
Number of channel-groups in use: 3
Number of aggregators:           3

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           LACP   Fa0/1(P) Fa0/4(P) 
2      Po2(SU)           LACP   Fa0/2(P) Fa0/5(P) 
3      Po3(SU)           LACP   Fa0/3(P) Fa0/6(P) 
                        </pre>
</p>
            </p>
        </p>
    </li>

<li> <p>CASE: Clarifying if CDP/LLDP is disabled:
 </p>
    <P>Command: 
        <p>
            <ol>
                <li>In Privileged mode:</li>
                <li><strong> show cdp neighbors </strong></li>
                <li><strong>show lldp neighbors</strong></li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <P>(DSW2)</P>
        <ol>
            <li><pre>Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
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

</pre></li>
            <li><pre>Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
Device ID           Local Intf     Hold-time  Capability      Port ID
ASW2                Po1            120        B               Fa0/1
ASW2                Po1            120        B               Fa0/4
ASW1                Po2            120        B               Fa0/2
ASW1                Po2            120        B               Fa0/5
MLS1                Gig0/2         120        R               Gig1/0/2
</pre></li>
        </ol>
    </p>
</li>

<li> <p>CASE: Spanning-tree testing
</p>
    <p>Command:  
        <p>
            <ul>
                <li>In Privileged mode use the following command:</li>
                <li><strong> show spanning-tree</strong></li>
            </ul>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ul>
                <li> The output depends on the device, but expected to see a DSW2 as a Root Bridge.</li>
            </ul>
        </p>
    </p>
</li>

<li> <p>CASE: Default Gateway and VLAN connectivity.</p>
    <P>Command: <ol>
    <li>From PC try to ping another host in the same VLAN.</li>
    <li>From any device in VLAN60 try: </li>
    <li><strong>ping 10.0.3.254 (VIP)</strong></li>
    <li><strong>ping 10.0.3.253 </strong></li>
    <li><strong>ping 10.0.3.252 </strong></li>
    <li><strong>ping 10.0.3.251 </strong></li>
    <li><strong>ping 10.0.3.250 </strong></li>
    </ol>
    </p>
    <p>Expected result: 
       <p>
            <ul>
                <li>Successful ping in each case</li>
            </ul>
       </p>                 
   </p>
</li>

<li> <p>CASE: Test for Internet connection, DHCP and DNS responses.</p>
    <P>Command: 
        <p> 
            <ol> 
    <li><strong> ipconfig /renew </strong>from any PC to check if DHCP messages is working.
    </li>
    <li><strong>nslookup internet.com</strong></li>
    <li><strong>Ping internet.com</strong> (10.0.0.86) from any PC.</li>
            </ol>
        </p>
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li> Successful DHCP responses</li>
                <li> Successful DNS responses with IP of 10.0.0.86 ;</li>
                <li> Unsuccessful ping is expected.</li>
            </ol>
        </p> 
    </p>
</li>

<li> <p>CASE: Test for connection between VLANs:</p>
    <P>Command: To test connection from any PC to the any other VLAN: <strong> ping pc1.vlanX.com </strong>, X - vlan number.</p>
    <p>Expected result: 
        <ol>
            <li>Successful ping to the VLAN20,30,40,50,60;</li>
            <li>Unsuccessful ping to the VLAN10,45;</li>
        </ol>
    </p>
</li>


<li> <p>CASE: Test to ping any remote server</p>
    <P>Command: 
        <p>
            <ul>
                <li>In command prompt use the following command:</li>
                <li>Use <strong>ping 10.0.0.226 </strong> (This is domain server. Can be any other Server) from any PC.</li>
            </ul>
        </p> 
    </p>
    <p>Expected result: 
        <p>
            <ol>
                <li>Successful ping.</li>
            </ol>
        </p>
    </p>
</li>


<li> <p>CASE: SSH Connection from PC_ADMIN </p>
    <P>Command: 
        <p>
          <ul>
              <li>From the PC_ADMIN in command prompt try following commands:</li>
               <li><strong>SSH -l *login* *IP or domain name of the host*  </strong> ,to check SSH connectivity to the ASW1,ASW2, DSW2, MLS1 or MLS2.</li>
         </ul>
        </p>
    </p>
    <p>Expected result: 
        <ul>
             <li>Successful connection with logging.</li>
             <li>If the brute force attempt detected(5 tries within 45 seconds) on MLS1/2 - block for a 2 minutes.</li>
             <li>SSH may be done only from PC_ADMIN, due to VTY-ACCESS-CLASS ACL.</li>
        </ul>
     </p>
</li>


<li> <p>CASE: NTP testing</p>
    <P>Command:
        <ol>
            <li> From the either ASW1,ASW2, DSW2 or MLS1/2 try to check NTP status. In Privileged mode:</li>
            <li><strong>show ntp associations</strong></li>
            <li><strong>show ntp status</strong></li>
        </ol>
    </p>
    <p>Expected result: 
        <p>(ASW2)
            <p>
                <pre>address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    6        16      37     1.00           1.00              0.12
                </pre>
            </p>
        </p>
    </p>
    </li>


<li> <p>CASE: File Transferring test</p>
    <P>Command: From the either ASW1, ASW2, DSW2 or MLS1/2, try to download any file by TFTP or FTP.
    <p>The (T)FTP server is 10.0.0.31.</p>
            <p>In Privileged mode:<strong> copy flash: ftp: </strong> ; *filename*.</p>
    </p>
    <p>Expected result: 
        <p>
             <ul>
                <li>Successful file transfer. (Need to wait a few minutes)</li>
             </ul>
        </p>
    </p>
    </li>


<li> <p>CASE: Syslog testing </p>
<P>Command: 
    <p>
        <ol>
            <li>From  ASW1, ASW2, DSW2 or MLS1/2 try to test if the Syslog UDP messages can go to the remote Syslog server (10.0.0.230).</li>
            <li>Go to the Global configuration mode, using following commands:
                <ol>
                    <li>interface *any unused interface*</li>
                    <li>no shutdown !enabling it</li>
                    <li>shutdown !disabling the interface to the default state</li>
                </ol>
            </li>
        </ol>
    </p> 
</p>
    <p>Expected result: 
        <p>
            <ul>
                <li>The new trap would be created and syslog message will appear on the remote server</li>
            </ul>
        </p>
    </p>
    </li>

</ol>





### CORE Testing

<ol>
    <list>
        <p>CASE: Interface and link status.</p>
        <P>Command:
            <p>
                <ol>
                    <li>Choose and click on the any device of the core</li>
                    <li>Enter to the privileged exec mode, then input following commands:
                    </li>
                    <li><strong>Show ip interface brief</strong></li>
                    <li><strong>Show cdp neighbors</strong></li>
                </ol>
            </p>
        </P>
        <p>Expected result of the second command: (R1)
            <p>
            <ul>
                <li><pre>Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
	                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
	Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
	R3           Gig 0/2          147            R       C2900       Gig 0/2
	MLS_L3_2     Gig 0/1          147                    3650        Gig 1/0/4
	R_B          Gig 0/1/0        147            R       C2900       Gig 0/1/0
	R2           Gig 0/0/0        147            R       C2900       Gig 0/0/0
	R_INT        Gig 0/0          148            R       C2900       Gig 0/0
</pre></li>
            </ul>
            </p>
        </p>
    </list>

<list>
    <p>CASE: </p>
    <p>Command: 
        <ul>
            <list>Choose one of the any device of the core, or any device who can send ICMP ping requests without policy issues.</list>
            <list>Enter to the Privileged EXEC mode and try to ping other device of the CORE, by using next commands:</list>
            <list><strong>ping *IP address* </strong></list>
            <list><strong>show ip route !to check if routes works properly.</strong></list>
        </ul>    
    </p>
    <p>Expected result:
        <p>
            <ul>Successful ping responses in both ways.</ul>
        </p>
    </p>

</list>

<list>
<p>CASE: OSPF clarifying</p>
<p>Commands:  
    <p> To clarify if OSPF works properly, run these commands directly on the CORE devices or from SSH:
        <ol>
        <li><strong>show ip protocol</strong></li>
        <li><strong>show ip ospf</strong></li>
        <li><strong>show ip ospf neighbor</strong></li>
        <li><strong>show ip ospf database</strong></li>
        <li><strong>show ip ospf interface</strong></li>
        <li><strong>show ip route</strong></li>
        </ol>
    </p>
</p>
<p>Expected results: 
    <p>
        <ul>
            <li><pre>(R1)#show ip ospf neighbor
            
	Neighbor ID     Pri   State           Dead Time   Address         Interface
	3.0.0.0           1   FULL/BDR        00:00:39    10.0.0.78       GigabitEthernet0/1/0
	3.4.4.4           1   FULL/DR         00:00:38    10.0.0.62       GigabitEthernet0/1
	3.3.3.3           1   FULL/DR         00:00:39    10.0.0.69       GigabitEthernet0/2
	3.2.2.2           1   FULL/DR         00:00:39    10.0.0.73       GigabitEthernet0/0/0
</pre></li>
        </ul>
    </p>
</p>

</list>


<list>
<p>CASE: NTP Status</p>

<p>Commands: 
    <p>
        <ol>
        <li>Choose any Core device, enter to the privileged exec mode and use on of the commands:</li>
        <li><strong>show ntp status</strong></li>
        <li><strong>show ntp associations</strong></li>
        </ol>
    </p>
</p>
<p>Expected results: 
    <p> (R1)
        <ul>
            <li><pre>address         ref clock       st   when     poll    reach  delay          offset            disp
*~10.0.0.228    127.127.1.1     1    5        16      375    1.00           0.00              0.24
 * sys.peer, # selected, + candidate, - outlyer, x falseticker, ~ configured

</pre></li>
        </ul>
    </p>
</p>

</list>


<list>
<p>CASE: Check SSH and Brute Force method</p>
<p>Commands: 
    <p>From any non-PC_ADMIN host, try next commands:
        <ol>
        <li><strong> SSH -l *Username of the device* *IP Address*</strong></li>
        <li>From PC_ADMIN in basement, try <strong> 'Username of the device* *IP Address* ' </strong></li>
        <li> After Valid logging, try to finish SSH connection by *exit*, or wait for a 5 minutes, and after try to login again, but with wrong credentials.  
        </ol>
    </p>
</p>
<p>Expected results: 
    <p>
        <ol>
            <li><pre>(VLAN50 PC) SSH -l MLS3_ADMIN 10.0.0.33 
			% Connection refused by remote host
            </pre>
            </li>   
            <li><pre>C:\>SSH -l MLS3_ADMIN 10.0.0.17

			Password: 


			----------------------------------------------------------------------------------
			ATTENTION! EACH ACTION IS LOGGED AND UNAUTHORIZED USAGE IS FORBIDDEN
			----------------------------------------------------------------------------------


			MLS3>en
			Password: 
			MLS3#
</pre></li>
            <li><pre>C:\>SSH -l MLS3_ADM 10.0.0.17

		       % Connection refused by remote host
</pre></li>
        </ol>
    </p>
</p>

</list>


<list>
<p>CASE: Check TFTP / FTP Status</p>
<p>Commands: 
    <p>From any device, try to download any file by TFTP or FTP. The
    (T)FTP server is 10.0.0.31.
        <ol>
        <li>Enter to the privileged exec mode</li>
        <li>Run <strong> copy flash: ftp:</strong> or <strong>*copy flash: tftp:*</strong></li>
        <li>Choose and input the needed filename</li>
        </ol>
    </p>
</p>
<p>Expected results: 
    <p>
        <ul>
            <li>Successful file transfer. (Need to wait a few minutes)</li>
        </ul>
    </p>
</p>

</list>


<list>
<p>CASE: Syslog checking</p>
<p>Commands: 
    <p>From any device of the CORE, try to test if UDP messages can go to the remote Syslog server (10.0.0.230).
        <ol>
        <li>Create a trap message by successful/unsuccessful logging</li>
        </ol>
    </p>
</p>
<p>Expected results: 
    <p>
        <ul>
            <li>In each cases, trap message will appear in Syslog server.</li>
        </ul>
    </p>
</p>

</list>



</ol>