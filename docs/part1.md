# Part 1: Getting Started

Before configuring a WireGuard VPN, you’ll need:

1. **A Linux server** (where you’ll host WireGuard).  
2. **At least one client device** (Windows, macOS, iOS, Android, or another Linux machine) to connect to that server.

This part ensures you have **WireGuard** readily available on both the server and the client device(s). If you need detailed installation instructions, refer to the [WireGuard Official Documentation](https://www.wireguard.com/install/) or your OS’s support resources.

---

## 1. Linux Server

- **Confirm You Have WireGuard**  
  - Check if WireGuard is in your distribution’s repositories.  
  - If you’re not sure, consult your distro’s documentation or the [official WireGuard site](https://www.wireguard.com/install/).  
- **Validate Tools**  
  - After installation (or if already installed), ensure you can run `wg` or `wg-quick` in a terminal.

No step-by-step install is provided here, but you must be **able** to install or verify that WireGuard is present on your server.

---

## 2. Windows Client

- **Download the WireGuard Client**  
  - Get the official Windows installer from [WireGuard.com](https://www.wireguard.com/install/).  
- **Open & Verify**  
  - After installing, launch the **WireGuard** program to ensure it starts without errors.  
  - You may see a prompt to import or create a tunnel configuration—wait until later parts of this guide.

If you encounter any issues, check the [WireGuard for Windows documentation](https://www.wireguard.com/install/) or your system’s help resources.

---

## 3. macOS Client

- **Obtain WireGuard from the Mac App Store**  
  - Search “WireGuard” in the App Store (or use [this link](https://apps.apple.com/us/app/wireguard/id1451685025)) and click **Get**.  
- **Launch & Confirm**  
  - After installation, open the **WireGuard** app.  
  - It should show an initial empty list of tunnels.

Again, detailed steps (like Apple ID logins) are outside this guide’s scope. Refer to [Apple Support](https://support.apple.com) if needed.

---

## 4. iOS & Android Clients

- **Install WireGuard Mobile**  
  - **iOS**: Find “WireGuard” in the iOS App Store.  
  - **Android**: Find “WireGuard” in Google Play.  
- **Launch the App**  
  - Ensure the app runs and can create or import a tunnel later.

For troubleshooting or permissions issues, see [WireGuard on iOS/Android](https://www.wireguard.com/install/) or your phone’s support documentation.

---

## 5. Next Steps

With WireGuard **available** on:
- **Linux server** (ready to act as the VPN host), and  
- **Client devices** (ready to connect),

you can now proceed to [_Configuring the Server_](part2.md), where you’ll set up the server’s WireGuard configuration (keys, interface settings, and firewall rules). Later, in [_Configuring Clients_](part3.md), you’ll create and import client configurations to connect securely to the server.

> **Note**: If you still need to **install** WireGuard on any platform, please consult the official installation instructions or your OS’s package manager documentation before proceeding.
