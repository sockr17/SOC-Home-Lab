# SOC Home Lab – Phase 1: SIEM Infrastructure & Endpoint Monitoring
*Infrastructure Deployment | Log Pipeline Validation | Wazuh SIEM*

<p align="center">

[![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-orange?style=for-the-badge&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![Kali Linux](https://img.shields.io/badge/Kali_Linux-2023.3-blue?style=for-the-badge&logo=kali-linux&logoColor=white)](https://www.kali.org/)
[![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-0056D2?style=for-the-badge&logo=wazuh&logoColor=white)](https://wazuh.com/)
[![VMware](https://img.shields.io/badge/VMware-Workstation-607078?style=for-the-badge&logo=vmware&logoColor=white)](https://www.vmware.com/)
[![Phase](https://img.shields.io/badge/Series-Phase_1_of_3-8B0000?style=for-the-badge)](https://github.com/kripy17)
[![Status](https://img.shields.io/badge/Status-Complete-2ea44f?style=for-the-badge)](https://github.com/kripy17)

</p>

---

## 📋 Table of Contents

<details open>
<summary><strong>🏗️ Phase 1 — Infrastructure Setup</strong></summary>

&nbsp;&nbsp;&nbsp;&nbsp;`01` [Project Overview](#1-project-overview)  
&nbsp;&nbsp;&nbsp;&nbsp;`02` [Lab Objectives](#2-lab-objectives)  
&nbsp;&nbsp;&nbsp;&nbsp;`03` [Lab Architecture](#3-lab-architecture)  
&nbsp;&nbsp;&nbsp;&nbsp;`04` [Network Configuration](#4-network-configuration)  
&nbsp;&nbsp;&nbsp;&nbsp;`05` [Lab Environment Configuration](#5-lab-environment-configuration)  

</details>

<details open>
<summary><strong>🖥️ Phase 2 — SIEM Deployment</strong></summary>

&nbsp;&nbsp;&nbsp;&nbsp;`06` [Server Hardening & Initial Setup](#6-server-hardening--initial-setup)  
&nbsp;&nbsp;&nbsp;&nbsp;`07` [Wazuh SIEM Installation & Configuration](#7-wazuh-siem-installation--configuration)  
&nbsp;&nbsp;&nbsp;&nbsp;`08` [Agent Deployment & Log Onboarding](#8-agent-deployment--log-onboarding)  

</details>

<details open>
<summary><strong>🧪 Phase 3 — Validation & Findings</strong></summary>

&nbsp;&nbsp;&nbsp;&nbsp;`09` [Attack Simulation & Detection Validation](#9-attack-simulation--detection-validation)  
&nbsp;&nbsp;&nbsp;&nbsp;`10` [Lessons Learned, Limitations & Future Improvements](#10-lessons-learned-limitations--future-improvements)  
&nbsp;&nbsp;&nbsp;&nbsp;`11` [Final Project Conclusion](#11-final-project-conclusion)  

</details>

---

## 1. Project Overview

This repository documents the design and implementation of a foundational **Security Operations Center (SOC) home lab**, focused on infrastructure deployment and log pipeline validation.

The primary objective is to build a **fully functional SIEM environment** using **Wazuh** within an isolated virtualized network — simulating a real SOC monitoring setup where endpoint logs are collected, processed, indexed, and visualized through a centralized dashboard.

The environment is intentionally designed to operate under limited hardware resources (8 GB host RAM), requiring careful VM sizing and service optimization for stable performance. This project forms the **infrastructure foundation** of a larger SOC lab series and establishes a fully operational monitoring pipeline for future detection and investigation exercises.

> 📌 **Back to portfolio:** [kripy17 – GitHub Profile](https://github.com/kripy17)

---

## 2. Lab Objectives

This project focuses on building the **foundational infrastructure** of a functional SOC within a controlled virtual environment.

**Primary objectives:**

* Deploy a centralized **Ubuntu Server** to act as the SOC node
* Install and configure a **SIEM platform using Wazuh**
* Configure secure remote access and implement basic server hardening
* Establish a controlled **NAT-based virtual network** for internal communication
* Onboard a monitored endpoint (**Kali Linux**) using the Wazuh agent
* Validate successful log ingestion from endpoint to SIEM
* Confirm alert generation and dashboard visibility
* Troubleshoot firewall and connectivity issues as they arise

> ⚠️ **Note:** This project intentionally focuses on **infrastructure deployment and log pipeline validation**. Advanced detection engineering, attack simulation, and incident investigation will be addressed in subsequent phases of the SOC lab series.

---

## 3. Lab Architecture

The SOC lab is deployed within a **virtualized environment** simulating a simplified internal enterprise network. The architecture separates the **monitoring server** and the **endpoint**, reflecting a realistic centralized logging model.

### Lab Components

| Component | Role |
|---|---|
| Host Machine (VMware Workstation) | Runs and manages all virtual machines |
| Ubuntu Server (SOC Node) | Centralized SIEM — log collection, processing, visualization |
| Kali Linux (Monitored Endpoint) | Generates system and security events forwarded to the SIEM |
| NAT Virtual Network | Isolated internal subnet with internet access |
| Wazuh (All-in-One Deployment) | Log collection, indexing (OpenSearch), detection engine, dashboard |

### Architectural Flow

The **Ubuntu Server** acts as the centralized SOC node running the Wazuh Manager, Indexer, and Dashboard in **all-in-one mode** to conserve system resources. The **Kali Linux VM** functions as a monitored endpoint — logs are collected by the Wazuh agent and securely forwarded to the manager over the internal NAT network.

The diagram below illustrates how these components connect and communicate within the lab:

<img src="screenshots/ubuntu/Architecture_Flow.png" width="800">

### Design Considerations

This architecture was intentionally built to:

* Operate within limited hardware (4GB SOC VM RAM)
* Maintain isolation from the external network
* Simulate centralized log aggregation
* Support realistic endpoint onboarding workflows
* Provide clear visibility into log ingestion and alert generation

Detection engineering, attack simulation, and multi-endpoint expansion will be implemented in future phases.

---

## 4. Network Configuration

All VMs are configured using **NAT networking mode** within VMware Workstation, placing them within the same internal subnet while retaining outbound internet access through the host.

### Subnet Configuration

| Setting | Value |
|---|---|
| Network Range | `192.168.1.0/24` |
| Subnet Mask | `255.255.255.0` |
| Gateway | Provided by VMware NAT service |
| Internet Access | Routed through host system |

The screenshot below shows the VMware virtual network configuration confirming the NAT subnet is correctly defined:

<img src="screenshots/ubuntu/nat_config.png" width="600">

### Why NAT Was Chosen

* Isolated internal communication between VMs
* Safe simulation within a controlled subnet
* Internet access for package installation and updates
* Clean separation from the physical LAN

### Required Communication Paths

For the SOC pipeline to function, the following paths must be active:

| Path | Purpose |
|---|---|
| Kali → Ubuntu SOC Server (1514, 1515) | Agent registration and log forwarding |
| Ubuntu → Internal OpenSearch services | Log indexing |
| Host Browser → Dashboard (443) | HTTPS dashboard access |

---

## 5. Lab Environment Configuration

### 5.1 Host Virtualization Platform

The SOC lab runs on **VMware Workstation**, chosen for its stable NAT networking, flexible resource allocation, reliable performance under constrained hardware, and snapshot support for safe testing and rollback.

### 5.2 Operating System Selection

The primary SOC server runs **Ubuntu Server 22.04 LTS** — selected for its long-term support stability, lightweight footprint, strong compatibility with security tooling, and wide adoption in enterprise environments.

### 5.3 Ubuntu SOC Server Specifications

| Resource | Allocation |
|---|---|
| RAM | 4 GB |
| CPU | 2 Cores |
| Disk | 40 GB |
| Network | VMnet8 (NAT) |

The VM was configured conservatively to support the Wazuh all-in-one stack within an 8GB host limit while keeping indexing and dashboard services stable. The following screenshot confirms resource allocation in VMware before the VM was started:

<img src="screenshots/ubuntu/vm_settings.png" width="700">

### 5.4 Disk Allocation During Installation

Proper disk allocation is critical for SIEM deployments — indexing services and log retention consume significant storage over time. The screenshot below shows disk configuration during Ubuntu installation, confirming the full 40GB allocation was correctly recognized:

<img src="screenshots/ubuntu/storage_config.png" width="700">

### 5.5 SSH Configuration

OpenSSH Server was enabled during installation to allow secure remote access to the SOC node. This removes the dependency on the VMware console and enables cleaner remote administration throughout the project:

<img src="screenshots/ubuntu/ssh_config.png" width="700">

### 5.6 System Verification After Installation

Post-installation, system details were verified to confirm the correct Ubuntu version, proper hostname, and a valid IP assignment within the NAT subnet. This baseline check ensures the server is ready before any hardening or service installation begins:

<img src="screenshots/ubuntu/system_check.png" width="800">

---

## 6. Server Hardening & Initial Setup

Before deploying Wazuh, the Ubuntu SOC server was updated, cleaned, and minimally hardened to establish a secure and stable baseline.

### 6.1 System Update and Package Upgrade

The package index was refreshed and all installed packages were upgraded to their latest stable versions. This ensures security patches are applied and dependency consistency is maintained before SIEM installation:

```bash
sudo apt update
sudo apt upgrade -y
```

The output below confirms the update and upgrade completed without errors:

<img src="screenshots/ubuntu/update_complete.png" width="800">

### 6.2 Removal of Unnecessary Snap Packages

To reduce system overhead and minimize the attack surface, unnecessary snap packages were reviewed and removed. This keeps the server profile lightweight and reduces background services competing for the limited 4GB RAM allocation:

<img src="screenshots/ubuntu/useless_snaps.png" width="800">

### 6.3 Firewall Baseline Configuration (UFW)

UFW was enabled with a default-deny inbound policy. At this stage only SSH was permitted, following a minimal exposure approach — additional ports will be opened only when specific services require them:

```bash
sudo ufw allow OpenSSH
sudo ufw enable
```

The screenshot below confirms UFW is active and only SSH is currently permitted:

<img src="screenshots/ubuntu/ufw_status.png" width="800">

### 6.4 SSH Service Verification

The SSH service was confirmed as active and running before proceeding. This ensures remote access is available throughout the rest of the deployment without needing to fall back to the VMware console:

```bash
sudo systemctl status ssh
```

<img src="screenshots/ubuntu/ssh_status.png" width="800">

### 6.5 Network Configuration Verification

The server's IP address was verified to confirm it received a valid NAT subnet assignment. This IP will be used throughout the project for agent configuration and dashboard access:

```bash
ip a
```

<img src="screenshots/ubuntu/system_check.png" width="800">

### 6.6 Resource Validation

Available memory and disk space were checked to confirm the server can handle Wazuh's indexing and log storage requirements before installation begins:

```bash
free -h
df -h
```

<img src="screenshots/ubuntu/resource_check.png" width="800">

### Hardening Summary

| Step | Status |
|---|---|
| System update & upgrade | ✅ Complete |
| Unnecessary snap removal | ✅ Complete |
| UFW firewall configured (SSH only) | ✅ Complete |
| SSH service verified | ✅ Complete |
| Network & IP validated | ✅ Complete |
| Resources verified | ✅ Complete |

---

## 7. Wazuh SIEM Installation & Configuration

With a hardened baseline in place, the Wazuh all-in-one SIEM stack was installed on the Ubuntu SOC server. This includes the Wazuh Manager, Indexer (OpenSearch), Filebeat, and Dashboard — all deployed as a single unified stack to work within the lab's resource constraints.

### 7.1 Installation Script Download

The official Wazuh installation script was pulled directly from the Wazuh repository. Using the official script ensures correct dependency resolution and component configuration:

```bash
curl -O https://packages.wazuh.com/4.14/wazuh-install.sh
```

The screenshot below confirms the script was successfully downloaded:

<img src="screenshots/wazuh/wazuh_script.png" width="800">

### 7.2 Executing the All-in-One Installation

The script was made executable and run in all-in-one mode. This single command handles the full stack — Manager, OpenSearch Indexer, Filebeat, and Dashboard with SSL configuration:

```bash
chmod +x wazuh-install.sh
sudo ./wazuh-install.sh -a
```

Installation progress is shown below. The process takes several minutes as each component is installed and configured sequentially:

<img src="screenshots/wazuh/wazuh_install_start.png" width="800">

### 7.3 Dashboard Credential Generation

Upon completion, the installation script outputs default credentials for the Wazuh Dashboard. These are required for the initial HTTPS login and were securely noted before proceeding:

<img src="screenshots/wazuh/wazuh_dashboard_credentials.png" width="800">

### 7.4 Service Verification

All four Wazuh services were checked individually to confirm they started correctly. A failed or degraded service at this stage would indicate a configuration or resource issue requiring investigation before continuing:

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-dashboard
sudo systemctl status wazuh-indexer
sudo systemctl status filebeat
```

All services returned `active (running)` as shown below:

<img src="screenshots/wazuh/wazuh_services_running.png" width="800">

### 7.5 Firewall Rule Expansion

With the dashboard now running, port 443 was opened to allow HTTPS access from the host browser. This follows the incremental exposure approach established during hardening — ports are only opened when a specific service requires them:

```bash
sudo ufw allow 443/tcp
```

The updated firewall status confirms port 443 is now permitted alongside SSH:

<img src="screenshots/wazuh/wazuh_port443_open.png" width="800">

### 7.6 Dashboard Access & Verification

The Wazuh Dashboard was accessed from the host machine using the server's NAT IP. A browser warning appeared due to the self-signed SSL certificate and was bypassed for lab purposes:

```
https://<server-ip_address>
```

The login page below confirms the dashboard is reachable and the web service is responding correctly:

<img src="screenshots/wazuh/wazuh_dashboard_login.png" width="800">

After logging in with the generated credentials, the dashboard home interface confirmed that the Manager, Indexer, and all core components are operational and communicating:

<img src="screenshots/wazuh/wazuh_dashboard_home.png" width="800">

---

## 8. Agent Deployment & Log Onboarding

With the SIEM stack confirmed operational, the next step is onboarding a monitored endpoint. A Kali Linux VM was provisioned and enrolled into the Wazuh infrastructure — mirroring real SOC workflows where endpoints must be registered and validated before telemetry begins flowing.

### 8.1 Kali Linux VM Configuration

| Resource | Allocation |
|---|---|
| RAM | 2 GB |
| CPU | 2 Cores |
| Disk | 30 GB |
| Network | VMnet8 (NAT) |

The Kali VM was deliberately kept lightweight at 2GB RAM to leave sufficient headroom for the Wazuh SOC server operating at 4GB. The hardware configuration in VMware is shown below:

<img src="screenshots/agent/kali_vm_hardware_config.png" width="800">

### 8.2 Network Connectivity Verification

Before installing the agent, internal network connectivity between the two VMs was validated. Without this, agent registration will silently fail or time out — a step that's easy to skip and painful to debug later.

The Kali IP address was confirmed first to ensure it received a valid NAT subnet assignment:

```bash
ip a
```

<img src="screenshots/agent/kali_ip_address.png" width="800">

A ping test to the Wazuh Manager IP then confirmed that both VMs can communicate over the internal network:

```bash
ping <server-ip_address>
```

Successful ICMP responses below confirm the path is open and no firewall is blocking at the network layer:

<img src="screenshots/agent/kali_ping_manager_success.png" width="800">

### 8.3 Installing the Wazuh Agent

The Wazuh agent was installed on Kali using the official repository. The Manager IP is passed in as an environment variable during installation so the agent knows where to register:

```bash
# Add GPG key
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor -o /usr/share/keyrings/wazuh.gpg

# Add repository
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list

# Install agent pointed at the SOC server
sudo apt update
sudo WAZUH_MANAGER='192.168.1.136' apt install wazuh-agent -y

# Enable and start
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

The GPG key addition is shown below — this authenticates the Wazuh package repository and prevents unsigned packages from being installed:

<img src="screenshots/agent/wazuh_gpg_key_added.png" width="800">

The repository was then added to the apt sources list, making Wazuh packages available for installation:

<img src="screenshots/agent/wazuh_repo_added.png" width="800">

The agent installation output below confirms the package downloaded, installed, and configured without errors:

<img src="screenshots/agent/wazuh_agent_installation.png" width="800">

Finally, the agent service status was checked to confirm it started correctly and is attempting to communicate with the manager:

<img src="screenshots/agent/wazuh_agent_service_running.png" width="800">

### 8.4 Secure Agent Registration (Key-Based Authentication)

Wazuh uses key-based authentication to establish a trusted relationship between agents and the manager. This prevents unauthorized endpoints from injecting data into the SIEM.

On the Ubuntu SOC server, a new agent entry was created for the Kali endpoint:

```bash
sudo /var/ossec/bin/manage_agents
```

The screenshot below shows the agent being added and a unique authentication key being generated:

<img src="screenshots/agent/manage_agents_add_kali.png" width="800">

That key was then imported on the Kali machine using the same utility, completing the mutual authentication handshake. The screenshot below shows the key import step on the endpoint side:

<img src="screenshots/agent/kali_import_agent_key.png" width="800">

After importing the key and restarting the agent, the manager was queried to confirm the registration was successful:

```bash
sudo /var/ossec/bin/agent_control -l
```

The output below confirms the agent was assigned an ID, its status is **Active**, and secure communication has been established:

<img src="screenshots/agent/agent_control_list_output.png" width="800">

### 8.5 Firewall Validation for Agent Communication

During initial connectivity testing, the agent was unable to complete registration. Investigation confirmed that ports 1514 and 1515 were blocked by UFW on the SOC server. These ports are required for agent data transmission and secure registration respectively:

```bash
sudo ufw allow 1514
sudo ufw allow 1515
```

The screenshot below shows the updated firewall rules after the ports were opened, resolving the connectivity issue:

<img src="screenshots/agent/if_server_firewall_blocks_port_1514.png" width="800">

### 8.6 Dashboard Agent Verification

With the agent registered and communicating, the Wazuh Dashboard was checked to confirm the endpoint appeared correctly. Under **Wazuh → Agents**, the `kali-linux` endpoint was visible with a status of **Active** and keepalive signals being received:

<img src="screenshots/agent/wazuh_dashboard_agent_active.png" width="800">

### 8.7 Log Generation & Event Validation

To confirm the full pipeline — from endpoint activity through to dashboard visibility — controlled events were generated on the Kali machine and traced end-to-end.

Authentication-related events were triggered first using invalid user attempts, as these are reliably captured by PAM and forwarded by the agent:

```bash
sudo su invaliduser
sudo su invaliduser
```

<img src="screenshots/agent/kali_auth_log_generation.png" width="800">

A package installation was also performed to generate system-level activity logs, confirming that package management events are captured alongside authentication events:

```bash
sudo apt install sl -y
```

<img src="screenshots/agent/kali_package_install_log.png" width="800">

Both event types were then located in the Wazuh Dashboard under Security Events, filtered to the `kali-linux` agent. The first screenshot shows the event list view confirming logs are being received and indexed:

<img src="screenshots/agent/wazuh_event_validation_kali1.png" width="800">

The second screenshot shows an expanded event detail, confirming rule matching, severity classification, and accurate timestamp correlation between the endpoint and the SIEM:

<img src="screenshots/agent/wazuh_event_validation_kali2.png" width="800">

The log pipeline is fully validated — events generated on the endpoint are collected, forwarded, indexed, rule-matched, and visible in the dashboard in real time.

---

## 9. Attack Simulation & Detection Validation

With the SIEM infrastructure operational and the log pipeline validated, controlled activity simulations were conducted on the Kali endpoint to confirm that the Wazuh detection engine responds correctly to real security-relevant events. The objective here is **validation of monitoring coverage** — confirming that the SIEM detects, classifies, and surfaces the right alerts, not just that logs are being received.

---

### 9.1 Authentication Abuse Simulation

**Objective:** Confirm that repeated failed authentication attempts generate alerts and are correctly classified by Wazuh.

Repeated invalid sudo attempts were executed on the Kali endpoint to generate a burst of authentication failure events:

```bash
sudo su invaliduser
sudo su invaliduser
sudo su invaliduser
```

The terminal output below shows the failed attempts as they occurred on the endpoint:

<img src="screenshots/attacks/auth_failed_generation.png" width="800">

In the Wazuh Dashboard, alerts were generated for the `kali-linux` agent within seconds. The screenshot below confirms the rule was triggered, the correct agent was attributed, a severity level was assigned, and the timestamps align accurately with when the attempts were made:

<img src="screenshots/attacks/auth_failed_alert.png" width="800">

---

### 9.2 Privilege Escalation Activity

**Objective:** Validate that privileged command execution is logged and surfaced as a security event.

Sudo enumeration and root session activity were performed to simulate privilege-related behavior:

```bash
sudo -l
sudo su -
exit
```

The execution is shown below as it appeared on the Kali endpoint:

<img src="screenshots/attacks/privilege_escalation_generation.png" width="800">

The Wazuh Dashboard generated corresponding alerts for the privileged activity. The screenshot below shows the alert detail, including the rule triggered, the severity assigned, and the agent attribution:

<img src="screenshots/attacks/privilege_escalation_alert.png" width="800">

---

### 9.3 Suspicious Tool Installation

**Objective:** Confirm that package installation events are monitored and logged — relevant in scenarios where an attacker installs tools post-compromise.

A network scanning tool was installed on the Kali endpoint to generate package management activity:

```bash
sudo apt install nmap -y
```

The terminal output below shows the installation as it ran on the endpoint:

<img src="screenshots/attacks/nmap_install_generation.png" width="800">

Wazuh logged the package activity and surfaced it as a system event in the dashboard. The screenshot below confirms the event was captured, indexed, and correctly attributed to the `kali-linux` agent:

<img src="screenshots/attacks/tool_installation_alert.png" width="800">

---

### 9.4 File Integrity Monitoring (FIM) Validation

**Objective:** Verify that Wazuh's File Integrity Monitoring detects and alerts on modifications to monitored files.

The `/etc/hosts` file was modified to simulate an unauthorized change to a critical system file:

<img src="screenshots/attacks/file_modification_generation.png" width="800">

Wazuh FIM detected the change and generated an alert. The screenshot below shows the FIM alert in the dashboard, confirming the exact file path was monitored, the change was detected in near real-time, and the event was correctly attributed to the `kali-linux` agent with an accurate timestamp:

<img src="screenshots/attacks/file_integrity_alert.png" width="800">

---

### 9.5 Detection Validation Summary

| Simulation | Alert Generated | Severity Assigned | Agent Attributed | Timestamp Accurate |
|---|---|---|---|---|
| Authentication abuse | ✅ | ✅ | ✅ | ✅ |
| Privilege escalation | ✅ | ✅ | ✅ | ✅ |
| Suspicious tool install | ✅ | ✅ | ✅ | ✅ |
| File integrity violation | ✅ | ✅ | ✅ | ✅ |

All four simulations confirmed that the monitoring pipeline is functioning correctly from endpoint activity through to dashboard alert visibility.

---

## 10. Lessons Learned, Limitations & Future Improvements

### 10.1 Controlled Network Architecture Matters

Deploying within a NAT-isolated environment kept the lab clean and predictable. Incremental firewall rule expansion — opening only what each service needed, when it needed it — was a practical discipline that mirrors real SOC deployment hygiene.

**Key takeaways:**
* Proper network segmentation is foundational to effective SOC design
* Minimal service exposure reduces unexpected behaviour and security risk
* Connectivity issues are easier to debug when changes are made incrementally

### 10.2 Resource Constraints in SIEM Deployments

Running the Wazuh all-in-one stack within a 4GB RAM VM made it clear why enterprise deployments use distributed architectures. The indexer is the most resource-hungry component — under normal conditions it was stable, but there was little headroom for spikes in log volume.

**Key takeaways:**
* Capacity planning before deployment prevents instability mid-project
* Log retention strategy directly affects storage consumption over time
* Understanding resource limits early helps make better architectural decisions

### 10.3 Detection Validation Is Not Optional

One of the most valuable lessons from this project: deploying a SIEM does not automatically mean it is detecting correctly. The attack simulation phase was essential — without it, there would be no proof the pipeline actually works end-to-end.

This is the difference between **infrastructure deployment** and **operational readiness** — a distinction that matters significantly in a real SOC environment.

### 10.4 Firewall Troubleshooting Is Part of the Process

The agent registration issue caused by blocked ports 1514 and 1515 was a practical reminder that security tooling often fails silently when network paths are blocked. Systematic troubleshooting — checking connectivity, then firewall rules, then service logs — resolved it efficiently.

### 10.5 Current Limitations

| Limitation | Impact |
|---|---|
| Single Linux endpoint only | No Windows telemetry, registry events, or process-level data |
| No log volume stress testing | Scalability under real-world conditions unvalidated |
| No EDR-level telemetry | Kernel-level and behavioural detection not included |
| Self-signed SSL certificate | Not production-ready; browser warnings in all sessions |

### 10.6 Planned Future Improvements

| Enhancement | Purpose |
|---|---|
| Add Windows VM + Sysmon | Process creation, registry, and persistence detection |
| Deploy Suricata IDS | Network-layer signature-based detection |
| Threat intelligence integration | IP reputation, domain blacklist, GeoIP enrichment |
| Custom detection rules + tuning | Reduce false positives, improve signal quality |
| Distributed Wazuh architecture | Separate Manager, Indexer, and Dashboard for scalability |

---

## 11. Final Project Conclusion

This project successfully delivered a fully deployed, validated, and operational SOC home lab using Wazuh SIEM.

**Capabilities confirmed through this project:**

* Endpoint onboarding and secure key-based agent registration
* Real-time log ingestion and OpenSearch indexing
* Alert generation and dashboard visibility
* File integrity monitoring
* Privilege escalation event detection
* Authentication abuse detection
* End-to-end pipeline validation through controlled simulations

The environment provides a solid and documented foundation for advanced detection engineering, blue-team exercises, and future SOC lab expansion across the remaining phases of this series.

---

<p align="center">
  <sub>
    Part of the <strong>SOC Home Lab Series</strong> by <a href="https://github.com/kripy17">Krish Patel</a><br><br>
    <a href="../phase-1-wazuh-deployment">Phase 1: SIEM Infrastructure</a> ✅ &nbsp;|&nbsp;
    <a href="../phase-2-windows-sysmon">Phase 2: Windows + Sysmon</a> ✅ &nbsp;|&nbsp;
    <a href="../phase-3-attack-simulation">Phase 3: Attack Simulation</a> 📋
  </sub>
</p>
