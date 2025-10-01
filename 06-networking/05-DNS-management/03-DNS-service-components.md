### **OCI Study Notes: DNS Service Components**

#### **1. Core Components of a DNS Zone**

**A. Domain**
*   **Definition:** A specific, delegated section of the DNS namespace on the internet.
*   **Example:** `oracle.com`
*   **Key Point:** It represents a complete part of the DNS tree that is under a user's or organization's control.

**B. Zone**
*   **Definition:** A contiguous portion of the DNS namespace that contains the DNS records for a specific domain and its subdomains (unless delegated away).
*   **Automatic Records:** When you create a zone in OCI, it automatically generates essential records, including:
    *   **Start of Authority (SOA) Record:** Contains administrative information about the zone (e.g., the primary name server, administrator's email, and timers for refreshing the zone).
    *   **Nameserver (NS) Records:** Specify the authoritative name servers for the zone.

**C. Label**
*   **Definition:** A string that is prepended to a zone name to form a more specific subdomain or hostname.
*   **Example:** In `www.oracle.com`, the string `www` is the label. It is combined with the zone name `oracle.com` to create the fully qualified domain name (FQDN).

**D. Child Zone (Delegation)**
*   **Definition:** An **independent subdomain** that has been delegated to its own set of authoritative name servers.
*   **Characteristics:**
    *   It has its own **SOA record** and **NS records**.
    *   It is managed separately from the parent zone.
*   **Requirement for Delegation:** The **parent zone must contain NS records** that point to the name servers responsible for the child zone. This "delegates" authority and tells recursive resolvers where to find the records for the subdomain.
    *   **Example:** The parent zone for `oracle.com` would contain NS records pointing to the name servers for the child zone `us.oracle.com`.

**E. Resource Records**
*   **Definition:** The entries within a DNS zone that contain specific mapping information.
*   **Purpose:** They provide the actual data that answers DNS queries.
*   **Common Types:**
    *   **A Record:** Maps a domain name to an **IPv4 address**.
    *   **AAAA Record:** Maps a domain name to an **IPv6 address**.
    *   **MX Record:** Specifies the **mail servers** responsible for receiving email for the domain.
    *   **CNAME Record:** Creates an alias from one domain name to another.