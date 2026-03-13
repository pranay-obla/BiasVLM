# BiasVLM
Exploring CLIPs weakness in understanding numeracy, negation, attributes and spatial info
Mitigating Affirmation and Confirmation Bias in Vision-Language Models

Authors: Faisal ,Pranay Obla Anandbabu, Jacob Brewer

Affiliation: University of Southern California (USC)

📌 Overview

Vision-Language Models (VLMs) like CLIP and LLaVA often exhibit affirmation bias (agreeing with false premises) and confirmation bias (hallucinating visual evidence to support a text prompt). This project systematically measures and mitigates these biases using a new benchmark, Bias-Bench, and targeted parameter-efficient fine-tuning (LoRA).

Our key findings:

Bias is Multimodal: Text-only interventions fail to fix compositional errors.

Vision is Key: Updating the Vision Encoder in CLIP solved spatial and attribute binding biases (jumping from ~50% to 100% accuracy on spatial tasks).

Numeracy Ceiling: Counting remains a structural bottleneck, plateauing at ~59% accuracy even after fine-tuning.

🚀 Key Features

Bias-Bench: A unified benchmark targeting 4 logical failures:

Negation: ("This is not a dog")

Numeracy: ("There are 3 dogs" vs "2 dogs")

Attribute Binding: ("Red Cube" vs "Blue Cube")

Spatial Reasoning: ("Left of" vs "Right of")

Synthetic Data Generation: Custom scripts to generate CLEVR-style geometric scenes and ambiguous-free spatial datasets.

LoRA Fine-Tuning: Implementation of Low-Rank Adaptation for both Contrastive (CLIP) and Generative (LLaVA) models.

Ablation Studies: Comparison of Linear Probe vs. Text-Only vs. Vision+Text fine-tuning.

🛠️ Methodology

1. Benchmark Generation

We constructed Bias-Bench using a hybrid approach:

Real Images (COCO/VLMBias): Used for Negation and Numeracy tasks. We extended annotations to include "hard negative" captions.

Synthetic Images (CLEVR-style): Used for Attribute Binding and Spatial Reasoning. We wrote custom Python scripts (using PIL/NumPy) to generate scenes with strict geometric constraints to ensure unambiguous ground truth.

2. Mitigation Strategy

We applied Low-Rank Adaptation (LoRA) to fine-tune the models while keeping the pre-trained backbone frozen.

CLIP: We injected LoRA adapters ($r=8, \alpha=32$) into the attention layers ($W_q, W_v$) of both the Image and Text encoders. This was critical for solving visual binding failures.


📊 Results

| Task               | Baseline Accuracy | LoRA (Both) Accuracy | Improvement |
|--------------------|-------------------|----------------------|-------------|
| Negation           | 60.0%             | 91.5%                | +31.5%      |
| Numeracy           | 31.3%             | 58.7%                | +27.4%      |
| Attribute Binding  | 58.7%             | 88.0%                | +29.3%      |
| Spatial Relations  | 54.0%             | 100.0%               | +46.0%      |




🔧 Installation & Usage

Clone the repository

Install dependencies:

pip install torch torchvision transformers peft scikit-learn pillow tqdm

Generate Data:

testBed & trainBed.ipynb

Run Fine-Tuning & eval:

fine_tune.ipynb



🙌 Acknowledgements

VLMBias Dataset: Vo et al. (2025) for the base images used in negation/numeracy tasks.

CLEVR: Johnson et al. (2017) for inspiration on synthetic visual reasoning.

Hugging Face PEFT: For the LoRA implementation.

This project was completed as part of CSCI 566 at USC.
