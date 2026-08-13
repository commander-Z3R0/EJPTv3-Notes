# Web Application Architecture & Components

## Web Application Architecture

Web application architecture refers to the structure, organization, and interaction of the components used to build a web application.

It defines how the application:

- Handles user requests.
- Processes business logic.
- Stores and retrieves data.
- Communicates with external services.
- Delivers responses to users.

A well-designed architecture is important for:

- Scalability.
- Maintainability.
- Performance.
- Reliability.
- Security.

For web application security testing, understanding the underlying architecture is essential. It helps identify where vulnerabilities, misconfigurations, insecure data flows, and weak access controls may exist.

---

## Client-Server Model

Web applications are usually built using the **client-server model**.

The application is divided into two main parts:

- **Client:** The front-end accessed by the user through a web browser.
- **Server:** The back-end that processes requests, applies business logic, communicates with databases, and returns responses.

### Basic architecture

```text
[ User / Client Browser ]
          |
          | HTTP / HTTPS Request
          v
[ Web Server ]
          |
          | Static files or forwarded request
          v
[ Application Server / Backend ]
          |
          | Database queries / API requests
          v
[ Database / External Services ]
          |
          | Data response
          v
[ Application Server / Backend ]
          |
          | HTTP / HTTPS Response
          v
[ User / Client Browser ]
```

---

## Core Web Components

| Component | Function |
|---|---|
| User Interface (UI) | The visual part of the application that users see and interact with, including web pages, forms, menus, buttons, and input fields |
| Client-side technologies | Technologies that run inside the user’s browser, such as HTML, CSS, and JavaScript |
| Web server | Receives HTTP requests and serves static content such as HTML, CSS, JavaScript, images, and files |
| Application server | Executes server-side code, processes business logic, handles dynamic requests, and communicates with databases |
| Server-side technologies | Languages and frameworks used to process requests and generate dynamic content, such as PHP, Python, Java, Ruby, or Node.js |
| Database server | Stores and manages application data, including user accounts, products, posts, configuration, and application records |
| Application logic | The business rules that define how the application works, including authentication, validation, authorization, and workflows |
| APIs | Interfaces that allow applications and services to exchange data programmatically |

---

## Client-Side Processing

Client-side processing occurs on the user’s device, usually inside the web browser.

It is responsible for presenting the application interface and handling interactive tasks without always needing to contact the server.

### Main client-side technologies

```text
HTML        = defines the structure and content of a page.
CSS         = defines the appearance, layout, colors, and fonts.
JavaScript  = adds interactivity and dynamic behavior.
Cookies     = store small pieces of browser data, often for sessions.
Local Storage = stores browser data that can persist after closing the browser.
```

### Common client-side tasks

- Displaying web pages.
- Handling buttons, menus, and forms.
- Performing basic input validation.
- Updating page elements dynamically.
- Sending asynchronous requests to APIs.
- Storing user preferences.
- Managing client-side application state.

### Important security note

Client-side controls must never be trusted on their own.

A user or attacker can modify:

- HTML.
- JavaScript.
- Hidden form fields.
- Browser requests.
- Cookies.
- Local storage values.

For this reason, critical validation, authorization, and security checks must also be implemented on the server.

---

## Server-Side Processing

Server-side processing occurs on the remote server where the application is hosted.

The server receives requests from clients, applies business logic, accesses data, performs security checks, and generates responses.

### Common server-side technologies

```text
PHP       = widely used language for dynamic web applications.
Python    = often used with frameworks such as Django or Flask.
Java      = used with enterprise frameworks and application servers.
Ruby      = commonly used with Ruby on Rails.
JavaScript / Node.js = allows JavaScript to execute on the server.
```

### Common server-side tasks

- Processing user login requests.
- Validating credentials.
- Applying authorization rules.
- Querying databases.
- Processing payments.
- Uploading and storing files.
- Sending emails.
- Calling external APIs.
- Generating dynamic HTML or JSON responses.
- Logging activity and security events.

### Security advantage

Server-side processing is more secure than client-side processing for sensitive operations because users cannot directly modify the server-side code.

However, server-side code can still contain vulnerabilities, such as:

- SQL injection.
- Command injection.
- Server-side request forgery.
- File inclusion.
- Insecure deserialization.
- Broken access control.
- Authentication bypasses.

---

## Web Server vs Application Server

### Web Server

A web server receives HTTP or HTTPS requests and serves static content.

Common web servers include:

```text
Apache HTTP Server
Nginx
Microsoft IIS
```

A web server commonly serves:

- HTML files.
- CSS files.
- JavaScript files.
- Images.
- Fonts.
- Downloadable documents.
- Static assets.

### Application Server

An application server executes server-side code and handles dynamic requests.

It typically:

- Processes application logic.
- Handles user actions.
- Communicates with databases.
- Generates dynamic pages.
- Returns API responses.
- Enforces business rules.

