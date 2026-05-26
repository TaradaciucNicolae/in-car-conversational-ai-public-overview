# In-Car Conversational AI — Public Overview

Public technical overview of a voice-first in-car dashboard prototype that lets drivers and passengers control non-driving cabin and infotainment features through natural-language commands.

> The full source code is private due to intellectual property considerations. This repository documents the project’s functionality, architecture, technologies, screenshots, and implementation approach without exposing private source code, secrets, generated assets, or internal Git history.

---

## Product Overview

**In-Car Conversational AI** is a local car-dashboard prototype that simulates the moment a driver enters a vehicle, authenticates manually or through face recognition, and then interacts with the car through a voice-first AI assistant.

After login, the user can say **“Hey Rhasspy”** to wake the assistant. The assistant listens to the command, understands the user intent, updates the relevant cabin or infotainment feature, and returns a spoken response.

Example command:

```text
Hey Rhasspy, set the temperature to 22 degrees.
```

The system can handle commands related to:

- climate control;
- fan speed;
- seat heating and cooling;
- ambient lighting;
- music search and playback;
- navigation and route simulation;
- driving simulation actions;
- general in-car follow-up requests.

---

## Core Problem

Modern car dashboards often require drivers to navigate through screens, menus, and touch controls for simple comfort or infotainment actions. This can make the interaction slower, less natural, and potentially distracting during driving.

This project explores a more natural interaction model: users speak to the car, and the system routes their request to the correct dashboard feature.

---

## Project Goals

- Reduce manual dashboard interaction for common non-driving actions.
- Provide a natural-language interface for cabin and infotainment controls.
- Demonstrate a full AI command pipeline from speech to dashboard state update.
- Build a cockpit-style UI that reflects state changes in real time.
- Combine face recognition, voice AI, cabin controls, music, and navigation in one experience.
- Keep the design focused on controlled, non-driving commands.

---

## Feature Overview

| Area | Functionality |
| --- | --- |
| Authentication | Manual login, registration, face recognition login, profile loading |
| Voice Assistant | Wake-word activation, browser microphone recording, transcription, intent routing, spoken response |
| Cabin Climate | Temperature control, fan speed, persisted cabin state |
| Seat Comfort | Seat heating/cooling and comfort-related controls |
| Ambient Lights | Configurable ambient light strips and visual dashboard glow |
| Music | YouTube-backed music search/player support and voice music intents |
| Navigation | Mapbox navigation view, destination search, route visualization, driving/parked views |
| Settings | Assistant voice, theme/profile details, and user preferences |
| Persistence | SQLite data, user profiles, ambient-light config, music showcase config, TTS cache |
| Developer Tooling | Docker-based startup, local backend/frontend development, tests, linting, production build |

---

## Screenshots

The following screenshots show the main application screens and demonstrate the project functionality.

### Login Page

Users can log in manually or start the face-recognition authentication flow.

<img width="1277" height="676" alt="Login page" src="https://github.com/user-attachments/assets/d49fbb7d-38c1-4ecb-af79-86affde94213" />

---

### Register Page

Users can create/enroll a profile for the dashboard experience.

<img width="1278" height="676" alt="Register page" src="https://github.com/user-attachments/assets/2e2be20b-c51b-4726-953e-6b98195e7baa" />

---

### Main Dashboard — Hidden Mode Enabled

The main cockpit dashboard with the interface in a reduced/hidden mode.

<img width="1279" height="679" alt="Main dashboard with hidden mode enabled" src="https://github.com/user-attachments/assets/dd8748aa-9b7c-4cb5-abb0-51f38f839785" />

---

### Main Dashboard — Hidden Mode Disabled

The main dashboard view with visible cabin and infotainment widgets.

<img width="1276" height="678" alt="Main dashboard with hidden mode disabled" src="https://github.com/user-attachments/assets/3d1af3f2-ca43-4563-91b3-2735a240ca7b" />

---

### Main Dashboard — Ambient Light Glow Enabled

The dashboard supports ambient-light visualization and cabin mood feedback.

<img width="1280" height="669" alt="Main dashboard with ambient light glow enabled" src="https://github.com/user-attachments/assets/51fb2f90-51c0-48ca-a1a0-ae391f6590ec" />

---

### Main Dashboard — Music and Maps Active

The assistant can coordinate multiple modules such as music playback and navigation.

<img width="1280" height="674" alt="Main dashboard with music and maps active" src="https://github.com/user-attachments/assets/9ace63ea-59c5-4250-b2ef-0c07de9992f0" />

