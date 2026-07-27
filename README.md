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
| [`/v3`](v3/) | **v3 — full prototype (recommended)** | Row–column **scanning mode**: highlight a row, blink (or press Space) to pick it, then scan the letters — a single-switch path for users who can't control head or gaze |
| [`/v2`](v2/) | **v2 — word prediction** | A prediction row (backed by a trie) suggests word completions to cut the number of selections |
| [`/v1`](v1/) | **v1 — nose-tracking keyboard** | Head-pointer cursor, dwell-to-select, blink-to-select, and text-to-speech |

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
