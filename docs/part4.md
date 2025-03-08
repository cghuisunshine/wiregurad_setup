# Part 4: Starting the VPN & Connecting

In this final part, we’ll **combine everything** from the previous steps so you can see your WireGuard VPN **in action**. We’ll explain **what** each step does, **why** it matters, and **how** to verify it’s working. If you’re new to this and find yourself unsure, don’t worry—this guide aims to spell it out in simple terms.

---

## What You’ll Accomplish

1. **Start the VPN** on your server, so it’s ready for incoming connections.  
2. **Generate or confirm** you have client configuration files (e.g., `.conf`) or QR codes for mobile.  
3. **Import** these files into WireGuard on any device (Windows, macOS, iOS, Android, or Linux).  
4. **Verify** that your VPN connection is actually working (ping, logs, and IP checks).  

By the end, you’ll have a **fully operational WireGuard VPN**, allowing you or your users to securely access remote networks.

---

## 1. Confirm Server-Side Configuration

Before starting the VPN, let’s ensure **everything on the server** is in place. In **Part 3**, you generated client keys and added `[Peer]` entries to your server’s config file. Double-check:

1. **Open `/etc/wireguard/wg0.conf`** on your server:
   ```bash
   sudo nano /etc/wireguard/wg0.conf
   ```
2. Inside `[Interface]`:
   - `PrivateKey = <server-private-key>` (should be the actual key, not `<...>`).  
   - `Address = 10.8.0.1/24` (or any other chosen subnet for your VPN).  
   - `ListenPort = 51820` (or the port you chose).
3. Make sure each `[Peer]` block has:
   - `PublicKey = <client-public-key>` (copied from the client’s `.pub` file).  
   - `AllowedIPs = 10.8.0.X/32` (the IP you assigned to that client).

**Why This Matters**: The server must know which clients can connect (their public key) and which internal IP (`10.8.0.X`) to assign each one. If you skip this, your clients won’t be recognized.

---

## 2. Bring Up the VPN Interface on the Server

```bash
sudo wg-quick up wg0
```
**Why**: This command loads your `wg0.conf` file and creates the `wg0` interface. It’s like “switching on” the VPN server.

- **If you see an error** (e.g., “RTNETLINK answers: File exists”), check your config or ensure `wg0` isn’t already active by running `sudo wg-quick down wg0` first.
- **If everything’s good**, you won’t see much output, but you can verify:

  ```bash
  sudo wg
  ```
  This shows info about the `wg0` interface, including the server’s **public key**, **listening port**, and any **peers**.

---

## 3. Enable WireGuard at Boot (Optional)

```bash
sudo systemctl enable wg-quick@wg0
```
**Why**: You probably want your VPN to **auto-start** after each server reboot. This systemd command ensures WireGuard is brought up every time your server restarts.

---

## 4. Generate or Confirm Your Client Configuration Files

If you haven’t already (or need a refresher):

- **Client `.conf` File** Example:
  ```ini
  [Interface]
  Address = 10.8.0.2/24
  PrivateKey = <client-private-key>
  DNS = 1.1.1.1

  [Peer]
  PublicKey = <server-public-key>
  Endpoint = <server-ip-or-domain>:51820
  AllowedIPs = 0.0.0.0/0
  PersistentKeepalive = 25
  ```
  
- **Why**: This `.conf` file tells the client:
  1. **Its IP** inside the VPN (`10.8.0.2`).
  2. **Its private key** (used to authenticate & encrypt traffic).
  3. **The server’s address & public key**.

If you need a QR code for **mobile devices**, install `qrencode` on the server and run:
```bash
qrencode -t ansiutf8 < client1.conf
```
Then scan with the WireGuard app on iOS/Android.

**Why**: QR codes make it easier to add a tunnel on phones without fiddling with `.conf` files.

---

## 5. Transfer the Client Configuration File (If Needed)

If you created the client `.conf` file on the server, you’ll need to **download** it to your local device (Windows, macOS, or another Linux machine):

- **SCP/SFTP** (Linux/Mac):
  ```bash
  scp user@yourserver.com:/etc/wireguard/keys/clients/client1.conf .
  ```
- **File Transfer App** (Windows):
  - Use WinSCP or FileZilla to **connect** to your server and **download** `client1.conf`.

**Why**: The `.conf` file must physically reside on the client device to be imported into the WireGuard app.

---

## 6. Import on Windows/macOS

1. **Open the WireGuard app**:
   - **Windows**: search for “WireGuard” in the Start menu.
   - **macOS**: open “WireGuard” from Applications.
2. **Click “Import Tunnel(s)”** (or the `+` icon).
3. **Select your `.conf` file**. The app should show a new tunnel entry.
4. **Activate** the tunnel by flipping the switch to “On” or “Connect.”

**Why**: Without importing the `.conf`, the WireGuard app has no idea how to connect to your server.

---

## 7. Import/Scan on iOS/Android

1. **Install WireGuard** from the App Store or Google Play.
2. **Open the app** and tap **“+”** → **“Create from file or archive”** (or “Scan from QR Code”).
3. If using a `.conf` file, transfer it to your phone (via AirDrop, email, or cloud).  
   - Tap “Import” and select the file.
   - Or, scan the QR code if you generated one.
4. **Activate** the tunnel by toggling it on.

**Why**: Mobile devices need the same tunnel details: server IP, server public key, client private key, etc.

---

## 8. Verify the Connection

Once your client tunnel is **active**:

1. **Ping the Server’s VPN IP**:
   ```bash
   ping 10.8.0.1
   ```
   - If it replies, you have **basic connectivity**.
2. **Check “latest handshake”** on the server:
   ```bash
   sudo wg
   ```
   - You should see a “latest handshake” timestamp under the client’s `peer:` section, indicating an active session.
3. **Confirm Your Public IP** (if routing all traffic through VPN):
   - Visit a “What’s my IP” website. You should see your **server’s** public IP address instead of your local ISP’s.

**Why**: These steps ensure data is flowing through the VPN, not just showing “connected” in the app without actual traffic.

---

## 9. Troubleshoot (If Needed)

- **Server Logs**:
  ```bash
  sudo journalctl -u wg-quick@wg0
  ```
  Look for errors like “Key is not the correct length” or “Handshake did not complete.”
- **Client Logs**:  
  - On Windows/macOS: In the WireGuard app, click the **Log** tab.  
  - On iOS/Android: Tap the tunnel, then look for “Log” or “Info.”
- **Double-Check Firewall**:  
  - Did you open UDP port `51820` on your server?  
  - If behind a home router, did you forward that port?

---

## 10. Know It’s Working

When successful, you’ll see:
1. A stable “Connected” status in the WireGuard client app.
2. **Ping** responses from `10.8.0.1`.
3. A recent “latest handshake” time in `sudo wg` on the server.
4. Potentially a new public IP (that of your server) if you route all traffic via the VPN.

**Why**: This is the moment you confirm your secure tunnel is established and data is encrypted in transit.

---


## Conclusion

By following these steps:

1. **Your server** is running the `wg0` interface and listening on port `51820`.  
2. **Clients** have valid `.conf` files (or QR codes) and can connect to your server’s VPN IP (`10.8.0.1`).  
3. **Connectivity tests** (ping, logs, IP checks) confirm the tunnel is fully operational.

You now have a functional WireGuard VPN! If you run into issues, review logs, verify your server’s firewall, and ensure each client’s config matches exactly. Happy secure networking!
