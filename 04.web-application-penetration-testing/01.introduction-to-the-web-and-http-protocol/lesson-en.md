# Introduction to Web App Security Testing

## What Are Web Applications?

A **web application** is a software application that runs on a web server and is accessed through a web browser.

Web applications provide dynamic and interactive functionality, allowing users to access information, submit data, authenticate, make purchases, manage accounts, and interact with online services.

Common examples include:

- Social media platforms such as Facebook and X.
- Webmail services such as Gmail and Outlook.
- E-commerce websites such as Amazon and eBay.
- Cloud productivity platforms such as Google Workspace and Microsoft 365.
- Online banking applications.
- Customer portals.
- Content-management systems.
- Web APIs and SaaS platforms.

---

## How Web Applications Work

Web applications generally use a **client-server architecture**.

```text
[ User / Browser ]
        |
        | HTTP or HTTPS request
        v
[ Web Server / Web Application ]
        |
        | Query or request
        v
[ Database / API / Internal Services ]
        |
        | Response
        v
[ Web Server / Web Application ]
        |
        | HTTP or HTTPS response
        v
[ User / Browser ]
```

### Main components

- **Client:** The user’s web browser, such as Firefox, Chrome, or Edge.
- **Web server:** Receives HTTP requests and returns responses.
- **Web application:** Contains the business logic of the application.
- **Database:** Stores information such as users, passwords, orders, and application data.
- **API:** Allows systems or applications to exchange data programmatically.

### User interface technologies

Web application interfaces are commonly built with:

- **HTML:** Defines the structure and content of a web page.
- **CSS:** Defines the appearance and layout of the page.
- **JavaScript:** Adds dynamic and interactive behavior in the browser.

---

## HTTP And Statelessness

Web browsers communicate with web servers mainly through the **HTTP** or **HTTPS** protocols.

HTTP is a **stateless** protocol. This means that every request is independent by default, and the server does not automatically remember previous requests.

Web applications use mechanisms such as cookies, session IDs, and tokens to maintain the user’s state.

### Example

```text
1. User logs in with a username and password.
2. The server validates the credentials.
3. The server creates a session or token.
4. The browser stores the session cookie or token.
5. The browser sends it with future requests.
6. The server recognizes the authenticated user.
```

If session handling is weak, attackers may attempt session hijacking, session fixation, or token theft.

---

## Web Application Security

Web application security focuses on protecting web applications from vulnerabilities, attacks, unauthorized access, data breaches, and service disruption.

The main objective is to preserve the **CIA triad**:

- **Confidentiality:** Sensitive information is accessible only to authorized users.
- **Integrity:** Data cannot be altered without authorization.
- **Availability:** The application remains accessible to legitimate users.

Web applications are attractive targets because they are often publicly accessible and can process valuable information, including:

- Usernames and passwords.
- Personal data.
- Payment details.
- Financial information.
- Internal documents.
- Business data.
- Intellectual property.
- Authentication tokens.

---

## Why Web App Security Matters

Web application security is important because a vulnerable application can cause:

- Exposure of sensitive data.
- Account takeover.
- Financial theft.
- Reputation damage.
- Regulatory penalties.
- Intellectual property theft.
- Service interruptions.
- Loss of customer trust.
- Website defacement.
- Data manipulation.

### Business impact

A successful attack against a web application may affect both the organization and its users.

For example, a SQL injection vulnerability may allow an attacker to access a database containing user accounts, email addresses, password hashes, or payment information.

---

## Web Application Security Practices

### Authentication And Authorization

Authentication verifies **who the user is**.

Authorization determines **what the authenticated user is allowed to do**.

```text
Authentication: “Who are you?”
Authorization: “What are you allowed to access?”
```

Examples of good practices:

- Enforce strong passwords.
- Use multi-factor authentication.
- Implement secure password-reset processes.
- Apply role-based access control.
- Verify authorization on every sensitive action.
- Prevent users from accessing other users’ resources.

