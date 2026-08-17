# Website Crawling & Spidering

## Overview

**Website crawling** and **spidering** are techniques used to discover and map the accessible content of a web application.

The objective is to identify the application’s visible attack surface, including:

- Pages and directories.
- Files and static resources.
- URL paths.
- Parameters.
- Forms.
- API endpoints.
- JavaScript files.
- Login pages.
- Administrative panels.
- Upload features.
- User roles and workflows.

Crawling is an important early stage of web application security testing because you cannot properly test an application until you understand what functionality exists.

### Important rule

Only crawl websites and applications that are explicitly within the authorized scope.

Define the scope before starting:

- Target domain names.
- Subdomains.
- IP addresses.
- Ports.
- Allowed user accounts.
- Excluded functionality.
- Crawl speed and limits.
- Authentication requirements.

---

## Crawling vs Spidering

The terms are often used interchangeably, but they can be understood as follows:

| Term | Meaning |
|---|---|
| Crawling | Discovering content by navigating pages, following links, processing forms, and observing requests |
| Spidering | Automatically following discovered links and URLs to identify additional content |
| Passive crawling | Mapping content from traffic and references without sending intrusive attack payloads |
| Active crawling | Automatically requesting discovered pages and resources; it may generate more traffic |
| Manual crawling | A tester manually navigates the application while a proxy records the requests |
| AJAX spidering | Crawling JavaScript-heavy applications by rendering pages and interacting with dynamic elements |

---

## Why Crawling Matters

Crawling helps build a map of the target application.

Without a proper map, it is easy to miss:

- Hidden directories.
- Unlinked pages.
- API endpoints.
- Parameters.
- Administrative functions.
- JavaScript routes.
- Authentication workflows.
- User-specific functionality.
- File uploads.
- Password-reset functions.
- Sensitive resources.

### Example attack surface map

```text
https://target.local/
|
+-- /
+-- /login
+-- /register
+-- /forgot-password
+-- /dashboard
+-- /profile
+-- /admin
+-- /uploads
+-- /api/
|   +-- /api/users
|   +-- /api/orders
|   +-- /api/profile
|
+-- /static/
    +-- /static/js/app.js
    +-- /static/css/style.css
```

---

# Passive Crawling With Burp Suite

## What Is Passive Crawling?

Passive crawling in Burp Suite maps the visible application content as your browser traffic passes through Burp Proxy.

It does not require active exploitation. Burp observes:

- URLs visited in the browser.
- Links found in responses.
- Forms.
- Scripts.
- Referenced resources.
- Parameters.
- Cookies.
- HTTP methods.
- Request and response headers.

The discovered content is added to the **Target > Site map**.

---

## Burp Suite Setup

### 1. Configure the scope

Before browsing the target, add the authorized target to Burp’s scope.

```text
Target > Scope > Add
```

Add the target domain, URL, IP address, or port.

Example:

```text
https://target.local
https://*.target.local
```

### Why scope matters

Scope helps you:

- Avoid sending traffic to unrelated websites.
- Keep the Site map organized.
- Focus passive crawling on the target.
- Prevent accidental testing of out-of-scope systems.

---

## 2. Use Burp Browser

Burp Suite includes a built-in browser that is already configured to use Burp Proxy.

```text
Proxy > Intercept > Open Browser
```

For passive crawling, it is usually easier to turn interception off while browsing.

```text
Proxy > Intercept > Intercept is off
```

This allows requests to flow through Burp without stopping every request.

---

## 3. Browse the Application Manually

Use the Burp browser to explore the web application as a normal user.

Navigate through:

- The home page.
- Login and logout pages.
- Registration pages.
- Password-reset pages.
- User profile pages.
- Search forms.
- Navigation menus.
- Settings pages.
- File-upload forms.
- Download links.
- Administrative pages, if authorized.
- APIs called by the application.
- User-generated content pages.

### Manual crawling workflow

```text
1. Open the target in Burp Browser.
2. Navigate through every visible menu.
3. Click all relevant links.
4. Submit non-destructive forms.
5. Log in with authorized test accounts.
6. Repeat the process with other user roles.
7. Review the Site map.
```

Burp records the traffic and populates the Site map with visited content. Burp can also infer additional locations from links and forms present in responses. [122][126]

---

## 4. Review Target Site Map

Open:

```text
Target > Site map
```

The Site map displays the discovered application structure.

Look for:

- Interesting directories.
- API endpoints.
- JavaScript files.
- Parameters.
- HTTP methods.
- Response codes.
- Cookies.
- Redirects.
- Hidden or greyed-out paths.
- Administrative endpoints.
- File upload locations.
- Different content for different user roles.

### Useful Site Map items

