# HTTP/S Protocol Fundamentals

## HTTP Protocol

**HTTP** stands for **Hypertext Transfer Protocol**. It is an application-layer protocol used to transfer web resources, such as HTML pages, images, JavaScript files, CSS files, API data, and web application content.

HTTP runs on top of **TCP** and was designed for communication between web browsers and web servers.

HTTP uses a client-server architecture:

- The **client** is usually a web browser, mobile application, script, or command-line tool.
- The **server** is the web server that receives requests and returns responses.

Resources are uniquely identified through a **URL** or **URI**.

```text
Client / Browser                         Web Server
       |                                      |
       | -------- HTTP Request -------------> |
       |                                      |
       | <------- HTTP Response ------------- |
       |                                      |
```

---

## Website And Web Server

### What Is A Website?

A website is a collection of interconnected web pages accessible over the internet.

It may contain:

- Text.
- Images.
- Videos.
- Forms.
- Links.
- Downloadable files.
- Interactive content.

### What Is A Web Server?

A web server is software or hardware that receives HTTP or HTTPS requests and sends web resources back to clients.

Common web-server software includes:

```text
Apache HTTP Server
Nginx
Microsoft IIS
```

### Types Of Servers

| Server Type | Function |
|---|---|
| HTTP/Web Server | Handles HTTP requests and serves static content such as HTML, CSS, JavaScript, images, and files |
| Application Server | Runs application code, processes data, manages user interactions, and generates dynamic content |
| Database Server | Stores and manages data used by web applications, such as users, products, sessions, and orders |

### Off-Premise Hosting

**Off-premise hosting**, also called cloud hosting, means that a website or application is hosted on remote infrastructure rather than on the organization’s local servers.

Examples include cloud platforms, virtual private servers, and managed hosting providers.

---

## HTTP Versions

### HTTP/1.0

HTTP/1.0 is an early version of HTTP.

It allows clients to request resources from a web server, but it usually requires a new TCP connection for each request, making it inefficient for modern web applications.

### HTTP/1.1

HTTP/1.1 is the most commonly encountered version in classic web traffic.

It improves on HTTP/1.0 by supporting persistent connections.

```text
HTTP/1.0  = usually creates a new connection for each request.
HTTP/1.1  = can reuse the same connection for multiple requests.
```

HTTP/1.1 uses `Connection: keep-alive` to keep a TCP connection open and send several requests through it.

### HTTP/2

HTTP/2 improves application performance by allowing multiple requests and responses to be sent through one connection at the same time.

Important improvements include:

- Multiplexing.
- Header compression.
- Reduced latency.
- More efficient resource loading.

### HTTP/3

HTTP/3 is designed to improve performance further by using the **QUIC** transport protocol.

It aims to reduce latency and improve connection setup times, especially on unreliable networks.

---

## HTTP Is Stateless

HTTP is a **stateless** protocol.

This means that every request is independent by default. The server does not automatically remember previous requests from the same user.

Web applications use sessions, cookies, and tokens to maintain user state.

```text
1. User logs in.
2. Server validates credentials.
3. Server creates a session or token.
4. Browser stores a session cookie or token.
5. Browser sends it with future requests.
6. Server recognizes the user as authenticated.
```

---

## HTTP Message Structure

During HTTP communication, the client and server exchange messages.

The client sends an **HTTP request**, and the server sends an **HTTP response**.

```text
[ Browser / Client ] ---- HTTP Request ----> [ Web Server ]
[ Browser / Client ] <--- HTTP Response ---- [ Web Server ]
```

HTTP messages use:

```text
\r     = Carriage Return; moves the cursor to the start of the line.
\n     = Line Feed; moves the cursor to the next line.
\r\n   = Carriage Return + Line Feed; marks the end of a line.
```

A blank line, represented by `\r\n\r\n`, separates HTTP headers from the optional message body.

---

# HTTP Requests

## HTTP Request Components

An HTTP request normally contains:

1. Request line.
2. Request headers.
3. Empty line.
4. Optional request body.

### Request Structure

```http
METHOD /path HTTP/version
Header-Name: Header-Value
Header-Name: Header-Value

Optional request body
```

---

## Request Line

The request line is the first line of an HTTP request.

It contains:

- The HTTP method.
- The requested URL path.
- The HTTP version.

### Example

```http
GET / HTTP/1.1
```

```text
GET       = HTTP method.
 /        = requested path; the root page.
HTTP/1.1  = HTTP protocol version.
```

---

