# Simulation: Netcat chat room

## Step 1: Install Netcat
### macOS and Linux
Most Linux distributions and macOS already have Netcat installed. You can check by running:
```sh
nc -h
```
If it’s not installed, you can install it using:
```sh
# Debian/Ubuntu
sudo apt install netcat

# Red Hat-based (Fedora, CentOS, RHEL)
sudo dnf install nc

# macOS (using Homebrew)
brew install netcat
```

### Windows
Windows does not have Netcat by default. You can download `ncat` (a Netcat alternative) from the official Nmap project:
- Download: [https://nmap.org/ncat/](https://nmap.org/ncat/)
- Place the executable in a known location (e.g., `C:\ncat\ncat.exe`)
- Add it to the system PATH (optional for easier use)

To test if `ncat` is installed, run:
```powershell
ncat -h
```
---

## Step 2: Start the Netcat Chat Server
As the instructor, you will start the chat server on your machine.

Find your local IP address by running:
```sh
# macOS/Linux
ipconfig getifaddr en0  # macOS (Wi-Fi)
ip a | grep inet        # Linux (look for local IP)

# Windows
ipconfig | findstr /i "IPv4 Address"
```

Once you have your local IP, start the server on a specific port (e.g., 4444):
```sh
# Linux
nc -lvnp 4444 

# macOS
nc -l 4444
```
```powershell
# Windows (using Ncat)
ncat -l -p 4444
```
Your server is now listening for incoming connections.

---

## Step 3: Users Connect as Clients
Each user will act as a client and connect to your server.

Users should replace `<SERVER_IP>` with the instructor’s local IP.

### macOS/Linux Clients
```sh
nc <SERVER_IP> 4444
```

### Windows Clients
```powershell
ncat <SERVER_IP> 4444
```

Once connected, users can start chatting by typing messages and pressing Enter.

---

## Step 4: Enhancing the Chat
### Enable Input from Multiple Users
To allow continuous communication, use `-k` (keep listening) on Linux/macOS:
```sh
nc -lk 4444
```
Windows `ncat` does not support `-k`, so you may need to restart the server manually if it disconnects.

### Logging Messages
If you want to log messages:
```sh
nc -lvnp 4444 | tee chat_log.txt
```

### Broadcasting Messages
You can send a message to multiple clients using:
```sh
echo "Hello Users!" | nc <CLIENT_IP> 4444
```

---

## Step 5: Closing the Connection
To stop the chat room:
- On the server, press `Ctrl + C` to terminate Netcat.
- On the client side, type `Ctrl + C` or simply close the terminal.

---
