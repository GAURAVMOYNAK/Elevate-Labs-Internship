
# 🔍 Task 1: Network Scan Report

## 📌 Task 1: Find Local IP Address and IP Range

### ✅ Objective
To identify the system’s local IP address, subnet mask, and calculate the corresponding IP address range using the `ifconfig` command in Kali Linux.

### 🛠️ Tool Used
- **Terminal in Kali Linux**
- Command: `ifconfig`

### 📝 Steps
1. Open Terminal in Kali Linux.
2. Run the command:  
   ```bash
   ifconfig
   ```
3. Observe the output under the interface `eth0`.

### 📤 Output & Findings

| Parameter         | Value              |
|------------------|--------------------|
| Local IP Address | 192.168.201.131    |
| Subnet Mask      | 255.255.255.0      |
| Broadcast Address| 192.168.201.255    |
| Network Interface| eth0               |

### 📐 IP Range Calculation
- **Subnet Mask:** `255.255.255.0` → `/24`
- **Network Address:** `192.168.201.0`
- **Usable IP Range:** `192.168.201.1` to `192.168.201.254`

### ✅ Result
```
IP Range: 192.168.201.1 to 192.168.201.254
```

---

## 📌 Task 2: Perform TCP SYN Scan Using Nmap

### ✅ Objective
Perform a **TCP SYN scan (`-sS`)** using Nmap on the local network `192.168.201.0/24` to discover active devices and their open TCP ports.

### 🛠️ Command Used
```bash
nmap -sS 192.168.201.0/24
```

### 🔍 Explanation
- **Nmap:** Tool used to discover devices and services on a network.
- **-sS:** Performs a stealthy TCP SYN scan.
- **192.168.201.0/24:** Scans all IPs from `.1` to `.254` in the subnet.

### 🌐 Network Range Scanned
- **192.168.201.0/24** — 254 possible IPs
- System IP: `192.168.201.131` (within range)

### 📤 Scan Result:

| IP Address       | Status   | Open Ports | Services | MAC Address           |
|------------------|----------|------------|----------|------------------------|
| 192.168.201.1    | Host Up  | 3306       | MySQL    | 00:50:56:C0:00:08 (VMware) |
| 192.168.201.2    | Host Up  | 53         | DNS      | 00:50:56:F3:5E:B6 (VMware) |
| 192.168.201.254  | Host Up  | None       | —        | 00:50:56:F9:90:B2 (VMware) |
| 192.168.201.131  | Host Up  | None       | —        | (Your own IP)              |

- **4 Hosts Detected as Active**

---

## 📌 Task 3: TCP SYN Scan and Wireshark Packet Analysis

### 🛠️ Tools Used
- **Nmap** – to perform TCP SYN scan.
- **Wireshark** – to capture and analyze packets.

### 1. Local IP Address and IP Range
- Command used:  
  ```bash
  ipconfig
  ```
- **IP Address:** `192.168.201.131`
- **Subnet Mask:** `255.255.255.0`
- **Range:** `192.168.201.0/24`

### 2. TCP SYN Scan (Again)
```bash
nmap -sS 192.168.201.0/24
```

#### Results Summary:

| IP Address      | Open Port(s) | Service |
|-----------------|---------------|---------|
| 192.168.201.1   | 3306/tcp      | MySQL   |
| 192.168.201.2   | 53/tcp        | DNS     |
| 192.168.201.254 | None          | —       |
| 192.168.201.131 | None          | —       |

- **4 Hosts Detected as Up**

### 3. Capturing Packets in Wireshark

#### Steps:
1. Open **Wireshark**.
2. Select your network interface (e.g., `eth0` or `ens33`).
3. Run the Nmap scan while Wireshark is capturing.
4. Stop capture after scan finishes.
5. Apply filters:

**To capture only SYN packets:**
```text
tcp.flags.syn == 1 and tcp.flags.ack == 0
```

**To capture SYN+ACK responses:**
```text
tcp.flags.syn == 1 and tcp.flags.ack == 1
```

---

## 🔍 Research: Common Services on Open Ports

### 📌 Port 3306 – MySQL

| Aspect            | Description |
|------------------|-------------|
| Use              | Open-source relational DBMS |
| Common Use       | Data storage in web apps and enterprise tools |
| Default Port     | TCP 3306 |

### Risks:
- **Unsecured Access**: Open to brute-force attacks
- **Data Leakage**: Poor config can expose data
- **Old Vulnerabilities**: Legacy versions may be exploitable
- **No IP Restrictions**: Accessible to any local device

**Mitigations:**
- Restrict access to trusted IPs
- Disable remote root login
- Use strong passwords
- Keep MySQL up to date
- Use firewall rules (e.g., `iptables`, `ufw`)

---

### 📌 Port 53 – DNS

| Aspect            | Description |
|------------------|-------------|
| Use              | Domain name to IP resolution |
| Protocols        | TCP and UDP |
| Default Port     | TCP/UDP 53 |

### Risks:
- **DNS Amplification**: Can be abused in DDoS
- **Zone Transfer Leaks**: Misconfiguration may expose DNS zones
- **Reconnaissance**: Internal mapping via DNS

**Mitigations:**
- Disable recursion on authoritative servers
- Block unauthorized zone transfers
- Limit access to trusted sources via firewall
- Monitor DNS logs for anomalies
