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

- **Cloud Whisper Transcription (default)**: Fast, free transcription via Groq's cloud Whisper API — no local download, nothing to go wrong. **On-device Whisper is available but opt-in** (`ENABLE_LOCAL_AI=true` — see Troubleshooting) for fully offline/private processing; supports different model sizes (Tiny/Base/Small/Medium).
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
    - **BYOK (Bring Your Own Keys)**: Choose your active provider (Groq, Claude, OpenAI, or Gemini), enter the API key, and configure model options. Audio is transcribed via Groq's cloud Whisper by default.
    - **Local AI**: Connect to a local Ollama or LM Studio instance for fully offline/private processing. **Requires `ENABLE_LOCAL_AI=true` in `.env`** — see [Local AI Setup](#local-ai-setup-ollama--lm-studio) and [Troubleshooting](#troubleshooting).

## Configuration & Environment Variables

Copy `.env.example` to `.env` in the project root to pre-load API keys for local
development:

- **`GEMINI_API_KEY`**, **`GROQ_API_KEY`**, **`ANTHROPIC_API_KEY`**, **`OPENAI_API_KEY`**: loaded at startup and take priority over a key typed into the app UI.

**`.env` is credentials-only, by design.** Which provider/model actually answers
your questions is controlled entirely by the select fields in Settings, and is
saved to `preferences.json`, not `.env`. Earlier versions let `.env` also set
`ACTIVE_ANSWER_PROVIDER` / `ENABLED_PROVIDERS` / `GROQ_MODEL` / `OPENAI_MODEL` —
that was removed because it silently overwrote your UI selection on every
restart (pick OpenAI in Settings, quit and relaunch, and it would silently
revert to whatever `.env` said). If you're editing this codebase, don't
reintroduce env-var overrides for *behavior* — only for *credentials*.

## Local AI Setup (Ollama / LM Studio)

> **Requires `ENABLE_LOCAL_AI=true` in `.env`** — disabled by default (see
> [Troubleshooting](#troubleshooting)). Without it, choosing Local AI in
> Settings will show "Local AI is disabled" instead of starting a session.

Local mode runs everything on your machine — no API keys, no per-request cost,
fully offline. Once enabled, in Settings choose **Local AI** and pick a backend:

- **Ollama**: install from [ollama.com](https://ollama.com), then `ollama pull <model>` (e.g. `gemma3:4b`, `llama3.1`). Point the app at `http://127.0.0.1:11434` (default) — the model dropdown refreshes live from whatever you have pulled.
- **LM Studio**: load a model in LM Studio, then **start its local server** (Developer tab → Start Server). The model in LM Studio's chat window being "loaded" is not enough — the *server* has to be running for this app to reach it, or you'll see "Cannot reach LM Studio (is the local server started?)". Default URL: `http://127.0.0.1:1234`.

**Hybrid mode** (Answer Engine → "Groq / Claude API" while in Local AI mode):
transcription still runs 100% on-device via Whisper, but answers are generated
by your configured Groq/Claude/OpenAI key instead of the local model. Use this
when local model generation feels too slow but you still want private,
on-device transcription with no Gemini involved at all.

## Ghost Mode (Click-through)

Press `Cmd+M` / `Ctrl+M` to toggle the overlay between two states:

- **Off (default)**: the window is a normal window — drag it by the top bar, click buttons, type in the input box.
- **On**: every click and keystroke passes straight through to whatever is behind the overlay (Zoom, your notes, a browser). The overlay becomes read-only — use it to glance at an answer while typing somewhere else. Press the shortcut again to get interaction back.

**Linux:** click-through relies on `setIgnoreMouseEvents(true, { forward: true })`,
which is not reliably supported on Linux window managers. On Linux, ghost mode
may leave the overlay fully click-through with no way to interact until you
toggle it off again via the keyboard shortcut — there is currently no
Linux-specific fallback. macOS and Windows are unaffected.

## Troubleshooting

**Local AI is disabled by default.** The native on-device Whisper worker (used
for local transcription and by the Ollama/LM Studio "Local AI" mode) has
caused real crashes on some Windows machines — most often
`"Protobuf parsing failed"`, meaning the Whisper model file downloaded
corrupted or incomplete (common on unstable/corporate-proxied connections, or
antivirus interfering with the download mid-write). Recovering by re-trying
another local model over the same broken connection just crash-loops.

Because of this, **local AI requires an explicit opt-in**: set
`ENABLE_LOCAL_AI=true` in `.env` to use it. With it unset (the default), the
app never starts the native Whisper worker or the Ollama/LM Studio local
mode — transcription always goes through Groq's cloud Whisper instead, which
requires a `GROQ_API_KEY` in `.env` or entered in Settings. If you see
`"Local transcription is disabled..."` in the status bar, that's this switch —
either add a Groq key, or set `ENABLE_LOCAL_AI=true` if you specifically want
on-device/offline processing and have verified it's stable on your machine.

If you enable it and hit the Protobuf error, manually delete the
`whisper-models` folder shown in the error log and retry on a more stable
connection — or just leave the switch off and use Groq's cloud Whisper, which
doesn't touch this code path at all.

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
