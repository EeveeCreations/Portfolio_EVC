# Quantum Extreme Learning Machines (QELM) Framework

## 2 Background

### 2.1 QELM Framework

QELMs are quantum-classical hybrid models that retain the principles of classical-only Extreme Learning Machines (ELMs). A QELM comprises a Parametrised Quantum Circuit (PQC) and a classical regression model.

- **PQC structure:**
  - Accessible qubits (input nodes)
  - Hidden qubits (hidden nodes)

The PQC transforms input data by combining an encoding scheme on the accessible qubits with a set of reservoir dynamics on both the accessible and hidden qubits. The resulting quantum state is measured on a set of \(M\) unique observables \(\{O_k\}_{k=1}^M\), whose expectation values \(\langle O_k 
angle\) provide an encoding vector. The classical regression model is trained to predict labels based on this vector.

- **Key properties:**
  - PQC is fixed during training; only the regression model is tuned.
  - The maximum size of the encoding vector scales exponentially with PQC size: for single-qubit observables limited to \(\sigma_x, \sigma_y, \sigma_z, I\), there are \(4^{n_q}\) unique system observables for \(n_q\) qubits.
  - QELMs allow fast training and can be adapted for different tasks by adjusting the regression output.

### 2.2 Fourier Representation Analysis

The PQC outputs a vector of expectation values of observables. For a single observable \(M\):

\[
\langle \psi | M | \psi 
angle, \quad |\psi
angle = U(x)|0
angle
\]

where \(U(x)\) is the parametrised unitary of the PQC. \(U(x)\) can be decomposed into \(L\) layers, each consisting of a trainable gate \(W\) followed by a parametrised encoding gate \(S(x)\) applying single-qubit rotations according to a Hamiltonian \(H\):

\[
S(x) = \exp(iHx), \quad U(x) = W^{(L+1)}S(x)W^{(L)}...W^{(1)}\]

After diagonalising \(H = V^\dagger H' V\), the PQC output state can be written as:

\[
|\psi
angle = \sum_{J \in [d]^L} \exp(-i\Lambda_J x) W^{(L+1)} ... W^{(1)} |0
angle
\]

The expectation value \(f(x)\) has the Fourier representation:

\[
f(x) = \sum_{\omega \in \Omega} c_\omega \exp(i\omega x), \quad \Omega = \{\Lambda_K - \Lambda_J\}
\]

For QELMs, the regression model predicts the label through a weighted sum of the PQC outputs:

\[
f_\eta(x) = \sum_{k=1}^{M} \eta_k \langle O_k 
angle_x = \sum_{\omega \in \Omega} b_\omega \exp(i \omega x)
\]

### 2.3 Data Encoding

Classical input \(x\) is encoded via a parametrised unitary \(U(x)\) using single-qubit Pauli rotation gates:

\[
U_k(x) = \exp(i H_k x)
\]

- **Pauli re-uploading:** same Hamiltonian for all qubits (\(H_k = H\)) → limited \(\Omega\)
- **Exponential encoding:** unique generators with exponentially separated eigenvalues → largest \(\Omega\)
- **Quadratic encoding:** intermediate approach with \(H_k = k^2 \sigma_z / 2\)

The Fourier-expressivity \(F[f_\eta]\) is bounded by:

\[
F[f_\eta] \le \min\{M, |\Omega|\}
\]

### 2.4 Reservoir Evolution

Reservoir unitaries ensure the model is rich (many non-zero Fourier coefficients). Effective reservoirs:

- Haar random unitary → uniform distribution of unitaries
- 1D chaotic Ising model:

\[
H_{	ext{Ising}} = J \sum_{i=1}^{n-1} Z_i Z_{i+1} + B_z \sum_i Z_i + B_x \sum_i X_i
\]

- Chaotic parameters: \(J=-1, B_x=0.7, B_z=1.5\)
- Small encoding changes lead to diverse frequency spectra.

### 2.5 Limitations

- Exponential concentration of observable expectation values → practical implementation may require an exponential number of measurements.
- In simulations, exact expectation values are accessible, mitigating this issue.

## 3 Method

### 3.1 Target Functions

Functions simulated:

1. Sphere function:
\[
f_{	ext{sphere}}(x) = \sum_{i=1}^{31} \alpha_k^2 + x^2 + eta_k^2
\]

2. Toy function:
\[
f_{	ext{toy}}(x) = \sum_{k=1}^{17} \alpha_k \cos(kx) + eta_k \sin(kx)
\]

3. Gaussian function:
\[
f_{GP}(x) = \sigma^2 \exp\left(-rac{|x-\mu|^2}{2l^2}
ight)
\]

- Input: 5000 equidistant points in \([0, 2\pi]\), 30% test.
- 6 accessible qubits, 0 hidden qubits.

### 3.2 Preprocessing

For Fashion-MNIST classification, preprocessing reduces input to two scalars per image:
- Centroid of intensity: \(\sqrt{x_c^2 + y_c^2}\)
- Shannon entropy: \(S = -\sum_{k=0}^{255} p_k \log(p_k)\)

### 3.3 Encoding Experiments

Three encoding schemes tested:
1. **Exponential encoding:** \(H_k = rac{1}{2}3^{k-1}\sigma_z\)
2. **Pauli re-uploading:** \(H_k = \sigma_z / 2\)
3. **Quadratic encoding:** \(H_k = k^2 \sigma_z / 2\)

Reservoirs:
- Haar random (QR decomposition)
- 1D chaotic Ising

### 3.4 Observables

- Pauli strings
- Linear independence ensures rich representations
- Suitable for simulations with small qubit numbers

### 3.5 QELM Settings

- Toy problem: 6 accessible, 0 hidden qubits
- Classification: 4 accessible, 2 hidden qubits, fully controllable regime
- Regression: linear regression
- Classification: ridge classifier

## 4 Results

### 4.1 Regression

| Target Functions | MSE (5000 samples) | MSE (200 samples) |
|-----------------|------------------|-----------------|
| \(f_{	ext{toy}}(x)\) | \(1.44 	imes 10^{-11} \pm 1.21 	imes 10^{-12}\) | \(120.63 \pm 6.03\) |
| \(f_{	ext{sphere}}(x)\) | \(72.53 \pm 1.45\) | \(33733.75 \pm 127.54\) |
| \(f_{GP}(x)\) | \(3.70 	imes 10^{-4} \pm 7.4 	imes 10^{-6}\) | \(0.059 \pm 1.18 	imes 10^{-3}\) |

- Toy function is fit almost perfectly.
- Gaussian function fitted closely.
- Sphere function has larger error due to scaling and over-smoothing.

Encoding performance:
- Exponential encoding significantly outperforms Pauli re-uploading and quadratic encoding due to exponential frequency scaling.

