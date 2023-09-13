---
description: May 17, 2022
---

# What's New With 2.1.0

### Added

* Ability for the `preMailSend` event to influence the `mail` record thanks to @gpickin
* Getters only work if there is a `variables.config` key in existence. Add reasonable defaults for commonly accessed mail fields
* New module setting: `runQueueTask` which is defaulted to `true`. If `false` it will not run the mail queue task in the background
