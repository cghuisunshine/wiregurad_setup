## Part 2: Configuring the Server

### **Overview**  
In this section, you’ll generate the server’s WireGuard keys, create the main configuration file, enable IP forwarding, and set up your firewall. By the end, your server will be ready to accept client connections.

---

### **Steps**  

1. **Enable IP Forwarding**  
   - Edit the sysctl configuration to allow packet forwarding between interfaces:  
     ```bash
     sudo nano /etc/sysctl.conf
     ```  
   - Uncomment or add the following line:  
     ```
     net.ipv4.ip_forward=1
     ```  
   - Apply the changes immediately:  
     ```bash
     sudo sysctl -p
     ```  

2. **Create a Directory for WireGuard Keys**  
   - This helps keep things organized and secure:  
     ```bash
     sudo mkdir -p /etc/wireguard/keys
     ```  
   - Ensure only the root user can access it:  
     ```bash
     sudo chmod 700 /etc/wireguard/keys
     ```

3. **Generate Server Private and Public Keys**  
   - Use a restricted umask so your keys are not world-readable:  
     ```bash
     cd /etc/wireguard/keys
     umask 077
     wg genkey | tee server-private.key | wg pubkey > server-public.key
     ```  
   - **Never** share your private key with anyone.

4. **Check Key Permissions**  
   - Make sure the private key is readable only by root:  
     ```bash
     ls -l /etc/wireguard/keys
     ```
   - If needed, adjust with `chmod 600 server-private.key`.

5. **Create the Server Configuration File** (`wg0.conf`)  
   - Open the file in your preferred editor:  
     ```bash
     sudo nano /etc/wireguard/wg0.conf
     ```  
   - Add the following (replace `<server-private-key>` with the contents of `server-private.key`):
     ```ini
     [Interface]
     Address = 10.8.0.1/24
     ListenPort = 51820
     PrivateKey = <server-private-key>

     # Example peer block:
     [Peer]
     PublicKey = <client-public-key>
     AllowedIPs = 10.8.0.2/32
     ```
   - You will add more `[Peer]` sections later for additional clients.

6. **Set DNS (Optional)**  
   - If you want to push a DNS server to your clients, add a line under `[Interface]` (assuming you use 1.1.1.1 or your own DNS):  
     ```
     DNS = 1.1.1.1
     ```
   - Some client apps (like Windows or macOS) can auto-import this setting.

7. **Open the Firewall Port (51820)**  
   - On Ubuntu with UFW:  
     ```bash
     sudo ufw allow 51820/udp
     ```  
   - On CentOS/RHEL with firewalld:  
     ```bash
     sudo firewall-cmd --add-port=51820/udp --permanent
     sudo firewall-cmd --reload
     ```
   - Make sure your hosting provider or router is forwarding UDP port 51820 to your server.

8. **Bring Up the Interface**  
   - Load the WireGuard configuration:  
     ```bash
     sudo wg-quick up wg0
     ```  
   - Check status:  
     ```bash
     sudo wg
     ```
   - You should see `wg0` interface details and any configured peers.

9. **Enable WireGuard on Boot**  
   - On systems using `systemd`, enable the service to start automatically:  
     ```bash
     sudo systemctl enable wg-quick@wg0
     ```  
   - This ensures your WireGuard interface comes up after a reboot.

10. **Verify IP Forwarding and Connectivity**  
   - Run:  
     ```bash
     sysctl net.ipv4.ip_forward
     ```  
     You should see `net.ipv4.ip_forward = 1`.  
   - Use tools like `ping` or `traceroute` to test connectivity from the server to the VPN interface (`10.8.0.1`) or other LAN devices if applicable.

> [!WARNING]  
> Never share your `server-private.key`! Store it securely and limit file permissions to root-only.

![Server config file screenshot](assets/server-config.png "Server configuration example")  
*Alt-text: Example WireGuard server configuration file.*

---

### **Conclusion**  
Your WireGuard server is now properly configured. Next, move on to **[Part 3: Configuring Clients](./part3.md)** to generate client keys, create client configuration files, and connect to the server.