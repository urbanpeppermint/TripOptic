# TripOptic

**TripOptic** is a **Snap Spectacles** lens experience that combines **voice-first trip planning**, **on-device spatial UI**, and **multimodal AI** so travelers can plan, refine, and validate a trip without juggling phone apps in the moment.

---

## What it does

TripOptic walks the user from **first intent** (“I’m going to Lisbon next month for work”) to a **structured trip draft** (departure, destination, dates, purpose), then requests a **single Gemini-powered trip plan** and surfaces it as **interactive category rows** in AR.

### Core capabilities

| Area | Description |
|------|-------------|
| **Voice intake** | Hands-free capture of cities, dates, and trip purpose via speech-to-text, with **deterministic parsing** so common phrases map reliably to `TripDraft` fields before any plan request. |
| **AI trip plan** | One-shot **Google Gemini** (`generateContent`) plan generation through **Remote Service Gateway**, aligned with typed `TripPlanResponse` cards (transport, stay, places, food, weather, pack). |
| **Category detail** | Pinch-to-expand **beta-style detail panels** per category, with formatted prices, options, and practical notes (`CategoryPlanDetailController`). |
| **Pack scan** | **Vision-assisted packing**: capture a frame from the device camera (or editor-safe text fallback), send to Gemini Vision, and show suggestions aligned with real weather when **AccuWeather** is wired (`PackScanController`, `WeatherAccuBridge`). |
| **Live weather** | Optional **AccuWeather** integration so forecasts and summaries can inform copy, packing hints, and prompts. |
| **Keyboard path** | **AR keyboard** flow for users who prefer typing (stepped fields + confirm), documented alongside voice in `Assets/Scripts/TravelPlanner/SCENE_SETUP.md`. |
| **TTS (optional)** | **OpenAI**-backed speech for status, keyboard guidance, and spoken category bodies when `AssistantTtsController` is configured. |
| **Destination feel** | Optional **destination imagery** hook (`DestinationVisualizer`, `NewInCityAssistant`) to reinforce place context after confirmation. |
| **Purpose modes** | **Leisure**, **Business**, and **Bleisure** wired through `TravelPlannerController` and synced into the assistant draft. |

---

## Who it is for (use cases)

1. **Business travelers** — quick voice capture of dates and cities, structured options for transport and accommodation, weather-aware packing.
2. **Bleisure / weekend trips** — blend work constraints with leisure categories (places, restaurants) in one plan surface.
3. **Explorers already in-market** — draft supports **local-only** behavior (e.g. skip long-haul transport) when the user is already at the destination.
4. **Spectacles-first users** — interaction built around **Spectacles Interaction Kit (SIK)** (pinch, toggles, container UI) so the lens remains usable while mobile stays in the pocket.

---

## Why TripOptic (benefits)

- **Calm orchestration** — Voice is parsed into a **stable draft**; Gemini is invoked when the draft is ready, reducing cost, latency, and “LLM roulette” on every utterance.
- **One coherent plan object** — Categories share a single `TripPlanResponse`, keeping UI, TTS, and detail panels aligned.
- **Grounded packing** — Pack checks can combine **what the camera sees** with **real forecast context**, not generic lists.
- **Accessibility** — Parallel **voice** and **keyboard** paths respect different environments (noisy streets vs. quiet rooms).
- **Operator-friendly scene wiring** — `SCENE_SETUP.md` documents bridges (`AIAssistantUIBridge`, mic mute, plan button, pack HUD roots) so teams can reproduce builds.

---

## Achievements (project highlights)

- End-to-end **Gemini trip planning** integrated with **Lens Studio** + **Remote Service Gateway** patterns (correct use of `Gemini.models()` / built-in parameters, not ad-hoc RSM endpoints).
- **Robust speech date handling** — Month/weekday tokens and ASR noise words are excluded from city inference to cut common voice bugs (`GeminiAssistant`).
- **Structured commerce-style fields** — Normalized **price / night / stay** display paths (`PlanPriceFormat`) for consistent accommodation lines.
- **Pack scan session UX** — Session holder, camera preview, capture/close flows, and category navigation coordinated so scan UI does not fight the detail panel (`PackScanController`, `CategoryPlanDetailController`).
- **Weather bridge** — First-party AccuWeather package wiring for **live** context in prompts and UI.
- **Optional TTS layer** — Pluggable narration without blocking core text UI.

---

## Tech stack

- **Snap Lens Studio** / **Spectacles**
- **TypeScript** components (`Assets/Scripts/TravelPlanner/`)
- **Spectacles Interaction Kit (SIK)**
- **Remote Service Gateway** — Google GenAI (Gemini), optional OpenAI for TTS
- **AccuWeather** remote module (see `Packages/Weather - AccuWeather API.lspkg`)

---

## Repository layout

| Path | Role |
|------|------|
| `Assets/Scripts/TravelPlanner/` | Application logic: assistant, UI bridges, pack scan, weather, TTS, types |
| `Assets/Scripts/TravelPlanner/SCENE_SETUP.md` | **Inspector wiring guide** — buttons, text targets, keyboard, TTS, pack HUD |
| `TripOptic.esproj` | Lens Studio project entry |

---

## Getting started

1. Open **`TripOptic.esproj`** in **Lens Studio**.
2. Configure **Remote Service Gateway** credentials for Gemini (and OpenAI if using TTS), per Snap’s RSG documentation.
3. Follow **`Assets/Scripts/TravelPlanner/SCENE_SETUP.md`** to verify scene references (`GeminiAssistant`, `AIAssistantUIBridge`, `ASRQueryController`, `PackScanController`, etc.).
4. Build to **Spectacles** for full microphone, camera, and **AR keyboard** behavior; editor preview has documented limitations (e.g. camera encode, `textInputSystem`).

---

## Credits

- **UI / UX design:** [Forouzan (@forouzan1990)](https://github.com/forouzan1990) — GitHub: [https://github.com/forouzan1990](https://github.com/forouzan1990) · [ArtStation](https://forouzan.artstation.com/)

---

## License

No default license file is shipped in this export. Add a `LICENSE` of your choice before redistributing if you need explicit terms.

---

## Disclaimer

TripOptic generates **informational** travel suggestions. Fares, availability, and entry rules change; users should **verify** bookings and requirements with official sources and providers.
