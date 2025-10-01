### **OCI Study Notes: Load Balancer Health Checks**

#### **1. Purpose of Health Checks**

Health checks are a critical mechanism that allows the load balancer to continuously monitor the status of its backend servers.

*   **Core Function:** To **confirm whether backend servers are available and healthy** to receive traffic.
*   **Analogy:** Similar to an employee health check. If an employee is unhealthy, a manager will not assign them work. Likewise, if a backend server fails its health check, the **load balancer stops sending traffic to it**, taking it "out of rotation" temporarily.
*   **Benefit:** This process is fundamental to providing **high availability and fault tolerance**, ensuring client requests are only handled by servers that are responsive.

#### **2. Health Check Protocols**

You can configure health checks using one of two methods, depending on your application's needs:

*   **TCP-level Health Check (Connection Attempt):**
    *   **How it Works:** The load balancer attempts to **establish a TCP connection** to the backend server on a specified port.
    *   **Determination of Health:** If the connection is successfully established, the server is considered healthy. This is a basic network-level check.
*   **HTTP-level Health Check (Request):**
    *   **How it Works:** The load balancer sends an **HTTP request** (e.g., a GET request) to a specific URI on the backend server.
    *   **Determination of Health:** The server is considered healthy only if it returns a **successful HTTP response code**, typically a `200 OK`.
    *   **Advantage:** This is an application-aware check, verifying that the application itself is running and responding correctly, not just that the port is open.

**Best Practice:** Select the health check protocol that matches your application's service (e.g., use HTTP checks for web servers, TCP for database connections).

#### **3. Health Check Configuration Parameters**

When setting up a health check policy, you configure the following key parameters:

**1. Protocol**
*   The choice between **HTTP** or **TCP**.

**2. Interval (in milliseconds)**
*   **Definition:** Specifies **how frequently** the load balancer runs the health check against each backend server.
*   **Default Value:** **10,000 milliseconds (10 seconds)**.

**3. Timeout (in milliseconds)**
*   **Definition:** The **maximum time the load balancer will wait for a reply** from the backend server after sending a health check request.
*   **How it Works:** If the server's response is received **within this timeout period**, the health check is considered successful. If the timeout is reached without a response, that health check attempt is logged as a failure.
*   **Example Default:** 3,000 milliseconds (3 seconds).

**4. Number of Retries**
*   **Definition:** The **number of consecutive failed health check attempts** required before a backend server is marked as **"unhealthy"**.
*   **How it Works:** The load balancer must fail to get a healthy response this many times in a row before it stops sending traffic to the server. This prevents a single network glitch from unnecessarily taking a server out of service.
*   **This also applies when recovering a server:** A server that was unhealthy must pass the health check for this many consecutive attempts to be marked "healthy" again and returned to rotation.
*   **Default Value:** **3 retries**.