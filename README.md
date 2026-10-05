# SpeakEasy — hands-free communication

A free, camera-based **AAC** (augmentative & alternative communication) web app. It lets a person
who can't use their hands or voice compose text and speak it aloud using only a device's front
camera. All processing happens **on-device in the browser** — no video is ever uploaded or saved.

Built for the **Congressional App Challenge 2026**.

**Live site:** `https://<your-username>.github.io/speakeasy-aac/`
_(fill this in once GitHub Pages is enabled — see below)_

## Versions

| | Version | What it adds |
|---|---|---|
| [`/v6`](v6/) | **v6 — listen mode (blind & low-vision)** | The v5 command board **by ear**: auditory scanning reads each option in a quick, quiet cue voice and speaks the chosen one in a full voice; earcons; spoken camera guidance ("move a little to your left") and a spoken switch calibration; one- or two-switch scanning with any key or Bluetooth switch (no camera needed); an **Ask** category whose answers the voice assistant speaks back; settings saved on the device |
| [`/v5`](v5/) | **v5 — command board (recommended)** | Speaks **whole commands in 1–2 selections** — smart-home control ("Alexa, turn on the AC"), urgent needs, and calls — for people for whom spelling is too slow. Selectable wake word (Alexa / Google / Siri). A **Type a message** keyboard suggests the person's own names, medicines, places and phrases (entered by a caregiver in Settings, stored only on the device) and learns words they use |
| [`/v4`](v4/) | **v4 — keyboard + phrases** | Free-text typing with word + next-word prediction and a quick phrase board (the spelling fallback) |
| [`/v3`](v3/) | **v3 — scanning mode** | Row–column **scanning** with a selectable switch (eyebrow raise, mouth open, blink, or Space) |
| [`/v2`](v2/) | **v2 — word prediction** | A prediction row (backed by a trie) suggests word completions |
| [`/v1`](v1/) | **v1 — nose-tracking keyboard** | Head-pointer cursor, dwell-to-select, blink-to-select, and text-to-speech |

**How the command board speaks to a voice assistant:** it says the command out loud (e.g., "Alexa,
turn on the air conditioner"). A nearby Echo/Nest/HomePod hears it and acts — no cloud API, account,
or internet link required, and nothing is uploaded. Edit the `MENU` list near the top of
`v5/index.html` to customise commands and contact names.

**Personal words (v5):** in Settings → *Personal words for typing*, a caregiver can enter names, medicines,
places and favourite phrases. The keyboard suggests these first (e.g. "met" → "Metformin", "my m" → "My
medicine is due."), and words the person speaks are ranked higher over time. Everything is kept in the
browser's `localStorage` on that device; *Clear personal words & typing history* removes it.

**Listen mode (v6):** built for blind and low-vision users. Start with any key or a tap anywhere; from
then on everything is spoken. Menus scan one option at a time, each read aloud, and the scan waits for
each option to finish before moving on. After three rounds with no selection it pauses until the switch
is used. The keyboard announces rows ("Letters A to L"), echoes each letter, and has **Read back**. With a
face switch, the app talks the person into camera view and calibrates the gesture to their face; with
**Key or physical switch only** the camera isn't used at all. In two-switch setup Space/arrows move and Enter
selects; Escape returns to the main menu. Tiles are real buttons with labels, so a screen reader works
when scanning is off.

**Starting v6 with no key press (dedicated device).** Browsers don't let a page speak until someone
presses a key or taps it, so by default v6's start screen is silent until then. On a computer set aside
for one person, a caregiver can set Chrome to allow it:

1. Make a Chrome shortcut that opens SpeakEasy with sound allowed, e.g. on a Mac:
   `open -na "Google Chrome" --args --autoplay-policy=no-user-gesture-required https://<your-username>.github.io/speakeasy-aac/v6/`
   (on Windows, add `--autoplay-policy=no-user-gesture-required` to the shortcut's *Target*).
2. Open it once and allow the camera.

From then on v6 says "Welcome to SpeakEasy" as soon as it opens and starts scanning by itself. If the
camera hasn't been allowed yet, it says "Press any key, or tap the screen, to start." Safari, iPhone and iPad
have no such setting, so there the first key press or tap is still needed (VoiceOver reads the start button).

Every version has an **All versions** button in the top bar that returns to the landing page.

Each version is a single self-contained HTML file (`vN/index.html`). Later versions include
everything from the earlier ones.

## How it works

- **Face / head tracking:** [MediaPipe Face Landmarker](https://ai.google.dev/edge/mediapipe/solutions/vision/face_landmarker)
  (`@mediapipe/tasks-vision`), loaded from a CDN. Runs in-browser via WebAssembly, returns 478 face
  landmarks per frame. The cursor is driven by landmark #1, the nose tip.
- **Blink switch:** the same library's facial "blendshape" scores (`eyeBlinkLeft`/`eyeBlinkRight`).
- **Speech output:** the browser's Web Speech API (`speechSynthesis`).
- **Word prediction (v2+):** an on-device trie seeded with common English + AAC words.
- **Input methods → one path:** head-pointer, blink, and scanning all feed a single `commit()`
  function. This is the seam where an SSVEP/brain-computer-interface input can plug in later.

## Run locally

The camera needs a secure context (a real address, not a double-clicked file):

```bash
# from the repo folder
python3 -m http.server
# then open http://localhost:8000 in Chrome
```

## Host on GitHub Pages

1. Create a new GitHub repo named `speakeasy-aac` and upload the contents of this folder
   (or push with git).
2. In the repo: **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**.
4. Branch: **main**, folder: **/ (root)**. Save.
5. Wait ~1 minute; your site appears at `https://<your-username>.github.io/speakeasy-aac/`.
   Versions live at `/v1/`, `/v2/`, `/v3/`.

(The included `.nojekyll` file tells Pages to serve the files as-is.)

## Notes for the Congressional App Challenge

- MediaPipe and the Web Speech API are permitted, **documented** external libraries. The original
  work — selection logic, UI, dwell/blink handling, word prediction, scanning, and integration —
  is the student's own and can be explained on request.
- Keep this README and the dependency list in the repo for the submission.

## License

MIT — see [LICENSE](LICENSE).
