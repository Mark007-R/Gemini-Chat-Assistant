# Gemini-Chat-Assistant

A small **Flask web application** powered by **Google's Gemini API** (`gemini-3.8-flash` model) with a light, paper-style chat interface.  
Users can ask questions and get formatted answers, and the conversation is remembered for the browser session.

---

## 📌 Features

- **Google Gemini integration** – Calls `gemini-3.8-flash` through the official `google-genai` SDK.
- **Flask backend** – One small `app.py`.
- **Conversation memory** – Earlier questions and answers in the session are sent along with each new question; `/clear` resets it.
- **Markdown answers** – Replies are converted from Markdown to HTML before they are shown.
- **Rate limiting** – 20 requests per minute overall and 5 per 10 seconds on `/ask` (Flask-Limiter).
- **Environment variable support** – The API key is read from a `.env` file that Git ignores.
- **JSON API endpoint** – `/ask` returns the answer as JSON.

---

## 📂 Project Structure

```text
Gemini-Chat-Assistant/
│── app.py                # Main Flask application
│── templates/
│   └── index.html        # Frontend HTML
│── static/
│   └── style.css         # CSS styling
│── requirements.txt      # Python dependencies
│── .env.example          # Template for the API key
│── .env                  # Your API key (ignored by Git)
│── .gitignore
│── README.md
```

## ⚙️ Setup

Requires Python 3.11 or newer.

```bash
python -m venv .venv
.venv\Scripts\activate        # Windows (macOS/Linux: source .venv/bin/activate)
pip install -r requirements.txt
```

Copy `.env.example` to `.env` and put in a Gemini API key from [Google AI Studio](https://aistudio.google.com/):

```text
GEMINI_API_KEY=your_gemini_api_key_here
```

## 🚀 Running the Application

```bash
python app.py
```

The app will run at:

```text
http://127.0.0.1:5000
```

## 🔌 API

| Route | Method | What it does |
|---|---|---|
| `/` | GET | Serves the chat page |
| `/ask` | POST | Body `{"question": "..."}`, returns `{"answer": "<html>"}` or `{"error": "..."}` |
| `/clear` | POST | Clears the conversation history for the session |

## 🔄 Changing the model

The model ID is the `MODEL_NAME` constant in `app.py`. Google retires model IDs over time (the `gemini-1.5-flash` this app first used is no longer served), so if `/ask` starts returning a "model not found" error, pick a current ID from the [Gemini models page](https://ai.google.dev/gemini-api/docs/models).
