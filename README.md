# Neuronal Decoding from M1: Hybrid CNN-BiLSTM for Bilateral Forelimb Movement Prediction

This repository implements the models and methods from the paper "Dissecting the Computational Necessity of Hybrid Deep Learning Models for Decoding Complex Motor Behavior from Neuronal Population Activity".

## Overview

This codebase implements an attention-based hybrid CNN-BiLSTM architecture for decoding skilled bilateral forelimb movements from in vivo two-photon (2P) calcium imaging of excitatory neuronal ensembles in unilateral primary motor cortex (M1) of mice. The central finding is that unilateral M1 population activity contains sufficient spatiotemporal information to support reliable decoding of both ipsilateral and contralateral forelimb movements during a complex grid-walking task.


## Repository Structure
```bash
.
├── config/                # Configuration files (Hydra)
│   ├── dataset/          # Dataset configurations
│   ├── model/            # Model architectures
│   └── training/         # Training parameters
├── scripts/              # Execution scripts
│   ├── train.py          # Training script
│   └── evaluate.py       # Evaluation script
├── src/                  # Source code
│   ├── data/             # Dataset handling
│   ├── models/           # Model implementations
│   ├── training/         # Training utilities
│   └── utils/            # Metrics and visualization
└── requirements.txt      # Dependencies
```

## Installation
```bash
git clone https://github.com/LatifiLab/neuronal_decoding.git
cd neuronal_decoding
 
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
 
pip install -r requirements.txt
```

## Data
The code expects calcium imaging data with the following structure:

- CSV files containing calcium transients from neurons and behavioral labels
- Each row represents a time frame, with columns for:
  - Frame index
  - Neuronal activity for each neuron
  - Behavioral label (0: no footstep, 1: contralateral footstep, 2: ipsilateral footstep)

The processed calcium imaging data is provided in the `data/` directory.
 
## Usage
## Training
Each architecture can be trained independently with customized parameters:

```bash
# Train CNN model (spatial features only)
python scripts/train.py model=cnn

# Train LSTM model (temporal features only)
python scripts/train.py model=lstm

# Train LSTM with Attention
python scripts/train.py model=lstm_attention

# Train Hybrid CNN-BiLSTM model (full architecture)
python scripts/train.py model=hybrid
```

Additional configuration options:
```bash
# Use specific dataset
python scripts/train.py model=hybrid dataset=neural_data

# Modify training parameters
python scripts/train.py model=hybrid training.batch_size=16 training.learning_rate=0.0005

# Enable W&B logging
python scripts/train.py wandb.mode=online wandb.project=neuronal-decoding
```

## Evaluation
To evaluate a trained model:
```bash
python scripts/evaluate.py model=lstm
```
or:
```bash
python scripts/evaluate.py model=hybrid
```
