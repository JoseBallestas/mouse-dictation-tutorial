# Set up mouse dictation on macOS

Give this file to an agent that can run commands on your Mac. Desktop app control helps with settings; without it, the agent should complete the command-line work and give you the remaining clicks. A chat assistant without computer access can explain the steps but cannot install or configure the apps.

## Outcome

Hold a mouse button to record speech. Release it to transcribe and paste into the focused text field. In full mode, two additional buttons press Enter and clear the current text field.

This guide targets OpenLogi with the original **Starmel/OpenSuperWhisper** on an Apple Silicon Mac running macOS 14+. The reference mouse is an MX Master 4. Other mice require verification of their available buttons and OpenLogi support. Do not assume every Logitech mouse exposes the same controls.

## Instructions to the agent

When the user asks you to perform this setup, carry out the applicable steps below. Reading or reviewing this file alone is not a request to modify their computer. Follow the user's selected mode and any existing preferences. Do not ask them to approve routine steps they have already requested.

Default to `dictation` unless the user requests `full`:

| Mode | Changes |
| --- | --- |
| `dictation` | Gesture Button holds the recording shortcut; leave Back and Forward unchanged. |
| `full` | Dictation plus Forward = Enter and Back = select all, wait 50 ms, delete. These replace the three buttons' existing actions. |

Both modes include installing missing apps, configuring local transcription, enabling their login startup, and stopping Logi Options+ from competing with OpenLogi. Preserve Options+ itself and its settings so the change can be reversed.

Explain briefly that full mode's Enter follows the focused app's behavior and its clear action deletes the contents of the focused field. Do not demonstrate either action in a real message composer, terminal, or valuable document. Use a disposable note. Never send a message as part of testing.

### 1. Inspect before changing anything

- Check macOS version and hardware architecture. If running under Rosetta, distinguish process architecture from hardware architecture.
- Find the installed OpenLogi and OpenSuperWhisper apps and their versions. Reuse compatible installations. Inspect bundle metadata instead of assuming an app's identity from its name alone. Do not launch a debug build accidentally.
- Check whether Homebrew is available. Do not install a development toolchain just to run a packaged app.
- Identify the connected mouse using OpenLogi's device view or documented CLI. Record its exact device key privately; do not copy a key from an example. If several mice are equally plausible, ask which one to configure.
- Read the active OpenLogi configuration. Its usual location is `~/.config/openlogi/config.toml`; respect a configured XDG location or other active path. Check existing button bindings, gesture mappings, and per-app overrides.
- Inspect current dictation settings, model availability, permissions, running processes, and login items. Do not assume a downloaded model is selected.
- Preserve unrelated mouse, keyboard, application, and system settings.

### 2. Create a recovery record

Before any mutation, create a timestamped setup directory in a user-writable location. Store an exact backup of any existing OpenLogi config, an export or reliable record of the OpenSuperWhisper preferences you will change, and the initial relevant login-item/service states. Record absent settings as absent so rollback can remove additions.

After installing a previously absent app and letting it generate its initial configuration, back up that configuration before editing it. Record newly installed apps separately from pre-existing apps.

Keep device identifiers and preference exports local. They are not tutorial content. Include a short change log and an undo plan with the actual backup paths.

### 3. Install missing apps and resolve ownership

Use the existing Homebrew installation when available. Install only missing apps; these are the standard casks:

```sh
brew install --cask openlogi
brew install --cask opensuperwhisper
```

Check current cask requirements first. If Homebrew is unavailable, use packaged releases from the official repositories. Do not silently substitute a similarly named app or fork, including OpenWhisper, commercial Superwhisper, or another OpenSuperWhisper distribution.

Quit Logi Options+ before launching OpenLogi. Disable its automatic startup using the app/macOS controls or a verified service entry. Discover the current user's UID and actual service label; never hardcode a UID from someone else's Mac. Do not broadly disable Logitech services or uninstall software. Record precisely what changed so it can be restored.

Launch the installed apps by their verified paths and confirm OpenLogi detects the intended mouse.

