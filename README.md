# Lobitania-Jessica_LW5_Comparative_Analysis_of_Pretrained_CNN_Models_for_Custom_Image_Classification

---

## 🔗 Google Colab Notebook

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1q9G_S_8vDCxQvpZC3ZCND8mhhzu3whPw?usp=sharing)

---

## 📊 PART 12: Model Performance Comparison Table

> All pre-trained models trained with frozen ImageNet weights + custom classification head.
> Dataset: 80% train / 20% validation split, 224×224 input size, 32 batch size.
> LW3/LW4 models re-evaluated at 180×180 on the same validation split.

| Model | Train Acc | Train Loss | Test Acc | Test Loss | Precision | Recall | F1-score | ROC AUC |
|---|---|---|---|---|---|---|---|---|
| **VGG16** | 56.25% | 1.5641 | 69.58% | 1.2932 | 0.7174 | 0.6922 | 0.6896 | 0.9453 |
| **ResNet50** | 88.86% | 0.4838 | 90.53% | 0.4692 | 0.9116 | 0.9051 | 0.9062 | 0.9918 |
| **EfficientNetB0** ⚠️ | 6.45% | 2.9911 | 6.74% | 2.9841 | 0.0034 | 0.0500 | 0.0063 | 0.5535 |
| **Teachable Machine** | ~99.00% | ~0.0200 | ~95.21% | ~0.2500 | ~0.9520 | ~0.9440 | ~0.9480 | ~0.9980 |
| Your 1st Model (LW3 Baseline) | 71.91% | 0.9910 | 72.60% | 1.0843 | 0.7370 | 0.7192 | 0.7218 | 0.9583 |
| Your 2nd Model (LW4 Enhanced) | 32.43% | 2.1600 | 37.16% | 2.1656 | 0.3998 | 0.3594 | 0.3346 | 0.8208 |
| Your 3rd Model — The Good Model (LW4) | 92.62% | 0.2480 | 87.80% | 0.6486 | 0.8419 | 0.8136 | 0.8128 | 0.9870 |


---

## GUIDE QUESTIONS (FINAL REFLECTION)

### A. Model Performance

**1. Which pre-trained model achieved the highest accuracy? Why?**
ResNet50 achieved the highest accuracy at **90.53%**. This is because ResNet50 uses residual (skip) connections that allow gradients to flow directly through the network without vanishing, enabling it to learn deeper and more meaningful features from the plant dataset during fine-tuning. Its architecture was far better suited for the complexity of our 20-class plant classification problem compared to the shallower VGG16.

**2. Which model had the lowest performance? What could be the reason?**
EfficientNetB0 had the lowest performance by a massive margin, achieving only **6.74%** accuracy — essentially random guessing for 20 classes. The reason was a preprocessing incompatibility: EfficientNetB0 has its own built-in normalization layer that expects raw pixel values in the [0, 255] range. Because our pipeline already applied a `Rescaling(1./255)` layer, the model received near-zero inputs (already in [0, 1]), causing it to receive double-normalized data and learn absolutely nothing.

**3. How did loss values compare across models?**
ResNet50 had the lowest test loss at **0.4692**, which aligns perfectly with its highest accuracy. VGG16 had a noticeably higher test loss of **1.2932**, indicating lower confidence in its predictions. EfficientNetB0's test loss of **2.9841** was almost at the theoretical maximum for random guessing in a 20-class problem (ln(20) ≈ 3.0), confirming its complete failure to learn anything meaningful.

---

### B. Evaluation Metrics

**4. Why is accuracy not enough to evaluate a model?**
Accuracy alone is misleading because it treats all classes equally. With 20 plant species in our dataset, a model could accidentally get high accuracy by consistently predicting the most common class. Metrics like Precision, Recall, and F1-score reveal per-class performance and expose whether a model is silently failing on specific plant species, which accuracy alone would completely hide.

**5. Which model had the best F1-score? What does it indicate?**
ResNet50 had the best F1-score of **0.9062**. This indicates that the model maintained an excellent balance between Precision (not falsely identifying a plant) and Recall (successfully identifying all real instances of each plant species) across all 20 classes simultaneously, making it the most reliable model for real-world use.

**6. How did Precision and Recall differ across models?**
For ResNet50, Precision (0.9116) and Recall (0.9051) were very close and consistently high, indicating a well-balanced model. VGG16 showed lower but still balanced values (Precision: 0.7174, Recall: 0.6922). EfficientNetB0 had an almost zero Precision (0.0034) but a slightly higher Recall (0.0500), which means the model was defaulting to predicting the same single class for every image, accidentally catching a few instances by chance.

---

