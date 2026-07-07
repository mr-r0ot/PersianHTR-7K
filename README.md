# PersianHTR-7K

A pretrained **Persian Handwritten Word Retrieval Model** for fast handwritten word recognition using **Embedding + FAISS**.

Unlike traditional OCR models that directly classify an image into one of thousands of classes, PersianHTR-7K converts each handwritten word image into a compact embedding vector and retrieves the most similar words from a FAISS index.

This model is designed to work together with **Korosh-HTR-Engine**, providing fast, scalable, and CPU-friendly handwritten word recognition.

## model is [here](https://huggingface.co/TahaGorji/PersianHTR-7K)
---

# Features

* Persian handwritten **word-level** recognition
* Retrieval-based recognition (Embedding + FAISS)
* CPU-only inference
* Portable model package
* Fast similarity search
* Easily scalable vocabulary
* Top-K candidate retrieval
* Designed for integration with downstream language models (LLMs)

---

# Model Overview

| Property            | Value                       |
| ------------------- | --------------------------- |
| Language            | Persian (Farsi)             |
| Recognition Level   | Word                        |
| Recognition Method  | Embedding + FAISS Retrieval |
| Encoder             | MobileNetV3-Small           |
| Embedding Dimension | 256                         |
| Index               | FAISS                       |
| Inference           | CPU                         |
| Output              | Top-K Candidate Words       |

---

# Training Dataset

The model was trained using a Persian handwritten word dataset.

| Property                 | Value                           |
| ------------------------ | ------------------------------- |
| Total Images             | **403,881**                     |
| Unique Words             | **7,391**                       |
| Average Samples per Word | ~55                             |
| Dataset Type             | Persian Handwritten Word Images |

Dataset structure:

```text
dataset/

    کتاب/
        00001.png
        00002.png
        ...

    مدرسه/
        ...

    ماهی/
        ...
```

---

# Training Configuration

| Parameter       | Value                  |
| --------------- | ---------------------- |
| Backbone        | MobileNetV3-Small      |
| Epochs          | 20                     |
| Embedding Size  | 256                    |
| Batch Size      | Auto (864 on Tesla T4) |
| Mixed Precision | Enabled                |
| FAISS           | Enabled                |

Training optimizations:

* Automatic Batch Size Tuning
* Automatic Checkpointing
* Automatic Resume
* GPU Image Augmentation
* Optimized Disk Cache
* AMP
* TF32
* channels_last

---

# Training Summary

| Metric                  | Value      |
| ----------------------- | ---------- |
| Images                  | 403,881    |
| Classes                 | 7,391      |
| Epochs                  | 20         |
| Final Training Accuracy | **68.2%**  |
| Final Training Loss     | **5.4345** |
| Generated Embeddings    | 403,881    |
| Embedding Dimension     | 256        |

Generated FAISS Index:

```text
Vectors   : 403,881
Dimension : 256
```

---

# About the Reported Accuracy

The reported **68.2% training accuracy is NOT the final capability of this model.**

This training run was intentionally performed as an **initial proof-of-concept** under limited computational resources and limited available training time.

Training was stopped after only **20 epochs**, even though the optimization process was still improving.

The model **did not show any sign of convergence or saturation** before training stopped.

During all training epochs:

* Training accuracy continuously increased.
* Training loss continuously decreased.
* No accuracy plateau was observed.
* No loss plateau was observed.

Training progression:

| Epoch |  Accuracy |       Loss |
| ----: | --------: | ---------: |
|     1 |      0.2% |     8.7655 |
|     5 |     22.2% |     7.3786 |
|    10 |     53.4% |     6.1466 |
|    15 |     65.5% |     5.5627 |
|    20 | **68.2%** | **5.4345** |

The training curves indicate that the network was **still learning** when training ended.

With additional training epochs, more computational resources, and further hyperparameter tuning, higher training accuracy and improved retrieval performance are expected.

> **Important:** PersianHTR-7K is intended to be used as a **retrieval model**, not as a conventional classifier. The classifier accuracy shown above is mainly used to learn a discriminative embedding space. For the intended application, retrieval metrics such as **Top-1 Accuracy**, **Top-K Accuracy**, and **Recall@K** are considerably more meaningful than classifier accuracy alone.

---

# Model Package

The exported model directory contains everything required for inference.

```text
output_model/

    model.pt

    faiss.index

    labels.json

    config.json

    metadata.json
```

Simply loading this directory is sufficient to perform inference.

---

# Example Output

```json
[
    {
        "word":"کتاب",
        "probability":0.83
    },
    {
        "word":"کباب",
        "probability":0.11
    },
    {
        "word":"حساب",
        "probability":0.06
    }
]
```

Instead of returning only one prediction, the model always returns the **Top-K most similar candidate words**, allowing downstream systems (such as LLMs) to make more informed decisions using contextual information.

---

# Recognition Pipeline

```text
Handwritten Word Image

            │

            ▼

Preprocessing

            │

            ▼

CNN Encoder

            │

            ▼

256-D Embedding

            │

            ▼

FAISS Search

            │

            ▼

Top-K Candidate Words

            │

            ▼

Probability Estimation

            │

            ▼

JSON Output
```

---

# Intended Workflow

```text
Handwritten Answer Sheet

            │

            ▼

Word Segmentation

            │

            ▼

PersianHTR-7K

            │

            ▼

Top-K Candidate Words

            │

            ▼

(Optional)

Large Language Model

            │

            ▼

Final Correct Word
```

---

# Limitations

Current version supports:

* Persian language only
* Word-level recognition only
* Offline inference
* CPU-only inference

Current version does **not** perform:

* Line segmentation
* Page layout analysis
* Text detection
* Full-page OCR
* Context-aware correction (handled by downstream systems)

---

# Future Work

Future versions may include:

* Larger Persian vocabulary
* Improved embedding models
* Retrieval-specific fine-tuning
* Better probability calibration
* Larger public evaluation benchmarks
* Better ranking algorithms
* Hard-negative mining
* More robust embeddings
* Improved retrieval accuracy

---

# Related Projects

Engine:

**Korosh-HTR-Engine**

https://github.com/mr-r0ot/Korosh-HTR-Engine

Dataset:

**Persian Word Handwritten Dataset**

https://www.kaggle.com/datasets/tahagorji/persian-word-handwritten-dataset

---

# License

Please refer to the LICENSE file included in this repository.

---

# Citation

If you use this model in academic research, publications, or commercial projects, please cite this repository.
