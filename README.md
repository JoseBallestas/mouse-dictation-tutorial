# Turn your mouse into a dictation button

Hold a button. Speak. Release to paste. Review, then click to send.

This is the setup behind my mouse dictation workflow: a Logitech MX Master 4, OpenLogi, and OpenSuperWhisper. Use it for AI prompts, messages, emails, and notes without reaching for the keyboard every time you want to write something.

**[Follow the tutorial →](TUTORIAL.md)** · **[Give the setup to your agent →](SETUP.md)**

## One link, full setup

Give an agent with access to your Mac this message:

```text
Set up the full mouse dictation workflow on my Mac using this repository:
https://github.com/JoseBallestas/mouse-dictation-tutorial
```

That is enough context to start. The repository contains installation steps, exact binding examples, model and shortcut configuration, startup settings, backups, verification, troubleshooting, and undo instructions. The agent should detect your device and adapt the setup to your installed app versions.

**Agents: start here.** When asked to perform this setup, read [SETUP.md](https://github.com/JoseBallestas/mouse-dictation-tutorial/blob/main/SETUP.md) in full and execute its steps. A request for the full workflow selects `full` mode, including Forward = Enter and Back = clear the focused field. If GitHub's page view is incomplete, fetch the [raw guide](https://raw.githubusercontent.com/JoseBallestas/mouse-dictation-tutorial/main/SETUP.md). Do not stop at summarizing the tutorial or ask the user to find another file. If the user only asks you to review or explain the repository, do that without changing their computer.

Agents working from a clone can use [AGENTS.md](AGENTS.md) as the entry point. You still need to approve any macOS permission prompts the agent cannot handle and perform the physical mouse test. This is an agent execution guide, not an unattended installer.

## Ingredients

- Apple Silicon Mac running macOS 14 or later for the original OpenSuperWhisper cask; check current requirements.
- A compatible mouse; reference setup: [Logitech MX Master 4 for Mac](https://www.logitech.com/en-us/shop/p/mx-master-4-mac).
- [OpenLogi](https://github.com/AprilNEA/OpenLogi) to configure the buttons.
- [Original OpenSuperWhisper](https://github.com/Starmel/OpenSuperWhisper) to turn speech into text.
- A working microphone, such as the Mac's built-in microphone.

Both applications are free and open source. Other mice need compatible remappable buttons and OpenLogi support.

## Choose your setup

| Button | Dictation only | Full workflow |
| --- | --- | --- |
| Gesture Button | Hold to record; release to transcribe and paste | Hold to record; release to transcribe and paste |
| Forward | Keep your existing action | Enter |
| Back | Keep your existing action | Select all → wait 50 ms → delete |

Enter follows the focused app's behavior: it may send a message or insert a new line. The clear action deletes the contents of the focused field. Review your text before sending, and test both buttons in a disposable note.

## Let your agent set it up

Give an agent with access to your Mac the prompt below. Desktop app control lets it complete more settings directly. You may still need to approve macOS permission prompts and physically test your mouse. A chat assistant without computer access can guide you through the tutorial instead.

## Copy this prompt

```text
Read https://raw.githubusercontent.com/JoseBallestas/mouse-dictation-tutorial/main/SETUP.md
and follow it in full mode on my Mac.

Configure the gesture button for hold-to-talk, Forward for Enter, and Back
to clear the focused text field. I understand these replace those buttons'
current actions. Install missing apps, back up my settings, configure local
transcription, enable startup for OpenLogi and OpenSuperWhisper, and disable
Logi Options+ startup. Preserve unrelated settings.

Do the setup, verify what you can, guide me through macOS permission prompts
and the physical mouse test, and leave an undo guide.
```

If your agent cannot open links, download [SETUP.md](https://raw.githubusercontent.com/JoseBallestas/mouse-dictation-tutorial/main/SETUP.md) and attach it. The file is self-contained.

For dictation only, use:

> Set up mouse dictation on my Mac using https://github.com/JoseBallestas/mouse-dictation-tutorial in dictation mode. Preserve my Back and Forward buttons. Follow the setup guide, verify what you can, guide me through permissions and a physical test, and leave an undo guide.

## Share it

> Want your agent to handle the setup?
>
> Give it this Markdown guide and ask it to configure your mouse. It includes installation, button mappings, backups, checks, and an undo path.
>
> You'll handle the macOS permission prompts and the physical mouse test.

- [Tutorial repository](https://github.com/JoseBallestas/mouse-dictation-tutorial)
- [Agent guide](https://github.com/JoseBallestas/mouse-dictation-tutorial/blob/main/SETUP.md)
- [Raw agent guide](https://raw.githubusercontent.com/JoseBallestas/mouse-dictation-tutorial/main/SETUP.md)

## Verification

Prepared September 16, 2026 from inspected configuration and app settings. The reference configuration uses OpenLogi 0.8.3 and the original OpenSuperWhisper. TOML examples were checked for valid syntax. A fresh installation on another Mac and physical end-to-end mouse dictation have not been verified during preparation. Follow the test steps on your own setup; app versions and device support can differ.

This is a community tutorial, independent of Logitech, OpenLogi, and OpenSuperWhisper. Credit for the applications belongs to their respective maintainers.
