# TripOptic

**TripOptic** is a **Snap Spectacles** lens that combines **voice-first trip planning**, **spatial AR UI**, and **multimodal AI** so you can shape a trip without juggling phone apps in the moment.

Open the project with **`Travel_Planner.esproj`** in **Lens Studio 5.15+** (Spectacles target).

---

## What it does

TripOptic walks you from **first intent** (“I’m going to Lisbon next month for work”) to a **structured trip draft** (departure, destination, dates, purpose), then requests a **single Gemini-powered trip plan** and surfaces it as **interactive category rows** in AR.

### Core capabilities

| Area | Description |
|------|-------------|
| **Voice intake** | Hands-free capture of cities, dates, and trip purpose via speech-to-text, with **deterministic parsing** so common phrases map reliably to draft fields before any plan request. |
| **AI trip plan** | One-shot **Google Gemini** (`generateContent`) through **Remote Service Gateway**, aligned with typed plan cards (transport, stay, places, food, weather, pack). |
| **Category detail** | Pinch-to-expand **detail panels** per category, with formatted prices, options, and practical notes. |
| **Pack scan** | **Vision-assisted packing**: camera frame (or editor-safe fallback) → Gemini Vision, with suggestions informed by **live AccuWeather** when wired. |
| **Live weather** | Optional **AccuWeather** integration for forecasts, summaries, and pack hints. |
| **Keyboard path** | **AR keyboard** stepped flow for typing instead of voice. |
| **Destination imagery** | Optional **Imagen** preview pushed to **Spatial Image** (`DestinationVisualizer`) after the destination is confirmed. |
| **TTS (optional)** | **OpenAI** narration for status, keyboard guidance, and category bodies when configured. |
| **Purpose modes** | **Leisure**, **Business**, and **Bleisure** through the planner controller and assistant draft. |

---

## Who it is for

- **Business travelers** — quick voice capture of dates and cities, structured transport and stay options, weather-aware packing.
- **Bleisure / weekend trips** — work constraints plus leisure categories in one surface.
- **Explorers already in-market** — local-only behavior when you are already at the destination.
- **Spectacles-first users** — built around **Spectacles Interaction Kit (SIK)** (pinch, toggles, container UI).

---

## Why TripOptic

- **Calm orchestration** — Speech becomes a **stable draft**; Gemini runs when the draft is ready, not on every utterance.
- **One plan object** — Categories share a single structured response so UI, TTS, and detail panels stay aligned.
- **Grounded packing** — Pack checks combine **what the camera sees** with **forecast context**, not generic lists.
- **Voice and keyboard** — Parallel paths for noisy streets vs. quiet rooms.
- **Reproducible wiring** — `Assets/Scripts/TravelPlanner/SCENE_SETUP.md` documents bridges, pack HUD, mic mute, and keyboard.

---

## Tech stack

- **Snap Lens Studio** / **Spectacles**
- **TypeScript** — `Assets/Scripts/TravelPlanner/`
- **Spectacles Interaction Kit (SIK)**
- **Remote Service Gateway** — Google GenAI (Gemini, Imagen), optional OpenAI for TTS
- **AccuWeather** — `Packages/Weather - AccuWeather API.lspkg`

---

## Repository layout

| Path | Role |
|------|------|
| `Assets/Scripts/TravelPlanner/` | Application logic: assistant, UI bridges, pack scan, weather, TTS, types |
| `Assets/Scripts/TravelPlanner/SCENE_SETUP.md` | **Inspector wiring guide** |
| `travel-lens-master-spec.md` | Reference architecture and API notes (documentation only) |
| `Travel_Planner.esproj` | Lens Studio project entry |

---

## Getting started

1. Open **`Travel_Planner.esproj`** in **Lens Studio**.
2. Configure **Remote Service Gateway** credentials for Gemini (and OpenAI if using TTS), per Snap’s RSG documentation.
3. Follow **`Assets/Scripts/TravelPlanner/SCENE_SETUP.md`** to verify scene references (`GeminiAssistant`, `AIAssistantUIBridge`, `ASRQueryController`, `PackScanController`, etc.).
4. Build to **Spectacles** for full microphone, camera, and **AR keyboard** behavior; editor preview has documented limitations (camera encode, `textInputSystem`).

Do **not** commit API keys or `.env` files. Use Lens Studio’s credential UI for RSG.

---

## Roadmap (not shipped in this build)

Ideas from `travel-lens-master-spec.md` that are **not** implemented in the lens today:

- **Fresh-session route & stay lookup** — optional **Internet Module WebView** per category (new session per tap, no persistent cookies).
- **Spatial itinerary timeline** and **flight-day mode** (gate/terminal context).
- **Saved trips** and optional price watch over time.

Transport and stay lines in the UI are **Gemini-generated suggestions** with comparison *pointers* in copy (e.g. “check Skyscanner”), not in-lens booking or live fare APIs.

---

## Credits

- **UI / UX design:** [Forouzan (@forouzan1990)](https://github.com/forouzan1990) · [ArtStation](https://forouzan.artstation.com/)

---

## License

No default license file is included. Add a `LICENSE` before redistributing if you need explicit terms.

---

## Disclaimer

TripOptic generates **informational** travel suggestions. Fares, availability, and entry rules change; **verify** bookings and requirements with official sources and providers.
