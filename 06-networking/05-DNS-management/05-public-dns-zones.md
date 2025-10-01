### **OCI Study Notes: Public DNS Zone Implementation**

#### **1. What is a Public DNS Zone?**

*   **Purpose:** A Public DNS Zone is used to **resolve hostnames on the public internet**.
*   **Function:** It hosts the **authoritative (trusted) DNS records** for your public domain. These records reside on and are served by OCI's global name servers.

#### **2. The Three-Step Implementation Process**

To make a domain publicly resolvable using OCI DNS, you must follow these three steps in order.

**Step 1: Create the Public DNS Zone in OCI**
*   **Action:** In the OCI console, create a public DNS zone. The zone name must **exactly match** your registered domain (e.g., `samvit.site`).
*   **Automatic Output:** Upon creation, OCI automatically generates two critical records for the zone:
    *   **Name Server (NS) Records:** A list of OCI's authoritative name servers for your zone.
    *   **Start of Authority (SOA) Record:** Administrative metadata for the zone.

**Step 2: Delegate the Zone at Your Domain Registrar**
*   **What is Delegation?** The process of telling the global DNS system that OCI's name servers are now the **authoritative source** for your domain's DNS records. This makes your OCI-hosted zone accessible from the internet.
*   **Action:**
    1.  Copy the list of **NS records** provided after you created the zone in OCI.
    2.  Log in to your domain registrar's control panel (e.g., GoDaddy, Namecheap).
    3.  Replace the existing default name servers with the **OCI name servers** you copied.
*   **Result:** After this change propagates (which can take up to 48 hours, but usually less), all DNS queries for your domain will be directed to OCI's DNS service.

**Step 3: Add Resource Records to the Zone**
*   **Action:** Once the zone is delegated, you add the specific DNS records inside your OCI zone that map names to resources.
*   **Record Data (RData):** This is the essential information contained within a DNS record.
    *   **Example for an A Record:** The "RData" is the **IPv4 address** (e.g., `192.0.2.1`) that the domain name should resolve to.
*   **Common Record Types:** You can add A records (for IPv4), AAAA records (for IPv6), CNAME records (aliases), MX records (for mail servers), etc.