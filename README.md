# Whisper Dictation, a Voice Typing Tool by Firsh

Press a hotkey, speak, and your words appear at the cursor. Free and offline.

[**Download latest release**](https://github.com/Firsh/whisper-dictation/releases/latest)

## What it does

- **Press a hotkey** and start dictating
- **Speech is transcribed locally** using OpenAI Whisper AI (large-v3-turbo model)
- **Text is pasted at your cursor** position and is available on the clipboard
- **Works in any application**
- **Private**, no cloud, no API keys, meaning all processing is local: no data sent to any server after initial model download
- Any language Whisper recognises: primary and secondary language are configurable during setup
- Batch transcription: drag and drop audio files onto a GUI window
- All your recordings are kept along with transcriptions, available for retry, and can be converted to opus for archival in monthly packages

## Why

I first started out with Wispr Flow then tried the then-free alternative, WhisperTyping. However, these tools became paid and I was really fed up when they didn't work, losing some dictations, or the fact that my voice was sent to the cloud and got transcribed there. Even if inference is faster when using a provider, simply the latency and privacy concerns made me want to write a local version without any fancy graphical interface. Basically I wanted the most minimal and performant thing that just gets the job done. I have used it extensively over a month with Claude Code, writing this tool with itself, polishing it adhering to this mindset. So, this became my daily driver as it's really simple but effective.

## System requirements

- Windows 10 or 11, 64-bit
- NVIDIA GPU recommended (CUDA 12.4 or 11.8 auto-detected) with any reasonable amount (like 8GB) of VRAM, as the model isn't big at all
- Fast, preferably NVME SSD
- Microphone obviously
- ~2 GB disk space for the AI model and tooling
- Internet connection only for first-time installation

## Installation

1. Extract the ZIP to any folder (this is portable)
2. Right-click `install.ps1` → `Run with PowerShell` (or the executable also runs this for you)
3. The installer detects your GPU, installs aria2c for fast downloads, procures the right Whisper.cpp build, FFmpeg, and the AI model, asks for your primary and secondary language (ISO codes, e.g. `en`, `hu`, `de`), detects your microphone, and writes the config file for you
4. Double-click `firsh-whisper-dictation.exe` to start (it is a built AHK script)
5. If you like it, set `Run at Startup` from the tray icon (copies a shortcut to your shell:startup folder)

## How to use

1. Press `AppsKey` (the context-menu key, between right Alt and right Ctrl)
2. Speak: the tray icon shows `Listening...` while recording
3. Press `AppsKey` again to stop
4. The transcription is pasted at your cursor and gets copied to the clipboard

## Hotkeys

All of these are toggles, not push-to-talk:

| Key            | Action                                         |
| -------------- | ---------------------------------------------- |
| `AppsKey`      | Record in primary language / stop              |
| `Ctrl+AppsKey` | Record in secondary language / stop            |
| `Win+AppsKey`  | Retry last recording with auto-detect language |

Note:

- There is no separate cancel key
- Retry is useful if you accidentally spoke in the other language and the result is weird or unexpected
- If you speak your secondary language while you started transcription using the primary, it gets translated on the fly, as a happy little accident

## Batch transcription

1. Right-click the tray icon and `Show Transcribe Window`
2. Drag and drop audio files (MP3, WAV, M4A, OGG, FLAC) onto it
3. Wait a bit. Each file is transcribed and saved as a `.txt` next to the original.

Note: Text is automatically split into paragraphs deterministically, but additional processing by an LLM may be required for best results.

## Configuration

Editing `config.ini` is optional, as is auto-created on first run:

- `RecordingsPath` is where audio files are saved (default: `Documents\Voice Typing\`)
- `MicrophoneName` is set manually if auto-detection picks the wrong device
- `EnableLogging` writes to `stt.log` for troubleshooting
- `DevMode` saves raw and processed output files side-by-side
- `PrimaryLanguage` / `SecondaryLanguage` ISO codes for your two languages (e.g. `en`, `hu`, `de`, `fr`)
- `VolumeFadeDuration` adjusts audio ducking duration in milliseconds while recording
- `PrimaryPrompt` / `SecondaryPrompt` is the Whisper punctuation hint; built-in for English and Hungarian, empty for others (write your own if needed)

Use the `Reload Script` from the tray menu after making changes.

## Archive recordings

The tray menu's `Archive Recordings` converts old WAV recordings to 32 kbps Opus (16 kHz mono). Saves roughly 90% disk space, becoming very cheap to store. Files are packed into monthly archives (RAR with recovery record, or ZIP as fallback). Transcription text files are kept as-is. Or you could delete them any time, the `Open Recordings Folder` tray menu option gets you there.

## Troubleshooting

- **Microphone not found** → set `MicrophoneName` in `config.ini` or run the installer again, or just connect (turn on) your microphone you used during installation
- **No speech detected** → check microphone volume in Windows settings
- **Slow** → The AI model takes a moment to load into GPU memory for every dictation job (then it unloads to not hog it). This is where a fast SSD shines!
- **GPU not used** → installer falls back to a CPU-optimized build automatically; check `stt.log` if `EnableLogging=true`

## Support

Feedback? Chat with me on bsky: [@firsh.dev](https://bsky.app/profile/firsh.dev)

**Enjoying my Whisper Dictation tool?** If you find it useful, [buy me a tea](https://buy.stripe.com/6oUcN44fycIi6xocIP0sU01) 🍵 via Stripe or support directly using crypto. Completely optional, but appreciated!

- **BTC** `bc1qwm8q89vl3hxzfugztf7p744hnuky7f7vk2s0vt`
- **LTC** `ltc1qehgstdq043fhy7th4gf8rs76gprd6vw54y3dqa`
- **DOGE** `DLPbYnt5afGzK9GQEYkKVt8qGpq5ESCu3M`

## Credits

This project stands on the shoulders of excellent open-source work:

- [OpenAI Whisper](https://github.com/openai/whisper) — the speech recognition model behind everything
- [whisper.cpp](https://github.com/ggml-org/whisper.cpp) by ggml-org — the fast C++ inference engine with CUDA support used here
- [ggml-large-v3-turbo model](https://huggingface.co/ggerganov/whisper.cpp) on HuggingFace — the GGML-quantised model weights
- [FFmpeg](https://github.com/BtbN/FFmpeg-Builds) (BtbN builds) — audio conversion and processing
- [AutoHotkey v2](https://www.autohotkey.com/) — the scripting runtime the dictation tool is built on
- [aria2c](https://aria2.github.io/) — multi-threaded downloader used by the installer for fast dependency downloads
