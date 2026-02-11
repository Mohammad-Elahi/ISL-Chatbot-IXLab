#Setup & Installation

# Unity

## Building

For building the project, selecting the scene `Conversation` is enough. No further setup is necessary.

The scene setup is more complex and shouldn't be touched if possible. On a high level, it contains an object that has the `ConversationManager` component, with two child components for the agents' postions. It also features an object with the `Player` component, which itself is a parent to the two cameras that are set up for the screens. Another object with the `Display Manager` component is responsible for enabling the rendering to multiple displays.

## Setup

This project needs a PC running Windows or Linux. Starting the built application is as easy as starting the `.exe` file from the build process. Two screens need to be connected to the PC, set up in portrait orientation. The application will render to display 1 and display 2. The UI should be placed on the **outer edges** (language on the left, microphone on the right) of the screens. If that isn't the case, pressing `F2` will swap the displays. This requires an external keyboard.


# Server

* Prerequisites: Python 3.9+

**Navigate to the backend directory:**
---
Bash
cd backend
---

**Create a virtual environment:**
```
Bash
python -m venv venv
```

# Windows:
venv\Scripts\activate

# Mac/Linux:
source venv/bin/activate

* Install dependencies:
```
Bash
pip install -r requirements.txt
```

**Configuration: Create a file named .env in the backend/ folder and add your credentials:**

Ini, TOML
OPENAI_API_KEY=sk-your-openai-key-here
OPENAI_MODEL=gpt-4o-mini

# Optional Configuration
MAX_USER_TURNS=5
FEEDBACK_LOG_PATH=data/feedback_log.jsonl

**Running the Server**
Start the live server using Uvicorn. The Unity client can connect to this address.

Bash
uvicorn main:app --reload
Local Swagger UI: Visit http://localhost:8000/docs to test endpoints manually.

Health Check: http://localhost:8000/ should return {"status": "healthy"}.

API EndpointsMethodEndpointDescriptionGET/startResets the session and generates a random "Hook" question to start the chat.POST/chatThe main logic loop. Accepts user text, updates state, and returns the AI response + current emotion.POST/sttSpeech-to-Text: Accepts a .wav file and returns the transcript using OpenAI Whisper.POST/ttsText-to-Speech: Accepts text and returns streaming audio bytes (MP3) using OpenAI TTS.

**Data Logging**
All visitor feedback is automatically structured and logged to data/feedback_log.jsonl.

Example Log Entry:

JSON
{
  "session_id": "user_123",
  "type": "answer",
  "exhibit": "VR experience",
  "question_id": "vr_comfort",
  "answer": "It made me feel a bit dizzy but the visuals were cool.",
  "ts": "2023-10-27T10:00:00"
}



