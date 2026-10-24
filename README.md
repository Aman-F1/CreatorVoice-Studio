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

### 0. 🎙️ Podcast Voice (Rich & Deep)
* **Logic:** Cuts low-end rumble, reduces mid-range mud, boosts clarity, applies medium compression, and targets -14 LUFS.
```bash
ffmpeg -i input.wav -af "highpass=f=80,equalizer=f=250:width_type=h:width=100:g=-2,equalizer=f=3000:width_type=h:width=200:g=3,compand=attacks=0.01:decays=0.1:points=-60/-60|-24/-12|-8/-3|0/0,loudnorm=I=-14:TP=-1.0:LRA=11" output.wav
===================================================================

CREATORVOICE STUDIO - MERGED PRESETS SPECIFICATION

===================================================================



This document merges the core audio processing logic and FFmpeg filter chains for:

1. ⚡ Crisp & Punchy (Tech / Gaming)

2. 🔇 Hiss & Noise Removal

3. 📢 YouTube Loudness Normalizer (-14 LUFS)



-------------------------------------------------------------------

1. ⚡ CRISP & PUNCHY (TECH / GAMING)

-------------------------------------------------------------------

Logic: Sharp high-end boost, fast attack compressor for speech presence, and aggressive loudness targeting for high-energy video commentary.



FFmpeg Command:

ffmpeg -i input.wav -af "highpass=f=100,equalizer=f=4500:width_type=h:width=300:g=4,compand=attacks=0.005:decays=0.05:points=-50/-50|-20/-8|0/0,loudnorm=I=-13:TP=-1.0" output.wav



Parameters:

- Highpass Filter: 100 Hz

- Presence Boost Frequency: 4500 Hz (+4 dB)

- Compressor Attack/Release: 5ms / 50ms

- Target Loudness: -13 LUFS



-------------------------------------------------------------------

2. 🔇 HISS & NOISE REMOVAL

-------------------------------------------------------------------

Logic: Filters out background room noise, microphone hiss, and fan hum with frequency bounds and FFT noise reduction before applying standard studio level adjustments.



FFmpeg Command:

ffmpeg -i input.wav -af "highpass=f=90,lowpass=f=15000,afftdn=nr=12:nf=-50,loudnorm=I=-16:TP=-1.5" output.wav



Parameters:

- Highpass Cutoff: 90 Hz

- Lowpass Cutoff: 15000 Hz

- FFT Noise Reduction: 12 dB

- Noise Floor Target: -50 dB

- Target Loudness: -16 LUFS

- True Peak Limit: -1.5 dB



-------------------------------------------------------------------

3. 📢 YOUTUBE LOUDNESS NORMALIZER (-14 LUFS)

-------------------------------------------------------------------

Logic: Pure dynamic range and loudness normalization specifically tuned to YouTube's standard without changing vocal timbre or introducing clipping distortion.



FFmpeg Command:

ffmpeg -i input.wav -af "loudnorm=I=-14:TP=-1.0:LRA=7" output.wav



Parameters:

- Target Integrated Loudness (I): -14 LUFS

- True Peak (TP): -1.0 dB

- Loudness Range Target (LRA): 7 LUFS



-------------------------------------------------------------------

COMBINED MASTER CHAIN (ALL-IN-ONE HYBRID PRESET)

-------------------------------------------------------------------

Logic: A combined filter chain that removes noise, boosts clarity/presence, and normalizes directly to YouTube's -14 LUFS standard in a single pass.



FFmpeg Command:

ffmpeg -i input.wav -af "highpass=f=90,lowpass=f=15000,afftdn=nr=12:nf=-50,equalizer=f=4500:width_type=h:width=300:g=4,compand=attacks=0.005:decays=0.05:points=-50/-50|-20/-8|0/0,loudnorm=I=-14:TP=-1.0:LRA=7" output.wav
