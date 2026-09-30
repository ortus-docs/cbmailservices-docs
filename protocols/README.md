---
description: The mail protocols bundled with cbMailServices and how to register them as mailers.
icon: plug
---

# Protocols

A **protocol** is the class that actually delivers a mail payload. You register protocols as named **mailers** in the [configuration](../essentials/configuration.md) and pick one per mail with the `mailer` argument. The module bundles seven protocols, each with a short alias you can use in the `class` key:

| Alias | Delivery | Required properties | Best for |
| ----- | -------- | ------------------- | -------- |
| [`BXMail`](bxmail.md) | BoxLang `bx:mail` | none | BoxLang apps (recommended) |
| [`CFMail`](cfmail.md) | CFML `cfmail` | none | Lucee and Adobe ColdFusion |
| [`File`](file.md) | HTML files on disk | `filePath` | Development and the [mail viewer](../essentials/development-mail-viewer.md) |
| [`InMemory`](inmemory.md) | An array in memory | none | Automated tests |
| [`Null`](null.md) | Nothing | none | Silencing mail |
| [`Postmark`](postmark.md) | Postmark HTTP API | `apiKey` | Transactional mail |
| [`Mailgun`](mailgun.md) | Mailgun HTTP API | `apiKey`, `domain` | Transactional mail |

```javascript
mailers : {
    "default"  : { class : "BXMail" },
    "files"    : { class : "File",     properties : { filePath : "/logs/mail" } },
    "postmark" : { class : "Postmark", properties : { apiKey : "abc" } }
}
```

Every protocol returns the same result structure, so your [callbacks](../essentials/sending-mail.md#callbacks) and [interceptors](../advanced/mail-events.md) work the same no matter how the mail is delivered:

```javascript
{ "error" : false, "messages" : [] }
```

{% hint style="info" %}
Need something else such as SES or SendGrid? See [Building Protocols](../advanced/building-protocols.md).
{% endhint %}
