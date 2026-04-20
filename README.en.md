# VoiceTray

**[正體中文](README.md)** | **[简体中文](README.zh-CN.md)** | **[English](README.en.md)**

> Voice recognition input tool — Runs smoothly on CPU alone, no GPU required. Fully local, no cloud needed (ideal for enterprises or classified environments), low-latency streaming ASR + SenseVoice text correction

## Project Status

✅ **Released v0.4.2** — Live Caption Mode adds mid-segment SenseVoice dual-color real-time recognition (experimental), speech-rate adaptive segmentation, and multiple stability fixes.

## Why Use This Tool

Compared to other voice input tools on the market, VoiceTray solves three core pain points:

### 1. Fully Local, Zero Privacy Concerns, No GPU Required
All speech recognition runs on your machine — audio is never uploaded to any server. It uses a dual-engine architecture combining streaming Zipformer for real-time recognition with SenseVoice for offline correction. **Runs smoothly on CPU alone — no dedicated GPU or CUDA required.** No internet connection or API keys required; just download the models and use it offline.

### 2. Streaming + Offline Dual-Engine Collaboration, Zero-Delay Feedback
An innovative streaming-offline collaboration architecture:
- **Streaming engine** (Zipformer): Starts recognizing the moment you press the hotkey — see results as you speak
- **Offline engine** (SenseVoice): After releasing the hotkey, performs high-accuracy recognition on the full audio and automatically replaces the streaming results
- The streaming engine handles real-time feedback while the offline engine ensures final quality — seamless collaboration

### 3. Single-Key CTRL, Smart Intent Detection
Just hold the Ctrl key to start recording. The app automatically detects your intent:
- If another key is pressed within 2 seconds of pressing Ctrl → auto-cancel, no interference with Ctrl+C, Ctrl+V, etc.
- Overly short recordings (< 0.8s) are automatically discarded to avoid accidental garbage text
- Original clipboard content is restored after pasting, preserving your existing copy-paste workflow

## Features

- **Fully Local** — Streaming Zipformer + SenseVoice dual-engine, entirely offline, zero privacy concerns
- **CPU-Only, No GPU Needed** — Runs smoothly on any modern laptop or desktop; no CUDA or dedicated GPU required
- **Streaming Real-Time Feedback** — See recognition results as you speak via the overlay floating window
- **SenseVoice Quality Correction** — Automatically replaces streaming results with high-accuracy offline engine output
- **SenseVoice Built-in Punctuation** — ITN enabled to output punctuation marks (。, ? etc.) directly, no extra model needed, faster recognition
- **Voice Activity Detection** — Silero VAD filters out silent pauses, eliminating garbage characters (adjustable sensitivity)
- **Offline Recognition** — No internet or API keys needed; models auto-download on first launch (including VAD model)
- **China Mainland Mirror** — Auto-detects system language and uses hf-mirror.com for faster downloads in Simplified Chinese environments
- **Multi-Engine ASR** — Local engine (default), Tencent Cloud ASR, Google Gemini — customizable priority order
- **Auto Fallback** — Set priority order; if one engine fails, the next one is tried automatically
- **Cross-Platform** — Supports Windows, macOS, and Linux
- **Hotkey Trigger** — Hold Ctrl to start recording, release to auto-recognize and paste
- **Listening Mode** — Double-tap right Ctrl to activate continuous listening and recognition until manually turned off
- **Live Caption Mode** 🧪 — Captures system audio output for real-time recognition, displayed as multi-line captions in the overlay, with auto-saved transcript on close (mid-segment SenseVoice dual-color recognition is an experimental feature)
- **System Tray** — Runs in the background without disrupting your workflow
- **Simplified/Traditional Conversion** — Automatically converts Chinese text based on system locale, applies to both streaming and final results
- **Multi-Language UI** — Supports 繁體中文, 简体中文, and English interface
- **Auto-Start on Boot** — Supports launching automatically on system startup
- **Clipboard Protection** — Original clipboard content is restored after pasting

## Supported ASR Engines

### Local Dual-Engine (Default)
- **Streaming Zipformer** (real-time) — Bilingual Chinese-English streaming recognition, results shown as you speak
- **SenseVoice-Small (INT8)** (offline correction) — High-accuracy offline recognition, supports Chinese, English, Japanese, and Korean, with built-in ITN for direct punctuation output
- Features: Fully offline, zero privacy concerns, auto-downloads models on first launch
- Model storage location: `C:\ProgramData\VoiceTray\models\` (Windows)

### Tencent Cloud ASR
- **SentenceRecognition API** — Synchronous recognition
- Supported engines: 16k_zh, 16k_zh_video, 16k_en, 16k_yue, 16k_ca
- Requires: Secret ID + Secret Key

### Google Gemini
- **Multimodal generateContent API** — AI-powered speech recognition
- Supported models: gemini-2.5-flash, gemini-2.5-flash-lite
- Automatic punctuation, filler word removal, configurable output language
- Requires: API Key

## Quick Start

### Installation

Download the installer from the [Releases page](https://github.com/ascetic168/VoiceTray/releases).

### First Launch

1. The settings window opens automatically on first launch
2. Click the "Download Models" button to auto-download streaming and offline recognition models
3. Once downloaded, you're ready to go — no API key configuration needed

### Usage

1. Hold the Ctrl key and start speaking (the overlay window shows streaming recognition results in real time)
2. Release the Ctrl key — SenseVoice automatically performs high-accuracy correction and adds punctuation
3. Recognition results are automatically pasted into the current application
4. Original clipboard content is automatically restored

## Technical Architecture

```
User presses Ctrl
       ↓
  ContinuousRecorder (cpal continuous audio stream, 48kHz stereo)
       ↓
  Audio chunks → downmix to mono → resample to 16kHz
       ↓
  ┌──────────────────────────────────────────┐
  │       Silero VAD Voice Activity Detection │
  │  Pre-buffer preserves speech onset context │
  │  Silent segments filtered automatically    │
  └──────────────────────────────────────────┘
       ↓ (speech chunks only)
  ┌──────────────────────────────────────────┐
  │       Streaming Recognition Thread         │
  │         (Zipformer)                        │
  │  Real-time partial text → Overlay display  │
  │  Endpoint detection → send audio segments  │
  │  to SenseVoice                             │
  └──────────────────────────────────────────┘
       ↓ (Ctrl released)
  StreamingFinished → remaining audio sent to SenseVoice
       ↓
  ┌──────────────────────────────────────────┐
  │     SenseVoice Correction Thread (offline) │
  │  High-accuracy recognition → replaces     │
  │  streaming results                        │
  └──────────────────────────────────────────┘
       ↓
  SenseVoice built-in ITN punctuation (auto-adds 。, ? etc.)
       ↓
  convert.rs (Simplified/Traditional conversion based on system locale)
       ↓
  clipboard.rs (save original clipboard → set text → Ctrl+V → restore clipboard)
```

## License

Closed-source software, free to use. Developed by Charlie Chu (朱國棟).
