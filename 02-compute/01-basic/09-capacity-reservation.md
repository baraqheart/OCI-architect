### **OCI Study Notes: Capacity Reservations**

#### **1. Core Definition and Purpose**

*   **What it is:** A **Capacity Reservation** allows you to **reserve compute capacity in a specific Availability Domain (AD) in advance**.
*   **Primary Purpose:** To provide an **assurance that you will have the necessary compute capacity available** when you need it, preventing scenarios where you cannot launch instances due to resource exhaustion in the AD.

#### **2. How It Works**

*   **Reservation:** You create a reservation specifying the exact shape (e.g., `VM.Standard.E3.Flex`) and the number of instances you want to reserve.
*   **Consumption:** When you launch a new instance, you can assign it to the capacity reservation. The instance then consumes one unit from the reserved pool.
*   **Return of Capacity:** When an instance using the reserved capacity is terminated, the capacity unit is **returned to the reservation pool** and is available for a new instance.

#### **3. Billing and Utilization**

*   **Charges:** You are **charged for the reserved capacity as soon as the reservation is created**, regardless of whether you are running instances that use it.
*   **Billing Rate:** **Unused reserved capacity is billed at 85%** of the standard on-demand price for that shape.
*   **Instance Billing:** **Instances created against a capacity reservation are billed at 100%** of the on-demand price.

#### **4. Key Use Cases**

*   **Guaranteeing Capacity for Migrations or New Projects:** Ensures capacity is available for large-scale deployments.
*   **Disaster Recovery (DR):** Guarantees that capacity will be available in your secondary (DR) region for a failover event.
*   **Handling Seasonal or Variable Workloads:** Reserves capacity for predictable usage spikes.
*   **Creating a Buffer:** Acts as a buffer for unexpected workload increases.

#### **5. Important Limitations and Rules**

*   **Scope:** A capacity reservation is **confined to a single Availability Domain** and cannot be shared across ADs.
*   **Immutability:** Reservations **cannot be moved** to a different AD or a different tenancy.
*   **Incompatibility:** Capacity reservations **cannot be used with Dedicated Virtual Machine Hosts**.
*   **Shape Flexibility:** Instances launched from a reservation are **limited to the exact shape** defined in the reservation's configuration.
*   **Partial Fulfillment:** If there is insufficient capacity in the AD to fulfill your entire request, the system will create a reservation with the **maximum available capacity** (e.g., if you request 20 instances but only 10 are free, a reservation for 10 is created).