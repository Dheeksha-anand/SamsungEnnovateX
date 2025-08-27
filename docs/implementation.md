# Implementation Details

## Training
- Base model: TinyLlama-1.1B.
- Adapter: Trained with QLoRA on Stanford Alpaca dataset.
- Saved in HuggingFace format.

## Conversion Steps
1. Convert base + adapter → `gguf` using llama.cpp.
2. Run `llama-embedding` on dataset:
   - Teacher (base + adapter).
   - Student (base).
   - Save `.npz` embeddings.
3. Perform ridge regression + SVD:
   - Map teacher embeddings → student embeddings.
   - Save learned weights (`.npy`).
4. Insert weights into **affecting layers**.
5. Save as `.safetensor`.
6. Convert with `convert_lora_to_gguf.py` → `.gguf`.

## Scripts Added to llama.cpp
- `scripts/run_embeddings.py` – extract embeddings batchwise.
- `scripts/ridge_svd.py` – regression + SVD pipeline.
- `scripts/convert_to_safetensor.py` – npy → safetensor.
- Extended `convert_lora_to_gguf.py`.

## Testing
- Load final gguf model in Termux.
- Run prompt → check personalized responses.

