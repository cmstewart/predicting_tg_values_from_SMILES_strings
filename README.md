# Predicting Polymer Glass Transition Temperature from SMILES Strings

This project builds a simple ML pipeline to predict glass transition temperatures (Tg) of polymers using only their SMILES string representations. It combines [Mol2Vec](https://pubs.acs.org/doi/10.1021/acs.jcim.7b00616) molecular embeddings with an [XGBoost](https://xgboost.readthedocs.io/) regressor to go from chemical structure to predicted Tg in degrees Celsius.

## Approach

1. Parse SMILES strings into molecular objects using [RDKit](https://www.rdkit.org/)
2. Project each molecule into a 300-dimensional embedding space using a pretrained Mol2Vec model
3. Train an XGBoost regressor with early stopping on the resulting tabular features
4. Evaluate on a held-out test set and an independent external dataset

## Results

This exercise is adapted from one in which I used proprietary data to train the model. I agreed not to divulge that data, so I have switched it out here for a publicly available dataset. The model achieves an R² of ~0.78 on a held-out test set (20% of the training data). When evaluated against a completely independent dataset of ~1430 polymers from a different source, R² drops a good bit. This is due to a distribution shift between the two datasets. Given that it does remain positive, the result suggests the model captures some aspects of a generalizable structure–property relationships.

## Data

Training data (662 polymers) is from [this Figshare dataset](https://springernature.figshare.com/articles/dataset/dataset_with_glass_transition_temperature/24219958). External validation data (~1430 polymers) is from [this 2024 study](https://pmc.ncbi.nlm.nih.gov/articles/PMC11398084/) ([GitHub](https://github.com/PolymerTg/Polymer-Tg-Machine-Learning)).

## Usage

Install dependencies:

```bash
pip install -r requirements.txt
```

Train from the command line:

```bash
python model.py --train data/external_tg.csv
```

Predict on new data (CSV must contain a `SMILES` column):

```bash
python model.py --predict path/to/new_data.csv
```

Or walk through the full analysis interactively in `Part 1.ipynb`.

## Files

| File | Description |
|------|-------------|
| `model.py` | Training and prediction code |
| `Part 1.ipynb` | Jupyter notebook with full walkthrough |
| `requirements.txt` | Python dependencies |
| `data/external_tg.csv` | Training dataset (662 polymers) |
| `data/polymer_tg_external.xlsx` | External validation dataset (~1430 polymers) |
