# 🔁 Universal VPN Gateway

[![Platform: Magisk Module](https://img.shields.io/badge/Platform-Magisk_Module-00AF9C?logo=magisk&style=for-the-badge)](https://magiskmanager.com/)
[![Requires: Android Root](https://img.shields.io/badge/Requires-Android_Root-3DDC84?logo=android&style=for-the-badge)](https://source.android.com/)
[![Supports: OpenVPN/WireGuard/etc.](https://img.shields.io/badge/Supports-OpenVPN%2FWireGuard%2Fetc.-8A2BE2?style=for-the-badge)](https://www.vpn.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

**Route ALL tethered traffic through a VPN.** A production-ready, battle-tested Magisk module that extends your phone's VPN protection to every device connected via **Wi-Fi hotspot, Bluetooth tethering, or USB Ethernet** with optimized streaming and call quality.

> 🎯 **Production Edition** | Based on the original `vpn-gateway` by `mastermind`, extended with USB/Bluetooth support, streaming optimizations, and router health check fixes.

---

## 📋 Table of Contents
- [✨ Features](#-features)
- [🚀 Quick Start](#-quick-start)
- [🔧 How It Works](#-how-it-works)
- [📦 Installation](#-installation)
- [⚙️ Configuration](#️-configuration)
- [🎯 Verified Use Cases](#-verified-use-cases)
- [🔍 Troubleshooting](#-troubleshooting)
- [🤝 Contributing](#-contributing)
- [📜 Changelog](#-changelog)
- [⚠️ Disclaimer](#️-disclaimer)

---

## ✨ Features

### **Core Functionality**
| Feature | Status | Description |
|---------|--------|-------------|
| **Wi-Fi Hotspot VPN** | ✅ **Fully Working** | Route Wi-Fi (`wlan0`) tethered traffic through VPN |
| **Bluetooth Tethering** | ✅ **Fully Working** | Route Bluetooth (`bnep0`) PAN traffic through VPN |
| **USB Ethernet/Tethering** | ✅ **Fully Working** | Route USB (`usb0`/`eth0`) traffic through VPN |
| **Multi-Interface Support** | ✅ **Simultaneous** | All tethering methods work at the same time |

### **Optimizations & Fixes**
| Optimization | Benefit | Verified Working |
|--------------|---------|------------------|
| **MTU Optimization (1400)** | Prevents packet fragmentation for streaming | ✅ **IPTV Streaming** |
| **TCP Buffer Tuning** | Smooth video calls & reduced buffering | ✅ **Video/Voice Calls** |
| **Router Health Check Bypass** | Prevents false "offline" detection | ✅ **OpenWrt/mwan3** |
| **DNS Leak Protection** | Forces Cloudflare DNS (1.1.1.1) | ✅ **ipleak.net tested** |
| **IPv6 Leak Blocking** | Blocks IPv6 traffic outside VPN | ✅ **Enabled by default** |

---

## 🚀 Quick Start

### **For End Users**
1. **Root your Android device** (Magisk 20.4+ recommended)
2. **Download** the latest module zip from [Releases](../../releases)
3. **Install** via Magisk Manager → Modules → Install from Storage
4. **Reboot** and connect to your VPN
5. **Enable tethering** (Wi-Fi/Bluetooth/USB) - all traffic now routes through VPN!

### **For Developers**
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
