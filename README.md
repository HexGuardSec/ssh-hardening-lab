# SSH Hardening Lab

**Hardened SSH setup on Ubuntu Server with key-based authentication, custom port, and fail2ban protection.**

---

## 📌 Project Overview

This project simulates the process of securing an Ubuntu Server's SSH configuration in a real-world IT environment. The goal is to improve security by using public key authentication, changing the default SSH port, and protecting against brute-force attacks with `fail2ban`.

> Environment:
- Ubuntu Server: 192.168.56.106
- Kali Linux (attacker/test client): 192.168.56.107
- SSH port: 2222

---

## 🛠️ Configuration Steps

### ✅ 1. SSH Key-Based Authentication
- SSH key generated on Kali using `ssh-keygen`
- Public key copied to Ubuntu via `ssh-copy-id`
- Password login disabled in `/etc/ssh/sshd_config` or the specific `/etc/ssh/sshd_config.d/50-cloud-init.conf` file (in case of cloud-init configurations)

### ✅ 2. Custom SSH Port
- Changed default SSH port from `22` to `2222`
- Config updated in `/etc/ssh/sshd_config` or `/etc/ssh/sshd_config.d/50-cloud-init.conf` if using cloud-init
- SSH service tested with `ss -tulnp` and `sshd -T`
- UFW adjusted (if enabled): `sudo ufw allow 2222/tcp`

### ✅ 3. `fail2ban` Installation & Protection
- Installed via `sudo apt install fail2ban`
- Configured `/etc/fail2ban/jail.local` to protect SSH on port 2222
- Ban time: 10 minutes
- Max retries: 3 within 5 minutes
- Verified via `fail2ban-client status sshd`

---

## 💻 Testing Scenario

1. Simulated multiple failed SSH login attempts from Kali using fake credentials.
2. Observed the ban trigger using:
   ```bash
   sudo fail2ban-client status sshd
````

3. Unbanned Kali IP manually using:

   ```bash
   sudo fail2ban-client set sshd unbanip 192.168.56.107
   ```

---

## ⚠️ Troubleshooting

### ❌ Error: `sshd: no hostkeys available -- exiting`

**Cause**: Missing host keys in `/etc/ssh/`
**Fix**:

```bash
sudo ssh-keygen -A
sudo systemctl restart ssh
```

---

## 📂 Project Structure

```
ssh-hardening-lab/
├── README.md
├── LICENSE
├── configs/
│   └── sshd_config
├── screenshots/
│   ├── ssh_keygen_kali.png
│   ├── ssh_connection_2222.png
│   ├── sshd_port_check.png
│   ├── jail_local_config.png
    └── fail2ban_status.png
```

---

## ✅ Final Result

SSH on Ubuntu Server is now:

* Accessible only via SSH keys
* Listening on a non-standard port (2222)
* Protected from brute-force attacks using `fail2ban`

---

## 🧠 Skills Practiced

* Linux system administration
* SSH configuration and debugging
* Security hardening
* Log analysis & troubleshooting
* Fail2ban usage and configuration