---

### Input Validation And Output Encoding

Applications must validate all user-controlled input.

Potential sources of input include:

- URL parameters.
- Form fields.
- HTTP headers.
- Cookies.
- JSON requests.
- File uploads.
- API requests.

Input validation helps prevent attacks such as SQL injection and command injection.

Output encoding helps prevent attacks such as cross-site scripting.

---

### Secure Communication

Applications should use HTTPS to encrypt traffic between the browser and the server.

```text
HTTP   = unencrypted communication.
HTTPS  = encrypted HTTP communication using TLS.
```

HTTPS protects sensitive data in transit, including:

- Login credentials.
- Session cookies.
- Payment information.
- Personal information.
- API tokens.

---

### Secure Coding

Secure coding practices reduce the chance of introducing vulnerabilities during development.

Important practices include:

- Validate all input.
- Use parameterized database queries.
- Avoid dangerous functions when possible.
- Implement secure error handling.
- Do not expose stack traces to users.
- Protect secrets and API keys.
- Use secure defaults.
- Review code before deployment.

---

### Regular Updates

Web applications depend on many components:

- Web servers.
- Frameworks.
- Libraries.
- CMS platforms.
- Plugins.
- Databases.
- Operating systems.

All components should be updated regularly because outdated software may contain known vulnerabilities.

---

### Least Privilege

The **principle of least privilege** means giving users, applications, services, and processes only the permissions they need.

Examples:

- A database account should not have administrator privileges unless required.
- A normal user should not access administrative pages.
- A web application should not run as root or Administrator.
- API tokens should have limited permissions and expiration dates.

---

### Web Application Firewall

A **Web Application Firewall**, or WAF, monitors and filters HTTP traffic between users and a web application.

A WAF can help detect or block:

- Common SQL injection attempts.
- Cross-site scripting payloads.
- Malicious bots.
- Known exploit patterns.
- Excessive requests.
- Suspicious HTTP requests.

A WAF is useful, but it does not replace secure development and security testing.

---

### Session Management

Secure session handling helps prevent attackers from hijacking authenticated sessions.

Important session-security practices include:

- Use long, random session identifiers.
- Set secure cookie attributes.
- Regenerate session IDs after login.
- Expire sessions after inactivity.
- Invalidate sessions after logout.
- Use HTTPS for all authenticated pages.
- Avoid exposing session tokens in URLs.

---

# Web Application Security Testing

## What Is Web Application Security Testing?

Web application security testing is the process of evaluating a web application to identify vulnerabilities, weaknesses, misconfigurations, and security risks.

The main goal is to identify and fix security flaws before malicious attackers can exploit them.

Security testing can assess:

- Authentication.
- Authorization.
- Input validation.
- Session management.
- API security.
- File upload functionality.
- Server configuration.
- Third-party components.
- Business logic.
- Data protection.

---

## Types Of Security Testing

### Vulnerability Scanning

Vulnerability scanning uses automated tools to identify known weaknesses.

A scanner may detect:

- Outdated software.
- Missing security headers.
- Weak TLS configuration.
- Common SQL injection indicators.
- Common XSS indicators.
- Exposed directories.
- Security misconfigurations.

Automated scanning is useful for broad coverage, but results must be manually verified because scanners can produce false positives and false negatives.

---

### Penetration Testing

Web application penetration testing simulates real-world attacks in a controlled and authorized manner.

A pentester attempts to:

- Identify vulnerabilities.
- Validate whether they can be exploited.
- Determine the impact of successful exploitation.
- Assess existing security controls.
- Identify paths to sensitive data or privileged functions.

Pentesting can involve controlled exploitation, but it must always remain within the agreed scope and rules of engagement.

---

### Code Review And Static Analysis

Code review examines the application’s source code to find security issues before deployment.

Static Application Security Testing, or **SAST**, analyzes source code without executing the application.

Common issues found through code review include:

