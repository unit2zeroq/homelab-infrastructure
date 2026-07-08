# Homelab Infrastructure

A hands-on engineering environment dedicated to system administration, network security, enterprise directory services, and redundant storage architectures. This repository tracks the hardware configurations, network topology, deployment steps, and documentation for all active and in-progress labs.

## Hardware Inventory & Topology

| Device Name | Type | Compute | RAM | Storage | Primary Role |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Workstation** | Desktop PC (Custom) | AMD Ryzen 5 5600X | 16GB DDR4 (G.Skill) | 500GB Boot SSD<br>1TB NVMe SSD | Primary workstation & Type-2 Hypervisor (Ubuntu VM, Windows Server 2025 VM) |
| **Server & NAS** | Desktop PC (OptiPlex 3060) | Intel i5-8500T | 32GB DDR4 | 256GB Boot SSD<br>2x 4TB WD Red Plus HDDs | Dedicated Homelab Server, Software RAID1 NAS, and Docker host |
| **Raspberry Pi** | Single-Board Computer | Pi 5 (1GB) | 1GB LPDDR4X | 32GB Class 10 microSD | DNS Sinkhole, network-wide ad blocking, and lightweight service monitoring |
| **TP-Link Switch** | Managed Switch (8-Port) | TL-SG208E | N/A | Gigabit Line Rate | Core lab layer-2 switch (configured for future 802.1Q VLAN segmentation) |

---

## Deployed Services & Labs

### 1. Active Directory Domain Services (AD DS)
* **Status:** In-Progress / Validated
* **Environment:** Windows Server 2025 Datacenter Edition (DC01) & Windows 11 Pro Client VM
* **Domain:** `homelab.local`
* **Core Core Components:** Integrated DNS, Global Catalog, Group Policy Management, RSAT Tools.
* **Implementation Summary:** Deployed a new forest root domain controller with a centralized static IP architecture. Successfully executed a workstation domain-join with a Windows 11 client VM. Verified end-to-end authentication, domain identity verification (`whoami`), and client-side group policy processing (`gpresult /r`).
* **Next Phase:** Build out a structured Organizational Unit (OU) hierarchy by department and implement custom Group Policy Objects (GPOs) to simulate enterprise desktop containment.

### 2. Network-Attached Storage (NAS) Storage Solution
* **Status:** Completed
* **Environment:** Dell OptiPlex 3060 | Ubuntu Server
* **Core Components:** `mdadm` (Software RAID), Samba (SMB), Docker Engine.
* **Implementation Summary:** Provisioned a highly available storage solution utilizing two 4TB NAS-grade hard drives configured in a software RAID1 array via `mdadm`. Integrated a write-intent bitmap to optimize resync recovery times, formatted the block array with `ext4`, and configured persistent mounting via `/etc/fstab` using unique filesystem UUIDs. Established a network share via Samba for cross-platform Windows access and configured Docker for future containerized application rollouts.

### 3. Pi-hole Network-Wide Ad Blocker
* **Status:** Completed
* **Environment:** Raspberry Pi 5 | Raspberry Pi OS Lite (64-bit)
* **Core Components:** Pi-hole core, Cloudflare Upstream DNS, SSH.
* **Implementation Summary:** Deployed an open-source DNS sinkhole to achieve network-wide ad and tracker mitigation. Housed the Pi 5 in an Argon POLY+ 5 case, configured a static IP layout, and pointed the upstream network gateway DNS to the Pi-hole instance. Enabled comprehensive query logging and custom blocklists.

### 4. Uptime Kuma Monitoring
* **Status:** Completed
* **Environment:** Raspberry Pi 5 | Docker Container
* **Core Components:** Docker Compose, Node-based Web Interface.
* **Implementation Summary:** Implemented a lightweight, self-hosted service availability and response-time tracker. Containerized via Docker Compose, Uptime Kuma monitors local hosts, VMs, and networking equipment across HTTP/HTTPS, TCP, and ICMP Ping protocols, providing live status telemetry via a custom administrative dashboard.

### 5. osTicket Self-Hosted Ticketing System
* **Status:** Completed
* **Environment:** Workstation Hypervisor | Ubuntu Server 22.04 LTS VM
* **Core Components:** Apache2, MySQL Server, PHP 8.x stack (LAMP).
* **Implementation Summary:** Erected a production-simulated service desk ticketing architecture. Installed and hardened a LAMP stack on Ubuntu, provisioned a dedicated `osticket` MySQL database with restrictive user privileges, and mapped web directory permissions (`www-data`). The platform acts as a sandbox environment for analyzing help desk workflows, intake triage, ticket routing, and SLA resolution tracking.

---

## Future Roadmap

* **Network Layer Segmentation:** Configure 802.1Q VLANs on the TP-Link TL-SG208E switch to isolate the production home network from experimental lab subnets (aligned with Network+ study goals).
* **Remote Access & Mesh Networking:** Deploy Tailscale to securely expose the internal Pi-hole instance and NAS resources to external client devices without opening hazardous WAN ports.
* **Enterprise Identity Hardening:** Construct department-specific OUs and implement granular GPOs (Password policies, execution restrictions, and mapping network shares natively via AD).
