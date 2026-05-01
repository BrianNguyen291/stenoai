# StenoAI Improvement Roadmap

Prioritized by impact / effort. Pick anything; items are mostly independent.

## Tier 1 — High impact, low effort (ship in days)

### Auto-update
- Add `electron-updater` to `app/`
- Configure GitHub Releases as feed (already publish DMGs there)
- UI: "Update available" badge → restart-to-update button
- Files: `app/main.js`, `app/package.json`

### Crash reporting
- Sentry SDK (`@sentry/electron`) — free tier covers small user base
- Hook into Electron `crashed`, `unhandled-rejection`, Python stderr fatal errors
- Anonymise user data before send
- Files: `app/main.js`, `simple_recorder.py`

### Settings: keep/delete audio toggle
- Add `keep_audio_files` config bool (default true)
- Pipeline checks flag before keeping or unlinking audio
- Settings UI: AI tab → "Storage" section → toggle
- Files: `src/config.py`, `simple_recorder.py`, `app/index.html`

### Search across meetings
- SQLite FTS5 index over summaries + transcripts + customer_memory
- Sidebar search box → live filter results
- Rebuild index lazily on `list-meetings`
- Files: new `src/search.py`, `simple_recorder.py`, `app/index.html`

### Tags / labels
- Multi-tag per meeting in frontmatter (`tags: [client, follow-up]`)
- Sidebar filter chips
- Bulk tag from context menu
- Files: `simple_recorder.py` (parser/save), `app/index.html`

### Export
- Right-click meeting → Export → Markdown / PDF / Word
- PDF: use `electron.webContents.printToPDF`
- Word: pandoc bundled, or generate via python-docx
- Files: `app/main.js` (IPC), `app/index.html` (menu)

### Audio waveform + scrubbing
- Replace bare `<audio>` with wavesurfer.js
- Click waveform → seek
- Show current playback position synced with timestamped transcript lines
- Files: `app/index.html` (script + UI), `app/package.json` (wavesurfer dep)

### Keyboard shortcuts
- Cmd+R = start/stop recording
- Cmd+K = focus Ask anything
- Cmd+/ = focus search
- Cmd+E = export
- Cmd+, = settings
- Files: `app/index.html` (keymap), `app/main.js` (global accelerator)

