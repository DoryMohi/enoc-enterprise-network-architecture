# 🌐 ENOC Headquarters Enterprise Network Architecture

A comprehensive enterprise network design and infrastructure proposal for the 12-storey Elbonia National Oil Company (ENOC) headquarters in Elbopolis. The solution enforces strict logical isolation between corporate departments, tenant leased offices, and public Wi-Fi zones while maintaining high availability, scalable bandwidth, and cloud data ingestion capabilities.

---

## 📌 Architectural Overview

* **Topology:** Collapsed Core / Distribution Layer topology deployed on Floor 7 (Technical Floor).
* **Perimeter Security:** FortiGate 400F NGFW handling NAT, IPsec VPN termination, and threat prevention.
* **Core Switching:** Cisco Catalyst 9300 Series (Layer 3) managing inter-VLAN routing and 10G SFP+ uplinks.
* **Access Layer:** Cisco Catalyst 9200L access switches paired with 24-port PoE+ switches per floor.
* **Wireless Infrastructure:** 58 Cisco Catalyst 9120AXI Wi-Fi 6 Access Points centrally managed across ENOC floors, tenant spaces, and public ground floor.
* **Physical Cabling:** 10G Fibre-Optic vertical backbone links paired with Cat6 horizontal UTP runs.

---

## 🔒 Security & Logical Segmentation

1. **VLAN Isolation:** Separate VLANs assigned to each ENOC department, individual tenant offices (Floors 2–6), and public guests (Floor 1) to eliminate cross-tenant access.
2. **Access Control Policies:** Core switch Layer 3 routing rules block tenant and guest networks from reaching corporate assets.
3. **Switch-Level Hardening:** Port Security (MAC-limiting), Spanning Tree Protocol (STP) loop protection, and sticky bit permissions.
4. **Guest Network Security:** Dedicated Guest VLAN restricted strictly to outbound Internet access.

---

## 📡 ISP & Cloud Architecture

* **ISP Selection:** Comnet Telecom — Selected for 1 Gbps symmetric fiber connectivity paired with a **99.9% SLA uptime guarantee**.
* **Real-Time Data Ingestion:** **Amazon Kinesis Data Streams** — Selected over Azure Event Hubs and GCP Pub/Sub for automatic scaling, high-throughput streaming, and persistent voting event storage during application outages.

---

## 🧮 IP Addressing Scheme (VLSM)

Calculated based on base network `28.0.0.0/8` using Variable Length Subnet Masking (VLSM):

| Segment / Location | Network Address | Mask | Usable Range | Max Hosts |
| :--- | :--- | :--- | :--- | :--- |
| **Public Wi-Fi (Floor 1)** | `28.0.0.0/23` | `255.255.254.0` | `28.0.0.1 - 28.0.1.254` | 500 |
| **ENOC Floor 8** | `28.0.2.0/25` | `255.255.255.128` | `28.0.2.1 - 28.0.2.126` | 100 |
| **ENOC Floor 9** | `28.0.2.128/25` | `255.255.255.128` | `28.0.2.129 - 28.0.2.254` | 100 |
| **ENOC Floor 10** | `28.0.3.0/25` | `255.255.255.128` | `28.0.3.1 - 28.0.3.126` | 100 |
| **ENOC Floor 11** | `28.0.3.128/25` | `255.255.255.128` | `28.0.3.129 - 28.0.3.254` | 100 |
| **ENOC Floor 12** | `28.0.4.0/25` | `255.255.255.128` | `28.0.4.1 - 28.0.4.126` | 100 |
| **Tenant Floor 2** | `28.0.4.128/26` | `255.255.255.192` | `28.0.4.129 - 28.0.4.190` | 40 |
| **Tenant Floor 3** | `28.0.4.192/26` | `255.255.255.192` | `28.0.4.193 - 28.0.4.254` | 40 |
| **Tenant Floor 4** | `28.0.5.0/26` | `255.255.255.192` | `28.0.5.1 - 28.0.5.62` | 40 |
| **Tenant Floor 5** | `28.0.5.64/26` | `255.255.255.192` | `28.0.5.65 - 28.0.5.126` | 40 |
| **Tenant Floor 6** | `28.0.5.128/26` | `255.255.255.192` | `28.0.5.129 - 28.0.5.190` | 40 |

---

## 💰 Capital Expenditure Summary

* **Firewall (FortiGate 400F):** $9,382.78
* **Core Switch (Cisco C9300L):** $2,397.00
* **Access Switches (21x 48-port C9200L):** $29,358.00
* **PoE+ Switches (11x 24-port C9200L):** $26,367.00
* **Wireless APs (58x Catalyst 9120AXI):** $67,694.86
* **Structured Cabling (Fibre + Cat6):** $25,000.00
* **Total Infrastructure CAPEX:** **~$160,199.64**

---

## 🎓 Academic Context

Developed for the **Network Operations** (5BUIS013C) module coursework at Westminster International University in Tashkent.
