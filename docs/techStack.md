# Technical Stack

We build entirely on **open-source software**:

- [TinyLlama-1.1B](https://huggingface.co/TinyLlama) – base model.
- [Stanford Alpaca dataset](https://github.com/tatsu-lab/stanford_alpaca) – instruction dataset.
- [QLoRA](https://arxiv.org/abs/2305.14314) – efficient low-rank adaptation.
- [llama.cpp](https://github.com/ggerganov/llama.cpp) – efficient inference + conversion to `gguf`.
- `convert_lora_to_gguf.py` – adapter → gguf tool (from llama.cpp repo).
- `ridge regression + SVD` (NumPy, SciPy).
- [safetensors](https://github.com/huggingface/safetensors) – safe tensor format.
- [Termux](https://termux.dev/) – Linux environment on Android.
- [Python](https://www.python.org/) – glue scripts for embeddings + conversions.
- [Android](https://developer.android.com/) – final app environment (Kotlin/React Native/Flutter optional for UI).

