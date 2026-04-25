# Laboratory_5
https://colab.research.google.com/drive/1xf9GOiidR0NUSagsbqc6k7AaIdzgIY_A?usp=drive_link
  1. Which pre-trained model achieved the highest accuracy? Why?
The ResNet50 model achieved the highest accuracy. This is because it uses residual connections, which allow it to train deeper networks effectively without suffering from vanishing gradient problems. As a result, it can extract more complex and meaningful features from the images.
2. Which model had the lowest performance? What could be the reason?
The VGG16 model had the lowest performance. This may be due to its older architecture, which lacks modern improvements such as skip connections and parameter efficiency. It also has a large number of parameters, making it prone to overfitting and slower learning.

3. How did loss values compare across models?
The model with the best performance (ResNet50) showed the lowest validation loss, indicating better generalization. Meanwhile, VGG16 had higher validation loss, suggesting weaker learning. MobileNetV2 had moderate loss values, balancing efficiency and performance.
4. Why is accuracy not enough to evaluate a model?
Accuracy alone does not provide a complete picture of model performance, especially in multi-class classification. It does not show how well the model performs on individual classes or how it handles imbalanced data. Metrics like precision, recall, and F1-score give a more detailed evaluation.

5. Which model had the best F1-score? What does it indicate?
ResNet50 had the best F1-score. This indicates that it achieved a good balance between precision and recall, meaning it correctly identified most classes while minimizing both false positives and false negatives.

6. How did Precision and Recall differ across models?
ResNet50 showed consistently high precision and recall across most classes. MobileNetV2 had slightly lower recall, meaning it missed some correct predictions. VGG16 had lower precision, indicating more false positives.
7. Which classes were frequently misclassified?
Classes with similar visual features—such as “cup” and “glass” or “keyboard” and “laptop”—were frequently misclassified. This is because the models struggled to distinguish between visually similar objects.

8. What patterns did you observe in the confusion matrix?
Most errors occurred between related or similar classes. The diagonal values were highest for well-learned classes, while off-diagonal values showed confusion between similar categories. This indicates that the model learned general features but struggled with fine distinctions.
9. Which model had the highest AUC score?
ResNet50 achieved the highest AUC score, indicating superior classification performance across all classes.

10. What does AUC tell us about model performance?
AUC (Area Under the Curve) measures the model’s ability to distinguish between classes. A higher AUC means the model is better at correctly separating different classes and making reliable predictions.
11. What did Grad-CAM reveal about model decision-making?
Grad-CAM showed which regions of the image influenced the model’s predictions. It revealed that better-performing models focused on the main object, while weaker models sometimes focused on irrelevant areas.

12. Did the model focus on relevant image regions?
Yes, especially ResNet50. It consistently focused on the key object in the image. However, VGG16 sometimes focused on background regions, which may explain its lower performance.

13. Which model produced the most meaningful heatmaps?
ResNet50 produced the most meaningful and accurate heatmaps, highlighting the correct object regions more clearly than the other models.
14. Which model would you recommend for deployment? Why?
ResNet50 is recommended for deployment because it achieved the highest accuracy, F1-score, and AUC. It also demonstrated better generalization and more reliable predictions.

15. How can you further improve your best-performing model?
The model can be improved by:
- Applying data augmentation
- Fine-tuning the deeper layers
- Increasing dataset size
- Using more training epochs
- Hyperparameter tuning

16. How can your model be applied in real-world scenarios?
This model can be used in applications such as:

Object recognition systems
Smart inventory management
Mobile apps for object detection
Assistive technology

17. What are the risks of deploying an inaccurate model?
An inaccurate model may produce incorrect predictions, which can lead to wrong decisions, reduced user trust, and potential harm depending on the application (e.g., medical or safety systems).

18. How can this system be integrated into a mobile/web app?
The model can be integrated by:
- Converting it to TensorFlow Lite for mobile apps
- Using a backend API (Flask or FastAPI)
- Connecting it to a frontend interface where users upload images for prediction
