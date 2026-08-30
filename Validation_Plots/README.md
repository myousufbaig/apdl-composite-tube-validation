# Validation Plots & Simulation Assets

This directory contains the primary ANSYS APDL GUI screenshots, finite element model displays, and POST26 response plots generated during the numerical validation of the `[±75/±55/±89.6]FW` composite cylinder with a total wall thickness of `1.56 mm`.

The figures document the mesh, eigenvalue results, boundary and contact setup, nonlinear solution behavior, and load-displacement response.

---

## Figure Catalog

### 1. `01_eigenmode_wireframe_mesh.png` — Linear Eigenmode Wireframe Mesh

**APDL Details:** `POST1` deformed wireframe mesh plot (`STEP=1, SUB=2, FACT=1.00931`).

**Technical Caption:**

> **Linear Eigenmode Wireframe Mesh:** Deformed wireframe representation of the composite shell following the linear eigenvalue analysis. The `108 × 45` `SHELL181` mesh provides the discretization required to represent the shell deformation pattern under axial compression.

---

### 2. `02_eigenmode_3d_contour.png` — 3D Displacement Contour

**APDL Details:** `POST1` nodal total displacement contour (`USUM`, `STEP=1, SUB=5, FACT=1.20325`).

**Technical Caption:**

> **3D Eigenmode Displacement Contour:** Three-dimensional total displacement contour showing the deformation distribution over the cylindrical shell for the eigenvalue solution corresponding to the `106.48 kN` target buckling load.

---

### 3. `03_elephant_foot_side_view.png` — Edge-Bulge ("Elephant's Foot") Side View

**APDL Details:** Side-elevation total displacement contour (`USUM`, `FACT=1.00931`).

**Technical Caption:**

> **Boundary-Induced Edge Bulge ("Elephant's Foot"):** Side-view displacement contour showing localized radial deformation near the constrained bottom boundary at `Z = 0`. The radial constraints (`UX = 0`, `UY = 0`) restrict lateral expansion at the boundary and produce localized deformation near the support. The resulting APDL eigenmode differs from the global `m = 9, n = 5` theoretical mode shape.

---

### 4. `04_table3_eigenvalue_set_list.png` — `SET,LIST` Eigenvalue Results

**APDL Details:** APDL `SET,LIST` output showing the extracted eigenvalue results.

**Technical Caption:**

> **Eigenvalue Result Verification (`SET,LIST`):** APDL `SET,LIST` output showing the calculated eigenvalue load factors, including `1.0022`, `1.0607`, and `1.0610`. Combined with the `894.43 N` circumferential nodal loading used in the model, the results provide the numerical verification of the target `106.48 kN` eigenvalue load.

---

### 5. `05_rigid_region_assembly.png` — Rigid Region and Contact Assembly

**APDL Details:** `PREP7` element and constraint display showing rigid regions and loading assembly.

**Technical Caption:**

> **Rigid Constraint Region and Contact Assembly:** Finite element representation of the rigid loading regions and constraint spiders used to transfer the axial compression load into the composite shell. The assembly includes `R3D4` rigid elements with `CONTA175`/`TARGE170` contact elements and a friction coefficient of `μ = 0.01`. Pilot nodes `100016` and `100033` are used to apply and transfer the loading through the rigid regions.

---

### 6. `06_solver_convergence_norm.png` — Riks Arc-Length Convergence

**APDL Details:** APDL Solution Tracker showing Absolute Convergence Norm versus Cumulative Iteration Number.

**Technical Caption:**

> **Nonlinear Solution Convergence:** Solution-tracker output from the nonlinear arc-length analysis showing the convergence behavior as the load factor approaches `0.417724`. The loss of convergence corresponds to the limiting point reached by the nonlinear solution.

---

### 7. `07_post26_load_displacement.png` — POST26 Load-Displacement Response

**APDL Details:** `POST26` time-history response showing axial reaction force (`FZ_2`) against the solution history.

**Technical Caption:**

> **POST26 Load-Displacement Response:** Reaction-force history obtained from the nonlinear analysis. The response reaches a maximum load of approximately **83.55 kN**, corresponding to a load factor of `0.417724` under the `200 kN` reference load, followed by a reduction in load-carrying capacity.

---

## Summary

The figures document the primary stages of the APDL validation workflow:

1. Finite element mesh and eigenmode generation
2. Three-dimensional displacement evaluation
3. Boundary-induced deformation behavior
4. Eigenvalue result extraction
5. Rigid loading and contact setup
6. Nonlinear arc-length convergence
7. POST26 load-displacement response

