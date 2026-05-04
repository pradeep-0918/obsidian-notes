# 📚 Web Security & Web Technology — 2 Marks Master Notes

> **Exam-Ready | Obsidian Format | Feynman Technique | Topper-Style Answers**

---

# 🔐 PART A — WEB SECURITY

---

## 1. Differentiate Between Authentication and Authorization

> **1-Line Core Idea:** Authentication asks _"Who are you?"_ — Authorization asks _"What can you do?"_

### 📌 2-Mark Answer

**Authentication** is the process of **verifying the identity** of a user or system (e.g., login with username & password).

**Authorization** is the process of **granting or denying access** to resources after identity is confirmed.

|Feature|Authentication|Authorization|
|---|---|---|
|**Purpose**|Verifies identity|Controls access|
|**Question**|Who are you?|What can you do?|
|**Happens**|First|After authentication|
|**Example**|Login page|Role-based access (admin vs user)|
|**Technique**|Password, OTP, Biometrics|ACL, RBAC, OAuth|

### ⚡ Quick Revision

- **Auth-N** = Identity check (Login)
- **Auth-Z** = Permission check (Access Control)
- Auth-N always comes **before** Auth-Z

---

## 2. What is Secure Socket Layer (SSL)?

> **1-Line Core Idea:** SSL is a **security protocol** that encrypts data between a client and server to ensure safe communication.

### 📌 2-Mark Answer

**SSL (Secure Socket Layer)** is a **cryptographic protocol** developed by Netscape that provides **secure, encrypted communication** over the internet between a web browser and a web server.

**Key Points:**

- Uses **public key cryptography** and **digital certificates** to establish a secure channel
- Ensures **data confidentiality, integrity, and authentication**
- Operates between the **Transport Layer and Application Layer** (OSI model)
- Modern replacement is **TLS (Transport Layer Security)**; HTTPS uses SSL/TLS

```
Client Browser  ←——— Encrypted Data (SSL/TLS) ———→  Web Server
    🔒                                                    🔒
     └──────────── Certificate Exchange ─────────────────┘
```

### ⚡ Quick Revision

- SSL → Encrypt → Secure Channel
- HTTPS = HTTP + SSL/TLS
- Uses **asymmetric** key exchange, **symmetric** data transfer

---

## 3. What is the Purpose of Security Testing in Web Applications?

> **1-Line Core Idea:** Security testing **finds and fixes vulnerabilities** in a web app before attackers exploit them.

### 📌 2-Mark Answer

**Security Testing** is a type of software testing that identifies **vulnerabilities, threats, and risks** in a web application to ensure data and resources are protected from malicious attacks.

**Key Points:**

- **Prevents data breaches** by detecting weaknesses like SQL Injection, XSS, CSRF
- **Ensures compliance** with security standards (OWASP, ISO 27001, GDPR)
- Validates **authentication, authorization, and session management**
- Protects **confidentiality, integrity, and availability (CIA Triad)** of data

### ⚡ Quick Revision

- Find vulnerabilities → Fix before attack
- CIA Triad = Confidentiality + Integrity + Availability
- Tools: Burp Suite, OWASP ZAP, Nessus

---

## 4. Define Security Incident Response Planning

> **1-Line Core Idea:** A **predefined action plan** to detect, contain, and recover from a security breach efficiently.

### 📌 2-Mark Answer

**Security Incident Response Planning (SIRP)** is a **documented set of procedures** that defines how an organization should detect, respond to, and recover from security incidents (cyberattacks, data breaches, etc.).

**Key Points:**

- Follows phases: **Preparation → Detection → Containment → Eradication → Recovery → Lessons Learned**
- Reduces **damage and downtime** during a security incident
- Assigns **roles and responsibilities** to response team members
- Ensures **legal, regulatory, and communication** requirements are met

### ⚡ Quick Revision

- Plan → Detect → Contain → Recover → Learn
- Also called **Incident Response Plan (IRP)**
- NIST SP 800-61 is the standard guideline

