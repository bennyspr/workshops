# Basic Networking

### 1. Understanding IP Addresses
An **IP (Internet Protocol) address** is a unique identifier assigned to a device in a network, allowing communication between devices. 

![IPv1 Address Format](https://media.geeksforgeeks.org/wp-content/uploads/20241217170153436945/IPv4-address-format.webp)

Run the following command to display your IP address:
```bash
ip a
```
**Exercise:** Compare the results with the `ifconfig` command to see the difference.

#### Questions:
- What is your current IPv4 address?
- What is your current subnet mask?
- Do you have an IPv6 address assigned?

---

### 2. IPv4 vs IPv6
#### What’s the Difference?
- **IPv4** uses a **32-bit address** (e.g., `192.168.1.1`).
- **IPv6** uses a **128-bit address** (e.g., `2001:db8::ff00:42:8329`).

![IPv4 vs. IPv6](https://signal.avg.com/hs-fs/hubfs/Blog_Content/Avg/Signal/AVG%20Signal%20Images/IPv4%20vs.%20IPv6%20addresses%20(Signal)/IPv4-vs-IPv6-EN.png?width=1320&name=IPv4-vs-IPv6-EN.png)


#### Exercise: Display IPv4 and IPv6 Addresses
```bash
ip a | grep inet
```

---

### 3. Default gateway
A **default gateway** is the networking device that routes traffic from a local network to other networks or the internet. It acts as an intermediary between your device and external networks.

![Default Gateway](https://www.homenethowto.com/wp-content/uploads/default-gateway.jpg)

#### Exercise: Find your system's default gateway.

```bash
ip route show
```
or
```bash
netstat -rn
```

---

### 4. Network Address Translation (NAT)
NAT allows multiple devices on a local network to share a single public IP address when accessing the internet.

![NAT](https://www.vmware.com/media/blt8c9a8aaca0ffd4ac/blt1b4d43cfd751feb5/66d1a48342d82e12814163ad/network-address-translation-diagram.png)

#### Exercise: Find Your Public IP
```bash
curl ifconfig.me
```

---

### 5. Domain Name System (DNS)
DNS translates domain names (e.g., `www.example.com`) into IP addresses (e.g., `12.34.56.78`).

![DNS](https://lh6.googleusercontent.com/9HL6XqrvIqt7YlVfRsCp9dpVfg6sj8aJ-M1FO-nzL0XLkGLpOBbe_orlVbdo9VeGpGFHFMgi_POOGzi83HGl1A1dHkZDfDsaC0XdZ2SzyBJ0bLy84ABckxC74a17mX11RcyyK_3QMIAu5sMl2dqfAUXZ6wmUGtZzEqxKGCnTX-uhEpcOUN8oazTf8Ccm5A)

#### Exercise: Find Google's IP address
```bash
nslookup google.com
```
**Try This:** Look up different domains and note the IP changes.

---

### 6. Using `host` and `nslookup`
The `host` command is a simple tool for performing DNS lookups. It is available on Linux and macOS by default.

```bash
host google.com
```

The `nslookup` command is another tool for querying DNS information. It is available on Windows, Linux, and macOS.


```bash
nslookup google.com
```

By default, `nslookup` uses your system’s configured DNS server. You can specify a different server as follows:

```bash
nslookup google.com 8.8.8.8
```
#### Key Differences Between `host` and `nslookup`

| Feature         | `host`                      | `nslookup`                   |
|---------------|----------------------------|-----------------------------|
| Simplicity    | Simple and direct output   | More detailed output        |
| Record Types  | Uses `-t` option           | Uses `-query=` option       |
| Reverse Lookup | Supports IP-to-domain     | Supports IP-to-domain       |
| Interactive Mode | No                        | Yes                         |
| Default OS   | Linux/macOS                 | Windows/Linux/macOS         |

**Try This:** Try these commands with `.gov` and `.edu` domains.

---

### 7. Tracing Network Routes with `traceroute`
The `traceroute` command shows the path data packets take from your device to a destination.

![Traceroute](https://www.cloudns.net/blog/wp-content/uploads/2021/03/Traceroute-command-ClouDNS-3.png)

#### Exercise: Trace the Route to Google
```bash
traceroute google.com
```

---

### 8. Understanding Ports
Ports are communication endpoints. Common ports include:
- **80**: HTTP
- **443**: HTTPS
- **22**: SSH

![Ports](https://www.stationx.net/wp-content/uploads/2022/12/Well-Known-Ports-Unencrypted-vs-Encrypted-Graphic-by-author.png)

#### Exercise: List Open Ports on Your System
```bash
netstat -tuln
```
**Try This:** Find out which service is running on a specific port.

---

### 9. Using `netstat`

The `netstat` (network statistics) command is a powerful tool used to monitor network connections, routing tables, interface statistics, and more. It is available on Linux, macOS, and Windows. In this tutorial, we will explore common netstat commands and how to interpret their output.

To see all ports that your computer is currently listening on:
```bash
netstat -l  
```

To see which processes are using network connections:
```bash
netstat -tulnp 
```

**Try This:** Use `netstat -r` to display routing information (routing table).

---

### 10. TCP vs UDP
- **TCP**: Reliable, connection-oriented (e.g., web browsing).
- **UDP**: Faster but connectionless (e.g., video streaming).

![TCP vs UDP Communication](https://www.colocationamerica.com/wp-content/uploads/2018/12/udp-tcp.jpg)

[![How TCP works - IRL](https://i.ytimg.com/vi/R6WN4_bBB1Q/maxresdefault.jpg?sqp=-oaymwEoCIAKENAF8quKqQMcGADwAQH4AbYIgAKAD4oCDAgAEAEYZSBLKEkwDw==&rs=AOn4CLAqQk4OzTmBHxvf6FLvXxIYGb_ZoQ)](https://youtu.be/R6WN4_bBB1Q?si=tQtPG7OL6tIeMnVU)


#### Exercise: Capture TCP/UDP Traffic
```bash
sudo tcpdump -i eth0 -n port 53
```
**Try This:** Capture traffic for a different port.

---

### 11. Using `nc` (Netcat)
Netcat is a powerful tool for reading and writing network connections.

#### Interactive Exercise: Chat with a Partner
1. **One student starts a TCP listener:**
   ```bash
   nc -lvp 4444
   ```
2. **The other student connects to it using their partner's IP:**
   ```bash
   nc <partner_ip> 4444
   ```
3. Type messages and see them appear on the other system.

**Try This:** Transfer a file instead of messages.

---

### 12. Testing Connectivity with `ping`

The `ping` command checks network connectivity and measures response time.

#### Interactive Exercise: Ping Each Other
1. Get your partner’s IP address using the `ip a` command.
2. Try pinging their machine:
   ```bash
   ping -c 5 <partner_ip>
   ```
3. Discuss if the pings succeed. If not, check firewall settings.

**Try This:** Experiment with `ping -i 0.2 <partner_ip>` to change intervals.

---