---

### Seat Comfort Page

Dedicated seat-comfort controls for heating/cooling and comfort interactions.

<img width="1280" height="675" alt="Seat comfort page" src="https://github.com/user-attachments/assets/8ec0fd06-ebb4-4337-b14c-1a37689d7a99" />

---

### Ambient Light Page

Configurable ambient-light page for adjusting cabin lighting behavior.

<img width="1280" height="672" alt="Ambient light page" src="https://github.com/user-attachments/assets/455f24ea-2b90-4f38-bbce-a188adcc2199" />

---

### Music Page

Music search and playback interface integrated into the in-car assistant experience.

<img width="1280" height="671" alt="Music page" src="https://github.com/user-attachments/assets/c63ecbc3-e8e6-4e8a-abf5-d98a3f2577c5" />

---

### Navigation Page — In Motion

Navigation interface with route visualization and driving-oriented state.

<img width="1280" height="673" alt="Navigation page in motion" src="https://github.com/user-attachments/assets/8a58089e-c85c-465e-8ac6-2ea671be7f14" />

---

### Navigation Page — Parked

Navigation interface shown while the simulated vehicle is parked.

<img width="1280" height="677" alt="Navigation page parked" src="https://github.com/user-attachments/assets/20b09c54-dbf5-4b92-9016-be640f354b5a" />

---

### Settings Page

Settings page for assistant voice, user profile details, theme, and preferences.

<img width="1280" height="679" alt="Settings page" src="https://github.com/user-attachments/assets/64336115-1dd9-4e2e-92f7-dd98cf44f328" />

---

### Rhasspy History Page

History view for previous assistant commands and interactions.

<img width="1280" height="670" alt="Rhasspy history page" src="https://github.com/user-attachments/assets/eb51c6d9-83a2-4fbe-8e87-91bad48961ca" />

---

## Voice Command Flow

The assistant uses a wake-word system to start command capture. Once **“Hey Rhasspy”** is detected, the browser records the user command and sends it to the backend.

The backend transcribes the audio, sends the transcript and cabin context through the AI intent flow, and then passes the result to an orchestration layer.

The orchestrator performs tool routing, selects the correct domain action, updates the system state, persists the result where needed, and returns a spoken response.

```text
Face recognition / login
        ↓
Wake-word detection
        ↓
Browser audio capture
        ↓
Audio upload to backend
        ↓
Speech-to-text transcription
        ↓
AI intent understanding
        ↓
Intent-to-tool routing
        ↓
Domain tool invocation
        ↓
Action execution
        ↓
State persistence
        ↓
Dashboard synchronization
        ↓
Spoken assistant response
```

---

## Example Interaction

```text
User: "Hey Rhasspy, set the temperature to 22 degrees."

1. The wake-word listener activates the assistant.
2. The browser records the user command.
3. The backend transcribes the audio.
4. The AI layer identifies a climate-control intent.
5. The orchestrator routes the request to the climate module.
6. The cabin temperature state is updated.
7. The dashboard UI reflects the new temperature.
8. The assistant responds with spoken confirmation.
```

---

## Technical Architecture

The project is structured around a frontend dashboard, a backend orchestration layer, AI services, domain-specific modules, and local persistence.

```text
React/Vite Dashboard
  ├─ Login / Register
  ├─ Face recognition UI
  ├─ Main dashboard
  ├─ Voice assistant views
  ├─ Climate and seat comfort pages
  ├─ Ambient lights page
  ├─ Music page
  ├─ Navigation page
  └─ Settings page

FastAPI Backend
  ├─ Authentication and profile handling
  ├─ Voice command endpoints
  ├─ Wake-word stream integration
  ├─ AI transcription and intent orchestration
  ├─ Domain tool routing
  ├─ Cabin state persistence
  ├─ Music and navigation integrations
  └─ Generated audio/cache handling

AI / External Services
  ├─ Speech-to-text transcription
  ├─ Intent understanding
  ├─ Text-to-speech response generation
  ├─ Map/navigation service integration
  └─ Music search/player integration

Persistence / Configuration
  ├─ SQLite database
  ├─ User profile snapshot
  ├─ Ambient-light configuration
  ├─ Music showcase configuration
  ├─ Uploaded profile assets
  └─ TTS response cache
```

---

## Technologies Used

### Frontend

- React
- Vite
- TypeScript / JavaScript frontend tooling
- Nginx serving inside the Docker stack
- Browser microphone recording
- Dashboard-style UI modules for cabin, music, maps, seats, lights, and settings

