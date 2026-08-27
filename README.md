<h1 align="center">🚀 AI Flows HUB</h1>

<p align="center">
  <strong>GPU-powered AI utilities for YouTube automation.</strong><br>
  Voice generation • Speech-to-text • Image upscaling • Live progress
</p>

<p align="center">
  <a href="https://colab.research.google.com/"><img src="https://img.shields.io/badge/Google%20Colab-Ready-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white" alt="Google Colab"></a>
  <img src="https://img.shields.io/badge/GPU-T4%20Friendly-7C3AED?style=for-the-badge" alt="GPU">
  <img src="https://img.shields.io/badge/Tools-3-10B981?style=for-the-badge" alt="Three tools">
  <img src="https://img.shields.io/badge/Interface-FastAPI%20Web%20UI-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI">
</p>

<p align="center">
  <a href="#-what-is-ai-flows-hub">What is it?</a> •
  <a href="#-features">Features</a> •
  <a href="#-tools">Tools</a> •
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-chatterbox-turbo">Chatterbox</a> •
  <a href="#-architecture">Architecture</a> •
  <a href="#-youtube-automation-workflow">YouTube Workflow</a>
</p>

---

## 🧠 What is AI Flows HUB?

**AI Flows HUB** is a notebook-based AI workspace designed around practical **YouTube automation workflows**.

Instead of installing and running three separate AI projects manually, the notebook prepares the tools, creates their runners, and exposes them through one browser-based dashboard.

### The current stack

| Tool | Job | Main Output |
|---|---|---|
| 🎙️ **Chatterbox Turbo** | Text-to-Speech / voice overs | `.wav` |
| 📝 **Whisper large-v3-turbo** | Speech-to-Text with word timestamps | JSON / TXT / SRT |
| 🖼️ **Real-ESRGAN** | AI image upscaling | `.png` / ZIP batch |

The notebook is split into two major stages:

**1. Warm-up / setup** → cache models, prepare isolated runners, create the runtime.

**2. Web UI** → launch the browser dashboard, execute jobs, stream terminal activity, and send completed outputs to Google Drive.

---

# ✨ Features

## 🔥 Core Features

- ✅ **Three AI tools in one notebook**
- ✅ **Google Colab + Kaggle detection**
- ✅ **GPU-aware execution**
- ✅ **Model caching / warm-up**
- ✅ **Isolated tool environments**
- ✅ **Standalone runner scripts**
- ✅ **FastAPI web dashboard**
- ✅ **Drag & drop file uploads**
- ✅ **Live upload progress**
- ✅ **Live processing progress**
- ✅ **Live terminal activity**
- ✅ **WebSocket job events**
- ✅ **Per-tool job state**
- ✅ **Per-tool cancellation**
- ✅ **One active job per tool**
- ✅ **Optional persistent GPU workers**
- ✅ **Google Drive output storage**
- ✅ **Direct Google Drive download links**
- ✅ **Automatic Cloudflare HTTPS tunnel**
- ✅ **Light / dark dashboard theme**
- ✅ **Runtime health checks**
- ✅ **Automatic runner readiness status**

## 🎬 Why it is useful for YouTube automation

A typical AI-assisted video pipeline repeatedly needs:

> **Voice → Transcript / timestamps → Images → Upscaling → Video assembly**

AI Flows HUB currently covers three important pieces of that pipeline:

| YouTube task | AI Flows HUB |
|---|---|
| Generate narration | ✅ Chatterbox Turbo |
| Create multi-speaker dialogue | ✅ Chatterbox Turbo |
| Transcribe narration/interviews | ✅ Whisper |
| Generate word timestamps | ✅ Whisper |
| Generate subtitle timing files | ✅ Whisper SRT |
| Upscale generated/downloaded images | ✅ Real-ESRGAN |
| Process many images at once | ✅ Real-ESRGAN ZIP batch |
| Keep outputs in cloud storage | ✅ Google Drive |
| Use from a browser | ✅ FastAPI Web UI |

> 🛠️ **FFmpeg and advanced subtitle rendering are not implemented as active tools in the current notebook.** They can be added later as additional runners/workers.

---

# 🧰 Tools

## 🎙️ 1. Chatterbox Turbo

**Purpose:** generate AI voiceovers from text using a reference voice.

### Supported modes

