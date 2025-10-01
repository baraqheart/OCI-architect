### **OCI Study Notes: Load Balancer Policies - Deep Dive**

#### **1. Overview of Load Balancing Policies**

There are three primary policies that determine how a load balancer distributes incoming traffic among backend servers:

1.  **Round Robin (Default)**
2.  **Least Connections**
3.  **IP Hash**

An additional feature, **backend server weighting**, can be combined with these policies to influence distribution further.

**Critical Concept:** The selected policy is applied differently depending on the protocol and session type. The behavior varies for:
*   **TCP Load Balancer**
*   **HTTP with Sticky Sessions (Session Persistence)**
*   **HTTP with Non-Sticky Sessions**

---

#### **2. Policy Application by Protocol and Session Type**

**A. TCP Load Balancer**
*   **Initial Request:** The load balancer uses the defined policy (and weight, if set) to select a backend server for the **first packet** of a new connection.
*   **Subsequent Packets:** All **subsequent packets on that same connection are automatically sent to the same backend server** that was chosen for the initial packet. The policy is not re-evaluated for the life of the TCP connection.

**B. HTTP with Sticky Sessions (Session Persistence)**
*   **All Requests:** The load balancer **ignores the load balancing policy** for requests that are part of a sticky session.
*   **Routing Method:** Traffic is directed to the **backend server specified by the session cookie**. The cookie, either set by the application or the load balancer, ensures all requests from a user session go to the same server.

**C. HTTP with Non-Sticky Sessions**
*   **Every Request:** The load balancer applies the defined policy (and weight, if set) to **each individual incoming HTTP request**.
*   **Result:** Different requests from the same client can be, and often are, sent to different backend servers.

---

#### **3. Detailed Policy Analysis**

**Policy 1: Round Robin (Default)**
*   **How it Works:** Distributes requests **sequentially** to each server in the list, one after the other. After sending to the last server, it starts again from the first.
*   **Ideal Use Cases:**
    *   When all **backend servers have similar or identical hardware capacity** (CPU, Memory). It is not efficient if one server is more powerful than others.
    *   When the **processing load or time required for each request does not vary significantly**. It is not suitable if one request takes 50 seconds to process and the next takes 10 seconds, as it does not account for the actual load on the server.

**Policy 2: Least Connections**
*   **How it Works:** Directs new requests to the backend server that currently has the **fewest number of active connections**.
*   **Key Feature:** This policy is more dynamic and responsive to the current load on each server compared to Round Robin.
*   **Weighted Option:** You can assign a **weight** to each backend server. A server with a higher weight (e.g., 3) will be considered to have fewer connections than it actually does, making it receive more traffic relative to a server with a lower weight (e.g., 1).

**Policy 3: IP Hash**
*   **How it Works:** Uses a **hashing algorithm on the client's source IP address** to determine which backend server receives the request.
*   **Key Benefit:** Ensures that **all non-sticky traffic from a specific client IP is consistently routed to the same backend server**. This provides a form of session affinity without using cookies.
*   **Important Distinction:** This policy **only affects non-sticky traffic**. If session persistence (sticky sessions) is enabled, the cookie-based method takes precedence.