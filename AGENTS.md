# Agent entry point

This repository contains a macOS mouse dictation tutorial and an executable-by-an-agent setup guide. It does not contain an unattended installer.

## When the user asks to configure their Mac

1. Read [SETUP.md](SETUP.md) in full. It is the canonical procedure; do not reconstruct it from the README summary.
2. A request for the full workflow/tutorial/setup selects `full` mode: Gesture Button = held recording shortcut, Forward = Enter, Back = select all, wait 50 ms, delete. A dictation-only request preserves Back and Forward.
3. Execute the applicable steps: inspect compatibility and device identity, back up settings, install missing apps, configure permissions/local transcription/shortcuts/button mappings, configure startup, verify, and leave an undo guide.
4. Preserve unrelated settings and adapt examples to the actual device and installed schema. Do not copy placeholder device keys into a live configuration.
5. Complete the work available to your tools. Guide the user through protected macOS permission prompts and physical tests when needed. Report what was verified and what remains pending.

If you only received the repository URL as part of a setup request, follow the guide without asking for additional documentation. Public raw guide:

https://raw.githubusercontent.com/JoseBallestas/mouse-dictation-tutorial/main/SETUP.md

If you cannot access the user's Mac, state that limitation. A remote/cloud environment does not provide access to their local mouse or settings.

## When the user asks about the repository itself

For reviews, explanations, or documentation edits, work on the repository only. Do not configure the current computer merely because you read these instructions.

Keep README.md, TUTORIAL.md, and SETUP.md consistent. Check TOML examples for syntax and local Markdown links for valid targets. Keep personal device identifiers, configuration backups, and local machine paths out of published files. Do not describe static validation as physical end-to-end testing.