---

## 5. What is the Purpose of Session Cookies in API Security?

> **1-Line Core Idea:** Session cookies **maintain user state** across multiple API requests after authentication.

### 📌 2-Mark Answer

A **Session Cookie** is a small piece of data stored on the client side that holds a **session identifier (Session ID)**, allowing the server to recognize and maintain the authenticated state of a user across multiple HTTP requests.

**Key Points:**

- Since HTTP is **stateless**, session cookies provide **continuity** between requests
- The session ID maps to server-side session data (**user info, roles, permissions**)
- Should be marked **HttpOnly, Secure, and SameSite** to prevent theft (XSS, CSRF)
- Expires when the browser is closed (session cookie) or after a set time (persistent)

### ⚡ Quick Revision

- Cookie → carries Session ID → Server recognizes user
- HttpOnly = JS can't read it → prevents XSS theft
- Secure flag = only sent over HTTPS

---

## 6. Why is Audit Logging Important in API Security?

> **1-Line Core Idea:** Audit logs **record every API activity** to detect, investigate, and prevent unauthorized access.

### 📌 2-Mark Answer

**Audit Logging** is the process of **recording all API requests, responses, and system events** for monitoring, security analysis, and compliance purposes.

**Key Points:**

- Enables **detection of suspicious activities** (brute force, unauthorized access, data leaks)
- Provides **forensic evidence** for incident investigation after a breach
- Ensures **regulatory compliance** (GDPR, HIPAA, PCI-DSS)
- Helps in **debugging, performance monitoring**, and tracking API misuse

### ⚡ Quick Revision

- Log → Monitor → Detect → Investigate → Comply
- Logs should include: timestamp, user, IP, endpoint, status code
- Use centralized logging tools (ELK Stack, Splunk)

---

## 7. What is the Vulnerability Assessment Lifecycle?

> **1-Line Core Idea:** A **structured process** to identify, analyze, prioritize, and remediate security vulnerabilities systematically.

### 📌 2-Mark Answer

The **Vulnerability Assessment Lifecycle** is a **cyclic, step-by-step process** used to discover and manage security weaknesses in systems, networks, or applications.

**Phases:**

```
1. Planning & Scoping
       ↓
2. Vulnerability Scanning
       ↓
3. Vulnerability Analysis
       ↓
4. Risk Assessment & Prioritization
       ↓
5. Remediation & Patching
       ↓
6. Verification & Re-scanning
       ↓
   (Repeat cycle)
```

**Key Points:**

- It is a **continuous and repeatable** process, not a one-time activity
- Helps organizations maintain a **proactive security posture**

### ⚡ Quick Revision

- Plan → Scan → Analyze → Prioritize → Fix → Verify
- Tools: Nessus, OpenVAS, Qualys
- Output: Vulnerability Report with CVSS scores

---

## 8. Name a Vulnerability Assessment Tool for Cloud Environments

> **1-Line Core Idea:** **Qualys Cloud Platform** is a widely used tool for scanning and assessing vulnerabilities in cloud environments.

### 📌 2-Mark Answer

**Qualys Cloud Platform** is a cloud-based **vulnerability assessment and management tool** that continuously scans cloud infrastructure (AWS, Azure, GCP) for security weaknesses.

**Key Points:**

- Provides **continuous monitoring** of cloud assets and configurations
- Detects **misconfigurations, unpatched software, and compliance violations**
- Generates **compliance reports** for standards like PCI-DSS, ISO 27001, HIPAA
- Other tools: **AWS Inspector, Prisma Cloud (Palo Alto), Tenable.io, Lacework**

### ⚡ Quick Revision

- **Qualys** → Cloud scanning, compliance, continuous monitoring
- **AWS Inspector** → Native AWS vulnerability scanner
- **Prisma Cloud** → Multi-cloud security platform

---

## 9. What is Social Engineering in Hacking?

