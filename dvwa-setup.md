# Setting Up DVWA (Damn Vulnerable Web Application)

## What is DVWA?

DVWA (Damn Vulnerable Web Application) is a 
deliberately insecure web application used by 
security professionals and students to enhance 
their cybersecurity skills in a safe and controlled 
environment.

It provides a legal and safe target to practice 
common web application attacks including SQL 
injection, brute force, cross site scripting and 
more without affecting real systems.

## Why DVWA?

In this home lab DVWA is used to:

- Simulate a vulnerable web application target
- Practice real world attack techniques
- Test Wazuh SIEM detection capabilities
- Generate security alerts for investigation
- Produce findings for a SOC incident report

## How DVWA Works in This Lab
```
Kali Linux VM → attacks DVWA
↓
Ubuntu-DVWA VM → hosts DVWA web application
↓
Wazuh SIEM → detects and logs the attacks
↓
SOC Analyst → investigates alerts and writes
incident report
```

## System Requirements

### Ubuntu-DVWA VM Specifications
- OS: Ubuntu Server 22.04 LTS
- RAM: 2GB
- Storage: 50GB
- Network: Bridged Adapter
- IP Address: 192.168.1.244

### Software Requirements
- Apache2 web server
- PHP
- MariaDB database
- Git

---

## Part 1: Installing DVWA

### Step 1: Install Required Packages

SSH into your Ubuntu-DVWA VM and run:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y apache2 php php-mysqli php-gd libapache2-mod-php mariadb-server git
```

This installs:
- **Apache2** — web server that serves DVWA
- **PHP** — programming language DVWA is built on
- **MariaDB** — database that stores DVWA data
- **Git** — used to download DVWA from GitHub

### Step 2: Download DVWA from GitHub

Clone the DVWA repository into the Apache web folder:

```bash
sudo git clone https://github.com/digininja/DVWA.git /var/www/html/dvwa
```

### Step 3: Configure DVWA

Copy the sample config file:

```bash
sudo cp /var/www/html/dvwa/config/config.inc.php.dist /var/www/html/dvwa/config/config.inc.php
```

Set correct permissions so Apache can access DVWA:

```bash
sudo chown -R www-data:www-data /var/www/html/dvwa
sudo chmod -R 755 /var/www/html/dvwa
```

### Step 4: Set Up Database

Start MariaDB:

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

Secure the installation:

```bash
sudo mysql_secure_installation
```

Create the DVWA database and user:

```bash
sudo mysql -u root -p
```

```sql
CREATE DATABASE dvwa;
CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'p@ssw0rd';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Step 5: Update Config File

Edit the DVWA config file:

```bash
sudo nano /var/www/html/dvwa/config/config.inc.php
```

Update the database password:

```php
$_DVWA['db_password'] = getenv('DB_PASSWORD') ?: 'p@ssw0rd';
```

### Step 6: Start Apache

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```

### Step 7: Change Network Adapter to Bridged

By default the VM uses NAT which gives a 
10.0.2.x address that is not accessible 
from your laptop browser.

1. Power off the VM
2. Go to **Settings → Network**
3. Change **Attached to: NAT** to **Bridged Adapter**
4. Start the VM again
5. Run `ip a` to get the new IP address

The VM should now have a **192.168.1.x** address!

---

## Part 2: Accessing DVWA

### Step 1: Find VM IP Address

```bash
ip a
```

Look for the **inet** address under **enp0s3**:
```
192.168.1.244
```

### Step 2: Access Setup Page

Open your laptop browser and go to:

`http://192.168.1.244/dvwa/setup.php`

You will see the DVWA setup page showing 
system information and a button at the bottom.

<img src="Screenshots/dvwa-setup/dvwa-setup.png" width="600" alt="DVWA Setup Page">
<p><em>DVWA setup page showing system status and create database button</em></p>

### Step 3: Create Database

Click the **Create / Reset Database** button 
at the bottom of the setup page.

This creates all the necessary database tables 
and populates DVWA with sample vulnerable data.

You will be automatically redirected to the 
login page after the database is created.

### Step 4: Login to DVWA

