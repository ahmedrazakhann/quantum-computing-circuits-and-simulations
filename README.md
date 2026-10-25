# Quantum Computing Journey: Circuits, Statevectors & Simulations

Welcome to **Quantum Computing Journey**! This repository documents my hands-on learning path into quantum computing, exploring the transition from fundamental linear algebra and quantum mechanics to practical circuit implementation using Python and **Qiskit**.

---

## 1. Project Purpose & What I Am Learning

The goal of this repository is to develop a deep, working intuition for quantum computation by building, analyzing, and simulating circuits step-by-step. 

Rather than treating quantum circuits as "black boxes," these notebooks explore the mathematical foundations that govern them:
- **Qubits & Statevectors:** Representing quantum states as normalized vectors in complex Hilbert space $\mathbb{C}^2$ and $\mathbb{C}^{2^n}$.
- **Amplitudes & Measurement Probabilities:** Using the **Born rule** ($P(i) = |\alpha_i|^2$) to calculate measurement odds from complex probability amplitudes.
- **Dirac Notation & Qiskit Bit Ordering:** Understanding bra-ket notation ($\langle \psi |$, $|\psi\rangle$) and navigating Qiskit's **little-endian** convention ($|q_{n-1} \dots q_1 q_0\rangle$).
- **Quantum Gates:** Applying unitary transformations:
  - Pauli gates: $X$ (bit flip / NOT), $Z$ (phase flip), $Y$ (bit + phase flip).
  - Hadamard gate ($H$): creating equal superpositions and enabling quantum interference.
  - Two-qubit entangling gates: Controlled-NOT ($CX$) and Controlled-Z ($CZ$).
- **Superposition & Interference:** Constructive and destructive interference (e.g., $H(H|0\rangle) = |0\rangle$).
- **Quantum Entanglement:** Preparing and measuring maximally entangled **Bell states** ($|\Phi^+\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}$) and multi-qubit correlations.
- **Circuits & Measurement Collapse:** Distinguishing pure mathematical statevectors from projective physical measurements using classical registers.
- **Simulation with Qiskit Aer:** Simulating circuit execution using high-performance local simulators, configuring shot counts, and analyzing statistical distributions.

---

## 2. Repository Structure

```text
quantum-computing/
├── README.md              # Project overview, learning roadmap, and documentation
├── requirements.txt       # Pinned, reproducible project dependencies
├── .gitignore             # Standard Python and Jupyter exclusions
├── LICENSE                # MIT Open Source License
├── Notebook1.ipynb        # Hands-on gate exploration, Bell states, and AerSampler execution
└── assignment_1.ipynb     # Classical registers, multi-qubit superposition, and AerSimulator statistics
```

### Notebook Overview

| File | Focus & Implemented Topics |
| :--- | :--- |
| [`Notebook1.ipynb`](./Notebook1.ipynb) | Multi-gate circuit exploration ($X, H, CX, CZ, Y$), statevector analysis with `qiskit.quantum_info.Statevector`, canonical Bell state preparation ($|\Phi^+\rangle$), initializing arbitrary statevectors (`[0, sqrt(0.2), 0, sqrt(0.8)]`), and introductory Qiskit Aer simulation. |
| [`assignment_1.ipynb`](./assignment_1.ipynb) | Building circuits with quantum and classical registers (`QuantumCircuit(2,2)`), two-qubit Hadamard operations, measuring qubits to specific classical bits (`measure(1,1)`, `measure(0,0)`), and running 1,000-shot simulations on `AerSimulator` with distribution plotting. |

---

## 3. Implemented Experiments (Ground Truth)

All experiments documented below reflect working code implemented in the repository's notebooks:

### Experiment A: Single & Multi-Qubit Gate Exploration
- **File:** `Notebook1.ipynb` (Cell 5) & `assignment_1.ipynb` (Cell 5)
- **Concept:** Constructing circuits and applying sequential single-qubit and two-qubit unitary operations:
  ```python
  from qiskit import QuantumCircuit
  qc = QuantumCircuit(2)
  qc.x(1)      # Flips qubit 1 to |1>
  qc.h(0)      # Places qubit 0 into equal superposition (|0> + |1>)/sqrt(2)
  qc.cx(0, 1)  # CNOT: flips qubit 1 if qubit 0 is |1>
  qc.cz(0, 1)  # Controlled-Z: applies phase flip if both qubits are |1>
  qc.y(0)      # Pauli-Y on qubit 0
  ```

### Experiment B: Statevectors & Dirac Representation
- **File:** `Notebook1.ipynb` (Cell 6, 9) & `assignment_1.ipynb` (Cell 6)
- **Concept:** Representing quantum states mathematically using `qiskit.quantum_info.Statevector`:
  ```python
  from qiskit import quantum_info as qi
  import numpy as np

  # Two-qubit state: (|00> + |11>) / sqrt(2)
  psi0 = qi.Statevector([1, 0, 0, 1] / np.sqrt(2))
  ```