## Request Path

The request path indicates the resource the client wants to access.

```http
GET / HTTP/1.1
```

The `/` path represents the home page or root directory of the website.

Other examples:

```http
GET /login HTTP/1.1
GET /downloads/index.php HTTP/1.1
GET /images/logo.png HTTP/1.1
GET /api/users/10 HTTP/1.1
```

The host header and path are combined to form the full URL.

```text
Host: example.com
Path: /login

Full URL: http://example.com/login
```

---

## HTTP Request Headers

Headers provide additional information about the HTTP request.

Their basic format is:

```text
Header-Name: Header-Value
```

Common request headers include:

| Header | Purpose |
|---|---|
| Host | Specifies the hostname of the server being requested |
| User-Agent | Identifies the client, browser, operating system, and sometimes language |
| Accept | Defines the content types the client can accept |
| Accept-Encoding | Defines acceptable compression formats, such as gzip or deflate |
| Authorization | Sends authentication credentials or tokens |
| Cookie | Sends client-side stored data, usually session identifiers |
| Referer | Indicates the page that linked to the requested resource |
| Origin | Indicates the origin of the request, especially important for CORS |
| Content-Type | Specifies the format of data sent in the request body |
| Content-Length | Specifies the size of the request body in bytes |
| Connection | Controls whether the connection is kept open or closed |

---

## Host Header

The `Host` header specifies which website the client wants to access.

```http
Host: www.example.com
```

This header allows one web server with one IP address to host multiple websites. This configuration is known as **virtual hosting**.

```text
IP Address: 192.168.1.10

Host: website-a.com
Host: website-b.com
Host: website-c.com
```

The server uses the `Host` header to determine which website configuration or virtual host should handle the request.

---

## User-Agent Header

The `User-Agent` header identifies the client making the request.

```http
User-Agent: Mozilla/5.0 (X11; Linux x86_64) Firefox/120.0
```

It can reveal information such as:

- Browser type.
- Browser version.
- Operating system.
- Device type.
- Language or platform information.

Web servers may use this information to return browser-specific content, but it can also expose unnecessary information about the user.

---

## Accept Header

The `Accept` header specifies the content types that the client is willing to receive.

```http
Accept: text/html,application/xhtml+xml,application/json
```

Common content types include:

```text
text/html          = HTML web page.
application/json   = JSON API data.
application/xml    = XML data.
text/plain         = plain text.
image/png          = PNG image.
image/jpeg         = JPEG image.
```

---

## Accept-Encoding Header

The `Accept-Encoding` header specifies which content encodings or compression algorithms the client supports.

```http
Accept-Encoding: gzip, deflate, br
```

Common values:

```text
gzip    = common compression format.
deflate = compression format.
br      = Brotli compression.
```

Compression reduces response size and improves performance.

---

## Connection Header

The `Connection` header controls how the network connection is handled.

```http
Connection: keep-alive
```

With HTTP/1.1, `keep-alive` allows the browser to reuse the same TCP connection for several requests.

```text
Connection: close       = close the connection after the response.
Connection: keep-alive  = keep the connection open for future requests.
```

---

## Authorization Header

The `Authorization` header sends credentials or authentication tokens.

Example of HTTP Basic Authentication:

```http
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

The value after `Basic` is Base64-encoded and should not be treated as encryption.

Example of a bearer token:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Security testing should verify that tokens:

- Are not exposed in URLs.
- Expire correctly.
- Cannot be reused after logout.
- Are validated by the server.
- Are protected by HTTPS.

---

## Cookie Header

The `Cookie` header sends browser-stored cookies to the server.

```http
Cookie: session=abc123; theme=dark
```

Cookies are commonly used for:

- Session management.
- Authentication.
- User preferences.
- Language selection.
- Shopping carts.
- Tracking.

### Important Security Note

Session cookies are sensitive because they may allow access to authenticated accounts.

---

## Request Body

Some HTTP methods include a request body containing data sent to the server.

Methods commonly using a body include:

- POST.
- PUT.
- PATCH.

### Form Data Example

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 31

username=student&password=test123
```

### JSON Example

```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "student",
  "email": "student@example.com"
}
```

---

# HTTP Request Methods

HTTP methods, also called HTTP verbs, define the action the client wants to perform on a resource.

