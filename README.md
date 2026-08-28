# Big3 Agent

Big3 Agent is a macOS desktop application for collecting Florida property data
and public-record documents in one workflow. Choose a supported county, search
for a property, and Big3 organizes the available property details, appraiser
documents, deeds, and tax bills into a local folder.

The desktop interface is built with JavaFX. A local TypeScript engine handles
county integrations, queued searches, document downloads, and browser
automation with Playwright.

> [!IMPORTANT]
> Big3 depends on independent government and third-party websites. Their
> availability, data, and anti-automation controls can change without notice.
> Treat generated results as research aids and verify important information
> against the authoritative source.

## Features

- Search seven Florida counties from one desktop application
- Use county-specific address, owner, parcel, account, and other search modes
- Run Full or Quick searches through a visible work queue
- Review live workflow logs and stop an active search
- Open collected files from the built-in output library
- Save structured property data alongside available PDFs, images, and links
- Receive signed backend-only updates with compatibility checks and rollback
- Protect the local desktop API with a per-process bearer token

## Supported counties

| County | Search options | Available collection workflows |
| --- | --- | --- |
| Brevard | Address, owner, account, or parcel | Property data, summary/TRIM documents, deeds, and tax bills |
| Broward | Unified property search | Property data, printable summaries, TRIM documents, deeds/screenshots, and tax bills |
| Orange | Address, owner, parcel, property name, subdivision, condo unit, deed book/page, or instrument | Property data, property card, summary, TRIM document, deed links, and tax bills |
| Osceola | Address, owner, parcel, business, or subdivision | Real or tangible-property data, property card, TRIM document, deeds, and tax bills |
| Indian River | qPublic property search | Property data, property card, TRIM document, sales records, and tax bills |
| Seminole | Unified property search | Property details and sales data; document collection is currently limited |
| Palm Beach | Owner, address, or PCN | Property data, summaries, TRIM document, deed links, and tax-collector results |

Document availability varies by property and by the source website's current
behavior. CAPTCHA, Cloudflare, maintenance windows, or redesigned county sites
may interrupt a search.

## Install the macOS app

The packaged release includes Java, Node.js, production dependencies, and a
Playwright browser. End users do not need to install developer tools.

1. Open the [latest release](../../releases/latest).
2. Download the macOS ZIP or DMG that matches your Mac.
3. Copy **Big3 Agent.app** to **Applications**.
4. Open Big3 Agent and sign in with a Waterview account, or create one in the
   app.

Current public builds are ad-hoc signed and are not notarized with an Apple
Developer ID. On first launch, macOS may require approval under **System
Settings → Privacy & Security**.

## Use Big3 Agent

1. Select a county.
2. Enter at least one supported property identifier.
3. Optionally add a file ID to name the result folder.
4. Choose **Run** for the broader Full Search workflow or **Quick run** for a
   reduced search.
5. Follow progress in the queue, then open the generated files from the output
   library.

By default, results are written to:

```text
~/Documents/Big3 Agent Output/
└── <file-id>/
    ├── <file-id> Property Data.json
    └── downloaded documents and links
```

Logs are stored at:

```text
~/Library/Application Support/Big3 Agent/logs/big3_agent.log
```

## Development setup

### Requirements

- macOS
- Node.js 20 or newer
- pnpm 11 (recommended) or npm
- JDK 21 or newer
- Apache Maven 3.9 or newer

Clone the repository, install dependencies and Playwright Chromium, then launch
the desktop app:

```bash
git clone https://github.com/calebcoomer/big3.git
cd big3
bash ./setup.sh
bash ./scripts/run-desktop.sh
```

`setup.sh` installs Node dependencies, downloads the local Playwright browser,
builds the TypeScript engine, and creates the default output and log folders.
`run-desktop.sh` rebuilds the engine and launches JavaFX through Maven.

### Run the engine without the desktop client

```bash
pnpm run build
pnpm start
```

The standalone engine listens on `127.0.0.1:5022` by default. Its health check
is available at `GET /api/health`.

### Validate changes

```bash
pnpm run typecheck
pnpm test
pnpm run check
mvn test
```

`pnpm run check` runs the TypeScript compiler, Node test suite, and production
build. Maven tests validate the Java desktop layer and backend-update contract.

## Project structure

```text
.
├── src-ts/                         TypeScript workflow engine
│   ├── server.ts                   Local API, queue, jobs, and lifecycle
│   ├── counties.ts                 County workflow registry
│   ├── county-utils.ts             Shared HTTP, parsing, PDF, and naming tools
│   ├── {county}.ts                 County-specific integrations
│   ├── tax-collector.ts            Shared tax-bill workflow
│   └── config.ts                   Runtime paths and environment settings
├── src/main/java/com/big3/desktop/ JavaFX client and Node process manager
├── test-ts/                        TypeScript/Node tests
├── src/test/                       Java tests
├── scripts/                        Development and release tooling
├── package.json                    Node scripts and dependencies
└── pom.xml                         Java build configuration
```

## Configuration

| Variable | Default | Purpose |
| --- | --- | --- |
| `BIG3_AUTH_API_URL` | Waterview production API | Override the account service; non-local URLs must use HTTPS |
| `BIG3_OUTPUT_DIR` | `~/Documents/Big3 Agent Output` | Change the generated-file library |
| `BIG3_LOG_DIR` | `~/Library/Application Support/Big3 Agent/logs` | Change the engine log directory |
| `BIG3_PORT` | `5022` | Change the standalone engine port; the desktop app selects an available port automatically |
| `BIG3_NODE_ENGINE_DIR` | Auto-detected | Point JavaFX at a directory containing `dist/server.js` |
| `BIG3_MAX_DEED_DOWNLOADS` | `3` | Limit deed downloads during supported Full Search workflows |
| `PLAYWRIGHT_BROWSERS_PATH` | Playwright default | Override the browser-runtime location |
| `BIG3_AUTO_UPDATE` | `1` | Set to `0` to disable signed backend-update checks |
| `BIG3_UPDATE_MANIFEST_URL` | Stable GitHub backend feed | Override the signed update manifest |
| `BIG3_BACKEND_HOME` | Application Support directory | Override downloaded backend storage |
| `BIG3_REQUIRE_AUTH` | `0` standalone; `1` in the desktop app | Require authentication on local API routes |
| `BIG3_DESKTOP_TOKEN` | Generated by JavaFX | Supply the local API bearer token when authentication is enabled |

## Packaging and releases

Build a self-contained macOS ZIP and DMG:

```bash
bash ./scripts/package-macos.sh
```

Build a reviewed, source-only archive suitable for publishing:

```bash
bash ./scripts/package-source.sh
```

Version tags and the **Release macOS app** GitHub Actions workflow publish ZIP,
DMG, and SHA-256 checksum assets. See [RELEASING.md](RELEASING.md) for the
release process and [BACKEND_UPDATES.md](BACKEND_UPDATES.md) for signed
logic-only updates.

## Security, support, and privacy

- Read [SECURITY.md](SECURITY.md) before reporting a vulnerability.
- Follow [SUPPORT.md](SUPPORT.md) when reporting a bug.
- Review [PRIVACY.md](PRIVACY.md) for data-handling details.

Never attach passwords, access tokens, unsanitized logs, or private property
documents to a public issue.

## License

Copyright © 2026 Jon Lack and WaterviewAI. This project is source-visible, not
open source. The compiled application may be used as permitted by
[LICENSE](LICENSE); modification, redistribution, hosted use, and derivative
works require written permission unless the license expressly says otherwise.
