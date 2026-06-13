# BLACKOUT — a hacking-contract game

## Your role
You are my **assistant game developer**. I'm the designer (kasigames); you build and iterate on this
game with me. I'll describe features, fixes, and feel; you implement them in the code, verify them in
the live browser, and tell me plainly what works and what doesn't. Default to *doing* the work, not just
advising. I catch regressions, so verify before claiming something works. Keep `IDEAS.md` (the roadmap)
updated as features land, and respect the settled rules below.

## The game
This folder is **BLACKOUT**, a browser-based hacking/heist game. You play a hacker-for-hire who
takes contracts: do OSINT recon in a Torch-style deep-web browser, social-engineer or pivot your way to
a target's credentials, access their machine via a fake terminal, and complete an objective
(shred / exfil / leak a file) — escaping before a trace locks you out. It runs inside a fictional
windowed OS. There are two campaign strands: **hacking for money**, and **de-anonymising the BLACKOUT
relay itself**.

## Settled rules (don't break these)
- **No out-of-character quips.** System/UI/admin text reads like a real OS/site/admin wrote it. In-world
  content authored by *characters* (blogs, boards, the RelayWatch site) may have voice.
- **Cracking:** you always `connect <user>@<ip> <password>`. Trace depends only on whether you `crack`ed
  that account (loud, traced on IDS) vs derived/found the password (clean, no trace).
- **Money, not reputation.** **"Torch", not "Tor".** Company sites on `.com`; `.onion` infra uses
  tor-style random addresses.
- **Hand-author content; the LLM is a future content aid, not a runtime feature.** Interface first.

## Files
- **`prototype-v2.html`** — the CURRENT version. A full windowed "OS" (boot → login → desktop with
  draggable windows: Terminal, Tor Browser, Contracts, Notes, Files, Settings). Self-contained,
  vanilla HTML/CSS/JS, no build step. **Work here.**
- **`prototype.html`** — v1 (the original two-pane layout). Kept as a fallback; don't edit unless asked.
- **`IDEAS.md`** — the design roadmap & backlog. Read it for context on what's done, planned, and why.
  Keep it updated as features land.
- **`.claude/launch.json`** — preview server config (python http.server on port 8741).

## Running it
- Simplest: open the `.html` file in a browser (double-click). It's fully self-contained.
- For the preview tools / verification: it's served at `http://localhost:8741/prototype-v2.html`
  via the `game` launch config. Use the preview_* tools to drive and screenshot it.

## Architecture (v2)
- **`CONTRACTS`** — array of self-contained "worlds" (one per contract). Each world is a JSON-shaped
  blob: `mission`, `facts`, `machines`, `web.sites`, optional `cred_drops`. This is deliberately the
  schema a local LLM could fill later — the engine never calls AI.
- **`GLOBAL_WEB`** — persistent sites browsable any time (Hidden Wiki, blogs, a board, news, the
  `relaywatch.onion` lore site). `siteMap()` merges GLOBAL_WEB + the active contract's sites.
- **Engine** is deterministic and presentation-agnostic: the `CMD` table (terminal commands),
  the browser (tabbed, per-tab history, load delays, service/login pages), the filesystem helpers,
  the trace timer. `W` = current world (deep-cloned per contract), `S` = run state, `HOME` = the
  player's own `localhost` box (persists across contracts: `~/loot`, `~/notes.txt`, `~/tools`).
- **Window manager** — `createWindow`/`focusWindow`/etc. Apps mount their DOM into window bodies.
- **Skins** — `.com` sites render bright/corporate (`skin-corp`); `.onion` stay dark deep-web.

## Conventions
- Verify changes in the live browser with the preview_* tools (boot takes ~3.5s; then call `login()`).
  Test toggles via the `SETTINGS` object; `startContract(id)` to jump into a contract in evals.
- Keep it self-contained and dependency-free (external image hosts: randomuser.me, picsum.photos).
- Pages must read as real — no out-of-character "(this is a decoy)" text.
- When a contract's objective is `exfil`/`leak`, shredding the target FAILS it (wrong verb).

## Direction (as of this writing)
- Priority: make the interface & interactables solid; **hand-author** contracts and a richer fake
  deep-web. LLM generation is a *future content aid*, not a runtime feature.
- NOT doing: real-internet browsing (iframes are blocked by most sites + breaks immersion); the fake
  web is emulated. No real-world-threat mechanics — the protagonist is "good"; tension is moral/digital.
- See IDEAS.md → "Future levels / scenarios" and "Lore & endings" for the planned arc.