| Method | Function | Safe | Idempotent |
|---|---|---:|---:|
| GET | Retrieves data from the server | Yes | Yes |
| POST | Sends data for processing or creation | No | No |
| PUT | Creates or replaces a complete resource | No | Usually Yes |
| DELETE | Removes a resource | No | Usually Yes |
| PATCH | Applies a partial update to a resource | No | Not always |
| HEAD | Retrieves headers only, without the response body | Yes | Yes |
| OPTIONS | Retrieves communication options and supported methods | Yes | Yes |

### Safe Methods

A **safe** method should not change the server state.

Examples:

```text
GET
HEAD
OPTIONS
```

### Idempotent Methods

An **idempotent** method should have the same result when repeated multiple times.

For example, calling the same `GET` request ten times should not change server data.

---

## GET

`GET` retrieves data from the server.

```http
GET /products?id=10 HTTP/1.1
Host: example.com
```

GET should not modify server data.

### Security Note

Sensitive information should not be sent through GET parameters because URLs can be stored in:

- Browser history.
- Server logs.
- Proxy logs.
- Bookmarks.
- Referrer headers.

Bad example:

```text
https://example.com/login?username=student&password=password123
```

---

## POST

`POST` sends data to the server for processing.

It is commonly used for:

- Login forms.
- User registration.
- File uploads.
- Creating orders.
- Creating comments.
- Submitting payment data.

```http
POST /register HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=student&password=securepassword
```

POST can change the state of the server and is not normally idempotent.

---

## PUT

`PUT` creates or replaces a resource at a specific URL.

```http
PUT /api/users/10 HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "username": "student",
  "role": "user"
}
```

If the resource exists, PUT usually replaces it completely. If it does not exist, PUT may create it.

---

## DELETE

`DELETE` requests the removal of a resource.

```http
DELETE /api/users/10 HTTP/1.1
Host: example.com
```

DELETE requests must be properly authenticated and authorized.

If authorization is weak, an attacker may delete data belonging to other users.

---

## PATCH

`PATCH` applies partial modifications to a resource.

```http
PATCH /api/users/10 HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "email": "new-email@example.com"
}
```

Unlike PUT, PATCH typically changes only selected fields instead of replacing the entire resource.

---

## HEAD

`HEAD` is similar to GET but returns only response headers, not the response body.

```http
HEAD /backup.zip HTTP/1.1
Host: example.com
```

HEAD can be useful for checking:

- Whether a resource exists.
- Response headers.
- Content type.
- Content length.
- Last modification time.

---

## OPTIONS

`OPTIONS` retrieves the communication options available for a resource.

```http
OPTIONS /api/users HTTP/1.1
Host: example.com
```

The server may return allowed methods through the `Allow` header.

```http
Allow: GET, POST, OPTIONS
```

During security testing, OPTIONS can help identify enabled methods such as PUT, DELETE, or PATCH.

---

# HTTP Responses

## HTTP Response Components

An HTTP response normally contains:

1. Status line.
2. Response headers.
3. Empty line.
4. Optional response body.

### Response Structure

```http
HTTP/version status-code status-message
Header-Name: Header-Value
Header-Name: Header-Value

Response body
```

### Example

```http
HTTP/1.1 200 OK
Date: Fri, 13 Mar 2015 11:26:05 GMT
Cache-Control: private, max-age=0
Content-Type: text/html; charset=UTF-8
Content-Encoding: gzip
Server: nginx
Content-Length: 258

<html>
  <body>
    <h1>Hello World</h1>
  </body>
</html>
```

---

## Response Status Line

The first line of an HTTP response is called the **status line**.

```http
HTTP/1.1 200 OK
```

It contains:

```text
HTTP/1.1 = protocol version.
200      = HTTP status code.
OK       = human-readable status message.
```

---

## Common HTTP Status Codes

| Status Code | Meaning |
|---|---|
| 200 OK | The request was successful and the server returned the requested resource |
| 201 Created | A resource was successfully created |
| 204 No Content | The request was successful but there is no response body |
| 301 Moved Permanently | The resource has permanently moved to another URL |
| 302 Found | The resource is temporarily available at another URL |
| 303 See Other | The client should retrieve another URL using GET |
| 307 Temporary Redirect | Temporary redirect; the request method should be preserved |
| 400 Bad Request | The server cannot process the request because it is malformed |
| 401 Unauthorized | Authentication is required or credentials are invalid |
| 403 Forbidden | The server understood the request but refuses access |
| 404 Not Found | The requested resource does not exist |
| 405 Method Not Allowed | The HTTP method is not allowed for the resource |
| 429 Too Many Requests | The client sent too many requests, often due to rate limiting |
| 500 Internal Server Error | The server encountered an unexpected error |
| 502 Bad Gateway | A gateway or proxy received an invalid response from an upstream server |
| 503 Service Unavailable | The service is temporarily unavailable or overloaded |

