# ANSYS APDL Validation: CFRP Composite Tube Buckling & Failure Analysis

**Author:** M. Yousuf Baig

---

## Project Overview

This repository contains the complete ANSYS APDL simulation scripts, finite element methodology, and numerical validation documentation for the structural analysis of carbon/epoxy filament-wound composite cylindrical tubes under axial compression.

The primary objective of this study is to benchmark and validate the analytical, numerical, and experimental failure mechanisms presented in the research literature.

> **Reference Paper:** Almeida Jr. et al., *"Buckling and post-buckling of filament wound composite tubes under axial compression: Linear, nonlinear, damage and experimental analyses."*

### Investigated Laminate Configuration

* **Stacking Sequence:** `[±75/±55/±89.6]FW` (thick multi-layered composite cylinder)
* **Primary Mechanism:** Contrast between elastic instability (buckling) and physical material shear failure

---

## Geometric & Material Specifications

| Parameter                      |         Value | Details                                                 |
| ------------------------------ | ------------: | ------------------------------------------------------- |
| **Inner Radius (`R`)**         |  **68.00 mm** | Outer Radius `Ro = 69.56 mm`                            |
| **Gage Length (`L`)**          | **192.00 mm** | Effective test length                                   |
| **Total Wall Thickness (`t`)** |   **1.56 mm** | 6-ply multi-layered composite wall                      |
| **±75° Ply Thickness**         |   **0.27 mm** | Deduced from single-layer baseline data (`0.54 mm / 2`) |
| **±55° Ply Thickness**         |   **0.28 mm** | Deduced from single-layer baseline data (`0.56 mm / 2`) |
| **±89.6° Ply Thickness**       |   **0.23 mm** | Deduced from single-layer baseline data (`0.46 mm / 2`) |

---

## Finite Element Discretization & Setup

### 1. Mesh Density & Kinematic Discretization

* **Mesh Grid:** `108 × 45` quadrilateral shell elements

  * 108 axial divisions
  * 45 circumferential divisions
* **Element Type:** `SHELL181`
* **Formulation:** Equivalent Single Layer (ESL)

**Rationale:** Initial coarse meshing caused artificial mathematical locking and artificially increased the critical buckling multipliers. Refining the model to a `108 × 45` grid provided sufficient kinematic degrees of freedom for the shell to deform into the required theoretical mode shapes.

### 2. Boundary Conditions

* **Bottom Edge (`Z = 0`):** Fully clamped

  * `UX = 0`
  * `UY = 0`
  * `UZ = 0`
  * `ROTX = 0`
  * `ROTY = 0`
  * `ROTZ = 0`

* **Top Edge (`Z = L`):**

  * `UX = 0`
  * `UY = 0`
  * `UZ = Free`

The top edge is therefore constrained radially and circumferentially while remaining free in the axial direction.

---

## Summary of Key Validation Results

| Assessment Milestone                    | Theoretical / Paper Target |        ANSYS APDL Numerical Result | Experimental Reality | Notes & Mode Shapes                                           |
| --------------------------------------- | -------------------------: | ---------------------------------: | -------------------: | ------------------------------------------------------------- |
| **Linear Eigenvalue Load (Table 3)**    |                  106.48 kN |                      **106.48 kN** |                    — | Verified using 894.43 N nodal force bypass                    |
| **Buckling Mode Shapes (Fig. 9)**       |             `m = 9, n = 5` | **Edge-Bulge ("Elephant's Foot")** |                    — | Nodal restraint artifact vs. theoretical `n = 5` global lobes |
| **Nonlinear Limit Load (Fig. 10f)**     |                Limit Crash |                       **83.55 kN** |                    — | Load factor `0.417724` under 200 kN reference load            |
| **Physical Material Failure (Fig. 2b)** |                          — |                                  — |        **127.59 kN** | Failure through transverse shear and in-plane shear           |

---

## Key Physical & Boundary Insights

