### **OCI Study Notes: Implementing FastConnect via Co-location with Oracle**

#### **1. Architectural Overview**

This model is designed for organizations that already have their own networking equipment physically deployed within an Oracle FastConnect location data center.

*   **Key Differentiator:** **Your company has its own cage/rack ("co-location")** inside the same physical data center building that houses the Oracle FastConnect infrastructure.
*   **The Connection:** The FastConnect is established by creating a very short, direct **cross-connect**—a physical fiber cable that runs within the data center, connecting your cage directly to the adjacent Oracle cage.
*   **Components in the Data Center:**
    *   **Oracle Cage:** Contains the Oracle FastConnect Edge devices.
    *   **Your Cage:** Contains your own Customer Premises Equipment (CPE) or edge routers.

#### **2. Step-by-Step Implementation Guide**

**Step 1: Prepare OCI for Private Peering (If Applicable)**
*   If you intend to use a **Private Virtual Circuit** to connect to a VCN, you must first create and set up a **Dynamic Routing Gateway (DRG)** in OCI.
*   This step is not required for Public Virtual Circuits.

**Step 2: Create Cross-Connect Group and Cross-Connect in OCI**
*   In the OCI console, you provision the logical representation of the physical link.
*   You create a **Cross-Connect Group** and then the individual **Cross-Connect** resources. A group is used for redundancy and increased bandwidth; a single cross-connect can be created if that is all that is required.

**Step 3: Submit the Letter of Authorization (LOA) for Cabling**
*   After creating the cross-connect resource, Oracle will provide a **Letter of Authorization (LOA)**.
*   You must submit this LOA to the **data center provider's technician** (the company that operates the physical data center building, e.g., Equinix, Digital Realty).
*   This LOA authorizes the data center technician to run the physical fiber cable between your designated port and Oracle's designated port.

**Step 4: Validate Physical Connectivity**
*   Once the data center technician has completed the cross-connect, you must verify:
    *   The **light levels** on the fiber are good (confirming a clean physical signal).
    *   The network **interfaces on your CPE for the cross-connect are "up"**.

**Step 5: Activate the Cross-Connect in OCI**
*   After confirming the physical link is operational, you must return to the OCI console and **activate the cross-connect**.
*   This crucial step informs Oracle that the physical cable is installed and tested, allowing the logical services to be enabled.

**Step 6: Create Virtual Circuits**
*   With an active physical cross-connect, you can now create the logical **Virtual Circuits** (Private or Public) that will carry your traffic.

**Step 7: Configure BGP on Your Co-located Routers**
*   Configure your routers located in your data center cage with the BGP peering information (the IP addresses) provided by Oracle for your virtual circuits.

**Step 8: Verify BGP Session**
*   From your co-located router:
    1.  Ping the **Oracle BGP IP address** to confirm network-level reachability.
    2.  Verify that the **BGP session with Oracle is in the "Established" state**.

**Step 9: Final End-to-End Testing**
*   Conduct thorough tests, such as pinging resources within your OCI VCN or performing data transfers, to validate the complete connection's functionality and performance.