---

## Response Headers

Response headers provide information about the response, server behavior, caching, cookies, and content.

Common response headers include:

| Header | Function |
|---|---|
| Date | Shows when the server generated the response |
| Content-Type | Specifies the media type of the response |
| Content-Length | Specifies the response body size in bytes |
| Content-Encoding | Specifies applied compression, such as gzip |
| Server | Identifies the web-server software or server banner |
| Set-Cookie | Instructs the browser to store a cookie |
| Cache-Control | Defines caching rules |
| Location | Specifies the redirect destination |
| Strict-Transport-Security | Instructs the browser to use HTTPS in the future |
| Access-Control-Allow-Origin | Defines which origins may access a resource through CORS |

---

## Date Header

The `Date` header indicates when the response was generated by the server.

```http
Date: Fri, 13 Mar 2015 11:26:05 GMT
```

It helps clients and intermediary systems evaluate response freshness and synchronize time-related information.

---

## Content-Type Header

The `Content-Type` header specifies the type of data returned by the server.

```http
Content-Type: text/html; charset=UTF-8
```

Examples:

```text
text/html                 = HTML page.
application/json          = JSON data.
application/xml           = XML data.
text/plain                = plain text.
image/png                 = PNG image.
application/pdf           = PDF document.
application/javascript    = JavaScript content.
```

Browsers use the content type to determine how to process and display the response.

---

## Content-Length Header

The `Content-Length` header indicates the size of the response body in bytes.

```http
Content-Length: 258
```

This helps the browser know how much content to expect.

---

## Content-Encoding Header

The `Content-Encoding` header specifies the compression applied to the response body.

```http
Content-Encoding: gzip
```

The browser uses this header to decompress the response correctly.

Common values:

```text
gzip
deflate
br
```

---

## Server Header

The `Server` header may reveal the web-server software or banner.

```http
Server: Apache/2.4.57
```

Other examples:

```text
Server: nginx
Server: Microsoft-IIS/10.0
Server: gws
```

### Security Note

Detailed server banners may help attackers fingerprint the server version and identify known vulnerabilities.

---

## Location Header

The `Location` header is commonly used with redirects.

```http
HTTP/1.1 302 Found
Location: https://example.com/login
```

The browser follows the location and requests the new URL.

---

## Set-Cookie Header

The `Set-Cookie` header tells the browser to store a cookie.

```http
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Lax
```

This is commonly used for session management after login.

---

# Cache-Control

The `Cache-Control` header defines how the browser and intermediary caches should store and reuse a response.

```http
Cache-Control: private, max-age=0
```

Common directives:

| Directive | Meaning |
|---|---|
| `public` | The response can be cached by browsers and shared intermediary caches |
| `private` | The response is intended for one user and should not be cached by shared proxies |
| `no-cache` | The response may be stored but must be revalidated before reuse |
| `no-store` | The response must not be stored by browsers or intermediaries |
| `max-age=<seconds>` | Defines how long the response may remain cached |

### Security Note

Sensitive pages, such as account pages or payment pages, should use restrictive caching rules.

```http
Cache-Control: no-store
```

---

# Cookies And Sessions

## What Is A Session?

A session allows a website to maintain temporary state between a user and the server.

Sessions allow the server to remember user-specific information while the user navigates through different pages.

Sessions are commonly used for:

- Authentication.
- Shopping carts.
- User preferences.
- Multi-step forms.
- Temporary access state.

## What Is A Cookie?

A cookie is a small piece of data sent by a website to the browser.

The browser stores it and sends it back to the server in later requests.

```http
Set-Cookie: session=abc123
```

Later, the browser may send:

```http
Cookie: session=abc123
```

---

## Cookie Attributes

Cookie attributes define the scope, lifetime, and security behavior of cookies.

| Attribute | Function |
|---|---|
| Name | Unique identifier of the cookie |
| Value | Data stored in the cookie |
| Domain | Defines which domain can receive the cookie |
| Path | Defines which URL path can receive the cookie |
| Expires / Max-Age | Defines the cookie lifetime |
| Secure | Sends the cookie only through HTTPS |
| HttpOnly | Prevents JavaScript from accessing the cookie |
| SameSite | Controls cross-site cookie behavior |
| Priority | Influences browser cookie eviction priority |
| Size | Total maximum size of the cookie name, value, and metadata |

