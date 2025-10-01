### **OCI Study Notes: Compute Autoscaling**

#### **1. Core Components**

Autoscaling in OCI is built upon two fundamental resources:

**A. Instance Configuration**
*   **Definition:** A **template** that defines the settings for creating compute instances (e.g., shape, image, metadata, VNIC, storage, subnet).
*   **Usage:** Used to create individual instances or multiple instances within an **Instance Pool**.
*   **Important Limitation:** It **does not include the data on attached block volumes**; it only defines the volume's configuration.

**B. Instance Pool**
*   **Definition:** A **group of compute instances** within the same region that are managed as a single entity.
*   **Management:** All instances in a pool are created from the **same Instance Configuration**. A pool can have only one associated instance configuration.
*   **Operations:** After creation, you can:
    *   Manually update the pool size.
    *   Add/remove existing instances.
    *   Attach/detach load balancers.

#### **2. Autoscaling Policy Types**

**A. Metric-based Autoscaling**
*   **Trigger:** A performance metric (e.g., CPU utilization, memory usage) meets or exceeds a defined threshold.
*   **Benefit:** Maintains consistent performance during high demand and reduces costs during low demand.

**B. Schedule-based Autoscaling**
*   **Trigger:** A pre-defined schedule (e.g., every weekday at 9 AM).
*   **Use Case:** For predictable, time-based workload changes.

#### **3. Step-by-Step Implementation**

1.  **Create an Instance Configuration:** Define your instance template, either from a running instance or by specifying settings manually.
2.  **Create an Instance Pool:** Use the instance configuration to deploy a pool of instances. Define the initial size (e.g., 2 instances).
3.  **Configure the Instance Pool:** Perform initial setup like attaching a load balancer.
4.  **Define Autoscaling Configuration:** Attach a policy to the pool with rules:
    *   **Scale-out Rule:** e.g., Add 2 instances if CPU utilization > 70%.
    *   **Scale-in Rule:** e.g., Remove 2 instances if CPU utilization < 30%.
    *   **Limits:** Set a **minimum** (e.g., 1) and **maximum** (e.g., 4) number of instances for the pool.

#### **4. Architectural Example**

*   **Initial State:** Instance pool with 2 running instances.
*   **Scale-Out Event:** CPU utilization exceeds 70%. The autoscaling policy triggers, adding 2 new instances. The pool now has 4 instances.
*   **Scale-In Event:** CPU utilization drops below 30%. The policy triggers, removing 2 instances. The pool scales back down to 2 instances.
*   The pool will always maintain between the defined minimum (1) and maximum (4) instances.