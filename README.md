# ARMS: Automated Reaction Mapping System

This project, Automated Reaction Mapping System (ARMS) is designed to automatically perform Atom-to-Atom Mapping (AAM) based on the change in reaction scaffold during a chemical reaction. It maps reactions using chemical SMILES and predicts reaction outcomes using simple machine learning models, providing chemists with an efficient tool to automate and predict reaction behavior.

## License
- **ARMS** code is licensed under a proprietary license. All rights reserved. You may not use, modify, or distribute the ARMS code without the author's prior written permission.
- Parts of the project may contain open source components. These components are licensed under the applicable open source license. See the relevant license file for details.

## Requirements
- Python 3.x
- Pandas
- scikit-learn
- rdkit

## How to use
1. Prepare a data file containing reaction SMILES and target values.
2. Run the notebook to:
- Response Mapping using ARMS
- Generate features
- Train a random forest model
- Save the trained model
- Predict the counter-reaction outcome

## Output
- Mapped Responses (in xlsx file)
- Predicted results (in xlsx file)
