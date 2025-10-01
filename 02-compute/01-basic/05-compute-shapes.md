### **OCI Study Notes: Compute Shapes**

#### **1. Core Definition**

*   A **shape** is a template that defines the hardware resources allocated to a compute instance.
*   It determines:
    *   Number of **OCPUs** (Oracle CPUs)
    *   Amount of **Memory (RAM)**
    *   Other resources like network bandwidth.

#### **2. Processor Types**

Shapes are available based on different processor architectures:
*   **AMD Processors**
*   **Intel Processors**
*   **ARM-based Processors**

#### **3. Shape Types: Fixed vs. Flexible**

**A. Fixed Shapes**
*   **Definition:** Pre-configured templates where you **cannot customize** the number of OCPUs or the amount of memory.
*   **Instance Types:** Can be used to create both **Bare Metal instances** and **Virtual Machine (VM) instances**.

**B. Flexible Shapes**
*   **Definition:** Templates that allow you to **customize** the number of OCPUs and the amount of memory when launching or resizing a VM.
*   **Instance Types:** Can **only** be used to create **Virtual Machine (VM) instances**. They are not available for Bare Metal.
*   **Scalable Resources:** In flexible shapes, the **network bandwidth** and the number of **virtual Network Interface Cards (vNICs)** scale proportionally with the number of OCPUs you select.

#### **4. Benefits and Example of Flexible Shapes**

*   **Primary Benefit:** Allows you to **precisely match VM resources to your workload requirements**, optimizing both performance and cost. You avoid paying for resources you don't need.

*   **Example: VM.Standard3.Flex**
    *   You can customize the number of OCPUs, for example, from **1 to 64**.
    *   You can customize the amount of memory, with a typical maximum of **64 GB of memory per OCPU**.
    *   **Calculation:** If you select **4 OCPUs**, the maximum memory you can configure is 4 OCPUs * 64 GB/OCPU = **256 GB of RAM**.