### Experiment C: Preparing the Canonical Bell State ($|\Phi^+\rangle$)
- **File:** `Notebook1.ipynb` (Cell 7–11)
- **Concept:** Transforming ground state $|00\rangle$ into a maximally entangled state using a Hadamard gate followed by a CNOT gate:
  ```python
  bell = QuantumCircuit(2)
  bell.h(0)     # Creates (|0> + |1>)/sqrt(2) on qubit 0
  bell.cx(0, 1) # Entangles qubit 0 with qubit 1 -> (|00> + |11>)/sqrt(2)
  ```
- **Result:** Measuring this state yields perfectly correlated outcomes: either `00` (50%) or `11` (50%). The outcomes `01` and `10` are strictly forbidden.

### Experiment D: Initializing Circuits with Arbitrary Amplitudes
- **File:** `Notebook1.ipynb` (Cell 13–14)
- **Concept:** Initializing a quantum circuit to a non-uniform amplitude statevector:
  $$|\psi\rangle = 0|00\rangle + \sqrt{0.2}|01\rangle + 0|10\rangle + \sqrt{0.8}|11\rangle$$
  ```python
  psi_custom = qi.Statevector([0, np.sqrt(0.2), 0, np.sqrt(0.8)])
  qc.initialize(psi_custom, [0, 1])
  ```
- **Theoretical Probabilities:** $P(01) = (\sqrt{0.2})^2 = 0.20$ (20%), $P(11) = (\sqrt{0.8})^2 = 0.80$ (80%).

### Experiment E: Classical Registers & Projective Measurement
- **File:** `assignment_1.ipynb` (Cell 8)
- **Concept:** Explicitly adding classical bits to store measurement outcomes:
  ```python
  c3 = QuantumCircuit(2, 2)  # 2 quantum bits, 2 classical bits
  c3.h(0)
  c3.h(1)
  c3.cx(0, 1)
  c3.h(0)
  c3.h(1)
  c3.measure(1, 1)  # Measure qubit 1 into classical bit 1
  c3.measure(0, 0)  # Measure qubit 0 into classical bit 0
  ```

### Experiment F: Qiskit Aer Simulation & Shot Sampling
- **File:** `assignment_1.ipynb` (Cell 10–12) & `Notebook1.ipynb` (Cell 16–18)
- **Concept:** Executing a quantum circuit on `AerSimulator` across multiple shots:
  ```python
  from qiskit_aer import AerSimulator
  from qiskit.visualization import plot_distribution

  qc = QuantumCircuit(1)
  qc.h(0)
  qc.measure_all()

  simulator = AerSimulator()
  result = simulator.run(qc, shots=1000).result()
  counts = result.get_counts()
  # Example output: {'0': 508, '1': 492}
  plot_distribution(counts)
  ```

---

## 4. Key Quantum Computing Concepts Explained

### 1. Statevectors vs. Physical Measurement
- A **statevector** $|\psi\rangle$ is a mathematical description of the complete quantum state before measurement. In simulation, Qiskit allows us to inspect statevectors directly (`Statevector(qc)`).
- A **physical measurement** collapses the superposition according to the Born rule into a single computational basis state ($|0\rangle$ or $|1\rangle$). On real hardware, the statevector is never directly observable; we can only infer probabilities by executing many repeated runs (**shots**).

### 2. Born Rule & Probabilities
For a state $|\psi\rangle = \sum_i \alpha_i |i\rangle$, the probability of measuring basis state $|i\rangle$ is the squared absolute value of its complex amplitude:
$$P(i) = |\alpha_i|^2$$
The normalization requirement states that the sum of all probabilities must equal 1:
$$\sum_i |\alpha_i|^2 = 1$$

### 3. Qiskit's Little-Endian Bit Ordering
Qiskit indexes qubits starting at 0 and writes measurement bitstrings from **right to left**:
$$|q_{n-1} \dots q_2 q_1 q_0\rangle$$

For a 2-qubit system:
- **Qubit 0** corresponds to the **rightmost** bit.
- **Qubit 1** corresponds to the **leftmost** bit.
- Example: Outcome `'01'` means qubit 1 is in state `0` and qubit 0 is in state `1`.

### 4. Why Shot Counts Fluctuate
When measuring an equal superposition ($P(0) = 0.5, P(1) = 0.5$) with $N = 1000$ shots, we rarely see exactly 500 zeros and 500 ones. Because measurement is probabilistic, each shot is an independent Bernoulli trial. The expected standard deviation is:
$$\sigma = \sqrt{N \cdot p \cdot (1 - p)} = \sqrt{1000 \cdot 0.25} \approx 15.8$$
Typical counts fall naturally within $500 \pm 30$ shots (e.g. 488 vs. 512). As shot count increases (e.g. 100,000 shots), the relative error decreases according to the Law of Large Numbers.

