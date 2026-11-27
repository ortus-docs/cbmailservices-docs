---
description: November 27, 2026
icon: file-circle-check
---

# What's New With 2.14.0

## Fixed

* The [File protocol](../../protocols/file.md) no longer overwrites mails sent within the same 100 ms. Sending several mails in a loop, such as one per recipient, left only the last mail on disk while `send()` still reported success, and the [development mail viewer](../../essentials/development-mail-viewer.md) showed a single message instead of several.

## Changed

* File protocol mail files are now named `mail.{MM-dd-yyyy}.{HH-mm-ss-SSS}.{8 hex chars}.html`, for example `mail.11-26-2026.21-12-56-123.a3f09c1e.html`. The time prefix now has full milliseconds and still sorts in order. The suffix makes every name unique. Code that matched the old `mail.{MM-dd-yyyy}.{HH-mm-ss-L}.html` pattern needs to be updated.
