### **OCI Study Notes: Compute GPU Shapes**

#### **1. Core Purpose**

*   **GPU Shapes** are designed for **hardware-accelerated workloads**.
*   **Hardware Acceleration:** A process where computationally intensive tasks are offloaded from the CPU to specialized hardware (the GPU) to significantly speed up processing.

#### **2. Key Features**

*   **NVIDIA GPU Powered:** OCI compute instances are powered by high-performance NVIDIA GPUs, including the **A100, A10, V100, and P100** series.
*   **High-Performance Networking:** Oracle's ultra-low latency **RDMA (Remote Direct Memory Access) cluster networking** provides **microsecond-level latency**, which is critical for parallel processing tasks across multiple GPUs.
*   **Massive Scalability:** Customers can build clusters hosting **more than 500 GPUs** without compromising performance.

#### **3. Use Cases**

GPU shapes are ideal for demanding computational workloads, such as:
*   **AI and Deep Learning Training**
*   **High-Performance Computing (HPC)** simulations and modeling
*   **Mainstream Graphics and Video** processing, rendering, and encoding

#### **4. Instance Types and GPU Series**

**A. Virtual Machine (VM) GPU Instances**
*   Support the following NVIDIA GPU architectures:
    *   **Ampere** (e.g., A100, A10)
    *   **Volta** (e.g., V100)
    *   **Pascal** (e.g., P100)

**B. Bare Metal GPU Instances**
*   Provide dedicated physical server access with attached GPUs.
*   Some Bare Metal GPU shapes support the high-performance **cluster networking** for the lowest possible latency between nodes.
*   These instances combine **Intel or AMD CPUs** with **NVIDIA graphics processors**.