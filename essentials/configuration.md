---
description: Every cbMailServices module setting, the supported mail defaults, and how to configure mailers per environment.
icon: gear
---

# Configuration

There are two ways to configure cbmailservices in your application:

{% tabs %}
{% tab title="ColdBox.cfc / ColdBox.bx" %}
Add a `cbmailservices` key under the `moduleSettings` structure in your `config/ColdBox.cfc` (CFML) or `config/ColdBox.bx` (BoxLang):

```javascript
// config/ColdBox.cfc or config/ColdBox.bx
moduleSettings = {
    cbmailservices = {
        // The default token Marker Symbol
        tokenMarker     : "@",
        // Default protocol to use, it must be defined in the mailers configuration
        defaultProtocol : "default",
        // Here you can register one or many mailers by name
        mailers         : {
            "default"  : { class : "BXMail" },
            "files"    : { class : "File",    properties : { filePath : "/logs" } },
            "postmark" : { class : "Postmark", properties : { apiKey : "234" } },
            "mailgun"  : { class : "Mailgun",  properties : {
                apiKey : "234",
                domain : "mailgun.example.com"
            } }
        },
        // The defaults for all mail config payloads and protocols
        defaults : {
            from : "info@mydomain.com",
            cc   : "sales@mydomain.com"
        },
        // Whether the scheduled task is running or not
        runQueueTask : true
    }
};
```
{% endtab %}

{% tab title="Module Config Override (bx)" %}
For BoxLang applications, create a `config/modules/cbmailservices.bx` file:

```java
// config/modules/cbmailservices.bx
class {

    function configure(){
        return {
            // The default token Marker Symbol
            tokenMarker     : "@",
            // Default protocol to use, it must be defined in the mailers configuration
            defaultProtocol : "default",
            // Here you can register one or many mailers by name
            mailers         : {
                "default"  : { class : "BXMail" },
                "files"    : { class : "File",    properties : { filePath : "/logs" } },
                "postmark" : { class : "Postmark", properties : { apiKey : "234" } },
                "mailgun"  : { class : "Mailgun",  properties : {
                    apiKey : "234",
                    domain : "mailgun.example.com"
                } }
            },
            // The defaults for all mail config payloads and protocols
            defaults : {
                from : "info@mydomain.com",
                cc   : "sales@mydomain.com"
            },
            // Whether the scheduled task is running or not
            runQueueTask : true
        }
    }

}
```
{% endtab %}

{% tab title="Module Config Override (CFML)" %}
For CFML applications, create a dedicated configuration file at `config/modules/cbmailservices.cfc`. This approach keeps the mail configuration separate from your main application configuration.

```javascript
// config/modules/cbmailservices.cfc
component {

    function configure(){
        return {
            // The default token Marker Symbol
            tokenMarker     : "@",
            // Default protocol to use, it must be defined in the mailers configuration
            defaultProtocol : "default",
            // Here you can register one or many mailers by name
            mailers         : {
                "default"  : { class : "CFMail" },
                "files"    : { class : "File",    properties : { filePath : "/logs" } },
                "postmark" : { class : "Postmark", properties : { apiKey : "234" } },
                "mailgun"  : { class : "Mailgun",  properties : {
                    apiKey : "234",
                    domain : "mailgun.example.com"
                } }
            },
            // The defaults for all mail config payloads and protocols
            defaults : {
                from : "info@mydomain.com",
                cc   : "sales@mydomain.com"
            },
            // Whether the scheduled task is running or not
            runQueueTask : true
        }
    }

}
```
{% endtab %}
{% endtabs %}

By default, the mail services are configured to send mail via the engine's native mail component (`BXMail` for BoxLang, `CFMail` for CFML engines) using a mailer called `default`.

## ⚙️ Settings Reference

