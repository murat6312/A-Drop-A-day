# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes, plus the working rules for this repository.

Tradeoff: These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

Don't assume. Don't hide confusion. Surface tradeoffs.

Before implementing:

* State your assumptions explicitly. If uncertain, ask.
* If multiple interpretations exist, present them - don't pick silently.
* If a simpler approach exists, say so. Push back when warranted.
* If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

Minimum code that solves the problem. Nothing speculative.

* No features beyond what was asked.
* No abstractions for single-use code.
* No "flexibility" or "configurability" that wasn't requested.
* No error handling for impossible scenarios.
* If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

Touch only what you must. Clean up only your own mess.

When editing existing code:

* Don't "improve" adjacent code, comments, or formatting.
* Don't refactor things that aren't broken.
* Match existing style, even if you'd do it differently.
* If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:

* Remove imports/variables/functions that YOUR changes made unused.
* Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

Define success criteria. Loop until verified.

Transform tasks into verifiable goals:

* "Add validation" → "Write tests for invalid inputs, then make them pass"
* "Fix the bug" → "Write a test that reproduces it, then make it pass"
* "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. Project Context

This repository is one app in a for-profit portfolio of cross-platform apps (React Native + Expo + TypeScript) published to both Google Play and the Apple App Store. The business process, default tech stack, store constraints, monetization defaults and marketing playbook are defined in @STRATEGY.md and apply here.

Market research (Faz 1) for this app is already done and approved; the product brief lives in `docs/PRODUCT.md`. Build what the brief says. If a request conflicts with the brief, say so before coding.

## 6. Working Rules for This Repository

* **Language:** talk to the user in Turkish. Code, identifiers, comments, commit messages and docs are in English.
* **No AI attribution, anywhere.** No `Co-Authored-By: Claude`, no "Generated with Claude Code", no session links, no "written by AI" wording in commits, PR descriptions, code comments or docs. `.claude/settings.json` turns the automatic trailers off; do not add them by hand either.
* **Plan before features.** For any feature beyond a small fix: propose the approach (files, data flow, libraries) and wait for approval, then implement with a verifiable success criterion.
* **Cloud sessions can't run the app.** There is no emulator, device or Expo Go in the cloud. Verify with `npx tsc --noEmit`, `npx expo-doctor`, the linter and unit tests. Do not start `expo start`, EAS builds or anything that needs a device. The user tests on a physical device locally.
* **Branch and PR.** Work on a feature branch, push it, and open a PR with a short description of what changed and how it was verified.
* **Secrets** never go into the repo. RevenueCat/API keys are read from `app.config.ts` `extra` + EAS secrets or environment variables.
