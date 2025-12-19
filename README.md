🔁 Universal VPN Gateway

https://img.shields.io/badge/Platform-Magisk_Module-00AF9C?logo=magisk&style=for-the-badge
https://img.shields.io/badge/Requires-Android_Root-3DDC84?logo=android&style=for-the-badge
https://img.shields.io/badge/Supports-OpenVPN%2FWireGuard%2Fetc.-8A2BE2?style=for-the-badge
https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge

Route ALL tethered traffic through a VPN. A production-ready, battle-tested Magisk module that extends your phone's VPN protection to every device connected via Wi-Fi hotspot, Bluetooth tethering, or USB Ethernet with optimized streaming and call quality.

🎯 Production Edition | Based on the original vpn-gateway by mastermind, extended with USB/Bluetooth support, streaming optimizations, and router health check fixes.

---

📋 Table of Contents

· ✨ Features
· 🚀 Quick Start
· 🔧 How It Works
· 📦 Installation
· ⚙️ Configuration
· 🎯 Verified Use Cases
· 🔍 Troubleshooting
· 🤝 Contributing
· 📜 Changelog
· ⚠️ Disclaimer

---

✨ Features

Core Functionality

Feature Status Description
Wi-Fi Hotspot VPN ✅ Fully Working Route Wi-Fi (wlan0) tethered traffic through VPN
Bluetooth Tethering ✅ Fully Working Route Bluetooth (bnep0) PAN traffic through VPN
USB Ethernet/Tethering ✅ Fully Working Route USB (usb0/eth0) traffic through VPN
Multi-Interface Support ✅ Simultaneous All tethering methods work at the same time

Optimizations & Fixes

Optimization Benefit Verified Working
MTU Optimization (1400) Prevents packet fragmentation for streaming ✅ IPTV Streaming
TCP Buffer Tuning Smooth video calls & reduced buffering ✅ Video/Voice Calls
Router Health Check Bypass Prevents false "offline" detection ✅ OpenWrt/mwan3
DNS Leak Protection Forces Cloudflare DNS (1.1.1.1) ✅ ipleak.net tested
IPv6 Leak Blocking Blocks IPv6 traffic outside VPN ✅ Enabled by default

---

🚀 Quick Start

For End Users

1. Root your Android device (Magisk 20.4+ recommended)
2. Download the latest module zip from Releases
3. Install via Magisk Manager → Modules → Install from Storage
4. Reboot and connect to your VPN
5. Enable tethering (Wi-Fi/Bluetooth/USB) - all traffic now routes through VPN!

For Developers

```bash
# Clone and explore
git clone https://github.com/mafiadan6/Mastermind-VPN-gateway-Magisk.git
cd Mastermind-VPN-gateway-Magisk

# Module structure
.
├── META-INF/          # Magisk installer scripts
├── common/            # Core module files
│   ├── service.sh     # Main routing script
│   └── module.prop    # Module metadata
└── README.md          # This file
```

---

🔧 How It Works

Network Flow

```
Client Devices → Phone Tethering → Universal VPN Gateway → VPN Tunnel (tun0) → Internet
      ↑                                      ↑
   VPN IP only                       Real IP blocked
   
Tethering Methods:
- Wi-Fi (wlan0)
- Bluetooth (bnep0) 
- USB (usb0/eth0)
```

Technical Implementation

The module works by:

1. Creating custom iptables rules to forward traffic from tethering interfaces to the VPN tunnel
2. Setting up policy-based routing (ip rule) to direct private IP traffic through VPN
3. Applying MTU optimizations (1400) to prevent packet fragmentation
4. Configuring TCP buffers for optimal streaming performance
5. Exempting ICMP traffic for router health checks

---

📦 Installation

Prerequisites

· ✅ Rooted Android device (Magisk 20.4+)
· ✅ Active VPN connection (OpenVPN, WireGuard, etc.)
· ✅ Tethering plan (check with your mobile carrier)

Installation Steps

1. Download the module zip from the Releases page
2. Open Magisk Manager → Modules → Install from storage
3. Select the downloaded zip file
4. Reboot your device
5. Verify installation:
   ```bash
   # Check if module is active
   su -c "logcat | grep -i 'vpn.gateway'"
   ```

⚠️ Important: If you have the original vpn-gateway installed, uninstall it first before installing this version.

---

⚙️ Configuration

Customizing Interface Names

Some devices use different interface names. To check yours:

```bash
su
# Check active interfaces
ip addr show

# Common names:
# Bluetooth: bnep0, bnep1, pan0
# USB: eth0, rndis0, usb0
# VPN: tun0, wg0, utun0
```

To modify, edit /data/adb/modules/vpn-gateway/service.sh and update the interface rules in the iptables_rules_for function.

Changing DNS Server

