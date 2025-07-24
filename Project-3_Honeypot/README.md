# Project 3 - Honeypot Deployment: T-Pot CE on Ubuntu

## 1. Tools & Technologies

- Honeypot Framework: T-Pot Community Edition (24.04.1)
- Operating System: Ubuntu Server 24.04 LTS
- Hypervisor: VMware Workstation
- Log and Visualization Stack: ELK (Elasticsearch, Logstash, Kibana)
- Web Interface: T-Pot Web UI (NGINX Frontend)
- Network Configuration: Bridged Adapter with internal IP assignment

---

## 2. Project Overview

This project involved deploying a honeypot environment using T-Pot Community Edition on a bridged Ubuntu 24.04 virtual machine. T-Pot includes multiple honeypots (Cowrie, Suricata, Mailoney, etc.) containerized with Docker and integrated into the ELK Stack for centralized log processing and visualization.

The goal was to simulate attacker behavior, observe how honeypots record malicious activity, and analyze events through Kibana. This helped build experience with honeypot deployment, container orchestration, and basic threat intelligence collection.

---

## 3. Setup Process

### 3.1 T-Pot Installation

1. I deployed Ubuntu 24.04 LTS in VMware Workstation with 8GB RAM and 2 CPUs.
2. I updated the system and installed Docker and Docker Compose.

   ![Docker Install](./images/DockerInstall.PNG)

3. I cloned the official T-Pot repository from GitHub:

   ```bash
   git clone https://github.com/telekom-security/tpotce.git
   ```

   ![Repository Cloned](./images/TPOT_Clone.PNG)

4. I launched the installation script and selected the `Hive` install type.

   ![Installer Splash Screen](./images/TPOT_Installer_Splash.PNG)
   ![Playbook Successful](./images/Playbook_Successful.PNG)
   ![Install Complete](./images/Install_Complete.PNG)

5. After installation, I rebooted the VM and verified the T-Pot stack was running via Docker:

   ![Docker Results](./images/Docker_Results.PNG)
   ![Docker Containers Healthy](./images/Docker_Results2.PNG)

6. I also confirmed system resource usage using htop:

   ![HTop Overview](./images/HTop.PNG)

---

### 3.2 Interface Access

Once T-Pot initialized, I accessed the web interface from my host system:

- URL: `http://192.168.68.156:64297`

   ![T-Pot Web Interface](./images/TPot_Home2.PNG)

The interface provides access to:
- Kibana (for log visualization)
- CyberChef
- Spiderfoot (OSINT automation)
- Elasticvue (index browsing)

---

### 3.3 Simulated Attack Traffic

Since my VM was isolated, I simulated attacker behavior manually from the same network. I SSH’d into the honeypot using fake credentials, which triggered Cowrie logging.

   ![SSH Login Attempt](./images/SSH_LogIn_Ubuntu.PNG)

The Kibana dashboard confirmed the event:
- Cowrie recorded the connection
- The username `don` and password `Libby221!!` appeared in the word cloud
- The SSH service (port 22) showed activity in the logs

   ![Kibana Populated Dashboard 1](./images/Kibana_Populated.PNG)
   ![Kibana Populated Dashboard 2](./images/Kibana_Populated2.PNG)

Logs also included:
- Source and destination IPs
- Flow IDs and ports
- Honeypot container names and timestamps

   ![Kibana Logs 1](./images/Kibana_Logs.PNG)
   ![Kibana Logs 2](./images/Kibana_Logs2.PNG)

---

## 4. Troubleshooting and Observations

- The `cups` service was using port 631, which blocked one honeypot container. I disabled it to resolve the conflict.
- The `nginx` container failed to appear at first; a full reboot resolved the issue.
- After installation, SSH was moved to port `64295`. I updated my SSH commands accordingly.
- Kibana initially displayed no results. After running the test login, dashboards and log streams populated correctly.

---

## 5. Log Interpretation

From my simulated attack:
- Cowrie logged an SSH login attempt using known bad credentials.
- Source IP matched my attacker VM.
- Kibana visualized both login fields and flow-level metadata.
- These results prove that T-Pot can capture and forward interaction data through the ELK stack for real-time monitoring and analysis.

---

## 6. Next Steps

- Forward honeypot logs to an external SIEM like Wazuh or Splunk for centralized correlation.
- Add packet inspection using Suricata’s alerting features.
- Set up email alerts for Cowrie login attempts.
- Deploy on a cloud VM or in a DMZ to observe real-world scanning and exploit behavior.

---

## 7. Summary

This project involved deploying T-Pot CE in a virtualized environment to simulate and monitor network-based attacks. I demonstrated its effectiveness by generating a controlled SSH login attempt and confirming successful log collection and visualization using Kibana. This hands-on experience highlighted the value of honeypots in detection, analysis, and proactive defense strategies.