- Hardcoded credentials.
- Unsafe database queries.
- Insecure cryptography.
- Missing input validation.
- Insecure file handling.
- Dangerous function calls.
- Weak authorization checks.

---

### Dynamic Application Security Testing

Dynamic Application Security Testing, or **DAST**, tests a running application from the outside.

DAST tools interact with the application through HTTP requests and responses.

They may identify:

- Reflected XSS.
- SQL injection indicators.
- Missing security headers.
- Insecure cookies.
- Exposed server information.
- Weak HTTP methods.
- Insecure redirects.

---

### Interactive Application Security Testing

Interactive Application Security Testing, or **IAST**, analyzes an application while it is running.

IAST combines elements of static and dynamic testing by monitoring how the application behaves during tests.

---

### Software Composition Analysis

Software Composition Analysis, or **SCA**, identifies third-party libraries, dependencies, and components that contain known vulnerabilities.

This is important because modern web applications often rely heavily on open-source packages and external libraries.

---

## Authentication And Authorization Testing

Authentication testing evaluates whether users can securely prove their identity.

Authorization testing evaluates whether users can access only the functions and data they are permitted to use.

Important checks include:

- Weak password policies.
- Default credentials.
- Missing multi-factor authentication.
- Insecure password-reset processes.
- User enumeration.
- Brute-force protection.
- Privilege escalation.
- Insecure direct object references.
- Broken access control.
- Missing role validation.

---

## Input Validation And Output Encoding Testing

This testing evaluates how the application handles data provided by users.

The main goal is to identify whether user input can change the application’s intended behavior.

Common vulnerabilities include:

- SQL injection.
- Cross-site scripting.
- Command injection.
- Server-side template injection.
- Path traversal.
- XML external entity injection.
- File inclusion.
- Insecure deserialization.

---

## Session Management Testing

Session management testing evaluates how the application handles authenticated users and session tokens.

Common issues include:

- Predictable session IDs.
- Session fixation.
- Session hijacking.
- Tokens exposed in URLs.
- Missing cookie flags.
- Sessions that do not expire.
- Sessions remaining active after logout.
- Reusable password-reset tokens.

Useful cookie attributes include:

```text
Secure     = the cookie is sent only through HTTPS.
HttpOnly   = JavaScript cannot access the cookie.
SameSite   = helps reduce cross-site request attacks.
```

---

## API Security Testing

APIs allow web applications, mobile applications, and services to exchange data.

API security testing should assess:

- Authentication mechanisms.
- Authorization checks.
- Token validation.
- Rate limiting.
- Input validation.
- Sensitive data exposure.
- Excessive data exposure.
- Insecure endpoints.
- API versioning.
- Error messages.
- Logging and monitoring.

---

# Web App Pentesting vs Security Testing

| Aspect | Web Application Security Testing | Web Application Penetration Testing |
|---|---|---|
| Objective | Identify vulnerabilities and weaknesses | Validate vulnerabilities through controlled exploitation |
| Scope | Broad: code, configuration, infrastructure, dependencies, and runtime behavior | Focused on discovering and exploiting realistic attack paths |
| Methodology | Manual and automated testing | Mainly manual testing supported by tools |
| Exploitation | Usually does not exploit vulnerabilities | Uses controlled exploitation when authorized |
| Impact | Generally non-intrusive | Can be intrusive and may affect application availability |
| Reporting | Identifies issues and remediation recommendations | Documents successful exploitation, impact, and remediation |
| Main goal | Improve the overall security posture | Validate defenses and response capabilities |

---

# Threats And Risks

## Threat vs Risk

A **threat** is a potential source of harm that may exploit a vulnerability.

Examples of threats:

- Cybercriminals.
- Malicious insiders.
- Phishing campaigns.
- Malware.
- Denial-of-service attacks.
- Natural disasters.
- Power outages.

A **risk** is the potential loss or harm that may occur if a threat successfully exploits a vulnerability.

Risk is often evaluated using:

```text
Risk = Likelihood × Impact
```

