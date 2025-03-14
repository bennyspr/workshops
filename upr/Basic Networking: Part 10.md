# Basic Networking: Part 10

## 1. **Understanding HTTP and HTTPS**

###  HTTP
HTTP (HyperText Transfer Protocol) is the foundational protocol used for communication on the web. It allows clients (like web browsers) to request resources from servers, enabling the retrieval of web pages, images, and other online content. However, HTTP does not provide encryption, making data transmission vulnerable to interception and attacks such as man-in-the-middle (MITM).

### HTTPS
HTTPS (HyperText Transfer Protocol Secure) is the secure version of HTTP. It incorporates **SSL/TLS (Secure Sockets Layer / Transport Layer Security)** to encrypt data exchanged between the client and the server. This encryption ensures confidentiality, integrity, and authentication, protecting sensitive information like login credentials, payment details, and personal data.

### Key Differences

![Image](https://cdn.venafi.com/994513b8-133f-0003-9fb3-9cbe4b61ffeb/87b0e3b5-1dc7-4778-acec-82a06e21217c/Difference_HTTP_HTTPS-2.png?fm=webp&q=85)

### How to Implement HTTPS
- Obtain an SSL/TLS certificate from a trusted Certificate Authority (CA) (e.g., Let's Encrypt, DigiCert).
- Install the certificate on your web server.
- Configure web server settings to enforce HTTPS.
- Update internal links and third-party resources to use HTTPS.


### Exercise: Compare HTTP and HTTPS connections.
1. Install `curl` if not installed:
   ```sh
   # Install in Linux
   sudo apt install curl -y 
   
   # Install in macOS
   brew install curl
   ```
2. Fetch a webpage using HTTP:
   ```sh
   curl -I http://example.com
   ```
3. Fetch a webpage using HTTPS:
   ```sh
   curl -I https://example.com
   ```
4. Observe the differences in responses.

More info: https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview

---

## 2. **Domain Name System (DNS)**

DNS (Domain Name System) is a fundamental component of the internet that translates human-friendly domain names (e.g., `example.com`) into machine-readable IP addresses (e.g., `192.168.1.1`). This process allows users to access websites using easily memorable names instead of numerical IP addresses.

![How it works](https://cf-assets.www.cloudflare.com/slt3lc6tev37/1NzaAqpEFGjqTZPAS02oNv/bf7b3f305d9c35bde5c5b93a519ba6d5/what_is_a_dns_server_dns_lookup.png)

[YouTube Video: DNS Explained in 100 Seconds](https://www.youtube.com/watch?v=UVR9lhUGAyU)

### Public DNS Providers
You can use alternative DNS resolvers for better speed, security, and reliability:

| Provider           | Primary DNS      | Secondary DNS    |
|--------------------|------------------|------------------|
| **Google DNS**     | `8.8.8.8`        | `8.8.4.4`        |
| **Cloudflare DNS** | `1.1.1.1`        | `1.0.0.1`        |
| **OpenDNS**        | `208.67.222.222` | `208.67.220.220` |

### Exercise: Resolve domain names to IP addresses.
1. Use `nslookup` to find an IP:
   ```sh
   nslookup google.com
   ```
   
More Info: https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/

---

## 3. **Dynamic Host Configuration Protocol (DHCP)**

Dynamic Host Configuration Protocol (DHCP) is a network management protocol used to automatically assign IP addresses and other network configuration settings to devices on a network. This eliminates the need for manual IP configuration and ensures efficient IP address allocation.

![DHCP](https://www.boardinfinity.com/blog/content/images/2023/05/DHCP-protocol.png)

[YouTube Video: What is DHCP and How Does it Work? | Network Essentials](https://youtu.be/ldtUSSZJCGg?si=-emZgd6v-QO7G0SY)

### Exercise: Check how DHCP assigns IP addresses.
1. Check network interface configuration:
   ```sh
   # Linux
   ip addr show
   
   # Linux
   ifconfig -a
   
   # macOS
   ifconfig
   
   # Windows
   ipconfig /all
   ```
2. Find DHCP lease information:
   ```sh
   # Linux
   cat /var/lib/dhcp/dhclient.leases
   
   # Linux
   cat /var/lib/dhclient/dhclient.leases
   
   # macOS
   cat /var/db/dhcpd_leases
   
   # macOS (Replace en0 if needed)
   ipconfig getpacket en0
   
   # macOS
   ipconfig getpacket en0 | grep -i lease
   
   # Windows
   ipconfig /all
   ```
More Info:
https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-top

---

## 5. **Understanding Subnet Masks**

A **Subnet Mask** is a numerical value used in networking to divide an IP address into a **network** and **host** portion. It helps determine which part of an IP address belongs to the network and which part identifies a specific device within that network.

![Subnet](https://miro.medium.com/v2/resize:fit:1400/0*4CZXdRfQlJqSMLvk.png)

[Youtuve Video: Subnet Mask - Explained)](https://www.youtube.com/watch?v=s_Ntt6eTn94)

### Exercise 1: Practice Subnetting IPv4.
1. Visit https://subnetipv4.com

### Exercise 2: Verify subnet masks.
1. Display network configuration:
   ```sh
   # Linux
   ip a
   
   # macOS and Linux
   ifconfig
   
   # Windows
   ipconfig /all
   ```
2. Calculate subnet masks using `ipcalc`:
   ```sh
   # Install on Linux
   sudo apt install ipcalc
   # Install on macOS
   brew install sipcalc
   
   # Linux
   ipcalc 192.168.1.0/24
   # macOS
   sipcalc 192.168.1.0/24
   
   # Windows PowerShell
   $ip = "192.168.1.0"
   $subnet = 24
   Write-Output "Network: $ip/$subnet"
   ```

More Info: https://www.spiceworks.com/tech/networking/articles/what-is-subnet-mask/

---

## 7. **Classless Inter-Domain Routing (CIDR)**

Classless Inter-Domain Routing (CIDR) is a method for allocating IP addresses and efficiently managing IP address space. It was introduced in **1993** (RFC 1519) to replace the older **class-based system (Class A, B, and C)**, which was inefficient in terms of IP address utilization.

Instead of using fixed-size subnet masks (e.g., Class A = `/8`, Class B = `/16`), CIDR allows **flexible subnetting** using variable-length subnet masks (VLSM). This helps in reducing IP address wastage and improving routing efficiency.

![CIDR](https://www.ipxo.com/app/uploads/2021/09/CIDR-2.png)

[Youtube Video: What is network CIDR notation?](https://www.youtube.com/watch?v=tpa9QSiiiUo)


1. Display network information:
   ```sh
   # Linux
   ip -o -f inet addr show | awk '{print $2, $4}'
   ```
2. Convert a subnet mask to CIDR:
   ```sh
   # Linux
   ipcalc 192.168.1.1 255.255.255.0
   ```

More Info: https://aws.amazon.com/what-is/cidr/

---

## 8. **MAC Address**

**Media Access Control (MAC)** is a sublayer of the **Data Link Layer (Layer 2)** in the **OSI model**. It is responsible for controlling how devices in a network access and transmit data over the physical medium.

Every network device, such as computers, routers, and smartphones, has a unique **MAC address** assigned to its network interface card (NIC). This address is used for device identification and communication within local networks (LANs).


![MAC](https://media.fs.com/images/ckfinder/ftp_images/tutorial/mac-addresse-numbers.jpg)

Check MAC addresses:
   ```sh
   # Linux
   ip link show
   
   # Linux 
   ifconfig -a
   
   # macOS
   ifconfig
   
   # Windows 
   ipconfig /all
   ```

More Info: https://www.geeksforgeeks.org/mac-address-in-computer-network/

---

## 9. **Default Gateway**

A **network gateway** is a device that connects different networks, allowing communication between them. In most home and office networks, the **default gateway** is a router that connects internal devices to external networks, such as the internet.

The default gateway acts as an intermediary between a local network (LAN) and other networks (WAN or the internet), forwarding packets that are not destined for the local subnet.


![Gateway](https://www.homenethowto.com/wp-content/uploads/default-gateway.jpg)

[Youtube Video: Routers and Default Gateways](https://www.youtube.com/watch?v=JOomC1wFrbU)

### Exercise: Find the default gateway of the system.

   ```sh
   # Linux
   ip route show
   
   # macOS
   netstat -rn
   
   # Windows 
   route print
   
   # To show only IPv4 routes in Windows 
   route print -4
   ```

More Info: https://nordvpn.com/blog/what-is-a-default-gateway

---
