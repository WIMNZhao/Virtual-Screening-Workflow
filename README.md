# Virtual Screening Workflow

Drug discovery increasingly benefits from access to vast chemical spaces,
driven by generative AI and, more importantly, the systematic enumeration
of synthetically feasible molecules. Meanwhile, advances in AI-based protein
structure prediction and co-folding have enabled docking-based virtual
screening against virtually any biological target.

This tutorial provides a practical, step-by-step workflow for performing
high-throughput virtual screening, including:

- 🧹 Library filtering
- 🌀 3D conformer generation with pH-dependent protonation state assignment
- 🧬 Protein preparation
- ⚡ High-throughput molecular docking
- 📊 Post-docking analysis and hit prioritization

---

## 📚 Example Library

The toy library contains **100 compounds**, including two compounds previously
identified through virtual screening against the kinase domain of **EPHB4**.
One of these compounds subsequently contributed to the discovery of a clinical
candidate for cancer treatment
([The science and art of structure-based virtual screening](https://doi.org/10.1021/acsmedchemlett.4c00093)).

This example demonstrates how a compound library can be processed, docked,
and analyzed using the **LeDock_NOVA** virtual screening pipeline.

---

## 1. 🧹 Library Filtering

Although higher-quality hits can often be identified by screening larger
libraries, docking the entire chemical library is not always necessary or
computationally efficient.

It is therefore recommended to prefilter compound libraries based on desired
physicochemical properties and/or pharmacophoric features before docking.

### Command

```bash
cd library

../tools/queryDB -filter ../param/library_filter.param smi lib.smi lib_f.smi
```

The `library_filter.param` file contains general-purpose filtering criteria,
including:

- Removal of problematic substructures (e.g., PAINS)
- Desired (privileged) substructures
- Lead-like physicochemical property filters

In a real drug discovery project, these parameters should be carefully
optimized according to the specific target, therapeutic area, and project
objectives.

---

## 2. 🌀 3D Conformer Generation

Generate 3D conformers with pH-dependent protonation state assignment:

### Command

```bash
../tools/leconf -i lib_f.smi -o lib_f.sdf --n 1 --t
```

For each molecule, `leconf`:

- Samples multiple conformations and selects the lowest-energy conformer
  (`--n 1`)
- Assigns protonation states at physiological pH (7.4)
- Selects the lowest-energy ring tautomer (`--t`)
- Generates energetically favorable chair conformations for six-membered
  saturated rings
- Preferentially selects equatorial ring substitutions while considering
  axial orientations caused by allylic strain
  ([The role of allylic strain for conformational control in medicinal chemistry](https://doi.org/10.1021/acs.jmedchem.3c00446))

The generated conformers are stored in `lib_f.sdf` for docking.

---

## 3. 🧬 Protein Preparation

Prepare the protein structure for docking:

### Command

```bash
cd ../protein
../tools/lepro_nova 2VWX.pdb
```

`lepro_nova` performs lightweight protein preprocessing, primarily by adding
hydrogen atoms. It also generates the docking configuration file:

```text
dock.in
```

By default, **20 docking poses** are generated per ligand.

In some benchmarking studies (e.g., DUD-based evaluations), only **one pose**
is generated to reduce computational cost and enable large-scale performance
comparisons. However, this is generally not recommended for real-world virtual
screening, where at least **5–10 poses per ligand** are typically required to
achieve reasonable sampling convergence.

---

## 4. ⚡ Docking of the Compound Library

Prepare the ligand list:

### Command

```bash
ls ../library/lib_f.sdf > ligands
```

Run parallel docking:

### Command

```bash
OMP_NUM_THREADS=4 ../tools/ledock_nova dock.in
```

The number of OpenMP threads can be adjusted according to available CPU
resources.

---

## 5. 📊 Post-docking Analysis

Perform pose filtering and hit prioritization:

### Command

```bash
../tools/lepose lib_f_dock.sdf ../param/post_docking.param
```

`lepose` filters docking poses according to:

- Scoring components
- Key protein–ligand interactions
- Allowed low-energy torsion angles

For kinase targets, example selection criteria may include:

- ✅ Hydrogen bonding interactions with the hinge region
- ✅ Occupancy of the ATP back pocket by aromatic groups

The final output is:

```text
lib_f_dock_pass.sdf
```

The selected hits can be directly visualized using the **ICM Browser**
([ICM Browser](https://www.molsoft.com/icm_browser.html))
or other molecular visualization software.

---

# 🚀 Summary

This workflow provides an end-to-end pipeline for high-throughput virtual
screening:

```
📚 Compound Library
        │
        ▼
🧹 Library Filtering
        │
        ▼
🌀 3D Conformer Generation
        │
        ▼
🧬 Protein Preparation
        │
        ▼
⚡ Molecular Docking
        │
        ▼
📊 Post-docking Analysis
        │
        ▼
🎯 Prioritized Hits
```

The workflow is designed for rapid screening of large compound libraries
while maintaining chemically meaningful filtering and biologically relevant
pose selection.
