### **OCI Study Notes: Implementing FastConnect with a Third-Party Provider**

#### **1. Architectural Overview**

This model is used when you want to use a network carrier that is **not** a pre-integrated Oracle Partner.

*   **Key Differentiator:** The third-party provider **does not have a pre-existing physical connection** to Oracle's equipment inside the FastConnect location.
*   **Provider's Responsibility:** The third-party provider (or network carrier) is responsible for establishing **two physical connections**:
    1.  A connection from **your on-premises data center to the provider's network**.
    2.  The **cross-connect**—the physical fiber cable connecting the provider's network to the **Oracle FastConnect Edge devices** within the Oracle cage at the FastConnect location.

#### **2. Step-by-Step Implementation Guide**

**Step 1: Prepare OCI for Private Peering (If Applicable)**
*   If you plan to use a **Private Virtual Circuit**, you must first create and set up a **Dynamic Routing Gateway (DRG)** and attach it to your VCN.
*   This step is not needed for Public Virtual Circuits.

**Step 2: Create Cross-Connect Group and Cross-Connect in OCI**
*   In the OCI console, you must provision the physical layer components.
*   You create a **Cross-Connect Group** (a logical container for multiple cross-connects) and then the individual **Cross-Connect** resources within it.

**Step 3: Obtain and Provide the Letter of Authorization (LOA)**
*   After creating the cross-connect in OCI, the system will generate a **Letter of Authorization (LOA)**.
*   You must **forward this LOA to your third-party provider**. This document authorizes the provider to physically cable their equipment to Oracle's designated ports in the FastConnect data center.

**Step 4: Provider Requests Cabling**
*   The third-party provider uses the LOA to request and perform the physical cross-connect installation at the FastConnect location on your behalf.

**Step 5: Validate Physical Connectivity**
*   Once the provider completes the cabling, you must confirm:
    *   The **light levels** on the fiber connections are good.
    *   The network **interfaces for each cross-connect are administratively "up"**.

**Step 6: Activate the Cross-Connect in OCI**
*   After confirming the physical layer is operational, you must return to the OCI console and **activate each cross-connect**.
*   This step critically **informs Oracle that the physical cable is ready** for use, allowing the logical layer to be established.

**Step 7: Create Virtual Circuits**
*   Now that the physical connection is active, you can create the logical layer by setting up one or more **Virtual Circuits** (Private or Public) over it.

**Step 8: Configure BGP on Your On-Premises Router**
*   Configure your Customer Premises Equipment (CPE) with the BGP peering information (IP addresses) provided by Oracle for your virtual circuits.

**Step 9: Verify BGP Session**
*   From your on-premises environment:
    1.  Ping the **Oracle BGP IP address** to confirm network reachability.
    2.  Verify that the **BGP session with Oracle is in the "Established" state**.

**Step 10: Final End-to-End Testing**
*   Perform comprehensive data transfer and application access tests to ensure the entire connection is functioning as expected.