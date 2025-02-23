## Part 4: Starting the VPN & Connecting

### **Overview**  
In this section, you’ll learn how to bring up the VPN interface on the server, enable any required network settings (like IP forwarding), and connect from your client devices. Once complete, your WireGuard VPN will be fully operational and ready for secure remote access.

---

### **Steps**

1. **Double-Check the Server Configuration**  
   - Ensure your `/etc/wireguard/wg0.conf` has the correct **PrivateKey**, **Address**, **ListenPort**, and [Peer] blocks for each client.  
   - Verify your firewall settings allow UDP traffic on port `51820`.

2. **Start WireGuard on the Server**  
   - Use the wg-quick script to bring up your WireGuard interface:
     ```bash
     sudo wg-quick up wg0
     ```  
   - This reads from `/etc/wireguard/wg0.conf` and starts the VPN interface.

3. **Enable WireGuard to Launch at Boot**  
   - To ensure the VPN interface automatically starts after a reboot:  
     ```bash
     sudo systemctl enable wg-quick@wg0
     ```

4. **Enable IP Forwarding on the Server**  
   - If you haven’t already done so in previous steps:
     ```bash
     echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
     sudo sysctl -p
     ```  
   - This allows your server to route traffic between the VPN interface and the internet/local network.

5. **Verify the Server Interface and Peers**  
   - Run `sudo wg` to see the status of your WireGuard interface.  
   - Confirm the interface `wg0` is up and that your clients’ public keys are listed under **Peers**.

6. **Connect Desktop Clients**  
   - **Windows** or **macOS**:  
     1. Open the WireGuard application.  
     2. Click **Import tunnel** and select your `client.conf`.  
     3. Flip the switch to **Activate** or **Connect**.  
   - **Linux** (using wg-quick):  
     ```bash
     sudo wg-quick up /path/to/client.conf
     ```

7. **Connect Mobile Clients**  
   - **iOS/Android**:  
     1. Install the WireGuard app from the App Store (iOS) or Play Store (Android).  
     2. Tap **+** (add) → **Scan from QR Code**.  
     3. Scan the QR code generated from your `client.conf`.  
     4. Tap **Activate** or **Connect**.

8. **Test Connectivity**  
   - From a connected client, ping the server’s VPN IP (e.g., `ping 10.8.0.1`).  
   - If you see replies, your VPN tunnel is working.  
   - Optionally, try accessing resources on your local network or the internet to confirm routing is correct.

9. **Check Logs for Troubleshooting** (if needed)  
   - **Server**:  
     ```bash
     journalctl -u wg-quick@wg0
     ```
     or
     ```bash
     sudo wg
     ```
   - **Clients**:  
     - In the WireGuard desktop/mobile app, check the log or status panel for any error messages (e.g., “Handshake did not complete”).

10. **(Optional) Customize Traffic Rules and NAT**  
   - If your VPN requires full internet routing (`AllowedIPs = 0.0.0.0/0`), ensure NAT is configured properly on the server (e.g., using iptables or firewalld).  
   - For example, on Ubuntu with iptables:  
     ```bash
     sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
     ```
     Replace `eth0` with the interface connected to the internet.

![WireGuard GUI screenshot](assets/gui.png "WireGuard client interface")  
*Alt-text: WireGuard desktop app with an active tunnel.*

---

### **Conclusion**  
Your WireGuard VPN is now active and clients can securely connect. Test further by checking local network resources and external internet access to confirm everything is routing correctly. If you run into issues, consult the logs or revisit your server and client configurations for typos or missing settings.  