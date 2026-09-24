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
| [`/v5`](v5/) | **v5 — command board (recommended)** | Speaks **whole commands in 1–2 selections** — smart-home control ("Alexa, turn on the AC"), urgent needs, and calls — for people for whom spelling is too slow. Selectable wake word (Alexa / Google / Siri) |
| [`/v4`](v4/) | **v4 — keyboard + phrases** | Free-text typing with word + next-word prediction and a quick phrase board (the spelling fallback) |
| [`/v3`](v3/) | **v3 — scanning mode** | Row–column **scanning** with a selectable switch (eyebrow raise, mouth open, blink, or Space) |
| [`/v2`](v2/) | **v2 — word prediction** | A prediction row (backed by a trie) suggests word completions |
| [`/v1`](v1/) | **v1 — nose-tracking keyboard** | Head-pointer cursor, dwell-to-select, blink-to-select, and text-to-speech |

**How the command board speaks to a voice assistant:** it says the command out loud (e.g., "Alexa,
turn on the air conditioner"). A nearby Echo/Nest/HomePod hears it and acts — no cloud API, account,
or internet link required, and nothing is uploaded. Edit the `MENU` list near the top of
`v5/index.html` to customise commands and contact names.

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
