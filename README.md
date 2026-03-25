# Text-Dataset-Quality-Pipeline
# Text Dataset Quality Pipeline

This project implements a production-grade data engineering pipeline designed to refine raw web text into high-quality datasets suitable for machine learning and language model pretraining. It replicates the workflows used by data teams to filter out noise, boilerplate, and low-quality content from massive public corpora.

## Project Overview

The core objective was to transform a raw sample of the English Wikipedia into a "clean" dataset. Large-scale models are highly sensitive to the quality of their training data; if the input is repetitive, toxic, or poorly structured, the model's performance will suffer. This pipeline addresses those issues through a multi-stage filtering process.

## Results

The pipeline significantly narrowed the dataset to a "High" and "Medium" quality subset, ensuring that only the most informative and well-structured text remained for downstream use.

* **Live Dashboard:** [View Processing Results](https://sara-iqbal.github.io/Text-Dataset-Quality-Pipeline/)
* **Development Notebook:** [Run the Code on Google Colab](https://colab.research.google.com/drive/1s0TXtk4p8yALdr1eUiPUN_RVT7HVz3ek#scrollTo=vWrSPlCdILsj)

## Methodology

The pipeline follows a sequential "funnel" approach, where each stage applies a specific set of rules to keep or discard documents.

### 1. Ingestion and Language Standardization
We began by streaming 5,000 documents from the English Wikipedia. A language detection filter was applied first to ensure the dataset remained strictly English, removing any cross-language contamination that could affect model training.

### 2. Structural Filtering
Text that is too short often lacks sufficient context, while excessively long documents are frequently outliers like logs or metadata. We removed documents falling outside the 50 to 100,000 word range. Additionally, we calculated the average line length to flag and remove "boilerplate" text—content that looks more like computer-generated code or menus than natural human language.

### 3. Deduplication
To prevent a model from memorizing specific phrases due to overexposure, we performed both exact and near-duplicate removal.
* **Exact:** Documents with identical cryptographic hashes for their first 500 characters were merged.
* **Near-Duplicate:** We used a sentence-level proxy to identify and remove documents that started with the same introductory text.

### 4. Quality Scoring and Entropy
Since measuring true "quality" is subjective, the pipeline uses information theory to score text. We calculated character and word entropy to measure the variety of information in a document. Repetitive "gibberish" or spam-like text has low entropy, while meaningful prose has higher entropy. These metrics, combined with a "bullet ratio" filter (to detect excessive lists), were used to generate a composite quality score from 0 to 100.

### 5. Toxicity and Classification
The final stage used pattern matching to flag content containing violent, hateful, or spam-oriented language. Based on the final scores and flags, every document was categorized into one of four tiers: High, Medium, Low, or Flagged.

Would you like me to help you draft the technical "Usage" section for this README so others know how to run your Python environment?
