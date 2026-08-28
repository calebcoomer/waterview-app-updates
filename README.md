# Big3 Agent

Big3 Agent is a macOS desktop application for collecting Florida property data
and public-record documents in one workflow. Choose a supported county, search
for a property, and Big3 organizes the available property details, appraiser
documents, deeds, and tax bills into a local folder.

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

### First launch on macOS

Current public builds are not signed with an Apple Developer ID or notarized by
Apple, so macOS will normally block the app the first time it opens.

1. Double-click **Big3 Agent.app** once. When macOS displays the security
   warning, dismiss it.
2. Open **Apple menu → System Settings → Privacy & Security**.
3. Scroll down to **Security**, find the message that Big3 Agent was blocked,
   and click **Open Anyway**.
4. Enter your Mac password or use Touch ID if prompted, then confirm by clicking
   **Open**.

The **Open Anyway** option appears only after an attempted launch and may be
available for about an hour. Once approved, Big3 Agent is saved as an exception
and can be opened normally. Only override this protection for a copy downloaded
from the official release page. See [Apple's instructions for opening an app
from an unknown developer](https://support.apple.com/guide/mac-help/open-a-mac-app-from-an-unknown-developer-mh40616/mac)
for additional guidance.

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

## License

Copyright © 2026 Jon Lack and WaterviewAI. This project is source-visible, not
open source. The compiled application may be used as permitted by
[LICENSE](LICENSE); modification, redistribution, hosted use, and derivative
works require written permission unless the license expressly says otherwise.
