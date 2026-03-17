# PwnFox+

A Firefox extension based on [PwnFox](https://github.com/yeswehack/PwnFox) with an added feature: **custom text labels on container groups**.

---

## Features

### From PwnFox
- **One-click Burp proxy** — toggle proxy to `127.0.0.1:8080` without needing FoxyProxy
- **Container profiles** — open new tabs in isolated Firefox containers (each with its own color)
- **X-PwnFox-Color header** — automatically adds a color header to requests so Burp can highlight and identify which container sent the request
- **Security header remover** — strips `Content-Security-Policy`, `X-XSS-Protection`, `X-Frame-Options`, and `X-Content-Type-Options` with one toggle
- **PostMessage logger** — captures all `postMessage` events on the active tab
- **Toolbox** — inject custom JavaScript on page load

### New in PwnFox+
- **Container labels** — assign a custom text label (e.g. `Admin`, `Victim`, `Attacker`) to any container color. The label is shown in the extension popup and renames the Firefox container identity so it appears next to the color circle in the URL bar.

---

## Installation

This extension is not signed, so it must be loaded as a temporary add-on or via an unlocked Firefox profile.

### Option A — Temporary (resets on Firefox restart)

1. Open Firefox and navigate to `about:debugging`
2. Click **This Firefox**
3. Click **Load Temporary Add-on...**
4. Select the `pwnfox-plus.zip` file (or any file inside the unzipped folder)

### Option B — Permanent (requires Firefox ESR / Nightly / Developer Edition)

1. Open `about:config` and set `xpinstall.signatures.required` to `false`
2. Navigate to `about:addons`
3. Click the gear icon → **Install Add-on From File...**
4. Select `pwnfox-plus.zip`

---

## Usage

### Proxy
Toggle **Enable proxy** in the popup to route all traffic through `127.0.0.1:8080` (Burp Suite default). Configure host and port in **Settings**.

### Container tabs
Click any color circle to open a new tab in that container. Each container is isolated — cookies, sessions, and local storage are separate from other containers and from the default browser profile.

### Adding a label
1. Hover over a color circle in the popup — a pencil icon appears at the bottom
2. Click the pencil icon
3. Type a label (e.g. `Admin`) and click **Save**
4. The Firefox container identity is renamed — the label now appears next to the color circle in the URL bar of any tab open in that container

To remove a label, open the edit modal and click **Remove label**.

### Burp integration
With **Add container header** enabled, every request from a container tab includes an `X-PwnFox-Color` header. In Burp, the companion Java extension reads this header to automatically highlight and color-code requests by container. The header is stripped before the request leaves Burp so it does not reach the target.

### PostMessage logger
Click **PostMessage** in the popup footer to open the logger page, then click **Inject into active tab** to start capturing `postMessage` events on the current page.

### Toolbox
Click **Toolbox** to open a JavaScript editor. Code saved here is injected into every page on load. Useful for adding detection hooks, helper functions, or custom behavior.

> ⚠ The toolbox runs in the `window` context. Do not inject secrets or credentials.

---

## File structure

```
pwnfox-plus.zip
├── manifest.json       Extension manifest (MV2)
├── popup.html          Extension popup UI
├── popup.js            Popup logic, container management, labels
├── background.js       Proxy settings, request header modification
├── settings.html       Settings page (proxy host/port, toggles)
├── toolbox.html        JS injection editor
├── postmessage.html    PostMessage event logger
├── newtab.html         Custom new tab page (used when a label is set)
└── icons/
    ├── icon16.png
    ├── icon32.png
    └── icon48.png
```

---

## Permissions

| Permission | Reason |
|---|---|
| `tabs` | Open new container tabs |
| `proxy` | Configure Burp proxy |
| `webRequest` / `webRequestBlocking` | Inject and strip headers |
| `contextualIdentities` | Create and rename container identities |
| `cookies` | Required for container isolation |
| `storage` | Save settings and labels locally |
| `<all_urls>` | Apply proxy and headers to all requests |

---

## Based on

[PwnFox](https://github.com/yeswehack/PwnFox) by [YesWeHack](https://www.yeswehack.com/) — released under MIT license.
