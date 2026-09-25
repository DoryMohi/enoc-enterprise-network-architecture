# 🧮 Variable Length Subnet Masking (VLSM) IP Allocation Plan

This document outlines the IP addressing scheme developed for the Elbonia National Oil Company (ENOC) 12-storey headquarters building using Variable Length Subnet Masking (VLSM).

---

## 📌 Base Network Calculation

The base network block was derived from the Student ID (`00018919`):
* **Sum of ID Digits:** $0 + 0 + 0 + 1 + 8 + 9 + 1 + 9 = 28$
* **Base Network Address:** `28.0.0.0/8`
* **Default Subnet Mask:** `255.0.0.0`

---

## 📊 Host Requirement Analysis

| Network Segment / Zone | Floor Location | Estimated Host Devices | Rationale & Device Types |
| :--- | :--- | :--- | :--- |
| **Public Wi-Fi Zone** | Floor 1 | ~500 hosts | High-density public visitors, retail customers, and guest devices. |
| **ENOC Department 1** | Floor 8 | ~100 hosts | Corporate PCs, IP phones, network printers, and internal Wi-Fi. |
| **ENOC Department 2** | Floor 9 | ~100 hosts | Corporate PCs, IP phones, network printers, and internal Wi-Fi. |
| **ENOC Department 3** | Floor 10 | ~100 hosts | Corporate PCs, IP phones, network printers, and internal Wi-Fi. |
| **ENOC Department 4** | Floor 11 | ~100 hosts | Corporate PCs, IP phones, network printers, and internal Wi-Fi. |
| **ENOC Department 5** | Floor 12 | ~100 hosts | Corporate PCs, IP phones, network printers, and internal Wi-Fi. |
| **Tenant Office 1** | Floor 2 | ~40 hosts | 15 leased office suites, workstations, printers, and tenant Wi-Fi. |
| **Tenant Office 2** | Floor 3 | ~40 hosts | 15 leased office suites, workstations, printers, and tenant Wi-Fi. |
| **Tenant Office 3** | Floor 4 | ~40 hosts | 15 leased office suites, workstations, printers, and tenant Wi-Fi. |
| **Tenant Office 4** | Floor 5 | ~40 hosts | 15 leased office suites, workstations, printers, and tenant Wi-Fi. |
| **Tenant Office 5** | Floor 6 | ~40 hosts | 15 leased office suites, workstations, printers, and tenant Wi-Fi. |

---

## ⚙️ Subnet Sizing & Mask Calculations

Subnets are arranged in descending order of size to ensure contiguous block allocation without address overlap:

### 1. Public Wi-Fi Network
* **Hosts Needed:** 500
* **Formula:** $2^9 - 2 = 510$ usable IP addresses
* **CIDR Block:** `/23` (`255.255.254.0`)

### 2. ENOC Department Floors (Floors 8–12)
* **Hosts Needed per Floor:** 100
* **Formula:** $2^7 - 2 = 126$ usable IP addresses
* **CIDR Block:** `/25` (`255.255.255.128`)

### 3. Tenant Leased Floors (Floors 2–6)
* **Hosts Needed per Floor:** 40
* **Formula:** $2^6 - 2 = 62$ usable IP addresses
* **CIDR Block:** `/26` (`255.255.255.192`)

---

## 🗺️ Complete VLSM Subnet Table

| Subnet Name | Network Address | CIDR / Mask | Usable Host Range | Broadcast Address | Available Hosts |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Public Wi-Fi** | `28.0.0.0` | `/23`<br>`255.255.254.0` | `28.0.0.1 - 28.0.1.254` | `28.0.1.255` | 510 |
| **ENOC Floor 8** | `28.0.2.0` | `/25`<br>`255.255.255.128` | `28.0.2.1 - 28.0.2.126` | `28.0.2.127` | 126 |
| **ENOC Floor 9** | `28.0.2.128` | `/25`<br>`255.255.255.128` | `28.0.2.129 - 28.0.2.254` | `28.0.2.255` | 126 |
| **ENOC Floor 10** | `28.0.3.0` | `/25`<br>`255.255.255.128` | `28.0.3.1 - 28.0.3.126` | `28.0.3.127` | 126 |
| **ENOC Floor 11** | `28.0.3.128` | `/25`<br>`255.255.255.128` | `28.0.3.129 - 28.0.3.254` | `28.0.3.255` | 126 |
| **ENOC Floor 12** | `28.0.4.0` | `/25`<br>`255.255.255.128` | `28.0.4.1 - 28.0.4.126` | `28.0.4.127` | 126 |
| **Tenant Floor 2** | `28.0.4.128` | `/26`<br>`255.255.255.192` | `28.0.4.129 - 28.0.4.190` | `28.0.4.191` | 62 |
| **Tenant Floor 3** | `28.0.4.192` | `/26`<br>`255.255.255.192` | `28.0.4.193 - 28.0.4.254` | `28.0.4.255` | 62 |
| **Tenant Floor 4** | `28.0.5.0` | `/26`<br>`255.255.255.192` | `28.0.5.1 - 28.0.5.62` | `28.0.5.63` | 62 |
| **Tenant Floor 5** | `28.0.5.64` | `/26`<br>`255.255.255.192` | `28.0.5.65 - 28.0.5.126` | `28.0.5.127` | 62 |
| **Tenant Floor 6** | `28.0.5.128` | `/26`<br>`255.255.255.192` | `28.0.5.129 - 28.0.5.190` | `28.0.5.191` | 62 |

---

## 🔒 Security & Route Summarization Advantages

1. **Logical Isolation:** Each subnet maps to an isolated VLAN ID on the core switch, ensuring that broadcast traffic is contained strictly within individual floors/tenants.
2. **Access Control (ACLs):** Inter-VLAN routing policies applied on the Cisco Catalyst 9300 core switch restrict tenant and guest subnets (`28.0.4.128/26`–`28.0.5.128/26` and `28.0.0.0/23`) from initiating traffic into internal ENOC department subnets (`28.0.2.0/25`–`28.0.4.0/25`).
3. **Route Summarization:** All internal ENOC floors can be summarized as `28.0.2.0/23` on upstream firewall policies, significantly reducing routing table complexity.
