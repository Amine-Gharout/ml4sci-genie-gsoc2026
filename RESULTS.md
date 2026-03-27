# Results Summary - ML4SCI GENIE1 (GSoC 2026)

This document reports the evaluation outcomes for the three submitted notebooks and clarifies the modeling/engineering decisions behind them.

## Consolidated Results

| Task | Notebook | Model | Objective | Key Metric | Result | Interpretation |
|---|---|---|---|---|---|---|
| Task 1 | `common_task1_autoencoder_quark_gluon.ipynb` | CNN Autoencoder | Learn image-level jet representations from ECAL/HCAL/Tracks | Reconstruction quality (qualitative + loss trend) | Stable convergence with faithful reconstructions | Establishes a reliable representation-learning baseline on the full detector image tensor |
| Task 2 | `common_task2_jets_as_graphs_gnn.ipynb` | GraphSAGE | Supervised quark/gluon classification on graphized jets | ROC-AUC | **0.7686** | Strong baseline; graph construction and neighborhood aggregation retain class-discriminative jet structure |
| Task 3 | `specific_task1_contrastive_graph_learning.ipynb` | GIN + InfoNCE (GraphCL-style) | Self-supervised representation learning with downstream linear evaluation | ROC-AUC | **0.6480** | Intentional GraphCL baseline that exposes the core limitation and motivates the non-anomalous contrastive design needed for GENIE1 |

## Task 3 Baseline Analysis (Why 0.6480 Matters)

**This is my baseline. Here is exactly why it falls short.**

The GraphCL-style augmentation policy used in Task 3 relies on structural perturbations such as edge dropping. For jet graphs, this is not an innocuous transformation: removing edges can erase physically meaningful local neighborhoods and distort the topology that encodes shower morphology. The result is weaker invariant learning and a clear drop relative to the supervised GraphSAGE baseline.

This negative result is scientifically useful because it pinpoints the research gap addressed by GLADC: replace topology-breaking augmentations with perturbations that preserve graph structure while still creating informative views. In practice, Gaussian feature noise and consistency constraints provide a better inductive bias for jet data than aggressive structural corruption.

Reference: Luo et al., *Scientific Reports* (2022), GLADC dual-encoder framework for graph anomaly detection.

## Engineering Note: Data/Memory Pipeline

Naively materializing all graph objects from the full HDF5 source would exceed practical local memory limits (approximately 26 GB working set in this setup). I implemented sequential HDF5 chunk streaming with on-disk intermediate artifacts, reducing effective in-memory pressure to fit a 16 GB budget while keeping throughput high.

This engineering path is the reason the pipeline is runnable and reproducible under constrained hardware, with observed training throughput around **42 s/epoch** versus an impractical **~40 h** naive end-to-end preprocessing/training flow.

## Memory & Compute Budget

| Resource | Constraint / Hardware | Practical Budget Used | Notes |
|---|---|---|---|
| GPU | NVIDIA Tesla T4 | 16 GB VRAM cap respected | Batch sizing and graph mini-batching tuned to avoid OOM |
| System RAM | Commodity workstation envelope | <=16 GB active pipeline footprint | Achieved via chunk streaming and incremental graph serialization |
| Storage I/O | Local disk for intermediate graph chunks | Sequential read/write optimized | Avoids repeated full-dataset graph reconstruction |
| Training Throughput | End-to-end graph training | ~42 s/epoch | Operationally feasible under GSoC iteration timelines |