### Reprocess with different model
- Reprocess button → dropdown with model picker
- Saves alongside original (don't overwrite)
- Files: `app/main.js`, `simple_recorder.py` (reprocess command)

---

## Tier 2 — Mid effort, big UX wins (1-2 weeks each)

### Person tagging + cross-meeting chat
- See `~/.claude/projects/-Users-macos-Documents-GitHub-stenoai/memory/person_memory_feature_plan.md`
- Tier 1 of that plan = ~2 days. Validates UX before bigger investment.

### Live transcription during recording
- Stream chunks from whisper as audio is captured (chunked transcribe)
- Display growing transcript in record view
- Useful for verification mid-meeting
- Files: `src/audio_recorder.py`, `src/transcriber.py`, `app/main.js`, `app/index.html`

### Mono speaker diarization (opt-in)
- Add `pyannote.audio` behind config flag — NOT default
- Settings: AI → Advanced → "Multi-speaker diarization (large download)"
- Requires HuggingFace token; show clear UX about gated model
- Files: `src/transcriber.py`, `requirements.txt`, `src/config.py`, `app/index.html`

### Editable markdown summary
- Detail view: click summary section → contenteditable
- Save back to `_summary.md` on blur
- Re-parse on save to update structured fields
- Files: `app/index.html`

### Meeting templates
- Predefined types: sales-call, doctor-visit, lecture, 1-on-1, interview, brainstorm
- Each template has its own summary prompt + structure
- Settings UI to manage templates
- Auto-detect template from transcript content (LLM call)
- Files: `src/templates.py` (new), `src/summarizer.py`, `app/index.html`

### Calendar integration deep wiring
- Read user's calendar (already partial UI)
- Pre-fill session name from calendar event
- Attach recording to event metadata
- Files: `app/main.js` (calendar IPC), `app/index.html`

### Multi-language summary
- "Generate also in [language]" button
- Re-runs summarizer with different output language
- Saves both versions in `_summary.md` under separate sections
- Files: `simple_recorder.py`, `app/index.html`

### Mobile companion (recording-only)
- iOS/Android record-only app uploads .m4a to desktop via local network
- Desktop polls or pushes via shared folder
- Out of scope for MVP — flag as future

---

## Tier 3 — Architecture (foundation work)

### Daemon mode for Python backend
- Long-running FastAPI/uvicorn process instead of subprocess per call
- Eliminates Ollama lifecycle bugs (zombie processes)
- Faster IPC (HTTP localhost vs spawn cost)
- Risk: bigger refactor, breaks PyInstaller bundle assumptions
- Files: most of `simple_recorder.py`, `app/main.js` IPC layer

### Frontend framework migration
- `app/index.html` is ~11K lines, no framework — extension is fragile
- Migrate to React/Svelte/Solid + Vite
- Component-by-component rewrite, keep existing IPC contracts
- Risk: months of work, churn during migration
- Files: full `app/` rewrite

### Test coverage
- Currently 2 test files. Target: every src/ module has tests
- Add E2E tests with Playwright for Electron
- CI: GitHub Actions runs tests on every PR
- Files: `tests/`, `.github/workflows/`

### Streaming-resilient saves
- Already partial: streamed_md persisted every 20 chunks
- Extend: recording state checkpointed to disk so crash mid-record doesn't lose audio
- Files: `src/audio_recorder.py`

### Whisper-medium model option
- Settings: AI → Transcription model (small/medium/large)
- Medium: better accuracy, ~3x slower, ~1.5GB
- Files: `src/transcriber.py`, `src/config.py`, `app/index.html`

---

## Tier 4 — Security & privacy

### Recordings folder encryption
- macOS data vault attribute on `~/Library/Application Support/stenoai`
- Or app-level encryption with key in Keychain
- Files: `src/config.py`, native bindings

### Cloud API key in Keychain
- Currently stored in plain text config
- Move to macOS Keychain via `keytar` (Electron) or `keyring` (Python)
- Files: `app/main.js`, `src/summarizer.py`

### Audit log
- Track exports, deletions, key changes, cloud calls
- Stored in `~/Library/Application Support/stenoai/audit.log`
- View in Settings → Privacy
- Files: new `src/audit.py`, integrate at sensitive call sites

---

## Tier 5 — Distribution

### Mac App Store
- Different signing (App Store cert), sandboxing required
- Ollama bundling tricky under sandbox — likely needs cloud-only mode for MAS build
- Two builds: direct DMG (full features), MAS (cloud-only)
- Files: `electron-builder` config

### Windows port
- Build pipeline already abstracts most of it; need Windows-specific:
  - Audio capture (replace ScreenCaptureKit)
  - Bundled Ollama Windows binary
  - Code signing cert (DigiCert, ~$500/yr)
- Files: `bin/` (download Windows binaries), `.github/workflows/build-release.yml`

### Linux port
- Same as Windows minus signing complexity
- AppImage or Flatpak
- Audio: PulseAudio / PipeWire
- Files: `electron-builder` config, audio module

### Pricing tier (if commercial)
- Free: local Ollama only
- Pro ($X/month): cloud LLMs, person memory, sync
- License key validation in `app/main.js`
- Stripe for billing
- Files: new `src/license.py`, `app/main.js`

---

## Tier 6 — "Smart" features (LLM-heavy, speculative)

### Auto-detect meeting type
- Run a tiny classifier prompt on first transcript chunk → pick template
- Files: `simple_recorder.py`, `src/summarizer.py`

### Push action items to calendar/todo apps
- Things 3, Reminders, Todoist via URL schemes
- "Send to Reminders" button per action item
- Files: `app/main.js`, `app/index.html`

### Smart compaction
- After N meetings for the same person, auto-summarise older ones into person profile
- Saves context budget for cross-meeting chat
- Files: `src/person_memory.py` (new)

### Voice commands during recording
- Detect phrases like "mark this", "remind me" → bookmark moment in transcript
- Lightweight wake-word lib or post-process detection
- Files: `src/audio_recorder.py`, `src/transcriber.py`

---

## Bug-fix backlog (known issues)

- 30-min wall-clock timeout in main.js — bump or make configurable for long recordings
- Multiple `audio_path.unlink()` paths previously deleted audio (fixed in current branch)
- `processing-started` events should arrive once per file uploaded (currently per-batch); UI placeholders may flicker
- llama3.2:3b ignores summary template intermittently; fallback parser dumps full text but loses structure — consider local model upgrade default to qwen3.5:9b for users with >8GB RAM
- Whisper transcribes both stereo channels separately, doubling time on stereo uploads — parallel channel transcription would halve wall-clock (see "Speedup options" in earlier session notes)

---

## Cut list (don't bother)

- Full agent framework (LangChain etc.) — overkill until person-memory scale demands it
- WhisperX — replaces working pywhispercpp for marginal accuracy gain
- Real-time speaker diarization on stereo — channel split already gives [You]/[Others] for free
- Native macOS UI rewrite (SwiftUI) — abandon Electron means losing Windows/Linux portability
