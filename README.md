# FUTURE_CS_03 – API Security Analysis Report (Task-03)

**Future Interns – Cyber Security Internship (Feb–March 2026)**

## Intern Details
- **Name:** Sanita Salomon  
- **Track:** Cyber Security  
- **CIN ID:** FIT/FEB26/CS6325  
- **Date Reported:** 15 March 2026  

---

## Task Objective
The goal of this task was to perform a structured **API security analysis** on a publicly available test API, focusing on:  
- Identifying common API security risks  
- Assessing authentication and access control mechanisms  
- Analyzing exposed data from API endpoints  
- Evaluating HTTP response headers and infrastructure information  
- Providing recommendations to improve API protection  

**Target API:** [JSONPlaceholder](https://jsonplaceholder.typicode.com)

---

## Tools Used
- **Postman** – Sending API requests and analyzing JSON responses  
- **Browser Developer Tools** – Inspecting network requests, response headers, and API behavior  
- **Manual Analysis** – Reviewing API responses for potential security risks  

---

## API Endpoint Analysis

### 1. `/posts` Endpoint
- **URL:** `https://jsonplaceholder.typicode.com/posts`  
- **Behavior:** Returns 100 posts with `userId`, `id`, `title`, and `body`.  

**Risks Identified:**  
- Lack of authentication (anyone can access all posts)  
- Excessive data exposure (all 100 posts returned in a single response)  
- No pagination or rate limiting  
- Predictable object identifiers (`id` 1–100)  

**Recommendations:**  
- Implement authentication (API keys, JWT, OAuth)  
- Apply access control to limit data access  
- Enable pagination (`/posts?page=1&limit=10`)  
- Add rate limiting to prevent abuse  
- Monitor API requests for unusual behavior  

---

### 2. `/users` Endpoint
- **URL:** `https://jsonplaceholder.typicode.com/users`  
- **Behavior:** Returns 10 user records with nested information (`id`, `name`, `username`, `email`, `phone`, `website`, `address`, `company`).  

**Risks Identified:**  
- Exposure of personal information (emails, phone numbers, addresses)  
- Lack of authentication  
- Excessive data exposure (nested fields)  
- Predictable user IDs (1–10)  

**Recommendations:**  
- Implement strong authentication  
- Apply access control policies (users access only their data)  
- Limit data exposure (return only necessary fields)  
- Add rate limiting to prevent automated scraping  
- Protect sensitive information  

---

### 3. `/posts/{id}` Endpoint
- **URL:** `https://jsonplaceholder.typicode.com/posts/1`  
- **Behavior:** Returns a single post object by ID (`userId`, `id`, `title`, `body`).  

**Risks Identified:**  
- Insecure Direct Object Reference (IDOR)  
- Lack of authentication  
- Predictable resource identifiers  
- No request rate limiting  

**Recommendations:**  
- Implement authentication for access  
- Apply authorization checks per user/resource  
- Use non-predictable identifiers (UUIDs)  
- Implement rate limiting  
- Monitor API usage  

---

## Website Header & Infrastructure Analysis
- **Request URL:** `https://jsonplaceholder.typicode.com/`  
- **Observed Headers:**  
  - `Server: cloudflare`, `Via: 2.0 heroku-router`, `X-Powered-By: Express` (information disclosure)  
  - CORS policy: `Access-Control-Allow-Credentials: true`  
  - Rate limiting headers: `X-Ratelimit-Limit: 1000`, `X-Ratelimit-Remaining: 999`  
- **Caching & Performance:** HIT via Cloudflare, cacheable for 12 hours, HTTP/3 supported  
- **Risks:**  
  - Low-level information disclosure through headers  
  - Missing security headers (CSP, HSTS, X-Content-Type-Options)  

**Recommendations:**  
- Disable `X-Powered-By` header  
- Add modern security headers to harden the API  

---

## API Documentation Review
- Reviewed endpoints, request methods, and data structures  
- Verified correct usage of API requests  
- Helped identify exposed fields and publicly accessible resources  

---

## Conclusion
The analysis demonstrated how API responses can be examined using browser developer tools and Postman to identify security gaps. Key observations include:  
- Unrestricted access without authentication or authorization  
- Multiple records exposed in a single response  
- Predictable identifiers facilitating resource enumeration  
- Certain headers revealing infrastructure information  

While JSONPlaceholder is a public test API with no real sensitive data, these findings highlight potential security risks in real-world APIs.  

**Key Takeaways:**  
- Implement strong authentication and access control  
- Limit data exposure and use non-predictable identifiers  
- Apply rate limiting and monitor API activity  
- Configure security headers to protect infrastructure  

---

