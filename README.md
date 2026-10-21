# CSE620 Final Project — Vision Transformer for CIFAR-10 Image Classification

**Course:** CSE620 — Deep Learning with PyTorch  
**Presenters:** Van Q. Phung and Wanling Jiang  
**Project date:** October 21, 2026

## Project overview

This project implements a **Vision Transformer (ViT) from scratch in PyTorch** to classify the ten categories of the CIFAR-10 image dataset. The project includes the model architecture, training procedure, evaluation, attention visualization, and a runnable inference demonstration using the trained model checkpoint.

**Presentation requirement:** three slides covering the problem, solution, and results, followed by a **live executable demo**. The total presentation is limited to 10 minutes. The notebook, not the PowerPoint, runs the model.

## Repository contents

| File | Description |
| --- | --- |
| [CSE620_ViT_Project_Demo_Checkpoint_Fixed_Run.ipynb](CSE620_ViT_Project_Demo_Checkpoint_Fixed_Run.ipynb) | Executed notebook with ViT implementation, evaluation results, and live demo |
| [CSE620_ViT_Three_Slide_Presentation.pptx](CSE620_ViT_Three_Slide_Presentation.pptx) | Three-slide presentation: problem, solution, and results |
| [README.md](README.md) | Project overview and setup instructions |

**Open in Google Colab:** [CSE620 ViT project notebook](https://colab.research.google.com/drive/1RFp8RPebZpGcDAEZ4wfcmLO9p_Lmfq3G)

> **Model checkpoint:** The trained checkpoint `vit_cifar10.pt` is required for immediate inference and is **not included in this repository**. Obtain the checkpoint from the presenters and make it available at the path configured in the notebook. The notebook's Colab version may use a Google Drive checkpoint path; verify or update that path before running.

## Problem and data

CIFAR-10 contains 32 × 32 RGB images belonging to ten categories: airplane, automobile, bird, cat, deer, dog, frog, horse, ship, and truck.

- **Training:** 45,000 images
- **Validation:** 5,000 images
- **Official held-out test set:** 10,000 images

Training augmentation is applied only to the training subset. The test set is reserved for final evaluation.

## Solution: Vision Transformer

The model divides each image into **4 × 4 patches**, yielding 64 patch tokens. A learnable classification (`[CLS]`) token is added, giving a sequence of **65 tokens**. The architecture uses:

| Component | Configuration |
| --- | --- |
| Image size | 32 × 32 RGB |
| Patch size | 4 × 4 |
| Embedding dimension | 192 |
| Encoder blocks | 6 |
| Attention heads | 6 |
| Parameters | 2,693,578 |
| Recorded training | 30 epochs |

The ViT uses learned patch embeddings, positional embeddings, multi-head self-attention, feed-forward layers, residual connections, and a classification head.

## Results

The executed notebook records the following results:

| Metric | Result |
| --- | --- |
| Best validation accuracy | **79.22%** |
| Test accuracy (10,000 images) | **79.91%** |
| Correct test predictions | **7,991 / 10,000** |
| Training epochs in checkpoint | **30** |
| Trainable parameters | **2,693,578** |

The notebook includes a confusion matrix, classification report, training-history analysis, attention visualization, and examples of misclassified images. In the recorded live-demo execution, **6 out of 8** randomly selected test images were classified correctly; this small demonstration sample is **not** the full test accuracy.

These numbers come from the saved notebook evaluation; a new execution may produce a different eight-image demo result when a different sample is selected.

## Run the live demo

### Option A — Google Colab

1. Open the [Colab notebook](https://colab.research.google.com/drive/1RFp8RPebZpGcDAEZ4wfcmLO9p_Lmfq3G), or upload the `.ipynb` file from this repository to Colab.
2. Select a Python runtime (GPU optional).
3. Make the trained `vit_cifar10.pt` checkpoint available. If the notebook uses a Google Drive path, mount Drive and verify that the configured checkpoint path points to the correct **30-epoch checkpoint**.
4. Run the notebook's **LIVE DEMO 1/3**, **2/3**, and **3/3** cells in order. The first run may download the CIFAR-10 test dataset, so prepare this **before presenting**.
5. Rerun **LIVE DEMO 3/3** with a different random seed to show fresh predictions, true labels, and confidence values.

**Avoid retraining during the presentation.** Use the saved checkpoint; training a new model is unnecessary for the live demo.

### Option B — VS Code / local Jupyter

1. Clone this repository and open the notebook with a Python 3 Jupyter kernel:

   ```bash
   git clone https://github.com/vanp2026/CSE620_ViT_Final_Project.git
   cd CSE620_ViT_Final_Project
   python -m pip install torch torchvision numpy matplotlib scikit-learn tqdm jupyter
   ```

2. Place `vit_cifar10.pt` where the notebook expects it. Update the checkpoint path in the setup cell if it currently points to Google Drive.
3. Connect to the internet on the first run so `torchvision` can download CIFAR-10, or use the dataset already cached locally.
4. Execute the demo cells in order, using the saved checkpoint.

**Checkpoint troubleshooting:** If you see an error about a missing file, verify the full resolved path; if the training-history analysis reports only one epoch, it is likely reading a different checkpoint. The intended trained checkpoint contains a **30-epoch history**.

## Suggested 10-minute presentation plan

| Segment | Time |
| --- | --- |
| Slide 1 — Problem | 1 minute |
| Slide 2 — Solution | 1 minute |
| Slide 3 — Results | 1 minute |
| Live model inference in Colab / VS Code | 4 minutes |
| Discuss an error case and questions | 2 minutes |
| **Total** | **9 minutes** |

Prepare the runtime, checkpoint, and dataset ahead of time. The PowerPoint supports the talk, while the **executable notebook demonstrates that the project works**.

## Notes and limitations

- The reported accuracy is based on the CIFAR-10 test dataset and one trained model run; it is not an estimate of performance on arbitrary images or out-of-distribution data.
- Attention visualizations show attention allocation but are not causal explanations of classification decisions.
- No controlled CNN baseline or patch-size ablation is claimed.
- The trained checkpoint is necessary for reproducing the inference demo without retraining.