### Secure Cookie Example

```http
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Strict; Path=/
```

```text
Secure          = cookie is sent only through HTTPS.
HttpOnly        = JavaScript cannot read the cookie.
SameSite=Strict = browser restricts cross-site cookie sending.
Path=/          = cookie is available across the entire site.
```

---

# Security Headers

Security headers help browsers apply additional security protections.

| Header | Purpose |
|---|---|
| Content-Security-Policy | Restricts permitted content sources and helps mitigate script injection |
| Strict-Transport-Security | Forces future connections to use HTTPS |
| X-Frame-Options | Controls whether the page can appear in a frame or iframe, helping prevent clickjacking |
| Referrer-Policy | Controls how much URL information is sent through the Referer header |
| X-Content-Type-Options | Prevents MIME-type sniffing |
| Permissions-Policy | Controls access to browser features such as camera, microphone, and geolocation |

### Content-Security-Policy

```http
Content-Security-Policy: default-src 'self'
```

This restricts content loading to the same origin unless other sources are explicitly allowed.

### Strict-Transport-Security

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

This instructs the browser to use HTTPS for the domain during the specified period.

### X-Frame-Options

```http
X-Frame-Options: DENY
```

This prevents the page from being embedded inside frames or iframes.

### Referrer-Policy

```http
Referrer-Policy: strict-origin-when-cross-origin
```

This reduces unnecessary URL information exposure when users navigate to other origins.

---

# HTTPS

## What Is HTTPS?

**HTTPS** stands for **Hypertext Transfer Protocol Secure**.

HTTPS is the secure version of HTTP and provides encrypted communication between a client and a web server.

HTTPS runs HTTP over **SSL/TLS**.

```text
HTTP Request / Response
          |
          v
SSL / TLS Encryption Layer
          |
          v
TCP
```

### HTTPS Architecture

```text
[ Browser ]
     |
     | HTTP data protected by TLS
     v
[ Internet ]
     |
     | Encrypted communication
     v
[ Web Server ]
```

---

## Why HTTP Is Insecure

By default, HTTP traffic is sent in clear text.

An attacker who can intercept HTTP traffic may be able to read or modify:

- Usernames.
- Passwords.
- Session cookies.
- Form data.
- API tokens.
- Personal information.
- Web page content.

HTTP does not provide strong encryption, integrity protection, or authentication between the browser and server.

---

## SSL And TLS

SSL (*Secure Sockets Layer*) and TLS (*Transport Layer Security*) are cryptographic protocols used to provide secure communication over a network.

TLS is the modern protocol. SSL is the older term often used informally when referring to HTTPS certificates and encrypted web traffic.

HTTPS provides:

- **Confidentiality:** Attackers cannot easily read encrypted traffic.
- **Integrity:** Attackers cannot easily modify data in transit without detection.
- **Authentication:** Certificates help the browser verify the server identity.

---

## HTTPS Advantages

### Encryption Of Data In Transit

HTTPS encrypts data transmitted between the browser and server.

Even if an attacker intercepts the traffic, they should not be able to read the encrypted content.

### Protection Against Eavesdropping

HTTPS helps protect sensitive data from network interception, including:

- Login credentials.
- Credit-card information.
- Personal details.
- Session tokens.
- Private messages.
- API keys.

### Protection Against Manipulation

HTTPS reduces the risk of attackers modifying HTTP traffic in transit.

For example, it helps prevent an attacker on the network from injecting malicious JavaScript into an unencrypted HTTP response.

---

## HTTPS Does Not Fix Web Vulnerabilities

HTTPS is essential, but it does not protect against flaws inside the web application.

The following vulnerabilities can still exist over HTTPS:

- SQL injection.
- Cross-site scripting.
- Broken access control.
- CSRF.
- SSRF.
- File upload vulnerabilities.
- Weak authentication.
- Insecure session management.
- Insecure APIs.
- Business logic flaws.

```text
HTTPS protects data while it travels between client and server.

HTTPS does not automatically protect the application from insecure code,
weak access controls, bad configurations, or vulnerable components.
```

---

# HTTP Method Enumeration

During an authorized security assessment, it can be useful to identify the HTTP methods accepted by a web server or resource.

## OPTIONS With cURL

```bash
curl -X OPTIONS -i http://target.local/  # sends an OPTIONS request and displays the full response headers.
```

Look for the `Allow` header:

```http
Allow: GET, POST, OPTIONS
```

