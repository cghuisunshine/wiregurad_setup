# WireGuard Setup Guide  

![WireGuard Logo](./assets/wiregurad-logo.svg)  

**A beginner-friendly guide to setting up a secure WireGuard VPN.**  


## Introduction
**WireGuard** is a lightweight, high-performance VPN that uses modern cryptography to secure internet traffic. This guide walks beginners through setting up a WireGuard VPN server and connecting clients (e.g., laptops, phones).

---

### Disclaimer
You must have a **Linux server** (e.g., Ubuntu, Debian, Fedora, etc.) with **root or sudo privileges**.  
You also need a separate **client device** (such as Windows, macOS, iOS, Android, or another Linux machine) to connect to the VPN.  
If you do **not** have both a Linux server and a separate client device, this guide will **not** apply to your setup.

---

### What is a VPN?
A **VPN** (Virtual Private Network) encrypts your internet traffic and routes it through a secure tunnel. This hides your IP address and protects your data from potential eavesdroppers on insecure networks (such as public Wi-Fi). It can also let you securely access remote services or devices as if they were on your local network.

**WireGuard** is a modern VPN protocol known for its simplicity, speed, and robust cryptography. Compared to older protocols like OpenVPN or IPSec, WireGuard is lighter, easier to configure, and often faster.

---

### Who Is This Guide For?
- **Skill Level**: This tutorial is aimed at **beginners** or those with **basic** Linux command-line experience.  
- **Requirements**:
  - A Linux server with a public IP address (or proper port forwarding configured).
  - A client device (Windows, macOS, iOS, Android, or Linux) to connect to the VPN.
  - Familiarity with text editors (e.g., `nano`, `vim`) and basic command-line usage.


### Intended Use
- Securely access home/office networks remotely.  
- Encrypt public Wi-Fi traffic.  
- Connect multiple devices privately.

### Tasks You’ll Perform
1. Install WireGuard on a server.  
2. Configure the server and clients.  
3. Start the VPN service.  
4. Connect devices to the VPN.

### Intended Readers
- Beginners with basic command-line experience.  
- Users seeking a privacy-focused VPN alternative.

### Conventions
- `Code blocks`: Commands to run in a terminal.  
- **Bold terms**: Key concepts or buttons.  
- *Italics*: File names or variables.

### Admonitions
> [!NOTE]  
> Helpful tips for better understanding.  

> [!WARNING]  
> Critical security or data-loss risks.  

> [!CAUTION]  
> Potential pitfalls or misconfigurations.

---

## Guide Parts

- [Part 1: Installing WireGuard](./docs/part1.md)  
- [Part 2: Configuring the Server](./docs/part2.md)  
- [Part 3: Configuring Clients](./docs/part3.md)  
- [Part 4: Starting the VPN & Connecting](./docs/part4.md)

---

## Glossary
- **VPN**: Virtual Private Network (encrypts internet traffic).  
- **Peer**: A device connected to the VPN (server or client).  
- **Endpoint**: Public IP and port of the WireGuard server.  
- **AllowedIPs**: IP ranges routed through the VPN.  
- **PersistentKeepalive**: Prevents NAT timeout (default: 25 seconds).

---

## Troubleshooting Guide
| Issue                   | Solution                                              |
|-------------------------|-------------------------------------------------------|
| **Connection fails**    | Open UDP port `51820` on the server firewall.         |
| **No internet access**  | Enable IP forwarding: `sysctl -p`.                    |
| **Key mismatch**        | Regenerate and replace keys.                          |
| **QR code won’t scan**  | Ensure `client.conf` has no typos.                    |
| **Slow speeds**         | Check server bandwidth or ISP throttling.             |