In smaller applications, the web server and application server may run on the same machine. In larger environments, they may be separated for performance and security.

---

## Databases

Databases store and manage the information used by web applications.

Common database contents include:

- User accounts.
- Password hashes.
- Product information.
- Customer records.
- Orders.
- Posts and comments.
- Application configurations.
- Session data.
- Audit logs.

### Common database technologies

```text
MySQL
MariaDB
PostgreSQL
Microsoft SQL Server
Oracle Database
MongoDB
```

### Security relevance

Databases are high-value targets because they may contain sensitive information.

Common database-related risks include:

- SQL injection.
- Weak database credentials.
- Excessive user privileges.
- Exposed database ports.
- Sensitive data stored without encryption.
- Backups exposed to the internet.
- Verbose database error messages.

---

## Application Logic

Application logic represents the rules and procedures that control how the application functions.

Examples include:

- User registration.
- Login and logout.
- Password reset.
- Role-based access control.
- Shopping cart calculations.
- Payment validation.
- File-upload rules.
- Data-validation rules.
- Account-management functions.
- Administrative workflows.

### Security relevance

Business logic vulnerabilities occur when an attacker abuses legitimate application features in an unintended way.

Examples:

- Changing a product price in a request.
- Reusing a discount code.
- Skipping a payment step.
- Accessing another user’s invoice by changing an ID.
- Changing account roles through modified requests.
- Bypassing approval workflows.

---

## HTTP Communication And Data Flow

Web applications communicate over the internet through **HTTP** or **HTTPS**.

When a user clicks a link, submits a form, or loads a page, the browser sends an HTTP request to the server.

The server processes the request, may query a database or external API, and sends an HTTP response back to the browser.

### Basic request flow

```text
1. User enters a URL or clicks a link.
2. Browser sends an HTTP/HTTPS request.
3. Web server receives the request.
4. Application server processes the request.
5. Backend queries a database or API if needed.
6. Backend generates a response.
7. Server sends an HTTP/HTTPS response.
8. Browser renders the response for the user.
```

### Example HTTP request

```http
GET /profile?id=10 HTTP/1.1
Host: example.com
Cookie: session=abc123
```

### Example HTTP response

```http
HTTP/1.1 200 OK
Content-Type: text/html

<html>
  <body>
    <h1>User Profile</h1>
  </body>
</html>
```

---

## How Web Pages Are Rendered

When a user visits a web address, the browser requests a resource from the server.

### Rendering process

```text
1. User visits a URL.
2. Browser requests the web page from the server.
3. Server returns an HTML document.
4. Browser parses the HTML.
5. Browser downloads linked CSS, JavaScript, images, and fonts.
6. Browser parses CSS and builds style information.
7. Browser executes JavaScript.
8. JavaScript may modify the page and request extra data from APIs.
9. Browser renders the final page for the user.
```

### Browser rendering model

```text
[ HTML Response ]
       |
       v
[ HTML Parser ] -----> Builds the DOM
       |
       +-----> Downloads CSS files
       |              |
       |              v
       |         [ CSS Parser ]
       |
       +-----> Downloads JavaScript files
                      |
                      v
              [ JavaScript Engine ]
                      |
                      v
              Modifies the DOM
                      |
                      v
              [ Rendered Web Page ]
```

### DOM

The **DOM** (*Document Object Model*) is a structured representation of the web page created by the browser.

JavaScript can read and modify the DOM dynamically.

For example, JavaScript can:

- Change page text.
- Hide or show elements.
- Modify forms.
- Add buttons.
- Load new data.
- Send API requests.
- Change browser behavior.

---

## Data Interchange

**Data interchange** is the process of exchanging data between different applications, systems, or services.

It allows systems with different programming languages, data structures, operating systems, or platforms to communicate.

Web applications commonly exchange data with:

- Databases.
- External APIs.
- Mobile applications.
- Payment gateways.
- Authentication providers.
- Cloud services.
- Internal business systems.
- Microservices.

---

## APIs

An **API** (*Application Programming Interface*) allows different software systems to communicate and exchange data.

For example, a web application may use APIs to:

- Process payments.
- Send emails.
- Retrieve weather information.
- Authenticate with Google or Microsoft.
- Access maps.
- Retrieve product information.
- Connect mobile applications to the backend.

### API security relevance

APIs can expose sensitive data or functions if they are poorly secured.

Important API security checks include:

- Authentication.
- Authorization.
- Token validation.
- Rate limiting.
- Input validation.
- Error handling.
- Data exposure.
- Endpoint enumeration.
- Logging and monitoring.

---

## JSON

**JSON** (*JavaScript Object Notation*) is a lightweight data-interchange format commonly used in web applications and APIs.

It is easy for humans to read and for applications to process.

### Example JSON data

```json
{
  "username": "student",
  "role": "user",
  "email": "student@example.com"
}
```