- **Likelihood:** How likely the event is to happen.
- **Impact:** How serious the consequences would be.

A threat may exist without creating a high risk if strong security controls reduce the likelihood or impact.

---

## Common Web Application Threats

### Cross-Site Scripting

Cross-Site Scripting, or **XSS**, happens when attackers inject malicious JavaScript into a web page viewed by other users.

Possible impact:

- Session-cookie theft.
- Account takeover.
- Browser manipulation.
- Credential theft.
- Content modification.
- Malicious redirects.

Main XSS types:

- Reflected XSS.
- Stored XSS.
- DOM-based XSS.

---

### SQL Injection

SQL injection, or **SQLi**, occurs when attackers manipulate application input to inject malicious SQL queries into a database.

Possible impact:

- Unauthorized access to data.
- Data leakage.
- Data modification.
- Authentication bypass.
- Database compromise.
- Deletion of database records.

Main prevention methods:

- Parameterized queries.
- Prepared statements.
- Input validation.
- Least-privilege database accounts.
- Secure error handling.

---

### Cross-Site Request Forgery

Cross-Site Request Forgery, or **CSRF**, tricks an authenticated user into performing an unintended action through their active session.

Possible impact:

- Changing account details.
- Changing email addresses.
- Initiating transactions.
- Modifying passwords.
- Performing privileged actions.

Common protections include:

- Anti-CSRF tokens.
- SameSite cookies.
- Re-authentication for sensitive actions.
- Origin and Referer validation.

---

### Security Misconfiguration

Security misconfiguration occurs when servers, applications, databases, cloud services, or frameworks are configured insecurely.

Examples include:

- Default credentials.
- Debug mode enabled.
- Directory listing enabled.
- Exposed administrative panels.
- Unnecessary services.
- Default error pages.
- Verbose error messages.
- Missing security headers.
- Excessive permissions.

---

### Sensitive Data Exposure

Sensitive data exposure happens when confidential information is not adequately protected.

Examples include:

- Passwords stored in plain text.
- Weak encryption.
- Unencrypted HTTP traffic.
- Sensitive data in logs.
- Exposed backups.
- API responses returning excessive user data.
- Credentials committed to source-code repositories.

---

### Brute Force And Credential Stuffing

A brute-force attack tries many possible passwords against an account.

Credential stuffing uses previously leaked username-password combinations against other services, relying on password reuse.

Defenses include:

- Multi-factor authentication.
- Rate limiting.
- Account lockout policies.
- CAPTCHA when appropriate.
- Monitoring failed logins.
- Password managers.
- Detection of unusual login activity.

---

### File Upload Vulnerabilities

Insecure file uploads can allow attackers to upload malicious or dangerous files.

Possible impact:

- Remote code execution.
- Malware distribution.
- Server compromise.
- Stored XSS.
- Path traversal.
- Denial of service.

Secure file-upload practices include:

- Validate file type and content.
- Rename uploaded files.
- Store uploads outside the web root.
- Restrict execution permissions.
- Enforce file-size limits.
- Scan uploaded files.
- Use allowlists rather than blocklists.

---

### Denial Of Service And DDoS

A Denial of Service, or **DoS**, attack attempts to make an application unavailable by exhausting resources.

A Distributed Denial of Service, or **DDoS**, attack uses many systems to overwhelm the target.

Possible impact:

- Application downtime.
- Lost revenue.
- Productivity loss.
- Reputational damage.
- Service disruption.

Defenses include:

- Rate limiting.
- Content delivery networks.
- Load balancing.
- DDoS protection services.
- Monitoring.
- Capacity planning.

---

### Server-Side Request Forgery

Server-Side Request Forgery, or **SSRF**, allows an attacker to make the server send requests to internal or external resources.

Possible impact:

- Access to internal services.
- Exposure of cloud metadata.
- Internal network scanning.
- Data theft.
- Bypass of network restrictions.

Defenses include:

