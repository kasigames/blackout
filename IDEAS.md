# BLACKOUT — Design Roadmap & Idea Backlog

A running list of ideas for the hacking-contract game. Nothing here is committed —
it's the parking lot we refer back to when deciding what to build next.

The core architectural principle, keep it in mind for everything below:
> **The engine is fixed and deterministic. The world is one JSON blob.**
> Difficulty, goals, and content are *data + flags* the engine reads — eventually
> generated at boot by a local LLM (Ollama, `format:json`) from an archetype +
> objective + a face pulled from the tagged pool. Almost every idea here is "add a
> field/flag to the world schema," not "rewrite the engine."

---

## ✅ Done so far (the proven vertical slice)
- Dual-pane UI: CRT terminal + Tor-style browser with history (back/forward).
- One `shred` contract, fully playable end-to-end.
- Recon → social-engineer password → `wordlist --gen` → `crack` → `connect` → `shred` → `disconnect`.
- Trace countdown (starts on connect; failing it = caught).
- Facts split into `cred` (feeds wordlist) vs `lead` (intel only, e.g. subnet).
- Search engine with keyword matching across sites.
- Command history (↑/↓), terminal text selection, dossier face from thispersondoesnotexist.
- World stored in the exact schema the LLM will later fill; `loadWorld()` is the stubbed wire-in point.

---

## 🎯 Near-term build order (foundation first)
1. ✅ **Contract board** — selection screen with payout + risk + tags. DONE.
2. ✅ **Difficulty as composable flags** — per-machine `ids`/`trace_seconds`/`hidden_fs`/`exposed`/`reveals_subnet`, world `cred_drops`. DONE.
3. ✅ **Settings menu** — trace on/off, auto-extract intel on/off (hard mode: manual `wordlist add` + `scan SUBNET`), typewriter, scanlines. Persists to localStorage. DONE.
4. ✅ **Your-device home base** — `localhost` FS (`~/loot/`, `~/notes.txt`, `~/wordlists/`, `~/tools/`); browse it with ls/cd/cat when not connected; `note add` command; prompt shows `root@localhost` vs `root@<host>`. Unlocked `exfil` (→ ~/loot) and `upload` (leak drop). DONE.
5. ✅ **One multi-stage contract** ("Glass House") — lateral movement + hidden FS + traced core. DONE.
6. **Richer / interlinked websites** — see World texture below.
7. **AI generation phase** — swap the stubbed contract.world for an Ollama call, generating *into* this richer schema.

---

