### **OCI Study Notes: Image Import/Export and Bring Your Own Image (BYOI)**

#### **1. Image Import/Export: Sharing Across Tenancies and Regions**

*   **Purpose:** To share custom images between different OCI tenancies or to copy them to different OCI regions.
*   **Mechanism:** The process leverages **OCI Object Storage** to transfer the image file.

**Process for Replicating an Image Across Regions:**
1.  **Export:** Export the custom image from the source region to an Object Storage bucket **within the same region**.
2.  **Copy:** Copy the image file from the source region's bucket to an Object Storage bucket in the **destination region**.
3.  **Import:** In the destination region, import the image using the URL path to the image object in the destination bucket.

#### **2. Image Import Launch Modes**

When importing an image, you must select the appropriate launch mode based on the image's origin and configuration:

*   **Emulation Mode:**
    *   **For:** Virtual machines created **outside of OCI** from older on-premises physical or virtual machines that **do not support paravirtualized drivers**.

*   **Paravirtualized Mode:**
    *   **For:** Virtual machines created **outside of OCI** that **do support paravirtualized drivers**.

*   **Native Mode:**
    *   **For:** Images that were originally **exported from Oracle Cloud Infrastructure**.

#### **3. Bring Your Own Image (BYOI)**

*   **Concept:** The ability to **export virtual machines from your existing on-premises or other cloud virtualization environment** and import them directly into OCI as a custom image.
*   **Supported Formats:** You can import images in **qcow2** or **VMDK** formats.
*   **Primary Use Case:** **Cloud migration projects**, enabling you to lift-and-shift existing workloads into OCI with minimal changes.

#### **4. Benefits and Considerations of BYOI**

*   **Benefits:**
    *   Facilitates cloud migration.
    *   Supports both old and new operating systems.
    *   Encourages experimentation and increases infrastructure flexibility.
*   **Critical Consideration:** You **must comply with all operating system and software licensing requirements** when importing and using your own images.