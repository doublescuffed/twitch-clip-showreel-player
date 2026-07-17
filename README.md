# Streamer.bot Showreel System

Play all Twitch clips created during your current stream with a single chat command (`!showreel`). The showreel plays clips in a dedicated OBS browser source and shows **"Clipped by [username]"** on each clip.

---

## Features

- **Automatic time tracking** – remembers when your stream started.
- **Plays only clips from the current stream** – ignores old clips.
- **Unlimited clips** – plays every clip viewers created during your stream.
- **Creator overlay** – shows "Clipped by [username]" on each clip.
- **No hosting required** – the widget is already hosted and ready to use.

---

## What's Included

| File | Purpose |
|------|---------|
| `Showreel - Stream Start Tracker.sb` | Saves stream start time when you go live. |
| `Showreel - Play Clips.sb` | Fetches, filters and plays clips in OBS. |
| `Showreel - Commands.sb` | Handles the `!showreel` chat command. |
| `index.html` | The clip player widget (hosted on Netlify – no need to host it yourself). |

---

## Requirements

- [Streamer.bot](https://streamer.bot/) (v1.0.4 or later)
- [OBS Studio](https://obsproject.com/) with WebSocket plugin enabled (v5 or compatible)
- A Twitch account (for clips)
- **No local web server needed** – the widget is hosted already!

---

## Quick Setup

### 1. Import Actions into Streamer.bot

1. Open Streamer.bot → **"Actions"** tab.
2. Click **"Import"** and select the `Showreel - Stream Start Tracker.sb` file.
3. Repeat for `Showreel - Play Clips.sb` and `Showreel - Commands.sb`.

---

### 2. Customize the Play Clips Action for Your OBS Setup

**This is the most important step.** You need to tell the system your OBS scene and browser source names.

1. Open the **`Showreel - Play Clips`** action.
2. Click on the **"ExecuteCode"** sub-action.
3. Look for these two lines near the top:

string sceneName = "End Credits";
string sourceName = "Showreel Player";

4. Change them to match your OBS:
   - `sceneName` – the name of the scene where your browser source lives.
   - `sourceName` – the name of your browser source.

**Example:** If your scene is called "End Screen" and your browser source is called "Clip Player", change to:

string sceneName = "End Screen";
string sourceName = "Clip Player";

5. Click **"Compile"** (the hammer icon). You should see **0 errors**.
6. Click **"OK"** to save.

---

### 3. Set Up OBS

- Create a scene (e.g., `End Credits` – this is the scene you just put in the code).
- Add a **Browser Source**:
  - **Name**: exactly as you set in `sourceName`
  - **Local file**: **OFF**
  - **URL**: leave **blank** (Streamer.bot will set it)
  - **Shutdown when not visible**: **OFF**
  - **Refresh browser when scene becomes active**: **ON**
  - Width: 1920, Height: 1080

---

### 4. Set Triggers

1. Open **`Showreel - Stream Start Tracker`**.
   - Add trigger: **Twitch → Channel - Stream Online**.

2. Open **`Showreel - Commands`**.
   - Add trigger: **Command Triggered** → command: `!showreel` (set permission to *Broadcaster*).
   - Inside the sub-actions, ensure the **"Run Action"** points to `Showreel - Play Clips`.

---

### 5. That's It!

1. Start your stream (or run the Start Tracker manually once to set the time).
2. Create a clip from any old VOD (viewers can also create clips if live).
3. Switch to your scene with the browser source.
4. Type `!showreel` in chat.
5. All clips from your current stream will play with the overlay.

---

## Customization

- **Overlay text position / size** – edit the `#overlay` CSS inside the hosted `index.html`.
- **Limit clip count** – in `Showreel - Play Clips`, add `.Take(20)` after the `Where` clause.
- **Change the command** – in `Showreel - Commands`, change the trigger from `!showreel` to whatever you like.

---

## How the Widget Works

The system uses a pre-hosted widget at:  
**https://twitch-clip-showreel.netlify.app/**

This widget is generic – it works for **any Twitch clip** from **any channel**. You don't need to host it yourself. If you prefer to host your own copy, you can replace the `baseUrl` in the `Showreel - Play Clips` code with your own URL.

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| "Stream start time missing" | Run the Start Tracker manually or go live. |
| No clips found | Start time is after clip creation – re-run Start Tracker before creating clips. |
| Clips don't autoplay | Right-click browser source → **Interact**, click play once. |
| Browser source shows 404 | Check the `baseUrl` in the C# code – it must point to a valid hosted widget. |

---

## License

Free to use, modify, and share. If you improve it, consider sharing back!

---

**Enjoy your showreel!**