**Default credentials:**
```
Username: admin
Password: password
```

<img src="Screenshots/dvwa-setup/dvwa-login-page.png" width="600" alt="DVWA Login">
<p><em>DVWA login page</em></p>

### Step 5: DVWA Dashboard

After logging in you will see the DVWA 
dashboard with a menu on the left side 
containing all the vulnerability modules:

- Brute Force
- Command Injection
- CSRF
- File Inclusion
- File Upload
- SQL Injection
- XSS (Reflected)
- XSS (Stored)
- And more!

<img src="Screenshots/dvwa-setup/dvwa-welcome.png" width="600" alt="DVWA Dashboard">
<p><em>DVWA dashboard showing all available vulnerability modules</em></p>

### Step 6: Set Security Level

1. Click **DVWA Security** in the left sidebar
2. Change security level to **Low**
3. Click **Submit**

Setting the level to Low makes all 
vulnerabilities fully exploitable for 
practice purposes.

<img src="Screenshots/dvwa-setup/security-level.png" width="600" alt="DVWA Security Level">
<p><em>DVWA security level set to Low for practice</em></p>

> Note: In a real environment Low security 
> would never be used. This setting is only 
> for controlled home lab practice.

---

## What's Next

DVWA is now fully installed and configured. 
In Phase 6 Kali Linux will be used to perform 
attacks against DVWA including:

- SQL injection attacks
- Brute force login attempts
- Cross site scripting (XSS)

Wazuh will monitor and detect these attacks 
generating security alerts for investigation 
and incident reporting.


---

## Phase 6: Attack Simulation Results

### Brute Force Attack (Hydra)

**Attack Command:**

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.1.244 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed" -V -t 4
```

**Command breakdown:**

- `hydra` — the brute-force tool itself
- `-l admin` — single login name to try (DVWA's default username, not guessing usernames)
- `-P /usr/share/wordlists/rockyou.txt` — password wordlist; Hydra tries every line in this file as a password (rockyou.txt contains ~14 million real passwords leaked from past data breaches)
- `192.168.1.244` — target IP (the DVWA VM)
- `http-post-form` — tells Hydra the login happens via an HTTP POST form, not SSH/FTP/etc.
- `"/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"` — the form details, split by colons:
  - `/dvwa/login.php` — the page the login form submits to
  - `username=^USER^&password=^PASS^&Login=Login` — the form fields; `^USER^` and `^PASS^` are placeholders Hydra fills in with each attempt
  - `Login failed` — the string Hydra looks for to decide an attempt failed (if this text is missing from the response, Hydra assumes success)
- `-V` — verbose mode, shows every individual login attempt as it happens
- `-t 4` — run 4 parallel threads to speed up the attack

**Attack ran from:** Kali attacker VM (192.168.1.200)

**Result:**

![Hydra brute force attack output](Screenshots/dvwa-setup/hydra-attack.png)

Hydra reported 4 "valid" passwords found within 2 seconds. Manual 
verification by logging into DVWA directly through a browser showed 
these credentials did NOT actually work — Hydra's "Login failed" 
string match was misconfigured against DVWA's actual failure 
response, producing false positives rather than a real crack.

### Detection Gap Identified

Despite the brute-force traffic hitting DVWA's login page, Wazuh's 
Security Events for the DVWA agent (004) showed:
- 0 authentication failures
- 1 unrelated authentication success (from an earlier sudo session)
- No alerts logged at the actual attack timestamp (15:46)

![Wazuh Security Events dashboard showing no attack-related alerts](Screenshots/dvwa-setup/wazuh-siem.png)

![Security Alerts table showing only unrelated 15:05 setup events](Screenshots/dvwa-setup/security-alert-table.png)

**Root cause:** The Wazuh agent on the DVWA VM monitors OS-level 
logs (auth.log, sudo, PAM, CIS compliance) by default, but does not 
ingest Apache's web server logs. Web-application-layer attacks are 
therefore invisible to Wazuh out of the box.

**Next step:** Configure the Wazuh agent to forward Apache access/
error logs and add detection rules for repeated failed login POST 
requests to close this gap.