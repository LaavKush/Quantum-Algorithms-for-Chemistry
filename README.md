<div align="center">

# Quantum Algorithms for Chemistry

**A Python and Qiskit quantum computing sandbox exploring foundational quantum circuits, local simulation, transpilation, and IBM Quantum Runtime workflows.**

[![Python](https://img.shields.io/badge/Python-3.9%2B-1f425f?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Qiskit](https://img.shields.io/badge/Qiskit-SDK-6929C4?style=for-the-badge&logo=qiskit&logoColor=white)](https://github.com/Qiskit/qiskit)
[![IBM Quantum](https://img.shields.io/badge/IBM_Quantum-Runtime-0F62FE?style=for-the-badge&logo=ibm&logoColor=white)](https://quantum.cloud.ibm.com/)
[![License](https://img.shields.io/badge/License-Apache_2.0-2ea44f?style=for-the-badge)](LICENSE.txt)

<br>

[Overview](#overview) &nbsp;&bull;&nbsp;
[Architecture & Scope](#architecture--scope) &nbsp;&bull;&nbsp;
[Repository Layout](#repository-structure) &nbsp;&bull;&nbsp;
[Installation](#installation--setup) &nbsp;&bull;&nbsp;
[Execution Guide](#usage-examples) &nbsp;&bull;&nbsp;
[Theoretical Background](#quantum-circuit-demonstrated) &nbsp;&bull;&nbsp;
[Roadmap](#development-roadmap)

</div>

---

## Overview

This repository provides a structured experimental workspace built around **Qiskit**. The current top-level scripts focus on preparing and simulating foundational quantum circuits, inspecting statevectors and shot-based measurement counts, transpiling circuits to target gate sets, and interfacing with the IBM Quantum Platform.

> [!NOTE]
> **Project Status:** The scripts currently checked into this repository demonstrate foundational Qiskit workflows (Bell-state preparation, Aer simulation, transpilation, and IBM Runtime execution). **Quantum-chemistry algorithms**—such as Variational Quantum Eigensolver (VQE), Quantum Phase Estimation (QPE), or molecular ground-state energy estimation—represent the long-term research direction outlined in the [Roadmap](#development-roadmap) and are not yet implemented.

---

## Architecture & Scope

### Core Capabilities

| Domain | Implementation Details | Primary Tools |
| :--- | :--- | :--- |
| **Quantum Circuit Synthesis** | Construct two-qubit maximally entangled Bell states using Hadamard ($H$) and Controlled-NOT ($CX$) gates. | `qiskit.QuantumCircuit` |
| **Local State & Shot Simulation** | Execute shot-based sampling and extract exact pre-measurement complex statevectors. | `qiskit_aer.AerSimulator` |
| **Circuit Transpilation** | Decompose and optimize circuits to target basis-gate sets (`u`, `cx`) at optimization level 3. | `qiskit.transpile` |
| **Cloud Backend Execution** | Authenticate securely, query accessible quantum processors/simulators, and submit Runtime jobs. | `qiskit_ibm_runtime` |
| **Data Visualization** | Render and export measurement-count probability distributions (`aer_histogram.png`). | `matplotlib` |

### Technology Stack

* **Core Language:** `Python 3.9+` — Experiment scripts and workflow orchestration
* **Quantum Framework:** `Qiskit` — Circuit construction, transpilation, and quantum information utilities
* **Simulation Engine:** `Qiskit Aer` — High-performance local statevector and shot simulation
* **Cloud Interface:** `Qiskit IBM Runtime` — Backend discovery and Sampler primitive execution
* **Scientific Computing:** `NumPy` & `Matplotlib` — Numerical linear algebra and histogram plotting

---

## Repository Structure

```text
Quantum-Algorithms-for-Chemistry/
├── qiskit/                     # Qiskit Python SDK source tree
├── crates/                     # Rust components
├── docs/                       # Documentation
├── test/                       # Qiskit test suite
├── tools/                      # Development and build tools
│
├── demo_more.py                # Bell state, counts, statevector, & transpilation
├── local_sim_test.py           # Bell-state simulation & histogram export
├── test_qiskit.py              # Basic Aer simulator verification
├── list_backends.py            # Query available IBM Quantum backends
├── ibm_runtime_example.py      # IBM Quantum Runtime Sampler workflow
├── interactive_save_token.py   # Secure interactive API token setup
├── save_ibm_token.py           # Token setup template (placeholder)
├── save_ibm_token_explicit.py  # Explicit token setup template (placeholder)
├── aer_histogram.png           # Generated simulator measurement histogram
│
├── requirements.txt            # Qiskit core runtime dependencies
├── pyproject.toml              # Python package & build configuration
├── CITATION.bib                # Citation metadata
├── LICENSE.txt                 # Apache License 2.0
└── README.md                   # Project documentation
```

---

## Installation & Setup

Ensure **Python 3.9 or later** is available in your environment (consistent with `pyproject.toml`).

### 1. Clone the Repository

```bash
git clone https://github.com/LaavKush/Quantum-Algorithms-for-Chemistry.git
cd Quantum-Algorithms-for-Chemistry
```

### 2. Initialize a Virtual Environment

<details open>
<summary><b>Linux / macOS</b></summary>

```bash
python3 -m venv .venv
source .venv/bin/activate
```
</details>

<details>
<summary><b>Windows (PowerShell)</b></summary>

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```
</details>

### 3. Install Required Packages

While `requirements.txt` tracks Qiskit's core runtime dependencies, the top-level experiment scripts require Aer, Matplotlib, and IBM Runtime:

```bash
python -m pip install --upgrade pip
python -m pip install qiskit qiskit-aer matplotlib qiskit-ibm-runtime
```

---

## Usage Examples

Execute all commands from the root directory of the repository.

| Script | Command | Description & Output |
| :--- | :--- | :--- |
| **Basic Verification** | `python test_qiskit.py` | Prepares a two-qubit Bell circuit, runs **256** simulator shots, and prints measurement counts. |
| **Simulation & Plot** | `python local_sim_test.py` | Runs `AerSimulator` for **1,024** shots, prints counts, and exports `aer_histogram.png`. |
| **Full Pipeline Demo** | `python demo_more.py` | Extracts pre-measurement statevectors, runs **1,024** shots, and transpiles (`u`, `cx`, level 3). |

---

## IBM Quantum Cloud Setup

To execute circuits on IBM Quantum hardware or cloud-hosted simulators, store your API credentials locally.

> [!CAUTION]
> **Credential Security:** Never commit real IBM Quantum API tokens to version control. Always use the interactive setup script or secure environment variables rather than hardcoding tokens into `save_ibm_token.py` or `save_ibm_token_explicit.py`.

1. **Authenticate via Interactive Prompt** *(prompts securely via `getpass` and saves under the `ibm_quantum_platform` channel)*:
   ```bash
   python interactive_save_token.py
   ```

2. **List Accessible Backends**:
   ```bash
   python list_backends.py
   ```

3. **Execute the Qiskit Runtime Sampler Workflow**:
   ```bash
   python ibm_runtime_example.py
   ```

---

## Quantum Circuit Demonstrated

The core examples prepare the maximally entangled two-qubit Bell state $|\Phi^+\rangle$:

$$|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}$$

### Circuit Topology

```text
     ┌───┐      ┌─┐   
q_0: ┤ H ├──■───┤M├───
     └───┘┌─┴─┐ └╥┘┌─┐
q_1: ─────┤ X ├──╫─┤M├
          └───┘  ║ └╥┘
c: 2/════════════╩══╩═
                 0  1 
```

### Gate Sequence & Expected Distribution

1. **Superposition:** Apply a Hadamard gate ($H$) to qubit $q_0$ to map $|0\rangle \mapsto \frac{|0\rangle + |1\rangle}{\sqrt{2}}$.
2. **Entanglement:** Apply a Controlled-NOT gate ($CX$) with $q_0$ as control and $q_1$ as target.
3. **Measurement:** Measure both qubits in the computational basis.

On an ideal noiseless simulator, outcomes `00` and `11` each occur with theoretical probability $P = 0.50$ (50%), subject to minor statistical variance from finite-shot sampling.

---

## Development Roadmap

To evolve this repository toward its quantum chemistry objective, planned milestones include:

- [ ] **Molecular Specification:** Define target molecular systems and equilibrium geometries (e.g., $\text{H}_2$, $\text{LiH}$).
- [ ] **Hamiltonian Construction:** Generate second-quantized fermionic Hamiltonians using electronic structure drivers.
- [ ] **Fermion-to-Qubit Mapping:** Transform fermionic operators into qubit Pauli operators (Jordan-Wigner / Bravyi-Kitaev).
- [ ] **VQE Implementation:** Implement a Variational Quantum Eigensolver workflow for ground-state energy estimation.
- [ ] **Classical Benchmarking:** Validate VQE energies against exact diagonalization (FCI) and classical Hartree-Fock references.
- [ ] **Hyperparameter & Noise Analysis:** Benchmark ansatz depth, classical optimizers, shot budgets, and error mitigation.

---

## Reproducibility & Scope Notes

* **Stochastic Variance:** Simulator scripts do not enforce a global random seed across all runs; minor shot-count fluctuations between executions are expected.
* **Hardware Calibration:** Results from IBM Quantum backends depend on backend availability and real-time hardware calibration at execution time.
* **Scientific Scope:** No chemical accuracy or quantum advantage claims are made by the current foundational scripts.

---

## License & Acknowledgements

* **License:** Distributed under the **Apache License 2.0**. Refer to [`LICENSE.txt`](LICENSE.txt) for full terms.
* **Acknowledgements:** Built with the open-source [Qiskit SDK](https://github.com/Qiskit/qiskit), Qiskit Aer, and [IBM Quantum Documentation](https://quantum.cloud.ibm.com/docs).
