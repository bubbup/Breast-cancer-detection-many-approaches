# Approach2; Boundary Aware Multi-task Transformer (EfficientNet- b0)

## Model Architecture

The model follows a sequential workflow to ensure the classifier focuses on the tumor:

- Encoder: EfficientNet-B0 (pretrained via timm) extracting features at 5 stages.
- Decoder: Bilinear upsampling + skip connection concatenation + convolutional blocks.
- Boundary Attention: Explicitly reweights decoder features using the predicted segmentation mask (sigmoid activated) before passing them to the classifier.
- Segmentation head: outputs the Predicted Mask
- Classification head: Boundary-attended features → AdaptiveAvgPool (GAP) → Flatten → MLP (Linear-ReLU-Dropout-Linear) → logits (B, 3)

## Sequence:

- Input Image passes through the Encoder (EfficientNet-B0).

- Features go into the Decoder (U-Net style).

- The Decoder produces Spatial Features (high-resolution map) AND the Segmentation Mask.

- Boundary Attention Layer: This layer takes the Mask and multiplies it with the Spatial Features. It essentially "blacks out" the background, forcing the features to only exist where the tumor is.

- Classification: These masked features are then passed to the classifier.


| Metric   | Score      |
| ** DSC** | **0.7898** |
| ** NSD** | **0.5971** |
| ** F1**  | **0.8531** |
| ** Acc** | **0.8737** |
