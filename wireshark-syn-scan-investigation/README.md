 Overview

This investigation analyzes a packet capture (PCAP) file using Wireshark to identify suspicious network activity. The primary objective was to determine whether reconnaissance or scanning behavior was present and to extract key indicators such as source, targets, ports, and exposed services.

 Objectives
Identify the source of suspicious activity
Determine scan start time (UTC)
Identify targeted hosts
Enumerate scanned ports
Detect exposed services (e.g., RDP)

 Key Findings
Category	Finding
Attacker IP	192.168.1.212
First SYN (UTC)	2024-02-02 14:40:36
First SYN/ACK Packet	#26
Targets	4 hosts
Ports Scanned	21 unique ports
RDP Open	4 hosts (3389)

 Methodology
1. Identify Scan Activity
Filtered for SYN packets to isolate scan attempts:
tcp.flags.syn == 1 && tcp.flags.ack == 0

2. Identify Attacker
Observed the IP with the highest volume of outbound SYN packets:
ip.src == 192.168.1.212

3. Determine Scan Start Time
Switched to UTC format
Identified the first SYN packet in filtered results

4. Identify Targets
Used:
Statistics → Endpoints (IPv4)
Statistics → Conversations (IPv4)
Filtered view:
ip.src == 192.168.1.212 && tcp.flags.syn == 1

5. Count Unique Ports
Used:
Statistics → Endpoints (TCP)
Filtered view:
ip.src == 192.168.1.212 && tcp.flags.syn == 1 && tcp.flags.ack == 0

6. Identify Open Services (RDP)
Filtered for SYN/ACK responses on port 3389:
tcp.port == 3389 && tcp.flags.syn == 1 && tcp.flags.ack == 1

 Analysis
The traffic pattern is consistent with a TCP SYN (half-open) scan, a common reconnaissance technique used to identify open ports without completing full TCP handshakes.

Key observations:
The scan originated from an internal host, suggesting:
Possible compromise and lateral movement, or
Controlled lab/testing activity
The attacker probed 21 ports across 4 hosts, indicating broad service enumeration
All targeted systems had RDP exposed, which presents a high-risk lateral movement vector
⚠️ Risks Identified
🔓 Multiple systems exposing RDP (port 3389)
🌐 Internal reconnaissance behavior
🤖 Likely automated scanning activity
🔁 Potential precursor to lateral movement or brute-force attacks

 Recommendations
Restrict RDP access:
Limit to VPN or specific IP ranges
Implement Network Level Authentication (NLA)
Enable:
Account lockout policies
Multi-factor authentication (MFA)
Monitor:
Internal scanning behavior
Unusual SYN traffic patterns
Deploy:
Endpoint Detection & Response (EDR)
Network intrusion detection rules for SYN scans
 Key Skills Demonstrated
Packet analysis using Wireshark
TCP flag analysis (SYN vs SYN/ACK)
Network reconnaissance detection
Use of Endpoints & Conversations statistics
Timeline reconstruction using UTC

 Conclusion
This investigation identified clear evidence of internal reconnaissance using a TCP SYN scan. The presence of exposed RDP services across multiple hosts significantly increases the risk of lateral movement and should be remediated to reduce attack surface.

📌 Tags
#Wireshark #NetworkForensics #CyberSecurity #BlueTeam #SOCAnalyst #ThreatDetection
