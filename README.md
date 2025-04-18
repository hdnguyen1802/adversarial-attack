# Enhancing Robustness in Feature Learning through Adversarial Training

This repository contains the code and resources for **the project of the course GE2340 - Artificial Intelligence – Past, Present, and Future**, focused on improving the robustness of deep neural networks (DNNs) against adversarial attacks through adversarial training.

## Abstract

Neural networks are often vulnerable to adversarial attacks, which are small perturbations to input data that can cause incorrect predictions. This project examined the accuracy performance of the neural networks by introducing two kinds of adversarial attacks on both defended and undefended models. The findings indicate that adversarial training helps models learn features that are crucial for prediction and resistant to such perturbations.

## Introduction

Feature learning is critical for the performance of machine learning models. While deep neural networks (DNNs) have automated much of this process, they remain susceptible to adversarial attacks. These attacks involve subtle input modifications that can mislead DNNs, highlighting the need for models to learn robust, human-interpretable features. Adversarial training, which involves training DNNs on both clean and adversarial examples, is a potential solution to enhance model defense and feature robustness. **This project of the course GE2340 - Artificial Intelligence – Past, Present, and Future** investigates the effectiveness of adversarial training by applying Fast Gradient Sign Method (FGSM) and Projected Gradient Descent (PGD) attacks to a DNN for image classification, aiming to build a resilient model that maintains accuracy despite adversarial perturbations.

## Repository Structure

This repository contains the following key components:

