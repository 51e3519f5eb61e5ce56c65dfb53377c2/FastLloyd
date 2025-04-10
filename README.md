# FastLloyd: Federated, Accurate, Secure, and Tunable k-Means Clustering with Differential Privacy

FastLloyd is an approach to privacy-preserving k-means clustering in horizontally federated settings. It offers
state-of-the-art utility while providing formal privacy guarantees through differential privacy, and achieves orders of
magnitude faster performance compared to previous privacy-preserving clustering methods.

## Overview

This repository implements the FastLloyd protocol described in the paper "FastLloyd: Federated, Accurate, Secure, and
Tunable k-Means Clustering with Differential Privacy". FastLloyd addresses the challenging problem of collaborative
clustering across multiple data owners without compromising privacy, through:

1. A novel differentially private k-means algorithm with radius constraints
2. A lightweight secure aggregation protocol for federated settings

## Installation

### Requirements

- Python 3.8 or higher
- Open MPI (for multiparty communication)
- Required Python packages listed in `env.yml`

### Setup

1. Clone the repository:

```bash
git clone https://github.com/51e3519f5eb61e5ce56c65dfb53377c2/FastLloyd.git
cd FastLloyd
```

2. Create and activate the conda environment:

```bash
conda env create -f env.yml
conda activate fastlloyd
```

## Repository Structure

```
├── configs/                                                              
│   ├── defaults.py       # Default configuration settings, dataset definitions                                                                                    
│   └── params.py         # Parameter class for clustering and privacy settings                                                                                  
│                                                                                                                                                    
├── data_io/                                                                                                                                          
│   ├── comm.py           # MPI communication wrapper with delay simulation                                                                          
│   ├── data_handler.py   # Functions for loading and processing datasets                                                                            
│   └── fixed.py          # Fixed-point arithmetic implementation                                                                                    
│                                                                                                                                                    
├── parties/                                                                                                                                          
│   ├── client.py         # Client implementations (masked and unmasked)                                                                             
│   └── server.py         # Server implementation with DP mechanisms                                                                                 
│                                                                                                                                                    
├── plots/                                                                                                                                           
│   ├── ablation_plots.py # Visualization for ablation studies                                                                                       
│   ├── per_dataset.py    # Dataset-specific result visualization                                                                                 
│   ├── scale_heatmap.py  # Heatmap generation for scalability results                                                                               
│   ├── synthetic_bar.py  # Bar charts for synthetic dataset results                                                                                 
│   └── timing_analysis.py # Analysis of timing experiments                                                                                          
│                                                                                                                                                    
├── scripts/                                                                                                                                         
│   ├── generator.R        # R script for generating synthetic datasets         
│   ├── experiment_runner.sh # Script for running accuracy and scale experiments
│   └── timing_runner.sh   # Script for running timing experiments                                                                                                                  
│                                                                                                                                                    
├── utils/                                                                                                                                           
│   ├── evaluations.py    # Clustering quality evaluation metrics                                                                                    
│   └── utils.py          # General utility functions                                                                                                
│                                                                                                                                                    
├── experiments.py        # Main experiment runner                                                                                                   
├── env.yml               # Conda environment specification                                                                                          
└── README.md             # Project documentation
```

## Usage

### Running Experiments

FastLloyd supports multiple experiment types:

1. **Accuracy**: Evaluate clustering quality across different privacy settings

```bash
python experiments.py --exp_type "accuracy"
```

2. **Scale**: Analyze scalability with dataset size, dimensions, and number of clusters

```bash
python experiments.py --exp_type "scale"
```

3. **Timing**: Measure communication and computation time

```bash
mpirun -np 3 python experiments.py --exp_type "timing"
```

You can also use the provided scripts to run multiple experiment types:

```bash
bash scripts/experiment_runner.sh  # For accuracy and scale experiments
bash scripts/timing_runner.sh      # For timing experiments with varying numbers of clients
```

### Visualization

The repository includes several visualization tools in the `plots` directory:

- `per_dataset.py`: Creates performance visualizations for individual datasets
- `scale_heatmap.py`: Generates heatmaps to analyze scalability
- `synthetic_bar.py`: Creates bar plots comparing performance on synthetic datasets
- `ablation_plots.py`: Creates plots for ablation studies
- `timing_analysis.py`: Analyzes and reports execution timing data

## Customization

You can customize various aspects of the experiments through the argument parser in `experiments.py`:

```bash
python experiments.py --exp_type "test" --datasets "mnist" "adult" --method "diagonal_then_frac" --alpha 0.8 --post "fold" --results_folder "my_results"
```

Key parameters include:

- `--exp_type`: Type of experiment to run (accuracy, scale, timing, test)
- `--datasets`: Datasets to use for the experiment
- `--method`: Maximum distance method to use
- `--alpha`: Maximum distance parameter
- `--post`: Post-processing method for centroids
- `--results_folder`: Folder to store results
