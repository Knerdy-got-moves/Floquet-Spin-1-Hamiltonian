# Floquet-Spin-1-Hamiltonian

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Knerdy-got-moves/Floquet-Spin-1-Hamiltonian/blob/main/Hilbert_space_fragmentation_in_spin_1_floquet_Hamiltonian.ipynb)

A numerical study of Hilbert space fragmentation in periodically-driven (Floquet) spin-1 quantum systems using exact diagonalization methods.

## Overview

This repository contains computational tools and simulations for studying **Hilbert space fragmentation** in a Floquet-driven spin-1 Hamiltonian. The system exhibits exotic quantum dynamics where the Hilbert space breaks into disconnected fragments, preventing thermalization and leading to persistent quantum memory effects.

## Physics Background

### The Model

We study a one-dimensional spin-1 quantum system described by a time-dependent Hamiltonian:

```
H(t) = H₁ + H₂(t)
```

where:
- **Hopping term**: `H₁ = J Σᵢ (Sᵢ⁺ Sᵢ₊₁⁻ + H.c.)`
- **Driving term**: `H₂(t) = Σᵢ V(t) (Sᵢᶻ)²`

Here, `Sᵢ±` and `Sᵢᶻ` are the standard spin-1 operators at site `i`. The driving is typically chosen as:
- `V(t) = V₀ cos(ωt)` with `V₀ >> J` (high-frequency regime)
- Period: `T = 2π/ω`

### Floquet Hamiltonian

In the high-frequency limit, the effective Floquet Hamiltonian can be derived using Magnus expansion:

```
H_F = (iℏ/T) log[U(T,0)]
```

The effective Hamiltonian includes modified hopping terms with amplitude renormalization and phase factors dependent on the local spin configuration through the operator:

```
X̂ᵢ = [1/2 - (Sᵢᶻ)²] + [1/2 - (Sᵢ₊₁ᶻ)²]
```

### Hilbert Space Fragmentation

The system conserves:
1. **Total number of +1 spins** (N₊)
2. **Total number of -1 spins** (N₋)

Due to constrained dynamics (e.g., vacancy-assisted hopping), the Hilbert space within each (N₊, N₋) sector fragments into disconnected components. This leads to:
- **Absence of thermalization**: Initial states retain memory indefinitely
- **Emergent quantum many-body scars**: Special states with revivals
- **Fragmentation-protected quantum information**

## Features

- ✅ **Time Evolution Simulation**: Numerical integration of the time-dependent Schrödinger equation
- ✅ **Fragmentation Analysis**: Automated detection of disconnected Hilbert space fragments
- ✅ **Observable Tracking**: Local magnetization `⟨Sᵢᶻ⟩` dynamics
- ✅ **Sector Analysis**: Study specific (N₊, N₋) charge sectors
- ✅ **Interactive Setup**: Flexible initial state specification

## Installation

### Requirements

