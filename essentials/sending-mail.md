# Sending Mail

You can initiate a mail payload via the mixin helper (`newMail()`) or via the injected mail service's `newMail()` method. The arguments you pass into this method will be used to seed the payload with all the arguments passed to the `cfmail` tag or the chosen protocol properties. You can also pass an optional `mailer` argument that will override the `default` protocol to one of your likings.

## Helper

The `newMail()` helper is useful so you can send mail from your handlers and interceptors.

{% hint style="info" %}
Please note that the mixin helper can **ONLY** be used in handlers, interceptors, layouts and views. You will need to use the delegater if you want to send mail from your models.
{% endhint %}

```javascript
// Mixin Helper Approach
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
.send()
.onSuccess( function( result, mail ){
	// Process the success
})
.onError( function( result, mail ){
	// Process the error
});
```

## Delegate

If you are using ColdBox 7 you can use the `Mailable@cbMailservices` delegate to add mailing capabilities to ANY model managed by WireBox.  It will add the `newMail()` method to your objects:

```javascript
component name="UserService" delegates="Mailable@cbMailservices"{

  ...
    newMail()
      .send();

}
```

## MailService

Use the WireBox ID of `MailService@cbmailservices` to inject the service in any model object to send mail from your models.

```javascript
component{

	property name="mailService" inject="MailService@cbmailservices";

	...

	function submitOrder( required order ){

		...

		variables.mailService
		.newMail( 
			to         : "email@email.com",
			from       : "no_reply@ortussolutions.com",
			subject    : "Mail Services Rock",
			type       : "html",
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
		.send()
		.onSuccess( function( result, mail ){
			// Process the success
		})
		.onError( function( result, mail ){
			// Process the error
		});

	}

}
```

## Mail Payload

The return of the `newMail()` calls will be a `Mail` payload object of type `cbmailservices.models.Mail`.  You can find all the full API Docs here:

{% embed url="https://s3.amazonaws.com/apidocs.ortussolutions.com/coldbox-modules/cbmailservices/2.0.0/models/Mail.html" %}
cbmailservices.models.Mail
{% endembed %}

This object will be used to set properties that the mail protocols can use to send mail.  We also have several utility methods that can be used to set mail headers, attachments, read receipts, send receipts and so much more.  Let's start investigating the Mail Payload Object.

### Constructor

The `newMail()` or `configure()` method is used to initiate and configure a mail payload.  Any argument you pass into these methods will be used to seed the mail `config` property which is used by all protocols to send email out.  Example, the `cfmail` protocol will read all those properties and pass them as an attribute collection to the `cfmail` tag.

```javascript
variables.mailService
.newMail( 
	to         : "email@email.com",
	from       : "no_reply@ortussolutions.com",
	subject    : "Mail Services Rock",
	type       : "html",
	bodyTokens : { 
		user    : "Luis", 
		product : "ColdBox", 
		link    : event.buildLink( 'home' )
	}
)

newMail()
.configure(
	to         : "email@email.com",
	from       : "no_reply@ortussolutions.com",
	subject    : "Mail Services Rock",
	type       : "html",
	bodyTokens : { 
		user    : "Luis", 
		product : "ColdBox", 
		link    : event.buildLink( 'home' )
	}
)
```

### Send()

