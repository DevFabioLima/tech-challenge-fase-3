# 📘 Fine-Tuning LLM on Product-Specific (Running locally)

This project demonstrates the effectiveness of fine-tuning a large language model (LLM) to generate more contextually accurate and product-specific descriptions, using **Clarence Larkin's work on the Book of Revelation** as the test case.

## 🧪 Objective

To evaluate how fine-tuning a language model on curated data (product metadata and historical information) affects the quality and relevance of generated responses when describing a product.

## 📥 Instruction Format

The test used the following alpaca prompt structure:

```
### Instruction:
DESCRIBE ABOUT THE PRODUCT.

### Input:
The Book of Revelation
```

## 📤 Results

### ✅ Response Before Fine-Tuning

> *The Book of Revelation is a complex and enigmatic text originating from the Second Temple period of Judaism. It is a collection of apocalyptic visions and symbolic representations of divine judgment, spiritual warfare, and the ultimate triumph of God. This work explores themes of persecution, exile, and salvation...*

**Analysis:**  
This response is generic and rooted in academic or theological commentary. It lacks alignment with Clarence Larkin’s distinctive focus on prophecy, dispensationalism, or his specific publication history.

---

### 🔁 Response After Fine-Tuning

> *The Book of Revelation. A second edition of the first edition of the Book of Revelation will provide clarity and understanding for those with an established theological background, or those who are new to the book. A text for all who seek to understand the end of days...*

**Analysis:**  
This version includes specific product-related references:
- Mentions of multiple editions (true of Larkin's work)
- Language suitable for both scholars and lay readers
- Emphasis on themes central to Larkin's theology: end times, divine control, and hope

---

## 📌 Summary

Fine-tuning resulted in a significantly improved output that more accurately reflects:
- The historical context of Clarence Larkin's publication
- The theological tone and dispensationalist framework of the original book
- The marketing language expected in a product description

## ⚙️ Tools Used

- [Unsloth](https://github.com/unslothai/unsloth) for efficient fine-tuning
- Hugging Face Transformers
- PyTorch
- Model unsloth/gemma-3-1b-it-unsloth-bnb-4bit
- Base dataset [HuggingFaces](https://huggingface.co/datasets/beTinti/AmazonTitles-1.3MM/viewer)
- Custom dataset based on product metadata and descriptions

## ⚙️ Hardware Configs

- GPU NVIDIA GeForce RTX 3060 12GB
- RAM 32GB
- SO Ubuntu 22.04.5 LTS


## ⚙️ Fine-Tuning Configuration

The model was fine-tuned using the `SFTTrainer` from the Hugging Face ecosystem. Below are the key hyperparameters and training settings used in this test:

### 🧾 Trainer Parameters

- **model / tokenizer**: Preloaded base model and tokenizer.
- **train_dataset**: Dataset prepared focused on product descriptions.
- **dataset_text_field**: `"text"` — field used for input data.
- **max_seq_length**: Set to limit input token length for memory efficiency.
- **dataset_num_proc**: `2` — number of processes used to preprocess the dataset in parallel.
- **packing**: `False` — disabled to avoid sentence merging in training samples.

### 🔧 TrainingArguments

- **per_device_train_batch_size**: `2` — batch size per GPU.
- **gradient_accumulation_steps**: `4` — simulates a larger effective batch size (2 × 4 = 8).
- **warmup_steps**: `5` — warm-up phase to stabilize early training.
- **max_steps**: `60` — total number of steps (early stopping to test effectiveness).
- **learning_rate**: `2e-4` — moderately high LR for quick convergence in low-step runs.
- **fp16 / bf16**: Mixed-precision training depending on hardware capabilities.
- **logging_steps**: `1` — log every step for detailed tracking.
- **optim**: `"adamw_8bit"` — memory-efficient optimizer using 8-bit precision.
- **weight_decay**: `0.01` — regularization to prevent overfitting.
- **lr_scheduler_type**: `"linear"` — learning rate decays linearly.
- **seed**: `3407` — for reproducibility.
- **output_dir**: `"outputs"` — directory where checkpoints and logs are saved.

## 📝 Conclusion

The Book of Revelation was just one example used to test the effectiveness of fine-tuning. The dataset includes multiple products, and this project aims to evaluate how well the model adapts across various product types. This example illustrates the potential of domain-specific fine-tuning to improve content generation in e-commerce, publishing, and digital product descriptions.
