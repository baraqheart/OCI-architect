### **OCI Study Notes: HTTP Redirects in DNS**

#### **1. Core Functionality**

*   The OCI DNS service includes a feature to create **HTTP redirects**, which automatically send web traffic from one domain or URL to another.
*   This is a **one-to-one mapping**, meaning a single source is redirected to a single target.

#### **2. Supported Redirect Scenarios**

You can configure HTTP redirects for the following common use cases:

1.  **Redirect an Entire Domain:**
    *   **Example:** Redirect all HTTP traffic for `test.net` to `test.com`.

2.  **Redirect a Specific Subdomain:**
    *   **Example:** Redirect traffic for `subdomain.test.net` to a specific HTTP URL.

3.  **Redirect to a URL with a Specific Port:**
    *   **Example:** Redirect `example.test.net` to `http://sample.test.net:8080`.

4.  **Permanent Redirect (301 Response Code):**
    *   **Use Case:** To inform search engines and browsers that a domain or page has been **permanently moved**.
    *   **Benefit:** Helps preserve search engine rankings for the new location and instructs browsers to cache the redirect.

#### **3. Implementation Requirement**

*   For the HTTP redirect to function, you **must create a CNAME record** in the source DNS zone that points to the OCI redirect service. The console often provides an option to create this record automatically during the redirect setup.

#### **4. Configuration Parameters**

When creating a redirect, you configure:

*   **Source:** The original domain or subdomain.
*   **Target:** The destination URL, including:
    *   Protocol (HTTP/HTTPS)
    *   Host
    *   Port (optional)
    *   Path and Query (optional)
*   **Response Code:** Choose between **301 (Moved Permanently)** or **302 (Found / Temporary Redirect)**.