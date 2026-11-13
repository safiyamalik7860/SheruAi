# SHERU AI — Web + Android starter

A responsive React chat interface with an Express API, SQLite-backed email/password accounts, AI chat and image input, browser speech recognition, and text-to-speech. Capacitor wraps the web app for Android.

## Requirements

- Node.js 20 or newer and npm
- An OpenAI API key for live AI replies and image understanding
- Android Studio + Android SDK only if building the Android app

## 1. Install and run

```bash
npm install
cp .env.example .env
```

Open `.env` and set:
- `OPENAI_API_KEY` to your API key
- `JWT_SECRET` to a long random secret (do not share it)

Start the backend in terminal 1:

```bash
npm run server
```

Start the frontend in terminal 2:

```bash
npm run dev
```

Open the local URL printed by Vite (usually http://localhost:5173).

For a single-server production-style run:

```bash
npm run build
npm start
```

Then open http://localhost:8787.

## 2. Android app

Install Android Studio and its Android SDK. Then:

```bash
npm run android:add
npm run android:sync
npm run android:open
```

In Android Studio, let Gradle sync, select an emulator or connected test device, and press Run. To make a release build, use Android Studio's Build > Generate Signed Bundle / APK wizard. Keep signing keys private.

**Important:** Android app ko AI backend se connect karne ke liye deployed HTTPS API endpoint configure karna hoga. A physical phone's `localhost` is the phone itself, not your development computer. For a phone build, create a `.env.production` file with `VITE_API_BASE=https://your-deployed-api.example.com` before `npm run android:sync` (replace with your real HTTPS backend URL). The backend must allow your app origin through CORS. Do not ship an API key inside the app.

## 3. Features

- Hindi, Hinglish and English chat (model follows the user's language)
- Voice input using browser SpeechRecognition (browser/device support varies)
- Read-aloud using browser SpeechSynthesis
- Image upload (up to 8 MB in UI); image is sent to the vision-capable chat model
- Coding help via normal chat
- Email/password signup and login, bcrypt password hashing, signed JWT
- Responsive desktop and mobile layout

## 4. Security before public launch

This is a starter prototype, not a production-audited service.

- Use HTTPS in production.
- Use a strong, unique `JWT_SECRET`; rotate it if exposed.
- Keep `.env`, API keys, SQLite database, and signing keys out of source control.
- Add rate limiting, request size limits at the reverse proxy, abuse monitoring, account recovery, email verification, and privacy/retention controls before public release.
- Add a proper privacy notice and terms before collecting real user data.
- Consider managed auth and a managed database before scaling.
- Add moderation and age-appropriate safety controls for a public chatbot.
- Never store passwords in plaintext or expose the OpenAI API key in frontend code.

## 5. Troubleshooting

- `AI is not configured yet`: add `OPENAI_API_KEY` to `.env` and restart `npm run server`.
- `Please log in again`: sign in again; the demo token expires after seven days.
- Voice button unavailable: use a supported browser (Chrome commonly supports Web Speech API) and grant microphone permission. Voice recognition is browser-dependent.
- Android build errors: install the Android SDK and a compatible JDK through Android Studio, then rerun `npm run android:sync`.

## Architecture

- `src/`: React UI
- `server/index.js`: auth, chat and health API
- `server/sheru.sqlite`: created automatically on first server start
- `capacitor.config.json`: Android wrapper configuration

## API routes

- `POST /api/auth/signup` — `{ "email": "...", "password": "..." }`
- `POST /api/auth/login` — `{ "email": "...", "password": "..." }`
- `POST /api/chat` — `{ "messages": [...] }`
- `GET /api/health` — service health check
