---
title: "Quantum Extreme Learning Machines (QELM) Framework"
authors: "Eefje Karremans"
layout: portfolio
image: "../img/portfolio/QA/ClassmodRich.png"
description: "A responsive web design project."
category: "Machine Learning / Quantum Algorithms"
date: May 2025
---

# Quantum Extreme Learning Machines (QELM) Framework

## 2 Background

### 2.1 QELM Framework

{% raw %}
QELMs are quantum-classical hybrid models that retain the principles of classical-only Extreme Learning Machines (ELMs). A QELM comprises a Parametrised Quantum Circuit (PQC) and a classical regression model.

- **PQC structure:**
  - Accessible qubits (input nodes)
  - Hidden qubits (hidden nodes)

The PQC transforms input data by combining an encoding scheme on the accessible qubits with a set of reservoir dynamics on both the accessible and hidden qubits. The resulting quantum state is measured on a set of \(M\) unique observables \(\{O_k\}_{k=1}^M\), whose expectation values \(\langle O_k \rangle\) provide an encoding vector. The classical regression model is trained to predict labels based on this vector.

- **Key properties:**
  - PQC is fixed during training; only the regression model is tuned.
  - The maximum size of the encoding vector scales exponentially with PQC size: for single-qubit observables limited to \(\sigma_x, \sigma_y, \sigma_z, I\), there are \(4^{n_q}\) unique system observables for \(n_q\) qubits.
  - QELMs allow fast training and can be adapted for different tasks by adjusting the regression output.
{% endraw %}

### 2.2 Fourier Representation Analysis

{% raw %}
The PQC outputs a vector of expectation values of observables. For a single observable \(M\):

$$
\langle \psi | M | \psi \rangle, \quad |\psi\rangle = U(x)|0\rangle
$$

where \(U(x)\) is the parametrised unitary of the PQC. \(U(x)\) can be decomposed into \(L\) layers, each consisting of a trainable gate \(W\) followed by a parametrised encoding gate \(S(x)\) applying single-qubit rotations according to a Hamiltonian \(H\):

$$
S(x) = \exp(i H x), \quad U(x) = W^{(L+1)} S(x) W^{(L)} \dots W^{(1)}
$$

After diagonalising \(H = V^\dagger H' V\), the PQC output state can be written as:

$$
|\psi\rangle = \sum_{J \in [d]^L} \exp(-i\Lambda_J x) W^{(L+1)} \dots W^{(1)} |0\rangle
$$

The expectation value \(f(x)\) has the Fourier representation:

$$
f(x) = \sum_{\omega \in \Omega} c_\omega \exp(i \omega x), \quad \Omega = \{\Lambda_K - \Lambda_J\}
$$

For QELMs, the regression model predicts the label through a weighted sum of the PQC outputs:

$$
f_\eta(x) = \sum_{k=1}^{M} \eta_k \langle O_k \rangle_x = \sum_{\omega \in \Omega} b_\omega \exp(i \omega x)
$$
{% endraw %}

### 2.3 Data Encoding

{% raw %}
Classical input \(x\) is encoded via a parametrised unitary \(U(x)\) using single-qubit Pauli rotation gates:

$$
U_k(x) = \exp(i H_k x)
$$

- **Pauli re-uploading:** same Hamiltonian for all qubits (\(H_k = H\)) → limited \(\Omega\)
- **Exponential encoding:** unique generators with exponentially separated eigenvalues → largest \(\Omega\)
- **Quadratic encoding:** intermediate approach with \(H_k = k^2 \sigma_z / 2\)

The Fourier-expressivity \(F[f_\eta]\) is bounded by:

$$
F[f_\eta] \le \min\{M, |\Omega|\}
$$
{% endraw %}

### 2.4 Reservoir Evolution

{% raw %}
Reservoir unitaries ensure the model is rich (many non-zero Fourier coefficients). Effective reservoirs:

- Haar random unitary → uniform distribution of unitaries
- 1D chaotic Ising model:

$$
H_{\text{Ising}} = J \sum_{i=1}^{n-1} Z_i Z_{i+1} + B_z \sum_i Z_i + B_x \sum_i X_i
$$

- Chaotic parameters: \(J=-1, B_x=0.7, B_z=1.5\)
- Small encoding changes lead to diverse frequency spectra.
{% endraw %}

### 2.5 Limitations

- Exponential concentration of observable expectation values → practical implementation may require an exponential number of measurements.
- In simulations, exact expectation values are accessible, mitigating this issue.

