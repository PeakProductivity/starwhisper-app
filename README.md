# StarWhisper

StarWhisper is a Windows desktop dictation app. After you install a local model, its local whisper.cpp engine can turn speech into text on your computer. A floating recording control and global hotkey help start and stop dictation without leaving the app you are using. StarWhisper can insert the result into the focused Windows text field. Compatibility, speed, and transcription quality vary with hardware, microphone, language, and settings. Review generated text before relying on it.

## Core features

- Local Mode for normal dictation after the required local engine and model are installed.
- Optional Cloud Mode, which sends audio to OpenAI for transcription when you choose it.
- Insertion into the focused Windows text field through the clipboard and paste path.
- A floating recording control and a configurable hotkey.
- Optional text formatting and settings sync, with separate data flows described below.

## Privacy and data flows

Local Mode is the normal path for dictation after the local engine is installed. The first-time setup microphone check is a separate case: if the local engine is still loading, or no words appear after about six seconds, its preview may be sent through the StarWhisper server to OpenAI. The app labels this state **Cloud preview**. Dictation after setup follows your Local Mode or Cloud Mode setting.

The app collects usage and diagnostic events linked to a random installation ID by default. You can turn this off in Settings. These events do not contain transcription content. Cloud settings sync is opt-in and has its own data flow. If you use AI formatting, the selected text goes through the StarWhisper server to OpenAI for that operation. Read the full [Privacy Policy](https://starwhisper.ai/privacy.html) before choosing a mode or feature.

## Install and updates

This README describes the website-installed Windows app. Download the [Windows package](https://starwhisper.ai/downloads/StarWhisper-Setup.zip), then follow the [official documentation](https://starwhisper.ai/docs/) for setup and model selection.

The public [download manifest](https://starwhisper.ai/api/bootstrap.json) lists current package choices, version numbers and file checksums. For the website-installed app, check for an update by choosing **Check for updates** in the app. Other distribution channels may have different update flows.

## Plans

Current plan and allowance details are maintained on the official [Pro page](https://starwhisper.ai/pro.html). This README does not duplicate prices, usage caps, or changing offer terms.

## License and third-party software

This repository contains documentation, descriptions, and promotional materials. They are licensed under [CC BY 4.0](https://github.com/PeakProductivity/starwhisper-app/blob/main/LICENSE). The StarWhisper application itself is proprietary; see its [Terms](https://starwhisper.ai/terms.html). Third-party components keep their own licenses. The local engine is based on [whisper.cpp](https://github.com/ggerganov/whisper.cpp).

## Links

- [StarWhisper website](https://starwhisper.ai/)
- [Documentation](https://starwhisper.ai/docs/)
- [Privacy Policy](https://starwhisper.ai/privacy.html)
- [Terms](https://starwhisper.ai/terms.html)
- [Pro](https://starwhisper.ai/pro.html)
