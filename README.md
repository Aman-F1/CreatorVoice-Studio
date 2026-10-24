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
```

### 2. ⚡ Crisp & Punchy (Tech / Gaming)
* **Logic:** Sharp high-end boost with fast attack compression for energetic commentary.
```bash
ffmpeg -i input.wav -af "highpass=f=100,equalizer=f=4500:width_type=h:width=300:g=4,compand=attacks=0.005:decays=0.05:points=-50/-50|-20/-8|0/0,loudnorm=I=-13:TP=-1.0" output.wav
```

### 3. 🔇 Hiss & Noise Removal
* **Logic:** Filters out static hiss, fan noise, and applies frequency clamping.
```bash
ffmpeg -i input.wav -af "highpass=f=90,lowpass=f=15000,afftdn=nr=12:nf=-50,loudnorm=I=-16:TP=-1.5" output.wav
```

### 4. 📢 YouTube Loudness Normalizer (-14 LUFS)
* **Logic:** Strictly normalizes voiceovers to YouTube's standard -14 LUFS without distortion.
```bash
ffmpeg -i input.wav -af "loudnorm=I=-14:TP=-1.0:LRA=7" output.wav
```

### 5. 🚀 All-in-One Master Creator Preset (Merged Hybrid)
* **Logic:** Combines noise reduction, clarity boost, dynamic compression, and YouTube loudness normalization into a single pass.
```bash
ffmpeg -i input.wav -af "highpass=f=90,lowpass=f=15000,afftdn=nr=12:nf=-50,equalizer=f=4500:width_type=h:width=300:g=4,compand=attacks=0.005:decays=0.05:points=-50/-50|-20/-8|0/0,loudnorm=I=-14:TP=-1.0:LRA=7" output.wav
```

---

## 📱 Planned UI Layout

```
+-----------------------------------+
|       CreatorVoice Studio         |
+-----------------------------------+
|  [ 🎙️ Record Audio ]               |
|               OR                  |
|  [ 📁 Import Audio File ]          |
+-----------------------------------+
|  Preview Waveform Visualizer      |
+-----------------------------------+
|  Select Creator Preset:           |
|   (o) Podcast Voice (Rich & Deep) |
|   ( ) Crisp & Punchy (Tech)       |
|   ( ) Hiss & Noise Removal        |
|   ( ) YouTube Normalizer          |
|   ( ) All-in-One Master Preset    |
+-----------------------------------+
|  [ ▶️ Play Preview ]               |
|  [ 💾 Export Enhanced Audio ]     |
+-----------------------------------+
```

---

## 🤝 How to Contribute

We welcome open-source contributions! Developers can contribute by:
1. Building or refining the Flutter UI components.
2. Integrating the `ffmpeg_kit_flutter` backend package.
3. Adding new FFmpeg filter chains in `assets/presets/preset_config.json`.

---

## 📄 License

This project is open-source under the [MIT License](LICENSE).
