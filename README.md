# Harbor Download Helper for Firefox

Send download links from Firefox directly to Harbor, the macOS download manager.

Harbor Download Helper is a Firefox extension that sends links to Harbor through the `harbor://` URL scheme. It provides right-click actions, a toolbar popup, and optional automatic download forwarding.

## Features

- **Right-click integration** — Send links and the current page to Harbor.
- **Quick popup** — Send the current tab URL or a URL you provide.
- **Automatic download forwarding** — Optionally hand eligible Firefox downloads to Harbor.
- **Native messaging** — Communicate with Harbor through a local macOS helper.
- **Referrer forwarding** — Pass the referring page URL when available.
- **Connection testing** — Check whether the native helper is reachable.
- **Notifications** — Receive handoff status and error messages.

## Requirements

- macOS
- Mozilla Firefox compatible with the extension's manifest
- Harbor installed
- The Harbor Firefox Native Helper
- Python 3 for the native helper, if required by its implementation

**Important:** Installing the Firefox extension alone is not enough. The native helper must also be installed and registered on your Mac.

## Installation

### 1. Install the Firefox extension

When available, install it from the [Firefox Add-ons website](https://addons.mozilla.org/firefox/).

You can also distribute a Mozilla-signed release through the project's GitHub Releases page.

### 2. Install the native helper

Download the matching native-helper package from the project's Releases page and extract it.

Open Terminal in the extracted folder and run:

```bash
chmod +x install-firefox.sh
./install-firefox.sh
```

Follow the installer's instructions. The helper registers Firefox Native Messaging so the extension can communicate with Harbor locally.

Completely quit Firefox with **Cmd + Q**, then reopen it.

### 3. Test the connection

1. Open Firefox.
2. Click the Harbor extension icon.
3. Run the native-helper connection test, if available.
4. Right-click a download link and choose the Harbor action.

## Automatic Download Forwarding

Automatic forwarding is optional and should be enabled in the extension settings.

When enabled, the extension attempts to hand eligible download URLs to Harbor rather than downloading them through Firefox. A successful handoff means the URL was passed to the local handoff mechanism; it does not guarantee that Harbor completed the download.

If forwarding fails, behavior depends on the extension's current implementation and Firefox's download state. Test this carefully before relying on automatic forwarding.

## Privacy

The extension is intended to pass URLs locally to Harbor through the native messaging helper.

- Download URLs and available referrer URLs may be sent to the local helper.
- Do not assume that cookies, login sessions, authentication state, or request headers are transferred.
- Some downloads that require an authenticated browser session may not work in Harbor.
- Review the extension's Mozilla Add-ons privacy disclosures for the exact data-collection declaration.

## Permissions

The extension's manifest may request permissions such as the following, depending on the published version:

| Permission | Purpose |
| --- | --- |
| `downloads` | Detect and manage downloads during handoff. |
| `contextMenus` | Add Harbor actions to the right-click menu. |
| `nativeMessaging` | Communicate with the local native helper. |
| `notifications` | Display status and error messages. |
| `storage` | Save extension preferences. |
| `tabs` | Read the current tab URL when needed for a user-requested action. |

Only the permissions present in the published manifest apply.

## Troubleshooting

### The native helper is not connected

- Confirm that the installer completed successfully.
- Check that the native messaging manifest is installed in the expected location.
- Restart Firefox completely.
- Run the connection test again.

### Harbor does not open

- Confirm Harbor is installed and opens normally.
- Check that macOS recognizes the `harbor://` URL scheme.
- Try opening Harbor directly before testing the extension.

### A download does not work after forwarding

- Check that the URL is accessible.
- Some websites require cookies, authentication, or request headers that are not transferred.
- Try the same URL directly in Harbor.

### Temporary testing

1. Extract the extension ZIP.
2. Open `about:debugging#/runtime/this-firefox` in Firefox.
3. Click **Load Temporary Add-on…**.
4. Select the extension's `manifest.json`.
5. Install the native helper separately.

Temporary add-ons are removed when Firefox restarts. For normal distribution, submit the extension to Mozilla for signing.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
