# 🌍 AI Waste Sorting System: From CNNs to Transfer Learning
**Achieved 93.11% Accuracy in Waste Classification using Deep Residual Learning.**

---

## 📖 Project Overview
This project developed an automated computer vision pipeline to classify waste into six categories (**Cardboard, Glass, Metal, Paper, Plastic, and Trash**). The core engineering challenge was overcoming "Texture Bias"—ensuring the model doesn't just see "shiny" and guess "metal," but actually understands the material properties of the waste.

## 📈 The Evolution of the Model
I followed an iterative engineering process, moving from basic convolutional layers to advanced deep residual architectures.

| Architecture | Resolution | Technique | Test Accuracy |
| :--- | :--- | :--- | :--- |
| **WasteCNN_1** | 64x64 | Custom 3-Block CNN | 79.6% |
| **WasteCNN_2** | 64x64 | Deeper Architecture | 83.0% (Overfitted) |
| **WasteResNet** | 64x64 | **Residual Learning (Skip Connections)** | 83.3% (Stable) |
| **Final ResNet18** | **224x224** | **Transfer Learning & Fine-Tuning** | **93.11%** |

---

## 🧠 Key Engineering Challenges & Solutions

### 1. Vanishing Gradients & Skip Connections
Standard deep networks often "forget" the original image features as they go deeper. I implemented **Residual Blocks**, using identity shortcut connections ($Output = F(x) + x$) to ensure the gradient could flow backward without diminishing, allowing for a much more stable training process.

### 2. Overcoming Texture Bias (The "Broken Glass" Problem)
During testing, I discovered a fascinating failure: the model confused **Broken Glass** with **Metal** (98% confidence) because of the sharp, reflective jagged edges. 
* **The Solution:** I used **Confusion Matrix Analysis** to diagnose the overlap between classes and applied **Fine-Tuning** to the final convolutional block (`layer4`) of a pre-trained ResNet18. This allowed the model to recalibrate its high-level filters specifically for waste materials.

### 3. Hyperparameter Optimization
* **Learning Rate Scheduling:** Implemented `ReduceLROnPlateau` to dynamically drop the LR from $1e^{-4}$ to $1e^{-6}$ when the loss stalled.
* **Weight Decay:** Applied $L2$ Regularization ($1e^{-4}$) to keep the weights small and prevent memorization of the training set.

---

## 🛠️ Tech Stack
* **Framework:** PyTorch
* **Architecture:** ResNet18 (Transfer Learning)
* **Optimization:** Adam Optimizer + LR Scheduling
* **Visualization:** Matplotlib, Seaborn (Confusion Matrices), PIL
* **Dataset:** [TrashNet](https://github.com/garythung/trashnet) & TACO

---

## 🚀 Real-World Inference
The system includes a custom inference script capable of processing images directly from the internet or local storage.
* **Preprocessing:** Images are normalized to ImageNet statistics ($\mu=[0.485, 0.456, 0.406]$).
* **Robustness:** Successfully identifies complex waste items (crushed cans, crumpled paper) with **>70% confidence**.

---

## 🏗️ Future Roadmap
- [ ] **Webcam Integration:** Real-time sorting using OpenCV.
- [ ] **OOD Detection:** Training a "Miscellaneous" class to handle items outside the core categories.
- [ ] **3D Visualization:** Simulating the sorting process in a virtual environment.

---
*Developed as an exploration into Deep Learning, Computer Vision, and Sustainability.*