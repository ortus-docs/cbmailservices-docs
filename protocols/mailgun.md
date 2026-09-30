---
description: Send transactional mail via the Mailgun API, including EU region support.
icon: paper-plane
---

# Mailgun

Sends mail through the [Mailgun](https://www.mailgun.com/) HTTP API. Requests have a 30 second timeout.

## Configuration

```javascript
mailers : {
    "mailgun" : {
        class      : "Mailgun",
        properties : {
            apiKey  : getSystemSetting( "MAILGUN_API_KEY" ),
            domain  : "mailgun.example.com",
            baseURL : "https://api.eu.mailgun.net/v3/"
        }
    }
}
```

| Property | Required | Default | Description |
| -------- | -------- | ------- | ----------- |
| `apiKey` | Yes | | Your Mailgun secret API key |
| `domain` | Yes | | The Mailgun domain to send through |
| `baseURL` | No | `https://api.mailgun.net/v3/` | Override for another [region](https://documentation.mailgun.com/en/latest/api-intro.html#mailgun-regions-1) such as the EU |

Missing properties throw `MailgunProtocol.PropertyNotFound`.

## Payload Mapping

| Mail payload | Mailgun |
| ------------ | ------- |
| `replyto` | `h:Reply-To` header |
| `test : true` | `o:testmode` option, Mailgun accepts but does not deliver |
| Mail params with a `name` | Custom headers, prefixed with `h:` unless they already start with `v:`, `o:` or `h:` |
| `bodyTokens` | Custom variables, prefixed with `v:` |
| `additionalInfo` items | Sent as top level Mailgun fields |
| `type : "html"` body | `html` field, any other type is sent as `text` |
| Mail parts | `html` and `text` fields |
| File mail params | `attachment` uploads |

The result also contains the `messageID` returned by Mailgun.

```javascript
newMail(
    to      : "user@example.com",
    from    : "hello@mailgun.example.com",
    subject : "Hello",
    mailer  : "mailgun",
    test    : true
)
.setBody( "Hi there" )
.addMailParam( name : "X-Campaign", value : "welcome" )
.send();
```