```text
/login
/logout
/register
/admin
/api
/api/v1/users
/uploads
/download
/profile?id=10
/search?q=test
/robots.txt
/sitemap.xml
```

Greyed-out entries in the Site map may be paths inferred from responses but not yet requested. You can manually open them in Burp Browser if they are within scope. [122]

---

## 5. Filter The Site Map

Use Site map filters to reduce noise.

Useful filters include:

- Show only in-scope items.
- Hide images.
- Hide CSS files.
- Hide fonts.
- Hide static JavaScript libraries.
- Show only dynamic content.
- Show only requested items.
- Show specific HTTP status codes.

This helps you focus on endpoints that may contain application functionality.

---

## Burp Suite Passive Crawl Checklist

- Add the target to scope.
- Open Burp Browser.
- Turn interception off during normal browsing.
- Browse every visible page.
- Follow links and menus.
- Submit safe forms.
- Log in with authorized test accounts.
- Test multiple user roles when available.
- Review `Target > Site map`.
- Identify API endpoints and parameters.
- Inspect JavaScript files.
- Review hidden or inferred endpoints.
- Document all relevant functionality.

---

# Burp Suite Professional Crawling

Burp Suite Professional includes automated crawling functionality.

### Start an automated crawl

```text
Target > Site map
Right-click the target root
Scan
Select Crawl
Start Scan
```

You can configure application login details if authentication is required.

### Important note

Burp Suite Community Edition does not include the same automated crawling capabilities as Burp Suite Professional.

With Burp Community Edition, use manual browsing through Burp Proxy to populate the Site map.

---

# Passive Crawling With OWASP ZAP

## What Is OWASP ZAP?

OWASP ZAP, also known as **Zed Attack Proxy**, is an open-source web application security testing tool.

ZAP can help with:

- Intercepting HTTP/S traffic.
- Passive scanning.
- Traditional spidering.
- AJAX spidering.
- Manual exploration.
- Site mapping.
- Header inspection.
- Cookie analysis.
- API discovery.

---

## Passive Scanning In ZAP

ZAP’s passive scanner analyzes HTTP and WebSocket messages that pass through ZAP without modifying them. [123][128]

It may identify issues such as:

- Missing security headers.
- Cookies without security attributes.
- Information disclosure.
- Sensitive data in URLs.
- Server banner exposure.
- Weak cache-control settings.
- Missing anti-clickjacking controls.
- CORS configuration issues.

### Important distinction

```text
Passive scan = analyzes existing traffic without sending attack payloads.
Active scan  = sends attack payloads and may affect the target.
```

Do not run active scans unless they are explicitly authorized. ZAP documents active scanning as an attack on selected targets. [125]

---

## ZAP Setup

### 1. Create a session

Start OWASP ZAP and create or open a session.

```text
File > New Session
```

A session stores:

- Discovered URLs.
- HTTP history.
- Alerts.
- Spider results.
- Context configuration.
- Authentication settings.

---

## 2. Define Context And Scope

Create a context for the target application.

```text
Sites > Right-click target
Include in Context > New Context
```

Add the target domain or URL.

Example:

```text
https://target.local
https://.*\.target\.local.*
```

Then define the target as in scope:

```text
Sites > Right-click target
Include in Scope
```

### Why use a context?

A ZAP context helps define:

- In-scope URLs.
- Authentication rules.
- Session-management rules.
- User accounts.
- Excluded paths.
- Spider restrictions.

---

## 3. Use Manual Explore

Use the built-in browser or configure your own browser to proxy traffic through ZAP.

```text
Quick Start > Manual Explore
```

Enter the target URL and launch the browser.

Browse the application normally:

- Open pages.
- Follow navigation links.
- Submit harmless forms.
- Log in with authorized accounts.
- Explore profile and settings pages.
- Use available user roles.
- Trigger API calls.

ZAP records the requests in:

```text
History
Sites
```

The application structure will be populated in the **Sites** tree.

---

# Traditional Spider In OWASP ZAP

## What Is The Traditional Spider?

The traditional ZAP Spider automatically requests pages and analyzes returned HTML to discover:

- Links.
- Forms.
- Resources.
- Parameters.
- Referenced paths.

It is usually fast and works well for traditional HTML websites.

### Start The Traditional Spider

```text
Sites > Right-click the target
Attack > Spider
```

Or use:

```text
Tools > Spider
```

Configure:

- Starting URL.
- Context.
- Maximum depth.
- Maximum number of children.
- Thread count.
- Excluded URLs.
- Recurse settings.

Then start the spider.

### Monitor results

Review results in:

```text
Spider tab
Sites tree
History tab
```

---

## Traditional Spider Limitations

The traditional spider mainly analyzes HTML returned by the server.

It may have difficulty discovering content in applications that rely heavily on:

