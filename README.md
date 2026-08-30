# apdl-composite-tube-validation
# ANSYS APDL validation study of CFRP tubes under compression, contrasting mathematical degradation with physical failure.


# ANSYS APDL Validation: CFRP Composite Tube Buckling and Failure Analysis

**Author:** M. Yousuf Baig

---

## 1. Project Overview

This repository contains the ANSYS APDL simulation scripts, finite element methodology, and numerical validation results for the structural analysis of carbon/epoxy filament-wound composite cylindrical tubes subjected to axial compression.

The primary objective of this study is to evaluate the buckling and failure behavior of a thick, multi-layered CFRP tube through analytical, numerical, and experimental comparison.

The analysis focuses on the distinction between elastic instability predicted by finite element analysis and the physical material failure observed during experimental testing.

### Investigated Laminate Configuration

* **Stacking Sequence:** `[±75/±55/±89.6]FW`
* **Structure:** Six-ply filament-wound composite cylinder
* **Loading:** Axial compression
* **Primary Mechanisms:** Elastic instability, nonlinear deformation, and material shear failure

---

## 2. Geometric and Material Specifications

| Parameter                  |     Value | Details                             |
| -------------------------- | --------: | ----------------------------------- |
| Inner Radius (`R`)         |  68.00 mm | Inner cylinder radius               |
| Outer Radius (`Ro`)        |  69.56 mm | Outer cylinder radius               |
| Gage Length (`L`)          | 192.00 mm | Effective test length               |
| Total Wall Thickness (`t`) |   1.56 mm | Six-ply composite wall              |
| ±75° Ply Thickness         |   0.27 mm | Based on single-layer baseline data |
| ±55° Ply Thickness         |   0.28 mm | Based on single-layer baseline data |
| ±89.6° Ply Thickness       |   0.23 mm | Based on single-layer baseline data |

The composite wall consists of six plies arranged according to the specified filament-wound stacking sequence.

---

## 3. Finite Element Discretization and Setup

### 3.1 Mesh Density and Element Formulation

The tube was modeled using `SHELL181` shell elements with an Equivalent Single Layer (ESL) formulation.

The final mesh consisted of:

* **Axial divisions:** 108
* **Circumferential divisions:** 45
* **Mesh:** `108 × 45` quadrilateral shell elements

Mesh density was refined during model development to improve the representation of the expected shell deformation.

Initial coarse meshes produced artificially high critical buckling multipliers because the available degrees of freedom were insufficient to represent the required deformation pattern.

The final `108 × 45` mesh provided sufficient kinematic freedom for localized and global shell deformation to develop.

### 3.2 Boundary Conditions

#### Bottom Edge — `Z = 0`

The bottom edge was fully constrained:

```text
UX   = 0
UY   = 0
UZ   = 0
ROTX = 0
ROTY = 0
ROTZ = 0
```

#### Top Edge — `Z = L`

The top edge was constrained in the radial and circumferential directions while remaining free in the axial direction:

```text
UX = 0
UY = 0
UZ = Free
```

This boundary condition permits axial compression while restricting lateral displacement at the loading edge.

---

## 4. Validation Results

| Assessment                | Target / Reference | ANSYS APDL Result | Experimental Result | Observation                           |
| ------------------------- | -----------------: | ----------------: | ------------------: | ------------------------------------- |
| Linear Eigenvalue Load    |          106.48 kN |         106.48 kN |                   — | Verified using 894.43 N nodal loading |
| Buckling Mode Shape       |     `m = 9, n = 5` |   Edge-bulge mode |                   — | Boundary-condition-dependent mode     |
| Nonlinear Limit Load      |         Limit load |          83.55 kN |                   — | Load factor = 0.417724                |
| Physical Material Failure |                  — |                 — |           127.59 kN | Transverse and in-plane shear failure |

---

## 5. Linear Eigenvalue Buckling

The linear eigenvalue analysis was performed using:

```text
ANTYPE, BUCKLE
```

The resulting critical load was:

**106.48 kN**

To obtain an eigenvalue multiplier of approximately 1.0 at the target load, an isolated nodal force of:

**894.43 N per node**

was applied around the circumference.

The resulting eigenvalue load matched the target value of **106.48 kN**.

### Buckling Mode Shape

The theoretical mode shape associated with the target solution is:

```text
m = 9
n = 5
```

corresponding to nine axial half-waves and five circumferential lobes.

However, the APDL model did not reproduce the global `m = 9, n = 5` deformation pattern. Instead, the calculated eigenvector exhibited a localized edge-bulging deformation near the constrained rim.

This difference is attributed to the applied boundary constraints and their influence on the shell deformation near the rigid supports.

---

## 6. Boundary-Induced Mode Discrepancy

The APDL model applies:

```text
UX = 0
UY = 0
```

