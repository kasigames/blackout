# BLACKOUT

**A hacking-contract game.** v0.6.0

You're a hacker-for-hire who takes contracts: do OSINT recon in a deep-web browser,
social-engineer or pivot your way to a target's credentials, access their machine via a
terminal, and complete an objective — shred, steal, or leak a file — then escape before a
trace locks you out. It all runs inside a fictional windowed operating system.

## Play it

It's a single self-contained HTML file — no install, no build step.

- **Just open `prototype-v2.html` in a browser.** That's it.
- Or serve it locally and open `http://localhost:8741/prototype-v2.html`.

## What's in here

| File | |
|------|--|
| `prototype-v2.html` | **The game.** Windowed OS: boot → login → desktop (Terminal, Torch Browser, Contracts, Notes, Files, Manual, Settings). |
| `prototype.html` | v1 — the original two-pane prototype, kept as a fallback. |
| `IDEAS.md` | Design roadmap & backlog. |
| `CLAUDE.md` | Notes for working on the codebase. |

## How it plays

1. **Recon** — open the Torch Browser, search a target, read their sites (bright corporate `.com`
   pages and dark deep-web sites) for buried clues.
2. **Work out a password** — derive it from what you find.
3. **Access** — in the Terminal, `crack` it (loud — gets you traced on monitored hosts) or, if you
   know it exactly, `connect` quietly (no trace).
4. **The job** — `shred`, `exfil`, or `leak` the target file.
5. **Get out** before the trace catches you.

Every command is documented in the in-game **Manual** app.

## Tech

Vanilla HTML/CSS/JS. No dependencies, no build, no backend. The version number lives in one
place: the `VERSION` constant in `prototype-v2.html`.

## Author

Made by **kasigames**.

## Status

Early but playable prototype — actively in development. See `IDEAS.md` for what's planned.
