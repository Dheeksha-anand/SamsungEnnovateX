# On-Device Fine-Tuning Framework for LLMs (Mobile)

![App Screenshot](b4ea168c-e0b1-47d5-8df4-2dd2254d383c.png)

## 📌 Overview
This project provides a framework for **on-device fine-tuning of billion+ parameter Large Language Models (LLMs)**.  
It is built around **TinyLLaMA (1.1B)** and optimized using **QLoRA adapters + llama.cpp** to enable **lightweight, private fine-tuning** directly on mobile devices (Android with Termux, portable to iOS).

---

## 🚀 Features
- **On-Device Fine-Tuning** – No internet required, fully private.  
- **Dynamic Personalization** – Fine-tune models with your own dataset.  
- **Efficient Compression** – QLoRA + Ridge Regression + SVD = tiny adapters.  
- **Mobile Ready** – Optimized with `llama.cpp` and `gguf`.  
- **Low Footprint** – Adapters are only a few MBs, no retraining full model.  
- **Cross-Platform** – Works in Termux (Android), adaptable to iOS.  
- **Reversible Updates** – Reset to base model anytime.  
- **User-Friendly** – Simple mobile interface for dataset input, fine-tune, and chat.  

---

## 🛠️ Installation Guide

### 1. Requirements
- **Android phone** (4GB+ RAM recommended)  
- **Termux** (from F-Droid)  
- **Model files**:
  - `base_model.gguf` (TinyLLaMA 1.1B)  
  - `adapter.gguf` (trained adapter, optional)  

### 2. Setup Environment
Open **Termux** and run:
```bash
pkg update && pkg upgrade -y
pkg install git wget cmake build-essential python -y

git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
make

cp /sdcard/Downloads/base_model.gguf ~/llama.cpp/
cp /sdcard/Downloads/adapter.gguf ~/llama.cpp/

./main -m base_model.gguf --lora adapter.gguf -p "Hello, how are you?"

