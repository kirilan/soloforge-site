# Solo Forge website — build plan

Handoff plan for building the Solo Forge landing site in THIS folder (`C:\Users\kiril\soloforge-site`).
The app repo lives at `C:\Users\kiril\mobile` — read-only source for assets and copy; never commit there.

## Decisions already made (don't re-ask)

- **Hosting:** Cloudflare Pages, connected to a new GitHub repo `kirilan/soloforge-site`.
- **Domain:** a subdomain of the user's existing Cloudflare-managed domain (ask which subdomain name when wiring DNS — that's the only open question).
- **Scope:** exactly two pages — `index.html` (landing) and `privacy.html` (standalone privacy policy; Google Play requires a dedicated URL).
- **Tech:** plain HTML + one CSS file. No framework, no build step, no JS unless something truly needs it (a screenshot lightbox does not). Dark-ish theme is a reasonable fit for a fitness app but designer's choice.

## Content sources (copy, don't invent)

All marketing copy already exists in the app repo:

- App name: **Solo Forge**
- Tagline: `C:\Users\kiril\mobile\fastlane\metadata\android\en-US\short_description.txt`
  ("Local-first intermittent fasting, calorie, weight & workout tracker")
- Body copy / feature list: `...\en-US\full_description.txt` — use it nearly verbatim; it's accurate and already tuned.
- Assets to copy into `assets/`:
  - `...\en-US\images\icon.png`
  - `...\en-US\images\featureGraphic.png` (hero background or banner)
  - `...\en-US\images\phoneScreenshots\1_home.png … 5_settings.png`

## index.html structure

1. **Hero** — icon, name, tagline, and the download badges:
   - "Get it on F-Droid" official badge → `https://f-droid.org/packages/com.kbul.spicycrab/`
   - GitHub badge/link → `https://github.com/kirilan/SoloForge/releases` (note: debug-signed APKs, not cross-installable with the F-Droid build)
   - No Google Play badge yet — the Play listing is still in review; leave an HTML comment where it goes.
2. **Screenshots** — the 5 phone screenshots in a horizontal scroll row or simple grid.
3. **Features** — the bullet list from full_description.txt.
4. **Privacy-first section** — the differentiator; make it prominent: GPL-3.0, no backend, no accounts, no analytics, no crash reporting, fully offline, backup to a single local file.
5. **AI food analysis explainer** — short, honest: the ONE optional network feature sends food photos/text to OpenRouter using the *user's own API key*; off by default, zero network calls otherwise.
6. **Footer** — GPL-3.0 (link to LICENSE in repo), source link, privacy policy link.

## privacy.html

Write it from the app's actual behavior (verified in the app repo's CLAUDE.md):

- No data collection by the developer. No backend, no accounts, no analytics, no crash reporting.
- All data (meals, weight, fasting history, workouts, settings) stays on-device; Android cloud backup disabled.
- The only network call: user-initiated food analysis → `openrouter.ai`, using the user's own API key, governed by OpenRouter's privacy policy (link it). Feature is opt-in and can be disabled.
- The website itself: static, no cookies, no analytics. (Keep it true — don't add Cloudflare Web Analytics.)
- Contact: kickbul@gmail.com. Date the policy.

## Deploy steps

1. `git init`, commit, create GitHub repo `kirilan/soloforge-site` (`gh repo create`), push.
2. Cloudflare dashboard → Workers & Pages → Pages → connect the repo, framework preset "None", no build command, output dir `/`.
3. Custom domains → add the chosen subdomain (DNS is same-account, so it's automatic).
4. Verify both pages render on mobile width (~375px) — most visitors arrive from a phone.

## Skipped on purpose

- Blog, FAQ, changelog page, i18n, donation links (none set up), OG-image generation beyond reusing featureGraphic.png, analytics of any kind.
- Add a Play badge only after the Play listing is live.
