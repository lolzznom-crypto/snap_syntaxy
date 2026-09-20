# Snap Syntax

A polished, dependency-free UI competition workspace with a split HTML/CSS editor and live preview.

## Run locally

```bash
python3 -m http.server 4173 --directory .
```

Open `http://localhost:4173`.

## Included

- Resizable split-screen code editor and sandboxed live preview
- HTML/CSS file create, switch, and delete flows
- Auto-save to browser local storage
- Desktop/mobile preview modes
- Fullscreen test mode with timer
- Automatic lock on tab switches, Alt+Tab/app switching, window focus loss, page hiding/minimizing, navigation away, or fullscreen exit
- Preset administrator passcode (`202626`) with salted SHA-256 verification in memory
- Failed-attempt logging and cooldown
- Submission export containing files, team details, timestamp, and violation log
- Responsive layout and accessible focus/target sizing

## Production deployment for 100+ teams

This build is a fully functional front-end prototype and can be hosted on any static host. For a real proctored event, do **not** rely on client-side enforcement alone—browser code can be modified by a determined participant.

Recommended architecture:

1. Serve this client over HTTPS from a CDN.
2. Add an API (Node/Fastify, Go, or serverless functions) with PostgreSQL.
3. Store only Argon2id/bcrypt passcode hashes server-side; issue short-lived, HttpOnly, SameSite session cookies.
4. Persist submissions, file revisions, test status, and append-only violation events server-side.
5. Use WebSockets or Server-Sent Events for autosave acknowledgements, admin unlocks, and live monitoring.
6. Apply per-team authorization, rate limits, CSRF protection, schema validation, audit logs, and database backups.
7. Run isolated preview compilation. This static version uses a sandboxed iframe; a production system should also set a strict Content Security Policy and never execute submissions on the API server.
8. Deploy at least two stateless API instances behind a load balancer. Redis is optional for shared WebSocket presence/rate limits.
9. Monitor latency, disconnects, save failures, and lock events. Load-test at 300 concurrent sockets to leave headroom for 100 teams.

## Important browser limitation

No normal website can guarantee that a participant did not use another device, browser profile, OS overlay, or developer tools. The app combines `visibilitychange`, window `blur`, `pagehide`, periodic `document.hasFocus()` checks, and Fullscreen API exits. This covers ordinary tab switching, Alt+Tab/app switching, minimizing, navigation away, focus loss, and fullscreen exits. High-stakes events should combine this with event supervision and server-side auditing.

## UI/UX direction

The interface follows a restrained professional design system: high-contrast editor chrome, clear visual hierarchy, semantic status colors, 44px interaction targets, responsive stacking, reduced-motion support, and minimal decoration. The requested UI/UX Pro Max repository was referenced for its professional interface guidance; network restrictions prevented downloading its full source during generation, so the built-in artifact design standards were also applied.

## Visual refresh

Updated with a polished midnight-studio interface, improved SVG icons, richer hierarchy, refined active states, glass-like surfaces, and enhanced responsive styling.

## Event identity

The interface now uses the supplied Snap Syntax artwork and its burgundy, coral-pink, violet, cream, and near-black palette. Branding assets are included for reuse; the standalone HTML also embeds optimized copies.

## Reload persistence

All files and active test metadata are stored locally in the browser without sign-in. Reloading during an active test restores the saved work and immediately locks the workspace; the preset administrator passcode is required to continue. Local storage is specific to the same browser and site origin.

## JavaScript and exam lockdown

The editor supports `.js` files and injects them into the sandboxed preview. HTML opening tags auto-close when `>` is typed. During an active test, common reload, navigation, context-menu, and developer-tools shortcuts are blocked. Focus, tab visibility, and fullscreen exits remain monitored. Window-size, split-screen, browser side-panel, and developer-tools size heuristics were intentionally removed to prevent false detections. Reloading from browser chrome cannot be technically prevented by a webpage, so any successful reload restores local work and immediately locks the session. Browser-only protections are deterrents, not a substitute for managed devices or server-side proctoring.

## Ninja Syntax starter project

The workspace now includes a folder-based `ninja-syntax/` project with `css/`, `js/`, and `assets/` subfolders. It contains HTML/CSS/JavaScript boilerplate and all supplied images. The file explorer renders nested folders, image files open in an asset preview, and relative asset paths are resolved to embedded data URLs in the live preview and exported JSON.

- `eat/` — EatFit food website starter with 11 supplied assets.

- Timer expiry now hard-locks editing, supports passcode-protected restart, and includes a navbar factory reset.

- Starter projects now use only `index.html`, `styles.css`, `app.js`, and `assets/` at each project root.
- Explorer folders are collapsible and supplied assets use short, readable names.

- Multiple editor tabs now behave like VS Code: the × closes a tab only, while right-clicking a file opens the non-modal Delete file action.

- Finished tests now offer a read-only review of saved code and rendered output without modifying the submission.

- Finished submissions can be opened in read-only review mode, with compact local persistence protecting code without re-saving embedded assets.
