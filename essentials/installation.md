# Installation

Leverage [CommandBox](https://www.ortussolutions.com/products/commandbox#download) to install into your ColdBox app:

```bash
box install cbmailservices
```

This will install the module in your application so you can [configure](configuration.md) it and [use it](sending-mail.md).  By default it will register a mixin helper called `newMail()` and a WireBox ID: `MailService@cbmailservices` which you can inject in your models to send mail.

It will register a `default` mailer that uses the `cfmail` protocol.  You can find all of the API Docs here:

{% embed url="https://s3.amazonaws.com/apidocs.ortussolutions.com/coldbox-modules/cbmailservices/2.0.0/index.html" %}
