# The_empathy_engine

A FastAPI-based service that detects emotion from text input (happy / neutral / frustrated) and produces emotionally-modulated speech using OpenAI’s `gpt-4o-mini-tts`.  
The generated speech is adjusted for **pitch**, **speed**, and **volume** to match the detected emotion, then served as a downloadable MP3.

---

# Features
- Emotion detection using cardiffnlp/twitter-roberta-base-sentiment-latest.
- TTS generation using OpenAI gpt-4o-mini-tts.
- Audio post-processing (pitch, speed, volume) with pydub.
- REST API built with FastAPI + Swagger UI.
- Downloadable MP3 via /api/audio/{filename} endpoint.

---

## 📦 Installation
 Clone repository
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

export OPENAI_API_KEY="sk-..."    # Linux/macOS
setx OPENAI_API_KEY "sk-..."      # Windows

Run the API server
uvicorn main:app --reload --host 0.0.0.0 --port 8000

1. Generate Audio
POST /api/empathy

Request Body

{
  "text": "I am very happy today!"
}
Response

{
  "emotion": "happy",
  "confidence": 0.95,
  "audio_url": "/api/audio/emp_a1b2c3d4e5.mp3"
}
2. Download Audio
GET {filename}

Example:
GET emp_a1b2c3d4e5.mp3

This will download or play the MP3 file.

# Design choices
| Emotion    | Pitch (semitones) | Speed Multiplier | Volume (dB) |
| ---------- | ----------------- | ---------------- | ----------- |
| happy      | +4                | 1.25             | +3          |
| neutral    | 0                 | 1.0              | 0           |
| frustrated | -3                | 0.85             | -2          |

Happy → higher pitch + faster speed
Frustrated → lower pitch + slower speed
Neutral → unchanged

These choices give clear, natural-sounding variation without sounding robotic.

# Tech Stack
FastAPI (REST API)
Hugging Face Transformers (emotion detection)
OpenAI TTS (gpt-4o-mini-tts)
pydub + ffmpeg (audio processing)
uvicorn (ASGI server)

PRs and suggestions are welcome.
