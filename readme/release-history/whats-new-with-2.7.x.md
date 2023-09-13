---
description: January 16, 2023
---

# What's New With 2.7.x

## \[v2.7.1] => 2023-FEB-14

### Fixed

* Fix usage of invalid named member function #33 ([https://github.com/coldbox-modules/cbmailservices/pull/33](https://github.com/coldbox-modules/cbmailservices/pull/33))

## \[v2.7.0] => 2023-JAN-16

A big thanks to [@richardherbert](https://github.com/richardherbert) for all the updates in this release.

### Fixed

* FIXED var scoping of attachments variable
* Updated to handle a response that is not JSON
* 🐛 FIX: Update GHA to avoid deprecated syntax

### Added

* Added test for `MAILGUN_BASEURL` property
* Updated to make `MAILGUN_APIURL` optional
* Added support for Mailgun EU region by making `MAILGUN_APIURL` an optional property with `https://api.mailgun.net/v3/` as the default.

### Changed

* Updated all GHA actions to the latest versions and moved to use `temurin` Java distributions due to deprecation of the service.
