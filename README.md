# GlassData LLM Engineering — Simplified Demonstrations

This package contains **two Google Colab notebooks** that demonstrate the four assigned topics with short, readable Python code.

## Files

```text
GlassData-LLM-Demo/
├── README.md
├── requirements.txt
└── notebooks/
    ├── 01_lora_qlora_demo.ipynb
    └── 02_speech_gateway_guardrails_demo.ipynb
```

## 1. Open the notebooks in Google Colab

Upload the `.ipynb` files to Google Drive, right-click each file and choose **Open with → Google Colaboratory**. Alternatively, upload them directly at https://colab.research.google.com/.

Run each notebook from the first cell to the last cell.

## 2. Notebook 1 — LoRA and QLoRA

This notebook automatically chooses a mode:

- **NVIDIA GPU + bitsandbytes available:** Qwen2.5-0.5B-Instruct is loaded in 4-bit NF4 and trained with LoRA adapters (QLoRA).
- **CPU runtime or 4-bit setup unavailable:** the notebook switches to a small DistilGPT-2 model and performs ordinary LoRA training. It does not stop with a GPU error.

### To run actual QLoRA

In Colab, select:

`Runtime → Change runtime type → T4 GPU`

Then run **Runtime → Run all**. The notebook detects CUDA and installs `bitsandbytes` only when needed. It does not reinstall PyTorch or pin old Transformers/Hugging Face versions.

If Colab does not allocate a GPU, the notebook still completes the LoRA fallback. Its final cell prints whether the run used `QLoRA` or `LoRA fallback`.

## 3. Notebook 2 — Speech, Gateway and Guardrails

This notebook runs on either CPU or GPU. It demonstrates:

1. Whisper Tiny for speech-to-text (ASR).
2. Qwen2.5-0.5B-Instruct for text generation.
3. gTTS for text-to-speech (TTS).
4. A small Python gateway with a fast route, detailed route, authentication check, rate limit, fallback and request log.
5. Guardrails for tool allowlisting, permissions, output validation and tenant-aware document access.

It first attempts to create a short sample audio file using gTTS. If that fails, it asks for a WAV or MP3 upload. Internet access is required to download the Hugging Face models and use online gTTS.

## 4. Requirements

Google Colab normally includes PyTorch. The notebooks install only missing packages and do not force a PyTorch upgrade.

For a local Python environment, install PyTorch appropriate for your machine first, then run:

```bash
pip install -r requirements.txt
```

`bitsandbytes` is intentionally not in the common requirements file. Notebook 1 installs it only when a CUDA GPU is detected. It is not required for the CPU LoRA fallback.

## 5. Important technical distinctions

- LoRA trains small adapter weights while keeping the base model frozen.
- QLoRA additionally loads the frozen base model in 4-bit quantization. Genuine bitsandbytes QLoRA requires a compatible NVIDIA GPU.
- The speech demonstration is a cascaded `ASR → text LLM → TTS` pipeline, not a natively trained speech LLM.
- The gateway is a teaching implementation inside the notebook, not a deployed API gateway or LiteLLM proxy.
- The guardrails demonstrate application-side checks. They are not a complete prompt-injection detector, moderation system or formal security guarantee.

## 6. Before sharing the assignment

- Run both notebooks from top to bottom.
- In Notebook 1, check the final output and state whether it used QLoRA or the LoRA fallback.
- In Notebook 2, confirm that transcription, text response and audio output are generated.
- Do not claim that QLoRA ran if the notebook reports `LoRA fallback`.
- Keep API keys and private credentials out of the notebooks.

## References

- [Hugging Face PEFT](https://huggingface.co/docs/peft)
- [Qwen2.5-0.5B-Instruct](https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct)
- [Whisper Tiny](https://huggingface.co/openai/whisper-tiny)
- [LiteLLM documentation](https://docs.litellm.ai/)
- [OWASP GenAI Security Project](https://genai.owasp.org/)
