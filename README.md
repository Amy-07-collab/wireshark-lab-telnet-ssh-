# Lab - Examining Telnet and SSH in Wireshark

## 📌 Overview
This repository documents the hands-on lab exercise **Examining Telnet and SSH in Wireshark** from the Cisco CyberOps Associate curriculum.

The objective of this lab is to configure and capture network traffic for both **Telnet** and **SSH** sessions on a Linux workstation (CyberOps Workstation VM), and analyze the captured packets using **Wireshark** to understand the critical security differences between unencrypted (plaintext) and encrypted network communications.

---

## 🛠️ Lab Environment & Resources
- **OS**: CyberOps Workstation Virtual Machine (Arch Linux base)
- **User Credentials**: `analyst` / `cyberops`
- **Network Interface**: Loopback (`lo`)
- **Tools Used**: Wireshark, Telnet client, SSH client, Bash Terminal

---

## 🚀 Lab Execution & Steps

### Part 1: Examining a Telnet Session in Wireshark

1. **Start Wireshark Capture**:
   - Opened terminal in CyberOps VM and launched Wireshark in background:
     ```bash
     wireshark &
     ```
   - Selected the **Loopback: lo** interface and started packet capture.

2. **Initiate Telnet Connection**:
   - Opened a separate terminal window and connected via Telnet to `localhost`:
     ```bash
     telnet localhost
     ```
   - Entered username `analyst` and password `cyberops` when prompted.

3. **Capture & Analyze Packets**:
   - Stopped Wireshark capture.
   - Applied display filter: `telnet`.
   - Selected a packet, right-clicked, and navigated to **Follow > TCP Stream**.

#### 🔍 Observations (Telnet):
- **Plaintext Exposure**: All session data—including login credentials (`analyst` / `cyberops`) and terminal commands—was transmitted completely in unencrypted plaintext.
- **Echoing Effect**: In the TCP Stream dialog, the login username appeared with duplicated characters (`sannaallyysstt`) due to Telnet's remote echo mechanism displaying both local keystrokes and server echo responses.

---

### Part 2: Examining an SSH Session in Wireshark

1. **Start Wireshark Capture**:
   - Initiated a new capture on the **Loopback: lo** interface.

2. **Initiate SSH Connection**:
   - Established an SSH session to `localhost`:
     ```bash
     ssh localhost
     ```
   - Accepted the ECDSA key fingerprint prompt (`yes`) and authenticated with password `cyberops`.

3. **Capture & Analyze Packets**:
   - Stopped Wireshark capture.
   - Applied display filter: `ssh`.
   - Right-clicked an `SSHv2` packet and navigated to **Follow > TCP Stream**.

#### 🔍 Observations (SSH):
- **Robust Encryption**: Unlike Telnet, the payload was entirely encrypted and unreadable garbled binary text.
- **Key Exchange**: Observed the initial handshake specifying key exchange algorithms (`curve25519-sha256`, `diffie-hellman-group-exchange-sha256`) and ciphers (`aes128-ctr`, `aes256-gcm@openssh.com`).

---

## 🖼️ Screenshots

Place your captured lab screenshots inside the `screenshots/` directory:

| Filename | Description |
|---|---|
| `01_telnet_capture.png` | Filtered Telnet packet list in Wireshark |
| `02_telnet_tcp_stream.png` | Unencrypted Telnet TCP Stream displaying plaintext login & password |
| `03_ssh_capture.png` | Filtered SSH packet list in Wireshark |
| `04_ssh_tcp_stream.png` | Encrypted SSH TCP Stream payload |

---

## ❓ Reflection Question

**Q: Why is SSH preferred over Telnet for remote connections?**  
**A:** Telnet transmits all communication—including usernames, passwords, and executed commands—in unencrypted plaintext. Anyone capable of sniffing network traffic with tools like Wireshark can easily intercept sensitive credentials and data. In contrast, SSH encrypts the entire session payload using strong cryptographic algorithms (such as AES, ECDSA, and Diffie-Hellman), guaranteeing confidentiality, data integrity, and mutual authentication.

---

## 📥 Git Bash Commands (Pushing to GitHub)

Follow these steps in **Git Bash** to push this folder to your GitHub account (`Amy-07-collab`):

```bash
# 1. Open Git Bash and navigate to your project folder
cd /path/to/wireshark-lab-telnet-ssh

# 2. Initialize git repository
git init

# 3. Add all files to staging area
git add .

# 4. Commit files
git commit -m "Add Wireshark Telnet vs SSH lab documentation and screenshots"

# 5. Rename branch to main
git branch -M main

# 6. Add remote GitHub repository
git remote add origin https://github.com/Amy-07-collab/wireshark-lab-telnet-ssh.git

# 7. Push local repository to GitHub
git push -u origin main
```
