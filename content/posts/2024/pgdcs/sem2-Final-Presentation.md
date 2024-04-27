---
title: Sem-2 Final Presentation
date: 2024-04-23T02:12:53+05:30
url: /sem2-Final-Presentation/
categories:
  - Networking
  - Security
  - Courses
  - Notes
tags:
  - XDR
  - BPC
  - Business
  - Parody
draft: false
author: aayan
showauthor: false
showDateOnlyInArticle: false
showDateUpdated: true
showHeadingAnchors: false
showPagination: false
showReadingTime: true
showTableOfContents: true
showTaxonomies: true
showWordCount: true
sharingLinks: false
showEdit: false
showViews: false
showLikes: false
showSummary: false
summary: ""
---
> Aayan Ta | 23rd April, 2024 | Final Presentation | Parody company | 02:16

## Business Name: *InCla* (parody)
---

### Business Model
---

- IT systems retailer and service provider specializing in small scale infrastructure such as servers, networking equipment, along with IT training services both remote and on-site.

### Location
---

- Based in Mumbai with a physical store for infrastructure sales and IT training facility equipped with advanced infrastructure.



### Services Provided
---
- **IT infrastructure sales** (on-site for now): 
	- EPIC Servers (64-cores)
	- Networking equipment: routers, switches
	- Air conditioning units
- **Training Facilities**:
	- Training room setting with 30 seats for hands-on training sessions (20 reserved) (queuing theory), equipped with the latest IT infrastructure for practical learning experiences.
	- Pre-recorded tutorials and training sessions that are available for purchase on the website (to be discussed in cloud infrastructure below).
    - On-Site training sessions at client locations is provided by our certified trainers.
    - **What kind of Tutorials?**
		- Cybersecurity awareness drives
			- Fundamentals
			- Advance labs: "How to respond to cybersecurity incidents"
			- Training on compliance like GDPR for data protection. 
			- Incident Response and Disaster Recovery
		- Operational Proficiency
			- Hardware Management and Troubleshooting (systems)
			- Software Installation and Configuration
			- Network Setup


### Physical Security
---
- **Access Control:** Biometric and card-based access controls at all entry points to secure physical access to the IT infrastructure.
- **Surveillance**: Utilize CCTV systems networked through high-speed connections to monitor premises 24/7.
- **How is this data stored?**
	- capable of communicating with your central security system via wired or wireless networks
	- communicate with our central system (name change) using wires.
	- Store and encrypt biometric and card information in a secure central database with AES-256.
	- Access Control Software
	- Integrate devices with Security Center software for managing access permissions and monitoring entry logs.
	- Monitor access attempts in real time; generate alerts for unauthorized access.





### Infrastructure Components
---
- **Operational IT Systems (on-prem):**
	- Utilize PostgreSQL databases to store sensitive customer data, ensuring GDPR compliance for data protection.
	- Dedicated systems for sales team operations, support for training related queries and IT support for the products being sold.
	- 60 Computers running the latest security patched windows 11 or stable release linux, networking devices for trainers, content creation department.
	- Additional 25 computing system that can be utilized by employees is reserved for business continuity during any disaster.
	- Robust AD policies applied on systems used by employees for access control. The AD is connected to our.

- **Server Room Configuration**:
	- Supermicro server configured with openSense, serving as both a **router** and a firewall. This setup allows efficient network management and enhanced security through VLANs and firewall policies.
	- Dual ISP connections with Threesa Infoway (local ISP with 99.99% uptime) and Reliance Jio (redundant) ensure high availability and redundancy.
	- A 48-port 25G **switch** for high-speed employee connections, and two 10G switches dedicated to security cameras, WiFi access points, and Network Video Recorder (**NVR**) systems.
	- 64-core Threadripper configured with Proxmox for **access control** server with Active Directory, **Wazuh** (XDR) for monitoring of traffic, **OpenVPN** for connecting site-to-site for trainers.
	- **Archive and Backup Server** runs TrueNAS on a 32-core CPU server with 2 x 20 T.B. SSD for fast access and 20 x 2.5 PB HDD for long-term storage, complemented by 100 GB of DDR5 memory, ensuring robust data integrity and retrieval capabilities. 
		- Note: Hourly backup on site and on cloud (to be discussed in cloud infrastructure below)
	- 64-core Threadripper with **Proxmox server** for hosting VMs for testing, K8s clusters, and Docker containers, also used for recording training videos. Essential for educational and training use.
	- 2 x 64-core EPYC servers with 384 GB RAM each, dedicated to video editing and **content creation**; 2 ensuring redundancy and high availability.