- Python 3.7+
- NumPy
- SciPy
- Matplotlib
- [QuSpin](https://weinbe58.github.io/QuSpin/) (Quantum many-body spin system package)

### Setup

The easiest way to run this code is via Google Colab (click the badge at the top).

For local installation:

```bash
# Clone the repository
git clone https://github.com/Knerdy-got-moves/Floquet-Spin-1-Hamiltonian.git
cd Floquet-Spin-1-Hamiltonian

# Install dependencies
pip install quspin numpy scipy matplotlib
```

## Usage

### Running the Simulation

Open the Jupyter notebook:

```bash
jupyter notebook Hilbert_space_fragmentation_in_spin_1_floquet_Hamiltonian.ipynb
```

Or run it directly in [Google Colab](https://colab.research.google.com/github/Knerdy-got-moves/Floquet-Spin-1-Hamiltonian/blob/main/Hilbert_space_fragmentation_in_spin_1_floquet_Hamiltonian.ipynb).

### Key Parameters

The simulation allows you to configure:

```python
L = 5           # Number of sites
J = 1.0         # Hopping strength
V0 = 1000.0     # Driving amplitude (V0 >> J for high-frequency regime)
T_period = 2π/V0  # Driving period
n_periods = 10  # Number of periods to simulate
```

### Initial State Specification

The code supports multiple input formats for the initial spin-1 product state:

1. **String notation**: `"20121"` (0→-1, 1→0, 2→+1)
2. **Spin values**: `-1 0 +1 -1 0` (space-separated)
3. **Patterns**: 
   - `"alt"`: Alternating pattern (even sites: ±1, odd sites: 0)
   - `"random"`: Fully random configuration

### Fragmentation Analysis

The code includes tools to analyze Hilbert space fragmentation:

```python
# Specify sector by number of +1 and -1 spins
N_plus = 2   # Number of +1 spins
N_minus = 3  # Number of -1 spins

# The code will:
# 1. Enumerate all basis states in the sector
# 2. Build the Hamiltonian connectivity graph
# 3. Find disconnected fragments using graph algorithms
# 4. Report fragment sizes and representative states
```

## Example Output

### Time Evolution

The simulation produces:
- **Magnetization dynamics**: Time-dependent local `⟨Sᵢᶻ⟩(t)` for each site
- **Conservation checks**: Verification of N₊ and N₋ conservation
- **Statistical measures**: Initial/final magnetization, max/min values

Example output:
```
Simulation parameters:
Number of sites: 5
Hopping strength J: 1.0
Driving amplitude V0: 1000.0
Driving period T: 0.006283
Number of periods simulated: 10

Initial average magnetization: 0.2000
Final average magnetization: 0.2000
Max |<S^z>|: 1.0000
Min |<S^z>|: 0.0000
```

### Fragmentation Results

Example fragmentation analysis:
```
L=8, PBC=False, rule=vacancy_assisted
sector (N_+,N_-) = (2,3)
sector size (combinatorial) = 560
sector size (enumerated)    = 560
number of fragments         = 10
fragment sizes (sorted)     = [56 56 56 56 56 56 56 56 56 56]

representatives (one per fragment):
  fragment   0: 22000111
  fragment   1: 20200111
  ...
```

## Code Structure

The notebook is organized into sections:

1. **Setup**: Package installation and imports
2. **Time Evolution**: Floquet-driven dynamics simulation
3. **Fragmentation Analysis**: Hilbert space connectivity analysis
4. **Visualization**: Plotting routines for dynamics and statistics

### Key Components

- **State enumeration**: Generates all basis states in a given (N₊, N₋) sector
- **Hamiltonian construction**: Uses QuSpin to build sparse matrix representations
- **Graph analysis**: Identifies fragments via connected components algorithm
- **Time evolution**: Numerical integration using matrix exponentials

## Physical Insights

### Move Rules

The code supports different dynamics rules:
- `"vacancy_assisted"`: Spins can only hop to vacant (0) sites
- `"all_swaps"`: Any two spins on neighboring sites can swap

The choice of move rule dramatically affects fragmentation structure.

### Conservation Laws

Strict conservation of N₊ and N₋ arises from:
- U(1) × U(1) symmetry of the Hamiltonian
- No processes that create/annihilate spin excitations

### Quantum Memory

States in different fragments never mix, allowing:
- Persistent oscillations without damping
- Quantum information storage
- Non-ergodic dynamics

## References

This work builds upon concepts from:

1. **Hilbert Space Fragmentation**: 
   - Sala et al., Phys. Rev. X 10, 011047 (2020)
   - Khemani et al., Phys. Rev. B 101, 174204 (2020)

2. **Floquet Engineering**:
   - Eckardt, Rev. Mod. Phys. 89, 011004 (2017)
   - Bukov et al., Adv. Phys. 64, 139 (2015)

3. **QuSpin Documentation**:
   - [QuSpin GitHub](https://github.com/weinbe58/QuSpin)
   - [QuSpin Documentation](https://weinbe58.github.io/QuSpin/)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source and available under the MIT License.

## Citation

If you use this code in your research, please cite:

```bibtex
@misc{floquet_spin1_fragmentation,
  author = {Knerdy-got-moves},
  title = {Floquet-Spin-1-Hamiltonian: Hilbert Space Fragmentation in Floquet Spin-1 Systems},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/Knerdy-got-moves/Floquet-Spin-1-Hamiltonian}
}
```

## Contact

For questions or collaboration opportunities, please open an issue on GitHub.

---

**Keywords**: Floquet systems, Hilbert space fragmentation, spin-1 chains, quantum dynamics, exact diagonalization, non-equilibrium quantum physics, quantum many-body systems
