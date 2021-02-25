# tautomer-data
A repo that includes all the data for the 'Fitting quantum machine learning potentials to experimental free energy data: Predicting tautomer ratios in solution' manuscript

# Data

We provide data for the following preprint https://www.biorxiv.org/content/10.1101/2020.10.24.353318v4.
The results for all calculations are shown here:
https://github.com/choderalab/tautomer-data/blob/main/data/results.csv (all values in kcal/mol).
## Dataset

The original data used for this study was taken from https://github.com/WahlOya/Tautobase. 
A subset of this was used to calculate relative free energies using a DFT approach, the molecules 
are shown here:
https://github.com/choderalab/tautomer-data/blob/main/data/input/b3lyp_tautobase_subset.txt
The subset of the subset above used for the calculations with the neural net potential is shown here:
https://github.com/choderalab/tautomer-data/blob/main/data/input/ani_tautobase_subset.txt

The text file includes the molecule name used in the manuscript, the SMILES for both tautomeric forms and the experimental ddG (in kcal/mol).


## Quantum chemistry data

The full QM data is deposited in https://github.com/choderalab/tautomer-data/blob/main/data/calculated/QM.pickle .

The pickle file contains a single dictionary (of dictionaries), with the molecule names as keys.
```
r = pickle.load(open('QM.pickle', 'rb'))
r['SAMPLmol2']
```

returns:

```
{'OC1=CC=C2C=CC=CC2=N1': {'solv': [mol1, mol2, ...],
  'vac': [mol1, mol2, ...]},
 'O=C1NC2=C(C=CC=C2)C=C1': {'solv': [mol1, mol2, ...],
  'vac': [mol1, mol2, ...]}}
```
.

For a single system (e.g. `'SAMPLmol2'`) the two tautomer molecules are identified with the SMILES string (e.g. `'OC1=CC=C2C=CC=CC2=N1'`), and the envrionment (e.g. `'solv'`).
Using these three keys (e.g. `r['SAMPLmol2]['OC1=CC=C2C=CC=CC2=N1']['solv])` one gets a list or rdkit molecules, each in the optimized 3D conformation.
The molecule contains properties that can be acces via `.GetProp()`.
The relevant properties are:

`'G'` ... the gibbs free energy in the specified environment calculated with RRHO and B3LYP/aug-cc-pVTZ

`'E_B3LYP_pVTZ'` ... electronic energy evaluated on this conformation using B3LYP/aug-cc-pVTZ

`'E_B3LYP_631G_gas'` ... electronic energy evaluated on this conformation using B3LYP/6-31G(d)

`'E_B3LYP_631G_solv'` ... electronic energy evaluated on this conformation using B3LYP/6-31G(d)/SMD
`'H'` ... the enthalpy in the specified environment calculated with RRHO and B3LYP/aug-cc-pVTZ 
`'graph_automorphism'` ... the number of graph automorphism. This is used for the calculation of `RT ln(D)` where `D` is this number.


## ANI data

### RRHO ANI dataset

The raw data for this data set is saved in https://github.com/choderalab/tautomer-data/blob/main/data/calculated/ANI1ccx_RRHO.pickle.
This pickle file contains a dictionary that can be queried using the molecule names as key.
For each molecule there are additional keys: `'t1-energies'`, `'t2-energies'`, `'t1-confs'` and `'t2-confs'`.
t1 and t2 correspond to the naming of the tautomers in the original dataset.

``t1-energies`` contains a list with the gibbs free energies after energy minimization,  `'t1-confs'` contains a rdkit molecule with the conformations after minimization.


### Alchemical free energy dataset

Results for 5 independent runs are stored here: https://github.com/choderalab/tautomer-data/tree/main/data/calculated/.
Each of the 5 ANI1ccx_vacuum_rfe_results_in_kT_300snapshots_p*.csv files contain per line three values: the name of the tautomer system, ddG [kcal/mol] and dddG [kcal/mol].

### Optimization

The retraining results are located here (including the log file and best parameter set):
https://github.com/choderalab/tautomer-data/tree/main/data/optimization .
The results for the tautomer dataset per epoch is stored in 
https://github.com/choderalab/tautomer-data/blob/main/data/optimization/combined_results.pickle
 and the training/validation/test split in
 https://github.com/choderalab/tautomer-data/blob/main/data/optimization/training_validation_tests.pickle.
 

