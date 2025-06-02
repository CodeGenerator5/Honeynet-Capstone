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

---

## Tools and Technologies Used

- **Virtualization**: VirtualBox / VMware
- **Monitoring & Analysis**: Wireshark, Zeek (Bro), tcpdump
- **Honeypots**: Cowrie, Dionaea, Honeyd  
- **Sandboxing**: Cuckoo Sandbox / Remnux / FLARE VM  
- **Operating Systems**: Kali Linux, Metasploitable2, Ubuntu Server, Windows 10  

---

## T-Pot Honeynet Deployment Process: Blue Team & Red Team Machine Setup

### 📘 Blue Team VM Setup (Ubuntu Server)

### 📥 Prerequisites

- [Download Oracle VirtualBox](https://www.virtualbox.org/)
- [Download PuTTY](https://www.putty.org/)
- [Download Ubuntu Server 24.04.2 ISO](https://ubuntu.com/download/server)

### 💻 Create Virtual Machine (VM)

Configure the VM in VirtualBox with the following specs:

- **Base Memory**: 20,000 MB
- **Processors**: 5 CPUs
- **Hard Disk Size**: 120 GB

### ⚙️ Unattended Installation

Use these settings during the unattended install:

- **Hostname**: `bluesclues`  
- **Domain Name**: `tpot.virtualbox.org`

### 🌐 Networking Setup

1. Go to `Settings` > `Network` > `Port Forwarding` and add the following rules:

| Name    | Protocol | Host IP       | Host Port | Guest IP      | Guest Port |
|---------|----------|---------------|-----------|---------------|------------|
| SSH     | TCP      | 127.0.0.1     | 2222      | 10.0.2.15     | 64295      |
| WebUI   | TCP      | 127.0.0.1     | 64297     | 10.0.2.15     | 64297      |

2. Under `Adapter 2`, enable `Bridged Adapter`.

---

### 🔐 UFW Firewall Rules (on the VM)

bash:  
sudo ufw reset  
sudo ufw default deny incoming  
sudo ufw default allow outgoing  
sudo ufw allow from 192.168.100.0/24 to any port 22  
sudo ufw allow from 192.168.100.10 to any port 64297/tcp  
sudo ufw enable  


### 🔧 First Time Setup on VM
sudo apt update && sudo apt upgrade -y  
sudo apt install git  
sudo apt install openssh-server  
sudo systemctl enable ssh  
sudo systemctl start ssh  


### 🐙 Clone and Install T-Pot
git clone https://github.com/telekom-security/tpotce  
cd tpotce  
chmod +x install.sh  
./install.sh  


### 📊 Access the Dashboard
1. Open Chrome
2. Visit: https://127.0.0.1:64297
3. Login with the credentials created earlier
4. Navigate to Kibana inside the dashboard

💡 Note: On VM reboot, verify internal IP with ip a or ifconfig to ensure connectivity.


---

## 🔴 Red Team Setup (Kali Linux Bare Metal)

📥 Tools & Image
• Download Kali Linux Everything ISO from an official or archived source.
• Download Rufus to create a bootable USB.

🛠️ Boot and Install Kali
1. Use Rufus to write the Kali ISO to a USB stick.
2. Insert USB into the Red Team machine.
3. Reboot and enter BIOS (F10 or appropriate key).
4. Select USB drive as boot device.
5. Install Kali Linux.

🔐 Red Team Login Credentials: Username, Password

---

## Steps Taken

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
