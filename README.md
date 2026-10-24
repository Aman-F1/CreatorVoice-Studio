# CreatorVoice-Studio
Offline FFmpeg-powered audio enhancing Android app for YouTube creators.
# 🎙️ CreatorVoice Studio

An open-source, 100% offline Android application designed for YouTube creators, podcasters, and video editors to enhance voiceovers using professional Adobe Audition-style presets.

---

## 📌 Problem & Solution

* **Problem:** Many YouTube creators face background noise, low voice volume, and lack of audio clarity. Existing repair tools require paid subscriptions or internet connections.
* **Solution:** **CreatorVoice Studio** provides a free, offline, local FFmpeg-based audio processing engine on Android with one-tap presets tailored for YouTube voiceovers.

---

## ✨ Key Features

* 🔒 **100% Offline & Private:** All audio processing takes place locally on the user's device via FFmpeg.
* ⚡ **One-Tap Creator Presets:** Pre-configured audio filter chains tuned specifically for voiceovers.
* 🎛️ **Audition-Style Processing:** Real-time chain of Parametric EQ, Dynamic Compression, Noise Reduction, and Peak Limiting.
* 📁 **Fast Export:** Export enhanced high-quality `.wav` and `.mp3` files directly to internal storage.
* 🛠️ **Open Configuration:** Presets are loaded dynamically from a JSON file (`assets/presets/preset_config.json`).

---

## 🛠️ Tech Stack & Architecture

* **Frontend Framework:** Flutter / Android Native (Kotlin)
* **Audio Engine:** `ffmpeg_kit_flutter` (FFmpeg binary engine)
* **Preset Format:** JSON-configured FFmpeg Filter Graphs

---

## 🎛️ Audio Processing Presets & FFmpeg Logic

The app dynamically maps creator presets into optimized FFmpeg audio commands:

### 1. 🎙️ Podcast Voice (Rich & Deep)
* **Logic:** Cuts low-end rumble, reduces mid-range mud, boosts clarity, applies medium compression, and targets -14 LUFS.
```bash
ffmpeg -i input.wav -af "highpass=f=80,equalizer=f=250:width_type=h:width=100:g=-2,equalizer=f=3000:width_type=h:width=200:g=3,compand=attacks=0.01:decays=0.1:points=-60/-60|-24/-12|-8/-3|0/0,loudnorm=I=-14:TP=-1.0:LRA=11" output.wav
