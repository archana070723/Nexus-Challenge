# 
# 🌐 Global Industry Challenge — Nexus-Challenge – Phase 3

**Team Name**: BrainiaQ  
## QBraid Launch 
[<img src="https://qbraid-static.s3.amazonaws.com/logos/Launch_on_qBraid_white.png" width="150">](https://account.qbraid.com?gitHubUrl=https://github.com/archana070723/Nexus-Challenge.git)
---
We have used data from the 'allPlanesVariables27-Feb-2021.mat' file in '081820_355n' folder

# Quantum Clustering with Hybrid Classical-Quantum Kernels

## Overview

This repository presents a framework for hybrid classical-quantum clustering using kernel methods. It supports evaluating spike train data using both classical kernel PCA techniques and quantum fidelity-based distance metrics derived from quantum circuit statevectors or actual quantum hardware. The experiments demonstrate fidelity-preserving encodings and realistic benchmarks on IonQ, Rigetti, and IBM devices through Qbraid integration.

## Features

* Classical kernel PCA-based clustering
* Quantum statevector simulations via Qiskit Aer
* Hardware execution via IonQ, IBM, and Rigetti (through Qbraid)
* Fidelity-based quantum distance matrix generation
* Agglomerative clustering with spatial connectivity constraints
* GPU support detection

## Installation

Install all dependencies with:

```bash
pip install -q scikit-learn qbraid qiskit-ibm-runtime qiskit-aer qiskit-aer-gpu \
                qiskit-ionq amazon-braket-sdk qiskit-braket-provider qiskit-rigetti
```

> **Note**: You may see warnings about deprecated setup mechanisms (e.g., `rpcq`, `antlr4`). These are benign but will need future mitigation via PEP 517.

## Environment Setup

1. Register and configure your Qbraid and IBM API keys:

```python
from qbraid import QbraidProvider
qbraid_provider = QbraidProvider(api_key="<your-qbraid-key>")
qbraid_provider.save_config()
```

```python
from qbraid.runtime.ibm import QiskitRuntimeProvider
ibm_provider = QiskitRuntimeProvider("<ibm-api-key>", "<your-instance>", channel='ibm_cloud')
```

2. Check system capabilities:

```python
import subprocess
try:
    subprocess.check_output('nvidia-smi')
    print('Nvidia GPU detected!')
except Exception:
    print('No Nvidia GPU in system!')
```

## Usage

### 1. Data Loading and Preprocessing

```python
data = load_data('data_1.mat', plane=-1)
X = preprocess_spike_data(data[3])
```

### 2. Classical Kernel Distance

```python
dist_classical = classical_kernel_distance(X, True, n_components_pca=10, kernel_pca='linear', metric='euclidean')
```

### 3. Quantum Kernel Distance (Simulator)

```python
circuits = [build_neuron_circuit(x, False, depth=4) for x in X]
dist_quantum = quantum_kernel_matrix_sim(circuits, metric_q='fidelity')
```

### 4. Quantum Kernel Distance (Hardware)

```python
circuits = [build_neuron_circuit(x, True, depth=4) for x in X]
time, dist_quantum_hw = quantum_kernel_matrix_hardware(circuits, provider='ionq')
```

### 5. Clustering and Visualization

```python
linkage_matrix, labels = hierarchical_cluster(XYZ, dist_quantum)
plot_figures(XYZ, linkage_matrix, labels, metric='fidelity')
```

### 6. Experiments

To run a full experiment pipeline use Experiments-Final.ipynb file. 

Control parameters like plane selection, quantum circuit depth, kernel type, and number of clusters, e.t.c. can be choosen in the experiment section which inturn is used in the parameter grid defined in `selected_parameters`.

# Results Reproducability
Experiments-Final.ipynb, Exp_runtime.ipynb file can be executed to reproduce the clustering and runtime results, respectively, as mentioned in the analysis pdf. This information also be generally found in Experiments-Final.ipynb for experiment of ones choice.

## Result Analysis

Find reults and its analysis in Nexus-Phase3_Submission.pdf
