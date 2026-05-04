# Crystal Plasticity Simulation of Aluminum 6061-T6

This repository contains a simplified 1D Python simulation of a macroscopic stress-strain ($\sigma-\epsilon$) curve using Crystal Plasticity principles. The model simulates the deformation behavior of **Aluminum 6061-T6** by implementing a self-consistent iterative algorithm to separate elastic and plastic strain increments.

## Theory & Concept

When a metal deforms, the total strain ($\epsilon$) is the sum of recoverable (elastic) and permanent (plastic) deformation:
$\epsilon = \epsilon_{el} + \epsilon_{pl}$

Crystal plasticity links the macroscopic response of a material to its microscopic deformation mechanisms, primarily the movement of line defects called **dislocations** through the crystal lattice.

### The Algorithm (Self-Consistent Iterative Process)

To draw the stress-strain curve computationally, the simulation breaks the test down into small time steps ($\Delta t$). At each step, a total strain increment ($\Delta \epsilon$) is applied. The code uses an iterative loop to determine how much of this increment is elastic and how much is plastic:

1.  **Initial Guess:** Assume the entire strain increment is elastic ($\Delta \epsilon_e = \Delta \epsilon$).
2.  **Trial Stress:** Calculate the resulting stress using Hooke's Law ($\Delta \sigma = E \cdot \Delta \epsilon_e$).
3.  **Resolved Shear Stress:** Convert the macroscopic trial stress to a microscopic shear stress ($\tau$) on the slip systems using the Taylor Factor ($M$).
4.  **Excess Stress:** Calculate if the shear stress exceeds the material's internal resistance to slip ($g$). $\Delta \tau = \tau - g$.
    *   If $\Delta \tau \le 0$: The material is still in the linear elastic regime. The plastic strain is zero.
    *   If $\Delta \tau > 0$: The material yields. 
5.  **Plastic Flow (Orowan Equation):** If yielding occurs, calculate the plastic strain rate ($\dot{\gamma}$) based on dislocation movement:
    $\dot{\gamma} = \rho \cdot b \cdot v$ where $v$ is dislocation velocity, dependent on excess stress and the drag coefficient $B$.
6.  **Convergence:** Calculate a new elastic strain based on the newly found plastic strain. If the new elastic strain matches the initial guess, the step is solved. If not, update the guess and repeat (Fixed-point iteration).
7.  **Hardening:** Update the material's resistance ($g$) due to strain hardening before moving to the next time step.

## Material Parameters (Aluminum 6061-T6)

The simulation uses literature-backed parameters specific to Aluminum 6061-T6:

*   **Elastic Modulus ($E$):** 68,900 MPa. [1]
*   **Burgers Vector ($b$):** $2.86 \times 10^{-10}$ m. Derived from the lattice constant of Al ($0.4050$ nm). [2]
*   **Taylor Factor ($M$):** 3.07. Geometric factor linking macro and micro stresses for isotropic FCC metals. [3]
*   **Initial Shear Resistance ($\tau_0$):** ~90.0 MPa. Calculated backward from the macro yield strength ($\sim 276$ MPa) using $\tau_0 = \sigma_y / M$. [1][4]
*   **Initial Dislocation Density ($\rho$):** $10^{13} \: m^{-2}$. [5]
*   **Dislocation Drag Coefficient ($B$):** $10^{-4}$ Pa.s. Viscous resistance to dislocation motion. [6]
*   **Hardening Rate:** 1500.0 MPa. A phenomenological constant to fit the strain hardening curve. [5]

Muhammed Eren Balıbey

2540091015

COMPUTATIONAL MODELING AND SIMULATION OF MATERIALS


**References**

Callister, W. D., & Rethwisch, D. G. (2018). Materials Science and Engineering: An Introduction (10th ed.). John Wiley & Sons.

Reyes-Ruiz, C., et al. (2015). Texture and lattice distortion study of an Al-6061-T6 alloy produced by ECAP. Materials Transactions, 56(11), 1781-1786.

Zhang, K., et al. (2019). Assessment of advanced Taylor models... Int. Journal of Plasticity, 114, 144-160.

Asaro, R. J. (1983). Micromechanics of crystals and polycrystals. Advances in Applied Mechanics, 23, 1-115.

Lee, W. S., Shyu, J. C., & Chiou, S. T. (1999). Effect of strain rate on impact response and dislocation substructure of 6061-T6 aluminum alloy. Scripta materialia, 42(1), 51-56.

Kumar, A., Hauser, F. E., & Dorn, J. E. (1968). Viscous drag on dislocations in aluminum at high strain rates. Acta Metallurgica, 16(9), 1189-1197.
