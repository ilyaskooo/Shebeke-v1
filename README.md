# ŞƏBƏКƏ — social network

This package contains the existing SHEBEKE web/API project plus an Android build layer.

## What is included
- React/Vite web application
- Node.js/Express API
- PostgreSQL
- Socket.IO realtime layer
- SMTP email verification
- media upload
- AI assistant endpoint
- Capacitor Android configuration
- GitHub Actions workflow that generates the Android project and builds a debug APK

No fake users or fake posts are seeded.

## Run web/API
1. Copy `.env.example` to `.env`.
2. Configure database, JWT and real SMTP values.
3. Run:
   `docker compose -f infra/docker-compose.yml up --build`

## Build Android locally
Requirements: Node.js 22+, Java 21, Android Studio/SDK.

From the repository root:
```bash
npm install
npm --prefix apps/web install
npm --prefix apps/mobile install
npm --prefix apps/web run build
cd apps/mobile
npx cap add android
npx cap sync android
cd android
./gradlew assembleDebug
```

The APK is:
`apps/mobile/android/app/build/outputs/apk/debug/app-debug.apk`

## Build APK in GitHub
Push this repository to GitHub. The workflow `.github/workflows/build-android.yml` runs on pushes to `main` and can also be started manually from Actions.

Set repository variable:
`VITE_API_URL=https://YOUR-DEPLOYED-API`

After the workflow succeeds, download the `shebeke-debug-apk` artifact from the workflow run.

## Important
An APK is only useful as a real social network when the API is deployed and `VITE_API_URL` points to it. SMTP and AI credentials stay on the server and must never be embedded in the Android app.
