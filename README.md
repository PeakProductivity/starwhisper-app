# StarWhisper — Offline AI Voice-to-Text for Windows

StarWhisper is a Windows desktop application that converts speech to text using OpenAI's Whisper AI model running entirely on your local machine. It works completely offline — no internet connection required, no cloud processing, no data collection. The app sits as a floating widget on your screen and can transcribe your voice into any application: Word documents, emails, browsers, code editors, chat apps, or anything else. For users who want maximum accuracy, StarWhisper can also connect to OpenAI's cloud Whisper API. The free tier includes 500 words per day with the Tiny model; Pro ($10/month or $80/year) unlocks unlimited transcriptions, larger models (Medium, Large), file transcription, wake word activation, and OpenAI API access.

**Website:** https://starwhisper.ai
**Download:** https://starwhisper.ai/downloads/StarWhisper-Setup-Full.exe
**Microsoft Store:** https://apps.microsoft.com/store/detail/9NXRF1N01VSN
**Documentation:** https://starwhisper.ai/docs/
**Current Version:** 1.3.117

---

## Table of Contents

1. [Key Features](#key-features)
2. [Why StarWhisper?](#why-starwhisper)
3. [How It Works](#how-it-works)
4. [Supported Models](#supported-models)
5. [Supported Languages](#supported-languages)
6. [Privacy and Security](#privacy-and-security)
7. [System Requirements](#system-requirements)
8. [Installation](#installation)
9. [Pricing](#pricing)
10. [Use Cases](#use-cases)
11. [FAQ](#faq)
12. [Comparison with Competitors](#comparison-with-competitors)
13. [Technical Details](#technical-details)
14. [Screenshots](#screenshots)
15. [Links and Resources](#links-and-resources)

---

## Key Features

### Core Transcription
- **Offline transcription** — All AI processing happens locally on your PC using whisper.cpp. No internet connection needed after initial model download.
- **99%+ accuracy** — Powered by OpenAI's Whisper model, the same technology used in professional transcription services.
- **Real-time transcription** — Words appear on screen as you speak via a floating overlay. Adjustable chunk size lets you see partial results instantly.
- **Auto-paste** — Transcribed text is automatically copied to clipboard and pasted into whatever window you're working in. Zero extra steps.
- **Global hotkey** — Press any configurable key combination (default: CapsLock or Ctrl+Space) to start/stop recording from any application without switching windows.

### AI Models
- **Five model sizes** — Tiny (75MB), Base (142MB), Small (466MB), Medium (1.5GB, Pro), Large (2.9GB, Pro).
- **GPU acceleration** — NVIDIA GPU support (GTX 900 series or newer) delivers 5–10x faster transcription using CUDA.
- **OpenAI Whisper API** — Pro users can route transcriptions through OpenAI's cloud for maximum accuracy (requires OpenAI API key).

### Productivity Features
- **Custom vocabulary** — Add domain-specific terms, names, brands, and technical jargon. Fuzzy matching auto-corrects similar-sounding variations.
- **Text macros** — Define short spoken phrases that expand into full text blocks. Say "sig" and your complete email signature appears.
- **Voice commands** — Say "new paragraph", "comma", "period", "delete last word", or "backspace" during dictation for hands-free formatting.
- **Wake word activation** (Pro) — Set a custom trigger phrase (e.g., "Hey StarWhisper") to start recording without pressing any key.
- **AI text formatting** (Pro) — Automatically reformat raw transcription as an email, formal letter, bullet-point notes, or paragraph prose.

### File Transcription (Pro)
- **Batch file processing** — Drop audio or video files onto the app and receive a full text transcript.
- **Supported formats** — MP3, M4A, WAV, FLAC, OGG, MP4, MKV, MOV, AVI, WebM.
- **Long-form support** — Files up to 1 hour in length.
- **Use cases** — Transcribe recorded interviews, meeting recordings, podcast episodes, lecture recordings, voice memos.

### App Experience
- **Floating widget** — A small draggable circle sits above all other windows. It pulses orange when recording. Move it anywhere on screen.
- **14 visual themes** — Choose preset gradient color schemes or create custom themes for each recording state.
- **Transcription history** — All transcriptions saved locally with timestamps. Searchable. Exportable as plain text or Markdown.
- **Cloud sync** — Vocabulary, macros, and settings sync across devices when signed in (Google or email sign-in).
- **Multiple languages** for the app interface — English, German, French, Spanish, Portuguese, Japanese, Korean, Chinese, Russian, Italian, and Dutch.

---

## Why StarWhisper?

### The Core Problem It Solves

Most voice-to-text tools send your audio to a remote server. Dragon NaturallySpeaking requires annual subscriptions costing $300–$600. Otter.ai, Fireflies, and similar tools are meeting-focused SaaS products not designed for general desktop dictation. Windows' built-in voice typing is limited to specific apps and has no customization. StarWhisper addresses all of these gaps: it runs locally, works in every application, costs far less, and gives you fine-grained control over accuracy and privacy.

### Key Differentiators

1. **True offline operation** — Once models are downloaded, StarWhisper works with no internet forever. Sensitive industries (healthcare, legal, finance) benefit from guaranteed data isolation.
2. **Works in every Windows application** — Unlike browser extensions or app-specific tools, StarWhisper's global hotkey and auto-paste work in any window: VS Code, Word, Excel, Gmail, Teams, Slack, terminal, games, anything.
3. **Whisper-quality accuracy** — Uses the same underlying model as professional transcription services, not older acoustic models or inferior APIs.
4. **No usage tracking on your content** — Transcriptions are stored only on your local machine at `%AppData%\StarWhisper\history.json`. StarWhisper servers never receive your voice or your text.
5. **Affordable pricing** — Free tier for casual use. Pro at $10/month is a fraction of Dragon's cost with comparable accuracy.

---

## How It Works

StarWhisper uses **whisper.cpp**, an open-source C++ port of OpenAI's Whisper model, as its transcription engine. Here is the full processing pipeline:

### Step 1: Audio Capture

When you press the hotkey or click the recording circle, StarWhisper opens your selected microphone via the Windows audio API (WASAPI). Audio is captured at 16kHz mono, which is the sample rate Whisper was trained on.

### Step 2: Voice Activity Detection

StarWhisper uses lightweight voice activity detection (VAD) to detect silence and optionally auto-stop recording after a configurable silence threshold. This prevents empty recordings and saves processing time.

### Step 3: Local Inference (whisper.cpp)

The captured audio is passed to whisper.cpp. The inference runs on your CPU or NVIDIA GPU (CUDA). whisper.cpp is a highly optimized implementation that achieves near-real-time transcription even on mid-range hardware. No data leaves your machine in this mode.

The processing steps inside whisper.cpp:
1. Audio is converted to a log-mel spectrogram (80 mel filter banks over 30-second windows).
2. The spectrogram is encoded by the Whisper encoder (a transformer architecture).
3. The decoder generates token probabilities and outputs the most likely transcription.
4. Optional language detection runs if you have set language to "auto".

### Step 4: Post-Processing

The raw transcription text is passed through StarWhisper's post-processing pipeline:
- Custom vocabulary substitution (exact matches and fuzzy matches)
- Macro expansion (short phrases → full text blocks)
- Voice command interpretation ("new paragraph" → insert `\n\n`)
- AI formatting (optional, Pro only — sends text to OpenAI GPT for reformatting)

### Step 5: Output

The final text is copied to the Windows clipboard and, if auto-paste is enabled, StarWhisper sends a Ctrl+V keystroke to the previously active window. The transcription is also appended to the local history file.

### Cloud Mode (Optional)

Pro users can configure an OpenAI API key. When cloud mode is active, the captured audio (WAV format) is sent to OpenAI's `whisper-1` API endpoint instead of running local inference. Cloud mode offers the highest accuracy but requires internet and uses your OpenAI API credits. Audio is not stored by OpenAI after processing.

---

## Supported Models

StarWhisper bundles whisper.cpp and allows you to download any of the standard Whisper model sizes. Larger models are slower but more accurate.

| Model  | File Size | RAM Required | Speed (CPU) | Speed (GPU) | Accuracy  | Plan Required |
|--------|-----------|--------------|-------------|-------------|-----------|---------------|
| Tiny   | 75 MB     | ~1 GB        | ~10 sec/min | ~2 sec/min  | ~85%      | Free          |
| Base   | 142 MB    | ~1 GB        | ~7 sec/min  | ~1.5 sec/min| ~90%      | Free          |
| Small  | 466 MB    | ~2 GB        | ~5 sec/min  | ~1 sec/min  | ~94%      | Free          |
| Medium | 1.5 GB    | ~4 GB        | ~15 sec/min | ~3 sec/min  | ~96%      | Pro           |
| Large  | 2.9 GB    | ~8 GB        | ~30 sec/min | ~5 sec/min  | ~98%      | Pro           |

**Notes:**
- Speed figures are approximate for a 1-minute audio clip on a modern CPU (Intel Core i7/Ryzen 7) and a mid-range NVIDIA GPU (RTX 3060).
- The "Small" model is recommended for most users: it fits in RAM easily, runs in near-real-time on CPU alone, and achieves 94% accuracy which is sufficient for natural dictation.
- The "Large" model provides near-human-level accuracy and is ideal for professional transcription of recorded content, interviews, and meetings.
- All model files are standard GGML format compatible with whisper.cpp.
- The GPU pack (~528 MB) enables CUDA acceleration and must be downloaded separately if you have an NVIDIA GPU.

### Downloading Models

Models are downloaded from within the application:
1. Right-click the floating circle → Settings → Transcription tab
2. Under "Local Model", click the download button next to your desired model
3. The model downloads directly from Hugging Face model hub

---

## Supported Languages

StarWhisper supports all languages that Whisper supports — **99 languages** for transcription. The app interface itself is available in 11 languages.

### Transcription Languages (99 total)

Whisper was trained on 680,000 hours of multilingual audio. The following languages have strong support (>90% accuracy on clear audio):

**Tier 1 — Highest Accuracy**
Afrikaans, Arabic, Armenian, Azerbaijani, Belarusian, Bosnian, Bulgarian, Catalan, Chinese (Simplified), Chinese (Traditional), Croatian, Czech, Danish, Dutch, English, Estonian, Finnish, French, Galician, German, Greek, Hebrew, Hindi, Hungarian, Icelandic, Indonesian, Italian, Japanese, Kannada, Kazakh, Korean, Latvian, Lithuanian, Macedonian, Malay, Marathi, Maori, Nepali, Norwegian, Persian, Polish, Portuguese, Romanian, Russian, Serbian, Slovak, Slovenian, Spanish, Swahili, Swedish, Tagalog, Tamil, Thai, Turkish, Ukrainian, Urdu, Vietnamese, Welsh

**Tier 2 — Good Accuracy**
Albanian, Amharic, Assamese, Bashkir, Breton, Burmese, Faroese, Gujarati, Haitian Creole, Hausa, Hawaiian, Javanese, Lao, Lingala, Luxembourgish, Maltese, Occitan, Pashto, Punjabi, Sanskrit, Shona, Sindhi, Sinhala, Somali, Sundanese, Tibetan, Turkmen, Uzbek, Yoruba

**App Interface Languages**
English, German, French, Spanish, Portuguese, Japanese, Korean, Chinese (Simplified), Russian, Italian, Dutch

### Language Detection

If you set the language to "Auto-detect", Whisper will identify the spoken language in the first 30 seconds of audio and proceed accordingly. Setting an explicit language improves accuracy by 2–3 percentage points and reduces processing time.

---

## Privacy and Security

Privacy is a core design principle of StarWhisper. Here is exactly what happens to your data:

### In Local Mode (Default)

- **Audio is never transmitted** — Raw audio captured from your microphone is held in RAM only during transcription. It is never written to disk as a recording file and never sent to any network endpoint.
- **Transcriptions are stored locally only** — All transcription output is saved to `%AppData%\StarWhisper\history.json` on your own computer. This file is never uploaded.
- **No analytics on your content** — StarWhisper does not track what you say, what text is produced, or what applications you use.
- **Account data is minimal** — If you sign in (optional for basic use), only your email address and subscription status (Free/Pro) are stored on StarWhisper's servers.

### In Cloud Mode (Optional, Pro)

- Audio is sent to OpenAI's `whisper-1` API endpoint over HTTPS.
- OpenAI does not store audio after processing (per OpenAI's API data policy).
- No audio or text is stored on StarWhisper's servers.
- Cloud mode is opt-in and requires you to supply your own OpenAI API key.

### HIPAA Compliance

When using **Local Mode**, StarWhisper is suitable for HIPAA-sensitive environments. Patient data (spoken or transcribed) never leaves the local machine. Cloud mode is NOT HIPAA compliant.

### What StarWhisper Collects (Full List)

| Data Type               | Collected? | Storage Location  | Purpose                     |
|-------------------------|------------|-------------------|-----------------------------|
| Voice recordings        | No         | —                 | —                           |
| Transcription text      | No (server)| Local disk only   | User's own history          |
| OpenAI API key          | No (server)| Local disk only   | Stored encrypted locally    |
| Email address           | Yes        | StarWhisper server| Account identification      |
| Subscription status     | Yes        | StarWhisper server| Feature gating              |
| App version             | Yes        | StarWhisper server| Update delivery             |
| Anonymous crash reports | Yes (opt-in)| StarWhisper server| Bug fixes                  |

---

## System Requirements

### Minimum Requirements

| Component  | Requirement                               |
|------------|-------------------------------------------|
| OS         | Windows 10 (64-bit, version 1903 or later)|
| CPU        | Intel Core i5 (6th gen) or AMD Ryzen 5    |
| RAM        | 4 GB                                      |
| Disk Space | 500 MB (app) + model size (75 MB–2.9 GB) |
| Microphone | Any Windows-compatible microphone         |
| Internet   | Required for download only (not for use)  |

### Recommended Requirements

| Component  | Recommendation                            |
|------------|-------------------------------------------|
| OS         | Windows 11 (64-bit)                       |
| CPU        | Intel Core i7/i9 or AMD Ryzen 7/9        |
| RAM        | 8 GB or more                              |
| GPU        | NVIDIA GTX 1060 or newer (CUDA support)  |
| Disk Space | 2 GB (app + GPU pack + Small model)      |
| Microphone | USB condenser mic or headset             |

### GPU Acceleration Requirements

GPU acceleration is optional but significantly improves speed:
- **Supported GPUs** — NVIDIA GTX 900 series (Maxwell) and all later generations (GTX 10xx, GTX 16xx, RTX 20xx, RTX 30xx, RTX 40xx)
- **VRAM** — Minimum 2 GB for Tiny/Base/Small models; 6 GB recommended for Medium; 10 GB for Large
- **Driver** — NVIDIA Game Ready Driver 471.11 or later, or NVIDIA Studio Driver
- **AMD GPUs** — Not currently supported for hardware acceleration
- **Intel Arc GPUs** — Not currently supported for hardware acceleration

### Installer Sizes

| Installer Type | Download Size | What's Included                                    |
|----------------|---------------|----------------------------------------------------|
| Small Installer| ~30 MB        | App only; downloads models on first run (~400MB+)  |
| Full Installer | ~1 GB         | App + Tiny model + Base model pre-bundled          |

---

## Installation

### Option 1: Direct Download (Recommended)

1. Go to https://starwhisper.ai and click "Download for Windows"
2. Extract the downloaded ZIP file (right-click → Extract All)
3. Run the installer `.exe` file
4. If Windows SmartScreen shows a warning, click "More info" then "Run anyway"
5. Follow the installer prompts (installation takes about 1 minute)
6. StarWhisper starts automatically after installation

### Option 2: Microsoft Store

Install from the Microsoft Store for automatic updates and no SmartScreen warnings:

```
https://apps.microsoft.com/store/detail/9NXRF1N01VSN
```

Or search "StarWhisper" in the Microsoft Store app.

### Option 3: WinGet (Windows Package Manager)

```powershell
winget install StarWhisper.StarWhisper
```

### First-Time Setup

After installation, StarWhisper's setup wizard walks you through:
1. **Microphone selection** — Choose your recording device from detected inputs
2. **Mode selection** — Local (offline, private) or Cloud (OpenAI API, maximum accuracy)
3. **Model download** — Select and download a Whisper model (Tiny is pre-selected for speed)
4. **Hotkey configuration** — Set your preferred key to start/stop recording
5. **Tutorial** — 30-second interactive walkthrough of core features

### Updating

StarWhisper checks for updates automatically. When an update is available, a notification appears in Settings → About. Updates are incremental (~189MB typical) and preserve all settings, history, and model files.

### Uninstalling

Uninstall via Windows Settings → Apps → StarWhisper → Uninstall. The uninstaller removes application binaries and GPU pack files but preserves your settings and history. To remove all data, delete `%AppData%\StarWhisper` manually after uninstalling.

---

## Pricing

StarWhisper uses a freemium pricing model with a permanently free tier and a single Pro subscription tier.

### Free Plan — $0/month, forever

| Feature                          | Free Plan          |
|----------------------------------|--------------------|
| Words per day                    | 500                |
| Words per week                   | 3,500              |
| Local transcription (offline)    | Yes                |
| Available models                 | Tiny, Base, Small  |
| GPU acceleration                 | Yes                |
| Custom hotkey                    | Yes                |
| Voice commands                   | Yes                |
| Custom vocabulary                | Yes                |
| Transcription history            | Yes                |
| App interface languages          | 11 languages       |
| Visual themes                    | 3 themes           |
| File transcription               | No                 |
| Wake word activation             | No                 |
| OpenAI Whisper API access        | No                 |
| Medium and Large models          | No                 |
| AI text formatting               | No                 |
| Cloud sync                       | No                 |

### Pro Plan — $10/month or $80/year (save 33%)

Everything in Free, plus:

| Feature                          | Pro Plan           |
|----------------------------------|--------------------|
| Words per day                    | Unlimited          |
| Words per week                   | Unlimited          |
| Available models                 | All 5 (incl. Large)|
| File transcription               | Yes                |
| Wake word activation             | Yes                |
| OpenAI Whisper API access        | Yes                |
| AI text formatting               | Yes                |
| Cloud sync                       | Yes                |
| Visual themes                    | 14 themes          |
| Priority support                 | Yes                |
| 7-day free trial                 | No credit card     |

### Trial

A 7-day free trial of Pro is available with no credit card required. Sign up at https://starwhisper.ai to activate the trial.

### Refund Policy

StarWhisper offers a 7-day money-back guarantee. Contact support@starwhisper.ai within 7 days of purchase for a full refund.

### Payment Methods

All major credit and debit cards (Visa, Mastercard, American Express, Discover) processed securely via Stripe.

---

## Use Cases

### Medical and Healthcare Professionals

Doctors, physicians, therapists, psychiatrists, and nurses use StarWhisper to dictate clinical notes directly into their EMR/EHR system. Local mode ensures patient data never leaves the device, making it suitable for HIPAA-sensitive environments. The Large model achieves ~98% accuracy on medical terminology when combined with a custom vocabulary of medical terms, drug names, and diagnostic codes.

Common workflows:
- Dictating SOAP notes after patient appointments
- Transcribing patient intake forms
- Drafting referral letters and clinical summaries

### Legal Professionals

Attorneys, paralegals, and court reporters use StarWhisper to dictate legal documents, correspondence, and case notes. Local processing ensures client confidentiality. Legal-specific vocabulary can be added to improve accuracy on Latin phrases, legal citations, and case names.

Common workflows:
- Drafting client letters and memos
- Dictating deposition summaries
- Transcribing recorded depositions or client calls (file transcription, Pro)

### Writers and Content Creators

Authors, bloggers, journalists, and copywriters use StarWhisper to dictate content at speaking speed (~150 wpm) rather than typing speed (~60 wpm). The voice commands feature handles punctuation and paragraph breaks hands-free. Macro expansion is popular for inserting boilerplate sections.

Common workflows:
- Drafting blog posts and articles by voice
- Writing fiction while pacing around the room
- Transcribing interview recordings into quotes

### Developers and Technical Users

Software developers use StarWhisper to dictate commit messages, code comments, documentation, and natural-language queries to AI assistants. The global hotkey means you can dictate without leaving your IDE. Works seamlessly with VS Code, JetBrains IDEs, terminal windows, and GitHub Copilot Chat.

Common workflows:
- Dictating lengthy code comments and docstrings
- Composing queries to Claude, ChatGPT, or Copilot
- Writing README files and technical documentation by voice

### Students and Academics

Students use StarWhisper to take lecture notes by voice, transcribe recorded lectures (Pro file transcription), and dictate essays. The free tier's 500 words/day covers typical lecture note sessions.

### Accessibility

People with repetitive strain injuries (RSI), carpal tunnel, arthritis, or other conditions affecting typing ability use StarWhisper as their primary input method. The floating widget with customizable hotkey minimizes the need for mouse or keyboard interaction.

### Meeting Transcription

Pro users can drop recorded meeting audio files (MP4, MP3, WAV) into StarWhisper's file transcription feature to produce a text transcript of the meeting. This works offline with no per-minute charges unlike cloud transcription services.

---

## FAQ

### General Questions

**Q: Is StarWhisper really free?**
A: Yes. StarWhisper has a permanently free tier that includes 500 words per day, up to 3,500 words per week. This includes local offline transcription with the Tiny, Base, and Small models, GPU acceleration, custom hotkeys, voice commands, and transcription history. The free plan never expires. Pro ($10/month or $80/year) unlocks unlimited words, larger models, file transcription, and OpenAI API access.

**Q: Does StarWhisper work without internet?**
A: Yes, completely. After downloading a Whisper model (one-time download), StarWhisper runs entirely offline. All transcription is performed locally on your CPU or GPU. No internet connection is required for transcription, not even a background check. The only internet use is for account sign-in and optional cloud transcription (Pro, opt-in).

**Q: What is the difference between StarWhisper and Windows built-in voice typing?**
A: Windows voice typing (Win+H) uses Microsoft's cloud servers, requires an internet connection, only works in certain applications, and has no customization. StarWhisper works offline, works in every application via auto-paste, supports larger Whisper models with higher accuracy, and provides custom vocabulary, macros, hotkeys, and transcription history.

**Q: How accurate is StarWhisper?**
A: Accuracy varies by model:
- Tiny model: ~85% (fast, good for simple dictation)
- Base model: ~90% (balanced)
- Small model: ~94% (recommended for most users)
- Medium model: ~96% (Pro, professional use)
- Large model: ~98% (Pro, best available)

With a quality microphone, good speaking conditions, and the Small or larger model, most users report 95%+ accuracy on natural speech.

**Q: Can I use StarWhisper with ChatGPT, Claude, or other AI assistants?**
A: Yes. StarWhisper works with any Windows application including browser-based AI chat tools. The auto-paste feature means your spoken text appears wherever your cursor is, including ChatGPT, Claude, Gemini, Copilot, and any other tool. Many users use StarWhisper specifically to dictate prompts to AI assistants.

**Q: Does StarWhisper work with Dragon NaturallySpeaking?**
A: They are separate applications and generally should not run simultaneously (both will compete for the microphone). StarWhisper is positioned as a modern alternative to Dragon that uses Whisper AI rather than Dragon's proprietary engine, at a fraction of the cost.

---

### Technical Questions

**Q: What is whisper.cpp?**
A: whisper.cpp is an open-source C++ implementation of OpenAI's Whisper speech recognition model, created by Georgi Gerganov. It runs efficiently on CPU with optional CUDA (NVIDIA GPU) and Metal (Apple GPU) acceleration. StarWhisper bundles whisper.cpp as its local transcription engine. The whisper.cpp project is available at https://github.com/ggerganov/whisper.cpp.

**Q: How does GPU acceleration work in StarWhisper?**
A: If you have an NVIDIA GPU (GTX 900 series or newer), StarWhisper can run Whisper inference on your GPU via CUDA. This is 5–10x faster than CPU-only processing. To enable it, go to Settings → Transcription, and if an NVIDIA GPU is detected, you will see a prompt to download the GPU pack (~528MB). Enable "Use GPU Acceleration" in the same settings panel. GPU acceleration is available on the free plan.

**Q: What models does StarWhisper use?**
A: StarWhisper uses standard GGML-format Whisper model files, which are the same format used by whisper.cpp. These are OpenAI's Whisper models converted to the GGML format for efficient CPU/GPU inference. Models are downloaded from Hugging Face model hub.

**Q: Can StarWhisper transcribe multiple speakers?**
A: StarWhisper does not currently provide speaker diarization (identifying who said what). It transcribes audio into a single text stream without speaker labels.

**Q: What sample rate does StarWhisper record at?**
A: Audio is captured at 16kHz mono, which is the native input format expected by Whisper. StarWhisper's audio capture automatically resamples from your microphone's native rate to 16kHz.

**Q: How does the real-time transcription feature work?**
A: Real-time transcription uses a sliding window approach. Every few seconds (configurable interval), the accumulated audio is fed to whisper.cpp for an intermediate transcription. The partial result appears in a floating overlay on screen. When you stop recording, the final full-pass transcription replaces the partial result. Real-time mode is faster with GPU acceleration.

**Q: Where are my transcriptions stored?**
A: All transcriptions are saved locally at `%AppData%\StarWhisper\history.json`. This file is never uploaded to any server. You can view, search, and export your history from the History tab in the app, or delete it entirely from Settings → Privacy.

**Q: What hotkey can I use to start recording?**
A: Any key or key combination can be configured as the recording hotkey. The default is CapsLock (single key, easy to reach). Other popular choices: Ctrl+Space, F9, or a mouse button. Configure in Settings → Basic tab → Hotkey section.

---

### Privacy Questions

**Q: Is my voice data sent to the cloud?**
A: No, by default. In local mode (the default), audio is processed entirely on your machine and never leaves it. No audio, no transcription text, and no usage data is sent to StarWhisper's servers or anywhere else. If you optionally enable OpenAI cloud transcription (Pro feature), audio is sent to OpenAI's API over HTTPS.

**Q: What data does StarWhisper collect?**
A: StarWhisper collects only: your email address (if you sign in), your subscription status (Free or Pro), and the app version number (for update delivery). It does NOT collect your voice recordings, transcription text, what applications you use, or any behavioral analytics. Anonymous crash reports may be sent if you opt-in during installation.

**Q: Is StarWhisper suitable for medical use (HIPAA)?**
A: Yes, when using local transcription mode. Patient data spoken during transcription and the resulting text never leave your local machine, which satisfies HIPAA's requirements for data isolation. Cloud mode is not HIPAA compliant. StarWhisper does not sign Business Associate Agreements (BAAs).

**Q: Can I delete my transcription history?**
A: Yes, at any time. Go to Settings → Privacy → Delete History. This deletes the local `history.json` file. To also delete your account, contact support@starwhisper.ai.

---

### Installation Questions

**Q: Why does Windows SmartScreen block the installer?**
A: Windows SmartScreen shows a warning for applications that do not yet have a large number of verified installs. The StarWhisper installer is safe. To proceed: click "More info" on the SmartScreen dialog, then click "Run anyway". Alternatively, install from the Microsoft Store (no SmartScreen warning).

**Q: How large is the download?**
A: The base installer is approximately 30MB. During first-run setup, a Whisper model is downloaded. The Tiny model is 75MB. The Small model (recommended) is 466MB. The GPU acceleration pack is ~528MB (optional). Total installation with Small model and GPU pack is approximately 1.5GB.

**Q: Can I install StarWhisper without admin rights?**
A: The installer requires admin rights for the initial system-level installation. However, the Microsoft Store version installs without requiring admin privileges.

---

### Pricing Questions

**Q: How long does the 7-day free trial last?**
A: The Pro trial lasts 7 days from activation and includes all Pro features with no restrictions. No credit card is required. After 7 days, you either subscribe to continue Pro access or revert to the free tier.

**Q: What happens to my data if I cancel Pro?**
A: All your data (transcription history, vocabulary, macros, settings) remains intact. You simply revert to the free tier limits (500 words/day). Nothing is deleted when canceling.

**Q: Is there a family or team plan?**
A: Not currently. Each subscription covers a single user account. Running StarWhisper on multiple devices under the same account is permitted.

**Q: Does StarWhisper offer educational or non-profit discounts?**
A: Contact support@starwhisper.ai with institutional details. Discounts are evaluated case by case.

---

## Comparison with Competitors

### StarWhisper vs Dragon NaturallySpeaking (Nuance)

| Feature                      | StarWhisper Pro     | Dragon Professional |
|------------------------------|---------------------|---------------------|
| Price                        | $10/month ($80/yr)  | $300/year+          |
| Offline operation            | Yes                 | Yes (some versions) |
| Underlying AI model          | OpenAI Whisper      | Proprietary LSTM    |
| GPU acceleration             | NVIDIA CUDA         | Limited             |
| Language support (speech)    | 99 languages        | ~20 languages       |
| File transcription           | Yes (Pro)           | Yes                 |
| Custom vocabulary            | Yes                 | Yes                 |
| Macros                       | Yes                 | Yes                 |
| Voice commands               | Yes (basic)         | Yes (extensive)     |
| Works in every application   | Yes                 | Yes                 |
| HIPAA compliant mode         | Yes (local mode)    | Yes                 |
| Free tier available          | Yes (500 words/day) | No                  |
| Platform                     | Windows only        | Windows, Mac        |

**Summary:** StarWhisper is a strong alternative to Dragon for users who want modern Whisper-based accuracy at a significantly lower price. Dragon has more mature voice command macros and longer market presence. StarWhisper has broader language support and a free tier.

---

### StarWhisper vs Otter.ai

| Feature                      | StarWhisper Pro     | Otter.ai Pro        |
|------------------------------|---------------------|---------------------|
| Price                        | $10/month           | $16.99/month        |
| Offline operation            | Yes                 | No (cloud-only)     |
| Privacy                      | Full local          | Cloud processed     |
| Primary use case             | Desktop dictation   | Meeting notes       |
| Real-time transcription      | Yes                 | Yes                 |
| File transcription           | Yes                 | Yes                 |
| Speaker diarization          | No                  | Yes                 |
| Works outside meetings       | Yes (any app)       | Limited             |
| HIPAA compliant mode         | Yes                 | Paid enterprise only|
| Windows integration          | Deep (auto-paste)   | Browser-based       |
| Language support             | 99 languages        | English focus       |

**Summary:** Otter.ai is optimized for meeting transcription with speaker identification. StarWhisper is better for general desktop dictation, offline use, and privacy. If you mainly need to transcribe meetings with speaker labels, Otter.ai has advantages. If you need dictation throughout your workday across all applications with privacy, StarWhisper is the better choice.

---

### StarWhisper vs Whisper API (Direct OpenAI)

| Feature                      | StarWhisper         | OpenAI Whisper API  |
|------------------------------|---------------------|---------------------|
| Cost model                   | Flat monthly fee    | $0.006/minute       |
| Offline operation            | Yes                 | No                  |
| Real-time recording UI       | Yes (full app)      | API only (no UI)    |
| Auto-paste to applications   | Yes                 | No (DIY)            |
| Custom vocabulary            | Yes                 | No                  |
| History and export           | Yes                 | No                  |
| GPU acceleration             | Yes                 | N/A (cloud)         |
| Setup complexity             | Low (installer)     | High (API coding)   |

**Summary:** StarWhisper uses OpenAI's Whisper model locally and can also connect to the Whisper API. The value of StarWhisper over using the API directly is that it provides a complete application layer: recording UI, hotkeys, vocabulary, history, auto-paste, and flat-rate pricing instead of per-minute billing.

---

### StarWhisper vs Descript

| Feature                      | StarWhisper Pro     | Descript            |
|------------------------------|---------------------|---------------------|
| Price                        | $10/month           | $24/month           |
| Primary use case             | Dictation / STT     | Podcast/video editor|
| Offline operation            | Yes                 | No (cloud-only)     |
| Real-time dictation          | Yes                 | No                  |
| Video editing                | No                  | Yes                 |
| File transcription           | Yes                 | Yes                 |
| Speaker diarization          | No                  | Yes                 |
| Works system-wide            | Yes                 | No (app-only)       |

**Summary:** Descript is primarily a video and podcast editing application that includes transcription. StarWhisper is a dedicated dictation and transcription tool. They serve different primary use cases.

---

### StarWhisper vs Windows Built-in Voice Typing (Win+H)

| Feature                      | StarWhisper         | Windows Voice Typing|
|------------------------------|---------------------|---------------------|
| Offline operation            | Yes                 | No (cloud)          |
| Works in all applications    | Yes                 | Limited             |
| Model quality                | OpenAI Whisper      | Microsoft SAPI      |
| Language support (speech)    | 99 languages        | ~70 languages       |
| Custom vocabulary            | Yes                 | No                  |
| Hotkey configuration         | Any key             | Win+H only          |
| Transcription history        | Yes                 | No                  |
| Cost                         | Free tier available | Free                |

**Summary:** Windows Voice Typing is a zero-install option but has significant limitations: it requires internet, only works in some applications, cannot be customized, and has no history. StarWhisper is a comprehensive replacement.

---

## Technical Details

### Architecture Overview

StarWhisper is built as an Electron desktop application wrapping native C++ inference code:

- **Frontend** — Electron / Node.js application with a floating widget and system tray integration
- **Transcription engine** — whisper.cpp compiled as a native Node.js addon (N-API)
- **Audio capture** — Windows WASAPI via the `naudiodon` Node.js module
- **GPU acceleration** — CUDA 11.8 runtime bundled separately in the GPU pack
- **Authentication** — Firebase Authentication (Google and email sign-in)
- **Subscription management** — Stripe API via a lightweight backend server
- **Cloud sync** — Firebase Firestore for vocabulary, macros, and settings

### Model Format

StarWhisper uses GGML format Whisper models, which are the standard format for whisper.cpp. These are binary files containing quantized model weights optimized for fast CPU/GPU inference. All models are available publicly on Hugging Face at https://huggingface.co/ggerganov/whisper.cpp.

### Update Mechanism

StarWhisper uses an Electron auto-updater with two update channels:
- **Stable** — For production installs downloaded from the website
- **Store** — Microsoft Store distribution, managed by Windows Update

Updates are differential where possible, minimizing download sizes.

### System Integration

- **Global hotkey** — Registered via Electron's `globalShortcut` module, works even when the app window is not focused
- **Auto-paste** — Implemented using Windows `SendInput` API to simulate Ctrl+V after clipboard write
- **Always-on-top** — The floating widget uses the Windows `HWND_TOPMOST` flag
- **Tray icon** — Provides quick access to settings and shows recording status without the full window

### whisper.cpp and the Whisper Model Architecture

OpenAI's Whisper model (on which whisper.cpp is based) is a transformer sequence-to-sequence model:
- **Input** — 30-second audio chunks processed as 80-dimensional log-mel spectrograms
- **Encoder** — Transformer encoder (depth varies by model size: 4 layers for Tiny, 32 for Large)
- **Decoder** — Transformer decoder with cross-attention to encoder output, auto-regressively generating tokens
- **Vocabulary** — 51,864 byte-pair encoding (BPE) tokens including language and task tokens
- **Training data** — 680,000 hours of multilingual audio from the internet (weakly supervised)
- **License** — MIT License (OpenAI Whisper), allowing commercial use

### Accuracy Notes

Whisper's Word Error Rate (WER) on standard benchmarks:
- English (LibriSpeech clean): Large model achieves ~2.7% WER (97.3% word accuracy)
- Multilingual (CoVoST2 21-language): Large model achieves competitive with specialized models
- Performance degrades on: heavy accents, overlapping speech, very low audio quality, domain-specific jargon without vocabulary customization

---

## Screenshots

> Screenshots will be added in a future update. The live application can be previewed at https://starwhisper.ai.

The StarWhisper interface consists of:

1. **Floating recording circle** — A small draggable circle (~80px diameter by default) that sits above all windows. Purple/violet when idle, orange when recording. Pulses in sync with voice amplitude.

2. **Settings panel** — Accessed by right-clicking the circle. Contains tabs for: Basic (hotkey, language, model), Audio (microphone selection, levels), Transcription (GPU, real-time), Appearance (theme, size, transparency), History, Vocabulary, Macros, Files (Pro), Plan, and About.

3. **Real-time overlay** — Optional floating text overlay that shows partial transcription results as you speak.

4. **History panel** — Lists all past transcriptions with timestamps, searchable, with one-click copy and export options.

---

## Links and Resources

### Official

- **Website:** https://starwhisper.ai
- **Download:** https://starwhisper.ai/downloads/StarWhisper-Setup-Full.exe
- **Microsoft Store:** https://apps.microsoft.com/store/detail/9NXRF1N01VSN
- **Documentation:** https://starwhisper.ai/docs/
- **FAQ:** https://starwhisper.ai/faq.html
- **Pro Upgrade:** https://starwhisper.ai/pro.html
- **Support:** support@starwhisper.ai

### Documentation Sections

- **Getting Started:** https://starwhisper.ai/docs/getting-started/installation.html
- **System Requirements:** https://starwhisper.ai/docs/getting-started/system-requirements.html
- **Basic Recording:** https://starwhisper.ai/docs/user-guide/basic-recording.html
- **Voice Commands:** https://starwhisper.ai/docs/user-guide/dictation-and-voice-commands.html
- **GPU Acceleration:** https://starwhisper.ai/docs/features/gpu-acceleration.html
- **Model Selection:** https://starwhisper.ai/docs/features/models.html
- **File Transcription (Pro):** https://starwhisper.ai/docs/features/file-transcription.html
- **Wake Word (Pro):** https://starwhisper.ai/docs/features/wake-word.html
- **Custom Vocabulary:** https://starwhisper.ai/docs/user-guide/vocabulary-and-macros.html
- **Language Settings:** https://starwhisper.ai/docs/features/language-settings.html
- **Hotkey Configuration:** https://starwhisper.ai/docs/features/hotkey-configuration.html

### Landing Pages by Use Case

- **Medical Dictation:** https://starwhisper.ai/landing/medical-dictation-software-pro.html
- **Legal Dictation:** https://starwhisper.ai/landing/legal-dictation-software-pro.html
- **Therapist Dictation:** https://starwhisper.ai/landing/therapist-dictation-software-pro.html
- **Dragon Alternative:** https://starwhisper.ai/dragon-alternative.html
- **Otter.ai Alternative:** https://starwhisper.ai/landing/otter-ai-alternative.html
- **Interview Transcription:** https://starwhisper.ai/landing/interview-transcription-software.html
- **Meeting Transcription:** https://starwhisper.ai/landing/meeting-transcription-software.html
- **Offline Speech to Text:** https://starwhisper.ai/blog/free-voice-to-text.html

### Related Open Source Projects

- **whisper.cpp** (transcription engine): https://github.com/ggerganov/whisper.cpp
- **OpenAI Whisper** (original model): https://github.com/openai/whisper
- **Whisper models on Hugging Face**: https://huggingface.co/ggerganov/whisper.cpp

---

## Changelog Highlights

### Version 1.3.117 (Current)
- Hold-to-talk recording mode (hold key → release to transcribe)
- Automatic microphone fallback after device failure
- Cloud fallback consent flow improvements
- Hardware profile telemetry for diagnostics (RAM, CPU, OS version)

### Earlier Notable Additions
- GPU acceleration via CUDA
- File transcription (Pro feature)
- Wake word activation (Pro feature)
- Real-time transcription overlay
- Custom vocabulary with fuzzy matching
- Text macros
- 14 visual themes
- AI text formatting (Pro)
- Cloud sync for settings and vocabulary

---

## License

This repository contains documentation and promotional materials for StarWhisper. These materials are released under the Creative Commons Attribution 4.0 International License (CC BY 4.0).

StarWhisper itself is proprietary software. See https://starwhisper.ai/terms for the End User License Agreement.

The whisper.cpp transcription engine bundled within StarWhisper is open source under the MIT License. See https://github.com/ggerganov/whisper.cpp/blob/master/LICENSE.

---

*Last updated: March 2026*
*StarWhisper version documented: 1.3.117*
*Website: https://starwhisper.ai*
