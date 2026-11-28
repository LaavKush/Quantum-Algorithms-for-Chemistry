# Quantum Algorithms for Chemistry

**A Python and Qiskit quantum computing sandbox exploring foundational quantum circuits, local simulation, transpilation, and IBM Quantum Runtime workflows.**

[Overview](#overview) • [Current Scope](#current-scope) • [Installation](#installation--setup) • [Usage](#usage-examples) • [Quantum Theory](#quantum-circuit-demonstrated) • [Roadmap](#development-roadmap)

---

## Overview

This repository serves as an experimental workspace built around **Qiskit**. The current top-level scripts focus on preparing and simulating foundational quantum circuits, inspecting statevectors and shot-based measurement counts, transpiling circuits, and interfacing with the IBM Quantum Platform.

> [!NOTE]
> **Project Status:** The scripts currently checked into this repository demonstrate core Qiskit workflows (Bell-state preparation, Aer simulation, and IBM Runtime execution). **Quantum-chemistry algorithms** (such as VQE, QPE, or molecular ground-state energy estimation) are part of the long-term direction outlined in the [Roadmap](#development-roadmap) and are not yet implemented.

---

## Current Scope

- **Bell-State Generation:** Prepare two-qubit maximally entangled states using Hadamard ($H$) and Controlled-NOT ($CX$) gates.
- **Local Simulation:** Execute circuits locally via `Qiskit Aer` and analyze shot-based measurement counts.
- **Statevector Inspection:** Extract and inspect exact pre-measurement quantum statevectors.
- **Circuit Transpilation:** Optimize and map quantum circuits to target basis-gate sets (`u`, `cx`) at optimization level 3.
- **IBM Quantum Connectivity:** Securely configure API credentials, query accessible quantum hardware/simulators, and execute jobs using `Qiskit Runtime`.
- **Visualization:** Generate and export measurement-count histograms (`aer_histogram.png`).

---

## Technology Stack

| Category | Technology | Purpose |
| :--- | :--- | :--- |
| **Core Language** | `Python 3.9+` | Experiment scripts and workflow orchestration |
| **Quantum SDK** | `Qiskit` | Circuit construction, transpilation, and quantum info utilities |
| **Local Simulator** | `Qiskit Aer` | High-performance local quantum circuit simulation |
| **Cloud Execution** | `Qiskit IBM Runtime` | Backend discovery and Runtime Sampler execution |
| **Numerics & Plotting** | `NumPy` & `Matplotlib` | Linear algebra operations and histogram visualization |

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

Ensure you have **Python 3.9 or later** installed (consistent with `pyproject.toml`).

### 1. Clone the Repository

```bash
git clone https://github.com/LaavKush/Quantum-Algorithms-for-Chemistry.git
cd Quantum-Algorithms-for-Chemistry
```

### 2. Create & Activate a Virtual Environment

**Linux / macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

**Windows (PowerShell):**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

While `requirements.txt` covers Qiskit's core dependencies, the experiment scripts require Aer, Matplotlib, and IBM Runtime:

```bash
python -m pip install --upgrade pip
python -m pip install qiskit qiskit-aer matplotlib qiskit-ibm-runtime
```

---

## Usage Examples

Run all commands from the root directory of the repository.

### 1. Basic Qiskit & Aer Verification

Prepares a two-qubit Bell circuit, runs 256 simulator shots, and prints measurement counts:

```bash
python test_qiskit.py
```

### 2. Local Simulation & Histogram Export

Executes the Bell-state circuit on `AerSimulator` for 1,024 shots and exports the distribution chart to `aer_histogram.png`:

```bash
python local_sim_test.py
```

### 3. Statevector & Transpilation Pipeline

Demonstrates pre-measurement statevector extraction, 1,024-shot simulation, and circuit transpilation (`u`, `cx` basis gates, optimization level 3):

```bash
python demo_more.py
```

---

## IBM Quantum Cloud Setup

To execute circuits on IBM Quantum hardware or cloud simulators, save your API credentials locally.

> [!CAUTION]
> **Security Best Practice:** Never commit real IBM Quantum API tokens to version control. Always use the interactive setup script or environment variables rather than hardcoding secrets into `save_ibm_token.py`.

1. **Save your token securely via interactive prompt:**
   ```bash
   python interactive_save_token.py
   ```
   *(Prompts for your API key using `getpass` and saves it under the `ibm_quantum_platform` channel.)*

2. **Verify accessible backends:**
   ```bash
   python list_backends.py
   ```

3. **Run the Qiskit Runtime Sampler example:**
   ```bash
   python ibm_runtime_example.py
   ```

---

## Quantum Circuit Demonstrated

The core examples prepare the maximally entangled two-qubit Bell state $|\Phi^+\rangle$:

$$|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}$$

### Circuit Operations:
1. **Superposition:** Apply a Hadamard gate ($H$) to qubit $0$ to create an equal superposition.
2. **Entanglement:** Apply a Controlled-NOT gate ($CX$) with qubit $0$ as control and qubit $1$ as target.
3. **Measurement:** Measure both qubits in the computational basis.

On an ideal simulator, outcomes `00` and `11` each occur with probability $P = 0.50$ (50%), subject to minor statistical fluctuations due to finite shot counts.

---

## Development Roadmap

To align the codebase with the repository's quantum chemistry focus, future milestones include:

- [ ] **Molecular Specification:** Define target molecular systems and equilibrium geometries (e.g., $\text{H}_2$, $\text{LiH}$).
- [ ] **Hamiltonian Construction:** Generate second-quantized fermionic Hamiltonians using electronic structure drivers.
- [ ] **Fermion-to-Qubit Mapping:** Transform fermionic operators into qubit Pauli operators (Jordan-Wigner / Bravyi-Kitaev).
- [ ] **VQE Implementation:** Build a Variational Quantum Eigensolver workflow for ground-state energy estimation.
- [ ] **Classical Benchmarking:** Validate VQE results against exact diagonalization (FCI) and classical Hartree-Fock references.
- [ ] **Hyperparameter & Noise Analysis:** Benchmark ansatz depth, classical optimizers, shot budgets, and noise mitigation.

---

## Reproducibility Notes

- **Stochastic Variance:** Simulator scripts do not fix a global random seed across all runs; minor shot-count variations between executions are expected.
- **Hardware Calibration:** Results from IBM Quantum backends reflect real-time hardware noise and calibration states at execution time.
- **Scope Disclaimer:** No chemical accuracy or quantum advantage claims are made by the current foundational scripts.

---

## License & Acknowledgements

- **License:** Distributed under the **Apache License 2.0**. See [`LICENSE.txt`](LICENSE.txt) for complete details.
- **Acknowledgements:** Built with the open-source [Qiskit SDK](https://github.com/Qiskit/qiskit), Qiskit Aer, and [IBM Quantum Documentation](https://quantum.cloud.ibm.com/docs).
