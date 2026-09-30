# Product Brief

Status: approved scope for v1 (2026-09-30). Market research summary is at the bottom; the full report lives outside this repo.

## One-liner

A free, ad-free mobile app (Turkish + English) that delivers one bite-sized item per notification — a fun fact, a joke, or a motivational quote — from the categories the user picked, at the times the user chose. The text is readable inside the notification; tapping opens the item with its source.

## Goals

- Learning project first: Expo notifications, widgets, i18n, EAS build/submit, store listings on both stores.
- Public GitHub project.
- No revenue goal for v1. If it gains users, monetization (RevenueCat) and a content backend come later.

## Decisions already made (do not reopen)

| Topic | Decision |
|---|---|
| Categories | Facts, Jokes, Motivation. All three ship in v1. |
| Onboarding choice | Multi-select, at least one category. Changeable later in Settings. |
| Languages | Turkish and English. Default from device locale; switchable in Settings. Each language has its own content pool (no machine translation of items). |
| Backend | None. All content is bundled in the app as JSON. No accounts, no sync, no analytics, no ads, no purchases, no in-app AI. |
| Content volume at launch | 100–150 items per category per language (600–900 total). |
| Personalization | v1: like → Favorites; dislike → never shown again. Both stored locally. MBTI / cognitive-function-based personalization (Ne/Se/Ni/Si) is a future idea, out of scope. |
| Monetization | None in v1. Do not build paywall scaffolding. |
| Name | **Damla** ("drop" in Turkish — a drop a day). Store title carries the keywords, e.g. "Damla: Günlük Bilgi & Söz" / "Damla: Daily Facts & Quotes". Not "Daily Reminder" (collides with to-do/reminder apps). |

## Content rules

- Facts: each item has a source URL. Preferred source: Wikipedia (TR / EN). Show the source on the detail screen with a CC BY-SA 4.0 notice where Wikipedia text is used. Every fact is checked against its source before it enters the pool. No AI-hallucinated "facts".
- Motivation: quotes from authors dead for 70+ years (public domain), widely circulated anonymous sayings, or original lines. Always show the author. Never copy a living creator's captions or another app's curated collection.
- Jokes: original, or widely circulated anonymous jokes retold in our own words. No comedian bits, no scraping joke sites, nothing adult or discriminatory (store rating 4+ / Everyone).
- Item length: notification body under 150 characters; a longer `detail` field is optional and shown on tap.
- Content file: `content/<lang>/<category>.json`, items `{ id, text, detail?, author?, sourceUrl?, tags? }`. IDs are stable and unique across the whole pool.

## v1 features

1. Onboarding: language (prefilled from device), category multi-select (≥1), items per day (1–3, default 1), delivery time window. Notification permission is requested after this step, and the app is fully usable without it.
2. Today: the items scheduled for today, one card each.
3. Item detail: full text, author/source, like, dislike, share, favorite.
4. Favorites and History (everything already delivered, newest first, filter by category).
5. Share as an image card (text + author/source + app name).
6. Settings: categories, items per day, time window, language, reset seen items.
7. Home-screen widget showing the latest item (iOS + Android) — target for v1; if it cannot be done inside the Expo managed workflow with the current SDK, defer to v1.1 and say so.

Out of scope for v1: backend, accounts, sync, analytics, ads, purchases, streaks, quizzes, Live Activities, rich-media notifications, AI generation, MBTI personalization.

## Notification behaviour

- Local notifications only (`expo-notifications`); no push server.
- Scheduling: keep roughly 30 days of notifications scheduled ahead (iOS caps pending local notifications at ~64); re-fill on every app open. Items per day × 30 must stay under the cap.
- Selection: draw from the user's categories, alternating categories when several are selected; never repeat an item until the pool for that category/language is exhausted (seen IDs stored locally); skip disliked items.
- Body is the item text itself (title = category label); tapping opens the item detail.
- Android 13+ runtime permission and iOS permission are both requested in-app after onboarding; denial must not break anything.

## Tech

- Expo (latest SDK, New Architecture), TypeScript strict, expo-router, Zustand for the small global state (settings, seen/liked IDs), AsyncStorage persistence.
- `expo-localization` + `i18n-js` for UI strings (`locales/tr.json`, `locales/en.json`).
- EAS Build for device builds (notifications are not testable in Expo Go on iOS); the user tests on a physical iPhone and an Android device.
- Feature-based folders: `features/onboarding`, `features/today`, `features/items`, `features/notifications`, `features/settings`, `content/`, `locales/`.

## Store plan

- iOS first (EN + TR listing), then Google Play (closed test with 12 testers × 14 days runs in parallel).
- Privacy: no data collected → simplest possible privacy labels / Data Safety form. Privacy policy URL still required.
- Age rating 4+ / Everyone; content must respect that.

## Verification for v1 (definition of done)

- `npx tsc --noEmit`, lint and unit tests pass (unit tests for the selection/scheduling logic: no repeats, category alternation, cap respected).
- On a physical iPhone and an Android device: notifications arrive at the chosen times with the chosen categories; body shows the text; tapping opens the detail; app works with permission denied.
- Both languages render every screen; switching language switches the content pool.
- Content pools reach 100–150 items per category per language, each fact with a source URL.

## Market research summary (Sep 2026)

Competitor sweep of both stores (US + TR) found no app that combines category choice + daily notification + TR/EN. Close cousins (Daily Dose: Facts Jokes Quotes; Small Talk – Daily Learning) bundle everything into one English feed and have little traction. Motivation quotes are dominated by two apps; joke apps show no revenue; fact apps have one leader. Conclusion was NO-GO on revenue grounds; the project proceeds as a learning/portfolio build with no revenue expectation. Store-policy notes that shaped the scope: Google's spam policy prefers one aggregated app over sibling apps; Apple 4.2 requires more than a list plus notifications (hence favorites, history, share, widget); notification permission cannot be mandatory.