> **1-Line Core Idea:** Social Engineering is **manipulating people psychologically** to trick them into revealing confidential information or performing actions that compromise security.

### 📌 2-Mark Answer

**Social Engineering** is a **non-technical hacking technique** where attackers exploit **human psychology** (trust, fear, urgency) rather than software vulnerabilities to gain unauthorized access to systems or data.

**Key Points:**

- Most common form: **Phishing** (fake emails/websites to steal credentials)
- Other forms: **Pretexting, Baiting, Tailgating, Vishing (voice phishing)**
- Targets **human error** — the weakest link in cybersecurity
- Defense: **Security awareness training, multi-factor authentication, strict access control**

### ⚡ Quick Revision

- Attacker → Manipulates Human → Gets Access
- Phishing = most common social engineering attack
- "People, not systems, are the easiest to hack"

---

## 10. What Vulnerability Does Cross-Site Scripting (XSS) Exploit?

> **1-Line Core Idea:** XSS exploits the **lack of input validation/output encoding** in web applications to inject malicious scripts into web pages viewed by other users.

### 📌 2-Mark Answer

**Cross-Site Scripting (XSS)** is a web security vulnerability that exploits **insufficient input validation and output encoding**, allowing attackers to **inject malicious JavaScript** into web pages that are then executed in the victim's browser.

**Key Points:**

- Exploits the browser's **trust in content served from a trusted website**
- Types: **Stored XSS, Reflected XSS, DOM-based XSS**
- Can steal **session cookies, redirect users, deface websites, or spread malware**
- Defense: **Input validation, output encoding (HTML escaping), Content Security Policy (CSP)**

```
Attacker injects → <script>document.cookie</script>
                         ↓
             Victim visits the page
                         ↓
          Script runs in victim's browser → Cookie stolen!
```

### ⚡ Quick Revision

- XSS = Inject JS → Runs in victim's browser
- Exploits: **missing input sanitization + output encoding**
- Fix: Escape output + CSP header + HttpOnly cookies

---

---

# 🌐 PART B — WEB TECHNOLOGY

---

## 11. Why is HTTP Called a Stateless Protocol?

> **1-Line Core Idea:** HTTP is stateless because **each request is independent** — the server has no memory of previous requests.

### 📌 2-Mark Answer

**HTTP (HyperText Transfer Protocol)** is called a **stateless protocol** because the server does **not retain any information** about previous client requests. Each HTTP request is treated as a **new, independent transaction** with no connection to prior requests.

**Key Points:**

- After the server responds, it **forgets the client completely**
- No session or state is maintained between requests by default
- This makes HTTP **scalable and simple** but requires additional mechanisms (cookies, sessions, tokens) for state management
- Example: After login, every new page request must carry a **session token/cookie** for the server to recognize the user

### ⚡ Quick Revision

- Each request = Fresh start for server
- Stateless → Scalable but needs cookies/sessions for continuity
- Contrast: FTP is **stateful** (maintains connection state)

---

## 12. State the Functions of a Web Server

> **1-Line Core Idea:** A web server **receives HTTP requests, processes them, and sends back responses** (HTML pages, files, data) to clients.

### 📌 2-Mark Answer

A **Web Server** is software (and/or hardware) that **stores, processes, and delivers web content** to clients over the internet using the HTTP/HTTPS protocol.

**Key Functions:**

- **Handles HTTP/HTTPS requests** from browsers and returns appropriate responses
- **Serves static content** (HTML, CSS, images, JavaScript files) directly
- **Forwards dynamic requests** to application servers (e.g., PHP, Java, Python backends)
- **Manages connections** — handles multiple concurrent client connections
- **Provides security** via SSL/TLS, access control, and authentication
- **Logs requests and errors** for monitoring and debugging

**Examples:** Apache HTTP Server, Nginx, Microsoft IIS

### ⚡ Quick Revision

