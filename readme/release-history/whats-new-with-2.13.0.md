---
description: September 2, 2026
icon: trash-can
---

# What's New With 2.13.0

## Added

* Delete messages from the [development mail viewer](../../essentials/development-mail-viewer.md): a single message, a selection, or everything in the log.
* New development-only endpoints: `DELETE /cbmailservices/log/message/:id` and `DELETE /cbmailservices/log/messages` (body: `{ "ids": [] }` or `{ "all": true }`).

## Fixed

* Fixed mail preview link targets in the development mail viewer.
