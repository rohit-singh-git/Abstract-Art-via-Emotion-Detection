# 🎙️ Abstract Art via Emotion Detection

A **Streamlit** web app that listens to your voice, detects your emotion using a fine-tuned **Wav2Vec2** model, and generates a matching piece of **abstract art** via a local **Stable Diffusion** pipeline — all running on your machine with no cloud API calls.

---

## ✨ Features

- 🎤 **Live microphone recording** or **WAV file upload**
- 🧠 **Emotion classification** across 8 states: `angry`, `calm`, `disgust`, `fearful`, `happy`, `neutral`, `sad`, `surprised`
- 📊 **Confidence score bar chart** for all detected emotions
- 💬 **Contextual text response** tailored to the detected emotion
- 🎨 **AI-generated abstract art** from Stable Diffusion, driven by the detected emotion
- ⚡ **CUDA-accelerated** inference (GPU recommended; CPU fallback available)
- 🔒 **Fully offline** — no external API required

---

## 🗂️ Project Structure

```
Abstract-Art-via-Emotion-Detection/
├── App/
│   ├── streamlit_app.py             # Main application
│   ├── requirements.txt             # Python dependencies
│   ├── recordings/                  # Saved microphone recordings
│   ├── results/                     # Generated artwork output
│   └── Model/
│        ├── Emotion Recognition/    # Wav2Vec2 model files
│        └── Image Generation/       # Stable Diffusion model files
├── python/
│   └── python.exe                   # Portable Python 3.12.4
├── pyproject.toml                   # UV project & dependency lock config
├── README.md                        # README file
└── uv.lock                          # Locked dependency versions
```

---

## ⚙️ Requirements

| Component | Requirement |
|-----------|-------------|
| Python | 3.12.4 (portable, included in `/python`) |
| Package Manager | [UV](https://github.com/astral-sh/uv) |
| GPU | CUDA 12.4 compatible (recommended) |
| Microphone | Required for live recording mode |

> **Note:** PyTorch is pinned to `cu124` (CUDA 12.4). CPU execution works but image generation will be significantly slower.

---

## 🚀 Getting Started

### 1. Install UV

If you don't have UV installed:

```bash
# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```

---

### 2. Clone the Repository

```bash
git clone https://github.com/rohit-singh-git/abstract-art-via-emotion-detection.git
cd abstract-art-via-emotion-detection/
```

---

### 3. Create the Virtual Environment

```bash
uv venv --python python/python.exe
.venv\Scripts\activate
```

This creates a `.venv` folder inside `abstract-art-via-emotion-detection/` using the bundled portable Python 3.12.4 and activates the virtual environment just created.

---

### 4. Sync Dependencies

UV will resolve and install all packages from the lockfile into the virtual environment:

```bash
uv sync
```

> This installs all dependencies exactly as specified in `uv.lock`, including PyTorch with CUDA 12.4 support.

---

### 4. Download the Models

Place the model files in the following directories before running:

```
App/Model/Emotion Recognition/    ← Wav2Vec2ForSequenceClassification fine-tuned model
App/Model/Image Generation/       ← Stable Diffusion pipeline (e.g. SD 1.5)
```

Both directories should contain the standard HuggingFace model format (`config.json`, `pytorch_model.bin` / `model.safetensors`, `tokenizer` files, etc.).

---

### 5. Run the App

```bash
uv run streamlit run App/streamlit_app.py
```

Then open your browser at **http://localhost:8501**

---

## 🎮 Usage

### Record from Microphone

1. Select **"Record Audio"** in the sidebar
2. Choose your **audio input device** from the dropdown
3. Set the **recording duration** (3–30 seconds)
4. Click **Start Recording** and speak naturally
5. View the detected emotion, confidence scores, and optional generated art

### Upload an Audio File

1. Select **"Upload Audio File"**
2. Upload a `.wav` file
3. The app will analyse it and display the results immediately

### Generate Abstract Art

After an emotion is detected, click the **🎨 Generate Art** button to produce a Stable Diffusion image inspired by your emotion. Images are saved automatically to `results/`.

---

## 🔧 Configuration

### Using the Portable Python Directly

```bash
# Windows
python\python.exe -m streamlit run App\streamlit_app.py

# macOS / Linux
python/python -m streamlit run App/streamlit_app.py
```

### Pointing UV to the Portable Python

```bash
uv sync --python python/python.exe
```

### Check Your CUDA Version

```bash
uv run python -c "import torch; print(torch.cuda.is_available(), torch.version.cuda)"
```

---

## 📦 Key Dependencies

| Package | Version | Purpose |
|--------|---------|---------|
| `streamlit` | 1.46.0 | Web UI framework |
| `transformers` | 4.52.4 | Wav2Vec2 emotion model |
| `diffusers` | 0.33.1 | Stable Diffusion image generation |
| `torch` | 2.6.0+cu124 | Deep learning backend |
| `torchaudio` | 2.6.0+cu124 | Audio processing |
| `sounddevice` | 0.5.2 | Microphone recording |
| `soundfile` | 0.13.1 | Audio file I/O |
| `scipy` | 1.15.3 | Audio resampling |

---

## 🛠️ Troubleshooting

**No audio input devices found**
> Check that your microphone is connected and not muted. On Windows, verify it's enabled in *Sound Settings → Input*.

**CUDA not available**
> Confirm your GPU drivers support CUDA 12.4 and that the `cu124` wheels were installed correctly. Re-run `uv sync` if needed.

**Model loading errors**
> Ensure the `Model/Emotion Recognition/` and `Model/Image Generation/` directories exist and contain valid HuggingFace model files.

**Slow image generation**
> Without a GPU, Stable Diffusion runs on CPU which can take several minutes. A CUDA-capable GPU is strongly recommended.

---

## 📁 Output Files

| Location | Contents |
|----------|----------|
| `recordings/` | `.wav` files from microphone sessions, timestamped |
| `results/` | Generated art images, named `{emotion}_{timestamp}_image.png` |

---

## 📄 License

This project is for educational and research use. Model weights may carry their own licenses — please review the terms of any HuggingFace models you use.