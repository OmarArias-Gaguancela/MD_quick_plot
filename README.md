# MD_quick_plot

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1RtROKVsXEXbweYUDkGCRmySlES3AgfvU?usp=drive_link)

## Overview

This tool is designed to **streamline the analysis of Molecular Dynamics (MD) simulation data** by simplifying the workflow to just two essential inputs:

1. **Structural file** (PDB, GRO)
2. **Trajectory file** (XTC, TRR, DCD)

No complex scripting required. Just load your files and run comprehensive analyses with publication-ready visualizations!!

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
    ligand_selection="resname LIG"      # Optional: customize ligand selection
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
| **FEL** | Free Energy Landscape | Global minimum identification |
| **Binding Energy** | Protein-ligand interaction energy | MM-based energy calculation |
| **P-L Distance** | Minimum protein-ligand distance | Contact monitoring |

---

## Key Advantages

**Simple Input**: Only requires topology + trajectory files  
**Automated Workflow**: Run all analyses with one command  
**Publication-Ready Figures**: High-resolution (300 DPI) plots  
**No Installation Hassle**: Runs in Google Colab  
**Flexible Selections**: Customizable atom/residue selections  
**Comprehensive Output**: Statistical summaries included  

---

## Input Requirements

### 1. Topology File (Structural File)

Supported formats:
- `.pdb` - Protein Data Bank format
- `.gro` - GROMACS structure file

### 2. Trajectory File

Supported formats:
- `.xtc` - GROMACS compressed trajectory
- `.trr` - GROMACS full-precision trajectory
- `.dcd` - CHARMM/NAMD trajectory

---

## Example Output

Each analysis generates:
- **High-quality PNG images** (300 DPI)
- **Statistical summaries** (mean, std, thresholds)
- **Frame identification** (e.g., global minimum in FEL)

### Sample Outputs
```
✓ RMSD plot saved to my_analysis_rmsd.png
✓ RMSF plot saved to my_analysis_rmsf.png
✓ Rg plot saved to my_analysis_rg.png
✓ FEL plot saved to my_analysis_fel.png
✓ Binding energy plot saved to my_analysis_binding_energy.png
✓ Distance plot saved to my_analysis_pl_distance.png
```

---

## Customization Options

### Custom Selections
```python
# Custom protein selection
analyzer = MDAnalyzer(
    topology="protein.pdb",
    trajectory="traj.xtc",
    protein_selection="protein and chain A",  # Chain-specific
    ligand_selection="resname ATP"             # Custom ligand name
)
```

### Individual Analyses
```python
# Run specific analyses
analyzer.plot_rmsd(save_path="custom_rmsd.png")
analyzer.plot_rmsf(save_path="custom_rmsf.png")
analyzer.plot_rg(save_path="custom_rg.png")
analyzer.plot_free_energy_landscape(save_path="custom_fel.png")
analyzer.plot_binding_energy(save_path="custom_be.png")
analyzer.plot_protein_ligand_distance(save_path="custom_dist.png")
```

### Advanced Options
```python
# Calculate raw data without plotting
time_ns, rmsd_values = analyzer.calculate_rmsd()
residues, rmsf_values = analyzer.calculate_rmsf()
time_ns, rg_values = analyzer.calculate_rg()
fel, xedges, yedges = analyzer.calculate_free_energy_landscape()
```

---

## Dependencies

The following packages are automatically installed in Google Colab:
```python
MDAnalysis
matplotlib
seaborn
numpy
scipy
```

---

## Use Cases

- **Protein Stability Analysis**: Monitor RMSD and Rg to assess structural stability
- **Protein-Ligand Binding**: Evaluate binding modes and interaction energies
- **Conformational Sampling**: Identify low-energy states via FEL analysis
- **Flexible Region Identification**: Map high-fluctuation residues using RMSF
- **Educational Workshops**: Teaching MD simulation analysis concepts

---

## Educational Resources

This tool is part of the **SciLearningWorkshops LLC** educational suite for computational biology training.

For workshop information, visit our website: https://scilearningworkshops.carrd.co/ 

---

## Citation

If you use this tool in your research or educational materials, please cite:
```
SciLearningWorkshops LLC (2025)
MD Trajectory Analysis Tool
Developer: Omar Arias-Gaguancela, PhD
GitHub: https://github.com/OmarArias-Gaguancela 
```

---

## Support & Contact

For questions, issues, or workshop inquiries:

- **Email**: Contact SciLearningWorkshops LLC
- **LinkedIn**: [Omar Arias-Gaguancela](https://www.linkedin.com/in/omararias/)
- **Issues**: Please open an issue on GitHub
- **Feature Requests**: Contributions welcome!

---

## Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## License

© 2025 SciLearningWorkshops LLC. All rights reserved.

This software is provided for educational and research purposes.

---

## Acknowledgments

- Built with [MDAnalysis](https://www.mdanalysis.org/)
- Visualization powered by [Matplotlib](https://matplotlib.org/) and [Seaborn](https://seaborn.pydata.org/)
- Hosted on [Google Colab](https://colab.research.google.com/)

---

## Get Started Now!

**Ready to analyze your MD simulations?**

**[Click here to open the notebook in Google Colab](https://colab.research.google.com/drive/1RtROKVsXEXbweYUDkGCRmySlES3AgfvU?usp=drive_link)** 

---

### Quick Example
```python
# Just two files needed!
analyzer = MDAnalyzer(
    topology="protein.pdb",
    trajectory="simulation.xtc"
)

# One command for complete analysis
analyzer.run_complete_analysis()
```


