# //Data Spaces - ISL Chatbot

**Members: Mohammad Elahi, Sayandip Srimani, Ruben Pratzka**

---

**ISL Chatbot: A system designed to collect visitor feedback naturally in exhibition spaces**

###  The Challenge:
* Traditional feedback forms are rarely filled out by visitors.
* Feedback collection in exhibitions feels unnatural and tedious.

###  Our Solution:
* Integrate feedback collection directly into the exhibition experience.
* Create natural, effortless conversations.
* Transform feedback into a conversational interaction.

---

## Project Demonstration



https://github.com/user-attachments/assets/6f78fcf9-164e-4ee3-9bde-819cfb6bfc3e


*[click here to view the full HD version on YouTube](https://youtu.be/E7ethBZINgo)*

---

##  Setup & Installation

### Hardware Requirements
This installation requires the following hardware components:
* **32-inch Touchscreen:** Main interaction point, connected to the PC in portrait orientation.
* **Camera with Microphone:** Mounted above the screen to capture voice input for the speech-to-text system.
* **Projector:** Creates a light path on the floor to attract visitors to the Chatbot.
* **Speakers:** Audio output for chatbot responses.
* **PC:** Running Windows or Linux (see Server prerequisites below).

### System Architecture
* **Unity Frontend:** Handles particle simulation and animation.
* **Python Backend:** Integrates with OpenAI's API for language processing (LLM), speech-to-text (STT), and text-to-speech (TTS).
* **Audio Input:** Uses the camera's built-in microphone to capture visitor queries.

### Installation Steps

**1. Hardware Setup**
* Connect the 32-inch touchscreen to the PC and configure it in portrait orientation.
* Mount the camera above the screen.
* Position the projector to create the desired floor light path and connect it to the PC.
* Connect speakers to the PC.

**2. Unity Application**
* Open the Unity project. In Unity Hub, select "Add Project from Disc", then select the folder `work/unity/ISLChatbot`.
* Build the Unity project using the `Particle Sim` scene. (File &rarr; Build Profiles &rarr; Build)
* Start the built application by running the `.exe` file. Ensure that the vertical screen is set as the main display and the projector as the secondary screen.
* Alternatively, you can simply run the provided `IXTD 3D Particle Simulation.exe` from the Build folder. (Windows only)

**3. Python Backend**
* Follow the instructions in the **Server** section below to install dependencies and run the backend.

---

## Server

**Prerequisites:** Python 3.9+

Navigate to the backend directory:
```bash
cd backend
```

Create a virtual environment:
```bash
python -m venv venv
```

Activate the virtual environment:
* **Windows:** `venv\Scripts\activate`
* **Mac/Linux:** `source venv/bin/activate`

Install dependencies:
```bash
pip install -r requirements.txt
```

**Configuration:** Create a file named `.env` in the `backend/` folder and add your credentials:
```ini
OPENAI_API_KEY=sk-your-openai-key-here
OPENAI_MODEL=gpt-4o-mini
```

**Optional Configuration:**
```ini
MAX_USER_TURNS=5
FEEDBACK_LOG_PATH=data/feedback_log.jsonl
```

**Running the Server:**
Start the live server using Uvicorn. The Unity client can connect to this address.
```bash
uvicorn main:app --reload
```
* **Local Swagger UI:** Visit `http://localhost:8000/docs` to test endpoints manually.
* **Health Check:** `http://localhost:8000/` should return `{"status": "healthy"}`.

---

##  API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/start` | Resets the session and generates a random "Hook" question to start the chat. |
| `POST` | `/chat` | The main logic loop. Accepts user text, updates state, and returns the AI response + current emotion. |
| `POST` | `/stt` | Speech-to-Text: Accepts a `.wav` file and returns the transcript using OpenAI Whisper. |
| `POST` | `/tts` | Text-to-Speech: Accepts text and returns streaming audio bytes (MP3) using OpenAI TTS. |

---

##  Data Logging
All visitor feedback is automatically structured and logged to `data/feedback_log.jsonl`.

**Example Log Entry:**
```json
{
  "session_id": "user_123",
  "type": "answer",
  "exhibit": "VR experience",
  "question_id": "vr_comfort",
  "answer": "It made me feel a bit dizzy but the visuals were cool.",
  "ts": "2023-10-27T10:00:00"
}
```
