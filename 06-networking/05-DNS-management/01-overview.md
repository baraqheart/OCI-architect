### **OCI Study Notes: DNS Management - Module Introduction**

#### **1. Core DNS Service Components**

This module covers the fundamental building blocks of the Domain Name System (DNS) and how they are implemented in OCI.

*   **Name Servers:** Specialized servers that store DNS records and answer queries about domain names.
    *   **Authoritative Name Server:** The ultimate source of truth for a specific domain. It holds the definitive DNS records (like A, CNAME, MX) for that domain and answers queries directly from its own data.
    *   **Recursive Resolver (Non-Authoritative Name Server):** A server that acts like a librarian. It doesn't store the original records but fetches them from authoritative name servers on behalf of a client, caching the results for a period of time.

*   **Zones:** A portion of the DNS namespace that is managed by a specific organization or administrator. It contains all the DNS records for a domain and its subdomains (unless delegated away).

*   **DNS Delegation:** The process of assigning responsibility for a subdomain (e.g., `region1.abc.com`) to a different set of name servers. This allows different teams or services to manage their own DNS segments.

#### **2. OCI DNS Zone Types**

OCI provides two distinct types of DNS zones to manage name resolution in different contexts:

*   **Public DNS Zone:** Used to manage DNS records for domains that are accessible from the **public internet**. When you create a public zone, OCI provisions authoritative name servers that are reachable globally. This is used for your public-facing websites and services.

*   **Private DNS Zone:** Used to manage DNS records for domains that are **only accessible from within your VCN and connected private networks** (via FastConnect or VPN). This allows you to use custom domain names for your internal resources (e.g., `database.private.corp`) without exposing them to the internet.

#### **3. Traffic Management Steering Policies**

Beyond basic DNS, OCI offers advanced traffic management capabilities to control how traffic is distributed to your endpoints. This module will cover policies such as:

*   **Failover:** Directs traffic to a primary endpoint while having a secondary backup endpoint on standby. If the primary endpoint fails health checks, traffic is automatically routed to the secondary endpoint. This is crucial for high availability and disaster recovery.