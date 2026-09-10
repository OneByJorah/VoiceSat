<div align="center">

![VoiceSat banner](docs/assets/banner.svg)

# VoiceSat

Jarvis-style wake-word voice pipeline

![License](https://img.shields.io/badge/license-MIT-brightgreen)
![Language](https://img.shields.io/badge/language-Python-blue)
</div>

---

<p align="center">
  <img src="docs/screenshots/dashboard.png" alt="VoiceSat dashboard preview" width="90%">
</p>

<br>

---

## Features

- **Wake Word Detection** — "Hey Jarvis" style activation with openWakeWord.
- **Local STT** — Speech-to-text with faster-whisper (no cloud).
- **Local LLM** — Private language model processing.
- **Local TTS** — Text-to-speech with Piper.
- **Persistent Memory** — Long-term conversation memory with Honcho.
- **Raspberry Pi** — Optimized for Raspberry Pi 4/5.
- **100% Private** — All processing stays on-device.
- **Multi-Satellite** — Deploy multiple satellite units.

## Architecture

VoiceSat runs as two components:

```
Satellite (Pi)                         Hub (server)
┌─────────────────┐    Tailscale     ┌─────────────────────────────┐
│ Microphone      │                   │ Bridge (FastAPI :8000)      │
│   ↓             │   HTTP POST       │   ↓           ↓           ↓ │
│ Wake Word       │ ───────────────▶ │ Ears(:9000) LLM  Mouth(:9001)
│   ↓             │ ◀─────────────── │   (STT)   (API)   (TTS)    │
│ Speaker ◀───────│   audio/wav       │                             │
└─────────────────┘                   │ Dashboard (/dashboard/)     │
                                      └─────────────────────────────┘
```

- **Satellite** (`satellite/wake.py`) — runs on the Pi, captures audio, detects wake word, sends WAV to hub, plays back TTS response.
- **Hub** — three microservices orchestrated via Docker Compose:
  - **Ears** — faster-whisper STT endpoint (`:9000`)
  - **Mouth** — Piper TTS endpoint (`:9001`)
  - **Bridge** — orchestrates STT → LLM → TTS, Honcho memory, dashboard (`:8000`)

## Quick Start

### Docker (recommended)

```bash
git clone https://github.com/OneByJorah/VoiceSat.git
cd VoiceSat

cp .env.example .env
# edit .env — set HERMES_API_URL, HERMES_API_KEY, HONCHO_URL, HUB_BIND_IP

# Start hub services
docker compose -f docker-compose.hub.yml up --build -d

# On the Raspberry Pi — start satellite
docker compose -f docker-compose.satellite.yml up --build -d
```

### Raspberry Pi (bare metal)

```bash
git clone https://github.com/OneByJorah/VoiceSat.git
cd VoiceSat

pip install -r satellite/requirements.txt

# Run the voice pipeline
python3 satellite/wake.py
```

See [SATELLITE_PI_DEPLOY.md](SATELLITE_PI_DEPLOY.md) for full Pi deployment guide.

## Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `HERMES_API_URL` | — | LLM API endpoint (OpenAI-compatible) |
| `HERMES_API_KEY` | — | API key for LLM |
| `HERMES_MODEL` | `hermes-3-llama-3.1-8b` | LLM model name |
| `HONCHO_URL` | — | Honcho server URL for memory |
| `HONCHO_API_KEY` | — | Honcho API key |
| `WHISPER_MODEL` | `small.en` | faster-whisper model size |
| `PIPER_VOICE` | `en_US-lessac-medium` | Piper TTS voice |
| `BRIDGE_URL` | — | Hub URL from satellite (`http://<hub>:18000/satellite/utterance`) |
| `WAKE_MODEL` | `hey_jarvis` | openWakeWord model |
| `SATELLITE_ID` | `satellite-1` | Unique ID per satellite |
| `HUB_BIND_IP` | `127.0.0.1` | Hub service bind address |

## Hardware Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **Board** | Raspberry Pi 4 (2GB) | Raspberry Pi 5 (4GB+) |
| **RAM** | 2GB | 4GB+ |
| **Storage** | 16GB SD | 32GB+ SSD |
| **Audio** | USB mic + speaker | ReSpeaker HAT |
| **Network** | WiFi | Ethernet |

## Model Downloads

| Model | Size | Purpose |
|-------|------|---------|
| openWakeWord | ~10MB | Wake word detection |
| faster-whisper (base) | ~150MB | Speech-to-text |
| Piper (lessac) | ~50MB | Text-to-speech |
| LLM (via API) | — | Remote language model |

## Contributing

Contributions are welcome. Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for community standards.

## Security

For security concerns, see [SECURITY.md](SECURITY.md). Please report vulnerabilities to **security@jorahone.com** — do not use public issues.

## License

[MIT License](LICENSE) © Jhonattan L. Jimenez (OneByJorah)

---

<p align="center">Built with 🌴 by <a href="https://github.com/OneByJorah">OneByJorah</a> · <a href="https://jorahone.com">jorahone.com</a></p>