- Receive Request → Process → Send Response
- Static: directly served | Dynamic: forwarded to app server
- Popular: Apache, Nginx, IIS

---

## 13. Code to Add an Image in HTML with Attributes

> **1-Line Core Idea:** The `<img>` tag is used to embed images in HTML, with attributes controlling source, size, and accessibility.

### 📌 2-Mark Answer

The **`<img>`** tag is used to embed images in an HTML web page. It is a **self-closing (void) tag** that uses attributes to define the image.

**Code:**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Image Example</title>
</head>
<body>
    <img src="photo.jpg" 
         alt="A beautiful landscape" 
         width="500" 
         height="300" 
         title="Landscape Photo">
</body>
</html>
```

**Key Attributes:**

|Attribute|Purpose|
|---|---|
|`src`|Path/URL of the image (**mandatory**)|
|`alt`|Alternate text if image fails to load (accessibility)|
|`width`|Width of image in pixels or %|
|`height`|Height of image in pixels or %|
|`title`|Tooltip text shown on hover|

### ⚡ Quick Revision

- `src` = source (required), `alt` = accessibility (required)
- `<img>` is a void tag — no closing tag needed
- Always include `alt` for SEO and accessibility

---

## 14. Justify the Need for Client-Side Scripting

> **1-Line Core Idea:** Client-side scripting **reduces server load** and provides **instant, interactive responses** to user actions without server round-trips.

### 📌 2-Mark Answer

**Client-Side Scripting** refers to scripts (primarily **JavaScript**) that execute on the **user's browser** rather than on the server.

**Justification / Need:**

- **Reduces server load:** Simple validations (form checks) happen on browser, no server call needed
- **Faster response time:** User gets **instant feedback** without waiting for server round-trip
- **Improves interactivity:** Enables dynamic UI updates, animations, dropdown menus, sliders
- **Offline functionality:** Certain operations work even without internet connection
- **Better user experience:** Real-time features like auto-suggestions, live search, interactive maps

**Example:** Form validation — checking if email format is correct _before_ submitting to server

### ⚡ Quick Revision

- Runs on: Browser | Language: JavaScript
- Benefits: Speed + Interactivity + Reduced server calls
- Example: Form validation, AJAX, DOM manipulation

---

## 15. Life Cycle Methods of Servlet

> **1-Line Core Idea:** A Servlet's lifecycle has **3 main stages** — Initialization, Service, and Destruction — managed by the Servlet Container.

### 📌 2-Mark Answer

The **Servlet Life Cycle** defines how a Java Servlet is **loaded, initialized, used, and destroyed** by the Servlet Container (e.g., Apache Tomcat).

**Life Cycle Methods:**

```
1. init()        → Called ONCE when servlet is first loaded
                   → Used for initialization (DB connection, config)

2. service()     → Called for EVERY client request
                   → Calls doGet() or doPost() based on HTTP method

3. destroy()     → Called ONCE when servlet is removed/server shuts down
                   → Used for cleanup (closing connections)
```

**Flow:**

```
Client Request → Servlet Container → init() [once]
                                   → service() [per request]
                                   → destroy() [once on shutdown]
```

### ⚡ Quick Revision

- `init()` → Once at start
- `service()` → Every request
- `destroy()` → Once at end
- Container manages the lifecycle automatically

---

## 16. Why Do We Need Namespace in XML?

> **1-Line Core Idea:** XML Namespaces **prevent element name conflicts** when combining XML documents from different sources or vocabularies.

### 📌 2-Mark Answer

**XML Namespace** is a mechanism to **uniquely identify element and attribute names** in an XML document using a URI (Uniform Resource Identifier), avoiding naming conflicts when multiple XML vocabularies are combined.

**Key Points:**

- When merging two XML documents, both may have a `<table>` element — one meaning an HTML table, another meaning a furniture table — namespace **disambiguates** them
- Declared using `xmlns` attribute: `xmlns:prefix="URI"`
- The URI doesn't need to point to anything — it's just a **unique identifier**
- Essential for **SOAP, WSDL, and web services** that combine multiple XML schemas

```xml
<root xmlns:html="http://www.w3.org/HTML"
      xmlns:furn="http://www.furniture.com">
  <html:table>...</html:table>
  <furn:table>...</furn:table>