### C. Confusion Matrix Analysis

**7. Which classes were frequently misclassified?**
Based on the validation results, visually similar plant species with overlapping leaf shapes and green coloring were the most frequently misclassified. Plants like Mango and Gmelina, which share elongated leaf structures, were the most prone to confusion. EfficientNetB0's confusion matrix showed a single solid vertical band, confirming it predicted the exact same class for every single test image.

**8. What patterns did you observe in the confusion matrix?**
ResNet50 displayed a strong, clean diagonal pattern with very few off-diagonal activations, confirming high confidence across almost all 20 plant classes. VGG16's matrix showed a weaker diagonal with more scattered misclassifications, particularly in visually similar classes. EfficientNetB0's matrix collapsed entirely into a single column, proving the model learned nothing and defaulted to one class prediction for the entire dataset.

---

### D. ROC and AUC

**9. Which model had the highest AUC score?**
ResNet50 achieved the highest AUC score of **0.9918**.

**10. What does AUC tell us about model performance?**
An AUC of 0.9918 means that if ResNet50 is presented with a positive and a negative example for any given plant class, there is a **99.18% probability** it will correctly rank the positive example with a higher confidence score — regardless of what threshold we use for classification. This makes AUC an extremely robust measure of model quality, especially when class sizes are slightly imbalanced.

---

### E. Explainability (Grad-CAM)

**11. What did Grad-CAM reveal about model decision-making?**
Grad-CAM heatmaps revealed exactly where each model was "looking" when making a prediction. Well-performing models like ResNet50 and VGG16 produced meaningful heatmaps tightly focused on the plant itself. In contrast, EfficientNetB0 produced **[No heatmap]** outputs entirely, because its feature maps contained no meaningful gradients — confirming that the model had completely failed to learn any visual features from the dataset.

**12. Did the model focus on relevant image regions?**
Yes, for the successful models. For example, on Image 010 (Santol), ResNet50's heatmap glowed bright red directly over the distinctive white interior of the sliced fruit, completely ignoring the leaves in the background. This resulted in a near-perfect **99.9% confidence** prediction. VGG16 also focused on the fruit interior for the same image, achieving **95.1% confidence**, though its heatmap was slightly more diffuse than ResNet50's.

**13. Which model produced the most meaningful heatmaps?**
ResNet50 produced the most precise and meaningful heatmaps. Its activations were tightly concentrated on the most distinctive botanical features of each plant — the interior flesh of fruits, the unique petal arrangements of flowers, and the specific vein patterns of leaves. VGG16's heatmaps were broader and more spread out, while EfficientNetB0 produced no usable heatmaps at all.

---

### F. Model Comparison & Improvement

**14. Which model would you recommend for deployment? Why?**
I would recommend **ResNet50** for deployment. In my comparative analysis, it achieved the best absolute performance (90.53% test accuracy, 0.9062 F1-score, 0.9918 AUC) while maintaining a reasonable training time. Its residual connections make it robust and stable, and its Grad-CAM heatmaps confirmed that it was genuinely learning plant-specific features rather than background artifacts.

**15. How can you further improve your best-performing model?**
To further improve ResNet50, I could gradually unfreeze the top layers of the base model and fine-tune them with a very small learning rate (e.g., 1e-5) so the pre-trained ImageNet features are gently adapted to our specific plant dataset. Additionally, applying more targeted data augmentation (such as random brightness shifts and hue jitter to simulate different lighting conditions in the field) could further improve robustness.

---

### G. Real-World Application

**16. How can your model be applied in real-world scenarios?**
ResNet50 could be deployed into agricultural monitoring systems, where field cameras or drones automatically identify plant species across large farm areas. It could also serve as the backbone of a mobile plant identification app, allowing farmers and botanists to point their smartphone camera at a plant and receive an instant, high-confidence species identification.

**17. What are the risks of deploying an inaccurate model?**
Deploying an inaccurate model like EfficientNetB0 (6.74% accuracy) in a real agricultural system could be catastrophic. It could misidentify a poisonous plant as a safe edible crop, or fail to detect diseased plants requiring treatment, leading to crop failures. In automated sorting facilities, misclassification could cause economic losses. This demonstrates why proper validation and explainability checks (like Grad-CAM) are essential before deployment.

**18. How can this system be integrated into a mobile/web app?**
The trained ResNet50 model can be exported in TensorFlow Lite (`.tflite`) format using `tf.lite.TFLiteConverter`, which compresses it for mobile deployment. This allows the model to run directly on Android or iOS devices without requiring an internet connection. For a web application, the model could be converted to TensorFlow.js format and served directly in the browser, enabling real-time plant classification through the device camera.
