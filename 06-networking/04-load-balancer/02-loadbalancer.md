### **OCI Study Notes: Load Balancer Core Concepts - Part 1**

#### **1. What is a Load Balancer?**

A load balancer is a networking device that acts as a single entry point for client traffic and distributes that traffic across multiple backend servers.

*   **Function:** Sits between clients and backend servers, distributing incoming requests based on a defined policy.
*   **Visualization:**
    *   Client -> Load Balancer -> [Backend Server 1, Backend Server 2, Backend Server 3]

#### **2. Key Benefits of Using a Load Balancer**

1.  **Scaling (Elasticity):** You can easily increase or decrease the number of backend servers behind the load balancer to handle changes in traffic volume without disrupting client access.
2.  **Resource Utilization:** The load balancer uses policies to distribute traffic evenly, ensuring that no single backend server is overwhelmed while others are idle. This leads to optimal use of your compute resources.
3.  **High Availability:** The load balancer continuously monitors the health of backend servers. If a server fails its health check, the load balancer **automatically stops sending traffic to it** and continues to distribute requests to the remaining healthy servers. This ensures the application remains available even with individual server failures.

#### **3. Types of Load Balancers (By Accessibility)**

*   **Public Load Balancer:**
    *   Assigned a **public IP address**.
    *   Accessible from the **public internet**.
*   **Private Load Balancer:**
    *   Assigned a **private IP address** from the subnet where it is deployed.
    *   Only accessible from within your **Virtual Cloud Network (VCN)** and networks connected to it (via FastConnect or VPN).

#### **4. Core Load Balancer Components & Concepts**

**1. Backend Server**
*   **Definition:** An individual compute instance that is responsible for **generating content** and processing requests (e.g., a web server, application server).
*   **Role:** These are the final destinations for traffic that the load balancer distributes.

**2. Backend Set**
*   **Definition:** A **logical grouping** of backend servers.
*   **Composition:** A backend set is not just a list of servers. It is defined by three things:
    *   The **list of backend servers**.
    *   A **health check policy** (to monitor server status).
    *   A **load balancing policy** (to determine how traffic is distributed).

**3. Health Check Policy**
*   **Purpose:** A test to **determine if a backend server is available and healthy** to receive traffic.
*   **How it Works:** The load balancer periodically sends a test request to each server in the backend set. If a server fails to respond correctly, it is taken "out of rotation."
*   **Health Check Methods:**
    *   **TCP-level Check:** The load balancer attempts to establish a TCP connection to the backend server. Success means the server is healthy.
    *   **HTTP-level Check:** The load balancer sends an HTTP request to a specific URI on the backend server. It validates the response code (e.g., a 200 OK) to determine health. This is more application-aware.

**4. Listener**
*   **Purpose:** A process that **"listens" for incoming traffic** on the load balancer's IP address.
*   **Configuration:** To set up a listener, you must specify:
    *   A **Protocol** (e.g., HTTP, HTTPS, HTTP/2, TCP).
    *   A **Port Number** (e.g., 80 for HTTP, 443 for HTTPS).
*   **Rule:** You need to configure **at least one listener for each type of traffic** you want the load balancer to handle (e.g., one for HTTP on port 80 and another for HTTPS on port 443).



### **OCI Study Notes: Load Balancer Core Concepts - Part 2**

#### **5. Load Balancing Policies**

A load balancing policy defines the algorithm used to distribute incoming traffic among the backend servers in a backend set.

*   **1. Round Robin (Default Policy):**
    *   **How it Works:** Distributes incoming requests **sequentially** to each server in the backend set in a repeating order (Server 1, then Server 2, then Server 3, then back to Server 1).
*   **2. Least Connections:**
    *   **How it Works:** Directs new requests to the backend server that currently has the **fewest number of active connections**. This helps keep the load evenly distributed based on current server load.
*   **3. IP Hash:**
    *   **How it Works:** Uses the **client's source IP address** as a key in a hashing algorithm to determine which backend server receives the request.
    *   **Key Benefit:** This ensures that **all non-sticky traffic from a specific client IP is consistently sent to the same backend server**.

**Important Note:** The behavior and applicability of these policies can vary depending on the protocol (TCP vs. HTTP) and the use of session persistence (sticky sessions).

#### **6. Load Balancer Shape (Bandwidth)**

OCI Load Balancers use a **flexible shape** model, allowing you to specify a bandwidth range for automatic scaling.

*   **What it Defines:** The **bandwidth** (throughput) of the load balancer.
*   **Minimum Bandwidth (e.g., 10 Mbps):**
    *   **Significance:** Guarantees **instant readiness** for a baseline load. This minimum capacity is always provisioned.
*   **Maximum Bandwidth (e.g., 3000 Mbps):**
    *   **Significance:** Places a **cap on scaling and cost**. The load balancer will not scale beyond this limit.
*   **Allowed Range:** You can specify a bandwidth range from a **minimum of 10 Mbps to a maximum of 8,000 Mbps**.

#### **7. SSL/TLS Handling Options**

The load balancer provides several methods for managing encrypted HTTPS traffic.

*   **1. SSL Termination:**
    *   **Process:** The load balancer **terminates (decrypts)** the incoming SSL/TLS connection from the client.
    *   **Traffic Flow:** Communication between the **client and load balancer is encrypted**. Communication between the **load balancer and backend servers is unencrypted**.
    *   **Use Case:** Offloads the CPU-intensive decryption work from backend servers.
