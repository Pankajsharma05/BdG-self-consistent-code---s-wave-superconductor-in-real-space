# BdG-self-consistent-code---s-wave-superconductor-in-real-space
# Self-Consistent s-Wave Superconductor Gap via Bogoliubov-de Gennes Method

This repository contains a Python Jupyter Notebook that performs a self-consistent mean-field calculation for the s-wave superconducting order parameter ($\\Delta$) on a 2D square lattice. The code uses the Bogoliubov-de Gennes (BdG) formalism to iteratively solve for the gap $\\Delta(T)$ as a function of temperature T. The result illustrates the second-order phase transition from a superconducting state to a normal metallic state at the critical temperature ($T\_c$).

-----

## Final Output 📊

The code produces the plot attached in pdf, showing the superconducting gap vanishing at the critical temperature $T\_c$:

-----

## Key Features

  - **Tight-Binding Model**: Implements a 2D tight-binding Hamiltonian on a square lattice with open boundary conditions.
  - **BdG Formalism**: Constructs the Bogoliubov-de Gennes Hamiltonian for an attractive Hubbard model under the mean-field approximation for s-wave singlet pairing.
  - **Self-Consistent Loop**: Iteratively diagonalizes the BdG Hamiltonian and updates the order parameter until convergence is reached.
  - **Temperature Dependence**: Calculates the converged order parameter over a range of temperatures to map out the $\\Delta(T)$ curve.
  - **Vectorized Implementation**: Uses NumPy for efficient, vectorized calculations, making the process fast and scalable.

-----

## Theoretical Background 🔬

The calculation starts with the attractive Hubbard model Hamiltonian:

$$\mathcal{H} = -t \sum_{\langle i,j \rangle, \sigma} (c_{i\sigma}^\dagger c_{j\sigma} + \text{h.c.}) - \mu \sum_{i, \sigma} n_{i\sigma} - U \sum_i n_{i\uparrow} n_{i\downarrow}$$

where **t** is the nearest-neighbor hopping, **μ** is the chemical potential, and **U** is the attractive on-site interaction strength ($U\>0$).

In the mean-field approximation, the interaction term is decoupled, leading to the s-wave superconducting order parameter:

$$\Delta_i = U \langle c_{i\downarrow} c_{i\uparrow} \rangle$$

The system can then be described by the Bogoliubov-de Gennes (BdG) Hamiltonian. This code constructs the BdG matrix in a real-space basis. By diagonalizing this Hamiltonian, we obtain the quasiparticle energies $E\_n$ and the corresponding eigenvectors, which contain the Bogoliubov coefficients ($u, v$).

The order parameter must be determined self-consistently. The new gap $\\Delta\_i'$ is calculated from the eigenvectors of the Hamiltonian constructed with the old gap $\\Delta\_i$:

$$\Delta'_i = -U \sum_{n} \left( u_{i\downarrow,n} v_{i\uparrow,n}^* (1 - f(E_n)) + u_{i\uparrow,n} v_{i\downarrow,n}^* f(E_n) \right)$$

where $f(E\_n) = (1 + e^{E\_n / k\_B T})^{-1}$ is the Fermi-Dirac distribution. This process is repeated until $\\Delta' \\approx \\Delta$.

-----

## Code Structure ⚙️

The Jupyter Notebook is organized into the following main cells:

1.  **Imports**: Imports `numpy` and `matplotlib`.
2.  **Hamiltonian Setup**:
      - Defines the lattice size (`nx`, `ny`) and physical parameters (`t`, `mu`, `U_0`, etc.).
      - Constructs the nearest-neighbor list for an open boundary condition lattice.
      - Builds the kinetic part of the Hamiltonian (`H`).
      - Assembles the full $4N \\times 4N$ BdG Hamiltonian (`H_BdG`) with an initial guess for $\\Delta$.
      - Performs a single diagonalization to show the initial quasiparticle spectrum.
3.  **`update_delta` function**:
      - A vectorized function that takes the BdG eigenvalues (`E`) and eigenvectors (`W`) to calculate the updated order parameter array `arr_delta` based on the self-consistency equation.
4.  **Main Loop and Plotting**:
      - The `delta_vs_Temp` function contains the self-consistent loop that iterates until the value of $\\Delta$ converges for a given temperature.
      - The main script block loops over a defined temperature range (`temp`), calls `delta_vs_Temp` for each point, and stores the converged gap value.
      - Finally, it uses `matplotlib` to generate and save the plot of $\\Delta(T)$ vs. $T$.

-----

## How to Run 🚀

### Dependencies

Ensure you have the required Python libraries installed:

```bash
pip install numpy matplotlib
```

### Execution

1.  Clone the repository:
    ```bash
    git clone <repository-url>
    cd <repository-directory>
    ```
2.  Open the Jupyter Notebook:
    ```bash
    jupyter notebook main_s_wave__self_consistent_delta_vs_Temp__BdG_code.ipynb
    ```
3.  Run all cells in the notebook. The calculation may take a few moments. Upon completion, the plot will be displayed and saved as `Delta_vs_T_BCS_s_wave_superconductor_calculated_self_consistently.pdf`.

-----

## Parameters

The key physical parameters can be modified in the second cell of the notebook:

  - `nx`, `ny`: Number of sites along the x and y axes of the lattice.
  - `t`: Hopping constant (set to 1).
  - `mu`: Chemical potential.
  - `U_0`: On-site attractive interaction strength.
  - `ite`: Maximum number of iterations for the self-consistent loop.
  - `temp`: The NumPy array defining the temperature range for the calculation. (last cell)
