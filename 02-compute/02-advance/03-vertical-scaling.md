### **OCI Study Notes: Compute Vertical Scaling**

#### **1. Core Definition**

*   **Vertical Scaling (Scale Up/Scale Down):** The process of **changing the shape** of an existing, running virtual machine instance to add or remove resources (OCPUs, Memory).
*   **Benefit:** Allows you to adjust performance and cost without needing to rebuild the instance or redeploy your application.

#### **2. What Changes and What Stays the Same**

**Resources That Are NOT Affected (Remain Unchanged):**
*   **IP Addresses:** Both the **public and private IP addresses** remain the same.
*   **Attachments:** All **Block Volume attachments** and **VNIC (network interface) attachments** are preserved.

**Resources That ARE Affected (Will Change):**
*   **Number of OCPUs**
*   **Amount of Memory (RAM)**
*   **Network Bandwidth**
*   **Maximum number of VNICs** the instance can support

#### **3. Important Process Details and Limitations**

*   **Instance Reboot:** If the instance is **running** when you change its shape, it will be **automatically rebooted** as part of the operation. This causes a brief downtime.
*   **Shape Selection Constraint:** The **target shape you can select depends on the original shape series and the image** (OS) of the instance. You cannot change to an arbitrary shape.

#### **4. Automation Potential**

*   You can **automate vertical scaling** using OCI's monitoring and functions services:
    1.  Use the **Monitoring Service** to track CPU/Memory metrics and set an alarm when a threshold is met.
    2.  Configure the alarm notification to trigger a **Function**.
    3.  The function can then execute an API call to **adjust the instance's shape** automatically.