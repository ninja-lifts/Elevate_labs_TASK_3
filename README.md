# 🔍 Vulnerability Assessment Report

**Scan Tool:** Greenbone Vulnerability Manager (OpenVAS) --> installed it in my kali Linux system ( Most difficut task cause i have to install all dependecies and updated my PUBLIC KEY for downloading on my system )     
**Scan Target IP:** `192.168.1.4`  

---

## 📊 Summary of Findings

| Severity | Count | Affected Port | Description                             |
|----------|-------|----------------|-----------------------------------------|
| 🟠 Medium  | 1     | TCP/135        | DCE/RPC Services Enumeration            |
| 🟡 Low     | 1     | General/TCP    | TCP Timestamp Information Disclosure    |

---

## 🛑 Medium Severity

### 🧩 DCE/RPC & MSRPC Services Enumeration  
- **Port:** TCP/135  
- **Severity:** Medium  
- **CVSS Score:** 5.0  
- **Description:**  
  Remote procedure call services like `lsass.exe`, `spoolsv.exe`, and `epmapper` are remotely discoverable via Microsoft RPC endpoint mapper. These services expose sensitive information such as named pipes and UUIDs.

- **Risk Impact:**  
  An attacker can enumerate these services to perform lateral movement, service exploitation, or privilege escalation. Specifically, access to `lsass.exe` is often a precursor to credential dumping.

- **Recommended Mitigations:**  
  - Block TCP/135 from untrusted networks using a firewall:
    ```bash
    sudo ufw deny in on eth0 to any port 135 proto tcp
    ```
  - On Windows:
    - Use Windows Defender Firewall to **block inbound traffic on port 135**
    - Disable unused services like Print Spooler (`spoolsv.exe`) if not needed:
      ```powershell
      Stop-Service -Name spooler
      Set-Service -Name spooler -StartupType Disabled
      ```

---

## 🟡 Low Severity

### 🧩 TCP Timestamp Information Disclosure  
- **Port:** General TCP  
- **Severity:** Low  
- **CVSS Score:** 2.6  
- **Description:**  
  The system responds to TCP packets with RFC 1323 timestamp values. This can allow attackers to estimate system uptime or fingerprint kernel versions.

- **Risk Impact:**  
  Not directly exploitable, but useful for reconnaissance, OS fingerprinting, and timing analysis of reboots or downtime.

- **Recommended Mitigations:**  
  - **On Linux:**
    ```bash
    echo "net.ipv4.tcp_timestamps = 0" | sudo tee -a /etc/sysctl.conf
    sudo sysctl -p
    ```
  - **On Windows (if supported):**
    ```cmd
    netsh int tcp set global timestamps=disabled
    ```

---

## ✅ Recommendations & Next Steps

- 🛠 Fix the above issues using recommended steps.
- 🔁 Re-run a full scan after applying patches or configuration changes.
- 🗓 Schedule regular scans (e.g., weekly) to catch new issues early.
- 📉 Minimize exposure of unnecessary services (e.g., spooler, SMB).
- 🧱 Harden firewall rules to limit external access to critical ports.

---

## 📎 Appendix

- **Tools Used:** Greenbone Community Edition (OpenVAS)
- **System Status:** Feeds up-to-date as of scan time
- **Total Open Ports Found:** *(Not listed – assumed by context)*

---

> _“Prevention is better than breach.”_  
> — Your security team

