# SniffPy-A Network Traffic Monitoring and Analysis Tool  

## Overview:
In today’s digital landscape, network traffic monitoring plays a critical role in 
maintaining security, diagnosing network issues, and ensuring optimal data flow. With 
the rise in cyberattacks, malicious traffic, and data breaches, it has become essential to 
analyze packets transmitted over a network to understand behaviors, detect threats, and 
take preventive actions. 

## Abstract:
This project focuses on the development of a network packet sniffer using Python raw 
sockets, without relying on external libraries such as Scapy. The tool is designed to intercept, 
decode, and display network packets in real time, offering a deeper understanding of network 
protocols and traffic flow. The implementation directly captures packets at the Ethernet level 
using the AF_PACKET socket family and processes them using Python’s built-in struct 
module for binary data unpacking. 
The sniffer identifies and parses various protocol layers, including Ethernet, IPv4, TCP, 
UDP, and ICMP. It extracts and displays detailed information such as MAC addresses, IP 
addresses, port numbers, sequence and acknowledgment numbers, TTL, flags (SYN, ACK, 
FIN, etc.), and raw payload data. Special attention is given to formatting and presenting multi
line payloads in a readable structure using custom text wrapping. 
By avoiding third-party dependencies, the project emphasizes low-level network 
programming and gives learners an opportunity to understand how packet analysis works 
internally. The tool serves as a foundation for building more advanced network utilities such 
as intrusion detection systems, traffic analyzers, and protocol testing tools. It also 
demonstrates the capability of Python for performing real-time network monitoring tasks 
efficiently and with precise control over packet structure.

## Problem Formulation:
While packet sniffing tools like Wireshark are widely used, they often function as black 
boxes and obscure the internal mechanics of how packet capture works. There is a lack 
of lightweight tools that: 
• Work without third-party libraries. 
• Allow users to learn how data is extracted from raw bytes. 
• Are highly customizable for academic or educational purposes. 
This project addresses the gap by building a real-time monitoring tool that captures and 
decodes raw Ethernet frames and network layer protocols using only the built-in socket 
and struct modules in Python. It is a manual and transparent approach, providing fine
grained control and insights into packet data. 

 ## What is Network Packet Sniffing?
Network packet sniffing refers to the process of intercepting and analyzing network 
traffic. Each packet transferred over a network contains headers and data payloads 
related to communication protocols. Sniffing tools examine these packets for various 
purposes—network diagnostics, traffic monitoring, intrusion detection, and security 
research. 
Traditional tools like Wireshark and tcpdump offer user-friendly interfaces, but abstract 
much of the underlying operations. For a foundational understanding, it is crucial to 
explore how these packets can be captured and interpreted manually at the byte level.
![image](https://github.com/user-attachments/assets/ba229e1c-06ba-4a01-b7d1-e2a6cdd152d2)

## Use Cases:
1.Performance Analysis
2.Network Troubleshooting
3.Security Research
4.Traffic Monitoring
5.Intrusion Detection

## METHODOLOGY :
1. Packet Capture- 
A raw socket is initialized using: 
This listens on all interfaces and captures every packet  
(ethertype 0x0003 = all protocols).

2. Ethernet Frame Parsing 
First, the 14-byte Ethernet frame is extracted and parsed: 
• Destination MAC 
• Source MAC 
• Protocol type (e.g., 0x0800 for IPv4) 
The struct.unpack('! 6s 6s H', data[:14]) function is used to 
split these components.

![image](https://github.com/user-attachments/assets/960ab7eb-b1d8-45e9-83ec-13952f9573c8)
Figure 3.1: Ethernet Header 

3. IPv4 Packet Parsing 
If the Ethernet protocol is 0x0800, it signifies an IPv4 packet. Key fields include: 
• Version and header length 
• TTL 
• Protocol (TCP=6, UDP=17, ICMP=1) 
• Source and destination IPs 
The program checks the protocol field to determine further parsing logic.

![image](https://github.com/user-attachments/assets/7244834d-18d0-4048-9bfe-e9b998434e8f) 
Figure 3.2: IPv4 Header 

4. Transport Layer Analysis 
Based on the protocol field in the IP header: 
• ICMP: The first 4 bytes are unpacked to get Type, Code, and Checksum. 
• TCP: Parses ports, sequence number, acknowledgment, and flag bits (URG, 
ACK, PSH, RST, SYN, FIN). 
• UDP: Extracts source/destination ports and length. 
Each layer is parsed manually using struct.unpack() and logical bitwise operations. 
 
5. Output Formatting 
The extracted data is printed with tabbed indentation for clarity. Multiline payloads are 
formatted using textwrap and a custom utility function to make hex data readable. 
This hierarchical and readable output helps in understanding the structure of each 
packet and is useful for debugging or training purposes.

## OUTPUT WINDOW:
![image](https://github.com/user-attachments/assets/ae14022a-fed5-4e5f-95ab-caf8b7e42624)
![image](https://github.com/user-attachments/assets/98ec2111-dcf7-488f-891e-dde098703817)
![image](https://github.com/user-attachments/assets/458003f3-e28d-4321-aa95-4e22ee43ea65)

## Future Extensions:-
The current implementation is robust for learning and analysis, but it can be extended into a more powerful tool through:
1. Data Storage and Reporting
Export logs to CSV, PCAP, or JSON formats.
Maintain a historical log of traffic for post-analysis.

2. Graphical User Interface (GUI)
Integrate with Tkinter, PyQt, or Dash for visual dashboards.
Real-time plotting of traffic volume, IP sources, and protocol distribution.

3. Anomaly and Intrusion Detection
Implement rule-based checks for:
i.Unusual port scans
ii.SYN flood attempts
iii.Unknown protocol traffic
iv.Serve as a lightweight Intrusion Detection System (IDS) prototype.

4. Protocol Support Expansion
Add support for:
Application layer protocols like DNS, HTTP, FTP
IPv6 packet parsing
TLS/SSL handshake metadata

5. Cross-Platform Compatibility
Abstract platform-dependent features to make it work on Windows and macOS, possibly with libraries like pypcap or WinPcap.

7. Cloud and Remote Logging
Send packet data to a remote server or cloud database for centralized monitoring.

## Potential Applications:-
1.Cybersecurity training labs for demonstrating packet analysis and header inspection.
2.Academic coursework in networking, operating systems, or system programming.
3.Troubleshooting tool in small networks or embedded devices.
4.Baseline for custom tools, like honeypots, firewalls, or traffic classifiers.
5.Network diagnostics utility in constrained environments where tools like Wireshark are too heavy.
