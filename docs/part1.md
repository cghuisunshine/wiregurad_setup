## Part 1: Installing WireGuard

### **Overview**  
You’ll install WireGuard on both the Linux server (where the VPN will be hosted) and any client devices (Windows, macOS, etc.). Follow these steps to ensure everything is set up correctly before moving on to configuration.

---

### **Steps**  

1. **Check Your Kernel Version** (Linux Server)  
   - WireGuard requires Linux kernel 5.6 or above, or backported modules on older distributions.  
   ```bash
   uname -r
   ```  
   - If your distribution is older (e.g., Ubuntu 18.04), look up “WireGuard backports” to install the module.

2. **Update & Upgrade System Packages** (Linux Server)  
   - Ensure your package index and installed packages are up-to-date:  
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```  

3. **Install WireGuard** (Linux Server)  
   - On Debian/Ubuntu:  
     ```bash
     sudo apt install wireguard -y
     ```  
   - On CentOS/RHEL:  
     ```bash
     sudo yum install epel-release -y
     sudo yum install kmod-wireguard wireguard-tools -y
     ```  

4. **Check for Required Tools** (Linux Server)  
   - Make sure the essential WireGuard tools (e.g., `wg` and `wg-quick`) are available:  
   ```bash
   which wg && which wg-quick
   ```  

5. **Verify WireGuard Installation** (Linux Server)  
   - Confirm the version:  
   ```bash
   wg --version
   ```  
   - You should see a version number and build date.

6. **Install WireGuard GUI or CLI** (Windows)  
   - Download the installer from [WireGuard.com](https://www.wireguard.com/install/) and run the `.exe` file.  
   - Follow on-screen prompts and click **Install**.

7. **Install WireGuard App** (macOS)  
   - Get the official WireGuard app from the [Mac App Store](https://apps.apple.com/us/app/wireguard/id1451685025).  
   - Drag the app to **Applications** if needed, then open it.

8. **Install WireGuard Mobile Apps** (iOS/Android)  
   - On iOS, search **WireGuard** in the App Store.  
   - On Android, install **WireGuard** from the Google Play Store.  

9. **Launch the WireGuard Client** (All Platforms)  
   - Open the application (Linux: `wg-quick`, Windows: WireGuard GUI, macOS/iOS/Android: WireGuard app).  
   - You’ll configure it later in [Part 3: Configuring Clients](./docs/part3.md).

10. **(Optional) Enable Automatic Updates** (Linux Server)  
   - Since security is crucial, consider enabling unattended upgrades or regularly run update checks:
   ```bash
   sudo apt install unattended-upgrades
   sudo dpkg-reconfigure unattended-upgrades
   ```

![Installation terminal screenshot](assets/install.png "WireGuard installation steps")  
*Alt-text: Terminal showing WireGuard installation commands.*

---

### **Conclusion**  
WireGuard is now installed on your server and any client devices. In the next section, you’ll configure your Linux server with keys, network settings, and firewall rules to get your VPN fully operational.  