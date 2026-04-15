# MD_quick_plot

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/15ddbCT7F7G_YPhXS4ctJ81nzAoShH6WH?usp=sharing)

## Overview

This tool is designed to **streamline the analysis of Molecular Dynamics (MD) simulation data** by simplifying the workflow to just two essential inputs:

1. **Structural file** (PDB, GRO)
2. **Trajectory file** (XTC, TRR, DCD)

No complex scripting required. Just load your files and run comprehensive analyses with publication-ready visualizations!

> **Main file:** `3_28_2026_md_quick_plot.py`

---

## Quick Start

### Click the badge above to open the notebook in Google Colab!

### Basic Usage
```python
# Initialize the analyzer
analyzer = MDAnalyzer(
    topology="your_structure.pdb",      # Your structural file
    trajectory="your_trajectory.xtc",   # Your trajectory file
    protein_selection="protein",         # Optional: customize protein selection
    ligand_selection="resname LIG",      # Optional: customize ligand selection
    dt_in_ps=None                        # Optional: override timestep (in ps)
)

# Run complete analysis suite
analyzer.run_complete_analysis(output_prefix="my_analysis")
```

---

## Analysis Features

This tool automatically generates the following analyses:

| Analysis | Description | Output |
|----------|-------------|--------|
| **RMSD** | Root Mean Square Deviation over time | Structural stability assessment |
| **RMSF** | Root Mean Square Fluctuation per residue | Residue flexibility mapping |
| **Rg** | Radius of Gyration over time | Protein compactness tracking |
| **FEL** | Free Energy Landscape (RMSD vs Rg) | Global minimum identification + frame extraction |
| **Binding Energy** | Protein-ligand MM interaction energy (Elec + VdW) | Interaction energy in kJ/mol |
| **P-L Distance** | Minimum protein-ligand distance | Contact monitoring (4 Å threshold) |
| **Frame Extraction** | Extract PDB at any time point (ns) | Snapshot for downstream analysis |

---

## Key Advantages

- **Simple Input**: Only requires topology + trajectory files
- **Timestep Override**: Correct wrong time axes with the `dt_in_ps` parameter
- **Automated Workflow**: Run all analyses with one command
- **Frame Extraction**: Export any trajectory frame as a PDB at a specified time (ns)
- **Global Minimum Detection**: FEL identifies the lowest-energy frame with time stamp
- **Publication-Ready Figures**: High-resolution (300 DPI) plots
- **No Installation Hassle**: Runs entirely in Google Colab with Google Drive integration
- **Flexible Selections**: Customizable MDAnalysis atom/residue selections
- **Comprehensive Output**: Statistical summaries included

---

## Input Requirements

### 1. Topology File

Supported formats:
- `.pdb` — Protein Data Bank format
- `.gro` — GROMACS structure file
- `.psf` — CHARMM/NAMD structure file
- `.top` — AMBER topology file

### 2. Trajectory File

Supported formats:
- `.xtc` — GROMACS compressed trajectory
- `.trr` — GROMACS full-precision trajectory
- `.dcd` — CHARMM/NAMD trajectory
- `.nc` / `.netcdf` — AMBER NetCDF trajectory

---

## What's New (v3.28.2026)

- **`dt_in_ps` parameter**: Override the timestep read from trajectory files. Useful when the time axis is missing or incorrect (e.g., merged DCD files).
- **Frame extraction cell**: Standalone section to extract and save a PDB snapshot at any user-specified time (ns). Automatically finds the closest available frame.
- **Google Drive folder picker**: Interactive widget to search and navigate to your working directory directly within Colab.
- **Global minimum reporting**: FEL analysis now prints a detailed summary of the lowest-energy frame (frame index, time in ns, RMSD, Rg).

---

## Usage Examples

### Run Complete Analysis
```python
analyzer = MDAnalyzer(
    topology="prot_lig_equil.pdb",
    trajectory="simulation_100ns.dcd",
    protein_selection="protein",
    ligand_selection="resname LIG",
    dt_in_ps=100  # e.g., 1000 frames × 100 ps = 100 ns total
)

analyzer.run_complete_analysis(output_prefix="my_analysis")
```

### Override Timestep
```python
# If your trajectory reports wrong times (common with merged DCD files),
# provide the true timestep in picoseconds
analyzer = MDAnalyzer(
    topology="protein.gro",
    trajectory="merged.dcd",
    dt_in_ps=100   # 100 ps per frame
)
```

