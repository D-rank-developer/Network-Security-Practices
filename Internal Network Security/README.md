# Internal Network Security Lab: Gateway/Router + Web Server + Workstation (VirtualBox)

**Author:** Dumanyie Chamberlain (D-rank-developer)  
**Focus:** Network simulation, routing, and secure traffic control using an Ubuntu Gateway VM  
**Platform:** VirtualBox (Windows)  
**Lab Type:** Practical – Internal network routing + firewall + IP forwarding

---

## Overview

In this lab, I set up an **Ubuntu Server VM as a Gateway/Router** and connected **two additional VMs** on separate internal networks. I then configured:

- VirtualBox network adapters (NAT + 2 Internal Networks)
- Static IP addressing
- IP forwarding (routing)
- Firewall rules to control and secure traffic
- Connectivity tests (ping, route checks, internet access through the gateway)

This lab demonstrates how a gateway controls traffic between isolated internal networks and the internet.

---

## Learning Outcomes

- Configure and deploy virtualised Ubuntu environments using VirtualBox for network simulation and testing.
- Implement and manage firewall rules and IP forwarding to secure and control network traffic.
- Scope, configure, deploy, and maintain network assets including routers, servers, and workstations.

---

## Required Tools

- **VirtualBox** (installed on university PC / Windows PC)
- **Ubuntu Server 24.04** (Gateway VM)
- **Ubuntu Server 22.04** (Web Server VM)
- **Ubuntu Desktop 22.04** (Workstation VM)

---

## Network Design (Diagram)

Below is the diagrammatic overview of my setup:

![Network Diagram](https://raw.githubusercontent.com/D-rank-developer/Network-Security-Practices/82cabd8eb30e038bb8faaa16b713cb04150ce538/Internal%20Network%20Security/VM%20config/image1.png)

---

## VM Network Configuration (VirtualBox)

### 1) Gateway VM (Ubuntu Server 24.04)
**Purpose:** Router + firewall + internet gateway

- **Adapter 1:** NAT (internet access) → IP via DHCP
- **Adapter 2:** Internal Network 1 (`intnet1`) → **Static IP: `192.168.100.1/24`**
- **Adapter 3:** Internal Network 2 (`intnet2`) → **Static IP: `192.168.200.1/24`**

---

### 2) Web Server VM (Ubuntu Server 22.04)
**Purpose:** Internal service host (web server role)

- **Adapter:** Internal Network 1 (`intnet1`) → **Static IP: `192.168.100.2/24`**
- **Gateway:** `192.168.100.1`
- **DNS:** `8.8.8.8` (or your preferred DNS)

---

### 3) Workstation VM (Ubuntu Desktop 22.04)
**Purpose:** Admin/user workstation for testing access and routing

- **Adapter:** Internal Network 2 (`intnet2`) → **Static IP: `192.168.200.2/24`**
- **Gateway:** `192.168.200.1`
- **DNS:** `8.8.8.8`

---

## IP Address Plan

| VM | Role | Network | IP Address | Gateway |
|---|---|---|---|---|
| Gateway (Ubuntu 24.04) | Router/Firewall | NAT + intnet1 + intnet2 | `192.168.100.1`, `192.168.200.1` | NAT via DHCP |
| Web Server (Ubuntu 22.04 Server) | Server | intnet1 | `192.168.100.2` | `192.168.100.1` |
| Workstation (Ubuntu 22.04 Desktop) | Client | intnet2 | `192.168.200.2` | `192.168.200.1` |

---



