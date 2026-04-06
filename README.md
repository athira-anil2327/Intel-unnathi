
# Image Deblurring using Residual U-Net

This repository implements a deep learning approach to image restoration, specifically targeting motion blur removal. The model utilizes a modified U-Net architecture integrated with residual blocks to effectively reconstruct sharp images from degraded inputs.

## Technical Overview

The project focuses on end-to-end mapping from blurred to sharp pixel spaces. Key technical components include:

### 1. Model Architecture
* **Encoder-Decoder Structure:** A U-Net based backbone for capturing multi-scale spatial information.
* **Residual Learning:** Integration of `ResidualBlock` modules within the bottleneck to facilitate deeper feature extraction and mitigate gradient vanishing issues.
* **Sub-pixel Reconstruction:** Utilizes transposed convolutions for upsampling to restore the original image dimensions ($256 \times 256$).

### 2. Loss Function
The model is trained using a composite loss function to ensure both pixel-level accuracy and structural integrity:
* **L1 Loss:** Minimizes the absolute difference between pixels, promoting sharper edges compared to L2 loss.
* **SSIM Loss:** A differentiable Structural Similarity Index (SSIM) loss component ($1 - \text{SSIM}$) to ensure the model preserves luminance, contrast, and structural textures.
* **Weighting:** $Loss = L1 + (0.5 \times SSIM\_Loss)$.

### 3. Training & Optimization
* **Optimizer:** Adam optimizer with a base learning rate of $1e-4$.
* **Scheduling:** `ReduceLROnPlateau` is implemented to dynamically reduce the learning rate when the loss stagnation is detected, ensuring finer convergence.
* **Data Augmentation:** Real-time geometric transformations (Horizontal Flips) and Gaussian blur simulation on the GOPRO dataset.

---

## Evaluation Metrics

The model is evaluated using two primary Image Quality Assessment (IQA) metrics:

| Metric | Description |
| :--- | :--- |
| **PSNR** | Peak Signal-to-Noise Ratio; measures pixel-level fidelity (higher is better). |
| **SSIM** | Structural Similarity Index; measures perceived structural integrity (closer to 1.0 is better). |

The included `visualize_and_evaluate` function provides a side-by-side comparison of **Blurred Input**, **Model Output**, and **Ground Truth**, calculating individual and average metrics across test samples.

---

## Usage

### Dependencies
* PyTorch / Torchvision
* Scikit-Image
* Matplotlib
* PIL

### Execution
1. Ensure the GOPRO dataset is structured in the `/content/GOPRO` directory.
2. The training loop runs for a default of 10 epochs (extendable to 500 for full convergence).
3. Post-training, the visualization script will automatically generate a performance report with 20 sample comparisons.

---

## Results
The implementation demonstrates a significant quantitative improvement in image quality. By combining L1 and SSIM losses, the model recovers high-frequency details that are typically lost in standard convolutional autoencoders.