| Mode | What it does | Input |
|---|---|---|
| 👤 **Single Speaker** | Generate a complete narration using one reference voice | `.txt` + reference audio |
| 👥 **Multi Speaker** | Generate dialogue using different reference voices | `.txt` + ZIP of speaker references |

### Single-speaker flow

```text
TXT Script
   +
Reference Voice
   ↓
Chatterbox Turbo
   ↓
Chunked generation
   ↓
Pause insertion
   ↓
WAV output
```

### Reference audio

Supported reference formats:

`WAV` • `MP3` • `M4A` • `FLAC` • `OGG`

The runner normalizes the reference audio before generation:

- resamples it to the model sample rate
- trims silence
- normalizes peak level
- limits the reference to 10 seconds
- pads short references to a minimum of about 5.1 seconds

### Smart script chunking

Long scripts are not sent as one giant generation request.

The runner:

1. splits text around sentence/newline boundaries
2. respects the configurable maximum character count
3. falls back to word-based splitting for oversized sentences
4. generates chunks sequentially
5. inserts configurable pauses between chunks
6. joins the generated audio into one WAV file

### Chatterbox controls

| Setting | Current UI range / default |
|---|---|
| 🌡️ Temperature | `0.1 → 2.0`, default `1.0` |
| ⏸️ Pause | `0 → 2.0s`, default `0.5s` |
| 🔤 Max Chars | `50 → 500`, default `200` |

### Multi-speaker script syntax

The runner accepts speaker tags such as:

```text
[speaker_1]: Welcome to today's story.

[speaker_2]: Thanks. I have been waiting for this.

[speaker_1]: Then let's begin.
```

It also supports single-letter speaker labels:

```text
A: Welcome to the show.
B: Happy to be here.
A: Let's get started.
```

### Speaker reference ZIP

For multi-speaker generation, provide a ZIP containing files such as:

```text
speaker_1.wav
speaker_2.wav
speaker_3.wav
```

The runner also recognizes the alternative naming form:

```text
spk1.wav
spk2.wav
spk3.wav
```

---

# 🎭 Chatterbox Turbo Emotion Tags

You can make generated narration much more expressive by placing supported tags directly inside your script.

## 💥 Emotion tags

Emotion tags affect the **overall delivery of the tagged text**.

| Tag | Best for | Example |
|---|---|---|
| `[dramatic]` | Breaking news, reveals, cliffhangers | `[dramatic] This changes everything!` |
| `[sarcastic]` | Irony, dry humor, disbelief | `[sarcastic] Oh sure, that went perfectly.` |
| `[happy]` | Celebrations, positive stories | `[happy] We just hit a million subscribers!` |
| `[surprised]` | Reactions, twists, disbelief | `[surprised] Wait, are you serious right now?` |
| `[angry]` | Outrage, frustration, intensity | `[angry] This is completely unacceptable!` |
| `[fear]` | Tension, warnings, alarm | `[fear] Something is very wrong here.` |
| `[crying]` | Sadness, sympathy, emotional moments | `[crying] I cannot believe they are gone.` |
| `[whispering]` | Secrets, asides, intimate delivery | `[whispering] Nobody is supposed to know this.` |
| `[narration]` | Documentary and neutral storytelling | `[narration] The year was nineteen sixty nine.` |
| `[advertisement]` | Promotional / sales-style delivery | `[advertisement] Try it free for thirty days!` |

## 🔊 Vocal sound effects

These tags inject a specific vocal sound.

| Tag | Effect |
|---|---|
| `[laugh]` | Full laugh |
| `[chuckle]` | Soft / brief laugh |
| `[gasp]` | Sharp intake of breath |
| `[sigh]` | Exhalation / weariness |
| `[groan]` | Displeasure / frustration |
| `[sniff]` | Nasal sound |
| `[cough]` | Cough |
| `[clear throat]` | Throat clearing |
| `[shush]` | Shushing sound |

## ✍️ Script-writing tips

### 1. Put emotion tags at the start of a sentence

```text
[dramatic] The verdict is in.
```

### 2. Sound effects can be placed inside a sentence

```text
Well [chuckle] that was unexpected.
```

### 3. Do not tag every line

Natural, untagged text can preserve the normal character of the reference voice.

### 4. Match the tag to the actual content

Good:

```text
[surprised] Wait... you actually did it?
```

Less useful:

```text
[surprised] The weather is twenty degrees today.
```

### 🎥 Great tags for YouTube / TikTok narration

