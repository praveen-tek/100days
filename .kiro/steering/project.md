# Steering — 100 Days Left (2026) Widget

## What this is
A personal-development motivation app with a home-screen widget (Android + iOS) that shows how many days are left in the last 100 days of 2026, pushing the user to act with urgency.

## Stack
- Expo (managed workflow unless a native module forces a prebuild)
- expo-router for navigation
- react-native-android-widget (Android widget)
- Native iOS Widget via WidgetKit + expo config plugin (or expo-apple-targets)
- TypeScript strict mode
- date-fns for date math

## Core rules
1. Single source of truth for the countdown: one util (`lib/countdown.ts`) computes days remaining. Widgets and app UI both read from it. Never recompute the date math in more than one place.
2. Widget data updates via a background task (expo-task-manager / expo-background-fetch) — do not rely on the widget polling on its own.
3. No hardcoded dates outside `lib/countdown.ts`. Target date = 2026-12-31, window start = 2026-09-23 (last 100 days).
4. Keep the widget UI dumb: it renders props it's given, no logic inside widget components.
5. State kept minimal — app has no backend; local storage only (expo-secure-store / AsyncStorage) for streaks/settings.

## Folder structure
```
app/            expo-router screens
components/     shared UI
widgets/
  android/      react-native-android-widget components
  ios/          WidgetKit target (Swift) + config plugin
lib/            countdown.ts, notifications.ts, storage.ts
constants/      colors, typography
```

## Naming
- Components: PascalCase
- Files: kebab-case except component files (PascalCase)
- Hooks: `useXyz`

## Design
- One primary number on screen at all times: days remaining.
- Motivational copy rotates from a fixed local array, no network calls.

## Out of scope for v1
- Accounts, sync across devices, cloud backup
- Custom themes beyond light/dark
- Android home-screen widget resizing beyond 2x2 and 4x2

## Open decisions
- [ ] Notification time (fixed daily reminder time TBD)
- [ ] Whether iOS widget ships in v1 or Android-first