# Project 2: Build a SIEM (Wazuh on Ubuntu)

## 1. Objective

The goal of this project was to deploy a functional Security Information and Event Management (SIEM) system using Wazuh. The SIEM platform was installed on Ubuntu 24.04 LTS and configured to receive logs from a separate Ubuntu agent VM. This setup allowed me to monitor real-time system activity, generate alerts, and analyze security data through the Wazuh dashboard.

---

## 2. Tools & Technologies

- SIEM Platform: Wazuh (v4.7)  

- Operating System: Ubuntu 24.04 LTS (server and agent)  

- Hypervisor: VMware Workstation  

- Log Source: Second Ubuntu VM (agent)  

- Web Interface: Wazuh Dashboard (via Kibana)  

- Network Setup: Both VMs on the same NAT subnet with dynamic IPs assigned via DHCP  

---

## 3. Setup Process

---

### 3.1 Wazuh Server Installation

1. Deployed Ubuntu 24.04 LTS in VMware Workstation with 8GB RAM and 2 CPUs.

2. Updated the OS and downloaded the Wazuh all-in-one installation script.

3. Installed Wazuh Manager, Indexer, Filebeat, and Dashboard using:

    ```bash

    curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh

    sudo bash wazuh-install.sh -a

    ```

4. Verified the Wazuh Dashboard was accessible at `https://192.168.155.128:5601` using default credentials.

### 3.2 Ubuntu Agent Deployment

1. Created a second Ubuntu 24.04 LTS VM to act as a monitored endpoint.

2. Ensured both VMs were on the same subnet:  

    - Wazuh Server: `192.168.155.128`  

    - Agent VM: `192.168.155.129`

3. Used the Wazuh Dashboard’s "Deploy new agent" tool to generate a Linux agent install command.

4. Ran the generated one-liner on the agent VM to install and configure the Wazuh agent.

5. Enabled and started the agent using:

    ```bash

    sudo systemctl daemon-reload

    sudo systemctl enable wazuh-agent

    sudo systemctl start wazuh-agent

    ```

6. Verified that the agent appeared as Active in the Wazuh Dashboard.

---

## 4. Testing Log Collection

- Ran various commands on the agent (e.g., `whoami`, `sudo`, `apt update`) to generate log activity.

- Viewed logs in the Wazuh dashboard under the Threat Hunting and MITRE ATT&CK sections.

- Observed categorized alerts (e.g., Privilege Escalation, Valid Accounts) and system-level logging in real time.

---
## 5. What I Learned

- How to deploy a fully functional Wazuh SIEM using an automated script.

- How to configure a Linux agent and connect it securely to the Wazuh server.

- How Wazuh captures, categorizes, and visualizes log data using MITRE ATT&CK tactics.

- Gained experience in managing multi-VM network environments in VMware.