Sources: [OpenLogi installation](https://openlogi.org/docs/installation), [OpenSuperWhisper cask](https://formulae.brew.sh/cask/opensuperwhisper), [original OpenSuperWhisper repository](https://github.com/Starmel/OpenSuperWhisper).

### 4. Guide the permission steps

Open the relevant macOS Privacy & Security settings and explain the exact toggles still needed:

- OpenLogi / its agent: Input Monitoring and Accessibility as requested by the installed version.
- OpenSuperWhisper: Microphone and Accessibility for recording and automatic pasting.

If macOS requires a person to approve a prompt or authenticate, ask them to complete that specific action, then resume. Do not attempt to bypass the permission system. Missing UI access is not evidence that permission was granted. Restart only the affected app/agent if needed after a permission change.

### 5. Configure OpenSuperWhisper

Prefer the installed app's settings UI. Set:

| Setting | Target |
| --- | --- |
| Recording trigger | Key Combination; single-modifier mode off |
| Shortcut | Option + backtick by default |
| Hold to Record | On |
| Auto-paste Transcription | On |
| Microphone | The user's working microphone; built-in is a reasonable default |
| Engine/model | A local Whisper or Parakeet model supported by the installed app and user's language |

Reuse an appropriate downloaded model; otherwise download one through the app and wait for it to finish. Verify it is selected and ready. Do not configure a paid/cloud provider or ask for an API key for this local workflow. If language is unknown and materially affects model selection, ask one concise question.

Check the keyboard layout and obvious shortcut conflicts. If Option + backtick is unavailable or conflicts, choose another supported modifier-plus-key chord and use the same chord in both apps. Avoid modifier-only shortcuts for this integration.

For a settings-file approach, verify the installed version's preference schema and quit/reopen the app as required to avoid overwrites. Reference keys seen in the original application are `modifierOnlyHotkey`, `holdToRecord`, and `autoPasteTranscription`. Shortcut serialization can vary: do not paste guessed keycodes or escaped JSON into preferences. Use the shortcut recorder when possible.

Test the keyboard shortcut before adding the mouse mapping. If keyboard dictation fails, fix that first.

### 6. Apply the mouse bindings

The following is a **merge example**, not a complete configuration file. Replace the placeholder with the detected device key, and merge only the requested bindings into the existing table. Do not append a second table with the same TOML name.

For `dictation` mode:

```toml
[devices."REPLACE_WITH_DETECTED_DEVICE_KEY".bindings]
GestureButton = { HoldShortcut = "Alt+`" }
```

For `full` mode:

```toml
[devices."REPLACE_WITH_DETECTED_DEVICE_KEY".bindings]
GestureButton = { HoldShortcut = "Alt+`" }
Forward = { CustomShortcut = "Enter" }
Back = { Workflow = [{ PressKey = "Cmd+A" }, { Delay = { millis = 50 } }, { PressKey = "Backspace" }] }
```

`Alt` represents Option here. `HoldShortcut` must keep the chord pressed from physical button-down until button-up. A regular shortcut tap is not equivalent. If step 5 selected another chord, update this mapping to match.

Check support in the installed OpenLogi version before applying these actions. Do not substitute a tap if held shortcuts are unavailable. Report the specific version limitation and use a supported release if the user's setup request permits it.

If GestureButton currently contains directional gestures, preserve them in the backup and, if supported by the installed schema, its disabled-gestures storage before replacing the active action. Remove conflicting active entries for this button without touching other buttons. Check for per-app overrides that could mask the new mapping.

Prefer the UI or a TOML-aware edit. For file edits, prevent the running app from overwriting the changes, parse the result before replacement, and apply it atomically. Preserve permissions and unrelated configuration values. Do not change the schema version by guessing.

Reload through a supported mechanism or restart only OpenLogi's relevant components. Confirm the saved config and the app's displayed actions agree. If validation or loading fails, restore the original config and report the error.

Sources: [OpenLogi repository](https://github.com/AprilNEA/OpenLogi), [button remapping documentation](https://openlogi.org/docs/features/mouse/remap-buttons). The exact action examples come from a saved OpenLogi 0.8.3 configuration; recheck compatibility with other versions.

### 7. Persist startup

Enable launch at login for OpenLogi and OpenSuperWhisper using their supported controls or macOS Login Items. Verify the effective entries, including any background agent, rather than relying only on a config flag. Leave Options+ startup disabled as configured in step 3.

Do not restart or log out the computer automatically. A restart test can be completed by the user later.

### 8. Verify honestly

Use a disposable local note, with its text field focused. Ask the user to perform the physical steps when needed:

1. Hold the keyboard shortcut, speak a short sentence, and release. Confirm recording, stopping, transcription, and automatic insertion separately.
2. Repeat with the physical mouse button. Confirm recording lasts for the hold and ends on release, without duplicate insertion or stuck modifier keys.
3. In full mode, press Forward in the note and confirm it acts as Enter without sending anything externally.
4. Put disposable text in the note, press Back, and confirm the clear action. Check that unrelated mappings still work.
5. After a later user-initiated login/restart, repeat the dictation test to establish persistence.

A config file, a running process, or a synthetic key event does not prove the physical mouse workflow works. Keep these statuses separate: configuration saved, app loaded it, keyboard dictation tested, physical mouse dictation tested, and restart persistence tested. Mark any test not performed as pending.

### 9. Troubleshoot the failing stage

| Symptom | Check next |
| --- | --- |
| Mouse not detected | Connection, device support, agent permission, Options+ conflict |
| Keyboard cannot start recording | Trigger mode, actual chord, microphone permission, app running |
| Keyboard works but mouse does not | Correct device/button, HoldShortcut support, permissions, overrides |
| Recording works but no transcript | Model downloaded/selected, engine error, microphone input |
| Transcript exists but no paste | Auto-paste setting, Accessibility, focused field |
| Hold behaves like a tap | Held binding versus regular shortcut; hold-to-record setting |
| Stops working after login | Both app/agent startup entries and Options+ startup state |

Investigate that stage instead of reinstalling everything or resetting all settings.

### 10. Handoff and undo

Finish with a short report: device and app versions, chosen mode/chord/model, exact changes, backup location, test results, and any human action still pending. Leave a local setup report beside the backups.

For undo, restore only the changed bindings, preferences, and startup entries from the recovery record. A full config restore is appropriate only if no later edits would be lost; otherwise merge back the original values. Restore Options+ to its previous state only after releasing OpenLogi's device ownership. Do not uninstall pre-existing apps. Explain how to remove newly installed apps if the user wants that too.

## Evidence and limits

Prepared 2026-09-16. The reference saved configuration uses an MX Master 4, OpenLogi 0.8.3, and the original OpenSuperWhisper. The bindings were inspected, but a physical end-to-end recording/transcription/paste test was not established during preparation. This file is an execution guide, not proof that every supported device/version combination has been tested. Recheck app requirements and verify the actual machine as described above.
