## Table of Contents

- [Objectives](#objectives)
- [Network Architecture](#network-architecture)
  - [Honeynet](#honeynet)
  - [Sandbox Network](#sandbox-network)
- [Tools and Technologies Used](#tools-and-technologies-used)
- [Deployment Process](#deployment-process)
- [Steps Taken](#steps-taken)
- [Results and Observations](#results-and-observations)
- [Challenges Encountered](#challenges-encountered)
- [Conclusion and Future Work](#conclusion-and-future-work)
- [Contributors](#contributors)

---

## Objectives

- Design and configure a **honeynet** to attract and log attacker activity.
- Set up a **sandbox environment** for malware analysis and controlled testing.
- Analyze captured data using tools such as Wireshark and Zeek.
- Understand real-world attack vectors and how to trace and mitigate them.

---

## Network Architecture

### Honeynet

- **Purpose**: To simulate vulnerable systems that attract real-world attackers.
- **Key Components**:
  - [ ] VM1: Vulnerable Web Server (e.g., DVWA or Metasploitable2)
  - [ ] VM2: Monitoring System (e.g., using Zeek, Wireshark, tcpdump)
  - [ ] VM3: SSH/FTP exposed service (intentionally misconfigured)

### Sandbox Network

- **Purpose**: Safe and controlled environment for malware testing and observation.
- **Key Components**:
  - [ ] Isolated Windows/Linux host with no internet access
  - [ ] Cuckoo Sandbox or manual analysis tools
  - [ ] Network monitoring/sniffer tool (Wireshark, tcpdump)

> **Note**: Please provide specific IP schemes, VM names, and any snapshots used if applicable.

---

## Tools and Technologies Used

- **Virtualization**: VirtualBox / VMware
- **Monitoring & Analysis**: Wireshark, Zeek (Bro), tcpdump
- **Honeypots**: Cowrie, Dionaea, Honeyd _(confirm which were used)_
- **Sandboxing**: Cuckoo Sandbox / Remnux / FLARE VM _(confirm choice)_
- **Operating Systems**: Kali Linux, Metasploitable2, Ubuntu Server, Windows 10 _(confirm which OSes were used)_

---

## Deployment Process

### 1. Planning and Design
- Defined objectives for both honeynet and sandbox.
- Identified necessary tools and VMs.
- Planned IP address allocation and VM interconnectivity.

### 2. Environment Setup
- Installed and configured virtualization platform.
- Created isolated internal network for sandboxing.
- Connected honeynet to an external bridged network (with firewall rules).

### 3. Honeynet Configuration
- Deployed vulnerable services (e.g., FTP, SSH, HTTP).
- Enabled logging for all inbound/outbound connections.
- Installed intrusion detection software (Zeek, Snort).

### 4. Sandbox Network Setup
- Installed Windows/Linux VM with malware analysis tools.
- Ensured no internet connectivity to avoid real-world infections.
- Installed Cuckoo Sandbox for automated malware execution (if applicable).

### 5. Attack Simulation / Waiting for Infections
- Left honeynet exposed to the internet (bridged or DMZ mode).
- Captured traffic and logged attacker behavior.
- Injected sample malware into sandbox manually or via attacker access.

---

## Steps Taken

> **Note**: This section should be customized with more granular logs or command lines used.

1. Configured VMs with internal IPs: 192.168.X.X
2. Set up Wireshark on monitoring VM to capture .pcap files.
3. Opened ports on honeynet (e.g., 21, 22, 80).
4. Installed Zeek for protocol-level analysis.
5. Launched basic scanning with tools like nmap to simulate attacker recon.
6. Deployed malware sample into sandbox using USB image or shared folder.
7. Analyzed results and logs from Zeek, system logs, and pcap captures.

---

## Results and Observations

- **FTP directory traversal** vulnerability was successfully exploited.
- SSH brute force attempts detected from multiple global IPs.
- Malware sample initiated suspicious outbound DNS requests (in sandbox).
- Zeek logs revealed HTTP GET requests with potential payload delivery.
- Wireshark .pcap files confirmed TCP reassembly and suspicious POST requests.

> **Note**: Add actual attacker IPs, timestamps, payload hashes, etc. if available.

---

## Challenges Encountered

- Network isolation was difficult due to _e.g., VirtualBox NAT limitations_.
- Cuckoo sandbox had driver installation issues.
- DNS resolution in the sandbox required local host mapping.
- Zeek log parsing required custom scripts to filter relevant traffic.

---

## Conclusion and Future Work

This project provided hands-on experience with creating deceptive and secure environments to study attacker behavior and safely interact with malware samples. While we faced some configuration and compatibility issues, the honeynet and sandbox both successfully captured and allowed us to analyze meaningful data.

### Next Steps

- Automate log analysis and alerting.
- Explore more advanced honeypots (e.g., Honeyd emulation).
- Integrate ELK stack for real-time visualization.
- Expand malware sample variety for sandbox analysis.
