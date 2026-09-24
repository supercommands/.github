<div align="center">

<img src="https://raw.githubusercontent.com/cmdOS-App/cmdOS/main/src/shared-components/assets/cmdOS_logo.png" alt="SuperCommands logo" width="80" height="80" />

# SuperCommands

**A keyboard-first workspace for the browser.**

Find and create notes, links, tasks, and other saved content; run browser actions from a command bar.

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Node](https://img.shields.io/badge/node-%3E%3D22.12.0-brightgreen)](https://nodejs.org)
[![pnpm](https://img.shields.io/badge/pnpm-9.15.1-orange)](https://pnpm.io)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

[**Getting Started**](#getting-started) · [**Features**](#features) · [**Contributing**](#contributing) · [**Wiki**](https://github.com/supercommands/supercommands/wiki) · [**Community**](https://github.com/supercommands/supercommands/community) · [**Security**](SECURITY.md) · [**Code of Conduct**](CODE_OF_CONDUCT.md) · [**License**](#license)

</div>

## What is SuperCommands?

SuperCommands is a Chrome Manifest V3 extension with a new-tab workspace and a keyboard-driven website command interface. The standard Chrome build opens Command Search with `Alt + S`; the legacy website interface uses `Alt + Shift + S` in builds that include it. Browser-assigned shortcuts can be checked at `chrome://extensions/shortcuts`.

Core records are stored locally using IndexedDB and Chrome extension storage. Optional connected features, including authentication and cloud backup, can exchange data with external services when used.

## Features

- Search and manage notes, links, todos, snippets, collections, and other workspace records.
- Open the website command interface and use `c <category>` to create, `c -s <category>` to save, or `c -f <category>` to filter. The longer `-save` and `-filter` forms are also supported.
- Use slash filters such as `/note`, `/link`, `/todo`, `/snippet`, and `/collection` to narrow search.
- Run browser actions including screenshots, page extraction, and exports where available.
- Customize the new-tab dashboard with workspaces, views, widgets, and appearance settings.

The available actions depend on the active surface and build variant. See the [project map](code%20structure/structure/project-map.md) and the [website popup V2 guide](src/pages/AltS_search_websites_v2/README.md) for implementation details.

## Tech stack

| Layer | Technology |
| --- | --- |
| UI | React 19, TypeScript |
| Extension build | WXT, Vite 6; Turborepo also supports workspace and legacy scripts |
| Styling | Tailwind CSS and existing theme tokens |
| Package manager | pnpm workspaces |
| Local data | Dexie.js (IndexedDB) and Chrome extension storage |
| Extension | Manifest V3 |


## Repository Structure

<pre>
cmdOS/
├── background/                  # Service worker, manifest, extension bootstrap
├── packages/                    # Shared internal packages (monorepo)
│   ├── ui/                      # Design system components
│   ├── shared/                  # Utility helpers and schemas
│   ├── storage/                 # Chrome storage helpers
│   ├── env/                     # Environment variable schemas
│   └── module-manager/          # Core module configuration
├── src/
│   ├── allObjectFolder/         # Core object types — the heart of the extension
│   │   └── src/createObject/
│   │       ├── links/           # Link and Tab Session objects
│   │       ├── notes/           # Rich note objects
│   │       ├── snippets/        # Reusable text snippets
│   │       ├── commands/        # Command definitions and handlers
│   │       ├── session/         # Session tracking objects
│   │       ├── automationBeta/  # Automation workflow objects
│   │       ├── ChatAgent/       # AI agent integration
│   │       ├── aiPrompt/        # AI prompt objects
│   │       ├── todos/           # Task and to-do objects
│   │       └── tags/            # Tag management
│   ├── settings/                # Extension settings UI and logic
│   │   ├── authentication/      # Login and auth UI
│   │   ├── backup/              # Data backup and restore
│   │   ├── generalSettingsPageUi/  # General settings panel
│   │   ├── uiPersonalization/   # Theme and appearance settings
│   │   ├── uxLayoutCustomization/  # Layout customization options
│   │   └── allWorkspaceManager/ # Workspace and folder management
│   ├── storage/                 # Storage abstraction layer
│   ├── shared-components/       # Reusable UI components and utilities
│   ├── welcomeGuide/            # Onboarding and tutorial flows
│   └── pages/                   # Extension entry points
│       ├── AltS_search_newtab/  # Primary new tab workspace dashboard
│       ├── popup/               # Browser toolbar popup
│       ├── contentScript/       # Injected content scripts
│       └── content-ui/          # Injected UI overlays
├── docs/                        # Documentation and guides
└── .env                         # Environment config (pre-configured for local dev)
</pre>


---

## Getting Started

### Prerequisites

- **Node.js** `>= 22.12.0` — [Download](https://nodejs.org)
- **pnpm** `>= 9.15.1` — `npm install -g pnpm`
- Google Chrome or any Chromium-based browser

### Installation

**1. Clone the repository**

```bash
git clone https://github.com/cmdOS-App/cmdOS.git
cd cmdOS
```

**2. Install dependencies**

```bash
pnpm install
```

The `.env` file is already pre-configured with the OSS extension public key and Google OAuth client ID. No changes needed to run locally.

---

### Development

Start the WXT dev server with hot reload:

```bash
pnpm dev
```

Then load the extension in Chrome:

1. Open `chrome://extensions/`
2. Enable **Developer mode** (top right toggle)
3. Click **Load unpacked**
4. Select the `.output/chrome-mv3/` directory

WXT watches your files and automatically reloads the extension when you save changes. All your notes, snippets, and data are stored locally in **Dexie.js (IndexedDB)** — nothing leaves your device.

---

### Building

Build the production extension (with the shared OSS extension ID locked):

```bash
pnpm run wxt:build:chrome:oss
```

The built extension will be in `.output/chrome-mv3/`. Load it in Chrome:

1. Open `chrome://extensions/`
2. Enable **Developer mode**
3. Click **Load unpacked**
4. Select `.output/chrome-mv3/`

> The `wxt:build:chrome:oss` command automatically sets the correct build variant, loads the OSS environment file, and locks the shared extension ID so Google OAuth redirect URIs match for all contributors.

To create a `.zip` ready for the Chrome Web Store:

```bash
pnpm run wxt:zip:chrome:oss
```

---

### Type Checking

```bash
pnpm type-check
```

### Linting

```bash
pnpm lint
pnpm prettier
```

---

## Contributing

We welcome contributions! Please read [CONTRIBUTING.md](CONTRIBUTING.md) and our [Code of Conduct](CODE_OF_CONDUCT.md) before opening a pull request.

---

## License

Copyright © 2024–2026 SuperCommands · [Apache License 2.0](LICENSE)
