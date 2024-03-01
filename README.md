# Rocking-your-Network
<h3>Phase 1: "I'd like to Teach the World to ping"</h3>
<ins>Instructions</ins>
  
You have been provided a list of network assets belonging to RockStar Corp. Ping the network assets for only the Hollywood office.  

IMPORTANT - You will need to run phase 1 from your local machine, and NOT the web lab.  

Determine the IPs for the Hollywood office and run ping against the IP ranges in order to determine which IP(s) are accepting connections.  

RockStar Corp doesn't want any of its servers, even if they are up, to indicate that they are accepting connections.  

Use ping [IP Address] and ignore any results that say "Request timed out."  

If any of the IP addresses send back a reply, press Ctrl+C to stop sending requests.  

Enter the relevant information for Phase 1 in your submission file, including the ping command(s) used, a summary of the results (including which IPs accept connections and which do not), and which OSI layer(s) your findings involve.  

<ins>Deliverable</ins>

1. Command(s) used to run ping against the IP ranges:  

 &emsp;&emsp;**ping 15.199.95.91  
 &emsp;&emsp;-doesn’t allow a connection  
 &emsp;&emsp;ping 15.199.94.91  
 &emsp;&emsp;-dones’t allow a connection  
 &emsp;&emsp;ping 203.0.113.32  
 &emsp;&emsp;-doesn’t allow a connection  
 &emsp;&emsp;ping 161.35.96.20   
 &emsp;&emsp;-allows a connection  
 &emsp;&emsp;ping 192.0.2.0  
 &emsp;&emsp;-doesn’t allow a connection**  

2. Summarize the results of the ping command(s):

&emsp;&emsp;**Only the Hollywood Application Server 1 is accepting connections**

3. List of IPs responding to echo requests:

&emsp;&emsp;**161.35.96.20**

4. Explain which OSI layer(s) your findings involve:

&emsp;&emsp;**Layer 3 - networking layer**

5. Mitigation recommendations

&emsp;&emsp;**Since the IP address 161.35.96.20 is accepting pings and is active we want to make sure that all of their servers are unreachable. They stated that they did not want any of their IP addresses to respond to any requests so they are not targets through an open port.**

<h3>Phase 2: "Some SYN for Nothin'"</h3>
<ins>Instructions</ins>  

Using your findings from Phase 1, determine which ports are open.

Run a SYN scan against the IP(s) accepting connections. Follow the instructions in the SYN Scan Instructions section below.

Using the results of the SYN scan, determine which ports are accepting connections.

<ins>Deliverable</ins>
1. Which ports are open on the RockStar Corp server?

&emsp;&emsp;**Port 22 SSH**

2. Which OSI layer do SYN scans run on? Explain how you determined which layer.

&emsp;&emsp;**Layer 4 - transport. This is in layer 4 because the protocols TCP and UDP establish the connection using the three-way handshake.**

3. Mitigation suggestions

&emsp;&emsp;**RockStar Corp needs to close port 22, leaving no access to SSH into the system.**

<h3>Phase 3: "I Feel a DNS Change Comin' On"</h3>
<ins>Instructions</ins>  

Using your findings from Phase 2, determine whether you can access the server(s) that accept connections.

RockStar Corp typically uses the same default username and password for most of its servers, so try this first:

&emsp;&emsp;Username: jimi

&emsp;&emsp;Password: hendrix

Try to figure out which port or service is used for remote system administration. Then, using these credentials, attempt to log in to the IP(s) that responded to pings in Phase 1.

RockStar Corp recently reported that it is unable to access rollingstone.com in the Hollywood office. Sometimes when they try to access the website, a different, unusual website comes up.

While logged into the RockStar server from the previous step, determine whether something was modified on this system that affects viewing rollingstone.com in the browser. When you successfully find the configuration file, record the entry that is set to rollingstone.com.

Terminate your SSH session to the rollingstone.com server, and use nslookup to determine the real domain of the IP address that you found in the previous step.

<ins>Deliverable</ins>
1. Summarize your findings about why access to rollingstone.com is not working as expected from the RockStar Corp Hollywood office:

&emsp;&emsp;**From my findings, I’ve come to the conclusion that the open port 22 allowed an attacker to change the IP address of the system**

2. Command used to query Domain Name System records:

&emsp;&emsp;**nslookup 98.137.246.8**

3. Domain name findings:

&emsp;&emsp;**name = unknown.yahoo.com**

4. Explain what OSI layer DNS runs on:

&emsp;&emsp;**This takes place in layer 7 - application**

5. Mitigation suggestions

&emsp;&emsp;**RockStar Corp would need to change their IP address back to their original address and then close port 22 to prevent further attacks.**

<h3>Phase 4: "ShARP Dressed Man"</h3>
<ins>Instructions</ins>  

Within the RockStar server that you SSH'd into and in the same directory as the configuration file from Phase 3, the hacker left a note about where they stored some packet captures.

View the file to find out where to recover the packet captures.

These packets were captured from the activity in the Hollywood office.

Use Wireshark to analyze this PCAP file and determine whether there was any suspicious activity that could be attributed to a hacker.

Record and identify your findings (e.g., OSI layers, protocols, IP addresses, and MAC addresses).

<ins>Deliverable</ins>
1. Name of file containing packets:

&emsp;&emsp;**packetcaptureinfo.txt  
&emsp;&emsp;Within the /etc directory   
&emsp;&emsp;https://drive.google.com/file/d/1ic-CFFGrbruloYrWaw3PvT71elTkh3eF/view?usp=sharing**  

2. ARP findings identifying the hacker’s MAC address:

&emsp;&emsp;**arp.opcode  
&emsp;&emsp;00:0c:29:1d:b3:b1  
&emsp;&emsp;Shows duplicate IP address was detected**  

3. HTTP findings, including the message from the hacker:

&emsp;&emsp;**Http.request.method == “POST”  
&emsp;&emsp;Found a message stating verbatim, “Hi Got The Blues Corp! This is a hacker that works at Rock Star Corp. Rock Star has left port 22, SSH open if you want to hack in. For 1 Milliion Dollars I will provide you the user and password!”**  

4. Explain the OSI layers for HTTP and ARP.

&emsp;&emsp;**HTTP - Layer 7 - Application  
&emsp;&emsp;ARP - Layer 2 - Data Link**  

5. Mitigation suggestions

&emsp;&emsp;**RockStar Corp should close port 22 and revert to the original IP address and make it unreachable from this point on.
There should also be a new policy in place to change all of the passwords on the system so there are no leaks AND everyone should have their own login information to begin with. There should be no crossing of information between regular users and admin users. This would eliminate the SSH problems.
Suspicious activity on logs should be monitored more closely.**
