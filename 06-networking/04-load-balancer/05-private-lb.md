### **OCI Study Notes: Private Load Balancer**

#### **1. Core Definition and Use Case**

*   **Purpose:** A private load balancer is used when you need to **isolate your application from the public internet** to improve security.
*   **Accessibility:** It is **only accessible from within your Virtual Cloud Network (VCN)** and networks connected to it (e.g., via FastConnect or Site-to-Site VPN). It is not reachable from the public internet.
*   **Subnet Requirement:** It **must be created within a private subnet**.

#### **2. Key Characteristics**

*   **IP Address:** It is assigned a **private IP address** from the hosting subnet's CIDR block. This private IP serves as the entry point for all incoming traffic.
*   **High Availability (HA) Subnet Requirement:** Unlike a public load balancer which may require two subnets, a private load balancer **only requires a single subnet** to host its entire high-availability pair (both the primary and standby instances).

#### **3. High Availability and IP Address Consumption**

The private load balancer is also deployed as a highly available pair, but it uses a floating *private* IP address instead of a public one.

*   **IP Address Requirements:** A private load balancer consumes **three private IP addresses** from its host subnet:
    1.  One private IP for the **primary (active)** load balancer instance.
    2.  One private IP for the **standby** load balancer instance.
    3.  One **floating private IP address** that is always associated with the active instance.
*   **Automatic Failover:** Similar to the public load balancer, if the active instance fails, the **floating private IP address automatically moves to the standby instance**, ensuring continuous service availability for clients within the private network.