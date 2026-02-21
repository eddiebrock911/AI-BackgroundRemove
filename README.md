<div align="center">

# 🖼️ AI Background Remove

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/AI-Powered-blueviolet?style=for-the-badge&logo=openai&logoColor=white"/>
  <img src="https://img.shields.io/badge/rembg-U2Net-ff69b4?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Pillow-Image%20Processing-yellow?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/License-Apache%202.0-red?style=for-the-badge"/>
</p>

<p align="center">
  A lightweight yet powerful <strong>AI-based background removal tool</strong> built with Python.<br/>
  Automatically detects and removes backgrounds from images using deep learning — no manual masking required.
</p>

<p align="center">
  <a href="#-installation">Installation</a> •
  <a href="#-usage">Usage</a> •
  <a href="#-how-it-works">How It Works</a> •
  <a href="#-example-output">Example</a> •
  <a href="#-contributing">Contributing</a>
</p>

</div>

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [How It Works](#-how-it-works)
- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)
- [Installation](#-installation)
- [Usage](#-usage)
- [Example Output](#-example-output)
- [Use Cases](#-use-cases)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🧠 Overview

**AI-BackgroundRemove** is a Python script that leverages state-of-the-art deep learning to remove backgrounds from images in seconds. Powered by **U²-Net** (via the `rembg` library), it produces clean, high-quality transparent PNG outputs — perfect for e-commerce, design, and automation workflows.

> No Photoshop. No manual work. Just AI.

---

## ✨ Features

- 🤖 **AI-powered** — Uses U²-Net deep learning model for accurate segmentation
- 🖼️ **Transparent PNG output** — Removes background, exports with alpha channel
- ⚡ **Fast & lightweight** — Single Python script, minimal setup
- 📁 **Single image & batch processing** support
- 🎯 **High accuracy** — Works on people, products, animals, and objects
- 💻 **Cross-platform** — Windows, macOS, Linux

---

## ⚙️ How It Works

```
Input Image (JPG / PNG / WEBP)
          │
          ▼
┌─────────────────────────────┐
│        rembg Library        │
│                             │
│  ┌─────────────────────┐   │
│  │   U²-Net Model      │   │  ← Deep learning segmentation model
│  │  (Salient Object    │   │     trained on DUTS-TR dataset
│  │   Detection)        │   │
│  └──────────┬──────────┘   │
│             │               │
│  ┌──────────▼──────────┐   │
│  │  Alpha Matte        │   │  ← Generates pixel-level foreground mask
│  │  Generation         │   │
│  └──────────┬──────────┘   │
└─────────────┼───────────────┘
              │
              ▼
┌─────────────────────────────┐
│       Pillow (PIL)          │  ← Applies mask, saves transparent PNG
└─────────────────────────────┘
              │
              ▼
  Output Image (PNG with transparency)
```

### U²-Net Architecture

U²-Net uses a **nested U-structure** with two levels:
- **Outer U-Net** — Captures full-resolution spatial context
- **Inner U-Net** — Learns fine-grained local features inside each block

This dual-nested design achieves high accuracy on salient object detection without requiring a pre-trained backbone.

---

## 🗂️ Project Structure

```
AI-BackgroundRemove/
│
├── 🐍 remove.py           # Core script — loads image, removes bg, saves output
├── 📦 requirements.txt    # Python dependencies
├── 📄 LICENSE             # Apache 2.0
└── 📄 README.md           # You are here
```

---

## 🛠️ Tech Stack

| Library | Version | Purpose |
|---|---|---|
| `rembg` | latest | AI background removal via U²-Net |
| `Pillow` | ≥ 9.0 | Image loading, manipulation & saving |
| `onnxruntime` | ≥ 1.13 | ONNX model inference (used by rembg) |
| `numpy` | ≥ 1.21 | Array operations on pixel data |

---

## 📦 Installation

**1. Clone the repository:**
```bash
git clone https://github.com/eddiebrock911/AI-BackgroundRemove.git
cd AI-BackgroundRemove
```

**2. Create & activate a virtual environment:**
```bash
# Create
python -m venv venv

# Activate — Linux/Mac
source venv/bin/activate

# Activate — Windows
venv\Scripts\activate
```

**3. Install dependencies:**
```bash
pip install -r requirements.txt
```

> On first run, `rembg` will automatically download the **U²-Net model weights** (~170MB). Ensure you have an internet connection.

---

## ▶️ Usage

### Basic — Remove background from a single image

```bash
python remove.py
```

### In your own Python code

```python
from rembg import remove
from PIL import Image
import io

# Open input image
input_image = Image.open("input.jpg")

# Remove background
output_image = remove(input_image)

# Save as transparent PNG
output_image.save("output.png")

print("✅ Background removed successfully!")
```

### Batch Processing (multiple images)

```python
import os
from rembg import remove
from PIL import Image

input_folder  = "images/input/"
output_folder = "images/output/"
os.makedirs(output_folder, exist_ok=True)

for filename in os.listdir(input_folder):
    if filename.lower().endswith((".jpg", ".jpeg", ".png", ".webp")):
        img = Image.open(os.path.join(input_folder, filename))
        result = remove(img)
        result.save(os.path.join(output_folder, f"{os.path.splitext(filename)[0]}.png"))
        print(f"✅ Processed: {filename}")
```

---

## 📋 Example Output

| Input | Output |
|---|---|
| `input.jpg` (with background) | `output.png` (transparent background) |
| Person standing in a park | Person isolated on transparent PNG |
| Product on white table | Product with pure transparent background |
| Animal in nature | Animal cleanly segmented |

> **Supported input formats:** `.jpg`, `.jpeg`, `.png`, `.webp`, `.bmp`
> **Output format:** `.png` (with alpha transparency channel)

---

## 💡 Use Cases

| Domain | Application |
|---|---|
| 🛒 **E-commerce** | Product photo cleanup for listings |
| 🎨 **Graphic Design** | Extract subjects for compositing |
| 📸 **Photography** | Portrait background replacement |
| 🤖 **ML Pipelines** | Preprocessing images for training datasets |
| 🪪 **ID / Passport Photos** | Auto white background generation |
| 📱 **Social Media** | Profile picture background removal |

---

## 🐛 Troubleshooting

| Problem | Solution |
|---|---|
| `ModuleNotFoundError: rembg` | Run `pip install -r requirements.txt` |
| Model download fails | Check internet connection; rembg downloads model on first run |
| Output image is fully transparent | Ensure input image is not corrupted or too small |
| Slow processing | Install `onnxruntime-gpu` for CUDA GPU acceleration |
| `PIL` import error | Run `pip install Pillow` |

### Enable GPU Acceleration (Optional)

```bash
pip uninstall onnxruntime
pip install onnxruntime-gpu
```

> Requires CUDA-compatible NVIDIA GPU and CUDA toolkit installed.

---

## 🤝 Contributing

Contributions are welcome!

```bash
# 1. Fork the repo on GitHub

# 2. Clone your fork
git clone https://github.com/your-username/AI-BackgroundRemove.git

# 3. Create a feature branch
git checkout -b feature/your-feature-name

# 4. Make your changes & commit
git commit -m "feat: describe your change"

# 5. Push & open a Pull Request
git push origin feature/your-feature-name
```

**Ideas for contributions:**
- 🖥️ Add a Streamlit / Gradio web UI
- 🗃️ Add CLI argument support (`--input`, `--output`, `--batch`)
- 🎨 Custom background color/image replacement
- 📊 Add image quality metrics

---

## 📄 License

This project is licensed under the **Apache 2.0 License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

Made with ❤️ by [eddiebrock911](https://github.com/eddiebrock911)

⭐ **Star this repo** if it saved you time!

</div>