*   **2. End-to-End SSL (Point-to-Point SSL):**
    *   **Process:** The load balancer terminates the client SSL connection, **then initiates a new, separate SSL connection** to the backend server.
    *   **Traffic Flow:** Communication is **encrypted from the client to the load balancer AND from the load balancer to the backend server**.
    *   **Use Case:** Required when security policies mandate encryption for the entire path, including the private network between the load balancer and backend servers.
*   **3. SSL Tunneling:**
    *   **Process:** The load balancer does not decrypt the traffic. Instead, it **passes the encrypted SSL stream directly through to the backend server**.
    *   **Traffic Flow:** The backend server is responsible for terminating the SSL connection.
    *   **Use Case:** Used when the backend application requires the original, unaltered client SSL connection.

#### **8. Session Persistence (Sticky Sessions)**

*   **Definition:** A feature that ensures **all requests from a single client during a session are sent to the same backend server**.
*   **Use Cases:** Essential for applications that maintain user state on the server, such as **shopping carts** or **user login sessions**.
*   **Implementation Methods:**
    *   **Application Cookie Stickiness:** The backend application sets a cookie to identify the session.
    *   **Load Balancer Cookie Stickiness:** The load balancer itself injects a cookie to manage the session affinity.

#### **9. SSL Certificates**

*   **Requirement:** When you configure an **HTTPS listener**, you must associate an **SSL certificate** with the load balancer.
*   **Purpose:** This certificate is used by the load balancer to **terminate the SSL connection**, authenticate itself to the client, and decrypt the incoming requests.

#### **10. Advanced Routing Policies**

The Layer 7 Load Balancer can route traffic based on the content of the HTTP request.

*   **Path-Based Routing:**
    *   **How it Works:** Directs traffic to different backend sets based on the **URL path** in the request.
    *   **Example:**
        *   `www.abc.com/app` -> Backend Set A (Application Servers)
        *   `www.abc.com/videos` -> Backend Set B (Video Servers)
        *   `www.abc.com/images` -> Backend Set C (Image Servers)
*   **Host-Based Routing:**
    *   **How it Works:** Directs traffic to different backend sets based on the **hostname** in the HTTP header.
    *   **Example:**
        *   `abc.example.com` -> Backend Set X
        *   `xyz.example.com` -> Backend Set Y


---

### **OCI Study Notes: Load Balancer - Review & Architectures**


#### **1. OCI Load Balancer: Definition & Recap**

The OCI Load Balancer is a service that acts as a single point of contact for clients, distributing incoming network traffic across multiple backend servers to ensure high availability, efficient resource utilization, and scalability.

**Key Advantages:**
*   **Resource Utilization:** Efficiently distributes load to prevent any single server from being overwhelmed.
*   **Scaling:** Allows for easy addition or removal of backend servers to handle changing traffic loads.
*   **Fault Tolerance & High Availability:** Automatically detects unhealthy servers and reroutes traffic to healthy ones, ensuring application continuity.

#### **2. Types and Protocols**

*   **Accessibility Types:**
    *   **Public Load Balancer:** Has a public IP and is accessible from the internet. Deployed in a public subnet.
    *   **Private Load Balancer:** Has a private IP and is only accessible from within the VCN or connected private networks. Deployed in a private subnet.
*   **Supported Protocols (Layer 4 and Layer 7):**
    *   **HTTP**
    *   **HTTPS**
    *   **HTTP/2**
    *   **TCP**

#### **3. Feature Summary**

The OCI Load Balancer supports the advanced features discussed in detail previously:
*   **Session Persistence** (Sticky Sessions)
*   **Path-Based Routing**
*   **Host-Based Routing**
*   **SSL Handling:** SSL Termination, End-to-End (Point-to-Point) SSL, and SSL Tunneling.

#### **4. Load Balancer Shape: Flexible Model**

*   **Current Model:** OCI now uses **flexible shapes** for load balancers. The older dynamic shape is no longer available for new deployments.
*   **Configuration:** You must specify a bandwidth range:
    *   **Minimum Bandwidth (10 Mbps - 8,000 Mbps):** Guarantees **instant readiness** for a baseline level of traffic. This capacity is always available.
    *   **Maximum Bandwidth (10 Mbps - 8,000 Mbps):** Places an upper limit on scaling to **control costs**. The load balancer will not scale beyond this specified maximum.

#### **5. Advanced Routing Architectures**

**A. Host-Based Routing (Content-Based Routing)**
*   **Concept:** A **single load balancer** can route traffic to different backend sets based on the **hostname** specified in the HTTP request header.
*   **Architecture:** The load balancer is deployed in a public subnet. It examines the incoming request's `Host` header.
*   **Example:**
    *   A request for `host1.domain1.com` is sent to **Backend Set 1**.
    *   A request for `host2.domain2.com` is sent to **Backend Set 2**.
*   **Benefit:** Allows you to load balance multiple different websites or services using a single, centralized load balancer.

**B. Path-Based Routing**
*   **Concept:** A single load balancer routes traffic to different backend sets based on the **URL path** of the request.
*   **Architecture:** The load balancer examines the path after the domain name in the request.
*   **Example:**
    *   A request to `www.abc.com/app` is sent to **Backend Set 1**.
    *   A request to `www.abc.com/videos` is sent to **Backend Set 2**.
*   **Benefit:** **Optimizes resource utilization** by directing specific types of requests (e.g., application logic, video streaming, image serving) to pools of servers specialized for those tasks.