---
description: Let's get up and running!
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

A structure of default variables will be seeded into the Mail payload. The protocols then use these as defaults. For example, the `CFMail` protocol will use all these as defaults to the `cfmail` tag.

#### RunQueueTask

By default, a task runs every minute to facilitate sending emails asynchronously (non-blocking). Setting `runQueueTask` to `false` will override the default, and the task will not run.

### Mail Protocols

The mail services can send mail via different protocols. The available protocol aliases you can register are:

* `BXMail` (BoxLang native, recommended for BoxLang apps)
* `CFMail` (CFML engines)
* `Null`
* `InMemory`
* `File`
* `Mailgun`
* `Postmark`

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
