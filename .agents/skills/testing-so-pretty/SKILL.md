---
name: testing-so-pretty
description: Test the So Pretty static bilingual breeder website end-to-end. Use when verifying the So Pretty landing page, cookie consent, IT/EN switching, awards lightbox, or mobile navigation.
---

# Testing So Pretty

So Pretty is a static HTML/CSS/JS site (no build system, no package manager, no pre-commit hooks). Entry points: `index.html`, `awards.html`, plus legal pages. Interaction logic and IT/EN dictionaries live in `script.js`; layout/responsive rules in `styles.css`.

## Run locally
```bash
cd /home/ubuntu/repos/so-pretty && python3 -m http.server 8081
```
Open `http://127.0.0.1:8081/`. Validate before testing:
```bash
node --check script.js   # JS syntax
```

## Primary end-to-end flow (record this)
1. Landing (IT default): hero shows logo, `SO PRETTY`, subtitle `Allevamento di Barboni Toy e Nani`, announcement bar.
2. Cookie consent: click `Gestisci preferenze` → `Necessari` checked+disabled, `Analitici` toggle, `Salva preferenze`. Choose, reload → banner stays hidden (persisted in `localStorage` key `soPrettyCookieConsent`).
3. Language: click `EN` in nav → announcement/nav/hero switch to English. Language persists in `soPrettyLanguage`.
4. Awards lightbox: click a certificate card → full-screen image + caption; click image toggles zoom; `×` (top-right) closes.
5. Mobile 390px: shrink window, reload → only brand + hamburger; open shows all 7 links + IT/EN; clicking a link closes menu and scrolls to section.

## Tips / gotchas
- Reset state before recording: `localStorage.clear()` in the console, then reload, so the cookie banner reappears and language resets to Italian.
- To test mobile width without DevTools, resize the actual Chrome window: `wmctrl -r :ACTIVE: -e 0,40,0,420,760`. Restore with `wmctrl -r :ACTIVE: -b add,maximized_vert,maximized_horz`. The hamburger appears below ~820px (see `styles.css`).
- Contact links (WhatsApp/Instagram/email) and the map are intentionally placeholders until the owner provides real details — do NOT treat these as failures unless real values were supposed to be present.
- All content is bilingual via `data-i18n` / `data-i18n-html` attributes resolved by `applyLanguage()` in `script.js`; if a section stays Italian after switching to EN, the missing key is the likely cause.

## Devin Secrets Needed
None — fully static, no login, API keys, or external services required.
