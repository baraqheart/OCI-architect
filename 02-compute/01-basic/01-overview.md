### **OCI Study Notes: Compute Service - Module Introduction**

#### **1. Module Overview**

This module provides a foundational understanding of the OCI Compute service, covering the core components and concepts needed to deploy and manage virtual machines.

#### **2. Key Topics Covered**

*   **Compute Service Overview:** A high-level introduction to the service and its capabilities.
*   **Shapes:** The pre-defined templates that determine the number of CPUs, amount of memory, and other resources allocated to a compute instance.
*   **Images:** The boot disks or templates that contain an operating system and sometimes additional software. This includes the concept of **Bring Your Own Image (BYOI)**, allowing you to use your own custom machine images.
*   **SSH Key Pairs:** The fundamental method for secure authentication and access to your Linux-based compute instances.
*   **Launching a Compute Instance:** A step-by-step walkthrough of the process to create a virtual machine.
*   **Capacity Management:** Understanding different provisioning models:
    *   **Capacity Reservations:** Guaranteeing compute capacity in a specific Availability Domain for future use.
    *   **Preemptible Instances:** Acquiring compute instances at a significantly lower cost, with the understanding that OCI may reclaim them if capacity is needed.
    *   **Dedicated Virtual Machine Hosts:** Physical single-tenant servers dedicated to your company's use, providing isolation for compliance and licensing needs.