JSON is commonly used when a browser communicates with a REST API.

---

## XML

**XML** (*eXtensible Markup Language*) is a data-interchange format that uses tags to define data structure.

It is often used for:

- Configuration files.
- Legacy systems.
- SOAP web services.
- Enterprise integrations.

### Example XML data

```xml
<user>
  <username>student</username>
  <role>user</role>
  <email>student@example.com</email>
</user>
```

### Security relevance

Applications that process XML may be vulnerable to XML External Entity attacks if XML parsers are insecurely configured.

---

## REST

**REST** (*Representational State Transfer*) is an architectural style commonly used for web APIs.

REST APIs usually use standard HTTP methods:

```text
GET     = retrieve data.
POST    = create data.
PUT     = replace or update data.
PATCH   = partially update data.
DELETE  = remove data.
```

### Example REST API endpoints

```text
GET    /api/users          # retrieves users.
GET    /api/users/10       # retrieves user 10.
POST   /api/users          # creates a user.
PUT    /api/users/10       # updates user 10.
DELETE /api/users/10       # deletes user 10.
```

### Security relevance

During testing, verify that authorization controls are applied to every API endpoint and every HTTP method.

---

## SOAP

**SOAP** (*Simple Object Access Protocol*) is a protocol for exchanging structured information between systems.

SOAP usually uses XML and provides a standardized communication method for web services.

It is often found in enterprise environments and legacy applications.

---

## Security Technologies

### Authentication And Authorization

Authentication confirms a user’s identity.

Authorization determines which application areas, actions, and data the user can access.

Examples include:

- Username and password authentication.
- Multi-factor authentication.
- Session cookies.
- JWT tokens.
- Role-based access control.
- Access-control lists.

---

### SSL And TLS

**TLS** encrypts communication between the client and the server.

HTTPS uses HTTP over TLS.

```text
HTTP  = data can be transmitted without encryption.
HTTPS = data is encrypted while in transit.
```

TLS helps protect against:

- Credential interception.
- Session-cookie theft on the network.
- Man-in-the-middle attacks.
- Sensitive-data exposure in transit.

---

## External Technologies

### Content Delivery Networks

A **Content Delivery Network**, or CDN, distributes static content across multiple servers located in different geographical regions.

CDNs commonly deliver:

- Images.
- CSS files.
- JavaScript libraries.
- Fonts.
- Video files.
- Downloadable assets.

Benefits include:

- Faster page loading.
- Reduced load on the origin server.
- Better availability.
- Improved reliability.
- Some protection against large traffic volumes.

---

### Third-Party Libraries And Frameworks

Web applications often use third-party libraries and frameworks to speed up development and add advanced features.

Examples include:

```text
Frontend frameworks: React, Angular, Vue.js
Backend frameworks: Django, Flask, Laravel, Spring
JavaScript libraries: jQuery, Lodash
CMS platforms: WordPress, Drupal, Joomla
```

### Security relevance

Third-party components can introduce vulnerabilities if they are outdated or insecure.

During a security assessment, identify:

- Framework versions.
- Server technologies.
- JavaScript libraries.
- CMS plugins.
- Dependencies.
- Known vulnerable components.

---

## Web Application Security Assessment View

When assessing a web application, understand which component processes each action.

```text
[ Browser ]
    |
    | Client-side: HTML, CSS, JavaScript, Cookies
    v
[ Web Server ]
    |
    | Apache, Nginx, IIS
    v
[ Application Server ]
    |
    | PHP, Python, Java, Ruby, Node.js
    v
[ Database ]
    |
    | MySQL, PostgreSQL, MSSQL, Oracle
    v
[ External APIs / Services ]
```

This helps identify potential attack surfaces:

| Component | Example Security Areas |
|---|---|
| Browser / Client | XSS, DOM manipulation, insecure local storage, exposed tokens |
| Web server | Misconfiguration, exposed files, weak TLS, directory listing |
| Application server | Authentication flaws, access-control issues, injection vulnerabilities, business logic flaws |
| Database | SQL injection, weak permissions, exposed data, insecure backups |
| APIs | Broken object authorization, token issues, excessive data exposure, missing rate limits |
| Third-party components | Outdated libraries, vulnerable plugins, insecure dependencies |

---

## Key Takeaways

- Web application architecture defines how components interact to process requests and manage data.
- Web applications commonly use a client-server model.
- Client-side technologies include HTML, CSS, JavaScript, cookies, and local storage.
- Server-side technologies include web servers, application servers, databases, and server-side languages.
- Sensitive validation, authorization, and business logic must always be enforced on the server side.
- HTTP and HTTPS are used for communication between clients and servers.
- APIs commonly exchange data using JSON or XML.
- REST APIs use methods such as GET, POST, PUT, PATCH, and DELETE.
- Understanding the architecture helps a pentester identify attack surfaces, vulnerabilities, and misconfigurations.
