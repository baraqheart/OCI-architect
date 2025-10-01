### **OCI Study Notes: FastConnect Redundancy & High Availability Best Practices**

#### **1. Oracle's Built-in Redundancy**

Oracle designs its FastConnect service with multiple layers of inherent redundancy to ensure high availability.

*   **Multiple Partners per Region:** For any given OCI region, there are multiple Oracle Partners available, preventing a single point of failure at the provider level.
*   **Redundant Physical Links to Partners:** Between each Oracle Partner and Oracle's own network, there are **multiple, separate physical connections**.
*   **Multiple FastConnect Locations:** Most OCI regions are served by **more than one physical FastConnect location**. This protects against a failure of an entire data center.
*   **Redundant Routers per Location:** Within a single FastConnect location, there is a **minimum of two independent FastConnect routers (edge devices)**.

#### **2. Customer Responsibility for End-to-End Redundancy**

While Oracle ensures redundancy within its cloud infrastructure, you are responsible for the resilience of your connection *to* that infrastructure. The goal is to eliminate every single point of failure.

**General Rule:** The customer must ensure that the physical connection from their network to the Oracle network is redundant. The specific implementation depends on the connectivity model.

#### **3. Redundancy with an Oracle Partner**

When using an Oracle Partner, redundancy is achieved by connecting to **different physical Oracle routers**.

*   **Oracle's Responsibility:** Handles the redundancy of the physical connections between the partner and Oracle, and the redundancy of routers within FastConnect locations.
*   **Your Responsibility:** Ensure your connection to the partner's network is redundant.

There are two partner connectivity models, each with different redundancy implications:

**A. Layer 2 Connection (BGP to Oracle Directly)**
*   The BGP session is established directly between your on-premises router and Oracle's routers.
*   **Redundancy Method:** You must create **at least two virtual circuits**. Each virtual circuit **must terminate on a different physical Oracle router** within the same or, ideally, different FastConnect locations.
*   **Visual:** One virtual circuit uses the blue line to Router 1; a second virtual circuit uses the red line to a Router 2 in a different location.

**B. Layer 3 Connection (BGP to the Partner)**
*   The BGP session is between your router and the partner's router. The partner then has its own BGP sessions with Oracle.
*   **Redundancy Method:** The partner often provides an **automatically redundant and diverse** connection to Oracle due to their pre-existing multiple physical links.
*   **Your Responsibility:** You must ensure that the connection from your data center to the partner's network is redundant (e.g., dual links).

#### **4. Redundancy with Third-Party Provider or Co-location**

For FastConnect Direct models (Third-Party Provider and Co-location), the principle is the same: **diversity at the physical layer**.

*   **Oracle's Responsibility:** Handles the redundancy of the routers within the FastConnect locations.
*   **Your Responsibility:** You are fully responsible for creating redundant physical connections to Oracle.

**Implementation:**
*   You must provision **two separate cross-connects**.
*   These cross-connects should ideally terminate in **two different FastConnect locations**. If that is not possible, they must, at a minimum, terminate on the **two different Oracle edge devices** within the same location.
*   You then create a **virtual circuit over each independent physical cross-connect**.

#### **5. Using Site-to-Site VPN as a Backup for FastConnect**

A common and cost-effective strategy is to use the Site-to-Site VPN service as a backup for your primary FastConnect connection.

*   **Architecture:** Your primary, high-bandwidth connection is via FastConnect (blue line). Your backup connection is via a Site-to-Site IPsec VPN (red dotted line) over the public internet.
*   **Configuration:** You configure **at least one of the two redundant VPN tunnels**.
*   **Routing:** OCI's dynamic routing (BGP) will automatically prefer the FastConnect path when it is available because it has a higher administrative preference. If the FastConnect connection fails, traffic will automatically fail over to the VPN tunnel.
*   **Scalability:** You can use Equal-Cost Multi-Path (ECMP) routing across multiple VPN tunnels to increase the available backup bandwidth.