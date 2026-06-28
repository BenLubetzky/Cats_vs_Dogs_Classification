# Cat vs Dog Image Classification

Binary image classification using transfer learning and fine-tuning with three pre-trained backbone models: MobileNetV2, EfficientNetV2B1, and InceptionV3.

---

## Table of Contents

- [Overview](#overview)
- [Models](#models)
- [Results](#results)
- [Results Analysis](#results-analysis)
- [Recommendation](#recommendation)
- [Implementation Highlights](#implementation-highlights)
- [Planning & Process](#planning--process)
- [What I Learned](#what-i-learned)
- [What I'd Do Differently](#what-id-do-differently)

---

## Overview

This project tackles binary classification to separate cat and dog images. Three models were built using transfer learning and fine-tuning, then compared across multiple metrics to determine the best candidate for deployment, particularly in resource-constrained environments such as mobile devices.

---

## Models

- **MobileNetV2**
- **EfficientNetV2B1**
- **InceptionV3**

All models were evaluated on accuracy, F1 score, precision, recall, ROC-AUC, inference time, and model size.

---

## Results

| Model | Accuracy | F1 Score | Precision | Recall | ROC-AUC | Inference (ms/sample) | Model Size (MB) |
|---|---|---|---|---|---|---|---|
| **MobileNetV2** | 99.00% | 0.9900 | 0.9901 | 0.9900 | 0.9984 | **3.83** | **23.92** |
| **EfficientNetV2B1** | **99.25%** | **0.9925** | **0.9925** | **0.9925** | **0.9990** | 5.78 | 42.60 |
| **InceptionV3** | 99.12% | 0.9912 | 0.9913 | 0.9912 | 0.9977 | 6.06 | 141.97 |

> Best results per column are marked in bold.

---

## Results Analysis

### Accuracy, F1 Score, Precision, Recall, ROC-AUC

EfficientNetV2B1 led on most classification metrics, but the margins across all three models were extremely narrow — the largest gap in accuracy was just 0.25%. After reviewing wrongly classified samples, it became clear that a portion of the errors stemmed from data quality issues rather than model failure. Examples include:

- Images that contained neither a cat nor a dog
- Images containing both animals simultaneously
- Low-quality images where the subject was indistinguishable

Given these data quality issues, the classification metrics alone are not a reliable basis for making a model recommendation.

### Inference Time

MobileNetV2 recorded the lowest inference time at **3.83 ms/sample**, nearly 2ms faster than EfficientNetV2B1 at 5.78 ms/sample. For mobile or real-time applications, this margin is meaningful and was weighted heavily in the final recommendation.

### Model Size

MobileNetV2 is the smallest model at **~24 MB**, compared to EfficientNetV2B1 at ~43 MB (roughly 1.8x larger) and InceptionV3 at ~142 MB (roughly 6x larger). Storage is a critical constraint on mobile devices, making this metric equally important to inference time.

### A Note on the Dataset

Several metrics became less decisive due to the presence of problematic data in the dataset. Potential remedies include:

- Sourcing a higher-quality, verified dataset
- Manually filtering out noisy or irrelevant samples
- Accepting the noise if the proportion of problematic data is small enough not to skew results significantly

It is also worth noting that in other projects, heavier models like InceptionV3 may demonstrate a clear advantage in accuracy and precision when the data is more complex. In this case, the task did not appear to require that extra complexity.

---

## Recommendation

**MobileNetV2** is the recommended model.

Since all three models performed within 0.25% of each other on every classification metric, the deciding factors were inference time and model size — both of which MobileNetV2 wins by a clear margin. It is nearly twice as small as the next smallest model and almost 2ms faster per sample, making it the natural choice for a lightweight, efficient deployment.

---

## Implementation Highlights

- **Organized into functions** — kept the codebase structured and easy to modify throughout the project.
- **Single output neuron with sigmoid** instead of two neurons with softmax — a cleaner and more standard architecture for binary classification. Required adjustments to the TF datasets pipeline and evaluation function, informed by research into the trade-offs.
- **Model size added to the evaluation function** — directly relevant to the final recommendation and not typically included by default.
- **CNN model** — a plain CNN was initially planned and implemented but was ruled out. The code remains in the project (commented out) to document the reasoning behind that decision.

---

## Planning & Process

The project began with a plan to use three datasets and build four models (one plain CNN plus three transfer learning models). Over time the scope was refined:

- The first dataset proved sufficient on its own; the other datasets were too clean and did not add useful challenge.
- The CNN model was ruled out after analysis, but kept in the codebase as documentation.

Despite these changes, the core structure held up throughout. The upfront planning made the project significantly faster to execute compared to previous work.

---

## What I Learned

- **Working in a more organized manner** — planning and documenting everything ahead of time made the process smoother and the output more professional.
- **Why the CNN model didn't work** — understanding the failure helped build intuition around the challenges of training deep networks from scratch.
- **Image preprocessing edge cases** — some images had 2-channel formats which caused downstream errors, and image corruption appeared when combining datasets, both of which required research and handling.

---

## What I'd Do Differently

- **Divide the project into more modular sections** from the start, allowing each phase to be adjusted based on what the previous phase revealed.
- **Explore data augmentation** and more advanced model types to continue building experience with different tools and documentation styles — a skill with practical long-term value.

---

*Hope you enjoyed the project.*
