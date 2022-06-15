---
description: cbMailServices is a module to send email in a fluent and abstracted approach.
---

# Introduction

## Welcome to the ColdBox Mail Services

Sending email doesn't have to be complicated or archaic. The ColdBox Mail Services (`cbmailservices`) module will allow you to send email in a _fluent_ and _abstracted_ way in multiple protocols for many environments in a **single cohesive API.** The supported protocols are:

* **CFMail** - Traditional `cfmail` sending
* **File** - Write emails to disk
* **InMemory** - Store email mementos in an array. Perfect for testing.
* **Null** - Ignores emails sent to it.
* **Postmark** - Send via the PostMark API Service (https://postmarkapp.com/)
* **Mailgun** - Send via the MailGun API Service ([https://www.mailgun.com/](https://www.mailgun.com/))

It also sports tons of useful features for mail sending:

* Async Mail
* Mail Queues
* Mail merging of variables
* Mail attachments, headers and parameters
* View and Layout+View rendering for mail
* Mail tracking
* Multiple mailers
* Success and Error callbacks
* So Much More!

```javascript
newMail( 
	to         : "email@email.com",
	from       : "no_reply@ortussolutions.com",
	subject    : "Mail Services Rock",
	type       : "html", // Can be plain, html, or text
	bodyTokens : { 
		user    : "Luis", 
		product : "ColdBox", 
		link    : event.buildLink( 'home' )
	}
)
.setBody("
    <p>Dear @user@,</p>
    <p>Thank you for downloading @product@, have a great day!</p>
    <p><a href='@link@'>@link@</a></p> 
")
.addAttachment( expandPath( "/tmp/reports/report.pdf" ) )
.setReadReceipt( "myemail@email.com" )
.send()
.onSuccess( function( result, mail ){
	// Process the success
})
.onError( function( result, mail ){
	// Process the error
});
```

## System Requirements

* Lucee 5+
* Adobe ColdFusion 2018+

## Versioning <a href="#versioning" id="versioning"></a>

`cbMailServices` is maintained under the [Semantic Versioning](http://semver.org) guidelines as much as possible. Releases will be numbered with the following format:

```
<major>.<minor>.<patch>
```

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major (and resets the minor and patch)
* New additions without breaking backward compatibility bumps the minor (and resets the patch)
* Bug fixes and misc changes bumps the patch

## License <a href="#license" id="license"></a>

Apache 2 License: [http://www.apache.org/licenses/LICENSE-2.0](https://www.apache.org/licenses/LICENSE-2.0)​

## Important Links <a href="#important-links" id="important-links"></a>

* Code: [https://github.com/coldbox-modules/cbmailservices](https://github.com/coldbox-modules/cbmailservices)​
* Issues: [https://ortussolutions.atlassian.net/browse/BOX](https://ortussolutions.atlassian.net/browse/BOX)​
* Community: [https://community.ortussolutions.com/](https://community.ortussolutions.com)​

## Professional Open Source <a href="#professional-open-source" id="professional-open-source"></a>

![www.ortussolutions.com](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LA-UVvG0NM7NpDzssBL%2F-LA-Uaei0WzTH7Su5CR7%2F-LA-UqN1BRXynZ7RUVO7%2Fortussolutions\_button.png?generation=1523647999385555\&alt=media)

This module is professional open source software backed by [Ortus Solutions, Corp](http://www.ortussolutions.com/services) offering services like:

* Custom Development
* Professional Support & Mentoring
* Training
* Server Tuning
* Security Hardening
* Code Reviews
* [Much More](http://www.ortussolutions.com/services)

## HONOR GOES TO GOD ABOVE ALL <a href="#honor-goes-to-god-above-all" id="honor-goes-to-god-above-all"></a>

Because of His grace, this project exists. If you don't like this, then don't read it, it's not for you.

> "Therefore being justified by **faith**, we have peace with God through our Lord Jesus Christ: By whom also we have access by **faith** into this **grace** wherein we stand, and rejoice in hope of the glory of God." Romans 5:5

