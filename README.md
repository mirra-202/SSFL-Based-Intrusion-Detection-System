# SSFL-IDS: Semi-Supervised Federated Learning for Intrusion Detection

A novel Intrusion Detection System (IDS) built using Semi-Supervised Federated Learning to detect anomalies and attacks in IoT networks. The system leverages the N-BaIoT dataset to simulate a distributed environment where edge devices collaboratively learn without sharing sensitive data, enhancing both privacy and performance.

## Overview

**SSFL-IDS** is designed to:
- Train intrusion detection models across multiple IoT devices in a **federated** and **privacy-preserving** manner.
- Handle unlabeled open data using **semi-supervised learning**.
- Integrate **Hard Label Voting** and **Knowledge Distillation** to improve prediction accuracy without centralizing data.

## Key Components

### 1. Federated Learning Setup
- Simulates IoT devices as clients, each with its own private dataset.
- A central server coordinates training but never accesses raw data.
- Ensures **data privacy**, **decentralized learning**, and **scalability**.

### 2. Semi-Supervised Learning
- Uses small labeled private datasets and a larger unlabeled open dataset.
- Clients learn from the unlabeled data through a two-stage training process.
- Reduces reliance on manually labeled data, which is scarce in real-world IoT settings.

### 3. Hard Label Voting
- Each client makes predictions on the open dataset.
- Predictions are converted to hard labels (via argmax).
- The final pseudo-label is decided through **majority voting** across clients.

### 4. Knowledge Distillation
- Server uses the hard-voted labels to train a student model.
- The knowledge is then distilled into client models during the second stage of training.
- Helps clients align with a more generalized and robust global model.

## Dataset

- NBaIoT Dataset - contains traffic from multiple IoT devices such as baby monitors, webcams, and thermostats under normal and attack conditions.
- Automatically split into:
  - **Private datasets:** Local to each client
  - **Open dataset:** Used for semi-supervised learning
  - **Test dataset:** Used for evaluation

## Model Architecture

- **Classification Model:** A Multi-Layer Perceptron (MLP) to classify traffic into 11 attack categories.
- **Discrimination Model:** A binary classifier (MLP) to distinguish between known (private) and unknown (open) data samples.

Both models are lightweight and optimized for the low-compute environment of IoT devices.

## Workflow

1. **Initialize** clients and server using private and open datasets.
2. **Stage I Training:** Clients train classification and discrimination models using private data.
3. **Open-set Evaluation:** Clients predict on open data.
4. **Hard Label Voting:** Combine predictions from all clients to get pseudo-labels.
5. **Stage II Training:** Use pseudo-labels and **knowledge distillation** to refine client models.
6. **Repeat** the above steps for several rounds to improve model performance.
7. **Final Evaluation** is done using the test dataset on the global model.
