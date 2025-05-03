# LAB-DC01 – Active Directory Domain Controller

**LAB-DC01** is the **primary Active Directory Domain Controller (AD DC)** for the RougeLabs Cyber Lab. It serves as the backbone of the lab’s authentication and directory infrastructure.

---

## 🖥️ Overview

| Item               | Details                                              |
|--------------------|------------------------------------------------------|
| Hostname           | LAB-DC01                                             |
| OS                 | Windows Server 2025 (or 2022)                        |
| Roles Installed    | Active Directory Domain Services, DNS Server         |
| Domain Name        | rougelabs.local                                      |
| IP Address         | 10.0.0.10 (static)                                   |
| VLAN               | Internal Lab VLAN (e.g., VLAN 10)                    |
| Snapshot Name      | LAB-DC01-BaseSetup                                   |
| QEMU Guest Agent   | ✅ Installed (via VirtIO ISO)                         |

---

## 🔧 Primary Functions

- **Active Directory Services:**
  - Centralized user, group, and machine authentication
  - Organizational Unit (OU) structure for lab machines and users

- **DNS Services:**
  - Internal name resolution for `rougelabs.local`
  - DNS forwarding to public DNS servers for external lookups

- **Group Policy (Optional):**
  - Manage security policies and system configurations across the lab

---

## 🗂️ Organizational Units & Test Users

Example AD structure:

---

## 🔑 Test Credentials (Example)

| Username   | Role            | Password    |
|------------|-----------------|-------------|
| labadmin   | Domain Admin    | Passw0rd!   |
| user1      | Standard User   | Passw0rd!   |
| user2      | Standard User   | Passw0rd!   |

*(⚠️ Change these in your own lab for security.)*

---

## 🛠️ Setup Summary

1️⃣ Windows Server installed with TPM + EFI  
2️⃣ Hostname set to `LAB-DC01`  
3️⃣ Static IP: `10.0.0.10`  
4️⃣ Roles added:
   - Active Directory Domain Services
   - DNS Server  
5️⃣ Promoted to domain controller: `rougelabs.local`  
6️⃣ DNS forwarders configured  
7️⃣ Organizational Units + test users created  
8️⃣ IE Enhanced Security disabled (optional)  
9️⃣ QEMU Guest Agent installed (for Proxmox integration)  
🔟 Snapshot taken after base configuration

---

## 🌐 Network Configuration

| Field            | Value             |
|------------------|-------------------|
| IP Address       | 10.0.0.10         |
| Subnet Mask      | 255.255.255.0     |
| Default Gateway  | 10.0.0.1          |
| DNS Server       | 10.0.0.10         |
| Alternate DNS    | 1.1.1.1           |

---

## 🔒 Security Notes

- Always keep the **DSRM password** stored securely.
- This VM should only be used in **isolated lab environments.**
- Use snapshots to revert to a clean state if compromised.

---

## 📝 Author

**RougeLabs (Dylan Barrett)**  
[https://dylanbarrett.work](https://dylanbarrett.work)

License: [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)
