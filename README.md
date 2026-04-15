# API Security Assessment: NoSQL Injection in Coupon Validation Systems

> ⚠️ This project was conducted in a controlled lab environment for educational and security research purposes only.

---

## Overview

This project presents a security assessment of an API vulnerable to **NoSQL injection**, conducted using the intentionally vulnerable **crAPI application**. The objective was to evaluate how improper input validation in API endpoints can lead to **business logic exploitation and unauthorized financial impact**.

The assessment simulates a real-world attack scenario in which an adversary manipulates API requests to bypass coupon validation and generate fraudulent discounts.

---

## Objectives

* Identify injection points in API request handling
* Exploit NoSQL injection vulnerabilities in coupon validation logic
* Analyze API response behavior to confirm exploitation
* Assess the security and business impact of the vulnerability
* Recommend mitigation strategies aligned with secure API design

---

## Tools & Technologies

* **Burp Suite** – HTTP interception, request manipulation, fuzzing (Intruder)
* **FoxyProxy** – Traffic routing through interception proxy
* **Postman** – API testing and simulation
* **crAPI Application** – Vulnerable test environment
* **Firefox Browser**
* **NoSQL Payloads** – `$gt`, `$nin`, `$ne`, `$regex`

---

## Attack Summary

The API’s coupon validation mechanism failed to properly validate and sanitize user input. This allowed injection of **NoSQL query operators** into API requests.

By manipulating the coupon parameter, it was possible to:

* Bypass validation logic
* Retrieve valid coupon data
* Apply unauthorized discounts

---

## Methodology

### 1. Interception & Traffic Analysis

Traffic was routed through Burp Suite using FoxyProxy, allowing interception of coupon-related HTTP requests.

### 2. Request Inspection

The POST request body was analyzed to identify the `coupon` parameter as a potential injection point.

### 3. Payload Injection & Fuzzing

Burp Suite Intruder was used to test multiple SQL and NoSQL payloads against the coupon field.

Example payloads:

```
{"$gt":""}
{"$nin":[1]}
```

### 4. Response Analysis & Refinement

Different payload configurations produced distinct server responses:

* **HTTP 500** → improper error handling
* **HTTP 422** → syntax validation failure
* **HTTP 200** → successful execution

Payload encoding and placement were adjusted to achieve successful injection.

### 5. Exploitation

Successful payloads returned valid coupon data. Extracted coupon codes were applied to the application, resulting in unauthorized account balance increases.

---

## Key Findings

* The API failed to properly validate and sanitize user input
* NoSQL operators bypassed coupon validation logic
* Response patterns (500 → 422 → 200) revealed exploitable conditions
* Sensitive coupon data was exposed via API responses
* Business logic could be manipulated for financial gain

---

## Evidence of Exploitation

The full report includes screenshots demonstrating:

* Intercepted HTTP requests in Burp Suite
* Intruder payload configuration and fuzzing results
* Response variations indicating successful injection
* Extracted coupon data from API responses
* Successful exploitation (balance increase from **$5100 → $5175**)

📄 Full detailed report:
`/docs/NoSQL-Injection-Lab-Report.pdf`

---

## Security Impact

This vulnerability introduces significant risks:

* **Unauthorized Access** – Retrieval of valid coupon data
* **Business Logic Abuse** – Fraudulent financial transactions
* **Data Exposure** – Leakage of sensitive API responses
* **Scalability of Attacks** – Potential for automated exploitation

---

## Recommended Mitigations

### Input Security

* Enforce strict input validation and sanitization
* Reject NoSQL operators in user-controlled fields

### Secure Development

* Use parameterized queries or ORM frameworks
* Implement strict API schema validation (JSON schema)

### Authentication & Transport Security

* Apply OAuth 2.0 for authorization
* Use secure JWT handling
* Enforce HTTPS/TLS encryption

### API Hardening

* Limit API response data exposure
* Implement rate limiting and monitoring
* Detect anomalous request patterns

---

## Research Perspective: Input Manipulation & System Behavior

This project highlights how small variations in input structure can significantly impact system behavior.

Observed response progression:

* HTTP 500 → system instability
* HTTP 422 → input validation boundary
* HTTP 200 → successful exploitation

Rather than a single failure point, the vulnerability emerged through **patterns of system responses**.

This concept is highly relevant to **AI security**, where adversarial inputs can produce unintended or unsafe outputs. Understanding these behavioral patterns is critical for building resilient systems.

---

## Skills Demonstrated

* API Security Testing
* NoSQL Injection Exploitation
* Burp Suite (Proxy, Intruder)
* Traffic Analysis & Request Manipulation
* Vulnerability Assessment & Reporting
* Adversarial Testing & Pattern Analysis

---

## Future Improvements

* Automate injection testing using Python scripts
* Expand testing across additional API endpoints
* Integrate detection logic into SIEM systems
* Explore parallels between API injection and adversarial AI inputs

---
