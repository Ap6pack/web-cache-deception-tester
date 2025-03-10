# **Web Cache Deception Testing Tool**
A Python tool for testing **Web Cache Deception (WCD) vulnerabilities** in **web applications and RESTful APIs**. It automates checking for improperly cached sensitive data by testing common **static file extensions**, **cache delimiters**, and **API-specific caching mechanisms**.

## **Features**
✅ **Tests for Web Cache Deception vulnerabilities**  
✅ **Supports both Web Applications and RESTful APIs**  
✅ **Detects Leaked JSON/XML Data in API responses**  
✅ **Checks API-Specific Caching Headers (`ETag`, `Last-Modified`)**  
✅ **Tests Static Extensions (`.css`, `.jpg`, `.txt`, etc.)**  
✅ **Tests Common Web Cache Delimiters (`;`, `@`, `#`, `?`, `!`, etc.)**  
✅ **Fully Configurable via CLI** – No need to modify the script  
✅ **Real-time feedback with `rich` formatting**

---

## **Installation**
### **1. Clone the Repository**
```sh
git clone https://github.com/Ap6pack/web-cache-deception-tester.git
cd web-cache-deception-tester
```

### **2. Install Required Dependencies**
Ensure you have Python **3.7+** installed, then install the required libraries:
```sh
pip install requests rich argparse
```

---

## **Usage**
Run the tool using the command-line interface.

### **Basic Test (Web Application)**
```sh
python cache_deception_test.py --url "https://example.com/user/profile" --cookie "session=YOUR_SESSION_COOKIE"
```
This tests if **user-specific content is being cached** and exposed to other users.

### **API Mode (RESTful API Testing)**
```sh
python cache_deception_test.py --url "https://api.example.com/user/data" --cookie "session=YOUR_SESSION_COOKIE" --api
```
This enables **API-specific testing**, including JSON response checks and API caching headers (`ETag`, `Last-Modified`).

### **Verbose Mode (Detailed Output)**
```sh
python cache_deception_test.py --url "https://example.com/user/profile" --cookie "session=YOUR_SESSION_COOKIE" --verbose
```
This provides **detailed logs and debugging information**.

---

## **How It Works**
1. **Tests Original Page/API Response**  
   - Checks if **sensitive data is returned** in the original response.  
   - Extracts `Cache-Control`, `ETag`, and `Last-Modified` headers.

2. **Tests Static Extensions (Tricking Cache Mechanisms)**  
   - Appends `.css`, `.jpg`, `.txt`, etc., to see if user-specific data is stored in cache.

3. **Tests Common Web Cache Delimiters**  
   - Injects **`;`, `@`, `#`, `?`, `!`, etc.** into URLs to trigger caching misconfigurations.

4. **Cross-User Exposure Testing**  
   - Checks if another user (unauthenticated session) can access cached sensitive data.

5. **Generates a Final Security Report**  
   - Summarizes which URLs are **vulnerable to Web Cache Deception**.

---

## **Example Output**
```
Starting Web Cache Deception Test on: https://example.com/user/profile

Testing original user page...
✔ Sensitive data detected in normal page/API response (expected behavior).
Cache-Control Header: private, no-cache

Testing static extensions for cache deception...
⚠ Warning: https://example.com/user/profile.css has caching enabled!
❗ Vulnerable: Sensitive data found at https://example.com/user/profile.css

Testing cache delimiters...
⚠ Warning: https://example.com/user/profile;cache has caching enabled!
❗ Vulnerable: Sensitive data found at https://example.com/user/profile;cache

Testing for cross-user exposure...
❗ CRITICAL: Cached user data is exposed at https://example.com/user/profile.css

Final Report:
┌──────────────────────────────────────────────┬─────────────┐
│ Tested URL                                  │ Vulnerable  │
├──────────────────────────────────────────────┼─────────────┤
│ https://example.com/user/profile.css        │ YES        │
│ https://example.com/user/profile;cache      │ YES        │
└──────────────────────────────────────────────┴─────────────┘

❗ Warning: Some endpoints are vulnerable. Implement proper cache-control headers.
```

---

## **How to Fix Web Cache Deception**
If a **Web Cache Deception vulnerability is detected**, apply these fixes:

### **1. Use Strict `Cache-Control` Headers**
```http
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
```
This prevents sensitive content from being cached.

### **2. Implement `Vary` Headers**
```http
Vary: Authorization, Cookie
```
Ensures the cache does not serve personalized content to unauthorized users.

### **3. Configure CDNs and Proxies**
- Ensure **CDNs do not cache dynamic content** like user profiles.
- Define strict **caching rules** in reverse proxies.

---

## **Contributing**
- Fork the repository and submit a **pull request**.
- Report issues via **GitHub Issues**.

---

## **License**
MIT License. You are free to modify and distribute this tool.

---

## **Disclaimer**
⚠️ **Use this tool for ethical security testing only!** Do not test **without proper authorization**. The author is not responsible for misuse. 🚨
