# **PCIe Gen5/Gen6 RTL + UVM + SVA/FV Verification Project**

This repository contains an **end-to-end, industry-style PCIe Gen5/Gen6 Endpoint design and verification framework**, including RTL modeling, UVM-based verification, formal properties, transaction-level observability, coverage automation, and CI/CD regression.

It is suitable for **interviews, research, advanced coursework, and verification portfolio demonstration.**

---

## 🚀 **Project Overview**

This project implements a **behaviorally realistic PCIe Endpoint** driven by an **AXI-Lite interface**, verified using **coverage-driven UVM**, strengthened with **SystemVerilog Assertions (SVA) and Formal Verification (FV)**, and supported by **professional debug, observability, and automation infrastructure.**

### **What You Can Claim from This Project**

You can credibly state that you have built and verified:

* A **PCIe Gen5/Gen6 Endpoint** with:

  * LTSSM (Link Training and Status State Machine)
  * Gen5 vs Gen6 negotiation
  * x1 / x4 lane modeling
  * Per-lane skew modeling
  * NRZ vs PAM4 abstraction

* A **PCIe Data Link Layer (DLL)** with:

  * Realistic Tx/Rx sequence numbers
  * Sliding-window retry buffer
  * ACK/NAK handling and replay logic

* **AXI-Lite → PCIe Bridge**

  * Converts AXI writes/reads into PCIe TLPs (MWr/MRd)

* **UVM Verification Environment**

  * Driver, Monitor, Sequencer, Agent, Environment
  * Directed + random sequences
  * Functional scoreboard with golden memory model

* **Coverage-Driven Verification**

  * TLP type coverage
  * Address region coverage
  * Cross coverage (TLP × Address)
  * Automated coverage goals
  * HTML coverage report
  * Excel-based coverage triage

* **Assertion-Based Verification (SVA)**

  * LTSSM correctness checks
  * Credit safety checks
  * Retry buffer validity checks
  * ACK/NAK mutual exclusivity

* **Formal Verification (SVA/FV)**

  * Safety properties (no illegal states)
  * Liveness properties (eventual L0 entry)
  * Skew and recovery correctness

* **Professional Debug & Observability**

  * Custom waveform dashboards (Questa/ModelSim)
  * Clickable assertion viewer
  * SimVision transaction-level tracing for TLPs

* **CI/CD Automation**

  * Batch regression script
  * GitHub Actions workflow
  * Automatic artifact generation and upload

---

## 📁 **Repository Structure**

```
pcie-gen5-gen6-RTL-uvm-coverage/
│
├── README.md
├── Makefile
├── run_coverage_html.tcl
├── run_coverage_goals.tcl
│
├── rtl/
│   ├── pcie_pkg.sv
│   ├── pcie_top.sv
│   ├── ltssm.sv
│   ├── flow_control.sv
│   ├── dll_layer.sv
│   ├── tlp_gen.sv
│   ├── tlp_decode.sv
│   ├── app_mem.sv
│   └── axi2pcie_bridge.sv
│
├── uvm/
│   ├── pcie_uvm_pkg.sv
│   ├── seq_item.sv
│   ├── driver.sv
│   ├── monitor.sv
│   ├── sequencer.sv
│   ├── agent_tx.sv
│   ├── agent_rx.sv
│   ├── scoreboard.sv
│   ├── coverage_collector.sv
│   ├── env.sv
│   └── sequences.sv
│
├── tb/
│   ├── top.sv
│   └── tests/
│       ├── test_link_train.sv
│       ├── test_mwr_mrd.sv
│       ├── test_retry.sv
│       └── test_coverage.sv
│
├── formal/
│   └── pcie_formal.sv
│
├── waves/
│   ├── pcie_dashboard.do
│   ├── pcie_minimal.do
│   ├── pcie_assertions.do
│   └── pcie_simvision.tcl
│
├── scripts/
│   ├── run_wave.do
│   ├── run_assertions.do
│   ├── run_simvision.do
│   ├── gen_coverage_triage_xlsx.tcl
│   └── run_regression.sh
│
├── reports/
│   └── (auto-generated coverage & assertion reports)
│
└── .github/
    └── workflows/
        └── ci.yml
```

---

## 🏗️ **Design Architecture (High Level)**

### **DUT (PCIe Endpoint)**

```
AXI-Lite Master
      │
AXI2PCIe Bridge → PCIe TLP Generator
      │
LTSSM (Gen5/Gen6, x1/x4, skew, NRZ/PAM4)
      │
Flow Control (per-lane credits)
      │
DLL (Seq Numbers + Retry Buffer)
      │
Transaction Layer (TLP Encode/Decode)
      │
App Memory (BAR Space)
```

---

## 🧪 **Verification Architecture (UVM)**

```
UVM Test
   │
pcie_env
   │
├── agent_tx → driver → DUT
│              monitor → coverage + scoreboard
│
├── coverage_collector (functional coverage)
│
└── scoreboard (golden model checking)
```

---

## 🔍 **Formal Verification (SVA/FV)**

The following properties are checked formally:

* **Safety**

  * ACK and NAK must never be asserted together
  * NAK must not be followed by immediate ACK
* **Liveness**

  * The link must eventually reach L0 state
* **Skew Handling**

  * Excessive skew must trigger RECOVERY state
* **Sequence Numbers**

  * Tx sequence numbers must be monotonically increasing

---

## 📊 **Coverage & Reporting**

Running coverage generates:

* `cov_data.ucdb` → Raw coverage database
* `coverage_html/` → Interactive HTML coverage report
* `reports/coverage_triage.xlsx` → Excel triage sheet
* `reports/assertions_report.txt` → Assertion results

---

## 🌊 **Debug & Observability**

### Waveform Dashboards

* `pcie_dashboard.do` → Full debug view (LTSSM, credits, skew, DLL, AXI)
* `pcie_minimal.do` → Lightweight view (clock, reset, state, ACK/NAK)
* `pcie_assertions.do` → Clickable assertion viewer

### SimVision Transaction Tracing

* Every TLP is tracked as a transaction:

  * Type
  * Address
  * Data
  * Tx/Rx sequence numbers

---

## 🔁 **CI/CD Regression (GitHub Actions)**

On every push or PR, the following runs automatically:

1. Compile RTL + UVM + Formal
2. Run coverage test
3. Generate HTML coverage report
4. Generate Excel triage sheet
5. Run assertion-only simulation
6. Upload artifacts to GitHub

---

## ▶️ **How to Run (Questa / EDA Playground)**

### **Compile**

```
vlog rtl/*.sv uvm/*.sv tb/top.sv tb/tests/*.sv formal/*.sv
```

### **Run Coverage Test**

```
vsim -c work.top -do "do run_coverage_goals.tcl"
```

### **View Waveforms**

```
vsim -do scripts/run_wave.do
```

### **Run SimVision**

```
vsim -do scripts/run_simvision.do
```

### **Run Full Regression**

```
./scripts/run_regression.sh
```

---

## 🎯 **Learning Outcomes**

By working with this project, you demonstrate:

* Deep understanding of **PCIe protocol layers**
* Hands-on experience with **RTL design + UVM verification**
* Mastery of **SVA + Formal Verification**
* Practical knowledge of **coverage closure techniques**
* Professional debugging using **SimVision and wave dashboards**
* Experience with **verification automation and CI/CD**

---

## ✍️ **Author**

**Your Name**
BIBIN N BIJI, Senior RTL and DV Engineer, Yarok14 Technologies Pvt Ltd.

---

## 📜 License

MIT License 

---
