# 🎧 ColoredNoise Pro

> **Industry-Standard Colored Noise Generator** — High-fidelity spectral synthesis for acoustic measurement, audio engineering, psychoacoustic research, and forensic analysis.

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Standard](https://img.shields.io/badge/Standards-IEC%2FANSI%2FISO%2FITU-orange)](https://github.com/yourusername/colorednoise-pro)

---

## 📖 Overview

ColoredNoise Pro is a production-grade Python tool for generating **six industry-standard colored noise types** with mathematically precise spectral shaping. Unlike recursive/IIR filter-based generators that accumulate phase errors and low-frequency drift, this tool uses **FFT-based spectral synthesis** to ensure exact amplitude slopes across the entire frequency spectrum — critical for acoustic measurement, room calibration, speaker burn-in, and forensic audio applications.

The generator adheres to international standards including **IEC 60268-1**, **ANSI S1.42**, **IEC 61672-1**, **ISO 226**, and **ITU-R BS.1770-4**, producing files suitable for professional broadcast, archival mastering, and scientific research.

### Why This Tool?

| Problem with Existing Tools | ColoredNoise Pro Solution |
|----------------------------|---------------------------|
| IIR filter drift over long durations | FFT-based exact spectral curves — no cumulative error |
| Inconsistent pink noise algorithms | ANSI S1.42 / IEC 61672-1 compliant `-3 dB/octave` |
| No grey noise (perceptual flatness) | Inverse A-weighting per ISO 226 equal-loudness contours |
| Poor dithering or clipping artifacts | TPDF dither + true peak normalization to `-1.0 dBFS` |
| Limited sample rate/bit depth options | Full range: 44.1kHz–192kHz, 16-bit to 64-bit float |

---

## ✨ Features

### Core Generation

- [x] **Six Noise Colors** — White, Pink, Brown (Red), Blue, Violet, and Grey
- [x] **FFT-Based Spectral Shaping** — Mathematically exact amplitude slopes without filter drift
- [x] **Gaussian White Noise Base** — Box-Muller transform for natural audio characteristics
- [x] **Customizable Duration** — From 1 second to hours (memory-limited)
- [x] **Reproducible Output** — Optional random seed for identical file generation

### Standards Compliance

- [x] **White Noise** — Flat PSD per **IEC 60268-1** (Sound System Equipment)
- [x] **Pink Noise** — `-3 dB/octave` per **ANSI S1.42** / **IEC 61672-1 Class 1**
- [x] **Brown Noise** — `-6 dB/octave` derived from Brownian stochastic process
- [x] **Blue Noise** — `+3 dB/octave` inverse pink spectrum
- [x] **Violet Noise** — `+6 dB/octave` inverse brown / 2nd derivative of Brownian
- [x] **Grey Noise** — Psychoacoustically flat via inverse **A-weighting (ISO 226 / IEC 61672-1)**

### Output Quality

- [x] **Sample Rates** — 44.1 kHz, 48 kHz, 88.2 kHz, 96 kHz, 176.4 kHz, 192 kHz
- [x] **Bit Depths** — 16-bit PCM, 24-bit PCM, 32-bit float, 64-bit float
- [x] **File Formats** — WAV (RIFF) and FLAC (lossless compression)
- [x] **True Peak Normalization** — `-1.0 dBFS` per **ITU-R BS.1770-4**
- [x] **DC Offset Removal** — Prevents asymmetric clipping per **AES-6id**
- [x] **TPDF Dithering** — Triangular PDF dither for 16/24-bit integer exports

### Usability

- [x] **Command-Line Interface** — Full argparse with help, examples, and validation
- [x] **Batch Generation** — Generate all six noise types in a single command
- [x] **Automatic File Naming** — Descriptive filenames with rate, depth, and duration
- [x] **Output Verification** — Post-write file info display (format, frames, size)
- [x] **Standards Reference** — Built-in `--standards` flag prints compliance documentation

---

## 🚀 Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/colorednoise-pro.git
cd colorednoise-pro

# Install dependencies
pip install numpy soundfile

# Optional: Faster FFT for large files (>10 min)
pip install pyfftw
```

### Basic Usage

```bash
# Generate pink noise (default: 96kHz, 32-bit float, 60s)
python noise_generator.py

# Generate all noise types at 48kHz/24-bit for 5 minutes
python noise_generator.py --all --rate 48000 --depth PCM_24 --duration 300

# Generate grey noise for psychoacoustic calibration (192kHz)
python noise_generator.py --type grey --rate 192000 --depth FLOAT_32

# Archive-quality FLAC output (smaller files, lossless)
python noise_generator.py --all --format FLAC --rate 96000 --depth PCM_24

# Reproducible generation (identical output every time)
python noise_generator.py --type brown --seed 42 --duration 3600
```

### Python API

```python
from noise_generator import ColoredNoiseGenerator, NoiseColor

# Initialize generator
gen = ColoredNoiseGenerator(
    sample_rate=96000,
    duration_seconds=300,
    bit_depth="FLOAT_32",
    output_dir="./my_noise",
    seed=42
)

# Generate single noise type
filepath = gen.generate(NoiseColor.PINK)

# Generate complete suite
paths = gen.generate_all(format="WAV")
```

---

## 📊 Noise Specifications

| Color | Slope | Standard | Primary Use |
|-------|-------|----------|-------------|
| **White** | `0 dB/octave` | IEC 60268-1 | Speaker burn-in, masking, room acoustics |
| **Pink** | `-3 dB/octave` | ANSI S1.42 / IEC 61672-1 | Room EQ (RTA), speaker testing, acoustic measurement |
| **Brown** | `-6 dB/octave` | Brownian process | Sleep aid, bass testing, waterfall analysis |
| **Blue** | `+3 dB/octave` | Inverse Pink | Dithering, high-frequency analysis |
| **Violet** | `+6 dB/octave` | Inverse Brown | Digital audio testing, ultrasonic emphasis |
| **Grey** | Perceptual flat | ISO 226 / IEC 61672-1 A-weighting | Psychoacoustic research, loudness calibration |

---

## 🛠️ Advanced Configuration

### CLI Reference

```
usage: noise_generator.py [-h] [--type {white,pink,brown,blue,violet,grey}]
                          [--all] [--rate {44100,48000,88200,96000,176400,192000}]
                          [--duration DURATION]
                          [--depth {PCM_16,PCM_24,FLOAT_32,FLOAT_64}]
                          [--format {WAV,FLAC}] [--output OUTPUT] [--seed SEED]
                          [--standards]

Industry-Standard Colored Noise Generator

options:
  -h, --help            show this help message and exit
  --type {white,pink,brown,blue,violet,grey}
                        Type of colored noise (default: pink)
  --all                 Generate all noise types
  --rate {44100,...}    Sample rate in Hz (default: 96000)
  --duration DURATION   Duration in seconds (default: 60)
  --depth {PCM_16,...}  Bit depth (default: FLOAT_32)
  --format {WAV,FLAC}   Output format (default: WAV)
  --output OUTPUT       Output directory (default: ./noise_files)
  --seed SEED           Random seed for reproducible generation
  --standards           Print industry standards reference
```

### Quality Tiers

| Tier | Sample Rate | Bit Depth | Format | Use Case |
|------|-------------|-----------|--------|----------|
| 🏆 Archive/Master | 96–192 kHz | 32-bit float | WAV | Long-term archival, forensic evidence |
| 🎬 Broadcast/Pro | 48 kHz | 24-bit | WAV/FLAC | Film/video post-production |
| 💿 CD Quality | 44.1 kHz | 16-bit | FLAC | Distribution, consumer playback |
| 🔬 Measurement | 48–96 kHz | 32-bit float | WAV | Acoustic analysis, calibration |

---

## 🗺️ Roadmap

### Phase 1 — Core Stability (Current)
- [x] FFT-based spectral synthesis for all six noise colors
- [x] CLI with argparse and validation
- [x] WAV and FLAC export
- [x] Standards compliance documentation

### Phase 2 — Enhanced Generation (v1.1)
- [ ] **Multi-channel output** — Stereo, 5.1, 7.1, Ambisonics (B-format)
- [ ] **Variable slope control** — Custom dB/octave beyond standard colors
- [ ] **Band-limited noise** — User-defined frequency range (e.g., 20Hz–20kHz)
- [ ] **Swept sine + noise hybrid** — For speaker distortion measurement
- [ ] **Real-time streaming** — Live output via PyAudio/PortAudio

### Phase 3 — Analysis & Validation (v1.2)
- [ ] **Integrated spectrum analyzer** — Plot and verify output with matplotlib
- [ ] **THD+N measurement** — Total Harmonic Distortion + Noise calculation
- [ ] **Crest factor analysis** — Peak-to-RMS ratio per noise type
- [ ] **ITU-R BS.1770 loudness** — Integrated LUFS measurement
- [ ] **Batch validation report** — Automated compliance checking against standards

### Phase 4 — Professional Integration (v1.3)
- [ ] **ASIO/CoreAudio output** — Low-latency professional audio interface support
- [ ] **MIDI/OSC control** — Remote parameter control for live environments
- [ ] **Head-related transfer function (HRTF)** — Binaural spatial noise generation
- [ ] **Weighted noise variants** — C-weighted, Z-weighted, custom weighting curves
- [ ] **Export to DAW formats** — Broadcast WAV (BWF) with embedded metadata

### Phase 5 — Research & Forensic Tools (v2.0)
- [ ] **Chain-of-custody hashing** — SHA-256/Blake3 per-file integrity verification
- [ ] **Timestamped generation logs** — Forensic audit trail for evidence files
- [ ] **Correlation detection** — Compare generated noise against unknown samples
- [ ] **Statistical fingerprinting** — Unique spectral signatures per generation run
- [ ] **Batch watermarking** — Inaudible forensic watermark embedding
- [ ] **Court-ready documentation** — Automated expert witness report generation

### Phase 6 — Platform Expansion (v2.1+)
- [ ] **Web UI (Streamlit/Gradio)** — Browser-based generation and preview
- [ ] **PWA packaging** — Offline-capable web app for mobile/tablet
- [ ] **REST API** — Server-based generation with queue management
- [ ] **Docker container** — Reproducible deployment for CI/CD pipelines
- [ ] **Plugin architecture** — VST3/AU audio plugin for DAW integration

---

## 📁 Project Structure

```
colorednoise-pro/
├── noise_generator.py      # Main generator script
├── README.md               # This file
├── LICENSE                 # MIT License
├── requirements.txt        # Python dependencies
├── examples/
│   ├── generate_suite.sh   # Bash script for full suite generation
│   └── api_example.py      # Python API usage examples
├── tests/
│   ├── test_spectral.py    # Spectral accuracy validation
│   └── test_compliance.py  # Standards compliance checks
└── docs/
    ├── STANDARDS.md        # Detailed standards reference
    └── SPECTRAL_ANALYSIS.md # Mathematical derivation of curves
```

---

## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Priority areas:
- Spectral accuracy validation against reference hardware
- Additional export formats (AIFF, CAF, DSD)
- Performance optimization for real-time generation
- Cross-platform testing (Windows, macOS, Linux)

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgments

- Spectral shaping methodology based on DSP best practices from [AES](https://aes.org) and [IEEE](https://ieee.org)
- A-weighting curve implementation per IEC 61672-1:2013
- True peak normalization per ITU-R BS.1770-4
- Inspired by the acoustic measurement community and forensic audio standards

---

> **Note:** This tool is designed for professional audio engineering, acoustic research, and forensic applications. Generated noise files are suitable for use as calibration references, measurement stimuli, and evidence-grade audio sources when proper chain-of-custody procedures are followed.

---

<p align="center">
  <sub>Built with precision for the sound production, audio engineering and forensic community.</sub>
</p>