at the top loading edge. These constraints suppress radial displacement at the boundary.

For the relatively thick composite wall (`t = 1.56 mm`), this restriction can produce localized deformation and bending near the supported region.

The resulting eigenvector develops an edge-bulging shape commonly described as an **"elephant's foot"** deformation.

This differs from the global non-axisymmetric diamond-lobe pattern associated with the theoretical `m = 9, n = 5` solution.

The result demonstrates the sensitivity of shell eigenvalue solutions to boundary conditions. In this model, the calculated linear eigenvector is strongly influenced by the radial constraints imposed at the rigid rim.

Therefore, the eigenvalue load and mode shape should be considered separately when evaluating the agreement between the numerical model and the theoretical solution.

---

## 7. Nonlinear Analysis

A nonlinear arc-length analysis was performed using:

```text
ARCLEN, ON
```

A reference load of **200 kN** was applied for the nonlinear solution.

The analysis reached a limiting load of:

**83.55 kN**

with a corresponding load factor of:

**0.417724**

The nonlinear analysis was used to evaluate the structural response beyond the initial linear eigenvalue condition.

---

## 8. Experimental Failure

The experimental specimen reached a physical failure load of approximately:

**127.59 kN**

The observed failure was associated primarily with:

* Transverse shear
* In-plane shear

This failure occurred above both the linear eigenvalue buckling load and the nonlinear numerical limit load.

---

## 9. Physical and Numerical Interpretation

The principal load levels obtained from the study are:

```text
Linear Eigenvalue Buckling     106.48 kN
Nonlinear Limit Load            83.55 kN
Experimental Failure           127.59 kN
```

The results demonstrate that the first elastic instability predicted by the numerical model does not directly correspond to the ultimate physical failure of the composite tube.

The linear eigenvalue analysis predicts elastic instability at **106.48 kN**, while the nonlinear arc-length analysis reaches a limiting load of **83.55 kN**.

In contrast, the experimental specimen sustained loading up to **127.59 kN** before physical material failure occurred.

The experimental failure was associated with transverse and in-plane shear rather than immediate structural collapse through the numerical elastic buckling mode.

This difference highlights the distinction between:

1. **Elastic buckling:** A geometric instability predicted by eigenvalue analysis.
2. **Nonlinear structural instability:** The response obtained from the nonlinear analysis.
3. **Material failure:** The physical failure mechanism observed in the composite specimen.

For thick composite tubes, these three conditions do not necessarily occur at the same load.

---

## 10. Repository Structure

```text
.
├── README.md
├── ansys_apdl_script.txt
└── Validation_Plots/
    ├── Mode_Shapes/
    ├── Load_Deflection/
    └── Results/
```

### Files

**`README.md`**
Project documentation, finite element methodology, validation results, and analysis observations.

**`ansys_apdl_script.txt`**
ANSYS Mechanical APDL macro containing the model definition and analysis procedures.

**`Validation_Plots/`**
Contains mode shape contours, load-deflection results, and other validation figures.

---

## 11. Running the APDL Script

1. Open **ANSYS Mechanical APDL**.
2. Set the working directory to the repository location.
3. Open the APDL command interface.
4. Read the input file:

```text
ansys_apdl_script.txt
```

Alternatively, use:

```text
File → Read Input From...
```

and select `ansys_apdl_script.txt`.

The macro performs the main stages of the analysis, including:

* Definition of material properties
* Definition of the Toray T700 / UF3369 epoxy material system
* Composite section definition
* `[±75/±55/±89.6]FW` laminate definition
* Cylinder geometry generation
* `108 × 45` shell meshing
* Application of boundary conditions
* Linear eigenvalue buckling analysis
* Rigid loading plate setup
* Nonlinear arc-length analysis
* Result extraction

---

## 12. Summary of Results

| Quantity                  |                         Result |
| ------------------------- | -----------------------------: |
| Inner Radius              |                       68.00 mm |
| Outer Radius              |                       69.56 mm |
| Gage Length               |                      192.00 mm |
| Wall Thickness            |                        1.56 mm |
| Laminate                  |            `[±75/±55/±89.6]FW` |
| Element Type              |                       SHELL181 |
| Mesh                      |                       108 × 45 |
| Target Eigenvalue Load    |                      106.48 kN |
| APDL Eigenvalue Load      |                      106.48 kN |
| Theoretical Mode          |                 `m = 9, n = 5` |
| APDL Mode                 | Edge-bulge / "elephant's foot" |
| Nonlinear Limit Load      |                       83.55 kN |
| Experimental Failure Load |                      127.59 kN |
| Eigenvalue Nodal Force    |                  894.43 N/node |
| Nonlinear Reference Load  |                         200 kN |
| Nonlinear Load Factor     |                       0.417724 |

---

## 13. Author

**M. Yousuf Baig**

