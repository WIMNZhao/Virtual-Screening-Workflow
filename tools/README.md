# 🧰 Tools

This directory contains standalone software tools that support the **Virtual Screening Workflow**.

The following tools are included in the workflow release packages:

- **QueryDB** — compound library filtering and substructure searching
- **LeConf** — ligand conformer generation and preparation
- **LePose** — post-docking pose analysis and filtering

Each tool can be used independently for its dedicated task.

Please visit the [Releases](https://github.com/WIMNZhao/Virtual-Screening-Workflow/releases) page to download the latest workflow package containing these tools.

---

## 🔍 QueryDB

A compound library filtering program for selecting molecules based on physicochemical properties, SMARTS-defined functional groups (e.g., PAINS motifs), and substructure queries.

### Typical applications

* 🧪 Physicochemical property filtering
* 🚫 SMARTS-based filtering (e.g., PAINS removal)
* 🔎 Single or multiple substructure searches

---

## 🧬 LeConf

A ligand preparation program that generates one 3D conformer for each top-ranked ring tautomer in its dominant protonation state.

### Typical applications

* 📐 3D conformer generation
* 🔄 Ring tautomer enumeration and ranking
* ⚖️ Dominant protonation state assignment
* 🚀 Ligand preparation for virtual screening

---

## 🧩 LePose

A post-docking analysis program that filters docking poses based on user-defined protein–ligand interactions, ligand torsional geometries, and scoring function components.

### Typical applications

* 🤝 Interaction-based pose filtering
* 🔄 Torsion-based filtering
* 📊 Score component-based filtering
* 🎯 Selection of poses satisfying multiple structural criteria

---

## ⚙️ Requirements

The utility programs are designed for Linux environments.

* 🐧 **Ubuntu 18.04 LTS or later**
* ⌨️ Command-line environment

---

## 📝 Notes

* ✅ Each utility is self-contained and can be used independently.
* ⌨️ Run utilities from the command line unless stated otherwise.

---

## Third-Party Software

These tools include third-party open-source components, including RDKit, NumPy, Python runtime components, and other supporting libraries.

The corresponding license texts are provided in `THIRD_PARTY_LICENSES.txt`.