| Use case | Useful tags |
|---|---|
| Hook / reveal | `[dramatic]` |
| Reaction | `[surprised]` |
| Commentary | `[sarcastic]` |
| Positive moment | `[happy]` |
| Factual narration | `[narration]` |
| Natural dialogue texture | `[laugh]`, `[chuckle]` |

### Example cinematic script

```text
[dramatic] At exactly midnight, everything changed.

[narration] The town had been quiet for decades.

[surprised] But then... the lights came back on.

[whispering] And nobody knew who turned them on.

[sigh] By morning, the entire story was gone.
```

> 💡 **Tip:** Treat emotion tags as storytelling tools, not decoration. A few well-placed tags usually make a script feel more intentional than tagging every sentence.

---

# 📝 2. Whisper — large-v3-turbo

**Purpose:** high-quality speech-to-text with **word-level timestamps**.

### Supported audio formats

`WAV` • `MP3` • `M4A` • `FLAC` • `OGG` • `AAC` • `WMA`

### Language options in the dashboard

- Auto-detect
- English
- Urdu
- Hindi
- Arabic
- Spanish
- French
- German
- Italian
- Portuguese
- Russian
- Chinese
- Japanese
- Korean
- Turkish

### Transcription behavior

The runner uses:

- `large-v3-turbo`
- transcription mode
- word timestamps
- previous-text conditioning
- zero temperature
- beam search
- `best_of=5`
- `beam_size=5`

### 📦 Six output formats

| Output | Purpose |
|---|---|
| `*_words.json` | Word-level timestamps as structured JSON |
| `*.json` | Segment-level timestamps as structured JSON |
| `*_segments.txt` | Human-readable segment timestamps |
| `*_words.txt` | Human-readable word timestamps |
| `*.srt` | Segment-based subtitles |
| `*_words.srt` | Word-timed SRT |

### Example word JSON

```json
[
  {
    "word": "Welcome",
    "start": 0.52,
    "end": 0.91
  },
  {
    "word": "to",
    "start": 0.92,
    "end": 1.04
  }
]
```

### Example segment JSON

```json
[
  {
    "id": 0,
    "start": 0.52,
    "end": 3.85,
    "text": "Welcome to the channel."
  }
]
```

### 🎬 Why this matters for Shorts

Word timestamps are useful for:

- animated captions
- karaoke-style subtitles
- word highlighting
- caption timing
- transcript synchronization
- short-form editing workflows
- future automated subtitle rendering

---

# 🖼️ 3. Real-ESRGAN

**Purpose:** upscale images for higher-resolution video assets and generated artwork.

### Supported image formats

`PNG` • `JPG` • `JPEG` • `WEBP` • `BMP` • `TIF` • `TIFF`

Also supports:

`ZIP` → batch processing

### Scale controls

| Scale | Supported |
|---:|:---:|
| `2×` | ✅ |
| `3.5×` | ✅ |
| `4×` | ✅ default |

The notebook caches:

- `RealESRGAN_x2plus.pth`
- `RealESRGAN_x4plus.pth`

### Single-image workflow

```text
Input Image
    ↓
RGB conversion
    ↓
Tiled Real-ESRGAN inference
    ↓
Optional target resize
    ↓
PNG output
```

### ZIP batch workflow

```text
ZIP
 ↓
Validate archive
 ↓
Extract images
 ↓
Upscale each image
 ↓
Preserve relative folder structure
 ↓
Create *_upscaled.png files
 ↓
Package results
 ↓
Google Drive
```

### Memory-friendly tiled inference

The runner processes images using:

- tile size: `512`
- tile padding: `32`
- no-grad inference
- GPU memory reporting during long jobs

This tiled design is especially useful for larger images where processing the full image at once would consume more VRAM.

---

# 🖥️ Web Dashboard

The notebook launches a browser-based dashboard called:

> **AI Flows Hub Dashboard**

Each tool gets its own control panel and live terminal activity area.

## Dashboard capabilities

| UI feature | Chatterbox | Whisper | Real-ESRGAN |
|---|:---:|:---:|:---:|
| Drag & drop | ✅ | ✅ | ✅ |
| File validation | ✅ | ✅ | ✅ |
| Upload progress | ✅ | ✅ | ✅ |
| Processing progress | ✅ | ✅ | ✅ |
| Live terminal logs | ✅ | ✅ | ✅ |
| Cancel job | ✅ | ✅ | ✅ |
| GPU warm mode | ✅ | ✅ | ✅ |
| Google Drive output | ✅ | ✅ | ✅ |
| Direct download | ✅ | ✅ | ✅ |
| Runtime status | ✅ | ✅ | ✅ |

