# Calorie & Macro Tracker — Spec

## Overview
A personal app to track daily calorie and macronutrient (protein, fat, carbs) intake against user-set targets, by logging food items under each meal of the day.

## Definitions
- **macros/macronutrients/meters** — calories, protein, fat, carbs. Use standard units (SI).
- **ingredients/products** — contain macro values.
- **recipe** — contains a list of ingredients and their amount.
- **meal** — breakfast, lunch, supper, extra — each can contain a list of products/recipes.

## User Stories

1. I want to set a daily target for each macro (once 3 of the macros are filled the fourth can be calculated automatically).
2. I want to set my current weight and LBM (lean body mass) values.
3. Alternative way to set daily target is by setting macros per weight or LBM weight — show the calculated target.
4. I want to be able to change anything that can be set (targets, LBM, weight, etc).
5. I want to be able to insert a new product and set its macros.
6. I want to be able to insert new recipes; its macros are calculated from the ingredients.
7. I can set macros for ingredients as 100 g or ml (depending on what is more appropriate).
8. I can set custom measurements like spoon, tablespoon, average meal.
9. I can search recipes and products.
10. I can add products and recipes to a meal.
11. I can see my current daily macro intake and what is left for the day.
12. I can see a weekly sum of my total macros and how much is left for this week.
13. All tracking should be persistent.
14. When adding a product or recipe to a meal, I can specify a serving/quantity, but it's not mandatory.
15. Recipe macros are shown per 100g by default. I can optionally specify a cooking adjustment factor (either one overall percentage, or per-macro) to correct for changes during cooking (e.g. water loss) and get accurate post-cooking macros.
16. Full editing and deletion is supported for any logged entry (products, recipes, meals), not just settings.
17. Weight and LBM are tracked as a history over time, not a single overwritten value, so I can see trends.
18. The weight/LBM-based target formula (macros per kg of weight/LBM) is user-editable, not predetermined.
19. The weekly period is configurable — either the last 7 days (rolling), a fixed week (e.g. Sunday–Saturday), or another user-defined boundary.
20. I can look back at history — past days and weeks, not just today/this week.
21. A day is defined as 00:00–23:59.
22. Global units (cup, tablespoon, teaspoon, etc.) are predefined in ml, and I can add my own custom units on top — custom units are also global, not tied to a specific product.
23. Backups of my data are supported (beyond just persistence surviving a reload).

## Deployment & Persistence
- **Data storage:** Firestore (same pattern as split-builder), so data is backed up and not tied to a single device/browser.
- **Hosting:** GitHub repo + GitHub Pages, built with Claude Code, same workflow as split-builder.
- **Update detection:** the app should detect when a newer version has been deployed and prompt to reload (same pattern as split-builder's version-check banner) — not necessarily a silent auto-update.
- **Mobile use:** the app should be fully usable for day-to-day logging from a phone (add to home screen / PWA-style), not just desktop.
- **Auth & security:** real login (e.g. email/password or Google sign-in) via Firebase Auth, tied to an actual account rather than an anonymous device ID — so data survives switching/reinstalling devices. Firestore security rules restrict read/write to the logged-in user's own data. Login persists per device (sign in once, stays signed in), not required every time the app opens.

## Units
- Global default conversions (in ml): 1 teaspoon = 5ml, 1 tablespoon = 15ml, 1 cup = 240ml. Editable per story 22 (custom units can be added/adjusted).

## Display
- **Rounding:** calories rounded to the nearest whole number; protein/fat/carbs rounded to 1 decimal place.
- **Version display:** current app version number is visible in the UI, plus a manual "check for updates" / force-update button (in addition to the automatic update-detection banner).

## Out of Scope (for now)
- Micronutrients beyond calories/protein/fat/carbs (fiber, sugar, sodium, etc.).
- Barcode scanning or external food-database API integration — products/recipes are entered manually.
- Multi-user support — single account, built for personal use only.

## Notes for Implementation (Claude Code)
- Follow the same stack/pattern as the split-builder repo (github.com/Matanyaa/split-builder): static HTML/CSS/JS, Firestore for storage, GitHub Pages for hosting, version-check banner for updates — no build step/bundler needed unless it becomes necessary.
- Where this spec says something is user-editable/configurable (e.g. the weight/LBM target formula, week boundary, cooking adjustment factor), don't hardcode a default silently — surface it as a setting.
- Items marked TBD/open elsewhere in this doc are intentionally undecided — flag a suggested default rather than assuming.
- Commit and push after each working version/change (same as split-builder's workflow) — don't batch up multiple unrelated changes into one commit.

## Prerequisites (before starting)
- [ ] Create a new empty GitHub repo (e.g. `macro-tracker`).
- [ ] Create a new Firebase project in the Firebase console, enable **Firestore** and **Authentication** (enable Email/Password and/or Google sign-in provider).
- [ ] Have the Firebase config (from Project Settings) ready to hand to Claude Code.
- [ ] Place this spec file in the repo root before opening Claude Code there, so it can read it directly.
