# Approach2: Multi-task + soft attention dual headed transformer [resnet-34]
<!-- [file:31] -->


## Model architecture
### MultiTaskAttentionUNet
- **Encoder:** ResNet-34  <!-- [file:31] -->
- **AttentionGate:** reweights skip features using a sigmoid attention map computed from decoder gating + encoder skip. <!-- [file:31] -->
- **DecoderBlock:** transposed-conv upsample + attention-gated skip concat + conv blocks. <!-- [file:31] -->
- **Segmentation head:** produces logits of shape `(B, 3, H, W)`. <!-- [file:31] -->
- **Classification head:** GAP on deepest encoder features + MLP → logits `(B, 3)`. <!-- [file:31] -->

---

## Training technique
The notebook trains the multi-task model with: <!-- [file:31] -->
- **Losses:** `CrossEntropyLoss` for segmentation and classification, combined as `loss = alpha*seg + (1-alpha)*cls`. <!-- [file:31] -->
- **Optimizer:** Adam. <!-- [file:31] -->
- **Scheduler:** ReduceLROnPlateau on validation loss. <!-- [file:31] -->
- **Early stopping:** stop if validation loss does not improve for a fixed patience. <!-- [file:31] -->

---

## Metrics (reported)
The notebook computes and prints: <!-- [file:31] -->
| Metric          | Score    |
| --------------- | ---------|
| Mean Dice (DSC) | 0.8647  |
| Mean NSD        | 0.5488 ​ |
| Macro F1        | 0.8600 ​ |
| Accuracy        | 86.01%  |
---

<!-- [file:31] -->
