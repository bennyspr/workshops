# **Simulation: Scanning and pinging computers on a LAN**

## **Step 1: Find Your Own IP Address**

### **Linux & macOS**
1. Open a terminal.
2. Find your IP address by running one of the following commands:

      ```sh
      # Linux
      ip a | grep "inet "
      ```
 
      ```sh
      # macOS
      ifconfig | grep "inet "
      ```
      
      ```sh
      # macOS WiFi
      ipconfig getifaddr en0
      ```
3. Look for an IP address in the format `192.168.x.x` or `10.x.x.x`. Write down your IP address.

### **Windows**
1. Open **Command Prompt**:
    - Press `Win + R`, type `cmd`, and press **Enter**.
2. Find your IP address by running:
   ```sh
   ipconfig
   ```
3. Look for **IPv4 Address** under your active network connection and write it down.

---

## **Step 2: Find All Connected Computers on the Network**
Now that you have your own IP, let's find other users' computers.

### **Linux & macOS**
1. Open a terminal.
2. Install `arp-scan` if it's not already installed:
   ```sh
   sudo apt install arp-scan -y  # Linux
   brew install arp-scan  # macOS
   ```
3. Scan the network by running:
   ```sh
   sudo arp-scan --localnet
   ```
4. This will list all connected devices, their IP addresses, and MAC addresses.
5. Write down the IP addresses of at least 3 other users.

### **Windows**
1. Open **Command Prompt**.
2. Run the following command to see connected devices:
   ```sh
   arp -a
   ```
3. This will list all devices on the local network. Write down the IP addresses of at least 3 other users.

---

## **Step 3: Ping Each Other**
Now, let's test connectivity by pinging your classmates' computers.

### **Linux & macOS**
1. Open a terminal.
2. Choose one of your classmates’ IP addresses (e.g., `192.168.1.25`).
3. Run:
   ```sh
   ping -c 4 192.168.1.25
   ```
4. If the connection is successful, you will see response times.
5. If the request times out, check with your classmate to ensure their firewall allows ICMP (ping) requests.
6. Repeat this step with the other users' IPs you wrote down.

### **Windows**
1. Open **Command Prompt**.
2. Choose one of your classmates’ IP addresses.
3. Run:
   ```sh
   ping 192.168.1.25
   ```
4. Observe the results and compare with your classmates.
5. Repeat this step with at least 3 users.

---

## **Troubleshoot**
If you cannot ping another user's computer:
- Make sure both computers are connected to the same Wi-Fi or Ethernet network.
- Temporarily disable firewalls:
    - **Linux/macOS:**
      ```sh
      sudo ufw disable
      ```
    - **Windows Defender:**
        1. Go to `Control Panel > Windows Defender Firewall > Advanced settings`
        2. Enable the rule: `File and Printer Sharing (Echo Request – ICMPv4-In)`.
- Check if the target computer responds to pings:
    - **Linux/macOS:**
      ```sh
      sudo ufw allow icmp
      ```

