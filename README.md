## Learny – A tiny study companion on your desktop

✨ **Learny** is a minimal macOS desktop app that combines:

- ⏱️ A focused 30‑minute Pomodoro timer  
- ✅ A structured To Do list (Up Next / Current / Anytime / Completed)  
- 💬 A small AI chat window (Gemini / OpenAI / Cerebras)  
- ☁️ Optional iCloud sync + daily summary export  

All running in a tiny always‑on‑top window that sits quietly on your screen and keeps you company while you study or work.

---

## Features

### ⏱️ Pomodoro (30 min)

- One‑click **30‑minute focus session**.
- Simple controls: **Start / Pause / Reset**.
- A small “Time’s up” hint when the session finishes.
- Designed to stay visible but unobtrusive on your desktop.

### ✅ To Do list that matches your energy

Learny’s To Do list is split into gentle, time‑aware groups:

- **Up Next (30 min)** – tasks you want to do in the next half hour.  
- **Current** – the *one* thing you’re focusing on right now.  
- **Overdue / Other** – items with due times or scheduled today.  
- **Anytime** – low‑pressure tasks you can do whenever.  
- **Completed** – finished items with timestamps for end‑of‑day review.

You can:

- Add, check off, and delete tasks quickly.  
- Use **clickable “Move” arrows** or **drag from Anytime/Overdue** into `Current` / `Up Next (30 min)` when you decide to focus.  
- Keep the To Do area and the “Say something” chat in a carefully balanced layout so both stay visible and usable.

### 💬 Friendly AI chat (“Say something?”)

Learny includes a tiny chat area at the bottom:

- Works with **Gemini**, **OpenAI**, or **Cerebras** (you choose).  
- API keys are stored **locally only** on your device.  
- You can set a **custom system prompt** in Settings to make the AI feel like:
  - a warm friend,
  - a study buddy,
  - or a concise productivity coach.
- The chat:
  - Supports **copy / paste**;  
  - Shows **emoji** correctly;  
  - Auto‑linkifies URLs so you can click to open them in your browser.

### ☁️ iCloud sync & daily summary (optional)

If you want cross‑device sync:

- In **Settings → Advanced**, choose a **Daily summary & sync folder** (iCloud Drive recommended).
- Learny will:
  - Keep a hidden `.studywithme-todos-sync.json` in that folder to sync your To Do list between Macs;
  - Write a **Markdown daily summary** file `studywithme-daily.md` with:
    - A short English title and summary paragraph;
    - The list of completed tasks for the day.

You can also manually trigger an export from Settings when you like.

---

## Prerequisites (for development)

- **Node.js** 18+ and **npm**
- **Rust** (for Tauri): [rustup](https://rustup.rs)
- **macOS** (for building the `.app` / DMG; the app targets macOS)

---

## Quick start (development)

```bash
# Install dependencies
npm install

# Run in development (Tauri + Vite)
npm run tauri dev
```

In the app: open **Settings** (gear icon), add your **API key** (Gemini / OpenAI / Cerebras), then use chat and “Classify & timeline with AI” as needed.

---

## Build

| Command              | Output                                                                 |
|----------------------|------------------------------------------------------------------------|
| `npm run build`      | Frontend only → `dist/`                                                |
| `npm run tauri build`| macOS app + DMG → `src-tauri/target/release/bundle/**` (Learny.app + DMG) |

To install on another Mac:

1. Copy the generated **`Learny_0.1.0_aarch64.dmg`** (or the `Learny.app` bundle).  
2. Open the DMG and drag **Learny.app** into **Applications**.  
3. Launch Learny from Applications or Spotlight.

---

## Basic usage

### 1. Choose and configure your AI provider

1. Click the **Settings (gear)** icon in the header.  
2. Under **“Use for chat”**, choose:
   - Gemini (Google)  
   - OpenAI (ChatGPT)  
   - Cerebras  
3. Paste your API key for the selected provider.  
4. Click **Test connection** until you see the **green “API connected”** badge in the header.

You can also:

- Choose a **model** (e.g. `gemini-1.5-flash`, `gpt-4o-mini`, `llama3.1-8b`) or leave it on `Auto`.  
- Set a **Custom system prompt** to change the assistant’s personality (warm friend, strict coach, etc.).

### 2. Add and organize tasks

- Use the `Add a task...` input to create new tasks.  
- Quickly categorize with:
  - **Up Next (30 min)** button – plan your next half hour;  
  - **Anytime** button – low‑pressure tasks.
- Use:
  - **Checkbox** to mark done / undone;  
  - **Small arrows** (↑ / ↓ and ↗ Move) to change priority or move a task between groups;  
  - **Drag & drop** from `Anytime` / `Overdue` into `Up Next` / `Current` to choose what to focus on now.

### 3. Focus with Pomodoro

1. Make sure the task you want to work on is in **Current**.  
2. Start the timer in the **Pomodoro (30 min)** section.  
3. When time’s up, decide whether to:
   - Mark the task done,  
   - Or move it back to Anytime / Overdue.

### 4. Chat with the AI

At the bottom:

- Type into the **“Say something?”** input.  
- Press **⌘ + Enter** to send, `Esc` to clear and blur.  
- You can:
  - Copy text from the chat;  
  - Paste tasks or links;  
  - Click links to open them in your browser;  
  - Use emoji naturally in messages.

### 5. Syncing tasks on a new Mac (optional)

If you install Learny on another Mac and want your tasks there:

1. Open **Settings → Advanced → Daily summary & sync folder** and choose the *same* iCloud folder you used on your first Mac.  
2. Close Settings and click the app title at the top (“StudywithMe / Learny”).  
   Learny will read `.studywithme-todos-sync.json` from that folder and restore your tasks.

---

## Privacy & data

- 🔐 **All data is local by default**:
  - Tasks, prompts, settings, API keys are stored only on your Mac (in local storage + your chosen folder).
- ☁️ If you enable iCloud sync:
  - Only the To Do JSON + summary Markdown are saved into your iCloud Drive folder, under your own Apple ID.
- 🌐 When you chat:
  - Requests go **directly** to the provider you selected (Gemini / OpenAI / Cerebras).
  - Learny does **not** run any external server and does not proxy or log your messages.

---

## Project structure

```text
Learny/
├── src/
│   ├── App.vue          # Main UI (Pomodoro, todos, chat, settings)
│   ├── main.ts
│   ├── config.ts        # Env and localStorage keys
│   ├── vite-env.d.ts
│   └── services/
│       └── ai.ts        # AI provider routing & prompts
├── src-tauri/           # Rust backend (Tauri 2)
│   ├── src/
│   │   ├── lib.rs       # Commands: file read/write, network helpers, etc.
│   │   └── main.rs
│   ├── tauri.conf.json
│   └── icons/
├── public/
│   ├── favicon.svg
│   └── app_logo.png
├── scripts/
│   └── create-dmg.sh    # (Optional) custom DMG styling script
├── index.html
├── vite.config.ts
├── tsconfig.json
└── package.json
```

---

## Tech stack

- **Frontend:** Vue 3 (Composition API), TypeScript, Vite 6  
- **Desktop:** Tauri 2 (Rust)  
- **AI providers:** Gemini, OpenAI, Cerebras (user-supplied keys)

---

## License

Private / unlicensed. Use and modify as you like on your own machines.