You will leverage the `send()` method to initiate a call to the mail protocols to deliver your mail using your payload.  You can then tap into the results of the mailing via the mail [callbacks](sending-mail.md#callbacks) or the [mail helper methods](sending-mail.md#mail-helper-methods).  Please also note that the sending operation [announces two interceptions](../advanced/mail-events.md) points (`preMailSend, postMailSend`) that you can use to influence the mail payload or listen to mail results.

### Callbacks

The mail payload allows you to register two callbacks to determine what happened to the mail payload when sending:

* `onSuccess( callback )`
* `onError( callback )`

Each `callback` argument is a function/closure/lambda that receives two arguments:

* `result` : The result structure with at least two keys: `{ error :boolean, messages: array }`
* `mail` : The mail payload itself.

```javascript
.send()
	.onSuccess( function( result, mail ){
		// Process the success
	})
	.onError( function( result, mail ){
		// Process the error
	});
```

### Changing Mailers

You can easily change to use a specific mailer protocol by specifying the `mailer` argument to the `newMail()` calls or by calling the `setMailer( mailer )` method.

```javascript
newMail( mailer : "files" ),,,


newMail()
	.setMailer( "files" )
```

### Body Tokens

The mail service allows you to register a structure of tokens that can be replaced by key name on the **body** content for you. The tokens are demarcated by the `tokenMarker` setting which defaults to `@`.  Here is the token pattern:

```js
@tokenName@
```

Before sending the mail, the service will replace all the tokens with the specific key names in your content and then send the mail. You can use the `bodyTokens` argument to the `newMail() or configure()` methods, or you can use the `setBodyTokens()` method.

```javascript
// Via constructor
newMail( 
	to         : "email@email.com",
	from       : "no_reply@ortussolutions.com",
	subject    : "Mail Services Rock",
	type       : "html",
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
.send()

// Body Tokens Method
newMail( 
	to         : "email@email.com",
	subject    : "Mail Services Rock",
	type       : "html",
)
.setBodyTokens( { 
	user    : "Luis", 
	product : "ColdBox", 
	link    : event.buildLink( 'home' )
})
.setBody("
	<p>Dear @user@,</p>
	<p>Thank you for downloading @product@, have a great day!</p>
	<p><a href='@link@'>@link@</a></p> 
")
.send()
```

### Rendering Views

You can also set the body of the email to be a view or a layout+view combination using the `setView()` method. Here is the method signature:

```javascript
/**
 * Render or a view layout combination as the body for this email.  If you use this, the `type`
 * of the email will be set to `html` as well.  You can also bind the view/layout with
 * the args struct and use them accordingly.  You can also use body tokens that the service will
 * replace for you at runtime.
 *
 * @view The view to render as the body
 * @args The structure of arguments to bind the view/layout with
 * @module Optional, the module the view is located in
 * @layout Optional, If passed, we will render the view in this layout
 * @layoutModule Optional, If passed, the module the layout is in
 */
Mail function setView(
	required view,
	struct args = {},
	module      = "",
	layout,
	layoutModule = ""
)
```

Please note that you can bind your views and layotus with the `args` structure as well. You can also use the `bodyTokens` in your views. Then you can use it in your mail sending goodness:

```javascript
newMail( 
	to         : "email@email.com",
	subject    : "Mail Services Rock",
	type       : "html",
)
.setBodyTokens( { 
	user    : "Luis", 
	product : "ColdBox", 
	link    : event.buildLink( 'home' )
})
.setView( view : "emails/newUser" )
.send()

newMail( 
	to         : "email@email.com",
	subject    : "Mail Services Rock",
	type       : "html",
)
.setView( view : "emails/newUser", layout : "emails" )
.send()
```

## Mail Attachments

You can easily add mail attachments using mail params (next section) directly or our fancy helper method called `addAttachments()`.&#x20;

### Signature

Here is our method signature:

```javascript
/**
 * Add attachment(s) to this payload using a list or array of file locations
 *
 * @files A list or array of files to attach to this payload
 * @remove If true, ColdFusion removes attachment files (if any) after the mail is successfully delivered.
 */
Mail function addAttachments( required files, boolean remove = false )
```

The `files` argument can be a list of file locations or an array of file locations to send.

### Example

```javascript
newMail(
	subject = "Hello",
	from    = "lmajano@mail.com",
	to      = "lmajano@mail.com",
	body    = "Here are your docs"
)
.addAttachments( "c:\temp\reports\report.pdf", true )
.addAttachments( expandpath( "/reports/anotherReport.pdf" ), true )
.addAttachments( [
	expandPath( "/logs/maillog.txt" )
	expandPath( "/logs/maillog2.txt" )
], true )
.send();
```

## Mail Params

You can easily add mail parameters (`cfmailparam`) to a payload so you can attach headers or files to the message by using the `addMailParam()` method. Please see the https://cfdocs.org/cfmailparam cfmail param docs for more information.

### **Signature**

```javascript
/**
 * Attach a file or adss a header to the email payload
 * 
 * @contentID The Identifier for the attached file.
 * @disposition How the attached file is to be handled: attachment, inline
 * @file Attaches file to a message. Mutually exclusive with name argument.
 * @type The MIME media type for the attachment.
 * @name The name of the email header to attach. See https://cfdocs.org/cfmailparam. Mututally exclusive with file
 * @value The value of the header
 * @remove Tells ColdFusion to remove any attachments after sucdcesful mail delivery
 * @content Lets you send the contents of a ColdFusion variable as an attachment 
 */
Mail function addMailParam(
	contentID,
	disposition,
	file,
	type,
	name,
	value,
	boolean remove,
	content
)
```

### **Example**

```javascript
newMail()
	.configure(
		from    = "info@coldbox.org",
		to      = "automation@coldbox.org",
		subject = "Mail With Params - Hello Luis"
	)
	.setBody( "Hello This is my great unit test" )
	.addMailParam(
		name  = "Disposition-Notification-To",
		value = "info@coldbox.org"
	)
	.addMailParam( name = "Importance", value = "High" )
	.send();
```

## Mail Parts

You can also add mail parts via the `cfmailpart` feature of `cfmail` (https://cfdocs.org/cfmailpart). This allows you to build multi-parted emails.

### **Signature**

```javascript
/**
 * Add a new mail part to this mail payload
 * 
 * @charset The charset of the part, defaults to utf-8
 * @type The valid mime type: text/plain or text/html
 * @wraptext Specifies the maximum line length, in characters of the mail text.
 * @type The MIME media type for the attachment.
 * @body The body of the email according to the type.
 */
Mail function addMailPart(
	charset = "utf-8",
	type,
	numeric wraptext,
	body
){
```

### **Example**

```javascript
newMail(
	from    = "info@coldbox.org",
	to      = "automation@coldbox.org",
	subject = "Mail MultiPart No Params - Hello Luis"
)
.addMailPart(
	type = "text",
	body = "You are reading this message as plain text, because your mail reader does not handle it."
)
.addMailPart( type = "html", body = "<h1>This is the body of the message.</h1>" )
.send()
```

## Mail Helper Methods

We have also registered several methods to help you when sending mail:

* `setReadReceipt( email )` - Set the read receipt email
* `setSendReceipt( email )` - Set the send receipt email
* `setHtml( body )` - Set a multi-part body for html
* `setText( body )` - Set a multi-part body for text
* `addAttachments( files, remove=false)` - Easily add attachments
* `getMemento()` - Get the entire mail settings for the payload
* `hasErrors():boolean` - Verifies if there are any errors in the mailing
* `getResultMessages():array` - Get's the array of messages of the sending of the mail
* `getResults():struct` - Get the structure of the results of sending the mail

## Mail Additional Info

The `Mail` object has some additional methods to allow you to pass additional information so protocols can leverage them:

```java
mail.setAdditionalInfo( struct );
mail.getAdditionalInfo();

mail.seavatAdditionalInfoItem( key, value );
mail.getAdditionalInfoItem( key );
```