- **Cloud Infrastructure:**
	- Website (referenced in training facilities): Customers get to us through a simple static website hosted using hugo and on an azure VM. The data captured is stored on the PostgreSQL and sent on site to the  
	- User data redundancy: Utilize cloud storage for secure database backups, employing encryption for data at rest (AES-256) and in transit (SSL/TLS) to enhance security.

- **Offsite Infrastructure for Backup (accessed using dedicated line):**
	- 64-core CPU server with 1 x 20 T.B. SSD and 40 x 2.5 PB HDD with 200 GB DDR5 memory running TrueNAS.
	- Backups performed every 12 hours.


### Compliance
---

- **GDPR (General Data Protection Regulation):**
	- Types of data collected (first name, last name, email, phone number, address, status, payment method)
	- Customer consent using 
	- Data Protection Officer is appointed
	- If breach happens, users will be notified within 42 hrs (72 hrs is a limit)

- **ISO 27001:**
	- Define the information, assets, and areas that ISMS will cover
	- Risk assessment
	- Risk mitigation
	- Training and awareness
	- Continuous improvement

- **GST (Goods and Services Tax):**
	- GST registered organisation.
	- Accounting software used by the sales team, data is stored in PostgreSQL
	- Invoice management (saved in the database)
	- Regular filling of GST returns as per the timeline and documentation is also done
	- Educate the finance and accounting team


- **PCI DSS (Payment Card Industry Data Security Standard):**
	- Card holder data environment (stored in the database)
	- Access control established so that only the employees who need it can access it.
	- Create annual internal audit report and submit to the bank or card brand we use.




### Cyber Security and Resilience
---
- **Business Continuity and Disaster Recovery (BCDR):**
    - Comprehensive BCDR plans that include regular backups to on-premise, cloud, and off-site storages, using encrypted channels for data transmission to ensure security and compliance.
    - Conducting regular tests and audits (Internal every 2 months, Externally every 12 months) of the backups and reliability of our customer data. Making necessary adjustments based on outcomes.
- **Incident Response Planning (IRP):**
    - Well-defined playbooks and communication plans to quickly address any security incidents, minimizing potential damage and downtime.

---

## Hypothetical Scenario
---
***Ransomware Attack on InCla (parody)***

**Background:** InCla (parody), an IT systems and services company in Mumbai, encounters a ransomware attack that encrypts customer data stored on their on-premise servers.

**Detection and Immediate Response:**
- The attack is detected by the XDR (Wazuh) system noticing unusual activity.
- Incident response team is activated, isolating affected systems and stopping the malware's spread using existing VLANs.

**Mitigation and Recovery:**
- Encrypted data is restored from backups stored on cloud for the customer data and TrueNAS systems which was off-site.
- External experts were hired to remove the ransomware, and restore clean systems to operation.

**Post-Incident Review and Lessons Learned:**
- Advanced email filtering is implemented to prevent phishing attacks.
- **Regular Backup Testing:** Backup and recovery processes are tested more frequently to ensure their effectiveness in real incidents.
- Incident response plan is revised to incorporate lessons learned, particularly around handling ransomware.

**Conclusion:** With robust IRP and BCDR strategies, InCla managed to recover with minimal disruption and improved their security posture to better handle future threats.


---

> Aayan Ta | Edit: 27th April, 2024 | General Feedback
## Presentation Feedback:
- Keeping it relevant throughout (not to explain topics like a lecture - they should be to the point)
- Prepare many designs + models
- Prepare or ask or design the structure in such a way that the flow is relevant (*my presentation feedback* - head points were up and down)
- A lecture is a lecture. Broad topics must use pt. 1
- Professional terminologies
- Hand eye contact
- Speaking stuff, but the the points are not on the slide
	- WAF is being spoken, but no keywords on the slide
	- Amount charged to the company (GDPR)