## 🌗 Theme support

The dashboard includes:

- dark mode
- light mode
- responsive layout
- status badges
- progress bars
- tool navigation
- terminal-style activity panels
- success/error states

---

# 🔥 GPU Warm Mode

One of the most important design ideas in AI Flows HUB is **optional persistent GPU workers**.

Without warm mode:

```text
Job
 ↓
Start subprocess
 ↓
Load model
 ↓
Run inference
 ↓
Exit
```

With warm mode:

```text
Warm worker
 ↓
Load model once
 ↓
Keep model in GPU
 ↓
Receive jobs
 ↓
Run inference
 ↓
Keep worker alive
```

### Why it exists

Repeatedly loading large AI models can create unnecessary startup overhead.

Warm mode lets each tool maintain its own persistent model worker.

### Per-tool isolation

Each tool has its own warm state:

```text
Chatterbox worker
Whisper worker
Real-ESRGAN worker
```

A job is blocked from conflicting with its tool's warm worker state.

---

# 🧱 Isolated Runner Architecture

The notebook does not put every model into one giant shared Python process.

It creates standalone runners:

```text
run_tts.py
run_whisper.py
run_realesrgan.py
```

This gives the orchestration layer a cleaner boundary:

```text
Web UI
   ↓
FastAPI
   ↓
Job Engine
   ↓
Tool Runner / Warm Worker
   ↓
GPU Model
   ↓
Output File
   ↓
Google Drive
```

### Runtime environments

| Component | Runtime |
|---|---|
| Chatterbox | `chatterbox_env` |
| Whisper | `run_whisper.py` / Whisper runtime |
| Real-ESRGAN | `realesrgan_env` |
| Uploaded files | `three_tools_uploads` |
| Job files | `three_tools_jobs` |
| Cloudflare binary | `cloudflared` |

This separation helps avoid dependency conflicts between the different AI stacks.

---

# 🌐 Remote Browser Access

The dashboard automatically:

1. finds an available local port
2. starts FastAPI/Uvicorn
3. downloads `cloudflared` when needed
4. creates a Cloudflare Quick Tunnel
5. detects the generated `trycloudflare.com` HTTPS URL
6. prints the URL in the notebook

Conceptually:

```text
Your Browser
      │
      │ HTTPS
      ▼
Cloudflare Quick Tunnel
      │
      ▼
FastAPI on Colab/Kaggle
      │
      ▼
AI Flows HUB Job Engine
```

This makes the notebook feel much more like a small hosted application than a collection of notebook cells.

---

# ☁️ Google Drive Integration

AI Flows HUB uses Google Drive as the output delivery layer.

The dashboard creates / uses:

```text
MyDrive/
└── AI Flows Hub Downloads/
```

Completed files are uploaded to Drive and the browser receives a direct Drive download URL.

### Why Drive is useful here

Notebook runtimes are temporary.

Drive gives completed outputs a persistent storage location outside the notebook session.

### Output examples

```text
Voice generation
→ WAV

Whisper
→ Words JSON
→ Segment JSON
→ Segment TXT
→ Words TXT
→ Segment SRT
→ Word SRT

Real-ESRGAN
→ Upscaled PNG
→ Upscaled ZIP
```

---

# 📊 Job & Progress System

The dashboard has a lightweight job engine built into the notebook.

Each job tracks information such as:

- job ID
- tool
- status
- input file
- output paths
- processing events
- cancellation state
- warm-worker usage
- Google Drive download links

### Live communication

The browser connects to:

```text
/ws/{job_id}
```

and receives events while the worker is running.

This powers the live **Terminal Activity** panels and processing indicators.

### Job lifecycle

```text
QUEUED
  ↓
UPLOADING
  ↓
STARTING
  ↓
MODEL LOAD / WARM WORKER
  ↓
PROCESSING
  ↓
SAVING
  ↓
DRIVE UPLOAD
  ↓
COMPLETED
```

Errors and cancellation are also reflected in the dashboard.

---

# 🧩 Warm-up Cell

The warm-up cell prepares the environment before the web application is started.

## It prepares

### Whisper

- installs OpenAI Whisper
- installs FFmpeg
- downloads/caches `large-v3-turbo`
- releases the temporary warm-up model from GPU memory

