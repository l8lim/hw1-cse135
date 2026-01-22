# CSE 135 – HW1: Client Side Basics, Site and Server Configuration

**Course:** CSE 135  
**Quarter:** Winter 2026  
**Student:** Lianna Lim  
**Deployment Platform:** DigitalOcean (Ubuntu)

---

## Server Access Information

### Grader Account
- **Username:** grader  
- **Password:** <<<PUT GRADER PASSWORD HERE>>>

(Verified that the grader account can log in using SSH.)

---

## Site Links

- **Main site:** https://liannalim.online  
- **Homework directory:** https://liannalim.online/hw1/  
- **PHP test page:** https://liannalim.online/hw1/hello.php  
- **GoAccess report:** https://liannalim.online/hw1/report.html  

---

## Part 1 – Apache & Server Setup

- Created a DigitalOcean droplet running Ubuntu.
- Installed Apache and verified the default page.
- Configured virtual hosts for:
  - `liannalim.online`
  - `collector.liannalim.online`
  - `reporting.liannalim.online`
- Installed SSL certificates using Certbot.
- Verified HTTPS access for all configured domains.

---

## Part 2 – Website Content

- Created a team homepage linking to homework and member pages.
- Created a personal member page under `/members/`.
- All pages validate using the W3C HTML validator.
- Added a favicon and a minimal `robots.txt` file.
- Site content is deployed from GitHub using automatic deployment (push to main branch updates the live site).

---

## Part 3 – Web Server Configuration

### LAMP Stack Completion
- Installed MySQL and secured it using `mysql_secure_installation`.
- Installed PHP and verified correct operation using a test page with `phpinfo()`.

---

### PHP Verification
- Created `hw1/hello.php` containing a PHP test page.
- Verified that PHP is executing correctly on the server.

---

### Basic Authentication
- Enabled basic authentication to protect the main site.
- Authentication is required to access site content.
- Implemented using Apache `.htpasswd`.

**Credentials:**
- Username: grader  
- Password: Puppylove6!123

---

### Compression
- Enabled Apache `mod_deflate`.
- Configured compression for HTML, CSS, and JavaScript files.
- Verified in Chrome DevTools that responses include:
Content-Encoding: gzip

- File transfer sizes were reduced after compression was enabled.

---

### Server Header Obfuscation
- The default Apache `Server` header could not be fully overridden using `ServerTokens` alone.
- Implemented Nginx as a reverse proxy in front of Apache.
- Apache was moved to port 8080, and Nginx handles ports 80 and 443.
- Used the Nginx `headers-more` module to override the response header.

**Resulting header:**
Server: CSE135 Server


This prevents Apache and OS information from being exposed.

---

### Custom 404 Page
- Created a custom `404.html` page in the web root.
- Configured Apache with:
ErrorDocument 404 /404.html

- Verified that invalid URLs route to the custom error page.

---

### Access Logs & Analytics
- Apache access logs are located at:
/var/log/apache2/access.log

- Reviewed logs and observed requests for nonexistent paths and automated bot scans.
- Installed GoAccess and generated an HTML analytics report using:
goaccess /var/log/apache2/access.log
-o /var/www/liannalim.online/public_html/hw1/report.html
--log-format=COMBINED
