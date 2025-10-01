### **OCI Study Notes: Preemptible Instances**

#### **1. Core Definition and Concept**

*   **What they are:** Preemptible instances are **short-lived compute instances** that are identical to on-demand instances but are offered at a significant discount.
*   **Key Trade-off:** In exchange for the lower price, OCI can **terminate ("preempt") these instances at any time** if the compute capacity is needed for on-demand instances.

#### **2. Pricing and Economic Benefit**

*   **Discount:** Preemptible instances are offered at a **90% discount** compared to on-demand pricing.
*   **Pricing Model:** The pricing is simple and consistent; it is always **90% cheaper** than the standard on-demand rate.

#### **3. Termination and Lifecycle**

*   **Termination Notice:** OCI provides a termination notice through the Event Service **30 seconds before** the instance is terminated.
*   **Event Type:** The notification is an **instance preemption action event**.

#### **4. Technical Limitations and Restrictions**

*   **Instance Type:** Preemptible capacity **does not support bare metal instances**; they are only available for Virtual Machine (VM) instances.
*   **Post-Launch Modifications:**
    *   You **cannot change the shape** of a preemptible instance after it is launched.
    *   You can only edit the instance's name.
*   **Conversion:** You **cannot convert** a preemptible instance to an on-demand instance.
*   **Management Features:** Preemptible instances **cannot be used** in:
    *   **Instance Configurations**
    *   **Instance Pools**

#### **5. Ideal Use Cases**

Preemptible instances are designed for **fault-tolerant, interruptible, and short-term workloads**, such as:
*   **Batch processing jobs** (e.g., video encoding, data transformation).
*   **Scientific computations** that can be checkpointed and resumed.
*   **Any workload that can handle sudden termination without data loss or business impact.**

#### **6. How to Create a Preemptible Instance**

*   The creation process is the same as for an on-demand instance.
*   The key difference is in the **Placement** section during creation, where you must select **"Preemptible"** as the capacity type instead of "On-Demand".