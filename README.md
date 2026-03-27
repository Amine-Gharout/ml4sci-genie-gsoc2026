# ML4SCI GENIE - GSoC 2026 Tasks

This repository contains the three notebooks used to complete the ML4SCI GENIE evaluation tasks for quark/gluon jet classification and reconstruction.

## Project Structure

### [common_task1_autoencoder_quark_gluon.ipynb](common_task1_autoencoder_quark_gluon.ipynb)
Common Task 1: Autoencoder for quark/gluon jet images.

This notebook:
- inspects the HDF5 dataset schema and basic event statistics,
- performs EDA on the three detector channels (ECAL, HCAL, Tracks),
- builds a convolutional autoencoder for 125x125x3 jet images,
- trains the model with a sparse weighted reconstruction loss,
- shows side-by-side original vs reconstructed events,
- saves and reloads a model checkpoint.

### [common_task2_jets_as_graphs_gnn.ipynb](common_task2_jets_as_graphs_gnn.ipynb)
Common Task 2: Jets as graphs.

This notebook:
- converts each non-zero jet image into a point cloud,
- constructs graph nodes from pixel coordinates and channel energies,
- builds k-NN edges from spatial proximity,
- preprocesses the dataset in sequential HDF5 chunks,
- trains a GraphSAGE classifier for quark vs gluon discrimination,
- reports ROC-AUC and accuracy.

### [specific_task1_contrastive_graph_learning.ipynb](specific_task1_contrastive_graph_learning.ipynb)
Specific Task 1: Deep Graph Anomaly Detection with Contrastive Learning.

This notebook:
- reuses the graph-based jet representation idea,
- applies graph augmentations to create two contrastive views,
- trains a GIN-based encoder with an InfoNCE loss,
- extracts frozen graph embeddings,
- evaluates a downstream logistic regression classifier,
- plots the contrastive loss curve and ROC-AUC performance.

## Dataset

All three notebooks use the same quark/gluon HDF5 dataset:

- `quark-gluon_data-set_n139306.hdf5`
- 3 channels: ECAL, HCAL, Tracks
- image size: 125x125

The notebooks are written to avoid loading the full dataset into RAM at once. They use sequential chunk access and local batching to stay within typical CPU and GPU memory limits.

## Results Snapshot

| Task | Scope | Model | Main Metric | Outcome |
|---|---|---|---|---|
| Task 1 | Common | CNN Autoencoder | Reconstruction quality | ✅ Stable reconstructions on 125x125x3 events |
| Task 2 | Common | GraphSAGE | ROC-AUC | **0.7686** |
| Task 3 | Specific (Contrastive) | GIN + InfoNCE | ROC-AUC | **0.6480** baseline |

The Task 3 score is an intentional GraphCL baseline that surfaces the exact research gap on jet graphs: topology-breaking augmentations (notably edge dropping) undercut representation quality for this domain. The summer deliverable is to implement the GLADC-style dual-encoder strategy from Luo et al., *Scientific Reports* (2022), replacing destructive structural perturbations with structure-preserving noise/consistency design. If self-supervised graph objectives can close the gap to supervised performance on this dataset, that is a substantive contribution rather than incremental benchmarking.

## How to Run

1. Run [common_task1_autoencoder_quark_gluon.ipynb](common_task1_autoencoder_quark_gluon.ipynb) first to inspect the dataset and train the autoencoder.
2. Run [common_task2_jets_as_graphs_gnn.ipynb](common_task2_jets_as_graphs_gnn.ipynb) next to build graph representations and train the baseline GNN.
3. Run [specific_task1_contrastive_graph_learning.ipynb](specific_task1_contrastive_graph_learning.ipynb) for the contrastive-learning graph model.

## Notes

- The notebooks assume the dataset file is present in the repository root.
- Some cells are written for Kaggle-style environments, but they can be adapted to local execution.
- The graph notebooks create and reuse chunked artifacts in `processed_graphs/`.

## Author

Amine Gharout - ESI Algiers