### Backend

- Python
- FastAPI
- Uvicorn
- SQLite
- Pytest
- Docker / Docker Compose

### AI & Voice

- Wake-word detection with openWakeWord
- Browser-based audio capture
- OpenAI transcription flow
- OpenAI Responses-based intent handling
- OpenAI text-to-speech responses
- Persistent TTS MP3 cache for repeated phrases
- Assistant voices configured through the app settings

### Face Recognition

- InsightFace embeddings
- Face enrollment
- Face login
- Profile-based dashboard personalization

### Navigation

- Mapbox-powered map and navigation views
- Destination search
- Route visualization
- Driving/parked route simulation behavior

### Music

- YouTube-backed music search/player support
- Voice music intents
- Persistent music showcase configuration

### Persistence & Config

- SQLite local database
- Portable user/profile snapshot
- Ambient-light layout configuration
- Music slideshow/showcase configuration
- Uploaded assets directory
- Generated TTS cache directory

---

## Main Application Routes

The private implementation includes the following main routes:

| Route | Purpose |
| --- | --- |
| `/login` | Face recognition, enrollment, and profile selection |
| `/` | Main dashboard home |
| `/voice` | Voice assistant view |
| `/heyrhaaspy` | Microphone/wake-word assistant page |
| `/heyrhasspy` | Alternative wake-word assistant route |
| `/ambientlights` | Ambient light controls |
| `/music` | YouTube-backed music page |
| `/maps` | Mapbox navigation |
| `/seats` | Seat comfort controls |
| `/settings` | User and assistant settings |

---

## Internal Project Structure

The private source repository is organized around the following structure:

```text
.
├── compose.yaml                # Docker Compose stack for backend + frontend
├── Dockerfile                  # Backend image
├── main.py                     # FastAPI/Uvicorn entrypoint
├── requirements.txt            # Backend Python dependencies
├── users.json                  # Portable user/profile snapshot
├── config/
│   ├── ambient_lights.json     # Ambient-light strip layout and render settings
│   └── music_showcase.json     # Music page slideshow queue
├── server/                     # FastAPI app, routers, services, database code
├── frontend/                   # React + Vite cockpit UI
├── scripts/                    # Start/stop and data utility scripts
├── tests/                      # Backend pytest suite
├── db/                         # Local SQLite data, ignored by git
└── uploads/                    # Uploaded assets and generated audio cache, ignored by git
```

This public repository does not include the private implementation files listed above. They are documented here to explain the real architecture of the working application.

---

## Data, Assets, and Cache

The private implementation handles several local data sources:

- SQLite database for local persistence.
- Portable user/profile snapshot for demo data.
- Ambient-light strip layout configuration.
- Music slideshow/showcase configuration.
- Uploaded profile photos and generated assets.
- Cached TTS audio responses to speed up repeated assistant replies.
- Mock/demo data initialization for local development.

Sensitive files, generated files, local databases, user uploads, and caches are intentionally excluded from this public overview.

---

## Verification and Quality Checks

The private implementation includes development checks such as:

- backend test suite with Pytest;
- frontend linting;
- TypeScript/project build checks;
- Vite production build;
- Docker-based local stack verification.

---

## Security and Safety Considerations

Because this project simulates an in-car assistant, the assistant should not directly execute raw speech commands without interpretation and validation.

The design uses an orchestration layer between the user command and the dashboard state update. The assistant interprets the request, routes it to a controlled domain tool, and then applies only supported actions.

Important safety/security considerations:

- Commands are interpreted before execution.
- Domain tools restrict what the assistant can control.
- The prototype focuses on non-driving features.
- Sensitive or unsafe actions should require validation.
- Commands that could be unsafe while driving should be blocked.
- API keys and secrets are never included in the public repository.
- Local databases and uploaded user assets are not included.
- Generated TTS cache files are not included.
- Private configuration files are not included.

---

## Future Improvements

Planned or possible improvements include:

- stronger safety guardrails for commands while driving;
- improved command validation;
- personalized presets for music, ambient lights, and comfort settings;
- additional cabin controls such as window height, trunk access, and seat position;
- lower-latency AI responses;
- improved model reliability and command understanding;
- reduced dependency costs for external AI model usage;
- stricter handling of sensitive actions;
- broader test coverage around intent routing and state updates.

---

## Source Code Availability

The full source code is private due to security, API, and intellectual property considerations.

A technical walkthrough, sanitized demo, or selected implementation details can be provided upon request.
