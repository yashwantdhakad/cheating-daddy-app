<img width="1299" height="424" alt="cd (1)" src="https://github.com/user-attachments/assets/b25fff4d-043d-4f38-9985-f832ae0d0f6e" />

## Recall.ai - API for desktop recording

If you’re looking for a hosted desktop recording API, consider checking out [Recall.ai](https://www.recall.ai/product/desktop-recording-sdk/?utm_source=github&utm_medium=sponsorship&utm_campaign=sohzm-cheating-daddy), an API that records Zoom, Google Meet, Microsoft Teams, in-person meetings, and more.

This project is sponsored by Recall.ai.

---

> [!NOTE]  
> Use latest MacOS and Windows version, older versions have limited support

> [!NOTE]  
> During testing it wont answer if you ask something, you need to simulate interviewer asking question, which it will answer

A real-time AI assistant that provides contextual help during video calls, interviews, presentations, and meetings using screen capture and audio analysis.

## Features

- **On-Device Whisper Transcription**: Transcribe audio 100% locally and privately (no API costs, no third-party transcription endpoints). Supports different model sizes (Tiny/Base/Small/Medium).
- **Multiple AI Providers (BYOK)**: Supports Anthropic Claude, OpenAI ChatGPT, Google Gemini, and Groq Cloud for answering questions. Simple selection card interface.
- **Screen & Audio Capture**: Analyzes what you see and hear for contextual responses.
- **Multiple Profiles**: Interview, Sales Call, Business Meeting, Presentation, Negotiation.
- **Transparent Overlay**: Always-on-top window that can be positioned anywhere.
- **Click-through Mode (Ghost Mode)**: Make window transparent to clicks when needed.
- **Panic Hide**: Fully hide the window from the dock, taskbar, Mission Control, Alt+Tab, and Task Manager processes with a single keybind.
- **Emergency Erase**: Clear all overlay text instantly.

## Setup

1. **Install Dependencies**: `npm install`
2. **Run the App**: `npm start`
3. **Choose Mode**:
    - **BYOK (Bring Your Own Keys)**: Choose your active provider (Groq, Claude, OpenAI, or Gemini), enter the API key, and configure model options. Audio is transcribed 100% locally using Whisper.
    - **Local AI**: Connect to a local Ollama or LM Studio instance for fully offline/private processing.

## Configuration & Environment Variables

The application supports loading configuration defaults via a `.env` file located in the project root. Copy `.env.example` to `.env` to customize:

- **`ENABLED_PROVIDERS`**: Comma-separated list of visible providers (e.g. `groq,openai` to show only Groq and ChatGPT).
- **`GROQ_API_KEY`**, **`OPENAI_API_KEY`**, etc.: Pre-load API keys so they are active automatically.
- **`ACTIVE_ANSWER_PROVIDER`**: Pre-select the active provider on startup (e.g. `groq` or `openai`).
- **`GROQ_MODEL`**, **`OPENAI_MODEL`**: Override default model selections.

## Keyboard Shortcuts

The app supports custom global shortcuts (configurable in Settings → Shortcuts). Defaults are:

- **Hide / Show Window**: `Cmd+Shift+H` (macOS) / `Ctrl+Shift+H` (Windows) - Fully hide the window from taskbar and task managers. Press again to restore.
- **Toggle Visibility**: `Cmd+\` (macOS) / `Ctrl+\` (Windows) - Show or hide the window.
- **Click-through (Ghost Mode)**: `Cmd+M` (macOS) / `Ctrl+M` (Windows) - Toggle mouse event pass-through.
- **Emergency Erase**: `Cmd+Shift+E` (macOS) / `Ctrl+Shift+E` (Windows) - Wipe all AI content on screen instantly.
- **Ask Next Step**: `Cmd+Enter` (macOS) / `Ctrl+Enter` (Windows) - Take screenshot and query the model for the next step.
- **Window Movement**: `Alt + Arrow Keys` (macOS) / `Ctrl + Arrow Keys` (Windows) - Shift the overlay window position.
- **Scroll AI Responses**: `Cmd+Shift+Up/Down` (macOS) / `Ctrl+Shift+Up/Down` (Windows) - Scroll content in ghost mode.
- **Switch Responses**: `Cmd+[` / `Cmd+]` (macOS) - Cycle through response history.

## Audio Capture

- **macOS**: SystemAudioDump for system audio capture
- **Windows**: Loopback WASAPI audio capture
- **Linux**: Microphone input

## Requirements

- Electron-compatible OS (macOS, Windows, Linux)
- An active API key (Groq, Claude, OpenAI, or Gemini) or a running local inference server (Ollama/LM Studio)
- Screen recording permissions
- Microphone/audio permissions
