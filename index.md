# WireGuard Setup Guide for Beginners

## Introduction
**WireGuard** is a lightweight, high-performance VPN that uses modern cryptography to secure internet traffic. This guide walks beginners through setting up a WireGuard VPN server and connecting clients (e.g., laptops, phones).

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

