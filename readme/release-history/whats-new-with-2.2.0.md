---
description: June 15 2022
---

# What's New With 2.2.0

## New Features and Improvements

Let’s investigate the new features and improvements.

### PreMailSend

* Ability for the `preMailSend` event to influence the `mail` record thanks to @gpickin

### New Module Setting: `runQueueTask`&#x20;

* This setting is defaulted to `true`.&#x20;
  * If `false` it will not run the mail queue task in the background

### Mailgun Mailer Protocol

You can now register a MailGun protocols by using its alias:

```jsx
moduleSettings = {
	cbMailservices : {
		defaultProtocol : "default",
		mailers : {
			"default" : { class : "CFmail" },
			"memory" : { class : "InMemory" },
			"mailgun" = {
				class = "Mailgun",
				// Required properties
				properties = {
					ApiKey  = '123',
					domain  = 'mg.somedomain.com'
				}
			}
		}
	}
}
```

###
