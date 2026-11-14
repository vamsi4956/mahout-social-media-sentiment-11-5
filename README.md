# Case Study 11.5: Social Media Sentiment Clustering using Apache Mahout K-Means

## Overview
Unsupervised distributed text clustering of the **Hugging Face `dair-ai/emotion`** dataset (20,000 social media posts across 6 emotion classes: *sadness, joy, love, anger, fear, surprise*) using **Apache Hadoop (HDFS + MapReduce)** and **Apache Mahout 0.13.0**.

## Pipeline Architecture
1. **Data Extraction:** Loaded `train` (16,000), `validation` (2,000), and `test` (2,000) Parquet splits from `dair-ai/emotion`.
2. **SequenceFile Packing:** Converted 20,000 documents into Hadoop `SequenceFile` format via `mahout seqdirectory`.
3. **TF-IDF Vectorization:** Extracted normalized TF-IDF vectors (`mahout seq2sparse -wt tfidf -nv -md 5 -n 2`), yielding 19,998 non-empty document vectors.
4. **Distributed K-Means ($K=6$):** Clustered vectors on MapReduce using `CosineDistanceMeasure` (`-k 6 -x 20 -cd 0.001`), converging at **Iteration 16**.

## Cluster Distribution ($N = 19,998$)
| Cluster ID | Document Count | Share (%) |
| :--- | :---: | :---: |
| `1480` | 2,256 | 11.28% |
| `8283` | 3,317 | 16.59% |
| `12225` | 7,422 | 37.11% |
| `13986` | 1,488 | 7.44% |
| `14616` | 3,114 | 15.57% |
| `18543` | 2,401 | 12.01% |
| **Total** | **19,998** | **100.00%** |
