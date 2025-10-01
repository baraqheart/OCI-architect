### **OCI Study Notes: Implementing FastConnect with an Oracle Partner**

#### **1. Architectural Overview**

This model leverages a network provider that has a pre-existing, integrated relationship with Oracle.

*   **OCI Region:** Contains your Virtual Cloud Network (VCN) with private and public subnets.
*   **FastConnect Location:** An Oracle data center with an Oracle cage housing the **FastConnect Edge devices**.
*   **On-Premises:** Your data centers, each with their own Customer Premises Equipment (CPE).
*   **Key Advantage:** The **Oracle Partner has already established redundant physical cross-connects** to the Oracle FastConnect Edge devices. Because this physical link pre-exists, **no Letter of Authorization (LOA) is required**.

**Your Responsibility:** You work with the partner to provision the network connection from **your on-premises data center to the partner's edge network**, which is already connected to OCI.

---

#### **2. Step-by-Step Implementation Guide**

**Step 1: Order the Connection from the Oracle Partner**
*   Initiate the process by placing an order with your chosen Oracle Partner.
*   **Important Note:** The provisioning time is dependent on the partner and is not instantaneous.

**Step 2: Prepare OCI Resources for Private Peering**
*   If you intend to use a **Private Virtual Circuit** (to connect to your VCN), you must first set up a **Dynamic Routing Gateway (DRG)** and attach it to your target VCN.
*   This step is not required for Public Virtual Circuits (for accessing public OCI services).

**Step 3: Create Virtual Circuits in OCI**
*   In the OCI Console, create one or more Virtual Circuits for your FastConnect connection.
*   You can create both Private and Public Virtual Circuits as needed.
*   **Key Output:** Upon creation, each virtual circuit is assigned a unique **OCID (Oracle Cloud Identifier)**.

**Step 4: Provide Virtual Circuit OCIDs to the Partner**
*   You must provide the OCID for each virtual circuit you created to your Oracle Partner.
*   The partner uses these OCIDs to configure and activate the logical connectivity on their end, completing the link to OCI.

**Step 5: Configure Your On-Premises CPE (Customer Premises Equipment)**
*   You must configure BGP routing on your on-premises router. There are two potential architectural scenarios for the BGP session:

    *   **Scenario A: BGP Session to Oracle Directly**
        Your CPE establishes a BGP session directly with Oracle's BGP IP addresses.

    *   **Scenario B: BGP Session to the Oracle Partner**
        Your CPE establishes a BGP session with the Partner's edge devices, which then peer with Oracle.

*   You will need the specific BGP peering IP addresses (provided by Oracle or the partner) to complete this configuration.

**Step 6: Validate Physical Connectivity**
*   Confirm with the partner that for all physical connections:
    *   The **light levels** on the fiber optics are good.
    *   The network **interfaces are administratively "up"**.

**Step 7: Establish and Verify BGP Sessions**
*   The verification process depends on the BGP scenario from Step 5:

    *   **If BGP to Oracle Directly:**
        1.  Ping the **Oracle BGP IP address** to confirm basic reachability.
        2.  Confirm that the **BGP session with Oracle is established** (state should be "Established").

    *   **If BGP to the Oracle Partner:**
        1.  Ping the **Oracle Partner's edge IP address**.
        2.  Confirm that the **BGP session with the Partner is established**.
        3.  Then, also ping the **Oracle BGP IP address** to ensure end-to-end reachability through the partner's network.

**Step 8: Final End-to-End Testing**
*   Perform comprehensive tests to validate the connection, such as pinging resources inside your VCN from on-premises or transferring data to confirm bandwidth and stability.