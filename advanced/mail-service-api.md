---
description: Reference for the MailService API, including runtime mailer registration, default settings and queue processing.
icon: code
---

# Mail Service API

`MailService@cbmailservices` is the engine behind `newMail()`. Inject it anywhere:

```javascript
property name="mailService" inject="MailService@cbmailservices";
```

## Sending

| Method | Returns | Description |
| ------ | ------- | ----------- |
| `newMail( ... )` | `Mail` | Creates a payload. Every argument seeds the mail config. Pass `mailer` to pick a non-default mailer. |
| `send( mail )` | `Mail` | Validates, parses [body tokens](../essentials/sending-mail.md#body-tokens), announces `preMailSend`, sends with the mailer, announces `postMailSend`. A failed validation sets an error result and never reaches the protocol. |
| `sendAsync( mail )` | `Future` | Sends on a ColdBox [async](async-mail.md) thread. |
| `queue( mail )` | `string` | Adds the mail to the in-memory queue and returns a task id. |
| `processQueue()` | | Sends every mail queued at the time of the call, first in first out. The scheduler calls this every minute when `runQueueTask` is `true`. You can also call it manually. |
| `parseTokens( mail )` | | Replaces `@token@` markers in the body and mail parts. Skipped when `type` is `template`. |

## Mailers

| Method | Returns | Description |
| ------ | ------- | ----------- |
| `registerMailer( name, class, properties )` | `MailService` | Registers one mailer. `class` is a protocol alias, a WireBox ID or a class path. |
| `registerMailers( mailers )` | `MailService` | Registers a struct of `{ class, properties }` by name. Throws `InvalidDefaultProtocol` if the default protocol is not among them. |
| `getMailer( name )` | `struct` | Returns `{ class, properties, transit }`. Throws `UnregisteredMailerException` for unknown names. |
| `getDefaultMailer()` | `struct` | The mailer record for `defaultProtocol`. |
| `getRegisteredMailers()` | `array` | The names of the registered mailers. |

```javascript
mailService.registerMailer(
    name       : "audit",
    class      : "File",
    properties : { filePath : "/logs/audit" }
);

newMail( mailer : "audit", to : "a@b.com", from : "c@d.com", subject : "Hi" )
    .setBody( "Hello" )
    .send();
```

{% hint style="info" %}
The `transit` key is the instantiated protocol. It is how you reach protocol specific methods like [InMemory](../protocols/inmemory.md)'s `hasMessage()`.
{% endhint %}

## Default Settings

The [`defaults`](../essentials/configuration.md#defaults) are available at runtime:

| Method | Description |
| ------ | ----------- |
| `getDefaultSetting( setting, defaultValue )` | Returns the default or `defaultValue`. Throws `SettingNotFoundException` if neither exists. |
| `setDefaultSetting( setting, value )` | Sets a default for all future mail. |

## Properties

| Property | Description |
| -------- | ----------- |
| `tokenMarker` | The token marker symbol |
| `defaultProtocol` | The default mailer name |
| `defaultSettings` | The struct of mail defaults |
| `mailers` | The struct of registered mailer records |
| `mailQueue` | The `ConcurrentLinkedQueue` backing `queue()` |
