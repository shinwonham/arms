# ARMS: Automated Reaction Mapping System

This project, Automated Reaction Mapping System (ARMS) implements a data-driven model to predict enantioselectivity (mirror-image selectivity) in a given catalytic reaction. It uses experimental screening data to learn how different catalyst ligand substituents (R1, R2, R3, Ar) and conditions influence outcomes like enantioselective excess (ee), yield, and ΔΔG. The goal is to find the optimal combination of catalyst substituents for a specific reaction. The code is written as a Jupyter Notebook and processes input Excel data to train machine-learning models (SVR, Random Forest, XGBoost) on molecular features. Once trained, the model can predict outcomes on new combinations of substituents (simulation) and identify promising catalyst designs.

## License
- **ARMS** code is licensed under a proprietary license. All rights reserved. You may not use, modify, or distribute the ARMS code without the author's prior written permission.
- Parts of the project may contain open source components. These components are licensed under the applicable open source license. See the relevant license file for details.

## Requirements & Installation
Python: Version 3.8 (the code was developed under Python 3.8).

Libraries: Install the following Python packages via pip. The versions used/tested include (latest stable as of 2024–2025 in parentheses):
 - Pillow (PIL fork, e.g. 9.4.0 for Python 3.8)
 - CGRtools (v4.1.35) – handles molecules and reactions (used to generate Condensed Graphs of Reaction).
 - pandas (e.g. 1.6.0) – for data manipulation.
 - scikit-learn (e.g. 1.2.2) – for machine learning pipelines (SVR, RandomForest, cross-validation, etc.).
 - openpyxl (e.g. 3.x) – for writing results back to Excel.
 - py-mini-racer (MiniRacer, v0.12.4) – a minimal V8 engine for Python (used by CGRtools for 2D clean drawing).
 - xgboost (e.g. 1.7.6) – for XGBRegressor.
 - catboost (e.g. 2.1.9) – for CatBoostRegressor.
 - matplotlib and numpy, scipy – for visualization and numerical operations.
 - ChemInfoTools: This code uses classes from the ChemInfoTools library (e.g. ComplexFragmentor, Augmentor). Install via pip install ChemInfoTools (or from the GitHub repository if not on PyPI).

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
 - Open Notebook: Launch JupyterLab or Jupyter Notebook and open the provided .ipynb file. The code assumes %matplotlib inline is enabled for plotting.

 - Set Data File: In the code cell, update the FileName variable to the name of your Excel data file (e.g. FileName = "./Data_yourfile.xlsx"). Ensure the file is in the same directory or provide the correct path.

 - Choose Target: The script uses a variable target to decide which column to predict ('ee', 'yield', 'ddG', or 'regio'). By default it is set to 'ee'. You can change it before running to predict other outcomes.

 - Run Cells in Order:
   1) Load & Clean
   Reads Ligands, Reactions, Exp (and optionally Exp_Simulate).
   Drops rows with missing target (ee by default).
   If target == "ddG", converts ee→ΔΔG using the provided thermodynamic functions.
   
   2) Feature Construction (CGR + CircuS)
   Expands ligand abbreviations (Ligands) into full structures.
   Builds Condensed Graph of Reaction (CGR) from reaction templates + substituents.
   Generates structure features: mol_full (full catalyst), mol_R3, mol_Ar, and CGR.
   
   3) Outlier Handling (optional)
   Visualizes target distribution and flags outliers (IQR / z-score)
   To disable removal, skip the filtering cell or set the threshold accordingly.
   
   4) Modeling
   Select a pipeline (default examples provided): RF_pipeline (Random Forest), SVR_pipeline (Support Vector Regression)
   Train/validation split and 10-fold CV cells are provided.
   Metrics reported: RMSE, MAE, R².
   Plots saved as VSplot_{target}_*.png and VSplot_{target}_log*.png (Pred vs Actual).
   
   5) Train Final Model
   Fit on the filtered full training set.

 - Predict on new data (screening) Use one of the two flows:
   A. Provide your own Exp_Simulate sheet
   B. Generate a large combinatorial screen

## Key Features and Workflow

 - Modular Feature Engineering
   
The notebook automatically handles chemical structure manipulations: it merges ligand fragments into catalyst/reactant SMILES and computes CGR descriptors. All structural features are numerically encoded for ML via the ComplexFragmentor class from ChemInfoTools, which generates CircuS fragment descriptors with tunable radius (the code uses radii 0–4).

 - Simulation of New Ligand Combinations
   
Given a list of possible substituents in the “Screen_simulate” sheet, the code generates all combinations and predicts the target. This helps identify promising catalysts not in the original experiments. Results are appended to the Excel file.

## Notes

 - Environment: This code was tested under Python 3.8. Higher Python versions (3.9–3.10+) may require updated library versions.

 - Data Consistency: Make sure that ligand abbreviations in the Exp data match those in the Ligands sheet, and that reaction names match the Reactions sheet.

 - Extending the Approach: While the notebook focuses on enantioselectivity (ee), the same workflow can be applied to any related output by changing the target. Additional models or descriptors could be integrated by expanding the pipelines.


This README should help you understand and run the provided Jupyter code to train and apply enantioselectivity prediction models on catalytic reaction data. 
Adjust paths, sheet names, and parameters as needed for your own datasets. 
