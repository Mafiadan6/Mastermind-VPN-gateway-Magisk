Universal VPN Gateway

https://img.shields.io/badge/Magisk-Module-00AF9C?logo=magisk&style=flat-square
https://img.shields.io/badge/Requires-Root-3DDC84?logo=android&style=flat-square
https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square

A powerful, extended Magisk module that routes all tethered traffic—including Wi-Fi hotspot, Bluetooth PAN, and USB Ethernet—through your device's active VPN connection. Based on the original vpn-gateway by mastermind.

🚀 Features

· 🛡️ Full VPN Tunneling: Routes all traffic from connected devices through your phone's VPN.
· 📶 Multi-Interface Support:
  · Wi-Fi Hotspot (wlan0)
  · Bluetooth Tethering (bnep0)
  · USB Ethernet/Tethering (eth0, usb0)
  · Universal Fallback for all RFC 1918 private IP ranges
· 🔒 DNS Leak Protection: Forces tethered device DNS queries through a public resolver (Cloudflare).
· ⚡ Seamless Integration: Works with the system's native tethering features—no extra apps needed.
· 🔄 Automatic Management: Rules are applied and cleaned up automatically when your VPN connects/disconnects.

📋 Prerequisites

· A rooted Android device (Magisk v20.4+ recommended)
· An active VPN connection (supports most VPN apps using a tun interface)

🛠️ Installation

1. Download the module zip from the Releases page.
2. Open the Magisk Manager app.
3. Go to Modules → Install from storage.
4. Select the downloaded zip file.
5. Reboot your device.

Note: If you have the original vpn-gateway installed, uninstall it first from Magisk Manager before installing this version.

🔧 Manual Configuration (If Needed)

The module works out-of-the-box for most devices. If Bluetooth or USB tethering doesn't route through VPN:

1. Identify your device's interface names (requires root terminal):
   ```bash
   su
   ip addr show
   ```
   Look for interfaces like bnep0, bnep1, eth0, rndis0, etc., when tethering is active.
2. Update the script (advanced users):
   · Extract the module zip.
   · Edit /common/service.sh.
   · In the iptables_rules_for function, update the interface names (e.g., change bnep0 to bnep1).
   · Repack the zip and install.

📖 How to Use

1. Connect to your VPN (WireGuard, OpenVPN, etc.) on your phone.
2. Enable your desired tethering method:
   · Wi-Fi Hotspot (Settings → Network & Internet → Hotspot & tethering)
   · Bluetooth Tethering (Pair device first, then enable)
   · USB Tethering (Connect via USB cable, then enable)
3. Connect your client device (laptop, tablet, another phone).
4. Verify the connection by visiting ipleak.net on the client device. You should see your VPN's IP address and DNS servers.

🧪 Testing with Two Devices

This module was perfected using a two-device setup:

Device Role Purpose
Essential PH-1 VPN Gateway / Server Handles VPN connection and routing. Excellent modem for strong signal.
Samsung Galaxy A11 Test Client Connects via tethering. Long battery life for extended testing.

Quick Test Procedure:

1. On PH-1: Connect VPN → Enable Bluetooth Tethering.
2. On A11: Pair & connect via Bluetooth.
3. On A11: Browse to ipleak.net. Confirm VPN IP appears.

⚙️ Technical Details

The module works by:

· Creating custom iptables rules to forward traffic from tethering interfaces to the VPN tun interface.
· Setting up policy-based routing (ip rule) to direct private IP traffic through a dedicated routing table for the VPN.
· Redirecting DNS (UDP port 53) to 1.1.1.1 to prevent DNS leaks outside the VPN tunnel.

❓ Troubleshooting

Issue Likely Cause Solution
No internet on tethered device VPN not connected Connect to your VPN first, then enable tethering.
Internet works but shows real IP (DNS leak) DNS rule not applied Ensure VPN is connected before tethering. Check logs for errors.
Bluetooth/USB tether not working Interface name mismatch Check interface names with ip addr show and update script if needed.
Module fails to install Conflicting module ID Uninstall any original vpn-gateway module first.

View module logs:

```bash
su
logcat | grep -i "vpn.gateway\|VPNGateway"
```

🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository.
2. Create a feature branch (git checkout -b feature/AmazingFeature).
3. Commit your changes (git commit -m 'Add some AmazingFeature').
4. Push to the branch (git push origin feature/AmazingFeature).
5. Open a Pull Request.

📄 License

Distributed under the MIT License. See LICENSE for more information.

🙏 Credits

· mastermind for the original vpn-gateway module.
· The Android and Magisk open-source communities.
· All contributors and testers.

⚠️ Disclaimer

This module modifies core network routing. Use at your own risk. The developers are not responsible for any security issues, connectivity problems, or device instability that may arise. Always ensure you comply with your local laws and your VPN provider's terms of service regarding tethering.

---

⭐ If this project helped you, please consider giving it a star on GitHub!

Last Updated: 2023-10-27
Version: 5.1-mod
Compatibility: Android 9+ (rooted with Magisk)
