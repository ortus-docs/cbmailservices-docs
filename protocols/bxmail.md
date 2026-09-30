---
description: Send mail through BoxLang's native bx:mail component.
icon: server
---

# BXMail

Added in `2.11.0`. Sends mail with the BoxLang `bx:mail` component, passing the mail payload configuration as the attribute collection. Mail params, parts and attachments are processed as `bx:mailparam` and `bx:mailpart`.

{% hint style="success" %}
This is the recommended protocol for BoxLang applications.
{% endhint %}

## Configuration

```javascript
mailers : {
    "default" : { class : "BXMail" }
}
```

It has no required properties. Everything the `bx:mail` tag understands, such as `server`, `port`, `username`, `password`, `useTLS` and `useSSL`, can be set in the module [`defaults`](../essentials/configuration.md#defaults) or on each mail payload.

```javascript
defaults : {
    from     : "info@mydomain.com",
    server   : "smtp.mydomain.com",
    port     : 587,
    username : getSystemSetting( "SMTP_USER" ),
    password : getSystemSetting( "SMTP_PASS" ),
    useTLS   : true
}
```

{% hint style="warning" %}
`BXMail` only works on BoxLang. On CFML engines use [CFMail](cfmail.md).
{% endhint %}
