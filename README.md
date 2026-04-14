# NoSQL Injection Attack on APIs: Coupon Manipulation Lab

## Author

Pamphyls Batila
Email:[ps.pbatilabatila@gmail.com](mailto:ps.pbatilabatila@gmail.com)
Location: Seattle, WA

---

## Overview

This project demonstrates a **NoSQL injection vulnerability** in a web application's API that allows attackers to **manipulate coupon codes and generate unauthorized discounts**.

The lab was completed as part of **CIS414 – Advanced Database Security** at Highline College and simulates a real-world API attack scenario using the vulnerable **crAPI application**.

---

## Objective

* Identify and exploit a **NoSQL injection vulnerability** in an API endpoint
* Manipulate coupon validation logic to generate unauthorized discounts
* Analyze API behavior using interception and fuzzing techniques
* Propose security mitigations to prevent similar attacks

---

## Tools & Technologies

* **Burp Suite** – HTTP interception and fuzzing (Intruder)
* **FoxyProxy** – Traffic redirection through Burp Suite
* **Postman** – API testing and request simulation
* **crAPI** – Vulnerable API application
* **Firefox Browser**

---

## Attack Summary

The application’s coupon system failed to properly validate user input, allowing injection of **NoSQL query operators** into API requests.

By modifying the coupon request payload, it was possible to:

* Bypass validation logic
* Retrieve valid coupon data
* Apply unauthorized discounts

---

## Step-by-Step Exploitation

### 1. Intercept API Request

* Enabled FoxyProxy to route traffic through Burp Suite
* Captured coupon request from the application

### 2. Identify Injection Point

* Located the `coupon code` field in the POST request body

### 3. Fuzz Input with Payloads

* Sent request to **Burp Intruder**
* Injected NoSQL payloads into the coupon field

Example payloads:

```
{"$gt":""}
{"$nin":[1]}
```

---

### 4. Analyze Responses

* Initial responses returned **500 errors**
* After modifying payload encoding:

  * Received **422 (syntax errors)** → confirmed injection point
* Adjusted payload placement within JSON structure

---

### 5. Successful Exploitation

* Received **HTTP 200 responses** indicating valid execution
* Extracted valid coupon data from API response

---

### 6. Impact

* Applied extracted coupon code (`TRAC075`)
* Balance increased from **$5100 → $5175**
* Demonstrated **unauthorized financial gain via API abuse**

---

## ⚠️ Vulnerability Details

The API failed to:

* Validate user input properly
* Sanitize NoSQL query operators
* Enforce strict schema validation

This allowed attackers to inject operators like:

* `$gt` (greater than)
* `$nin` (not in list)

---

## Security Impact

This vulnerability could allow attackers to:

* Manipulate financial transactions
* Bypass business logic
* Access or modify sensitive data
* Automate exploitation at scale

---

## Recommended Mitigations

* **Input Validation**

  * Strictly validate and sanitize all API inputs
  * Reject special operators in user-controlled fields

* **Use Parameterized Queries**

  * Avoid dynamic query construction

* **Schema Enforcement**

  * Enforce strict JSON schema validation

* **Authentication & Authorization**

  * Implement **OAuth 2.0**
  * Use **JWT tokens securely**

* **Encryption**

  * Enforce HTTPS (SSL/TLS)

* **API Security Best Practices**

  * Limit API responses
  * Implement rate limiting
  * Monitor abnormal behavior

---

## Key Takeaways

* Small input validation failures can lead to **critical business logic exploits**
* NoSQL databases are flexible—but require **strict input controls**
* API security must be treated as a **primary attack surface**

---

## Future Improvements

* Automate testing using Python scripts
* Expand payload testing with larger datasets
* Integrate findings into a detection/monitoring system (SIEM)

---

## Related Skills Demonstrated

* API Security Testing
* NoSQL Injection Exploitation
* Burp Suite (Intruder, Proxy)
* Threat Analysis & Vulnerability Assessment
* Incident Documentation & Reporting

---

## Disclaimer

This project was conducted in a controlled lab environment for educational purposes only. Do not attempt these techniques on systems without proper authorization.
