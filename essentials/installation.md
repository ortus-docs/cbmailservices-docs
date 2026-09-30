---
description: Install cbMailServices with CommandBox and start sending mail.
icon: download
---

# Installation

Leverage [CommandBox](https://www.ortussolutions.com/products/commandbox#download) to install into your ColdBox app:

```bash
box install cbmailservices
```

This will install the module in your application so you can [configure](configuration.md) it and [use it](sending-mail.md). By default, it will register a mixin helper called `newMail()` and a WireBox ID: `MailService@cbmailservices` which you can inject into your models to send mail.

It will register a `default` mailer that uses the engine's mail protocol (`BXMail` for BoxLang, `CFMail` for CFML engines). You can find all of the API Docs here:

## Next Steps

1. [Configure](configuration.md) your mailers and defaults
2. [Send your first mail](sending-mail.md)
3. See it in your browser with the [Development Mail Viewer](development-mail-viewer.md)

## API Docs

{% embed url="https://s3.amazonaws.com/apidocs.ortussolutions.com/coldbox-modules/cbmailservices/2.13.0/index.html" %}
