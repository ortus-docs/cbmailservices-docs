---
description: August 29, 2026
icon: inbox
---

# What's New With 2.12.0

## Added

* New **development mail viewer** at `/cbmailservices/log`. It is an interactive, development-only inbox for the mail written by your `File` protocol mailers. See the [Development Mail Viewer](../../essentials/development-mail-viewer.md) guide.
* Message search, preview and source tabs, light and dark themes, and 5 second auto-refresh.
* The `File` protocol now embeds `data-from`, `data-to`, `data-subject` and `data-sent` metadata in each log file so the viewer can list messages quickly.
* New `development()` module routes (`/log`, `/log/messages`, `/log/message/:id`) that only register in the `development` environment.
