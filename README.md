Honeynet and Sandbox Network Deployment Report

Overview

This project documents the design, deployment, and experimentation of a Honeynet and a Sandbox Network. The goal was to simulate a realistic yet isolated environment where we could study attacker behavior, analyze malware safely, and apply cybersecurity concepts such as intrusion detection, log analysis, and forensic investigation.

⸻

Table of Contents
	•	Objectives
	•	Network Architecture
	•	Honeynet
	•	Sandbox Network
	•	Tools and Technologies Used
	•	Deployment Process
	•	Blue Team Machine Setup
	•	Red Team Machine Setup
	•	Steps Taken
	•	Results and Observations
	•	Challenges Encountered
	•	Conclusion and Future Work
	•	Contributors

⸻

Objectives
	•	Design and configure a honeynet to attract and log attacker activity.
	•	Set up a sandbox environment for malware analysis and controlled testing.
	•	Analyze captured data using tools such as Wireshark and Zeek.
	•	Understand real-world attack vectors and how to trace and mitigate them.

⸻

Network Architecture

Honeynet
	•	Purpose: To simulate vulnerable systems that attract real-world attackers.
	•	Key Components:
	•	VM1: Vulnerable Web Server (e.g., DVWA or Metasploitable2)
	•	VM2: Monitoring System (e.g., using Zeek, Wireshark, tcpdump)
	•	VM3: SSH/FTP exposed service (intentionally misconfigured)

Sandbox Network
	•	Purpose: Safe and controlled environment for malware testing and observation.
	•	Key Components:
	•	Isolated Windows/Linux host with no internet access
	•	Cuckoo Sandbox or manual analysis tools
	•	Network monitoring/sniffer tool (Wireshark, tcpdump)

Note: Please provide specific IP schemes, VM names, and any snapshots used if applicable.

⸻

Tools and Technologies Used
	•	Virtualization: VirtualBox / VMware
	•	Monitoring & Analysis: Wireshark, Zeek (Bro), tcpdump
	•	Honeypots: Cowrie, Dionaea, Honeyd (confirm which were used)
	•	Sandboxing: Cuckoo Sandbox / Remnux / FLARE VM (confirm choice)
	•	Operating Systems: Kali Linux, Metasploitable2, Ubuntu Server, Windows 10 (confirm which OSes were used)

⸻

Deployment Process

Blue Team Machine Setup
	1.	Download and install Oracle VirtualBox and PuTTY.
	2.	Download Ubuntu Server (24.04.2) ISO image.
	3.	Create a VM with the following settings:
	•	Base Memory: 20,000 MB
	•	Processors: 5 CPUs
	•	Hard Disk Size: 120 GB
	4.	Use unattended installation with:
	•	Hostname: bluesclues
	•	Domain: tpot.virtualbox.org
	5.	VM Network Configuration:
	•	Port forwarding rules:
	•	Name: SSH | Protocol: TCP | Host Port: 2222 → Guest Port: 64295
	•	Name: WebUI | Protocol: TCP | Host Port: 64297 → Guest Port: 64297
	•	Add Adapter 2: Set to Bridged Adapter
	6.	Inside the VM, run:

sudo ufw reset
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from 192.168.100.0/24 to any port 22
sudo ufw allow from 192.168.100.10 to any port 64297 proto tcp
sudo ufw enable


	7.	Update and install essential tools:

sudo apt update && sudo apt upgrade -y
sudo apt install git
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh


	8.	Use PuTTY to connect to VM using 127.0.0.1:2222
	9.	Clone and install T-Pot:

git clone https://github.com/telekom-security/tpotce
cd tpotce
chmod +x install.sh
./install.sh

	•	Select option (h) for T-Pot Standard.
	•	WebUI credentials:
	•	Username: [Your Username Here]
	•	Password: [Your Password Here]

	10.	Reboot VM:

sudo reboot


	11.	After reboot, run:

sudo docker compose up


	12.	Access the WebUI via https://127.0.0.1:64297.
	13.	Login to the dashboard and access Kibana.
	14.	Use ip a to check VM IP and verify connectivity from red team box.

Red Team Machine Setup
	1.	Download Kali Linux Everything ISO image.
	2.	Use Rufus to create a bootable USB with the ISO.
	3.	Boot red team machine from USB and install Kali Linux.
	4.	Set credentials: Username, Password

⸻

Steps Taken
	1.	Configured VMs with internal IPs: 192.168.X.X
	2.	Set up Wireshark on monitoring VM to capture .pcap files.
	3.	Opened ports on honeynet (e.g., 21, 22, 80).
	4.	Installed Zeek for protocol-level analysis.
	5.	Launched basic scanning with tools like nmap to simulate attacker recon.
	6.	Deployed malware sample into sandbox using USB image or shared folder.
	7.	Analyzed results and logs from Zeek, system logs, and pcap captures.

⸻

Results and Observations
	•	FTP directory traversal vulnerability was successfully exploited.
	•	SSH brute force attempts detected from multiple global IPs.
	•	Malware sample initiated suspicious outbound DNS requests (in sandbox).
	•	Zeek logs revealed HTTP GET requests with potential payload delivery.
	•	Wireshark .pcap files confirmed TCP reassembly and suspicious POST requests.

Note: Add actual attacker IPs, timestamps, payload hashes, etc. if available.

⸻

Challenges Encountered
	•	Network isolation was difficult due to VirtualBox NAT limitations.
	•	Cuckoo sandbox had driver installation issues.
	•	DNS resolution in the sandbox required local host mapping.
	•	Zeek log parsing required custom scripts to filter relevant traffic.

⸻

Conclusion and Future Work

This project provided hands-on experience with creating deceptive and secure environments to study attacker behavior and safely interact with malware samples. While we faced some configuration and compatibility issues, the honeynet and sandbox both successfully captured and allowed us to analyze meaningful data.

Next Steps
	•	Automate log analysis and alerting.
	•	Explore more advanced honeypots (e.g., Honeyd emulation).
	•	Integrate ELK stack for real-time visualization.
	•	Expand malware sample variety for sandbox analysis.

⸻

Contributors

(Include names, roles, and contributions here.)
