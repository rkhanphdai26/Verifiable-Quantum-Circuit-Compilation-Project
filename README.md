# Verifiable-Quantum-Circuit-Compilation-Project
Reducing the resources required for quantum applications
# Quantum Inverse Square Root (`invsqrt`) Compilation

Resource-efficient compilation and optimization of fault-tolerant quantum circuits for the **`invsqrt` benchmark** in the **Google Quantum AI - Verifiable Quantum Circuit Compilation** challenge.

## Overview
Simulating electrostatic interactions between charged particles in fusion plasmas requires evaluating the inverse square root function (1/√x) inside quantum phase estimation and Hamiltonian simulation routines. Because fault-tolerant quantum computers incur heavy overhead from error correction, standard reversible arithmetic quickly becomes prohibitively expensive.

This repository focuses on minimizing **logical qubit count** and **Toffoli-equivalent gate cost** for the `invsqrt` circuit while passing randomized classical fuzz-testing verification.

## Approach & Optimization Techniques
* **Arithmetic & Approximation:** Exploring piecewise polynomial approximations, Newton–Raphson iterations, and lookup-table hybrids to reduce Toffoli depth.
* **Measurement-Based Uncomputation:** Leveraging `kickmix` phase kickback and measurement-based uncomputation to free ancilla qubits early.
* **Automated Verification:** Validating circuit correctness against Google's reference `kickmix` verifier across input bit-widths.

## Tech Stack
* **Python 3.12**
* **Qualtran & KickMix:** Used for circuit construction, resource estimation (qubit and Toffoli counts), and linear-time randomized verification.

## Setup & Installation
1. Clone the repository:
   ```bash
   git clone https://github.com
   cd quantum-invsqrt-compilation
   ```
2. Create and activate a virtual environment:
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows use: .venv\Scripts\activate
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Usage
Build the `invsqrt` circuit, inspect resource counts, and run verification checks:
```bash
python main.py
```

## License
Distributed under the **Apache 2.0 License**.