## Nmap HTTP Methods Script

```bash
nmap -p 80,443 --script http-methods <TARGET_IP>  # checks supported HTTP methods on ports 80 and 443.
```

```text
-p 80,443              = scans common HTTP and HTTPS ports.
--script http-methods  = uses Nmap's HTTP method enumeration script.
<TARGET_IP>            = target IP address or hostname.
```

Only test systems that are within the authorized scope.

---

# Useful cURL Commands

`curl` is a command-line tool for sending HTTP requests and viewing server responses.

```bash
curl http://example.com  # retrieves the page content from the URL.
curl -I http://example.com  # retrieves only response headers.
curl -i http://example.com  # retrieves response headers and body.
curl -L http://example.com  # follows HTTP redirects.
curl -A "Custom User Agent" http://example.com  # sends a custom User-Agent header.
```

## Send A POST Request

```bash
curl -X POST -d "param1=value1&param2=value2" http://example.com/api  # sends form data through an HTTP POST request.
```

```text
-X POST = specifies the POST HTTP method.
-d       = sends request-body data.
```

## Send JSON Data

```bash
curl -X POST http://example.com/api \
  -H "Content-Type: application/json" \
  -d '{"username":"student","role":"user"}'  # sends JSON data in a POST request.
```

## Use Basic Authentication

```bash
curl -u username:password http://api.example.com/data  # sends HTTP Basic Authentication credentials.
```

## Download A File

```bash
curl -O http://example.com/file.txt  # downloads file.txt using its remote filename.
```

## Upload A File

```bash
curl --upload-file test.txt http://example.com/upload/test.txt  # uploads test.txt using an HTTP PUT request when supported.
```

---

# Other Basic Web Assessment Tools

## Nmap

Nmap helps identify open ports, services, versions, and HTTP-related information.

```bash
nmap -sV -p 80,443 <TARGET_IP>  # detects service versions on HTTP and HTTPS ports.
```

```bash
nmap -p 80,443 --script http-title,http-headers <TARGET_IP>  # retrieves HTTP page titles and headers.
```

## DIRB

DIRB is a directory and file enumeration tool.

It uses wordlists to search for potentially hidden directories and files on a web server.

```bash
dirb http://target.local  # starts directory and file enumeration against the target website.
```

Examples of potentially interesting results:

```text
/admin
/login
/uploads
/backups
/config
/robots.txt
/.git
```

## Burp Suite

Burp Suite is a web application security-testing platform.

It can be used to:

- Intercept HTTP/S traffic.
- Inspect requests and responses.
- Modify requests manually.
- Replay requests.
- Test parameters.
- Analyze cookies and headers.
- Map the application attack surface.

Useful Burp Suite components include:

```text
Proxy      = intercepts browser traffic.
Repeater   = manually modifies and re-sends requests.
Intruder   = automates controlled request variations.
Decoder    = encodes and decodes data.
Comparer   = compares requests and responses.
```

---

# Practical HTTP/S Workflow

A basic HTTP/S assessment workflow is:

1. Identify the target web server and open ports.
2. Browse the application and map functionality.
3. Inspect HTTP requests and responses with browser tools or Burp Suite.
4. Check HTTP headers, cookies, redirects, and status codes.
5. Enumerate supported HTTP methods.
6. Review security headers.
7. Verify whether HTTPS is enabled and enforced.
8. Check session-cookie security attributes.
9. Identify directories, files, APIs, and parameters.
10. Test only within the approved scope.

---

# Key Takeaways

- HTTP is a stateless application-layer protocol that runs on top of TCP.
- HTTP uses a client-server architecture where clients send requests and servers return responses.
- HTTP requests contain a request line, headers, and an optional body.
- HTTP responses contain a status line, headers, and an optional body.
- Important request headers include `Host`, `User-Agent`, `Accept`, `Authorization`, and `Cookie`.
- Important response headers include `Content-Type`, `Content-Length`, `Set-Cookie`, `Cache-Control`, and `Server`.
- HTTP methods define how clients interact with resources; common methods are GET, POST, PUT, DELETE, PATCH, HEAD, and OPTIONS.
- Cookies and sessions allow web applications to maintain user state.
- HTTPS uses TLS to provide confidentiality, integrity, and server authentication.
- HTTPS protects data in transit but does not prevent web application vulnerabilities such as XSS, SQL injection, or broken access control.
- Tools such as cURL, Nmap, DIRB, and Burp Suite are useful for understanding and assessing HTTP/S communication.
