# ProteinMPNN run (the example with NME7–TCHP_ELLEn motif)

## Table of Contents

	- [Overview](#overview)
	- [1. Prepare structural template (AF3 model)](#1-prepare-structural-template-af3-model)
	- [2. Isolate peptide and receptor in PyMOL](#2-isolate-peptide-and-receptor-in-pymol)
	- [3. Rename objects](#3-rename-objects)
	- [4. Define chains](#4-define-chains)
	- [5. Remove ligands and ions](#5-remove-ligands-and-ions)
	- [6. Export cleaned complex](#6-export-cleaned-complex)
	- [7. Prepare working directory](#7-prepare-working-directory)
	- [8. Run ProteinMPNN redesign](#8-run-proteinmpnn-redesign-with-nme7_chaina_ellen_chainb.pdb-input-structure)
	- [9. Inspect ProteinMPNN output](#9-inspect-proteinmpnn-output)
	- [10. Result](#10-result)


## Overview

	- The following example FlexPepDock run uses NME7 as receptor with TCHP-derived ELLEn peptide motif (DOI:  10.1038/s41467-024-46737-3:) as template binder.
	- Template: AlphaFold3 (AF3) model
	- Software: PyMOL + ProteinMPNN
	- Goal: Refine peptide–protein interaction and redesign peptide sequence

---

## 1. Prepare structural template (AF3 model)

An AlphaFold2 model of human NME7 and TCHP was used as the starting structure.

---

## 2. Isolate peptide and receptor in PyMOL

The ELLEn motif (binder peptide template) and NME7 receptor were separated into individual objects (obj01 and obj02).

---

## 3. Rename objects in PyMOL

PyMOL> set_name obj01, ellen
PyMOL> set_name obj02, nme7

---

## 4. Define chains

PyMOL> alter ellen, chain='B'
PyMOL> alter nme7, chain='A'

---

## 5. Remove ligands and ions

Manually removed ions from NME7:ELLEn model complex

---

## 6. Export cleaned complex

save NME7_chainA_ELLEn_chainB.pdb

---

## 7. Prepare working directory

mkdir -p ~/my_project_folder/NME7_ELLEn
cd ~/my_project_folder/NME7_ELLEn

cp "/mnt/c/Users/kensch/OneDrive - Kraeftens Bekaempelse/Skrivebord/Projects/PhD project/Minibinders/NME7_ELLEn/NME7_chainA_ELLEn_chainB.pdb" .

mkdir -p proteinmpnn_out

---

## 8. Run ProteinMPNN redesign with NME7_chainA_ELLEn_chainB.pdb input structure, chain B peptide binder template, perform sequence redesign on fixed backbone using chain B as target while keeping chain A fixed as structural context, and the generation of 10 redesigned model only (as an example). Results deposited in proteinmpnn_out folder

mamba activate torch_cuda_env
cd /home/kensch/ProteinMPNN

python protein_mpnn_run.py \
    --pdb_path /home/kensch/my_project_folder/NME7_ELLEn/NME7_chainA_ELLEn_chainB.pdb \
    --pdb_path_chains B \
    --out_folder /home/kensch/my_project_folder/NME7_ELLEn/proteinmpnn_out \
    --num_seq_per_target 10 \
    --sampling_temp "0.1" \
    --batch_size 1

### Run summary
	- Number of edges: 48 
	- Training noise level: 0.2A
	- Generating sequences for: NME7_chainA_ELLEn_chainB 
	- 10 sequences of length 401 generated in 41.3211 second

---

## 9. Inspect ProteinMPNN output

cat /home/kensch/my_project_folder/NME7_ELLEn/proteinmpnn_out/seqs/*.fa
Example output:

>NME7_chainA_ELLEn_chainB, fixed_chains=['A'], designed_chains=['B']
SLEARREKLRQLMQEEQDLLARELE

>T=0.1, sample=1
SLEERKEKLAKLLAEEEKRNKELLA

---

## 10. Result

	- ProteinMPNN generated redesigned peptide–protein sequence 
	- Output structures were written to NME7_ELLEn/proteinmpnn_out /
	- Chain A (NME7) was used as receptor
	- Chain B (ELLEn) was treated as peptide
	- ProteinMPNN successfully redesigned chain B
	- A new peptide sequence was generated:

SLEERKEKLAKLLAEEEKRNKELLA

	- The workflow completed without errors and is reproducible
<img width="910" height="2918" alt="image" src="https://github.com/user-attachments/assets/eac18d11-2485-48e7-90e2-30dd8a008239" />