### Extract a Frame at a Specific Time
```python
from extract_frame import extract_frame_from_trajectory  # or run the Colab cell

extract_frame_from_trajectory(
    topology_file="prot_lig_equil.pdb",
    trajectory_file="simulation.dcd",
    time_ns=56.70,
    output_pdb="frame_56ns.pdb",
    selection="protein or resname LIG"
)
```

### Run Individual Analyses
```python
analyzer.plot_rmsd(save_path="my_rmsd.png")
analyzer.plot_rmsf(save_path="my_rmsf.png")
analyzer.plot_rg(save_path="my_rg.png")
analyzer.plot_free_energy_landscape(save_path="my_fel.png")
analyzer.plot_binding_energy(save_path="my_binding_energy.png")
analyzer.plot_protein_ligand_distance(save_path="my_distance.png")
```

### Access Raw Data
```python
time_ns, rmsd_values   = analyzer.calculate_rmsd()
residues, rmsf_values  = analyzer.calculate_rmsf()
time_ns, rg_values     = analyzer.calculate_rg()
fel, xedges, yedges    = analyzer.calculate_free_energy_landscape()
time_ns, energies      = analyzer.calculate_binding_energy_mm()
time_ns, distances     = analyzer.calculate_protein_ligand_distance()
```

---

## Example Output

Each analysis generates:
- **High-quality PNG images** (300 DPI)
- **Statistical summaries** (mean ± std)
- **Frame identification** (global minimum frame number and time in ns)

```
✓ RMSD plot saved to my_analysis_rmsd.png
✓ RMSF plot saved to my_analysis_rmsf.png
✓ Rg plot saved to my_analysis_rg.png
✓ FEL plot saved to my_analysis_fel.png
✓ Binding energy plot saved to my_analysis_binding_energy.png
✓ Distance plot saved to my_analysis_pl_distance.png
```

---

## Common Selection Strings

**Protein:**
```
protein                     # All protein atoms
backbone                    # N, CA, C, O
name CA                     # C-alpha only
protein and resid 1-100     # Specific residue range
protein and chain A         # Chain-specific
```

**Ligand:**
```
resname LIG
resname ATP
resid 999
not protein and not resname SOL
```

---

## Dependencies

Automatically installed in Google Colab:
```
MDAnalysis
matplotlib
seaborn
numpy
scipy
ipywidgets
```

---

## Performance Notes

- Large trajectories (>10,000 frames) may take several minutes
- The MM binding energy uses simplified Coulomb + Lennard-Jones potentials and is approximate
- For rigorous binding free energies, use MM-PBSA or MM-GBSA tools externally

---

## Use Cases

- **Protein Stability Analysis**: Monitor RMSD and Rg to assess structural stability over time
- **Protein-Ligand Binding**: Evaluate binding modes and MM interaction energies
- **Conformational Sampling**: Identify low-energy states via FEL and extract key frames as PDBs
- **Flexible Region Mapping**: Identify high-fluctuation residues using RMSF
- **Educational Workshops**: Teaching MD simulation analysis concepts

---

## Educational Resources

This tool is part of the **SciLearningWorkshops LLC / ProteinReach** educational suite for computational biology training.

For workshop information, visit: [https://scilearningworkshops.carrd.co/](https://scilearningworkshops.carrd.co/)

---

## Citation

If you use this tool in your research or educational materials, please cite:

```bibtex
@software{md_quick_plot,
  author       = {Omar Arias-Gaguancela},
  title        = {MD\_quick\_plot: A Tool for Streamlined MD Trajectory Analysis},
  year         = {2025},
  publisher    = {GitHub},
  url          = {https://github.com/OmarArias-Gaguancela/MD_quick_plot}
}
```

**Or in text format:**
```
Arias-Gaguancela, O. (2025). MD_quick_plot: A Tool for Streamlined MD Trajectory Analysis.
GitHub. https://github.com/OmarArias-Gaguancela/MD_quick_plot
```

---

## Support & Contact

For questions, issues, or workshop inquiries:

- **LinkedIn**: [Omar Arias-Gaguancela](https://www.linkedin.com/in/omarariasgaguancela/)

---

## Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## Acknowledgments

- Built with [MDAnalysis](https://www.mdanalysis.org/)
- Visualization powered by [Matplotlib](https://matplotlib.org/) and [Seaborn](https://seaborn.pydata.org/)
- Hosted on [Google Colab](https://colab.research.google.com/)
