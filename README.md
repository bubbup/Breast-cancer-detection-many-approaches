# Approach1: Multi-task (ResNet-50)

# Model Overview:
- Encoder: ResNet-50 frozen as a feature extractor.
- Bridge: Bottleneck features from the deepest ResNet layer.
- Decoder (Segmentation): Transposed convolutions + skip connections from ResNet stages to reconstruct the mask
- Segmentation head: Conv2D layer producing (B, 256, 256, 1) sigmoid output.
- Classification head: Global Average Pooling on bridge features + Dense layers + Dropout → (B, 3) softmax output.

# Trainng
- Simultaneous Optimization: The model is defined with two outputs (seg_output and class_output) and compiled with a single optimizer.
- Shared Encoder Updates: The ResNet50 encoder acts as a shared backbone. In every training step (batch), gradients from both the Segmentation head (Dice Loss) and the Classification head (CrossEntropy) flow back into the ResNet50 encoder simultaneously. This forces the encoder to learn features that are useful for both tasks at once.


# Metrics

| Metric             | Value   | 
| ------------------ | ------- | 
| Accuracy           | 92.95%  |
| F1-Score           | 0.9213  |
| Dice Coefficient   | 0.5430  | 
| NSD (Surface Dice) | 0.5224  | 
| Inference Time     | 0.1495s | 