</root>
```

### ⚡ Quick Revision

- Namespace → Avoids name collision in XML
- `xmlns:prefix="URI"` syntax
- Critical in SOAP, WSDL, and merged XML docs

---

## 17. Mention the Document Object Model (DOM)

> **1-Line Core Idea:** DOM is a **tree-structured programming interface** that represents an HTML/XML document, allowing scripts to dynamically access and modify its content, structure, and style.

### 📌 2-Mark Answer

The **Document Object Model (DOM)** is a **platform-independent, language-neutral API** defined by W3C that represents an HTML or XML document as a **hierarchical tree of objects (nodes)**.

**Key Points:**

- Every HTML element becomes a **node** in the DOM tree
- JavaScript can use DOM to **add, remove, or modify** elements and attributes dynamically
- Structure: `Document → Root Element → Child Elements → Text/Attributes`
- DOM enables **dynamic web pages** — interactive, real-time content updates

```
        Document
            |
          <html>
         /      \
      <head>   <body>
        |       /   \
    <title>  <h1>   <p>
       |      |      |
    "Page"  "Hi"  "Text"
```

### ⚡ Quick Revision

- DOM = Tree representation of HTML/XML
- Nodes: Document, Element, Text, Attribute
- JavaScript manipulates DOM → Dynamic web pages

---

## 18. Illustrate the Features of JSP (JavaServer Pages)

> **1-Line Core Idea:** JSP allows embedding **Java code inside HTML** to create dynamic web content on the server side.

### 📌 2-Mark Answer

**JSP (JavaServer Pages)** is a **server-side technology** that allows embedding Java code directly within HTML pages using special tags, enabling the creation of **dynamic web content**.

**Key Features:**

- **Platform Independent:** Runs on any OS with a JVM (Write Once, Run Anywhere)
- **Separation of Presentation & Logic:** HTML for design, Java for logic
- **JSP Tags:** Uses `<% %>` (scriptlets), `<%= %>` (expressions), `<%! %>` (declarations)
- **Implicit Objects:** Pre-defined objects like `request`, `response`, `session`, `out` available without declaration
- **Auto-compilation:** JSP pages are automatically compiled into Servlets by the container
- **Reusable Components:** Supports JavaBeans and custom tag libraries (JSTL)
- **Session Management:** Built-in support via `session` implicit object

```jsp
<html>
<body>
    <% String name = "Student"; %>
    <h1>Welcome, <%= name %>!</h1>
</body>
</html>
```

### ⚡ Quick Revision

- JSP = HTML + Java code
- Compiled to Servlet automatically
- Key Tags: `<% %>` scriptlet, `<%= %>` expression, `<%! %>` declaration

---

## 19. What is the Concept Behind JAX-RPC Technology?

> **1-Line Core Idea:** JAX-RPC allows Java applications to **call remote procedures (methods) over a network** using XML-based SOAP messaging, enabling web service communication.

### 📌 2-Mark Answer

**JAX-RPC (Java API for XML-based Remote Procedure Call)** is a Java API that enables Java applications to **invoke methods on remote services** over a network using **SOAP (Simple Object Access Protocol)** and XML messaging.

**Key Points:**

- Based on the **RPC (Remote Procedure Call)** model — call a method on a remote machine as if it's local
- Uses **SOAP over HTTP** for message transmission and **WSDL** for service description
- Provides **stubs on client side** and **ties on server side** for transparent communication
- **Deprecated** and replaced by **JAX-WS (JAX Web Services)** in modern Java

```
Java Client → [SOAP/XML Request] → Network → Server
           ← [SOAP/XML Response] ←          ← Remote Method Executes
