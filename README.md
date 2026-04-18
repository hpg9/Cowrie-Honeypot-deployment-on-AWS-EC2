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
  