* **`final.ipynb`**: The Jupyter Notebook containing the Python code implementation for the project, including data loading, model definition, training (baseline and adversarial), attack generation, and evaluation.
* **`IEEE_report.pdf`**: The detailed report documenting the project's background, methodology, experiments, findings, and conclusions.
* **Pre-trained Model Weights **: To facilitate testing and exploration without the need for time-consuming retraining, the following pre-trained model state dictionaries are provided through the [link](https://drive.google.com/drive/folders/16geOQjMNB1GluZZQhenlpxcPTey46tPB?usp=sharing):
    * `resnet50_basic_training_state_dict.pth`: Weights for the baseline ResNet50 model trained on clean images.
    * `resnet50_fgm_state_dict.pth`: Weights for the ResNet50 model adversarially trained using the FGSM attack.
    * `resnet50_pgd_state_dict.pth`: Weights for the ResNet50 model adversarially trained using the PGD attack.
    * `resnet50_IAT_state_dict.pth`: Weights for the ResNet50 model trained using Interpolated Adversarial Training (IAT).


## Methodology

### Dataset

* **The project** utilizes the [Kaggle Animal Image Dataset](https://www.kaggle.com/datasets/iamsouravbanerjee/animal-image-dataset-90-different-animals), comprising 5400 images across 90 categories.
* The dataset was split 80/20 for training and testing, using a stratified split to maintain class proportions.

### Baseline Model

* A ResNet-50 architecture, pre-trained on ImageNet, served as the baseline.
* The model was fine-tuned for the 90 animal classes by modifying the output layer.
* Fine-tuning involved two phases: training only the last layer for 5 epochs (Adam optimizer, LR=0.0005) and then unfreezing all layers for 10 epochs (LR=0.0001).

### Adversarial Attacks

* **Fast Gradient Sign Method (FGSM):** A computationally efficient, single-step attack that uses model gradients to create adversarial examples.
* **Projected Gradient Descent (PGD):** A more advanced, iterative attack that takes multiple small steps to maximize prediction error, projecting perturbations back into allowed limits. PGD attacks with 7 and 20 iterations were used.

### Adversarial Training Models

Three defense models were developed using different adversarial training techniques:

1.  **FGSM Model:** Trained using adversarial examples generated by the FGSM attack.
2.  **PGD Model:** Trained using adversarial examples generated by the PGD attack (7 iterations, epsilon=0.1).
3.  **IAT Model (Interpolated Adversarial Training):**
    * Utilizes the Mixup technique to interpolate between clean examples and labels before calculating initial loss.
    * After parameter updates, PGD attack (7 iterations, epsilon=0.1) is applied to original images, and these perturbed images are used for further training.

## Evaluation & Results

The models' performance was evaluated based on accuracy against normal inputs and different adversarial attacks.

| Model          | Normal inputs (%) | FGSM attack (%) | PGD attack (7 iterations) (%) | PGD attack (20 iterations) (%) |
| :------------- | :---------------- | :-------------- | :---------------------------- | :----------------------------- |
| Baseline Model | 94.81             | 23.61           | 1.85                          | 2.13                           |
| FGSM Model     | 74.35             | 49.91           | 24.67                         | 10.37                          |
| PGD Model      | 74.81             | 50.19           | 58.24                         | 38.89                          |
| IAT Model      | 79.07             | 47.87           | 53.80                         | 28.06                          |

### Key Findings:

* Adversarial training significantly improved model robustness against attacks compared to the baseline. Accuracy under FGSM attack roughly doubled.
* The FGSM-trained model showed lower robustness against the more sophisticated PGD attacks compared to PGD-trained models.
* Training with PGD inputs provided the best overall defense against all tested attacks, though it requires more training time.
* A trade-off exists: adversarial training reduced accuracy on clean inputs by about 20%.
* Interpolated Adversarial Training (IAT) helped mitigate this trade-off, achieving higher accuracy on clean inputs (~5% more than FGSM/PGD models) while maintaining good robustness against attacks, even outperforming the FGSM model against PGD attacks.

## Conclusion

**This project of the course GE2340 - Artificial Intelligence – Past, Present, and Future** successfully demonstrated that adversarial training enhances the robustness of deep learning models against adversarial attacks, encouraging the learning of robust features. The results showed initial success: The model's accuracy when attacked by adversarial examples increased remarkably compared to the baseline model. Training with stronger adversarial examples yields better defense but increases training time. While adversarial training typically reduces performance on clean inputs, techniques like IAT can help balance robustness and standard accuracy.

## References

* Animal Image Dataset (90 different animals). (2022, July 17). [https://www.kaggle.com/datasets/iamsouravbanerjee/animal-image-dataset-90-different-animals](https://www.kaggle.com/datasets/iamsouravbanerjee/animal-image-dataset-90-different-animals)
* Bai, T. (2021, February 2). Recent advances in adversarial training for adversarial robustness. arXiv.org. [https://arxiv.org/abs/2102.01356](https://arxiv.org/abs/2102.01356)
* Kurakin, A. (2016, November 4). Adversarial machine learning at scale. arXiv.org. [https://arxiv.org/abs/1611.01236](https://arxiv.org/abs/1611.01236)
* Lamb, A., Verma, V., Kawaguchi, K., Matyasko, A., Khosla, S., Kannala, J., & Bengio, Y. (2022). Interpolated Adversarial Training: Achieving robust neural networks without sacrificing too much accuracy. Neural Networks, 154, 218-233. [https://doi.org/10.1016/j.neunet.2022.07.012](https://doi.org/10.1016/j.neunet.2022.07.012)
* Li, Y., Cheng, M., Hsieh, C., & Lee, T. C. (2022). A review of Adversarial Attack and Defense for classification methods. The American Statistician, 76(4), 329-345. [https://doi.org/10.1080/00031305.2021.2006781](https://doi.org/10.1080/00031305.2021.2006781)
* Madry. A. (2017, June 19). Towards deep learning models resistant to adversarial attacks. arXiv.org. [https://arxiv.org/abs/1706.0608](https://arxiv.org/abs/1706.0608)
* Zhang, H., Cisse, M., Dauphin, Y. N., & Lopez-Paz, D. (2017). mixup: Beyond Empirical Risk Minimization. ArXiv (Cornell University). [https://doi.org/10.48550/arxiv.1710.09412](https://doi.org/10.48550/arxiv.1710.09412)

### Feel free to submit issues or pull requests if you find any bugs or want to contribute improvements to the code or email me at haidangnguyen1815@gmail.com.

