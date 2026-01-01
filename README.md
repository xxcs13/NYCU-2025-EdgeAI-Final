# 2025-NYCU-EdgeAI-Final

## Project Overview

This project is the final assignment for the Edge AI course. Its primary objective is to deploy an ExLlamaV2-based language model and conduct a comprehensive evaluation that includes text generation, inference performance benchmarking (throughput), and language modeling quality assessment using perplexity (PPL).

## Core Features

* Loading and execution of an ExLlamaV2 Q4 quantized model
* Text generation using a customized `generate` function
* Perplexity (PPL) evaluation on the WikiText-2 dataset
* Measurement of inference throughput (tokens generated per second)
* Exporting evaluation results to `19.csv`

## Execution Instructions

### 1. Environment Setup

```bash
git clone https://github.com/eric1236002/2025-NYCU-Edgeai-Final.git
conda create -n exllama python=3.12
conda activate exllama
git clone https://github.com/turboderp-org/exllamav2
cd exllamav2
pip install -r requirements.txt
<-----T4 only---->
conda install nvidia::cuda-toolkit
<-----T4 only---->
pip install . --use-pep517 --no-build-isolation
pip install datasets==3.5.0
```

### 2. Model Preparation

* Download the ExLlamaV2 Q4 quantized model from
  [https://huggingface.co/wei123602/exllama-Llama-3.2-3B-Instruct-3.0bpw](https://huggingface.co/wei123602/exllama-Llama-3.2-3B-Instruct-3.0bpw)
* Place the downloaded model under the `./llama3.0bpw` directory, or modify the `model_path` variable in `result.py` accordingly.

```bash
cd 2025-NYCU-Edgeai-Final
pip install -U huggingface_hub
huggingface-cli download --resume-download --local-dir ./llama3.0bpw wei123602/exllama-Llama-3.2-3B-Instruct-3.0bpw
```

Alternatively, the model can be quantized manually:

```bash
cd exllamav2
python convert.py \
    -i  /root/Sherry/EdgeAI/final/1w/llama-3.2-3b-instruct \
    -o  /root/Sherry/EdgeAI/final/1w/tmp \
    -cf /root/Sherry/EdgeAI/final/1w/llama3.0bpw/ \
    -b 3.0
```

### 3. Running the Main Program

```bash
python result.py
```

### 4. Output Results

* The evaluation results are automatically written to `19.csv` with the following format:

| Id | Value      |
| -- | ---------- |
| 0  | PPL        |
| 1  | Throughput |

## Experimental Results Summary

| Metric     | AWQ 4bit + LoRA | AWQ 4bit | HQQ + StaticCache | ExLlamaV2 (3.0 bpw) | ExLlamaV2 (2.5 bpw) |
| ---------- | --------------- | -------- | ----------------- | ------------------- | ------------------- |
| Perplexity | 11.6            | 40435    | 11.37             | 11.4                | 33                  |
| Throughput | 51              | 51       | 62.6              | 110.7               | 122                 |

## Key Files

* [`result.py`](result.py): The main entry point of the project, responsible for model loading, text generation, and evaluation of both throughput and perplexity.
* `19.csv`: Automatically generated after execution, containing the recorded perplexity and throughput metrics.

## References

* [ExLlamaV2](https://github.com/turboderp/exllamav2)
* [WikiText-2 Dataset](https://huggingface.co/datasets/wikitext)


