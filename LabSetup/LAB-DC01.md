# RougeLabs Cyber Lab – LAB-DC01 Setup Guide

🚨 **LAB-DC01** is the Active Directory Domain Controller (AD DC) for the RougeLabs cyber lab. This guide walks through the **manual setup** of LAB-DC01 using Windows Server 2025 (or 2022) inside Proxmox VE.

---

## 🖥️ 1️⃣ Proxmox VM Creation

**VM Settings:**

| Setting               | Value                                           |
|-----------------------|-------------------------------------------------|
| Name                  | LAB-DC01                                        |
| OS                    | Windows Server 2025 (or 2022)                   |
| System BIOS           | OVMF (UEFI)                                     |
| Machine Type          | Q35                                             |
| EFI Disk              | ✅ Add EFI Disk                                 |
| TPM                   | ✅ Add TPM (version 2.0)                        |
| ISO                   | Mount official Server 2025 ISO                  |
| CD/DVD (optional)     | Mount VirtIO ISO (for drivers)                  |
| SCSI Controller       | VirtIO SCSI (or SATA if skipping VirtIO)        |
| Hard Disk             | 60GB+ (VirtIO SCSI or SATA)                     |
| CPU                   | 2+ cores                                        |
| Memory                | 4096MB (4GB)                                    |
| Network Device        | VirtIO (or E1000 if no VirtIO)                  |
| VLAN Tag              | Set to your internal lab VLAN (e.g., 10)        |
| Boot Order            | CD-ROM first, Disk second                       |

➡️ **Tip:** Set the **boot order** to prioritize the hard disk after the OS install is complete.

---

## 💽 2️⃣ Install Windows Server

1. Boot the VM from the ISO.
2. Proceed with a **standard install:**
   - Select: *Windows Server 2025 Standard (Desktop Experience)*
3. If you're using VirtIO disks:
   - Load the **VirtIO storage driver** during disk selection (from the VirtIO ISO).
4. Complete installation.

---

## ⚙️ 3️⃣ Initial Configuration

### ✅ Set Hostname

In **PowerShell (as Administrator):**

```powershell
Rename-Computer -NewName "LAB-DC01" -Restart
```

---

### ✅ Set Static IP Address

Example configuration:

| Field            | Value               |
|------------------|---------------------|
| IP Address       | `10.0.0.10`         |
| Subnet Mask      | `255.255.255.0`     |
| Default Gateway  | `10.0.0.1`          |
| DNS Server       | `10.0.0.10` (itself) |
| Alternate DNS    | e.g., `1.1.1.1`     |

Steps:

- Open **Network Connections**
- Right-click your adapter ➔ Properties ➔ IPv4 ➔ Enter the static IP info.

---

### ✅ Install QEMU Guest Agent (optional but recommended)

1. Mount the **VirtIO ISO.**
2. Run: `qemu-ga-x86_64.msi`
3. Verify the **QEMU Guest Agent service is running.**

---

## 📦 4️⃣ Add Roles & Features

Open **Server Manager ➔ Add Roles and Features:**

- **Roles:**
  - ✅ Active Directory Domain Services
  - ✅ DNS Server

- **Features:**
  - (Optional) Group Policy Management

Do **NOT** promote to Domain Controller yet—just install the roles for now.

---

## 🌐 5️⃣ Promote to Domain Controller

Once AD DS role is installed:

1. In Server Manager, click the notification flag ➔ **Promote this server to a domain controller.**
2. Select:
   - ✅ **Add a new forest**
   - Root domain name: `rougelabs.local`
3. Set the **Directory Services Restore Mode (DSRM) password.**
4. Accept defaults for:
   - DNS options
   - NetBIOS name: `rougelabs`
   - Paths
5. Review & Install.

💥 The server will **reboot automatically** after promotion.

---

## 🖧 6️⃣ Configure DNS Forwarders

- Open **DNS Manager**
- Right-click the server ➔ Properties ➔ **Forwarders tab**
- Add reliable public DNS servers (e.g., `1.1.1.1`, `8.8.8.8`)

---

## 👥 7️⃣ Create Organizational Units (OUs) and Users

### ✅ Open: **Active Directory Users and Computers**

Example structure:

```
rougelabs.local
├── OU: Users
│   ├── user1
│   ├── user2
├── OU: Admins
│   ├── labadmin
├── OU: Workstations
│   ├── WIN10-CLIENT
│   ├── WIN11-CLIENT
```

---

### ✅ PowerShell Commands (Example):

```powershell
# Create OUs
New-ADOrganizationalUnit -Name "Users"
New-ADOrganizationalUnit -Name "Admins"
New-ADOrganizationalUnit -Name "Workstations"

# Create Users
New-ADUser -Name "labadmin" -SamAccountName "labadmin" -AccountPassword (ConvertTo-SecureString "Passw0rd!" -AsPlainText -Force) -Enabled $true
Add-ADGroupMember "Domain Admins" "labadmin"

New-ADUser -Name "user1" -SamAccountName "user1" -AccountPassword (ConvertTo-SecureString "Passw0rd!" -AsPlainText -Force) -Enabled $true
New-ADUser -Name "user2" -SamAccountName "user2" -AccountPassword (ConvertTo-SecureString "Passw0rd!" -AsPlainText -Force) -Enabled $true
```

---

## 🌐 8️⃣ Disable IE Enhanced Security (Optional)

- Open **Server Manager ➔ Local Server**
- Click **IE Enhanced Security Configuration**
- Set to:
  - Administrators: **Off**
  - Users: **Off**

---

## ✅ Final Checklist

- [x] VM created with TPM, EFI, and VirtIO (if applicable)
- [x] Windows Server installed and updated
- [x] Hostname: `LAB-DC01`
- [x] Static IP: `10.0.0.10`
- [x] Roles added: AD DS + DNS
- [x] Domain: `rougelabs.local` created and promoted
- [x] DNS forwarders configured
- [x] Test users + OUs created
- [x] QEMU Guest Agent installed (optional)

---

# 🔒 Security Notes

- ✅ Save the **DSRM password** securely.
- ✅ Lock down the Proxmox console and VM access.
- ✅ Take a Proxmox snapshot once setup is complete for easy rollback.
