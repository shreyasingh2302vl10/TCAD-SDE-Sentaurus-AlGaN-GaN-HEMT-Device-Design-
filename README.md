# AlGaN/GaN HEMT Device Design Using Sentaurus TCAD (SDE)

## Description
Designed and developed an AlGaN/GaN HEMT device structure in Sentaurus TCAD (SDE), including geometry creation, material definition, contact engineering, doping profile implementation, and mesh generation for power electronic applications.

## Implementation Workflow

### 1. Parameter Definition
- Defined device dimensions and layer thicknesses.
- Parameterized source, gate, drain, substrate, buffer, barrier, and p-GaN regions.

### 2. Coordinate Generation
- Generated scalable X-axis and Y-axis coordinates for device construction.

### 3. Structure Creation
- Created:
  - Silicon Substrate
  - GaN Buffer Layer
  - AlGaN Barrier Layer
  - p-GaN Gate Region
  - Source Region
  - Drain Region
  - Gate Metal Electrode

### 4. Doping Assignment
- Applied substrate background doping.
- Assigned buffer doping.
- Implemented Mg doping in the p-GaN region.
- Defined heavily doped source and drain regions.

### 5. Contact Definition
- Defined Source (S), Gate (G), and Drain (D) contacts.
- Assigned contacts to corresponding device boundaries.

### 6. Mesh Refinement
- Applied localized mesh refinement in critical regions:
  - GaN Buffer
  - AlGaN Barrier
  - Gate Region

### 7. Model Generation
- Generated the final device structure.
- Built the mesh and exported the model for further TCAD studies.

## Tools Used
- Synopsys Sentaurus TCAD
- Sentaurus Structure Editor (SDE)
- Scheme-Based Scripting
