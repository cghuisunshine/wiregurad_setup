# WireGuard Setup Guide for Beginners

---

## Introduction
**WireGuard** is a lightweight, high-performance VPN that uses modern cryptography to secure internet traffic. This guide walks beginners through setting up a WireGuard VPN server and connecting clients (e.g., laptops, phones).  

### **Intended Use**  
- Securely access home/office networks remotely.  
- Encrypt public Wi-Fi traffic.  
- Connect multiple devices privately.  

### **Tasks You’ll Perform**  
1. Install WireGuard on a server.  
2. Configure the server and clients.  
3. Start the VPN service.  
4. Connect devices to the VPN.  

### **Intended Readers**  
- Beginners with basic command-line experience.  
- Users seeking a privacy-focused VPN alternative.  

### **Conventions**  
- `Code blocks`: Commands to run in a terminal.  
- **Bold terms**: Key concepts or buttons.  
- *Italics*: File names or variables.  

### **Admonitions**  
> [!NOTE]  
> Helpful tips for better understanding.  
>  
> [!WARNING]  
> Critical security or data-loss risks.  
>  
> [!CAUTION]  
> Potential pitfalls or misconfigurations.  

---

## Part 1: Installing WireGuard

### **Overview**  
Install WireGuard on a Linux server and client devices.  

### **Steps**  
1. **Update Packages** (Linux Server):  
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```  

2. **Install WireGuard**:  
   ```bash
   sudo apt install wireguard -y
   ```  

3. **Verify Installation**:  
   ```bash
   wg --version
   ```  

> [!NOTE]  
> For Windows/macOS, download the GUI client from [WireGuard.com](https://www.wireguard.com/install/).  

![Installation terminal screenshot](assets/install.png "WireGuard installation steps")  
*Alt-text: Terminal showing WireGuard installation commands.*  

### **Conclusion**  
WireGuard is now installed. Proceed to configure the server.  

---

## Part 2: Configuring the Server

### **Overview**  
Generate encryption keys and create a server configuration file.  

### **Steps**  
1. **Generate Server Keys**:  
   ```bash
   umask 077  
   wg genkey | tee server-private.key | wg pubkey > server-public.key  
   ```  

2. **Create Config File** (`/etc/wireguard/wg0.conf`):  
   ```ini
   [Interface]  
   Address = 10.8.0.1/24  
   ListenPort = 51820  
   PrivateKey = <server-private-key>  

   [Peer]  
   PublicKey = <client-public-key>  
   AllowedIPs = 10.8.0.2/32  
   ```  

> [!WARNING]  
> Never share `server-private.key`!  

![Server config file screenshot](assets/server-config.png "Server configuration example")  
*Alt-text: Example WireGuard server configuration file.*  

### **Conclusion**  
Your server is ready. Configure clients next.  

---

## Part 3: Configuring Clients

### **Overview**  
Create client configurations for devices like laptops or phones.  

### **Steps**  
1. **Generate Client Keys**:  
   ```bash
   wg genkey | tee client-private.key | wg pubkey > client-public.key  
   ```  

2. **Create Client Config** (`client.conf`):  
   ```ini
   [Interface]  
   Address = 10.8.0.2/24  
   PrivateKey = <client-private-key>  

   [Peer]  
   PublicKey = <server-public-key>  
   Endpoint = <server-ip>:51820  
   AllowedIPs = 0.0.0.0/0  
   ```  

> [!CAUTION]  
> Replace `<server-ip>` with your server’s public IP.  

![Client config file screenshot](assets/client-config.png "Client configuration example")  
*Alt-text: Example WireGuard client configuration file.*  

### **Conclusion**  
Clients can now connect to the server.  

---

## Part 4: Starting the VPN & Connecting

### **Overview**  
Activate the VPN and connect clients.  

### **Steps**  
1. **Start WireGuard on the Server**:  
   ```bash
   sudo wg-quick up wg0  
   sudo systemctl enable wg-quick@wg0  
   ```  

2. **Enable IP Forwarding**:  
   ```bash
   echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf  
   sudo sysctl -p  
   ```  

3. **Connect Clients**:  
   - **Desktop**: Import `client.conf` into the WireGuard app.  
   - **Mobile**: Scan the QR code of `client.conf`.  

![WireGuard GUI screenshot](assets/gui.png "WireGuard client interface")  
*Alt-text: WireGuard desktop app with active connection.*  

### **Conclusion**  
Your VPN is now active! Test connectivity with `ping 10.8.0.1`.  

---

## Glossary  
- **VPN**: Virtual Private Network (encrypts internet traffic).  
- **Peer**: A device connected to the VPN (server or client).  
- **Endpoint**: Public IP and port of the WireGuard server.  
- **AllowedIPs**: IP ranges routed through the VPN.  
- **PersistentKeepalive**: Prevents NAT timeout (default: 25 seconds).  

---

## Troubleshooting Guide  
| Issue                   | Solution                                  |  
|-------------------------|-------------------------------------------|  
| **Connection fails**    | Open UDP port `51820` on the server firewall. |  
| **No internet access**  | Enable IP forwarding: `sysctl -p`.        |  
| **Key mismatch**        | Regenerate and replace keys.              |  
| **QR code won’t scan**  | Ensure `client.conf` has no typos.        |  
| **Slow speeds**         | Check server bandwidth or ISP throttling. |  

---