### Real-ESRGAN

- detects Kaggle vs Colab
- checks existing dependencies
- caches x2 and x4 model weights
- creates a standalone runner
- avoids unnecessary NumPy/Pillow reinstalls
- leaves the warm-up model unloaded from GPU

### Chatterbox Turbo

- reuses the existing PyTorch/CUDA installation
- avoids reinstalling the system PyTorch stack
- installs the isolated TTS runtime packages
- installs the Chatterbox source
- installs the Perth dependency
- downloads `ResembleAI/chatterbox-turbo`
- applies compatibility patches used by the notebook
- creates `run_tts.py`

---

# ⚡ Quick Start

## 1️⃣ Open the notebook

Upload:

```text
AI_Flows_HUB.ipynb
```

to either:

- Google Colab
- Kaggle Notebook

A GPU runtime is strongly recommended.

---

## 2️⃣ Mount Google Drive

Run the first notebook cell.

It mounts Google Drive so completed outputs can be stored persistently.

---

## 3️⃣ Run the warm-up cell

Run:

```text
AI Flows HUB (Warm-UP Tools)
```

This prepares:

```text
Whisper
Real-ESRGAN
Chatterbox Turbo
```

You should see completion messages for all three.

---

## 4️⃣ Run the Web UI cell

Run:

```text
AI Flows HUB (WEB UI)
```

The notebook will start:

```text
FastAPI
+
WebSocket job events
+
Cloudflare HTTPS tunnel
+
Google Drive integration
```

Then look for:

```text
🌐 OPEN: https://....trycloudflare.com
```

Open that URL in your browser.

---

# 🎬 YouTube Automation Workflow

A practical workflow using the current tools can look like this:

```text
                ┌─────────────────────┐
                │     Your Script     │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │  Chatterbox Turbo   │
                │   Voice Generation  │
                └──────────┬──────────┘
                           │
                           ▼
                     Narration WAV
                           │
                           ├───────────────┐
                           │               │
                           ▼               ▼
                ┌────────────────┐  ┌────────────────┐
                │     Whisper    │  │   Image Assets │
                │ Word Timestamps│  │ / AI Images    │
                └───────┬────────┘  └───────┬────────┘
                        │                    │
                        ▼                    ▼
                  JSON / SRT         Real-ESRGAN
                        │                    │
                        │                    ▼
                        │              High-res PNGs
                        │                    │
                        └────────┬───────────┘
                                 ▼
                        Future Video Layer
                           (FFmpeg etc.)
```

### Current automation coverage

| Pipeline stage | Status |
|---|---|
| Script → voice | ✅ |
| Voice → word timestamps | ✅ |
| Word timestamps → SRT | ✅ |
| Image → high-res image | ✅ |
| Bulk image upscale | ✅ |
| Final video assembly | ⏳ Future |
| Advanced animated subtitle rendering | ⏳ Future |

---

# 📁 Files Created by the Notebook

The notebook prepares a runtime structure similar to:

```text
/kaggle/working/          # or /content on Colab
│
├── run_tts.py
├── run_whisper.py
├── run_realesrgan.py
│
├── chatterbox_env/
│   ├── hf_cache/
│   └── models/
│       └── chatterbox-turbo/
│
├── whisper_env/
│
├── realesrgan_env/
├── realesrgan_models/
│   ├── RealESRGAN_x2plus.pth
│   └── RealESRGAN_x4plus.pth
│
├── three_tools_uploads/
├── three_tools_jobs/
└── cloudflared
```

Exact runtime paths depend on whether the notebook is running on Kaggle or Colab.

---

# 🛡️ Safety & Practical Notes

### File validation

The UI validates extensions before starting jobs.

### ZIP validation

The orchestration layer validates uploaded ZIP archives and protects extraction paths against unsafe archive entries.

### Filename sanitization

Uploaded filenames are sanitized before being used by the server.

### One job per tool

The dashboard prevents duplicate simultaneous jobs for the same tool.

### Cancellation

Jobs can be cancelled from the UI.

### Runtime health

The dashboard periodically checks whether all three runner files are available.

---

# 🧠 Notebook Design Philosophy

AI Flows HUB intentionally follows a modular approach:

```text
Notebook
   │
   ├── Environment setup
   │
   ├── Model caching
   │
   ├── Standalone runners
   │
   ├── FastAPI orchestration
   │
   ├── WebSocket events
   │
   ├── Optional GPU workers
   │
   └── Drive output delivery
```