To use a different DNS (not Cloudflare's 1.1.1.1):

```bash
# Edit the PREROUTING rules in service.sh
PREROUTING -t nat ! -i $tun_name -s 192.168.0.0/16 -p udp --dport 53 -j DNAT --to 8.8.8.8  # Google DNS
```

Adjusting MTU for Your Network

If streaming still buffers:

```bash
# Test different MTU values
su
ip link set dev tun0 mtu 1350  # Try lower values
# Test streaming for 2 minutes
ip link set dev tun0 mtu 1300  # Try even lower
```

---

🎯 Verified Use Cases

✅ Confirmed Working

Use Case Tested With Status
IPTV Streaming via USB Various IPTV services ✅ Perfect
Video Conferencing Zoom, WhatsApp, Teams ✅ Improved
Online Gaming Mobile games via tethering ✅ Stable
Router Integration OpenWrt with mwan3 ✅ Compatible
Multiple Clients 3+ devices simultaneously ✅ Working

🔧 Tested Device Combinations

Phone (Gateway) Client Device Tethering Method Result
Essential PH-1 Samsung A11 USB ✅ Best performance
Essential PH-1 Laptop Wi-Fi Hotspot ✅ Excellent
Samsung A11 Tablet Bluetooth ✅ Good for battery

Performance Characteristics

Metric Wi-Fi Hotspot USB Tethering Bluetooth
Max Speed 300-500 Mbps 400-700 Mbps 25-40 Mbps
Latency 20-40ms 15-30ms 50-100ms
Best For Multiple devices Speed/Stability Battery life

---

🔍 Troubleshooting

Common Issues & Solutions

Problem Symptoms Solution
No internet on tethered device Can connect but no data Connect VPN first, then enable tethering
Shows real IP, not VPN IP DNS leak at ipleak.net Ensure VPN is connected before tethering
USB tethering not working Device doesn't recognize Check interface name: ip addr show | grep usb
Streaming buffers on USB IPTV/video stutters Try lower MTU: ip link set dev tun0 mtu 1350
Router shows red lights mwan3 marks interface offline Set router tracking IP to 127.0.0.1

Diagnostic Commands

```bash
# Check if rules are applied
su -c "iptables -L FORWARD -v -n | grep -E '(wlan|bnep|eth|usb)'"

# Monitor module logs
su -c "logcat --pid=$(pidof magiskd) | grep -i gateway"

# Test VPN routing
su -c "curl --interface tun0 ifconfig.me"

# Check interface status
su -c "ip link show | grep -E '(tun0|usb0|wlan0)'"
```

Quick Fix Sequence

1. Reboot phone
2. Connect VPN first
3. Enable tethering second
4. Test at ipleak.net
5. If issues persist, check router health check settings

---

📜 Changelog

v7.0.0 - Current Stable

· Added: TCP buffer optimizations for streaming & calls
· Verified: IPTV streaming over USB tethering
· Improved: Video/voice call quality
· Maintained: All previous functionality

v6.0.0 - MTU Optimization

· Added: MTU fix (1400) for USB streaming
· Fixed: Packet fragmentation causing buffering
· Added: Universal ICMP exemptions for router health checks

v5.0.0 - Universal Gateway

· Added: Bluetooth tethering support (bnep0)
· Added: USB Ethernet support (eth0, usb0)
· Extended: Original Wi-Fi hotspot functionality

v1.0.0 - Base Version

· Based on: Original vpn-gateway by mastermind
· Core: Wi-Fi hotspot VPN routing

---

🤝 Contributing

Contributions are welcome! Here's how you can help:

1. Report Issues: Found a bug? Open an issue
2. Suggest Features: Have an idea? Start a discussion
3. Submit Code: Fork the repo and create a pull request
4. Test & Verify: Try it on different devices/configurations

Development Setup

```bash
# Clone repository
git clone https://github.com/mafiadan6/Mastermind-VPN-gateway-Magisk.git

# Create test module zip
cd Mastermind-VPN-gateway-Magisk
zip -r universal-vpn-gateway.zip META-INF/ common/

# Install on device
adb push universal-vpn-gateway.zip /sdcard/
# Then install via Magisk Manager
```

---

⚠️ Disclaimer

Use at your own risk. This module:

· Modifies core network routing on your device
· Requires root access (bypasses Android security)
· May not work with all carriers or VPN providers
· Could potentially violate your VPN provider's terms of service

Always:

· Comply with local laws and regulations
· Respect your mobile carrier's tethering policies
· Use VPN services responsibly
· Test thoroughly before relying on critical applications

THE SOFTWARE IS PROVIDED "AS IS", without warranty of any kind. The developers are not responsible for any security issues, connectivity problems, or device instability that may arise.

---

📞 Support & Community

· Report Issues: GitHub Issues
· Discussion: XDA Developers Thread (coming soon)
· Credits: Original module by Kr328, extended by mafiadan6

Like this project? Give it a ⭐ on GitHub!

---

Last Updated: 2023-12-17
Version: 7.0.0-streaming-optimized
Compatibility: Android 9+ (rooted with Magisk)

Happy secure tethering! 🔐📡
