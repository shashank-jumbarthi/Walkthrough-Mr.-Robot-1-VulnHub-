# Walkthrough: Mr. Robot: 1 (VulnHub)

## 1. Reconnaissance

- **Identify Target IP:**
  - Use `netdiscover` or check your DHCP leases to find the VM's IP.
- **Scan for Open Ports and Services:**
  ```bash
  nmap -A -p- <target-ip>
  ```
  *Findings: Ports 22 (SSH), 80 (HTTP), and 443 (HTTPS) are open.*

## 2. Enumeration

- **Web Service:**
  - Browse to `http://<target-ip>`.
  - Notice Mr. Robot themed webpage.
  - Check `robots.txt` for clues:
    ```
    Disallow: /fsociety
    Disallow: /key-1-of-3.txt
    ```
    - Download `key-1-of-3.txt` (first flag).

- **Directory Brute-force:**
  - Use `gobuster` or `dirb`:
    ```bash
    gobuster dir -u http://<target-ip> -w /usr/share/wordlists/dirb/common.txt
    ```
  - Discover `/admin`, `/login`, `/wp-login.php`.

- **WordPress Enumeration:**
  - Use `wpscan`:
    ```bash
    wpscan --url http://<target-ip> -e u
    ```
  - Find default or weak credentials, enumerate users.

## 3. Exploitation

- **Login to WordPress:**
  - Try common usernames (`elliot`) and passwords.
  - Gain access to the admin panel.

- **Upload a Reverse Shell:**
  - Use a plugin or theme editor to upload a PHP reverse shell.
  - Start a listener:
    ```bash
    nc -lvnp 4444
    ```
  - Get a shell as `www-data`.

## 4. Privilege Escalation

- **Enumerate the System:**
  - Look for other flags (`key-2-of-3.txt`, `key-3-of-3.txt`).
  - Check for SUID binaries:
    ```bash
    find / -perm -4000 -type f 2>/dev/null
    ```
  - Check `/etc/passwd`, sudo privileges, and potential exploits.

- **Exploit for Root:**
  - Use local privilege escalation (e.g., outdated kernel, writable scripts).
  - Switch to root and access final flag.

## 5. Capture the Flags

- **Flag #1:** `/key-1-of-3.txt`
- **Flag #2:** (Found after further enumeration, e.g., `/home/robot/key-2-of-3.txt`)
- **Flag #3:** (Root flag, `/root/key-3-of-3.txt`)

## 6. Documentation & Reporting

- Summarize attacks (recon, web enumeration, exploitation, escalation).
- List tools used: Nmap, Gobuster, Wpscan, Netcat, Burp Suite.
- Note lessons learned and OSCP-aligned methodologies practiced.

---

*Replace details with specifics from your chosen VulnHub box. This template can be adapted for other labs such as Basic Pentesting: 1, with appropriate modifications for services and vulnerabilities discovered!*