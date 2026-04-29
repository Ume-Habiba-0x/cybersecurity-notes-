# Nmap Quick Review

## 1. Host Discovery

| Command          | Purpose                       | When to Use                       |
| ---------------- | ----------------------------- | --------------------------------- |
| `nmap -sn IP/24` | Ping sweep to find live hosts | Discover active systems on subnet |
| `nmap -Pn IP`    | Skip host discovery ping      | When ICMP is blocked by firewall  |

---

## 2. Common Scans

| Command                       | Purpose                            | Notes                      |
| ----------------------------- | ---------------------------------- | -------------------------- |
| `nmap -sC -sV IP`             | Default scripts + service versions | Best first targeted scan   |
| `nmap -p- IP`                 | Scan all TCP ports                 | Finds hidden ports         |
| `nmap -p- --min-rate 5000 IP` | Fast full port scan                | Good for labs / exam speed |
| `nmap -sU IP`                 | UDP scan                           | Check DNS, SNMP, TFTP      |

---

## 3. Fast Workflow

| Step | Command                                        | Purpose                    |
| ---- | ---------------------------------------------- | -------------------------- |
| 1    | `nmap -Pn -p- --min-rate 5000 -oN full.txt IP` | Discover all TCP ports     |
| 2    | `nmap -sC -sV -p PORTS -oN targeted.txt IP`    | Enumerate discovered ports |

---

## 4. OS Detection

| Command      | Purpose                                   |
| ------------ | ----------------------------------------- |
| `nmap -O IP` | Detect operating system                   |
| `nmap -A IP` | Aggressive scan (OS, scripts, traceroute) |

---

## 5. Firewall Evasion Techniques

| Command                    | Method               | Explanation                                                  |
| -------------------------- | -------------------- | ------------------------------------------------------------ |
| `nmap -f IP`               | Fragmentation        | Splits packets into smaller fragments to test weak firewalls |
| `nmap --mtu 24 IP`         | Custom MTU           | Adjusts packet size to bypass poor filtering                 |
| `nmap -D RND:10 IP`        | Decoys               | Mixes your scan with fake IPs                                |
| `nmap --source-port 53 IP` | Source Port Spoofing | Uses trusted ports like DNS                                  |
| `nmap --data-length 25 IP` | Packet Padding       | Changes packet signature                                     |

---

## 6. Output Saving

| Command                | Purpose            |
| ---------------------- | ------------------ |
| `nmap -oN scan.txt IP` | Save normal output |
| `nmap -oA scan IP`     | Save all formats   |

---

## 7. Important Ports to Check

### Windows Targets

| Port      | Service  | Why Important           |
| --------- | -------- | ----------------------- |
| 139/445   | SMB      | Shares, users, exploits |
| 5985/5986 | WinRM    | Remote PowerShell       |
| 3389      | RDP      | Remote desktop          |
| 389       | LDAP     | Active Directory        |
| 88        | Kerberos | Domain authentication   |

### Linux Targets

| Port   | Service | Why Important           |
| ------ | ------- | ----------------------- |
| 22     | SSH     | Remote shell            |
| 21     | FTP     | Anonymous login / files |
| 2049   | NFS     | Mount shares            |
| 80/443 | HTTP    | Web exploitation        |

### General Targets

| Port | Service |
| ---- | ------- |
| 53   | DNS     |
| 161  | SNMP    |
| 25   | SMTP    |

---

## 8. Why Port 445 is Important

| Port | Service | Meaning                                |
| ---- | ------- | -------------------------------------- |
| 445  | SMB     | Windows file sharing, AD communication |

### Why It Matters

* File and printer sharing
* Active Directory authentication
* Remote administration
* Common target for enumeration and exploits

### In Labs

Often intentionally open for:

* SMB Enumeration
* Guest share discovery
* User enumeration
* Vulnerability testing

---

## 9. Be strategic!

| Rule | Action                                |
| ---- | ------------------------------------- |
| 1    | Run full port scan first              |
| 2    | Run targeted scan after               |
| 3    | Enumerate every open service          |
| 4    | Check UDP if stuck                    |
| 5    | Save all results                      |
| 6    | Port 445 open = enumerate immediately |
