# Text → Audio & Image (Notebook)

Hi — I'm the author of this small experimental project. I built a Colab/Jupyter notebook that demonstrates how to combine a few generative models and utilities to turn text prompts into audio (speech and simple music), images, spectrograms and even a tiny GIF-style "video". This README explains what the notebook does, how to run it, and important notes I learned while working on it.

---

## What this project does (the short version)

The notebook `text_to_audioand_imgae.ipynb` stitches together several open-source model components so you can:

- Convert text to speech using pyttsx3 with simple "emotion" adjustments (rate & volume).
- Generate short music clips using MusicGen (Audiocraft).
- Create images from text prompts using Stable Diffusion (via diffusers).
- Produce a mel-spectrogram image from generated audio (librosa + matplotlib).
- Simulate a short video by composing a sequence of Stable Diffusion images into a GIF.
- Run a small conversational demo with GPT-2 (toy-size, for quick local experiments).
- Launch a Gradio interface so you can interact with the pipeline in the browser.

I wrote this to experiment and learn how these pieces can be combined in a single interactive demo.

---

## Project structure

- `text_to_audioand_imgae.ipynb` — the main Jupyter/Colab notebook. Everything is inside it (installation, model loading, helper functions, and a Gradio UI).
- (No packaged app — everything runs in the notebook.)

---

## Key dependencies (what I used)

I tested this in Colab and my local machine. The notebook installs many packages. The important ones are:

- Python 3.8+ (I used 3.11 in my environment)
- torch (with CUDA for GPU)
- diffusers
- transformers
- audiocraft (MusicGen)
- pyttsx3
- gradio
- librosa
- matplotlib
- soundfile (pysoundfile)
- scipy
- pillow (PIL)
- numpy

Note: Stable Diffusion and MusicGen download model weights (hundreds of MBs → multiple GBs depending on the model). Expect long downloads on first run.

---

## How to run (Colab)

1. Open the notebook in Colab:
   - Open `text_to_audioand_imgae.ipynb` and choose "Open in Colab".
2. Use a GPU runtime:
   - Runtime → Change runtime type → GPU.
3. Run cells top-to-bottom. The notebook includes pip install commands. The first model loads will take time.
4. The Gradio demo is set up and will auto-share in Colab (Gradio sets `share=True` in notebooks). Keep an eye on the cell output — it will provide a public link while the notebook is running.

---

## How to run (locally)

1. Create and activate a virtual environment:
   - python -m venv venv
   - source venv/bin/activate (macOS / Linux) or venv\Scripts\activate (Windows)
2. Install packages (example):
   - pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu118
   - pip install -r requirements.txt
   (There isn't a `requirements.txt` in this repo — see the first cell of the notebook for the install list.)
3. Run the notebook with Jupyter or jupyter lab, or convert selected cells into Python scripts.
4. For Stable Diffusion and MusicGen, a CUDA-capable GPU will greatly speed things up and is recommended.

---

## Tips, gotchas & notes (from me)

- pyttsx3 is used for TTS because it's simple and offline; on Colab it may behave unexpectedly (Colab servers don't always allow local audio playback). I save output to a temporary mp3 file in the notebook, but if you run locally you can get better voice control and playback.
- MusicGen and Stable Diffusion require significant disk and GPU resources. If you're on a free Colab runtime you might hit memory/timeout limits.
- GPT-2 is included as a toy conversation model. GPT-2 is small and can rant — it’s not a production chatbot.
- The notebook uses very simple "emotion detection" (keyword matching). It's just a demo of controlling TTS parameters.
- The Gradio share link created by Colab is temporary (72 hours). For a more permanent demo consider deploying to Hugging Face Spaces or running Gradio locally and exposing with a tunnel.
- Model versions and APIs change frequently. If something breaks, check the package changelogs for diffusers / audiocraft / transformers.

---

## Example usage (what I tried)

- Enter a short prompt like "sunset over a quiet beach, gentle waves" → choose "Music" to generate a short music file, then generate a spectrogram image.
- Enter "Tell me about my day" → choose "Conversation" to get a GPT-2 reply + an image about the conversation.
- Enter "An excited narrator telling a joke" → choose "Text to Audio" to save TTS with a slightly faster and louder voice to reflect "happy".

---

## Troubleshooting

- "Model download failing / OOM": try using a smaller model, or move to a machine with more RAM/GPU memory.
- pyttsx3 issues: pyttsx3 behaves differently across platforms. On Linux you may need additional audio packages. On headless servers, saving-to-file is more reliable than playback.
- Gradio not launching: ensure network is allowed, or run with `share=False` locally and open `localhost:7860`.

---

## Contributing

This project is an experimental notebook — if you want to contribute:
- Open an issue describing what you'd like to improve.
- If you want to add features (better emotion detection, larger conversational model, more robust TTS), fork and submit a PR with a modified notebook or a script.

---

## License & credits

- This repository is experimental. Use models and weights in accordance with their licenses (Hugging Face/diffusers, Audiocraft, etc.).

Acknowledgements:
- Hugging Face diffusers team — Stable Diffusion pipeline
- Meta/Audiocraft — MusicGen
- Hugging Face transformers — GPT-2 and tokenizer
- Gradio — quick demo UI

---

