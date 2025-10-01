
### **OCI Study Notes: Network Load Balancer (NLB)**

#### **1. Core Definition and Operation**

*   **What it is:** A load balancing service that operates at the **connection level (Layer 3 and Layer 4)** of the OSI model.
*   **Key Characteristic:** It is a **pass-through** load balancer. This means it **preserves the source and destination IP addresses and ports** of the original client packet. The backend servers see the client's actual IP address.

#### **2. Performance and Scalability**

*   **High Throughput & Ultra-Low Latency:** The NLB is engineered to provide maximum throughput while maintaining extremely low latency. This makes it ideal for **latency-sensitive workloads**.
*   **Handles Volatile Traffic:** Its architecture is designed to efficiently handle sudden, large bursts of traffic.
*   **Elastic Scalability:** The NLB **scales automatically** up and down based on the volume of client traffic.
*   **No Bandwidth Configuration:** Unlike the Layer 7 Load Balancer, you **do not need to configure a bandwidth shape (minimum/maximum)**. The scaling is fully managed by OCI.

#### **3. Load Balancing Policies**

The Network Load Balancer uses hash-based policies to determine how to distribute connections to backend servers.

*   **5-Tuple Hash (Default Policy):**
    *   Routes traffic based on a hash of **five elements**: Source IP, Source Port, Destination IP, Destination Port, and Protocol.
    *   This provides the most granular level of distribution, ensuring a consistent backend for a specific connection.

*   **3-Tuple Hash:**
    *   Routes traffic based on a hash of **three elements**: Source IP, Destination IP, and Protocol.

*   **2-Tuple Hash:**
    *   Routes traffic based on a hash of **two elements**: Source IP and Destination IP.