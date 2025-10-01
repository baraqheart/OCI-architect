### **OCI Study Notes: Traffic Management Steering Policy Types**

#### **1. Overview of Policy Types**

OCI Traffic Management provides five distinct steering policies to control how DNS traffic is routed:

1.  **Failover**
2.  **Load Balancer**
3.  **Geolocation**
4.  **ASN Steering**
5.  **IP Prefix Steering**

#### **2. Detailed Policy Breakdown**

**A. Failover Policy**
*   **Purpose:** To provide high availability and disaster recovery.
*   **How it Works:**
    *   You **prioritize endpoints** (e.g., Primary and Secondary).
    *   OCI health checks continuously monitor all endpoints.
    *   **Normal Operation:** All DNS traffic is steered to the healthy Primary endpoint.
    *   **During Failure:** If the Primary endpoint fails its health check, DNS traffic is **automatically and immediately steered to the Secondary** endpoint.

**B. Load Balancer Policy**
*   **Purpose:** To distribute traffic across multiple endpoints for scalability and efficient resource utilization.
*   **How it Works:**
    *   You assign a **weight** to each healthy endpoint (e.g., 50/50, 10/90).
    *   Traffic is distributed according to the specified weights.
    *   **Dynamic Adjustment:** If an endpoint becomes unhealthy, its traffic is automatically redistributed to the remaining healthy endpoints based on their weights.

**C. Geolocation Steering Policy**
*   **Purpose:** To route users to the nearest or a specific endpoint based on their geographic location.
*   **How it Works:** The policy directs DNS queries to a predefined endpoint based on the **user's country or geographic region**.
*   **Example:** Users in Australia are sent to an endpoint in the APAC region, while users in Germany are sent to an endpoint in the EU region.

**D. ASN Steering Policy**
*   **Purpose:** To route traffic based on the user's network provider.
*   **How it Works:** Routes DNS queries to a specific endpoint based on the **Autonomous System Number (ASN)** from which the query originated. An ASN typically identifies a large network, such as an ISP or a corporate network.

**E. IP Prefix Steering Policy**
*   **Purpose:** To route traffic based on the user's specific IP address range.
*   **How it Works:** Directs DNS queries to a specific endpoint based on the **IP prefix (subnet)** of the originating query. This allows for very granular control.

#### **3. Key Use Cases**

*   **Automated Disaster Recovery:** Using **Failover** policies to ensure application availability if a primary site goes down.
*   **Cloud Migration & Canary Testing:** Using **Load Balancer** policies with weights to gradually shift traffic (e.g., 10% to the new cloud environment, 90% to the old one) and validate performance before a full cutover.
*   **Global Performance Optimization:** Using **Geolocation** policies to direct users to the closest data center, reducing latency.
*   **Internal vs. External Routing:** Using **IP Prefix Steering** to send internal corporate users (from the company IP range) to a test version of an application, while directing all other external traffic to the production version.
*   **Zero-Rating Services:** Using **ASN Steering** to direct users from a specific mobile operator (identified by its ASN) to free, sponsored resources, while directing all other users to standard, paid resources.

#### **4. Important Service Characteristics**

*   **Cloud Agnostic:** Traffic Management can steer traffic to **any publicly accessible resource**, including:
    *   OCI resources
    *   Other cloud providers (AWS, Azure, GCP)
    *   On-premises data centers
*   **Requirement:** The target endpoints **must be resolvable on the public internet**.