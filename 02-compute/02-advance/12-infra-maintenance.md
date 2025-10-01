### **OCI Study Notes: Infrastructure Maintenance for Virtual Machines**

#### **1. Overview**

OCI performs routine maintenance on the underlying physical infrastructure. To minimize impact, it offers several methods to relocate your virtual machine instances.

#### **2. Maintenance Types and Processes**

**A. Live Migration (Minimal Disruption)**
*   **Process:** OCI **transparently moves** a running VM from a host needing maintenance to a healthy host.
*   **Impact:** **Minimal disruption**; the instance remains running.
*   **Notifications:** Infrastructure maintenance events are emitted when the migration starts and ends.
*   **Limitations:**
    *   Only supported for **Linux operating systems**.
    *   Restricted to **specific VM shapes**.

**B. Reboot Migration (Planned Downtime)**
*   **Trigger:** For VMs that **do not support live migration**.
*   **Process:** OCI provides a **14 to 16-day advance notification** with a due date.
    *   **Proactive Action:** You can initiate the reboot migration yourself before the due date to control the timing of the downtime.
    *   **Automatic Action:** If you take no action, OCI will **stop, migrate, and restart** the instance on the due date.
*   **Impact:** A **short downtime** occurs during the stop/start process.
*   **Critical Note:** A standard OS reboot **does not** trigger this migration. You must use the Console, CLI, or API to perform the "reboot migration" action.

**C. Manual Migration (Customer Action Required)**
*   **Trigger:** For instances where no reboot migration date is provided, indicating manual intervention is required.
*   **Process:** You must **terminate the instance** and **recreate it**.
*   **Data Preservation:** To preserve data, you **must preserve the boot volume** during termination so it can be reattached to a new instance.

#### **3. Instance Recovery During Infrastructure Failure**

**A. Standard Virtual Machine Instances**
*   **Process:** OCI automatically performs a **reboot migration** to restore the instance on a healthy host.

**B. Dense I/O Virtual Machine Instances**
*   **Primary Process:** OCI attempts to recover the instance by **rebooting it on the same physical host**.
*   **If Recovery Fails:** OCI will notify you and provide a **14-day deadline** to manually delete the instance.
*   **Consequence of Inaction:** If you do not delete the instance by the deadline, OCI will disable it and then **delete it within the next 7 days**.
*   **Data Preservation:** In all cases, the **boot volume and any remotely attached block volumes are preserved** and not deleted.