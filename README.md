# RougeLabsProxmoxADLab
🚨 **A full-scale cybersecurity lab powered by Proxmox, designed for both offensive (red team) and defensive (blue team) training.** This lab simulates a real-world enterprise network with automated Windows and Linux deployments, VLAN segmentation, and attack/defense scenarios.

---

## 🔥 What's Included

| Component                        | Description                                                                                   |
|----------------------------------|-----------------------------------------------------------------------------------------------|
| **Windows Server 2025**          | Active Directory Domain Controller (rougelabs.local)                                          |
| **Windows Server 2022**          | Exchange Server + IIS                                                                          |
| **Windows 10 & Windows 11**      | Client machines for red/blue team operations                                                   |
| **Ubuntu (LAMP Stack)**          | Web server in DMZ for web app testing (e.g., DVWA, WordPress)                                  |
| **Ubuntu (Blue Team Box)**       | Defensive tools (Wazuh, Snort, etc.)                                                           |
| **Kali Linux**                   | Red team box (offensive tools)                                                                 |
| **IIoT VLAN**                    | Simulated Industrial IoT environment                                                           |
| **Proxmox VE**                   | The full lab runs inside Proxmox VE with VLAN tagging and snapshots                            |
| **VirtIO Drivers**               | Integrated for smooth Windows VM installs                                                      |

---

## 🛠️ Features

- **Automated Install:**  
  - `autounattend.xml` for Windows Server 2025 + 2022  
  - Integrated PowerShell scripts to set up AD, users, DNS, and disable IE Enhanced Security

- **Network Segmentation:**  
  - DMZ VLAN for external-facing services  
  - IIoT VLAN for isolated industrial systems  
  - Red, Blue, and White team segmentation

- **ISO Customization:**  
  - Custom-built ISOs with VirtIO drivers and automation scripts  
  - Step-by-step guide for ISO rebuilding using `oscdimg`

- **Proxmox Snapshots:**  
  - Clean rollback strategy to maintain a fresh lab state  
  - Optional full VM backups

---

## 🖥️ Lab Architecture (Planned)

```mermaid
graph TD;
    Internet --> Firewall
    Firewall --> DMZ
    Firewall --> Internal_Network
    DMZ --> LAMP_Server
    Internal_Network --> AD_DC
    Internal_Network --> Exchange_Server
    Internal_Network --> BlueTeam_Box
    Internal_Network --> Windows10_Client
    Internal_Network --> Windows11_Client
    Internal_Network --> RedTeam_Box
    Internal_Network --> IIoT_VLAN
