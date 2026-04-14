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
## Key Findings 

### Captured Attack Data
Real attacker commands and data from their session logged by Cowire and organized for real threat analysis. This Data includes Source IPs of attackers, and commands ran captured during live attacks.
<img width="1468" height="917" alt="Image" src="https://github.com/user-attachments/assets/f9cc8d41-99ec-47db-b0ba-188203dab215" />


## Threat: Multiverze Cryptomining botnet family
- **Comfirmed via:** VirusTotal - classified as cryptomining malware
- **Attack vector:** SSH brute force --> shell command execution
- **Objective:** Deploy XMRig based monero miner on vulnerable hosts
- **Multiverze threat actors IPS and malicious commands ran**
  


