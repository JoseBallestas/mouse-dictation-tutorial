# Set up the full mouse dictation workflow

[Back to the overview](README.md) · [Agent setup instructions](SETUP.md)

Hold your mouse's gesture button while speaking, release it to transcribe and paste, then review your words. Optionally, use Forward for Enter and Back to clear the current text field.

This guide uses an MX Master 4 and an Apple Silicon Mac running macOS 14 or later. See the [ingredients](README.md#ingredients) before starting.

## 1. Install the apps

If you already have Homebrew, install whichever apps are missing:

```sh
brew install --cask openlogi
brew install --cask opensuperwhisper
```

Without Homebrew, use packaged downloads from [OpenLogi](https://github.com/AprilNEA/OpenLogi/releases) and [Starmel/OpenSuperWhisper](https://github.com/Starmel/OpenSuperWhisper/releases). You do not need to build from source. Reuse compatible installations.

Quit Logi Options+ before opening OpenLogi. They can compete for control of the mouse. Turn off Options+'s automatic startup, recording its previous state so you can undo the change. Keep the app installed if you may want to return to it.

Sources: [OpenLogi installation](https://openlogi.org/docs/installation), [OpenSuperWhisper cask and requirements](https://formulae.brew.sh/cask/opensuperwhisper).

## 2. Grant permissions

Launch the apps and follow their prompts. In macOS System Settings → Privacy & Security:

- Grant OpenLogi / its agent the requested Input Monitoring and Accessibility permissions.
- Grant OpenSuperWhisper Microphone and Accessibility permissions for recording and automatic pasting.

Reopen the affected app if prompted. Confirm the intended mouse appears in OpenLogi. The permission entry may identify its background agent separately from the main app.

## 3. Prepare transcription

In OpenSuperWhisper, select your microphone and a local Whisper or Parakeet model suitable for your language. Download the model through the app if needed and wait until it is ready. Try a short recording inside the app.

This tutorial uses the original [Starmel/OpenSuperWhisper](https://github.com/Starmel/OpenSuperWhisper). Similarly named apps and forks can have different settings. This local workflow does not require configuring a cloud provider.

## 4. Set the recording shortcut

In OpenSuperWhisper's settings:

| Setting | Value |
| --- | --- |
| Recording Trigger | Key Combination; Single Modifier Key mode off |
| Shortcut | Option + backtick (`) |
| Hold to Record | On |
| Auto-paste Transcription | On |

In an empty note, hold the keyboard shortcut, say a short sentence, and release. Confirm the text appears. If the chord conflicts with another shortcut or is awkward on your keyboard layout, choose another supported modifier-plus-key combination and use it in both apps.

## 5. Back up your settings

Save a copy of OpenLogi's active configuration before remapping. The default location is `~/.config/openlogi/config.toml`. If your app uses another location, back up that file instead. Give the backup a unique date/time in its filename and keep it outside this repository. Note your previous shortcut, transcription, and startup settings too.

## 6. Connect the gesture button

In OpenLogi, select your mouse, open its button controls, and assign the Gesture Button a held shortcut matching OpenSuperWhisper's shortcut. The required action in the reference configuration is:

```toml
GestureButton = { HoldShortcut = "Alt+`" }
```

`Alt` means Option. `HoldShortcut` keeps the chord pressed until the physical button is released. A regular shortcut tap does not provide the same behavior.

If your version's interface does not expose this action, check its support before editing the configuration. The [agent guide](SETUP.md#6-apply-the-mouse-bindings) can help. Do not substitute a tap for a held shortcut.

### Configuration example

Merge this into your existing mouse bindings. It is not a complete replacement configuration:

```toml
[devices."REPLACE_WITH_YOUR_DEVICE_KEY".bindings]
GestureButton = { HoldShortcut = "Alt+`" }
```

Use your mouse's actual device key from the existing config. Do not duplicate the bindings table. If GestureButton has directional gesture entries, preserve them in your backup and replace the active action; do not leave both definitions active. Leave unrelated settings intact.

For manual edits, close the settings app and use its supported agent stop/reload mechanism as needed to prevent overwrites. Validate the TOML, reopen/reload OpenLogi, and confirm it displays the binding. See the [agent guide](SETUP.md#6-apply-the-mouse-bindings) for merge and validation requirements.

Examples reflect a saved OpenLogi 0.8.3 configuration. Confirm action support in your installed version. See [OpenLogi's remapping documentation](https://openlogi.org/docs/features/mouse/remap-buttons).

## 7. Test the physical button

1. Open a disposable note and click its text field.
2. Hold the gesture button and say a sentence.
3. Release the button.
4. Confirm recording stops and the transcription appears once.

If keyboard dictation works but the mouse does not, check the device, held binding, OpenLogi permissions, and per-app overrides. If neither works, resolve microphone, model, or shortcut issues first.

You can stop here for dictation only and keep your other buttons unchanged.

## 8. Add Enter and clear-draft buttons (optional)

Map Forward to a custom Enter shortcut. Map Back to a workflow: press Cmd+A, wait 50 milliseconds, then press Backspace.

The full bindings are:

```toml
[devices."REPLACE_WITH_YOUR_DEVICE_KEY".bindings]
GestureButton = { HoldShortcut = "Alt+`" }
Forward = { CustomShortcut = "Enter" }
Back = { Workflow = [{ PressKey = "Cmd+A" }, { Delay = { millis = 50 } }, { PressKey = "Backspace" }] }
```

These replace the buttons' previous actions. Enter follows the focused app: it may send a message or insert a new line. The clear action selects and deletes the focused field's contents. Test with disposable text in a note, and review dictation before sending anything.

OpenLogi supports per-app mappings if you want these actions only in selected apps. Follow its current documentation rather than guessing the profile schema.

## 9. Keep it working after login

Enable launch at login for OpenLogi and OpenSuperWhisper through their settings or macOS Login Items. Keep Options+ startup disabled. After your next restart/login, repeat the physical dictation test; a saved preference alone does not prove persistence.

## Troubleshooting

| Problem | Check |
| --- | --- |
| Mouse missing | Connection, model support, agent permission, Options+ conflict |
| No recording | Microphone permission/input, app running, shortcut mode |
| Keyboard works; mouse does not | Device/button selection, HoldShortcut support, permissions, overrides |
| Recording but no text | Model downloaded and selected, engine errors, microphone input |
| Transcript exists but no paste | Auto-paste setting, Accessibility, intended field focused |
| Recording ends immediately | Held binding versus a tap; Hold to Record setting |
| Works until restart | Both app/agent login entries and Options+ startup state |

## Undo

Restore original actions for the changed buttons from your backup, along with previous OpenSuperWhisper settings and login-item states. If you have made other configuration changes since taking the backup, merge back only these changes instead of replacing the entire file.

If returning to Options+, first quit/disable OpenLogi's device control, then restore Options+'s previous startup state. You do not need to delete either application's settings.

## What has been checked

The saved reference bindings and app settings were inspected, and the TOML examples were parsed. A fresh install on another Mac and physical end-to-end dictation were not established during preparation. Verify the workflow on your machine using the steps above.