```

### ⚡ Quick Revision

- JAX-RPC = Java + SOAP + Remote Method Call
- Uses WSDL for service description
- Replaced by **JAX-WS** (modern standard)

---

## 20. Advantages and Disadvantages of AJAX

> **1-Line Core Idea:** AJAX allows web pages to **update content asynchronously** without full page reloads, making apps faster and more interactive — but adds complexity.

### 📌 2-Mark Answer

**AJAX (Asynchronous JavaScript and XML)** is a technique that allows web pages to **send and receive data from a server asynchronously** in the background without reloading the entire page.

**Advantages:**

- **Faster Performance:** Only specific page parts update → reduces bandwidth and load time
- **Better User Experience:** No page flicker/reload → feels like a desktop application
- **Reduced Server Load:** Only necessary data is exchanged, not entire HTML pages
- **Real-time Updates:** Enables live search, auto-complete, live notifications

**Disadvantages:**

- **SEO Challenges:** Search engines may not index dynamically loaded content properly
- **Browser History Issues:** Back/Forward buttons may not work correctly
- **JavaScript Dependency:** Fails if JavaScript is disabled in the browser
- **Security Risks:** Vulnerable to **XSS and CSRF** attacks if not properly secured
- **Debugging Difficulty:** Asynchronous code is harder to test and debug

### ⚡ Quick Revision

- AJAX = Async data fetch → Partial page update
- Pro: Speed + UX + Less bandwidth
- Con: SEO issues + JS dependent + Security risks

---

---

# ⚡ MASTER QUICK REVISION — ALL 20 TOPICS

|#|Topic|1-Line Core|
|---|---|---|
|1|Auth vs Authz|AuthN = Who are you? AuthZ = What can you do?|
|2|SSL|Encrypts client-server communication using certificates|
|3|Security Testing|Find and fix vulnerabilities before attackers do|
|4|Incident Response|Predefined plan: Detect → Contain → Recover|
|5|Session Cookies|Carry Session ID to maintain state in stateless HTTP|
|6|Audit Logging|Record API activities for monitoring and forensics|
|7|VA Lifecycle|Plan→Scan→Analyze→Prioritize→Fix→Verify|
|8|Cloud VA Tool|Qualys, AWS Inspector, Prisma Cloud|
|9|Social Engineering|Hack humans using psychology, not code|
|10|XSS|Injects malicious JS via missing input validation|
|11|HTTP Stateless|Server forgets client after each request|
|12|Web Server Functions|Receive HTTP → Process → Serve content|
|13|HTML Image|`<img src alt width height>`|
|14|Client-Side Scripting|JS on browser → Fast, interactive, less server load|
|15|Servlet Lifecycle|init() → service() → destroy()|
|16|XML Namespace|Prevents name collision using URI prefix|
|17|DOM|Tree of HTML nodes; JS manipulates it dynamically|
|18|JSP Features|Java in HTML; auto-compiled; implicit objects|
|19|JAX-RPC|Java remote method call via SOAP/XML|
|20|AJAX Pros/Cons|Fast + UX / SEO issues + JS dependent|

---

# 🎯 Possible Exam Questions

1. **"Differentiate Authentication and Authorization with an example."** → Use the table format with 5 comparison points
2. **"Explain the Servlet life cycle with a diagram."** → Draw the init→service→destroy flow
3. **"What are the advantages and disadvantages of AJAX? Explain with examples."** → 4 pros + 4 cons with real examples (Google search autocomplete)
4. **"Explain XSS attack with diagram and prevention techniques."**
5. **"What is DOM? Draw the DOM tree for a sample HTML document."**

---

> 📝 **Exam Tip:** Always write **definition first**, then **points**, then **example/diagram**. Use keywords like _"CIA Triad", "stateless", "asynchronous", "cryptographic protocol"_ — examiners reward technical vocabulary used correctly.