# 🛠️ Task 1: Local Network Port Scanning

## 📌 Objective
Discover active devices and open ports in the local network using Nmap. Optionally observe network packets using Wireshark to understand scan behavior and exposure.

## 🧰 Tools Used
- **Nmap** – For scanning the local network (`-sS` TCP SYN scan)
- **Wireshark** *(optional)* – For capturing and analyzing SYN packets
- **Kali Linux** – Virtual Machine used for scanning

## 📝 Steps Performed

1. **Identified local IP and subnet range**  
   Command:
   ```bash
   ifconfig
   ```
   - Example IP: `192.168.201.131`
   - Network Range: `192.168.201.0/24`

2. **Scanned network using Nmap TCP SYN scan**  
   Command:
   ```bash
   nmap -sS 192.168.201.0/24
   ```

3. **Devices & Ports Detected**
   | IP Address      | Open Ports | Service |
   |------------------|-------------|---------|
   | 192.168.201.1    | 3306        | MySQL   |
   | 192.168.201.2    | 53          | DNS     |
   | 192.168.201.254  | None        | -       |
   | 192.168.201.131  | None        | -       |

4. **(Optional) Packet Capture using Wireshark**
   - Started capture on active interface (e.g., eth0)
   - Ran Nmap scan
   - Applied filter:
     ```bash
     tcp.flags.syn == 1 && tcp.flags.ack == 0
     ```

## 🔐 Risks Identified

| Port | Service | Risk Summary                        |
|------|---------|-------------------------------------|
| 3306 | MySQL   | Possible unauthorized DB access     |
| 53   | DNS     | Can be exploited for DNS attacks    |

## ✅ Outcome
Successfully scanned the network and identified 4 live hosts. Found open MySQL and DNS ports that may be vulnerable if not secured. Optional Wireshark capture confirmed TCP SYN packet behavior.
