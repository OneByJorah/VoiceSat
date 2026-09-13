<div align="center">

![VoiceSat banner](docs/assets/banner.svg)

# VoiceSat

**Jarvis-style wake-word voice pipeline — a hub and Raspberry Pi satellites that hear, think, and speak entirely on your own hardware**

[![GitHub release](https://img.shields.io/github/v/release/OneByJorah/VoiceSat?color=7C3AED&label=release&logo=github)](https://github.com/OneByJorah/VoiceSat/releases)
[![PyPI version](https://img.shields.io/pypi/v/voicesat?color=7C3AED&label=pip&logo=pypi)](https://pypi.org/project/voicesat/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)](https://www.docker.com/)
[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/downloads/)
[![openWakeWord](https://img.shields.io/badge/wake%20word-openWakeWord-7C3AED?style=flat-square&logo=raspberrypi&logoColor=white)](https://github.com/dscripka/openWakeWord)
[![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-satellites-C51A4A?style=flat-square&logo=raspberrypi&logoColor=white)](https://www.raspberrypi.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?color=FFB300&logo=open-source-initiative&logoColor=FFB300)](https://opensource.org/licenses/MIT)

</div>

![VoiceSat screenshot](docs/assets/screenshot.png)

## What This Is

Smart speakers stop working when the cloud does, and they hear everything you say. VoiceSat splits the job: a cheap Raspberry Pi satellite listens for a wake word and streams audio to a hub that runs STT, an LLM, and TTS locally and keeps conversation memory in Honcho. Add as many satellites as you want; they all share the same brain.

Built for privacy-conscious users who want a voice assistant that works entirely on their own hardware.

## Quick Start

### pip

```bash
pip install voicesat
```

### Docker (recommended)

```bash
git clone https://github.com/OneByJorah/VoiceSat.git
cd VoiceSat
docker compose up -d
```

### From Source

```bash
git clone https://github.com/OneByJorah/VoiceSat.git
cd VoiceSat
pip install -r requirements.txt
python3 hub.py
```

## Install

### pip

```bash
pip install voicesat
```

### Docker

```bash
git clone https://github.com/OneByJorah/VoiceSat.git
cd VoiceSat
docker compose up -d
```

### From Source

```bash
git clone https://github.com/OneByJorah/VoiceSat.git
cd VoiceSat
pip install -r requirements.txt
python3 hub.py
```

## Features

- **Wake-word detection** — openWakeWord-based wake word on Raspberry Pi satellites
- **Local STT** — speech-to-text runs entirely on your hardware
- **Local LLM** — conversation intelligence runs offline
- **Local TTS** — text-to-speech with no cloud dependencies
- **Multi-satellite** — add as many Raspberry Pi satellites as you want
- **Shared brain** — all satellites share the same conversation memory via Honcho
- **Privacy-first** — no cloud, no data leaves your network
- **Docker support** — run with `docker compose up -d`

## Tech Stack

- **Hub** — Python 3.11+, STT, LLM, TTS, Honcho memory
- **Satellites** — Raspberry Pi, openWakeWord, pyaudio, sounddevice
- **Communication** — WebSockets
- **Deployment** — Docker Compose, pip install
- **Audio** — numpy, sounddevice, pyaudio, torch

## Package Badges

[![GitHub release](https://img.shields.io/github/v/release/OneByJorah/VoiceSat?color=7C3AED&label=release&logo=github)](https://github.com/OneByJorah/VoiceSat/releases)
[![PyPI version](https://img.shields.io/pypi/v/voicesat?color=7C3AED&label=pip&logo=pypi)](https://pypi.org/project/voicesat/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?color=FFB300&logo=open-source-initiative&logoColor=FFB300)](https://opensource.org/licenses/MIT)

## Architecture

```
Raspberry Pi Satellite (wake word + audio capture)
        │
        ▼ (WebSocket audio stream)
  VoiceSat Hub (STT → LLM → TTS + Honcho memory)
        │
        ▼ (WebSocket audio stream)
  Raspberry Pi Satellite (TTS playback)
```

## Contributing

Contributions are welcome. [Open an issue](https://github.com/OneByJorah/VoiceSat/issues) to report a bug or request a feature.

## License

MIT — see [LICENSE](LICENSE).