## 🧩 Difficulty & game-feel
- **Three separate clocks** (don't conflate):
  - **Window** = narrative soft deadline (the 09:00 backup actually deletes the file if you're slow; or a server only reachable in business hours). Pressure, no stopwatch.
  - **Trace** = live countdown, *only while connected, only on systems with `ids:true`*. Low-tier targets (a personal laptop) have no trace; elite systems trace hard.
  - **Noise / Heat** = footprint meter that rises with loud actions (brute force, failed logins, `shred` is louder than `exfil`). Crossing a threshold is what *triggers* the trace — making stealth a real playstyle.
- **Hidden filesystem** (`hidden_fs: true`) — hard mode hands you no path. Explore with `cd` / `ls` / `grep -r`; read clues to know which folder.
- **Trace only on elite systems** — per-machine flag, not global.
- **Heat carries between contracts** — sloppy work (skipping `wipe-logs`) makes the *next* job harder. Ties missions together.
- **Red-herring intel** — a fact that looks like password material but isn't, so `wordlist --gen` isn't a guaranteed one-shot.

## 🎮 Goals / objective verbs (one engine, many missions)
- ✅ `shred` (destroy) — Paper Trail, Glass House, Cold Inbox.
- ✅ `exfil` (steal a copy → `~/loot/`) — Sideline.
- ✅ `leak` (exfil then `upload <drop>`) — Daylight.
- `plant` (frame someone — drop a file). TODO.
- `expose` (leak to a fake press site). TODO.
- `locate` — OSINT-only, find a physical address / real identity, no terminal needed. TODO.
- `impersonate`. TODO.

### Contracts so far (5)
Paper Trail (shred, low) · Glass House (shred, lateral+hidden-fs, high) · Cold Inbox (shred, email-pivot, med) · Sideline (exfil, med) · Daylight (leak, high).

## 🌐 Multi-stage / lateral movement (the "puzzle → game" jump)
- Breach a low-value entry point (weak creds, no IDS) → internal `scan` reveals a subnet you couldn't see from outside → `pivot`/`ssh` between hosts, reading configs/creds → reach the target server → execute.
- For exfil: `download` to your device, then `upload <breach-site>`.
- Each hop is a mini version of the existing loop. Skill becomes *reading a network*.

## 🔓 Solve-path variety (so it's not always social engineering)
- **Social-engineer** the password from personal facts (current).
- **Breach dump** — password sitting in a leaked DB you find by searching their email on a breach forum.
- **Phishing** — send a crafted email from a fake IT address, wait, check for a reply with creds.
- **Auth variety** — SSH key vs web login vs 2FA. A 2FA target means also compromising their email/phone for the code (a second recon thread).

## 💰 Meta-game / progression  *(user-requested)*
- **Currency system** — earn from contracts; spend on WiFi/connection upgrades, tools.
- **Tool shop** — purchases that change the rules:
  - faster cracking, smarter `wordlist` mutations.
  - `proxychains` (longer trace window), `nmap` (richer scans), a `0day` (one free crack).
- **Reputation score** — harder jobs pay more *per job*; higher reputation raises pay *overall* and unlocks higher-tier contracts. (Two curves: per-contract payout vs reputation-multiplier.)
- **Factions / who hires you** — leak collective wants `expose`; a corporation wants quiet `exfil`; a fixer wants `shred`. Your choices tilt which contracts appear, giving the verbs meaning.

## ⚙️ Settings / config menu  *(user-requested)*
- Toggle potentially annoying features: **trace on/off**.
- Difficulty toggles that make it *harder*: disable **auto-fact discovery**, disable **auto-clues** (so you read pages yourself and decide what matters).
- Likely also: auto-fact off ⇒ you manually `note`/extract facts into `~/notes.txt`.

## 🏠 Your own device (home base)
- `localhost` FS: `~/wordlists/`, `~/loot/` (exfil lands here pre-upload), `~/notes.txt` (jot facts), `~/tools/` (purchased gear).
- Enables exfil→upload flow, tool progression, cross-session persistence, manual note-taking when auto-facts are off.

## 🌍 World texture (make recon less straightforward)
- **More websites** in the search index — old forums, defunct blogs, news mentions; most lead nowhere, a few hide the real clue. Bury the signal. (AI will fill this out heavily once generation is on.)
- **Properly browsable, interlinked sites** — pages with real sub-pages and **embedded links between them** (a profile → their employer → a news article → a forum thread), so you *navigate* a site, not just read one card. Multi-page sites instead of single blurbs.
- ✅ **Browser tabs** — multiple tabs, per-tab history, smart labels. DONE.
- ✅ **Browser resets per contract**, gender-matched dossier faces. DONE.
- **NO meta / out-of-character text** on pages — never label a decoy as "(dead end)" or "(no creds here)". Pages must read as real. (Removed the early placeholder labels — keep it this way.)
- **Decoy people** — search a name, get several matches; only one fits the dossier face/role. Adds a verification step.
- **Cover your tracks** — optional `wipe-logs` objective (see Heat, above).

## 🪜 Recon chains — difficulty by ABSENCE, not just noise  *(user idea)*
- Make some targets hard because the info simply **doesn't exist publicly**, not because it's buried in noise.
- Forces escalation: no social profile → break into their **work email** to learn enough → use that to get into a **private email / personal account** → which holds the real key. Each step gates the next.
- This is the same machine-graph / cred-drop mechanic applied to *accounts and inboxes*, not just servers. An email account is just another "host" with `files` (messages) and creds found one step back.
- Pairs with auth variety (2FA → need the inbox for the code) and the home base (somewhere to stash what you pull from each inbox).

### Realism: gated content + password-reset pivot  ✅ DONE (contract "Cold Inbox")
- Built: interactive **service pages** in the browser (`type:"service"`, kind `webmail`|`portal`) — real login forms, authed inbox/portal app views, sign out.
- Built: **password-reset pivot** — portal "Forgot password?" generates a code, delivers it as a message into the recovery `reset_inbox`, player reads it, submits code + new password → logged in.
- Built: portal `on_open` reveal hands workstation creds + subnet to the terminal side (`S.foundCreds`/`S.knownSubnets`), so the browser chain feeds the existing scan→connect→shred endgame.
- Built: **world is deep-cloned per contract** (`structuredClone`) so shred/login/reset mutations don't leak into replays. (Also fixed the older shred-persistence bug.)
- Still TODO here: security-question path, 2FA variant (need inbox for the code), and reading reset codes into the **home base** notes instead of just on-screen.

## 🖼 Faces / content generation
- ✅ **Gender-matched faces (interim)** — missions carry `gender` + `faceId`; dossier pulls from randomuser.me `portraits/{men|women}/{id}.jpg` via `faceUrl()`. Fixes the male-pic-for-women bug. Trade-off: these are real model photos, not AI faces, and the pool is only 100 each (will repeat at scale).
- **Shippable version** — pre-pool ~40–60 thispersondoesnotexist faces, tagged `male/female × younger/older` (+ optional vibe). Cache locally → offline, instant, matchable, fictional. `faceUrl()` is the single swap point. (Add age tags then — randomuser isn't age-sortable.)
- Generation flow: pick archetype + objective + a tagged face → feed the face's tags into the LLM prompt as constraints → it writes a consistent persona (bio, emails, clues, the cred/lead-tagged facts, the solution_path).
- Validate the JSON against the schema; retry on malformed output (small local models need this gate).
- Generator should plant a **soft breadcrumb** in the brief toward the key recon page (e.g. "she's careless on social media"), so recon has a trail.

## 🖥 Prototype v2 — windowed OS  (in progress, `prototype-v2.html`)
v1 (`prototype.html`) kept intact as a fallback. v2 reuses the whole engine; the shell is new.
- ✅ Boot sequence → login → desktop (wallpaper + grid).
- ✅ Window manager: draggable, focus/z-order, minimize/close, taskbar buttons.
- ✅ Apps: Terminal, Tor Browser, Contracts, Notes, Files (~/loot), Settings — all as windows.
- ✅ Relay **load delays** (spinner "routing through 3 relays") + a `loaddelay` setting toggle.
- ✅ Richer, denser sites on Paper Trail (long bios, buried clues, decoy news/alumni pages); search shows snippets. *(Content pass: extend the same density to the other 4 contracts.)*
- All 5 contracts + email-pivot + exfil/leak verified working in the windowed shell.
- ✅ Window manager polish: minimise (stays in taskbar), maximise (green dot / double-click title), **resize** (corner handle), ✕/–/+ controls on hover, fixed sidebar icons (the `#windows` layer was eating clicks).
- ✅ Load delays now on **searches too** + a Tor-style **load bar** in the address bar + longer (bad-routing feel).
- ✅ **Wallpaper** (procedural skyline) + **colour themes** (Phosphor / Amber / Ice) in Settings.
- ✅ **Unix-ish shell**: whoami · uname · date · echo · mkdir · touch · rm — poke around any host. Easter egg: `rm -rf /` on localhost → kernel-panic self-destruct.
- ✅ "Are you sure" confirm when abandoning a contract.
- **Next for v2:** richer content across all 5 contracts; the new apps the OS unlocks — Mail client, **Shop** (economy), Network map for multi-stage.

### v2 polish round 4 (done)
- ✅ Fixed: window **close/minimise** (event bubbled to focus handler — added stopPropagation). Only maximise had worked.
- ✅ Fixed: browser **load-bar streak** never clearing (single owning timer now).
- ✅ Fixed: **portal/login forms on the bright corp skin** (were black inputs) → light inputs + blue button.
- ✅ Fixed: **difficulty colours** now theme-independent (LOW green / MED amber / HIGH red, hardcoded) — switching theme no longer recolours them.
- ✅ **CRT / 80s mode** setting — full-screen scanlines + curvature vignette + flicker + phosphor glow. (True barrel-distortion bulge skipped: SVG displacement blurs text; faked curvature reads fine.)
- ✅ More **the pit** threads (board humour). `CLAUDE.md` added so the folder self-documents.

### v2 polish round 5 (done)
- ✅ Load-bar **actually** fixed — `finishLoadbar()` runs inside `render()`, so any page display (incl. back/forward orphan case) clears the bar. Verified.
- ✅ Removed the dud terminal-only "CRT scanlines" toggle (redundant beside full CRT mode); terminal keeps subtle scanlines by default.
- ✅ **Procedural web** (`proceduralSite()`) — any unknown domain-ish URL resolves to deterministic filler (parked / under-construction / lonely blog / members-only login / 503), ~20% genuinely down. The web now feels boundless when you type or follow links. Authored sites always take precedence.

### "Make the internet appear larger" — approaches  *(user priority)*
(Correction noted: WttG's later browser CAN load real sites — I was wrong it was fully emulated.)
- ✅ **Procedural filler** (done) — biggest cheap win; explore anywhere, always get a page.
- **Search padding** — TORCH could return a few procedural results alongside authored ones (keep recon clean — only pad generic queries, not target-name searches).
- **Templated site families** — many sites sharing a template with seeded variation (forums, blogs, shops) so breadth is cheap.
- **A real-web bridge (later, optional)** — a tiny local proxy server could fetch/rewrite real pages so the in-game browser shows them (this is roughly how WttG does live sites). Needs a server + careful sandboxing; only if we go "proper app". Big immersion payoff, real complexity. NOT now.

### v2 polish round 6 (done)
- ✅ **Hacking depth — loud `crack` vs silent `login`.** `crack` = noisy brute-force (animated counter into the hundreds + rolling candidate strings); on an IDS host it gets TRACED. `login <user>@<ip> <password>` = if you've *derived* the exact password, a clean login with NO trace, even on IDS hosts. Provenance tracked via `S.loud`. Rewards reading over brute-forcing. Verified.
- ✅ **Manual app** (desktop `?` icon) — sidebar docs: Welcome, Contracts, TORCH/Browser, Terminal, Recon→Access (loud vs silent), Objectives, Your Device, Effects & Tips.
- ✅ **Relay-unlock loading** on the login button (verifying key → decrypting volume → …).
- ✅ **Boot interrupt fixed** — now listens for key OR mouse, longer window, and a visible "[ press any key to interrupt boot ]" hint.
- ✅ **Procedural web v2** — more templates: parked, under-construction, lonely blog, garish **90s/00s personal homepage** (marquee, visitor counter), **conspiracy zine**, members-only, **dead archived forum**, 503. ~14% genuinely down.
- ✅ **BLACKOUT relay admin easter egg** *(user idea)* — `185.220.101.99` (numeric mess) and `blackoutrelay.onion/admin`, login `admin`/`password` (the joke), revealing an operator console: "911 active sessions… root@localhost → everything you've ever done … logged… Especially you." Breadcrumbed from `relaywatch.onion`. Direct hook into the Lore & endings.
- Decided: NOT loading the real web (WttG's is limited too) — "Unable to connect" / procedural filler instead.

### v2 polish round 7 (done)
- ✅ **Version** in one shared place (`VERSION` const → boot, login screen). **README.md** added (author: kasigames). Git repo initialised; remote = github.com/kasigames/blackout (push pending user auth).
- ✅ **Cracking (settled):** you ALWAYS `connect <user>@<ip> <password>`. Trace depends only on whether you `crack`ed that account. `crack` = loud dictionary attack (X/5000 from common.txt), reveals the password but flags the account → connecting is traced on IDS hosts. Deriving the password yourself, or finding it in a file/portal = clean login, no trace even on IDS. No more password-less `connect`.
- ✅ **Quips removed** — relay admin console is now a dry operator panel (status / session log / config); the session log includes the player as one anonymous `sess#` among 911, NOT singled out. `rm -rf /` shows a realistic mount/connection error, no "you did this".
- ✅ **Relay handle** at unlock — must pick a unique handle; ~45% "already exists" rejection, dropping to ~12% if it contains digits. Stored as `playerHandle`.
- ✅ **"Tor" → "Torch"** throughout (Torch Browser, `torch://search`). Removed the misleading "press any key to interrupt boot" hint.
- TODO content: most contract company sites should be `.com`; any company infra on `.onion` should use tor-style random/numeric addresses (do in the web pass).

### Two strands (the spine — user framing)
The game is being built around two parallel campaigns:
1. **Hacking for money** — the contract loop (money only; no reputation system).
2. **De-anonymising the relay** — uncovering/【ending】 BLACKOUT itself. Seeds in place: `relaywatch.onion`, the relay admin console (`185.220.101.99` / `blackoutrelay.onion/admin`), the interruptible boot.

### Strand 2 — privilege-escalation chain  *(user idea, design)*
A multi-step path toward the relay:
- interrupt boot → **escalate** (root on your own box / a relay node) → **scan** → log into an important server →
  explore for an **encrypted file** → **decrypt** it (a `decrypt` tool, maybe bought in a Shop / its own contract) →
  the decrypted data yields the **relay admin portal + an IP to a core machine** →
  final choice: **shred the relay system**, **report it** (police/press), etc. → multiple endings.
- Needs: a boot-interrupt that actually branches, an `escalate` mechanic, encrypted-file + `decrypt` tooling, a Shop.

### Desktop & downloads  *(user idea)*
- Give the file system a **desktop / icon view**, and let the player **download** `.txt`/files from web pages to `~/loot` (or a Downloads folder) and open them in a viewer. Ties the browser and the file system together.

### Web-building plan (the next big push — user priority)
Goal: a genuinely full, believable fake deep-web + clearnet, hand-authored. NOT real-internet browsing
(iframes are blocked by most real sites + it breaks immersion; emulate instead, like Welcome to the Game).
- Decide a **site catalogue**: per archetype — corporate sites (bright), social profiles, breach forums,
  blogs, boards/chatrooms, news, marketplaces, webmail/portals, govt/records, personal homepages.
- Make boards/chatrooms feel alive (more threads; maybe the player can post → flavour responses).
- More interlinks; more dead/seized links for texture.
- Contracts stay **hand-authored** for now. LLM = a *content aid later* (a prompt that emits the
  `world` JSON / site HTML to paste in), not a runtime dependency. Build the interface & interactables
  solid first.

### v2 polish round 3 (done)
- ✅ **Free browsing** — the Tor Browser now works with NO contract. A persistent **global web** (`GLOBAL_WEB`) is always browsable; contracts layer their target sites on top via `siteMap()`. Open the Browser from the desktop and just explore.
- ✅ **Global web content** — The Hidden Wiki (link directory, live + dead links), NightOwl (a personal blog), the pit (anonymous board), DeepFeed (leak news), **RelayWatch** (lore site questioning who runs the BLACKOUT relay). TORCH start page has quick links.
- ✅ **Bright "clearnet" skin** — any `.com` site now renders as a real bright corporate website (white bg, blue links, headshot cards); `.onion` stays dark deep-web. Big believability jump. Per-site `skin` field also supported.
- ✅ **Wrong-verb FAIL** — shredding the target on an `exfil`/`leak` job now FAILS the contract ("you burned the asset") instead of counting as a win. New `failContract()`.

### Still-rich-web TODO
- Bring the other contracts' sites up to Open House density + give them the bright skin where clearnet.
- More global-web texture: more blogs, board threads, a working(ish) market, chat rooms.

### Contract idea: "dark wiki → burn a competitor"  *(user idea)*
- Shady client hires you via a dark-wiki listing to destroy a rival's whole system — objective is a full `rm -rf` of a remote server (not just one file). Introduces a **"wipe entire host" objective** (vs single-file shred) and uses the Hidden Wiki as the contract hub.

### v2 polish round 2 (done)
- ✅ Paginated help (`help` / `help 2`); every command documented.
- ✅ Self-destruct re-themed: "RELAY FAULT — unable to connect to relay / machine architecture missing" (not kernel-panic).
- ✅ Proper BLACKOUT/Linux boot: ASCII logo + systemd-style `[ OK ]` lines.
- ✅ Real **photo wallpaper** (picsum under a heavy dark-green gradient so it's always moody).
- ✅ **"Open House"** contract — deep multi-page corporate website (home/about/team/careers/news/contact) with **images** (hero photos via picsum, team **headshots** via randomuser) → employee **portal** login as the assistant → reveals the partner's workstation creds → exfil. 6 contracts total now.
- **Still the biggest lever:** more rich, image-laden, interlinked sites like Open House across the other contracts.

## 🎬 Future levels / scenarios  *(user asked — brainstorm bank)*
Fresh mission *types* (each reuses the engine + a flag or two; verbs in parens):
- **The Mole** — read several employees' inboxes/chats to deduce *which one* is leaking (deduction, no single "password").
- **The Vanishing** — `locate`: OSINT-only, no terminal; piece together a person's real location/identity from scattered sites.
- **Inside Job** — `plant`: drop fabricated evidence on a target to frame them (morally darker — keep optional).
- **Counter-trace** — someone is surveilling the client; trace *them* back through their own cameras/logs. Reverses the usual flow.
- **Ransom** — a business is locked by ransomware; break the encryption or find the key file before a deadline.
- **Whistleblower Protection** — race a rival hacker to wipe a source's trail before they're exposed (you're the *good* guy; a timer + an AI opponent).
- **Smart Home** — breach a target's IoT (cameras, smart lock, router) — different "device" types than servers.
- **The Auction** — several hackers after the same file; hard time pressure, heat matters.
- **Capstone corp** — a heavily segmented network: multiple pivots + 2FA + log-wiping + heat management in one job.

### The campaign / lore spine
- A handler character who hands you contracts and a through-line; stakes escalate; a mid-game betrayal.
- The **interruptible boot** pays off here: someone (a rival? the feds? a ghost of a previous operator) is already inside your relay — recurring presence that escalates.
- Branching morality: shred vs leak vs sell shifts faction standing and which contracts appear next.
- A **nemesis hacker** who counter-traces you across jobs (heat carries).

### Difficulty knobs these unlock
no reused passwords (force phishing/breach) · 2FA everywhere (need the inbox) · honeypot machines that spike heat · time-of-day access windows · security-question attacks.

### Long-term lore hook  *(user idea, TBD)*
- Pressing a key during the boot sequence now triggers a red "unscheduled interrupt… you are not the first to log in here" teaser (seed only).
- Goal: an interruptible boot that leads into a story layer — navigable lore, a presence already on the system. Design the actual thread later.

### Lore & endings  *(user direction)*
- **Premise:** the protagonist is genuinely *good*. They route through nodes to reach the **BLACKOUT relay**, which in turn anonymises its own web traffic the same way — relay-of-relays.
- **The catch:** BLACKOUT itself is problematic — whoever runs it sees everything routed through it (seeded in-game via the `relaywatch.onion` site). The player gradually learns this.
- **Two endings (final mission):**
  1. Take the relay/sites down for good (burn the network you've been using).
  2. Turn on the person who's been contracting you (hack the handler).
- These give the campaign a moral spine and a real choice. The interruptible boot + RelayWatch are the breadcrumbs toward it.
- Note: NO external "assassin"/real-world-threat mechanics (deliberately unlike Welcome to the Game) — the tension is moral and digital, not physical.

## 🗂 Project structure — one file vs many (decided)
- **Stay single-file (`prototype.html`) for now.** It opens by double-click with zero setup, which is worth a lot while iterating fast on the engine.
- **Why not split yet:** ES modules (`import`/`export`) don't work from `file://` — they need a server (CORS). Splitting now would mean you *must* run the local server every time. Not worth it yet.
- **When to split:** at the **AI generation phase**, or once there are many contracts. At that point pull *data* out first — a `contracts/` folder, the engine in `engine.js`, styles in `style.css` — loaded as classic `<script src>` (globals), which still works from `file://`. Or commit to always running the dev server (already configured in `.claude/launch.json`, port 8741).
- Translation for non-devs: one file = simplest to run; many files = easier to manage when it gets big, but needs the little server running. We'll switch when the size pain outweighs the convenience.

---

*Last updated: 2026-06-12*
