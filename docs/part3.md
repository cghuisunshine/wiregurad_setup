## Part 3: Configuring Clients

### **Overview**  
In this section, you’ll generate client keys, create the client configuration file(s), and learn how to import them on various devices (Linux, Windows, macOS, iOS, Android). By the end, your client devices will be ready to securely connect to your WireGuard server.

---

### **Steps**

1. **Decide on the Client IP Addressing Scheme**  
   - Each client (peer) needs a unique IP address in the VPN subnet (e.g., `10.8.0.x/24`).  
   - Plan out which IP addresses to assign if you have multiple clients (e.g., `10.8.0.2`, `10.8.0.3`, etc.).

2. **Create a Separate Directory for Client Keys** (Optional)  
   - On the same server where you generated the server keys, keep client keys organized:  
     ```bash
     sudo mkdir -p /etc/wireguard/keys/clients
     sudo chmod 700 /etc/wireguard/keys/clients
     cd /etc/wireguard/keys/clients
     ```

3. **Generate a New Client Private/Public Key Pair**  
   - Use `wg genkey` just like you did for the server:  
     ```bash
     umask 077
     wg genkey | tee client-private.key | wg pubkey > client-public.key
     ```  
   - This example shows how to create just one client; repeat for each additional client, using unique filenames (e.g., `client1-private.key`, `client1-public.key`).

4. **Back Up Your Client Keys** (Optional but Recommended)  
   - Keys can be regenerated, but it’s often convenient to keep them in a secure backup location.  
   - For example, copy them to an encrypted USB drive.

5. **Add the Client to the Server Configuration**  
   - Open your server config (`/etc/wireguard/wg0.conf`) and append a new `[Peer]` section:  
     ```ini
     [Peer]
     PublicKey = <client-public-key>
     AllowedIPs = 10.8.0.2/32
     ```  
   - Replace `10.8.0.2/32` with the IP you plan to assign to this client.  
   - Save and exit, then apply the changes:  
     ```bash
     sudo wg-quick down wg0 && sudo wg-quick up wg0
     ```

6. **Create the Client Configuration File** (`client.conf`)  
   - On the server or on your local machine, create a file named `client.conf` (use a text editor):  
     ```ini
     [Interface]
     Address = 10.8.0.2/24
     PrivateKey = <client-private-key>
     DNS = 1.1.1.1        # (Optional) Provide a DNS server

     [Peer]
     PublicKey = <server-public-key>
     Endpoint = <server-ip-or-domain>:51820
     AllowedIPs = 0.0.0.0/0
     PersistentKeepalive = 25
     ```
   - Replace `<client-private-key>` with the contents of `client-private.key`.  
   - Replace `<server-public-key>` with the contents of your `server-public.key`.  
   - Replace `<server-ip-or-domain>` with your server’s public IP or domain.

7. **(Optional) Generate a QR Code for Mobile Apps**  
   - For easier import on mobile devices, install `qrencode` on your Linux machine:
     ```bash
     sudo apt install qrencode -y
     ```  
   - Convert `client.conf` into a QR code:  
     ```bash
     qrencode -t ansiutf8 < client.conf
     ```  
   - For iOS or Android WireGuard app, tap **Scan from QR Code** and point your camera at the generated code.

8. **Import the Client Configuration on Windows/macOS**  
   - **Windows**:  
     1. Download the `.conf` file onto your Windows system.  
     2. Open the WireGuard app and click **Import tunnel(s)**.  
     3. Select your `client.conf` file.  
   - **macOS**:  
     1. Download the `.conf` file.  
     2. In the WireGuard app, click **Import tunnel(s) from file** and select `client.conf`.

9. **Import the Client Configuration on Linux**  
   - If using `wg-quick`:  
     ```bash
     sudo cp client.conf /etc/wireguard/
     sudo wg-quick up /etc/wireguard/client.conf
     ```  
   - Adjust file paths based on your distro’s setup. You can also rename `client.conf` to something like `wg-client.conf` if desired.

10. **Test the Connection**  
   - From the client device, activate the WireGuard tunnel.  
   - Verify your new VPN IP address:  
     - On Linux, run `ip addr show` and look for `10.8.0.x`.  
     - On Windows/macOS, the WireGuard UI usually shows “Connected” or “Active.”  
   - Check if you can reach the server (e.g., `ping 10.8.0.1`). If you allowed full internet routing (`AllowedIPs = 0.0.0.0/0`), verify with an IP-checking service that your public IP matches the server’s.

> [!CAUTION]  
> Replace `<server-ip-or-domain>` with your server’s actual public IP or a resolvable domain. Using an incorrect address will prevent the client from connecting.

![Client config file screenshot](assets/client-config.png "Client configuration example")  
*Alt-text: Example WireGuard client configuration file.*

---

### **Conclusion**  
Your client devices now have valid private/public keys, a corresponding configuration, and are ready to connect. In **[Part 4: Starting the VPN & Connecting](./docs/part4.md)**, you’ll finalize the setup by bringing up the VPN on both the server and clients and verifying traffic flow.