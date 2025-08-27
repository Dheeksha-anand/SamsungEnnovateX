# Approach

## Problem We Solve
On-device fine-tuning of **billion+ parameter LLMs** using **private user data**, without requiring cloud resources or exposing sensitive information.

## Our Unique Contribution
- **First-of-its-kind lightweight framework** that performs **dynamic fine-tuning** fully on mobile devices.
- Uses **TinyLlama-1.1B** as the base model, fine-tuned with **QLoRA adapters** and converted to `gguf` for efficient mobile inference via `llama.cpp`.
- Introduced a **student-teacher embedding regression** pipeline (ridge regression + SVD) to compress adapter knowledge into small safetensors → `gguf`.
- End result: **personalized model on-device**, no internet needed, with **reasonable latency and storage footprint**.

## High-Level Flow
1. **Base model + QLoRA adapter** trained on dataset (e.g., Stanford Alpaca).
2. Convert both into `gguf` with `llama.cpp`.
3. Run **embedding extraction** on-device:  
   - Teacher = (base + adapter)  
   - Student = (base only)  
   - Save `.npz` embedding pairs.
4. Run **ridge regression + SVD** to map teacher knowledge into compact weights.
5. Export → `.npy` → `.safetensor` → `gguf`.
6. Final fine-tuned model runs **locally on mobile** with user’s private data.

![Pipeline Diagram](pipeline.png)
