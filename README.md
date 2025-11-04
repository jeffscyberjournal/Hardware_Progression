# Hardware_Progression
# 🖥️ Virtualization Journey: From Laptop Bottlenecks to Hardened Lab

This document tracks the evolution of my home lab — from early experiments on a laptop to enterprise‑class workstations, networking, and backup strategies. Each phase highlights bottlenecks, upgrades, and lessons learned.

---

## Phase 1: Early Experiments on Limited Hardware
- **Starting Point:** Acer laptop, 2 cores, 8 GB RAM  
- **Challenge:** Running 2 VMs in VirtualBox was painfully slow  
- **Diagnosis:**  
  - CPU and RAM underutilized  
  - Disk pegged at 100% usage  
  - Mechanical HDD throughput only 8–20 MB/s  
- **Upgrades:**  
  - Crucial 1 TB SSD (~360 MB/s)  
  - RAM increased to 16 GB (partial disassembly required)  
- **Result:** Major uplift in VM responsiveness  
- **Extra:** Replaced failing keyboard  

---

## Phase 2: Professional Exposure (Logitrain Internship)
- **Environment:** VMware ESXi booted from USB, dedicated VM storage drive  
- **Observed Bottleneck:** Disk throughput limited (20–45 MB/s)  
- **Lesson:** Storage throughput dominates virtualization performance  

---

## Phase 3: High‑Performance Storage
- **Procurement:** M.2 NVMe drives + ASUS Hyper card (PCIe Gen4, bifurcation support)  
- **Performance Gains:**  
  - 3,500 MB/s per drive  
  - Up to 4,800 MB/s in parallel  
- **Architecture:**  
  - NVMe for active workloads  
  - 2 TB HDD for cold storage/backups  
- **RAID Consideration:** Rejected RAID on NVMe (wear/failure risk, RAID ≠ backup)  

---

## Phase 4: Backup & Recovery Strategy
- **Spare PC:** Runs Veeam for backups  
- **Coverage:** Entire laptop + C: and D: drives on main PC  
- **Gap:** NVMe workloads not backed up due to size  
- **Discovery:** Some VMs not easily replaceable (e.g., Kali Gen2 requirement)  
- **Permissions Issue:** VM files locked to single PC → keep neutral backups with simplified ACLs  
- **Recovery Practice:** Test restores on alternate systems  

---

## Phase 5: Network & Connectivity Evolution
- **Initial Setup:** USB Wi‑Fi adapter + Cisco switch (200 W draw)  
- **Optimization:** Replaced Cisco switch with domestic router (25 W)  
- **Final Step:** Independent mini PC with pfSense firewall + Cat6a direct link → simpler, segmented, more secure, after experimentation with pfSense in VM first to determine its capability.  

---

## Phase 5.5: Hands‑On Networking & Certification Prep
- **Motivation:** CompTIA virtual labs were unstable → opted for real hardware  
- **Hardware:** Cisco Layer 3 switch (second‑hand)  
- **Software:**  
  - Cisco Packet Tracer → lightweight, stable, accurate for CCNA  
  - GNS3 → heavier, integrates with real VMs  
- **Learning Path:** Completed *Cisco_CCNA_Lab_Guide_v200‑301f*  
- **Outcome:** Packet Tracer sufficient for CCNA prep; real hardware taught quirks (e.g., no power switch, must pull plug)  

---

## Phase 6: Workstation Acquisition & Overprovisioning
- **Specs:**  
  - HP workstation, dual Xeons (24 cores / 48 threads)  
  - 64 GB ECC RAM  
  - Nvidia Quadro M6000 GPU
  - Low price of $600, sounded like a good starting point.
- **Realization:**  
  - Overprovisioned: rarely needed >12 cores for ~5 VMs  
  - Xeon clock speed lower than consumer CPUs  
  - Beneficial for Hashcat, but GPU (≈3000 CUDA cores) still dominated
  - Required 2nd GPU if passthrough is required. CUDA work done in Windows and seperate OS on USB stick.
- **Expansion:**  
  - Second workstation for backups/personal use (6‑core Xeon @ 3.5 GHz)  
  - Both fitted with ASUS Hyper cards (4 TB initially NVMe, eventually 4TB more in back up machine).  
- **Lesson:** Install Hyper cards fully populated upfront — thermal paste adhesion makes later upgrades risky  

---

## Phase 6.5: Power Draw, PSU, and Operational Quirks
- **Power Consumption:**  
  - Dual Xeons ~240 W max, ~130 W idle  
  - Full load with GPU CUDA: 550–600 W  
- **PSU Selection:**  
  - Must size for peak load  
  - 850 W 80+ Gold/Platinum ideal for efficiency at 200–400 W typical draw  
- **BIOS/Enterprise Quirks:**  
  - Defaults often RAID, not AHCI → OS fails after CMOS reset  
  - Bifurcation must be enabled for ASUS Hyper card  
  - RDP requires GPU if CPU lacks iGPU → hidden dependency  
- **Safeguards:**  
  - Document baseline configs (RAID/AHCI, bifurcation, virtualization flags)  
  - Keep USB recovery media ready  

---

## Phase 7: Device Segmentation & Experimentation
- **Roles:**  
  - Main workstation → virtualization/experimentation  
  - Backup workstation → cold storage, private docs  
  - Laptop → attack/test system  
  - Tablet (rooted) → Wi‑Fi testing (later replaced by laptop + dongle, since required device rooted and developer options enabled at all times)  

---

## 🔄 Alternative Considerations for what I should have done instead
- **NUC (6–8 cores):** Efficient, low power, but no PCIe x16 → no GPU; reliant on external/NAS storage  
- **Dedicated NAS:** Smarter for cold storage than NVMe; slower but safer and expandable  
- **mITX Build:** Supports discrete GPU, lower power than Xeon workstation, but higher upfront cost  

---

## 📌 Key Lessons Learned
- **Empirical Diagnosis:** Always validate bottlenecks (disk vs CPU vs RAM)  
- **I/O First:** Storage throughput is the primary limiter in virtualization  
- **Incremental Upgrades:** SSD → NVMe → PCIe bifurcation, each step justified by measured gains  
- **Backups vs Redundancy:** RAID ≠ backup; true resilience requires versioned, portable backups  
- **VM Portability:** Test restores on alternate systems to confirm portability, extra drive space and CPU cores allow for full replication in my circumstance, drive space is main requirement for this on NUC or mITX
- **Networking:** Packet Tracer is sufficient for CCNA; GNS3 adds realism; real hardware teaches quirks  
- **Power Economics:** PSU efficiency and idle draw matter as much as peak performance  
- **Enterprise Quirks:** BIOS defaults, bifurcation, GPU dependencies — document configs for recovery  
- **Segmentation:** Isolate workloads (personal vs lab) to reduce risk  
- **Efficiency vs Capability:** NUCs, workstations, and mITX each have trade‑offs in cost, expandability, and power  

