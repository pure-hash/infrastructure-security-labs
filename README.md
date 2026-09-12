# Infrastructure & Cybersecurity Lab Portfolio

This repository documents my hands-on work across networking, cybersecurity, systems administration, cloud infrastructure, and automation.

Each project is built around a realistic technical or business scenario. Projects include architecture decisions, configuration files, implementation steps, validation evidence, troubleshooting, security considerations, and recommendations for improving the design in a production environment.

The purpose of this portfolio is to demonstrate practical skills beyond certification exams by showing how I design, build, test, troubleshoot, secure, and document technical environments.

## Lab Portfolio

| Area | Project | Technologies and Skills | Status |
|---|---|---|---|
| Networking and Security | [Secure Three-Floor Office Network](networking/secure-office-network/) | Cisco Packet Tracer, VLANs, 802.1Q, Layer 3 switching, DHCP relay, DNS, PAT, ACLs, port security, SSH and troubleshooting | Complete |
| Network Analysis | Packet Capture and Troubleshooting Lab | Wireshark, DNS, DHCP, TCP/IP and packet analysis | Planned |
| Cybersecurity | Detection and Incident Response Lab | Windows, Sysmon, SIEM, detection rules and incident analysis | Planned |
| Cybersecurity | Vulnerability Management Lab | Vulnerability scanning, prioritization, remediation and verification | Planned |
| Cloud | Secure AWS Infrastructure | AWS, VPC networking, IAM, logging and Terraform | Planned |
| Cloud Security | Cloud Detection Pipeline | CloudTrail, security monitoring and alerting | Planned |

## Featured Project

### Secure Three-Floor Office Network

Designed and implemented a segmented network for a fictional three-floor office supporting approximately 75 users.

The environment includes:

- Department and device segmentation using ten VLANs
- Layer 3 switching and inter-VLAN routing
- Centralized DHCP and DNS services
- DHCP relay across multiple VLANs
- Simulated ISP connectivity and NAT/PAT
- Guest wireless isolation
- Camera and IoT network restrictions
- SSH management limited to authorized IT systems
- Port security, PortFast and BPDU Guard
- Positive and negative validation testing
- Documented troubleshooting and remediation

[View the complete project →](networking/secure-office-network/)

## Repository Organization

Each project is stored in its appropriate technical area:

```text
networking/       Network design, routing, switching and packet analysis
cybersecurity/    Detection, vulnerability management and incident response
cloud/            Cloud infrastructure, networking and security
systems/          Windows, Linux and identity administration
automation/       Python, PowerShell, Terraform and workflow automation
