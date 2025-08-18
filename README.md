# ARMS: Automated Reaction Mapping System

This project, Automated Reaction Mapping System (ARMS) implements a data-driven model to predict enantioselectivity (mirror-image selectivity) in a given catalytic reaction. It uses experimental screening data to learn how different catalyst ligand substituents (R1, R2, R3, Ar) and conditions influence outcomes like enantioselective excess (ee), yield, and ΔΔG. The goal is to find the optimal combination of catalyst substituents for a specific reaction. The code is written as a Jupyter Notebook and processes input Excel data to train machine-learning models (SVR, Random Forest, XGBoost) on molecular features. Once trained, the model can predict outcomes on new combinations of substituents (simulation) and identify promising catalyst designs.

## License
- **ARMS** code is licensed under a proprietary license. All rights reserved. You may not use, modify, or distribute the ARMS code without the author's prior written permission.
- Parts of the project may contain open source components. These components are licensed under the applicable open source license. See the relevant license file for details.

## Requirements
- Python 3.x
- Pandas
- scikit-learn
- rdkit

## Data Format
Prepare an Excel file (in the same folder as the notebook) with at least the following sheets and columns:
 - Ligands: Contains ligand substituent abbreviations and their SMILES.
 - Columns: Abbr (ligand shorthand), Smiles (SMILES string of ligand).
 - Reactions: Defines each reaction’s base structures.
 - Columns: Name (unique reaction name), Reactant (SMILES of core reactant with placeholders [R1]/[R2]), Product (SMILES of core product with [R1]/[R2]), Catalyst (SMILES of core catalyst with [R3]/[Ar]).
 - Exp: Experimental results for each screening point.
 - Columns include: Name (matching a Reaction name), R1, R2, R3, Ar (ligand choices by abbreviation), solvent, T(K) (temperature in Kelvin), and target columns such as yield, ee (enantioselective excess), regio, etc. Missing target values will be dropped automatically.
 - Screen_simulate (optional): If you want to generate predictions on new combinations, include this sheet. Each column (e.g. R1, R2, R3, Ar, solvent, T(K)) lists possible values. The code will create all combinations of these to simulate.
Example: In the provided workbook Data_2503-paper_250425.xlsx, “Ligands” maps abbreviations like “Ph” to SMILES, “Reactions” gives base SMILES for reactants/products/catalysts, and “Exp” contains measured ee, yield, etc. The code reads these sheets and constructs “full” molecule SMILES by substituting ligands into placeholders.
## How to use (Jupyter Notebook)
Open Notebook: Launch JupyterLab or Jupyter Notebook and open the provided .ipynb file. The code assumes %matplotlib inline is enabled for plotting.

Set Data File: In the code cell, update the FileName variable to the name of your Excel data file (e.g. FileName = "./Data_yourfile.xlsx"). Ensure the file is in the same directory or provide the correct path.

Choose Target: The script uses a variable target to decide which column to predict ('ee', 'yield', 'ddG', or 'regio'). By default it is set to 'ee'. You can change it before running to predict other outcomes.

## Output
- Mapped Responses (in xlsx file)
- Predicted results (in xlsx file)
