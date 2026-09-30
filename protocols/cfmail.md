---
description: Send mail through the CFML cfmail tag on Lucee and Adobe ColdFusion.
icon: envelope
---

# CFMail

Sends mail with the `cfmail` tag, passing the mail payload configuration as the attribute collection. Mail params, parts and attachments are processed as `cfmailparam` and `cfmailpart`. This is the module's built-in default mailer for CFML engines.

## Configuration

```javascript
mailers : {
    "default" : { class : "CFMail" }
}
```

It has no required properties. Any attribute that `cfmail` supports can be seeded in the module [`defaults`](../essentials/configuration.md#defaults) or on each mail payload:

```javascript
defaults : {
    from   : "info@mydomain.com",
    server : "smtp.mydomain.com",
    port   : 587,
    useTLS : true
}
```

If you do not set a `server`, the engine's administrator mail settings are used.
