# 📱 On-Device Finetuning Framework (Mobile)

## 🚀 Overview
This project demonstrates a **mobile-first framework** for **on-device finetuning of billion+ parameter LLMs** with **private data**.  
We use **TinyLlama-1.1B** with the **Stanford Alpaca dataset** as a baseline example, apply **QLoRA adapters**, and run **efficient ridge regression + SVD compression** directly on an **Android device** using **Termux** and **llama.cpp**.  

The result:  
✅ Private, personalized LLMs  
✅ No cloud dependency  
✅ Lightweight finetuning workflows optimized for mobile hardware  

---

## 🧩 Framework Logic

1. **Base Model & Adapter Training (Laptop/Server)**  
   - Train **QLoRA adapter** on the **Stanford Alpaca dataset**.  
   - Convert both **base model** and **adapter** to **GGUF** using `llama.cpp` tools.  

2. **Mobile Setup (Termux on Android)**  
   - Clone `llama.cpp` repo on device.  
   - Transfer converted **base + adapter GGUF files** to device storage.  

3. **Dynamic Finetuning Pipeline**  
   - Run `llama-embedding` script on device.  
   - Input private user data in JSON format:  
     ```json
     {"instruction": "...", "input": "...", "output": "..."}
     ```
   - Generate embeddings **batchwise** for:  
     - **Teacher model** = base + SA adapter  
     - **Student model** = base only  
   - Save outputs into `.npz` files.  

4. **Efficient Adaptation (Mobile-Friendly)**  
   - Load `.npz` embeddings.  
   - Run **ridge regression** and **SVD** to compute low-rank deltas.  
   - Store intermediate weights in `.npy` files.  

5. **Adapter Conversion**  
   - Map affecting layers → convert `.npy` → `.safetensor`.  
   - Use `convert_lora_to_gguf.py` to obtain a **final .gguf adapter**.  

6. **Testing & Deployment**  
   - Load the personalized adapter in `llama.cpp`.  
   - Run local inference for **private, finetuned responses**.  

---

## 📲 Mobile Application Vision
This framework will be wrapped into a **mobile app** interface that provides:  
- Upload private datasets  
- Configure finetuning (batch size, α, rank)  
- Monitor progress (logs, metrics)  
- Test finetuned model with custom prompts  
- Save / manage multiple adapters  

---

## ⚙️ Technical Stack
- **Model**: TinyLlama-1.1B  
- **Dataset**: Stanford Alpaca (example)  
- **Adapter Training**: QLoRA  
- **Inference Backend**: [`llama.cpp`](https://github.com/ggerganov/llama.cpp)  
- **Embeddings & Compression**: Ridge Regression + SVD  
- **Formats**: GGUF, Safetensor, NumPy  
- **Platform**: Android (Termux)  

---

## 📂 File Workflow

| Step | Input | Output |
|------|-------|--------|
| Train Adapter | Alpaca dataset | QLoRA adapter |
| Convert | Base + Adapter | `.gguf` |
| Embedding | JSON (user data) | `.npz` |
| Regression + SVD | `.npz` | `.npy` |
| Convert | `.npy` | `.safetensor` |
| Final Conversion | `.safetensor` | `.gguf` |
| Test | `.gguf` | Personalized model |

---

## 🛠️ Usage Workflow

### 1. Setup (on Android via Termux)
```bash
pkg install git python clang cmake
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
