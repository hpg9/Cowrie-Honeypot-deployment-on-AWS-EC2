# Cowrie-Honeypot-deployment-on-AWS-EC2

## Overview
AWS EC2 honeypot infrastructure deploying Cowrie SSH honeypot with Suricata IDS for 
network-layer detection. Captured and analyzed real-world attack campaigns including 
the **Multiverze cryptomining botnet**, mapped attacker TTPs to the MITRE ATT&CK 
framework, and visualized telemetry through an ELK stack (Elasticsearch, Filebeat, 
Kibana) with GeoIP mapping.

---

## Architecture
```text
Internet --> AWS EC2 (t3.micro)
                |
                +--iptables NAT (port 22 --> 2222)
                +--Cowrie SSH Honeypot (port 2222)
                +--Suricata Intrusion detection system (network layer detection)
                        |
                        |
                      Filebeat
                        |
                        |
                ELK Stack (Docker, local Mac)
                    +-- Elasticsearch (indexing + GeoIP)
                    +-- Kibana (dashboards + maps)


```
# AWS setup
### EC2 instance
<img width="1465" height="705" alt="Image" src="https://github.com/user-attachments/assets/094f276b-8772-44db-8501-f21b67bfc10a" /> 

### Security groups
<img width="1243" height="449" alt="Image" src="https://github.com/user-attachments/assets/3d4f48fd-f71f-4bb8-85e3-952d1105359c" />

### Cloudwatch Alarm and SNS alerts
| Attack Spike Graph | SNS email notification |
|---|---|
| <img width="1337" height="652" alt="Image" src="https://github.com/user-attachments/assets/231c397b-c15c-45d5-9197-7bc9a47c8bff" /> | <img width="575" height="672" alt="Image" src="https://github.com/user-attachments/assets/060ae275-92c6-4b62-8a58-28809343a24b" />

# Log analysis and visulization
All attack data has been parsed and visualized using elastic search, filebeat, and kibana with Geoip inrichment
<img width="1468" height="794" alt="Image" src="https://github.com/user-attachments/assets/67b78d36-7f62-4d53-a263-b43b5a3f860c" />
<img width="1458" height="654" alt="Image" src="https://github.com/user-attachments/assets/ac0963ef-46b8-4b37-b0af-870ca80bd3ac" />
| Bar Graph | Record count |
|---|---|
| <img width="737" height="422" alt="Image" src="https://github.com/user-attachments/assets/f86f4233-c0d9-488a-9e24-f15a19034547" /> | <img width="760" height="396" alt="Image" src="https://github.com/user-attachments/assets/5e5095c6-68f1-4e16-8f4c-9d8de11299fd" /> 


## Key Findings 

### Captured Attack Data
Real attacker commands and data from their session logged by Cowire and organized for real threat analysis. This Data includes Source IPs of attackers, and commands ran captured during live attacks.
<img width="1468" height="917" alt="Image" src="https://github.com/user-attachments/assets/f9cc8d41-99ec-47db-b0ba-188203dab215" />


## Threat: Multiverze Cryptomining botnet family
- **Comfirmed via:** VirusTotal - classified as cryptomining malware
- **Attack vector:** SSH brute force --> shell command execution
- **Objective:** Deploy XMRig based monero miner on vulnerable hosts
- **Multiverze threat actors IPS and malicious commands ran**
  | | |
  |---|---|
  | <img width="1468" height="867" alt="Image" src="https://github.com/user-attachments/assets/fe4df690-4ad6-4137-a5e2-5fcef5f20b42" />| <img width="1463" height="758" alt="Image" src="https://github.com/user-attachments/assets/ef2ce4d4-3d81-4665-a818-72c6f8146931" /> |

  ## MITRE ATT&CK Framework Mapping

Attacker behavior observed through Cowrie logs was mapped to the MITRE 
ATT&CK framework to classify tactics and techniques used during live attacks.

| Tactic | Technique | ID | Evidence Observed |
|---|---|---|---|
| Reconnaissance | Active Scanning | T1595 | Automated scanners probing port 22 within minutes of deployment |
| Initial Access | Exploit Public-Facing Application | T1190 | SSH honeypot targeted continuously across the deployment period |
| Credential Access | Brute Force: Password Spraying | T1110.003 | Thousands of login attempts using common credentials (root, admin, ubuntu) |
| Execution | Command and Scripting Interpreter: Unix Shell | T1059.004 | chmod +x scripts, curl payload downloads, nohup persistence commands |
| Persistence | Boot or Logon Initialization Scripts | T1037 | nohup sshd & commands attempting to survive reboots |
| Defense Evasion | Masquerading | T1036 | Malware dropped in /tmp with randomized filenames |
| Discovery | System Information Discovery | T1082 | uname -a, cat /etc/passwd, /proc/cpuinfo enumeration commands |
| Lateral Movement | Remote Services: SSH | T1021.004 | Botnet spread attempts to 50+ C2 IPs via SSH |
| Collection | Data from Local System | T1005 | Attackers reading system files to profile the host |
| Impact | Resource Hijacking | T1496 | XMRig Monero miner deployment via Multiverze botnet |
  


