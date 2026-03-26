# ML4SCI Genie GSoC 2026 Tasks

Below are the tasks that we will use to evaluate prospective students for the Genie projects. After completing the first 2 common tasks, please thoroughly complete the specific third task for your project of interest and (optionally) any other third task if you would like to also be considered for all Genie projects (this may increase your chances of success, but make sure to not do it at the expense of the specific project you are more interested in).

> **Note:** Please work in your own GitHub branch (i.e. **NO PRs should be made**). Send us a link to your code when you are finished and we will evaluate it.

**Dataset**: Data of Quark/Gluon jet events available [here](https://drive.google.com/file/d/1WO2K-SfU2dntGU4Bb3IYBp9Rh7rtTYEr/view?usp=sharing). The dataset consists of 3 channels (ECAL, HCAL and Tracks) each containing 125×125 images.

---

## Common Task 1 — Auto-encoder of the quark/gluon events

- Please train an auto-encoder to learn the representation based on three image channels (ECAL, HCAL and Tracks) for the dataset.
- Please show a side-by-side comparison of original and reconstructed events.

---

## Common Task 2 — Jets as graphs

Please choose a graph-based GNN model of your choice to classify (quark/gluon) jets. Proceed as follows:

1. Convert the images into a point cloud dataset by only considering the non-zero pixels for every event.
2. Cast the point cloud data into a graph representation by coming up with suitable representations for nodes and edges.
3. Train your model on the obtained graph representations of the jet events.

- Discuss the resulting performance of the chosen architecture.

---

## Specific Task 1 — Deep Graph Anomaly Detection with Contrastive Learning

> *(Complete this if you are interested in the "Deep Graph Anomaly Detection with Contrastive Learning" project)*

- Classify the quark/gluon data with a model that learns data representation with a contrastive loss.
- Evaluate the classification performance on a test dataset.

---

## Specific Task 2 — Learning Parametrization with Implicit Neural Representations

> *(Complete this if you are interested in the "Learning Parametrization with Implicit Neural Representations" project)*

- Use an INR model of your choice to represent the events in Task 1.
- Please show a visual side-by-side comparison of the original and reconstructed events and an appropriate evaluation metric of your choice.

---

## Specific Task 3 — Learning the Latent Structure with Diffusion Models

> *(Complete this if you are interested in the "Learning the Latent Structure with Diffusion Models" project)*

- Use a Diffusion Network model to represent the events in Task 1.
- Please show a side-by-side comparison of the original and reconstructed events and an appropriate evaluation metric of your choice that estimates the difference between the two.

---

## Specific Task 4 — Non-local GNNs for Jet Classification

> *(Complete this if you are interested in the "Non-local GNNs for Jet Classification" project)*

- Build a non-local GNN model to complete Task 2.
- Compare results for non-local GNN and baseline GNN using ROC-AUC as a metric.

---

## Test Submission Instructions

> ⚠️ **IN ORDER TO BE CONSIDERED AS AN APPLICANT FOR GOOGLE SUMMER OF CODE YOU MUST ALSO SUBMIT A PROPOSAL THROUGH THE GOOGLE SUMMER OF CODE PORTAL**

- Additional tasks are listed below.
- You must calculate and present the required evaluation metrics.
- When completed, please use [this Google form](https://forms.gle/SPXo8kSwHHptcBmk9) to submit.
