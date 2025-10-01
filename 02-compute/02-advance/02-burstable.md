### **OCI Study Notes: Burstable Instances**

#### **1. Core Concept**

*   **What it is:** A virtual machine instance that provides a **baseline level of CPU performance** with the ability to temporarily **burst to a higher level** to handle occasional spikes in usage.
*   **Design Purpose:** For workloads that are typically idle or have low CPU utilization but require short periods of high CPU performance.

#### **2. Performance and Cost Model**

*   **Baseline Utilization:** When creating the instance, you specify a baseline CPU utilization as a fraction of each OCPU core. The options are:
    *   **12.5%**
    *   **50%**
*   **Cost:** Burstable instances **cost less** than regular instances with the same total OCPU count.
*   **Billing:** You are **charged based on the baseline OCPU** you select, not the maximum OCPUs or the burst capacity used.

#### **3. Bursting Mechanics**

*   **Burst Credit System:** The system allows bursting **only if the instance's average CPU utilization over the last 24 hours is below its baseline**.
*   **No Guarantee:** There is **no guarantee** that an instance will be able to burst if the underlying physical host is oversubscribed.
*   **Memory:** **Memory does not burst**; it is fixed based on the shape's configuration.

#### **4. Supported Shapes and Use Cases**

*   **Supported Shapes:** Burstable instances are available on specific flexible shapes, including:
    *   `VM.Standard3.Flex`
    *   `VM.Standard.E3.Flex`
    *   `VM.Standard.A1.Flex`
*   **Ideal Workloads:**
    *   Microservices
    *   Development and Test environments
    *   CI/CD tools
    *   Static websites
    *   Monitoring systems

#### **5. How to Create a Burstable Instance**

The process is similar to creating a standard on-demand instance, with one key difference in the shape configuration:

1.  Select a **supported flexible shape** (e.g., `VM.Standard3.Flex`).
2.  Select the **Burstable** option.
3.  Configure:
    *   **Total OCPUs:** The maximum number of OCPUs the instance can burst to.
    *   **Baseline:** Set the baseline utilization to either **12.5%** or **50%**. This defines the minimum guaranteed CPU and your billing rate.