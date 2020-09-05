Elmer Release Notes for version 8.5
===================================

Previous release: **8.4**  
Period covered: **18 Dec 2018 - 30 Aug 2020**  
Number of commits: **~1140** (excluding merges)  

These release notes provide information on the most essential changes.
You can get a complete listing of commit messages, for example, with:  
git log --since="2018-12-18"  > log.txt

Apart from the core Elmer team at CSC (Juhani K., Mika M., Juha R., Peter R., Thomas Z.)
git log shows contributions from from Daniel B., Denis C., Eef v. D., Eelis T., Fabien G-C,
Foad S. F., Fredrik R., Olivier G., Joe T., Luz P., Mondher C., Rupert G., Sami I.,
Sami R., Samuel C., and Saeki T. have contributed to making this release. 

Additionally there are many ongoing developments in several branches
that have not been merged to this release, and are therefore not covered here. 
Also sometimes the code has been passed on by the original author by means other than the
git, and in such cases the names may have been accidentally omitted.

The contribution of all developers is gratefully acknowledged! 


New Solver Modules
------------------

### IncompressibleNSVec
- Incompressible Navier-Stokes solver utilizing vectorized and threaded assembly
- Includes built-in support for block preconditioning (Schur complement approximation included)
- Includes non-newtonian material laws
- Intended for Elmer/Ice community but also other may find it usefull. 


### BeamSolver
- The Timoshenko solver for elastic beams embedded in 3-D space.

### GmshReader
- Reads the mesh and results from simple Gmsh file format (that can be written by ElmerSolver as well)
- Solver includes interpolation of the fields to the current mesh
- May be used for hierarchical simulations where results are inherited from previous simulations


Enhanced Solver Modules
-----------------------

### ResultOutputSolver
- Vtu format:
    - Enable saving of pieces i.e. bodies and boundaries 
    - Improved saving of elemental, DG and IP fields
- Gmsh format:
    - Improved use of masking features in output

### StructuredMeshMapper
- Enable arbitrary number of layers, before limited to three. 


### HeatSolver
- New tentative vectorized version: HeatSolverVec
- Enable symmetric 3D cases for view factor computation. 
- Make Gebhart factors linear system symmetric, if possible "ViewFactor Symmetry".

### StressSolver
- Added Maxwell visco-elastic model to linear elasticity solver
- possible also to be run incrompressible (introducing pressure variable)
- optional pre-stress advection term for layered Earth-deformation model

ElmerSolver library functionality
---------------------------------

### Treatment of Block systems
- The block approach for solving complex problems has been enhanced.
  Currently the block approach can be used at several ways during some stage of the solution.
     1. Split up monolithic equations into subproblems that are easier to solve (e.g. IncompressibleNS)
     2. Combine linear multiphysical coupled problems into a block matrix (e.g. FSI problems)
- For problems belonging to class 1) we may perform recreation of a monolithic matrix. This will
  allow better use of standard linear algebra to utilize direct solvers, or change the system to
  be harmonic or eigenvalue problem.

### More economical integration rules
- A collection of economical Gauss quadrature rules for prismatic elements are introduced to replace
    tensor product rules for quadrilateral p-elements when 1 < p <= 8. The tensor
    product rule with n = (p+1)**2 points is now replaced by more economical ones.

### Conforming BCs by elimination
- System can identify conforming boundaries such that dofs related to nodes or edges on opposing sides may be
  assembled into one degree of freedom.
- This decreases the size of the linear system and is numberically favourable.
- Antiperiodicity may be included. For vector valued problems all components must be treated alike. 
- Conforming BCs for edge dofs may consider direction of edge.
- See test cases with "Apply Conforming BCs" and "Conforming BC" defined.
    
### Improved internal partitioning with Zoltan
- Enable internal partitioning with Zoltan to honor connected boundaries.

### Enable primary solver to call other solvers
- Enables calling before and after solving the primary problem.
- Also possible to call before and after each nonlinear iteration.

### Anderson Acceleration for linear systems
- Implemented a version of Anderson Acceleration where previous solutions and
  residuals are used to accelerate the nonlinear convergence.
- May increase linear convergence to quadratic, quadratic convergence (Newton's method) is not improved.

### Run Control
- Enable external loop control over the simulation.
- May be used in optimization and parametric scanning etc.
- Applicable also to transient system as the variable "time" is not used for the control level. 



ElmerGrid
---------
- Fixes for UNV, mptxt and Gmsh file format import.
- Tentative reader for FVCOM format
- Add possibility to define seed for Metis partitioning (-metisseed).
- Maintain entity names in extrusion



ElmerGUI
--------
- Include many improvements by Saeki

Configuration & Compilation
---------------------------
    
- cmake improvements:


Elmer/Ice
---------
- New features in Elmer/Ice are documented in elmerfem/elmerice/ReleaseNotes/release_elmerice_8.5.txt