The goal is not to make one giant AI script.

The goal is to build a small **AI tool runtime** that can gradually grow into a complete YouTube automation stack.

---

# 🛣️ Roadmap Ideas

These are future expansion ideas, not active tools in the current notebook:

- ⏳ FFmpeg video processing runner
- ⏳ Automated subtitle rendering
- ⏳ Animated word-by-word captions
- ⏳ 9:16 Shorts composition
- ⏳ Automatic image/video sequencing
- ⏳ Background music layer
- ⏳ Audio ducking
- ⏳ Final MP4 export
- ⏳ End-to-end one-click YouTube video generation

The current repository already provides the foundation for these additions through the runner/job architecture.

---

# 🗂️ Feature Summary

| Category | Feature | Status |
|---|---|:---:|
| Runtime | Colab detection | ✅ |
| Runtime | Kaggle detection | ✅ |
| Runtime | GPU detection | ✅ |
| Runtime | Model caching | ✅ |
| Runtime | Isolated environments | ✅ |
| Runtime | Standalone runners | ✅ |
| Runtime | Optional warm GPU workers | ✅ |
| Web | FastAPI dashboard | ✅ |
| Web | Drag & drop | ✅ |
| Web | Upload progress | ✅ |
| Web | Processing progress | ✅ |
| Web | WebSockets | ✅ |
| Web | Terminal activity | ✅ |
| Web | Tool health checks | ✅ |
| Web | Job cancellation | ✅ |
| Web | Dark/light theme | ✅ |
| Network | Cloudflare HTTPS tunnel | ✅ |
| Storage | Google Drive integration | ✅ |
| TTS | Chatterbox Turbo | ✅ |
| TTS | Single speaker | ✅ |
| TTS | Multi speaker | ✅ |
| TTS | Voice reference normalization | ✅ |
| TTS | Smart text chunking | ✅ |
| TTS | Temperature control | ✅ |
| TTS | Pause control | ✅ |
| TTS | Max-character control | ✅ |
| STT | Whisper large-v3-turbo | ✅ |
| STT | Auto language detection | ✅ |
| STT | Word timestamps | ✅ |
| STT | Segment timestamps | ✅ |
| STT | JSON export | ✅ |
| STT | TXT export | ✅ |
| STT | Segment SRT | ✅ |
| STT | Word SRT | ✅ |
| Image | Real-ESRGAN x2 | ✅ |
| Image | Real-ESRGAN x3.5 | ✅ |
| Image | Real-ESRGAN x4 | ✅ |
| Image | Tiled inference | ✅ |
| Image | ZIP batch mode | ✅ |
| Video | FFmpeg tool | ⏳ |
| Video | Subtitle rendering engine | ⏳ |

---

# 📜 Source Projects

AI Flows HUB uses and integrates open-source projects / model sources including:

- [OpenAI Whisper](https://github.com/openai/whisper)
- [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN)
- [Chatterbox](https://github.com/resemble-ai/chatterbox)
- [Perth](https://github.com/resemble-ai/Perth)

The notebook's warm-up implementation downloads the relevant model/runtime components automatically.

---

# ❤️ Support the Project

If AI Flows HUB saves you time, helps you build your automation workflow, or teaches you something useful, consider supporting the project.

<p align="center">
  <a href="https://www.buymeacoffee.com/YOUR_USERNAME">
    <img src="https://img.shields.io/badge/☕%20Buy%20Me%20a%20Coffee-Support%20the%20Project-FFDD00?style=for-the-badge&logo=buymeacoffee&logoColor=000000" alt="Buy Me a Coffee">
  </a>
</p>

> 🔧 Replace `YOUR_USERNAME` in the link above with your actual Buy Me a Coffee username.

---

# ⭐ Contributing

Ideas, bug fixes, runner improvements, UI improvements, and new AI tools are welcome.

The architecture is intentionally modular so future tools can be added without rebuilding the entire dashboard.

---

# 📌 Repository

Recommended repository structure:

```text
AI-Flows-HUB/
│
├── AI_Flows_HUB.ipynb
├── README.md
└── ...
```

---

<p align="center">
  <strong>🎙️ Generate. 📝 Transcribe. 🖼️ Upscale. 🎬 Automate.</strong><br><br>
  Built as a practical foundation for AI-powered YouTube workflows.
</p>
