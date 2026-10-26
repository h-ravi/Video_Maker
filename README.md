<div align="center">

# ⚡ imgAud_vid
### High-Performance Cinematic Ken Burns Video Generator
**Written Natively in BoseLang • Hardware Accelerated by NVIDIA NVENC**

[![Language](https://img.shields.io/badge/Language-BoseLang-FF6B35.svg?style=for-the-badge&logo=code)](https://github.com/h-ravi/BoseLang)
[![Platform](https://img.shields.io/badge/Platform-Windows%20Only-0078D6.svg?style=for-the-badge&logo=windows)](https://www.microsoft.com/windows)
[![Hardware](https://img.shields.io/badge/GPU-NVIDIA%20NVENC-76B900.svg?style=for-the-badge&logo=nvidia)](https://developer.nvidia.com/video-encode-decode-gpu-support-matrix)
[![Engine](https://img.shields.io/badge/Engine-FFmpeg%20%2B%20BoseLang-007808.svg?style=for-the-badge&logo=ffmpeg)](https://ffmpeg.org/)
[![Resolution](https://img.shields.io/badge/Resolution-1080p%20%7C%204K%20UHD-00B4D8.svg?style=for-the-badge)](https://en.wikipedia.org/wiki/Display_resolution)
[![License](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)

<br/>

<p align="center">
  <strong>Transform any single static image and audio recording into a continuous, broadcast-grade cinematic motion picture in seconds.</strong><br>
  <em>Powered by cubic smoothstep easing, 3x super-resolution anti-jitter, and pure GPU encoding.</em>
</p>

</div>

---

> [!IMPORTANT]
> **Operating System Requirement: Windows Only (Windows 10 / 11)**  
> This engine currently uses Windows PowerShell asynchronous process management for its high-speed parallel multi-worker rendering pipeline. *(Linux / macOS cross-platform support will be introduced in a future update).*

---

## 📖 Overview

Making engaging video content for YouTube documentaries, audiobooks, podcasts, historical narrations (e.g., Vikram-Betaal, mythological lore), or educational reels typically requires hours of manual keyframing in heavy video editors like Premiere Pro or DaVinci Resolve.

**`imgAud_vid`** automates this entire production pipeline from the terminal. Given a single picture and an audio file, it:
1. Analyzes audio duration with sub-millisecond precision.
2. Synthesizes a **non-repeating, continuous camera flight path** with natural pans and zooms.
3. Applies **Cubic Smoothstep Easing** to ensure organic, fluid camera dynamics without robotic starts or stops.
4. Elevates production value with subtle **vignetting, dynamic 35mm film grain**, and synchronized audio-video fade transitions.
5. Renders the final output at **blazing speed** using **NVIDIA NVENC hardware acceleration** (up to 10x faster than CPU encoding).

---

## 🌟 Why imgAud_vid Beats Traditional Python Scripts

| Capability | Standard Python Script | `imgAud_vid` (BoseLang Turbo) |
| :--- | :--- | :--- |
| **Execution Architecture** | Single-threaded sequential (1 segment at a time) | ⚡ **Parallel Multi-Worker Engine** (4x concurrent FFmpeg threads utilizing all CPU cores) |
| **Hardware Engine** | Slow CPU software (`libx264` preset `medium`) | 🚀 **NVIDIA NVENC GPU Acceleration** (`h264_nvenc` preset `p2`) — **Up to 10x faster** |
| **Canvas Optimization** | 5K ($5760 \times 3240$) 18M pixels CPU choke | 🔍 **4K UHD Canvas ($3840 \times 2160$)** — 55% less pixel load with 100% jitter-free smoothness |
| **Camera Continuity** | Prone to abrupt cuts or jumping between segments | 🎯 **Seamless Coordinate Handoff** — Zero jump cuts, strictly continuous movement |
| **Workspace Hygiene** | Leaves orphaned segment files on crash or exit | 🧹 **Auto-Sweeper Workspace** — Cleans temporary files immediately upon completion |
| **CLI Ergonomics** | Hardcoded file paths in script body | 💻 **Universal Flexible CLI** — Pass arguments directly via terminal or configure via file |
| **Code Footprint** | Bloated third-party dependencies | 💎 **Pure Native BoseLang** — Clean syntax, 1-based human indexing, zero boilerplate |

---

## 📁 Repository Structure

```text
imgAud_vid/
├── config.bose             # Global user configuration (resolution, GPU, camera parameters, VFX)
├── main.bose               # Core production engine (timeline, easing math, NVENC pipeline)
├── sample/                 # Out-of-the-box working test assets
│   ├── sample_image.png    # 1080p sample test graphic
│   └── sample_audio.mp3    # 5-second sample audio tone
├── output/                 # Destination folder for rendered videos
│   └── cinematic_video.mp4
└── README.md               # Complete architecture & documentation
```

---

## 🚀 Quick Start Guide

> 💡 **Prerequisites:** This project requires `bose.exe` ([Download from Releases](https://github.com/h-ravi/BoseLang/releases)) and `ffmpeg`. If you don't have them yet, see the [3-Step Installation Guide](#%EF%B8%8F-step-by-step-setup--installation-guide) below.

### 1. Zero-Config Sample Run
Test the entire pipeline immediately using the pre-bundled sample assets:

```bash
bose imgAud_vid/main.bose
```
*Or from inside the directory:*
```bash
cd imgAud_vid
bose main.bose
```

### 2. Universal Command-Line Invocation (CLI)
Render your own files instantly without modifying any configuration:

```bash
bose imgAud_vid/main.bose "path/to/character.png" "path/to/narration.mp3" "output/final_video.mp4"
```

### 3. Custom Project Setup via `config.bose`
Open [`config.bose`](config.bose) to fine-tune resolution, camera parameters, and visual effects:

```bose
/* File Paths */
fix IMAGE_PATH = "sample/sample_image.png"
fix AUDIO_PATH = "sample/sample_audio.mp3"
fix OUTPUT_PATH = "output/cinematic_video.mp4"

/* Canvas & Framerate */
fix WIDTH = 1920
fix HEIGHT = 1080
fix FPS = 30

/* GPU Hardware Acceleration */
fix USE_GPU = true              /* Set false to fallback to CPU encoding */
fix GPU_ENCODER = "h264_nvenc"
fix GPU_PRESET = "p4"          /* Balanced speed vs quality (p1 = fastest, p7 = best) */

/* Camera Motion Limits */
fix ZOOM_MIN = 1.03             /* 1.0 = original frame; 1.03 = subtle initial zoom */
fix ZOOM_MAX = 1.14             /* Maximum deep zoom */
fix PAN_MIN = 0.15              /* Boundary safety margins (prevents border exposure) */
fix PAN_MAX = 0.85
fix MIN_SEGMENT_SECONDS = 10     /* Duration of each continuous camera move */
fix MAX_SEGMENT_SECONDS = 20

/* Cinematic Visual Effects */
fix ENABLE_VIGNETTE = true      /* Adds subtle darkened lens edges */
fix ENABLE_GRAIN = true         /* Adds subtle organic 35mm film grain */
fix GRAIN_STRENGTH = 5          /* Range: 1 - 20 (subtle) */
fix FADE_IN_SECONDS = 2.0       /* Intro video fade-in */
fix FADE_OUT_SECONDS = 3.0      /* Outro synchronized audio & video fade-out */
```

---

## 🧠 Mathematical Architecture: The Ken Burns Engine

### 1. Seamless Coordinate Handoff (Zero Jump Cuts)
A primary flaw in amateur video generators is that each camera segment picks random start and end points, causing jarring visual "snaps" or jumps between cuts. 

`imgAud_vid` guarantees **$C^0$ positional continuity**:
$$\mathbf{P}_{\text{start}}^{(k+1)} = \mathbf{P}_{\text{end}}^{(k)}$$
The initial camera position for segment $k+1$ is strictly bound to the final position of segment $k$. The image never jumps.

### 2. Cubic Smoothstep Easing Function
Linear camera movement feels robotic and unnatural. To simulate a human camera operator or physical crane, `imgAud_vid` computes a normalized cubic Hermite interpolation (Smoothstep):

$$t = \frac{\text{frame}}{N - 1}, \quad \text{ease}(t) = t^2 \times (3 - 2t)$$

$$\mathbf{Zoom}(t) = z_0 + (z_1 - z_0) \times \text{ease}(t)$$
$$\mathbf{PanX}(t) = x_0 + (x_1 - x_0) \times \text{ease}(t)$$
$$\mathbf{PanY}(t) = y_0 + (y_1 - y_0) \times \text{ease}(t)$$

* **Derivative Behavior:** $\frac{d}{dt}\text{ease}(0) = 0$ and $\frac{d}{dt}\text{ease}(1) = 0$, guaranteeing zero velocity at arrival and departure.

### 3. Anti-Jitter Super-Resolution Canvas
FFmpeg's internal `zoompan` filter calculates coordinates using integer and subpixel approximations. When applied directly to a $1920 \times 1080$ frame, slow pans exhibit noticeable micro-stutter (pixel jitter).

`imgAud_vid` eliminates this by first applying a **Lanczos 3x upscale** to a massive $5760 \times 3240$ base canvas:
```text
scale=5760:3240:force_original_aspect_ratio=increase:flags=lanczos,crop=5760:3240
```
Coordinates are calculated on the high-density grid and downsampled during render, creating **glass-smooth motion**.

---

## ⚙️ Configuration Reference Table

| Variable | Type | Default | Description |
| :--- | :---: | :---: | :--- |
| `IMAGE_PATH` | `text` | `"sample/sample_image.png"` | Path to source graphic (`.png`, `.jpg`, `.webp`) |
| `AUDIO_PATH` | `text` | `"sample/sample_audio.mp3"` | Path to source audio (`.mp3`, `.wav`, `.aac`, `.m4a`) |
| `OUTPUT_PATH` | `text` | `"output/cinematic_video.mp4"`| Final MP4 output path |
| `WIDTH` | `int` | `1920` | Output video width in pixels |
| `HEIGHT` | `int` | `1080` | Output video height in pixels (Set `1920` for 9:16 Shorts) |
| `FPS` | `int` | `30` | Framerate (30 standard, 60 ultra-smooth) |
| `CONCURRENT_WORKERS`| `int` | `4` | Number of segments rendered simultaneously in parallel (2, 4, 6, 8) |
| `USE_GPU` | `bool` | `true` | `true` for NVIDIA NVENC; `false` for CPU fallback |
| `GPU_ENCODER` | `text` | `"h264_nvenc"` | Hardware encoder name |
| `GPU_PRESET` | `text` | `"p2"` | NVENC preset (`p1/p2` fastest $\rightarrow$ `p7` highest quality) |
| `CPU_ENCODER` | `text` | `"libx264"` | Software encoder fallback |
| `CPU_PRESET` | `text` | `"fast"` | x264 software preset (`ultrafast`, `fast`, `medium`, `slow`) |
| `VIDEO_CRF` | `int` | `18` | Constant Rate Factor for CPU fallback (lower = better quality) |
| `ZOOM_MIN` | `point` | `1.03` | Minimum zoom ratio ($1.0 = 100\%$) |
| `ZOOM_MAX` | `point` | `1.14` | Maximum zoom ratio ($1.14 = 114\%$) |
| `PAN_MIN` | `point` | `0.15` | Minimum normalized pan boundary margin |
| `PAN_MAX` | `point` | `0.85` | Maximum normalized pan boundary margin |
| `MIN_SEGMENT_SECONDS`| `int` | `10` | Shortest camera flight duration |
| `MAX_SEGMENT_SECONDS`| `int` | `20` | Longest camera flight duration |
| `ENABLE_VIGNETTE` | `bool` | `true` | Enables cinematic edge darkening |
| `ENABLE_GRAIN` | `bool` | `false` | Film grain simulation (Set `false` for 3x faster rendering) |
| `GRAIN_STRENGTH` | `int` | `5` | Film grain opacity/intensity ($0 - 20$) |
| `FADE_IN_SECONDS` | `point` | `2.0` | Video intro fade-in duration |
| `FADE_OUT_SECONDS`| `point` | `3.0` | Synchronized audio/video outro fade duration |
| `BASE_WIDTH` | `int` | `3840` | Anti-jitter canvas width (4K UHD) |
| `BASE_HEIGHT` | `int` | `2160` | Anti-jitter canvas height (4K UHD) |

---

## 🛠️ Step-by-Step Setup & Installation Guide

> 🪟 **Supported OS:** Windows 10 / Windows 11 (64-bit) with PowerShell.

If you are running this project for the first time, follow these 3 simple steps:

### Step 1: Get the BoseLang Runtime (`bose.exe`)

`imgAud_vid` is written in **BoseLang**. You need the standalone `bose.exe` executable to run it.

* **Download Pre-compiled Binary:**
  Download the latest standalone executable from the official repository:
  👉 **[Download bose.exe (GitHub Releases)](https://github.com/h-ravi/BoseLang/releases)**  
  *(Source code available at: [github.com/h-ravi/BoseLang](https://github.com/h-ravi/BoseLang))*.

* **Two Easy Ways to Use It:**
  * **Option A: Portable Mode (Zero-Setup - Simplest):**
    Copy `bose.exe` directly into this `imgAud_vid/` folder. Now you can run it immediately without modifying your system settings:
    ```powershell
    .\bose.exe main.bose
    ```
  * **Option B: Global System PATH Mode (Recommended):**
    Place `bose.exe` in a directory of your choice (e.g. `C:\Program Files\BoseLang` or `D:\Tools`) and add that folder to your Windows System Environment Variables `PATH`. Now you can run `bose` from any terminal anywhere:
    ```bash
    bose --version
    ```

---

### Step 2: Install FFmpeg & FFprobe

`imgAud_vid` drives FFmpeg for hardware-accelerated video rendering and audio muxing.

* **Windows (via Winget - Recommended 1-liner):**
  Open terminal and run:
  ```powershell
  winget install Gyan.FFmpeg
  ```
  *(Restart your terminal after installation completes)*.

* **Manual Download:**
  Download the complete build from [gyan.dev/ffmpeg/builds](https://www.gyan.dev/ffmpeg/builds/) (`ffmpeg-release-full.7z`), extract, and add the `bin/` folder to your system `PATH`.

* **Verify Installation:**
  ```bash
  ffmpeg -version
  ffprobe -version
  ```

---

### Step 3: GPU Hardware Check (Optional but Recommended)

* **NVIDIA GPU Users:**
  If you have an NVIDIA GeForce GTX/RTX GPU, hardware acceleration is enabled by default (`h264_nvenc`) for up to **10x faster rendering**.
* **CPU / AMD / Intel Users:**
  If you do not have an NVIDIA GPU, simply open [`config.bose`](config.bose) and set:
  ```bose
  fix USE_GPU = false
  ```
  The engine will automatically route through high-quality multi-threaded CPU software encoding (`libx264`).

---

## 🚀 How to Run the Generator

Once you have completed the setup above:

### Method 1: Instant Test Run with Sample Assets
Verify that everything is operational out-of-the-box:

```bash
# If bose is on PATH:
bose main.bose

# If bose.exe is in the project folder:
.\bose.exe main.bose
```

### Method 2: Render Custom Files via Command Line (CLI)
Pass your own image, audio, and output destination directly:

```bash
# Syntax: bose main.bose <IMAGE_PATH> <AUDIO_PATH> [OUTPUT_PATH]
bose main.bose "my_character.png" "my_story.mp3" "output/cinematic.mp4"

# Portable mode:
.\bose.exe main.bose "my_character.png" "my_story.mp3" "output/cinematic.mp4"
```

### Method 3: Configure via `config.bose`
Place your files in the folder and configure [`config.bose`](config.bose) once. Then simply run `bose main.bose` whenever you want to render!

---

## ❓ Frequently Asked Questions (FAQ)

#### Q: How can I generate vertical 9:16 videos for YouTube Shorts or Instagram Reels?
**A:** Simply change the dimensions in [`config.bose`](config.bose):
```bose
fix WIDTH = 1080
fix HEIGHT = 1920
```
The Lanczos super-sampling engine automatically adjusts the crop matrix to match vertical framing.

#### Q: Can I run this without an NVIDIA GPU?
**A:** Yes! Open `config.bose` and set `fix USE_GPU = false`. The engine will seamlessly route encoding through high-quality `libx264` multi-threaded CPU software encoding.

#### Q: Does it support WAV or AAC audio?
**A:** Yes. Any format supported by FFmpeg (`.mp3`, `.wav`, `.aac`, `.m4a`, `.ogg`, `.flac`) is automatically decoded and muxed with standard high-bitrate AAC audio in the final MP4.

---

## 📜 License

Distributed under the **MIT License**. Free for personal and commercial content creation.

Dedicated to the enduring spirit of **Netaji Subhash Chandra Bose** 🇮🇳.
