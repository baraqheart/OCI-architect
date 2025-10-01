### **OCI Study Notes: Load Balancer & WAF Services - Introduction**

#### **1. Overview of OCI Load Balancing Services**

Oracle Cloud Infrastructure provides two distinct types of load balancers to distribute traffic efficiently across multiple backend servers.

*   **Load Balancer:** Distributes application traffic based on Layer 7 (HTTP/HTTPS) and Layer 4 (TCP) protocols. It is designed for advanced traffic management of web applications.
*   **Network Load Balancer:** Distributes connection traffic based on Layer 4 (IP protocol) data. It is designed for ultra-high performance and low latency, handling non-HTTP(S) traffic.

#### **2. Deployment Models: Public vs. Private**

Both the Load Balancer and Network Load Balancer can be deployed in two modes:

*   **Public Load Balancer:** Has a public IP address and is accessible from the internet. It is typically placed in a public subnet within your VCN.
*   **Private Load Balancer:** Has a private IP address and is only accessible from within your VCN or connected networks (via FastConnect or VPN). It is placed in a private subnet.

#### **3. OCI Web Application Acceleration (WAF) Service**

*   **Purpose:** A service that works in conjunction with the Layer 7 **Load Balancer** to **speed up web application traffic**.
*   **Mechanism:** It uses a combination of **caching** (storing frequently accessed content) and **compression** (reducing the size of data transfers) to improve performance and reduce latency for end-users.
*   **Key Point:** This is an enhancement specifically for the Layer 7 Load Balancer, not the Network Load Balancer.