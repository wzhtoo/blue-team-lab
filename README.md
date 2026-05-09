## **SOC Analyst Lab: Infrastructure & Endpoint Hardening**

This laboratory project documents the implementation of network monitoring, security controls, and system hardening following best practices and **CIS Benchmarks**.

### **Project Structure Overview**

#### **01. Monitor Traffic**

This section focuses on monitoring and analyzing traffic flowing within the network.

- **Tshark:** Capturing and analyzing network traffic via the command line.
- **Zeek (Bro):** Performing Network Security Monitoring (NSM) and generating detailed traffic logs.
- **Suricata:** Implementing an Intrusion Detection System (IDS) to generate alerts for malicious activity.

#### **02. Firewalls**

Setting up firewalls as the primary perimeter defense for network security.

- **pfSense:** Utilizing an enterprise-grade open-source firewall for network segmentation and granular traffic control.

#### **03. Windows 10 Endpoint**

Strengthening user-end Windows machines through **Endpoint Hardening**.

- **CIS Compliance:** Adjusting Windows 10 settings to align with official CIS Benchmarks.
- **Services Hardening:** Reducing the attack surface by disabling unnecessary services.
- **Sysmon Installation:** Deploying Microsoft Sysmon to enable advanced event logging.

#### **04. Infrastructure Hardening**

Protecting servers and core infrastructure using global CIS standards.

- **Linux Hardening:** Monitoring Linux servers using `auditd`. This implementation is based on the **CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0**.

---

### **Key Learning Objectives**

- **Visibility:** Gaining deep insights into network and host-level activities through log analysis.
- **Hardening:** Modifying systems to be significantly more secure than default configurations.
- **Compliance:** Gaining practical experience in applying world-class **CIS Benchmarks**.

### **Resources & References**

- **Windows 10 CIS Benchmark:** Referenced for all Endpoint hardening sections.
- **Ubuntu 22.04 CIS Benchmark v3.0.0:** Referenced for Infrastructure hardening (specifically the Auditd section).
- **Tool Documentation:** Official manuals for Tshark, Zeek, Suricata, and pfSense.
- **Section 4.1:** Configuration of System Accounting (`auditd`) to meet **Level 2 (High Security)** requirements for both Servers and Workstations.

### **Project Status**

**Work in Progress (WIP):** This laboratory project is currently under active development. I am testing security controls and documenting the results in real-time. Upcoming updates will include **Active Directory (Keymaster)** setup and **SIEM integration**. Stay tuned!
