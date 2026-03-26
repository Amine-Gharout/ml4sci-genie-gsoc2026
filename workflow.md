# Jupyter Notebook Coding Challenge: Quark/Gluon Jet Images

## Overview

This challenge involves completing a Jupyter notebook coding challenge on a real physics dataset of quark/gluon jet images. The completed notebook should be pushed to a public GitHub branch and submitted via Google Form. There are a total of 4 tasks.

## Tasks

### Common Task 1 — Mandatory for Everyone
**What it asks:** Autoencoder
**Details:**
*   A simple symmetric CNN.
*   Encoder compresses (3, 125, 125) → latent vector.
*   Decoder reconstructs back to (3, 125, 125).
*   Loss is MSE.
*   Train for 10–20 epochs.
*   **Final cell:** matplotlib grid showing 8 original vs 8 reconstructed side by side.

### Common Task 2 — Mandatory for Everyone
**What it asks:** Jets as Graphs
**Details:**
*   **Core task.** For each event:
    *   Extract non-zero pixels from all 3 channels → list of (x, y, channel, intensity) tuples = nodes.
    *   Build edges using k-nearest neighbors in (x, y) space with k=7.
    *   Node features: [x, y, ECAL_intensity, HCAL_intensity, Track_intensity].
    *   Label: 0 = quark, 1 = gluon.
*   Use `torch_geometric.data.Data(x=node_features, edge_index=edges, y=label)`.
*   Train a `GCNConv` or `GATConv` model with global mean pooling for graph-level classification.
*   Report accuracy and ROC-AUC.

### Specific Task 1 — Required for GENIE1 (Your Primary)
**What it asks:** Contrastive Learning (GENIE1)
**Details:**
*   Take the GNN encoder from Task 2.
*   Add a projection head.
*   Use NT-Xent loss (SimCLR style) or supervised contrastive loss.
*   **Idea:** embeddings of same-class jets should cluster together, different classes should repel.
*   After contrastive pre-training, add a linear classifier head and fine-tune.
*   Report accuracy and ROC-AUC on the test set.

### Specific Task 4 — Optional, but covers GENIE4 Simultaneously
**What it asks:** Non-local GNN (GENIE4, optional)
**Details:**
*   Replace `GCNConv` from Task 2 with `GATConv` (Graph Attention Network).
*   Attention weights capture long-range dependencies.
*   Run the same training loop.
*   Compare ROC-AUC of baseline GCN vs GAT in a table and a single ROC curve plot.
*   **Recommendation:** Do all 4 tasks (Common 1 + Common 2 + Specific 4 is a small extension of Common 2. One test submission covers two projects).

## Technical Stack

*   Everything in Python.
*   **Installation (if not present):**
    ```bash
    pip install torch torchvision torch-geometric
    pip install torch-scatter torch-sparse -f https://data.pyg.org/whl/torch-2.x.x+cpu.html
    pip install matplotlib scikit-learn numpy h5py
    ```

## Dataset

*   Google Drive link from the test doc: [drive.google.com/file/d/1WO2K-SfU2dntGU4Bb3IYBp9Rh7rtTYEr/view?usp=sharing](drive.google.com/file/d/1WO2K-SfU2dntGU4Bb3IYBp9Rh7rtTYEr/view?usp=sharing)
*   It is an HDF5 or numpy file with shape roughly `(N_events, 3, 125, 125)`.
*   **Action:** Download it first and verify the shape before writing any model code.

## What Good Looks Like vs. What Fails

*   **Good:**
    *   Clean notebook with section headers.
    *   Training loss curve plotted.
    *   Explicit metric table (accuracy, ROC-AUC).
    *   Brief written analysis of results (2–3 sentences per task explaining what you observe).
    *   Code is readable, not monolithic.
*   **Bad:**
    *   No metrics reported, just "it runs".
    *   No analysis.
    *   Copy-paste architecture with no understanding.
    *   A notebook that crashes.

## Action Plan

*   **Today, Mar 22:** Download the dataset. Set up the notebook. Complete Common Task 1 (autoencoder) as a warmup.
*   **Mar 23:** Common Task 2 (graph construction + GNN). This is the hardest part; budget 5–6 hours.
*   **Mar 24:** Specific Task 1 (contrastive, 3–4h) + Specific Task 4 (GAT comparison, 2h). Push to public GitHub branch.
*   **Mar 25:** Write the proposal using the notebook as evidence of your technical ability.

***

**Please note:** Download the dataset and verify its shape. I can then assist with writing the graph construction code from scratch.