- Strict allowlists for outbound requests.
- Block access to private IP ranges when not required.
- Validate URLs and protocols.
- Segment internal networks.
- Restrict the web server’s network permissions.

---

### Broken Access Control

Broken access control occurs when users can access functions or data outside their authorized permissions.

Examples include:

- Accessing another user’s profile by changing an ID in a URL.
- Viewing administrative pages as a normal user.
- Downloading documents belonging to another account.
- Accessing APIs without authorization checks.
- Modifying objects owned by other users.

This is one of the most important areas to test during web application pentesting.

---

### Vulnerable Components

Using components with known vulnerabilities introduces risk into the web application.

Affected components may include:

- Frameworks.
- CMS platforms.
- Plugins.
- JavaScript libraries.
- Server software.
- API dependencies.
- Container images.

Mitigation includes maintaining an inventory of dependencies, monitoring security advisories, and applying patches quickly.

---

# Web Application Pentesting Methodology

## 1. Define Scope And Rules

Before testing, confirm:

- Target domains and IP addresses.
- Allowed test accounts.
- Test dates and time windows.
- Allowed testing techniques.
- Out-of-scope systems.
- Sensitive functions that must not be disrupted.
- Data-handling requirements.
- Emergency contacts.
- Reporting requirements.

---

## 2. Information Gathering

Collect information about the application before testing.

Identify:

- Domains and subdomains.
- Web technologies.
- Frameworks.
- Server headers.
- Directories and endpoints.
- Login pages.
- APIs.
- JavaScript files.
- Parameters.
- Forms.
- Cookies.
- User roles.
- Third-party integrations.

---

## 3. Attack Surface Mapping

Map all accessible functionality.

Focus on:

- Authentication pages.
- Registration pages.
- Password reset functions.
- Account settings.
- File uploads.
- Search forms.
- Shopping carts.
- Administrative functions.
- API endpoints.
- Payment flows.
- User-generated content.
- HTTP methods.
- Hidden parameters.

---

## 4. Vulnerability Testing

Test the identified attack surface for common weaknesses.

Typical categories include:

- Authentication flaws.
- Authorization flaws.
- Session-management issues.
- Input-validation issues.
- File-upload issues.
- API weaknesses.
- Security misconfigurations.
- Business-logic flaws.
- Sensitive-data exposure.
- Vulnerable components.

---

## 5. Controlled Exploitation

When allowed by the rules of engagement, validate important vulnerabilities through controlled exploitation.

The objective is to demonstrate impact while minimizing risk.

Examples of impact validation include:

- Confirming access to another test user’s data.
- Demonstrating a limited authorization bypass.
- Proving that sensitive data is exposed.
- Showing that an upload restriction can be bypassed in a safe lab.
- Confirming a vulnerable component version.

Avoid destructive actions unless specifically authorized.

---

## 6. Reporting

A good web application pentest report should include:

- Executive summary.
- Scope and methodology.
- Identified vulnerabilities.
- Severity and business impact.
- Evidence of findings.
- Affected URLs, endpoints, or components.
- Reproduction steps.
- Remediation recommendations.
- Risk rating.
- Retesting results, if available.

---

## 7. Remediation And Retesting

After vulnerabilities are fixed, retest the application to verify that:

- The issue is resolved.
- The fix does not introduce new vulnerabilities.
- Security controls work as expected.
- The original proof of concept no longer works.

---

# Key Takeaways

- Web applications are client-server applications accessed through web browsers.
- Web application security protects confidentiality, integrity, and availability.
- HTTP is stateless, so applications must manage sessions securely.
- Security testing identifies weaknesses, while pentesting validates vulnerabilities through controlled exploitation.
- Common threats include XSS, SQLi, CSRF, SSRF, broken access control, file upload flaws, insecure configurations, and DDoS.
- Secure coding, strong authentication, input validation, secure sessions, patching, least privilege, and continuous testing are essential.
- Web application security is an ongoing process, not a one-time activity.