### 1. Theoretical Instability vs. Physical Failure

While the linear eigenvalue analysis predicts an elastic buckling load of **106.48 kN** and the nonlinear arc-length analysis produces a limiting load of **83.55 kN**, physical testing reaches a failure load of **127.59 kN**.

The experimental results indicate that the thick composite tube does not undergo physical failure at the predicted elastic buckling loads. Instead, failure occurs through transverse and in-plane shear mechanisms at **127.59 kN**.

### 2. Eigenvalue Nodal Force Bypass

To achieve an eigenvalue multiplier of approximately **1.0** at **106.48 kN**, an isolated nodal load of **894.43 N per node** was applied around the circumference.

This loading approach provides the required reference force for the eigenvalue calculation.

### 3. Boundary-Induced Mode Discrepancy

The theoretical closed-form linear solution predicts a global non-axisymmetric diamond-lobe mode:

```text
m = 9
n = 5
```

However, the APDL model produces a localized edge-bulging **"Elephant's Foot"** mode.

The radial constraints:

```text
UX = 0
UY = 0
```

at the rigid rims suppress radial deformation at the boundaries. For the `1.56 mm` thick composite wall, this produces localized Poisson expansion and associated bending near the constrained region.

As a result, the APDL eigenvector develops localized edge bulging rather than the distributed global `n = 5` lobe pattern predicted by the theoretical solution.

This demonstrates the sensitivity of linear eigenvalue mode shapes to boundary conditions. In this model, the calculated eigenvector is strongly influenced by the boundary constraints and should therefore be distinguished from the experimentally observed material failure path.

---

## Repository File Structure

```text
.
├── README.md
├── scripts/
│   ├── script(eigen and modeshape).txt
│   └── script(nonlinear and graph).txt
└── Validation_Plots/
```

### Script Descriptions

**`script(eigen and modeshape).txt`**
Stage 1: Laminate setup, mesh generation, boundary conditions, circumferential nodal loading, and linear eigenvalue buckling analysis.

**`script(nonlinear and graph).txt`**
Stage 2: Rigid contact modeling, nonlinear Riks/arc-length analysis, and POST26 load-displacement extraction.

**`Validation_Plots/`**
Contains mode shape contours, load-deflection plots, and other simulation figures.

---

## How to Run the APDL Scripts

The finite element validation pipeline is executed in two sequential stages using the macro files in the `scripts/` directory.

### Stage 1: Linear Eigenvalue Buckling Analysis

1. Open **ANSYS Mechanical APDL Interactive**.
2. Set the working directory to the project folder.
3. Select **File → Read Input From...**
4. Select:

```text
scripts/script(eigen and modeshape).txt
```

Alternatively, enter the following command in the APDL command line:

```text
/INPUT,'scripts/script(eigen and modeshape)','txt'
```

The script:

* Defines the orthotropic material properties.
* Defines the `[±75/±55/±89.6]FW` ply stacking sequence.
* Generates the `108 × 45` shell mesh.
* Applies the `894.43 N` circumferential nodal force bypass.
* Applies the specified boundary conditions.
* Executes the linear eigenvalue buckling analysis using `ANTYPE, BUCKLE`.

### Stage 2: Nonlinear Arc-Length (Riks) Analysis

1. In APDL, select **File → Read Input From...**
2. Select:

```text
scripts/script(nonlinear and graph).txt
```

Alternatively, enter:

```text
/INPUT,'scripts/script(nonlinear and graph)','txt'
```

The script:

* Builds the upper and lower rigid compression plates.
* Defines `TARGE170` / `CONTA175` contact elements.
* Applies a friction coefficient of `μ = 0.01`.
* Enables large-deflection nonlinear analysis.
* Enables Riks/arc-length stepping using `ARCLEN, ON`.
* Extracts POST26 time-history reaction forces and axial displacement.

---

## Author

**M. Yousuf Baig**
