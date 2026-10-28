# Day 3: Content Discovery Techniques

Content discovery involves finding unlinked, hidden, or non-public information hosted on a web server. It uses three main methodologies: Manual Discovery, OSINT (Open Source Intelligence), and Automated Discovery.

---

## 1. Manual Content Discovery
Techniques performed directly via the browser or command line without brute-forcing tools:

* **`robots.txt`:** A text file instructing web crawlers (e.g., Googlebot) which directories/pages to index (`Allow`) or ignore (`Disallow`). Often exposes sensitive paths (e.g., `/admin`, `/backup`).
* **`sitemap.xml`:** An XML map provided to search engines listing all active site URLs. May expose hidden, unlinked, or newly added endpoints.
* **HTTP Response Headers:** Inspected via browser DevTools or `curl -I <URL>`. Reveals backend web server software, versions, framework info (e.g., `Server: Apache/2.4.41`, `X-Powered-By: PHP/7.4`).
* **Framework Documentation / Default Assets:** Identifying the framework (e.g., WordPress, Joomla) allows checking default file locations, admin login paths (`/wp-admin`), or release notes.

---

## 2. OSINT (Open Source Intelligence)
Gathering publicly accessible data about a target from third-party services:

### A. Google Dorking (Advanced Search Operators)
* `site:` Restricts search results strictly to a specific domain (e.g., `site:example.com`).
* `inurl:` Filters results to pages containing specified text inside the URL path.
* `intext:` Searches for specific words within the body text of a webpage.
* `intitle:` Filters results to pages containing specified terms inside the HTML `<title>` tag.
* `cache:` Displays Google’s cached snapshot of a page.
* `filetype:` Searches for specific file extensions (e.g., `filetype:pdf`, `filetype:env`, `filetype:log`).

### B. OSINT Tools & Platforms
* **Wappalyzer:** Browser extension that identifies web technologies, CMS frameworks, server software, and analytics tools running on a site.
* **Wayback Machine (archive.org):** Provides historical snapshots of web pages, useful for discovering old login portals, removed endpoints, or sensitive developer notes.
* **GitHub:** Public repositories and commit histories may accidentally leak source code, internal API endpoints, credentials, or old configuration files.
* **Amazon S3 Buckets:** Cloud storage locations (URLs usually follow `https://<name>.s3.amazonaws.com`). Misconfigured S3 buckets can expose public access to internal backup files or static site assets.

---

## 3. Automated Content Discovery (Gobuster)
Automated tools brute-force paths, subdomains, and virtual hosts using wordlists (e.g., SecLists).

### Gobuster Modes & Commands:
* **Directory Mode (`dir`):** Brute-forces hidden files and directories on a web server.
  * `gobuster dir -u http://<target-ip> -w /path/to/wordlist.txt`
* **DNS Mode (`dns`):** Brute-forces subdomains via public DNS lookups.
  * `gobuster dns -d target.com -w /path/to/wordlist.txt`
* **Virtual Host Mode (`vhost`):** Brute-forces subdomains hosted on the same IP by checking HTTP `Host` headers (requires `--append-domain`).
  * `gobuster vhost -u http://target.com -w /path/to/wordlist.txt --append-domain`
  * *Tip:* Use `--exclude-length` to hide false-positive responses (e.g., default 404 pages returning HTTP 200/302 with identical line lengths).   
<img width="1914" height="926" alt="day03" src="https://github.com/user-attachments/assets/9a751538-6c64-46f3-869a-78a634575108" />