- JavaScript.
- Single-page application frameworks.
- Dynamic DOM changes.
- Client-side routing.
- AJAX requests.
- Buttons that do not use standard links.

For JavaScript-heavy applications, use the AJAX Spider in addition to the traditional Spider.

---

# AJAX Spider In OWASP ZAP

## What Is The AJAX Spider?

The **AJAX Spider** is designed for modern web applications that rely heavily on JavaScript, AJAX, dynamic elements, and client-side rendering.

It uses the Crawljax crawler to render pages and interact with AJAX-rich applications. [124][127]

The AJAX Spider can discover pages and states that a traditional HTML spider might miss.

### When To Use It

Use the AJAX Spider when the application uses:

- React.
- Angular.
- Vue.js.
- Single-page applications.
- Dynamic menus.
- JavaScript buttons.
- AJAX requests.
- Client-side routing.
- Content loaded after the initial page response.

### Start The AJAX Spider

```text
Sites > Right-click the target
Attack > AJAX Spider
```

Or:

```text
Tools > AJAX Spider
```

Configure the target URL and context, then start the crawl.

### Important note

The AJAX Spider is slower than the traditional Spider because it renders pages, executes JavaScript, and interacts with dynamic content.

For broader coverage, use both:

```text
1. Manual browsing.
2. Traditional Spider.
3. AJAX Spider.
4. Passive scanning.
```

ZAP recommends using the native Spider as well as the AJAX Spider because they identify different types of content. [127]

---

# ZAP Passive Crawl Checklist

- Create a new ZAP session.
- Define the target context.
- Add the target to scope.
- Use Manual Explore.
- Browse the application with authorized accounts.
- Review the Sites tree.
- Run the Traditional Spider.
- Run the AJAX Spider for JavaScript-heavy applications.
- Wait for passive scanning to complete.
- Review passive-scan alerts.
- Export or document discovered endpoints.
- Do not run active scans without authorization.

---

# Burp Suite vs OWASP ZAP

| Feature | Burp Suite | OWASP ZAP |
|---|---|---|
| License | Community Edition is free; Professional Edition is commercial | Open source and free |
| Passive crawling | Uses Proxy traffic and Site map | Uses proxied traffic, Sites tree, and passive scanner |
| Manual browsing | Burp Browser and Proxy | Manual Explore and browser proxy |
| Automated crawl | Available in Burp Suite Professional | Traditional Spider and AJAX Spider available |
| JavaScript crawling | Available through Burp Professional crawler features | AJAX Spider is available |
| Passive scanning | Basic passive analysis and Site map | Built-in passive scanner and passive rules |
| Active scanning | Professional feature | Available but must be explicitly authorized |
| Main mapping view | Target > Site map | Sites tree and History |

---

# Useful Discovery Targets

During passive crawling and spidering, look for:

```text
/login
/logout
/register
/forgot-password
/reset-password
/profile
/settings
/admin
/dashboard
/api
/api/v1/
/uploads
/downloads
/backups
/config
/robots.txt
/sitemap.xml
/.git
/.env
```

Also identify:

- URL parameters.
- Form fields.
- Cookies.
- Session tokens.
- API requests.
- JavaScript endpoints.
- User roles.
- HTTP methods.
- Redirects.
- File uploads.
- Error pages.
- Third-party integrations.

---

# Practical Workflow

## Burp Suite Community Edition

```text
1. Open Burp Suite.
2. Add the target to Target > Scope.
3. Open Burp Browser.
4. Disable interception for normal browsing.
5. Browse all accessible functionality.
6. Log in with authorized accounts.
7. Review Target > Site map.
8. Investigate interesting paths and parameters.
9. Inspect Proxy > HTTP history.
10. Document discovered endpoints.
```

## OWASP ZAP

```text
1. Open OWASP ZAP.
2. Create a new session.
3. Create a context for the target.
4. Add the target to scope.
5. Start Manual Explore.
6. Browse the application normally.
7. Review the Sites tree and History.
8. Run the Traditional Spider.
9. Run the AJAX Spider if the application uses JavaScript heavily.
10. Wait for passive scan results.
11. Review alerts and document findings.
```

---

# Key Takeaways

- Crawling and spidering help map a web application’s visible attack surface.
- Passive crawling observes traffic and response content without using attack payloads.
- Burp Suite populates `Target > Site map` as traffic passes through Burp Proxy.
- Burp Suite Community Edition relies mainly on manual browsing for crawling.
- OWASP ZAP provides Manual Explore, a Traditional Spider, an AJAX Spider, and passive scanning.
- Traditional spiders work well for classic HTML applications.
- AJAX spiders are useful for JavaScript-heavy and single-page applications.
- Passive scanning can identify security issues without actively attacking the target.
- Always define scope, use authorized accounts, and avoid active scanning unless it is explicitly allowed.
