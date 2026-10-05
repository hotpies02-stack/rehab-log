# Rehab log (standalone)

A personal rehab logger that installs on your phone and works fully offline. Your data stays on your phone. Nothing is sent anywhere.

## Files

| File | What it does |
|---|---|
| `index.html` | The whole app |
| `manifest.webmanifest` | Lets the app install with its own icon |
| `sw.js` | Makes it open with no connection |
| `icon-192.png`, `icon-512.png`, `icon-maskable-512.png` | App icons |
| `README.md` | This guide |

## First-time setup (about 10 minutes, all from your phone)

1. **Create a GitHub account** at github.com.
2. **Create a repository.** Tap **+** then **New repository**. Name it `rehab-log`, set it to **Public**, and tap **Create repository**.
   GitHub Pages is free for public repositories. Only the app's code is public. Your log never leaves your phone.
3. **Upload the files.** In the repository, tap **Add file**, then **Upload files**. Select all the files in this folder, then tap **Commit changes**.
4. **Turn on GitHub Pages.** Go to **Settings**, then **Pages**. Under **Build and deployment**, choose **Deploy from a branch**, branch **main**, folder **/ (root)**, then **Save**.
5. **Wait a minute or two.** Your app's address appears at the top of the Pages screen: `https://YOUR-USERNAME.github.io/rehab-log/`
6. **Install it.** Open that address in **Chrome**, tap the **⋮** menu, then **Add to home screen** (or **Install app**). It now has its own icon and opens full screen.
7. **Open it once while online.** That saves everything it needs for offline use.

## Moving your data across from the Claude version

1. In the Claude version, go to **Program**, then **Rest timer and backup**, then **Copy backup as text**. Copy the text.
2. In the new app, go to **Program**, then **Rest timer, backup and sharing**, then **Restore from pasted text**. Paste and tap **Restore**.

Your sessions, check-ins, program, history and cue cards all come across.

## Backups

Your log lives only on this phone. The app reminds you weekly to save a backup file to your Downloads. If your phone is lost or reset, install the app again and use **Restore from file**.

## Updating the app

1. Ask Claude for the change. Claude gives you a new `index.html`.
2. In the repository, open `sw.js`, tap the pencil icon, change `rehab-log-v1` to the next number (`rehab-log-v2` and so on), and commit. This tells phones there's an update.
3. Upload the new `index.html` (**Add file**, then **Upload files**) and commit.
4. Open the app. The update takes effect the next time you open it.

Keep the latest `index.html` in your Claude project files so Claude always edits the current version.

---

## For Claude: paste code format

The app has no Claude connection. Program changes and cue cards arrive as a JSON code that the user pastes into **Program**, then **Paste from Claude**. The app finds the JSON (a fenced code block, or the text from the first `{` to the last `}`), shows a review screen, and applies nothing until the user confirms.

Transcribe only what the physio wrote. Don't add, change or suggest exercises, and list anything unclear in `unclear`.

### Changes to the current program (for example, a sticky note)

```json
{
  "rehablog": 1,
  "title": "Physio note, 9 Oct",
  "ops": [
    {"op": "add", "block": "gym", "after": null,
     "exercise": {"name": "Single leg squat onto bench", "sets": 3, "target": "10 reps each leg",
                  "kind": "reps", "reps": 10, "secs": null, "rpe": null, "notes": "",
                  "sided": true, "weight": false, "kg": null}},
    {"op": "update", "block": "gym", "name": "Single leg block RDL", "set": {"sets": 3, "target": "8 reps each side", "reps": 8}},
    {"op": "remove", "block": "warmup", "name": "Side plank (short lever)"}
  ],
  "unclear": ["Note doesn't say warm-up or gym, so added to Gym"]
}
```

- `block` is `warmup`, `gym` or `conditioning`.
- `update` and `remove` must use the exercise's exact existing name, or the session ID for conditioning.
- `update.set` holds only the fields that change.
- Exercise fields are `name`, `sets`, `target` (as written), `kind` (`reps`, `time` or `distance`), `reps` (top of range), `secs` (hold seconds), `rpe`, `notes`, `sided` (each side or single leg), `weight` (load applies) and `kg`.
- Conditioning fields are `id`, `erg`, `work` (seconds), `rest` (seconds), `sets` and `rpe`.

### A whole new program (replaces any block included)

```json
{"rehablog": 1, "title": "Physio plan, week of 12 Oct",
 "warmup": [EXERCISE, ...] or null,
 "gym": [EXERCISE, ...] or null,
 "conditioning": [CONDITIONING, ...] or null,
 "unclear": []}
```

Use `null` for a block the sheet doesn't cover, so the app keeps the current one.

### Cue cards (can be combined with either of the above)

```json
{"rehablog": 1, "cues": {
  "Exact exercise name": {
    "what": "One sentence: what it is and the main muscles",
    "setup": ["2 to 4 steps"], "move": ["2 to 5 steps"],
    "cues": ["2 to 4 form cues"], "mistakes": ["2 to 4 mistakes, each with its fix"],
    "search": "YouTube search phrase"}}}
```

Cue cards are general technique guidance only. No diagnosis, no medical advice, no changes to sets, reps, load or progression, and nothing that contradicts the physio's notes. Use plain Australian English.