### 5. Qiskit SDK vs. Qiskit Aer vs. IBM Quantum Runtime
- **Qiskit SDK (`qiskit`):** Core library for designing quantum circuits, applying gates, and computing statevectors.
- **Qiskit Aer (`qiskit-aer`):** High-performance C++ simulator for executing circuits locally on CPU/GPU with ideal or realistic noise models.
- **IBM Quantum Runtime (`qiskit-ibm-runtime`):** Cloud service for executing circuits on real IBM Quantum superconducting hardware or managed cloud primitives.

---

## 5. Prerequisites & Environment Setup

### Recommended Python Version
- **Python 3.11 or 3.12** is recommended. Qiskit Aer provides official pre-compiled C++ wheels for these versions.

### Setup Instructions (Linux / Ubuntu / macOS)

1. **Clone the repository:**
   ```bash
   git clone https://github.com/ahmedrazakhann/quantum-computing-circuits-and-simulations.git
   cd quantum-computing-circuits-and-simulations
   ```

2. **Create and activate a virtual environment:**
   ```bash
   # Linux / macOS
   python3.11 -m venv .venv
   source .venv/bin/activate
   ```

   *(On Windows PowerShell: `python -m venv .venv` followed by `.venv\Scripts\Activate.ps1`)*

3. **Install dependencies:**
   ```bash
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

4. **Register the Jupyter kernel:**
   ```bash
   python -m ipykernel install --user --name=quantum-journey --display-name "Python (quantum-journey)"
   ```

5. **Launch Jupyter:**
   ```bash
   jupyter lab
   # or
   jupyter notebook
   ```
   Open `Notebook1.ipynb` or `assignment_1.ipynb` and select the **"Python (quantum-journey)"** kernel.

---

## 6. Learning Roadmap

This roadmap organizes quantum computing topics into a progressive path, tracking concepts explored in this repository and upcoming milestones:

- [x] **Stage 1: Linear Algebra & Complex Amplitudes** — Statevectors, normalization, inner products, and complex numbers.
- [x] **Stage 2: Qubits & Basis States** — Computational basis states $|0\rangle, |1\rangle$, statevector representation with `quantum_info`.
- [x] **Stage 3: Single-Qubit Quantum Gates** — Unitary transformations, Pauli-X, Z, Y, and the Hadamard ($H$) gate.
- [x] **Stage 4: Superposition & Interference** — Creating equal superpositions, constructive/destructive interference ($H^2 = I$).
- [x] **Stage 5: Two-Qubit Gates & Entanglement** — CNOT ($CX$), Controlled-Z ($CZ$), and canonical Bell states ($|\Phi^+\rangle$).
- [x] **Stage 6: Quantum Circuits & Classical Measurement** — Multi-qubit circuits, classical registers, and projective measurements.
- [x] **Stage 7: Simulation with Qiskit Aer** — Executing circuits on `AerSimulator`, configuring shot counts, and plotting histograms.
- [ ] **Stage 8: Multi-Qubit Entanglement & GHZ States** *(Next)* — 3-qubit Greenberger-Horne-Zeilinger states ($|000\rangle + |111\rangle)/\sqrt{2}$.
- [ ] **Stage 9: Introductory Quantum Algorithms** *(Planned)* — Deutsch-Jozsa algorithm, Quantum Teleportation, and Superdense Coding.
- [ ] **Stage 10: Quantum Noise & Error Mitigation** *(Planned)* — Modeling decoherence, relaxation ($T_1$), and dephasing ($T_2$).
- [ ] **Stage 11: Real Quantum Hardware via IBM Quantum** *(Planned)* — Running circuits on real superconducting QPUs via IBM Quantum Runtime.

---

## 7. References & Learning Resources

- **[IBM Quantum Documentation](https://docs.quantum.ibm.com/):** The authoritative documentation for Qiskit SDK and IBM Quantum services.
- **[Qiskit Aer Documentation](https://qiskit.github.io/qiskit-aer/):** Guides and API references for the Aer simulation engine.
- **[IBM Quantum Learning](https://learning.quantum.ibm.com/):** Interactive courses and visual learning paths from IBM Quantum.
- **"Quantum Computation and Quantum Information"** by Michael A. Nielsen & Isaac L. Chuang (Cambridge University Press): The foundational textbook for quantum information theory.
- **"Quantum Computing: An Applied Approach"** by Jack D. Hidary (Springer): Practical guide bridging quantum theory and code.

---

## 8. License

This project is licensed under the [MIT License](./LICENSE) — feel free to use, modify, and learn from it.
