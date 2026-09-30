---
description: Send transactional mail via the Postmark API.
icon: paper-plane
---

# Postmark

Sends mail through the [Postmark](https://postmarkapp.com/) HTTP API. File attachments added with `addAttachments()` are encoded for Postmark automatically. Requests have a 30 second timeout.

## Configuration

```javascript
mailers : {
    "postmark" : {
        class      : "Postmark",
        properties : { apiKey : getSystemSetting( "POSTMARK_API_KEY" ) }
    }
}
```

| Property | Required | Description |
| -------- | -------- | ----------- |
| `apiKey` | Yes | Your Postmark server token, sent as `X-Postmark-Server-Token` |

A missing `apiKey` throws `PostmarkProtocol.PropertyNotFound`.

{% hint style="warning" %}
Never commit API keys. Read them from environment variables or secrets with `getSystemSetting()`.
{% endhint %}