| Setting | Type | Default | Description |
| ------- | ---- | ------- | ----------- |
| `tokenMarker` | string | `@` | The symbol that wraps [body tokens](sending-mail.md#body-tokens) |
| `defaultProtocol` | string | `default` | The name of the mailer used when a mail does not specify `mailer`. It must exist in `mailers`. |
| `mailers` | struct | `{ "default" : { class : "CFMail" } }` | The named [protocol](../protocols/README.md) registrations |
| `defaults` | struct | `{}` | Default mail attributes seeded into every mail payload |
| `runQueueTask` | boolean | `true` | Runs the scheduled task that delivers [queued mail](../advanced/async-mail.md) |

The module also sets an entry point of `cbmailservices` and registers two interception points: `preMailSend` and `postMailSend`. See [Mail Events](../advanced/mail-events.md).

{% hint style="info" %}
The default `mailers` value in the module is `CFMail`. On BoxLang, register `{ class : "BXMail" }` as your `default` mailer to use the native `bx:mail`.
{% endhint %}

#### TokenMarker

The `tokenMarker` is used when doing mail merges with variables. The service will look in the body of the email and do replacements according to the following pattern:

```
@{key}@
```

#### DefaultProtocol

The name of the mailer key will be used by default to send mail. The default is called `default`. For BoxLang applications, consider registering your default mailer with `BXMail` for native BoxLang mail support.

#### Mailers

A structure of mailer protocol registrations by key name. Each mailer is registered with the following pattern:

```javascript
mailerKey : {
    class : "Alias|wireBoxID|CFCPath",
    properties : {}
}
```

#### Defaults

A structure of default variables will be seeded into the Mail payload. The protocols then use these as defaults. For example, the `CFMail` protocol will use all these as defaults to the `cfmail` tag. Anything you pass to `newMail()` overrides the default for that mail.

The following keys are supported:

| Key | Type | Description |
| --- | ---- | ----------- |
| `from`, `to`, `cc`, `bcc`, `replyto`, `failto` | string | Addresses |
| `subject`, `body` | string | Content defaults |
| `type` | string | `html`, `text` or `plain` |
| `charset` | string | Character set of the mail |
| `server`, `port`, `username`, `password` | string, numeric | SMTP server connection |
| `useSSL`, `useTLS` | boolean | Connection security |
| `timeout` | numeric | Connection timeout in seconds |
| `priority` | string or numeric | Mail priority |
| `mailerid` | string | The `X-Mailer` header |
| `debug` | boolean | Default `false`. Enables engine mail debugging |
| `spoolenable` | boolean | Engine spooling of mail |
| `wraptext` | numeric | Wraps text at this column |
| `mimeattach` | string | File to attach as MIME |
| `query`, `group`, `groupcasesensitive`, `maxrows`, `startrow` | various | Query driven mail, same as the `cfmail` tag |

```javascript
defaults : {
    from     : "info@mydomain.com",
    replyto  : "support@mydomain.com",
    type     : "html",
    server   : "smtp.mydomain.com",
    port     : 587,
    username : getSystemSetting( "SMTP_USER" ),
    password : getSystemSetting( "SMTP_PASS" ),
    useTLS   : true
}
```

{% hint style="info" %}
Which attributes are honored depends on the protocol. API protocols like [Postmark](../protocols/postmark.md) and [Mailgun](../protocols/mailgun.md) ignore the SMTP connection attributes.
{% endhint %}

#### RunQueueTask

By default, a task runs every minute to facilitate sending emails asynchronously (non-blocking). Setting `runQueueTask` to `false` will override the default, and the task will not run.

### Mail Protocols

The mail services can send mail via different protocols. Each protocol has its own page with all of its properties. The available protocol aliases you can register are:

* [`BXMail`](../protocols/bxmail.md) (BoxLang native, recommended for BoxLang apps)
* [`CFMail`](../protocols/cfmail.md) (CFML engines)
* [`Null`](../protocols/null.md)
* [`InMemory`](../protocols/inmemory.md)
* [`File`](../protocols/file.md)
* [`Mailgun`](../protocols/mailgun.md)
* [`Postmark`](../protocols/postmark.md)

{% hint style="warning" %}
Please note that some of the protocols have property requirements.
{% endhint %}

```javascript
defaultProtocol : "default",
mailers : {
	// BoxLang Native Mail (bx:mail)
	"default" : {
		class : "BXMail"
	},

	// CFML Native Mail (cfmail)
	"cfmail" : {
		class : "CFMail"
	},

	// FileProtocol
	"files" : {
		class : "File",
		// Required Properties
		properties : {
			filePath   : "logs",
			autoExpand : true
		}
	},

	// NullProtocol
	"null" : {
		class      : "Null",
		properties : {}
	},

	// InMemoryProtocol
	"memory" : {
		class      : "InMemory",
		properties : {}
	},

	// Postmark
	"postmark" : {
		class : "Postmark",
		// Required properties
		properties : {
			apiKey : "123"
		}
	},

	// Mailgun
	"mailgun" : {
		class : "Mailgun",
		// Required properties
		properties : {
			apiKey  : "123",
			domain  : "mailgun.example.com",
			// Optional property, defaults to https://api.mailgun.net/v3/
			// https://documentation.mailgun.com/en/latest/api-intro.html#base-url-1
			// https://documentation.mailgun.com/en/latest/api-intro.html#mailgun-regions-1
			baseURL : "https://api.eu.mailgun.net/v3/" // for the EU region
		}
	}
}
```

#### Mailer WireBox ID

You can also register _ANY_ WireBox ID or classpath as the mailer. This will allow you to register mailers from your application or any other module.

```javascript
moduleSettings = {
	cbmailservices : {
		defaultProtocol : "default",
		mailers : {
			"default" : { class : "CFMail" },
			// Custom amazon mailer from the amazonsns module
			"amazon" : { class : "Mailer@amazonsns" }
		}
	}
}
```

Or using a module config override:

{% tabs %}
{% tab title="BoxLang (.bx)" %}

```java
// config/modules/cbmailservices.bx
class {
    function configure(){
        return {
            defaultProtocol : "default",
            mailers : {
                "default" : { class : "BXMail" },
                "amazon"  : { class : "Mailer@amazonsns" }
            }
        };
    }
}
```
{% endtab %}

{% tab title="CFML (.cfc)" %}

```javascript
// config/modules/cbmailservices.cfc
component {
    function configure(){
        return {
            defaultProtocol : "default",
            mailers : {
                "default" : { class : "CFMail" },
                "amazon"  : { class : "Mailer@amazonsns" }
            }
        };
    }
}
```
{% endtab %}
{% endtabs %}

## 🌎 Environment Specific Configuration

ColdBox environment functions let you swap mailers per environment. A common setup sends real mail in production and writes to disk in development, which also turns on the [Development Mail Viewer](development-mail-viewer.md):

```javascript
// config/ColdBox.cfc
function configure(){
    environments = { development : "localhost,127\.0\.0\.1" };

    moduleSettings = {
        cbmailservices : {
            defaultProtocol : "default",
            mailers         : {
                "default" : { class : "Postmark", properties : { apiKey : getSystemSetting( "POSTMARK_KEY" ) } }
            },
            defaults : { from : "info@mydomain.com" }
        }
    };
}

function development(){
    moduleSettings.cbmailservices.mailers[ "default" ] = {
        class      : "File",
        properties : { filePath : "/logs/mail" }
    };
}
```

## 🧪 Testing Configuration

Use the [InMemory](../protocols/inmemory.md) or [Null](../protocols/null.md) protocol in your test environment so no mail leaves your test runs, and set `runQueueTask` to `false` if you don't want the scheduler running during tests.

## 🔎 Runtime Registration

You can also register mailers at runtime with the [Mail Service API](../advanced/mail-service-api.md), for example from another module.
