# 🔁 Universal VPN Gateway

[![Magisk Module](https://img.shields.io/badge/Platform-Magisk_Module-00AF9C?logo=magisk&style=for-the-badge)](https://magiskmanager.com/)
[![Requires Root](https://img.shields.io/badge/Requires-Android_Root-3DDC84?logo=android&style=for-the-badge)](https://source.android.com/)
[![VPN Support](https://img.shields.io/badge/Supports-OpenVPN%2FWireGuard%2Fetc.-8A2BE2?style=for-the-badge)](https://www.vpn.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

**Route ALL your tethered traffic through a VPN.** Extends your phone's VPN protection to every device connected via **Wi-Fi hotspot, Bluetooth tethering, or USB Ethernet**.

> ✨ **Extended Fork** | Based on the original `vpn-gateway` by `Kr328` with added Bluetooth & USB support.

---

## 📋 Table of Contents
- [✨ Features](#-features)
- [🛠️ How It Works](#️-how-it-works)
- [📦 Installation](#-installation)
- [🚀 Usage](#-usage)
- [🔧 For Advanced Users](#-for-advanced-users)
- [❓ FAQ & Troubleshooting](#-faq--troubleshooting)
- [🤝 Contributing](#-contributing)
- [⚠️ Disclaimer](#️-disclaimer)

---

## ✨ Features

| Feature | Description | Status |
|---------|-------------|--------|
| **🔗 Multi-Interface Support** | Works with Wi-Fi, Bluetooth, **and** USB tethering simultaneously | ✅ **Fully Working** |
| **🌐 Universal VPN Routing** | All tethered traffic forced through your active VPN tunnel | ✅ **Core Function** |
| **🛡️ DNS Leak Protection** | DNS queries redirected to secure public resolver (1.1.1.1) | ✅ **Enabled** |
| **⚡ System Integrated** | Uses native Android tethering - no extra apps needed | ✅ **Seamless** |
| **🔄 Automatic Management** | Rules apply/clean up automatically with VPN connection | ✅ **Hands-free** |

## 🛠️ How It Works

```mermaid
graph LR
    A[Laptop/Tablet/Phone] -->|Wi-Fi| B[Android Phone]
    A -->|Bluetooth| B
    A -->|USB Cable| B
    
    B --> C{Universal VPN Gateway<br/>Magisk Module}
    C -->|Routes all traffic| D[VPN Tunnel<br/>tun0/wg0]
    D --> E[Internet]
    
    F[Real IP] -.->|Blocked| A
    D -->|VPN IP Only| A
