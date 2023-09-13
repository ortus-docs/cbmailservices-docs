---
description: November 2021
---

# What's New With 2.0.0

**cbMailServices** has been rewritten from the ground up for the 2.x release. It was about time to add some 💕 and modernize it. Please note that this is a **major** upgrade and it WILL require some changes on your configuration and usage. We have documented all the changes for you to provide for a smooth transition.

## Compatibility Changes

The following are the changes you will need to do to upgrade to version 2.x.

### Adobe 2016 Support Dropped

It’s too old and too cumbersome to support now. So upgrade to a supported engine.

### MailSettingsBean Dropped

This CFC was unnecessary and dropped in favor of encapsulation in the `MailService`

### Configuration Changes

You will have to place the configuration in the standard ColdBox approach now of `moduleSettings.cbmailservices` in the `config/coldbox.cfc` configuration file instead of the top level `mailServices` struct.

```jsx
moduleSettings = {
	cbMailservices : {

	}
};
```

### No More Top Level Mail Settings

In the previous version you would put any payload mail default settings as a top-level key under the `mailServices` struct. Now you will use the `defaults` struct instead.

```jsx
cbmailservices : { 
  tokenMarker : "@",
  defaults : {
    from : "info@coldbox.org",
    cc : "info@ortus.com",
    server : "myserver"
  }
}
```

### Default Protocol

In 2.x you can define multiple mailers in an application. So the previous `protocol` struct is now removed. You will have to register the protocol by name in the `mailers` struct and add a `defaultProtocol` key which points to the one you define in the mailers.

```jsx
cbmailservices : {
  defaultProtocol : "cfmail",
  mailers : {
    "files" : { class : "File" },
    "cfmail" : { class : "CFMail" }
  }
}
```

If no `defaultProtocol` is added and no `mailers` then the mail services module will register a `cfmail` protocol for you as the **`default` mailer.**

### Mail bean returned from send() method

Previous a struct containing `error` and `errorArray` would be returned from the `send()` method. Now, the `Mail` bean is returned (whether using `mail.send()` or `mailService.send( mail )`). The previously available struct can be accessed using `mail.getResults()`. (Be sure to see the [changes to the `errorArray` name below](whats-new-with-2.0.0.md#no-more-errorarray).)

### No More `errorArray`

The return struct from the mail services has been modified. The `errorArray` has been renamed to `messages`. Which is still an array but can be used by any protocol to register an array of messages about the mailing for any status.

### Mail config() renamed to configure()

The mail payload `config()` method was renamed to `configure()`

```jsx
mailService
	.newMail()
	.configure(
		from    = "info@coldbox.org",
		to      = "automation@coldbox.org",
		subject = "Mail With Params - Hello Luis"
	)
```

### Postmark `message_id` renamed to `messageId`

The return message identifier from postmark has been renamed from `message_id` to `messageID` to be consistent with the postmark API results.



## New Features and Improvements

Let’s investigate now the new features and improvements.

### newMail() Helper

The module now registers several new mixin helpers. You can now use `newMail()` in any handler, and interceptor to start a fluent mailing.

```jsx
function save( event, rc, prc ){
	...

	newMail( to: "lmajano@ortussolutions.com", subject : "Hello" )
		.setBody( "hello body" )
		.send();
}
```

### Mailer Aliases

You can now register your mailer protocols by using their alias instead of the full CFC path:

* CFMail
* File
* InMemory
* Null
* Postmark

```jsx
moduleSettings = {
	cbMailservices : {
		defaultProtocol : "default",
		mailers : {
			"default" : { class : "CFmail" },
			"memory" : { class : "InMemory" }
		}
	}
}
```

### Mailer WireBox ID

You can now also register ANY WireBox ID or class path as the mailer as well. This will allow you to register mailers that come from your application or any other module.

```jsx
moduleSettings = {
	cbMailservices : {
		defaultProtocol : "default",
		mailers : {
			"default" : { class : "CFmail" },
			"amazon" : { class : "Mailer@amazonsns" }
		}
	}
}
```

### Protocol Names

All protocols now have a `name` property that can be used to give a human readable name for all protocols.

```java
/**
 * Initialize the InMemory protocol
 *
 * @properties A map of configuration properties for the protocol
 */
InMemoryProtocol function init( struct properties = {} ){

	variables.name = "InMemory";

	super.init( argumentCollection = arguments );
	variables.mail = [];
	return this;
}
```

### Fluent Mail

The mail payload object has been completely rewritten to allow you to use it to not only construct a mail payload, but to do it in a fluent and more approachable manner. It also allows you to send the payload itself without using the mail service. It also sports a dynamic getter/setter approach for any property stored in the internal `config` structure. This structure is used to model all the mail settings and properties that will be used to send mail through any protocol.

```jsx
mailService
	.newMail()
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
	.send()
  .onSuccess ( () => {

  } )
  .onError( () => {
  } );
```

#### Success - Errors Callback

The `send()` method will return itself so you can interact with the payload for either success or failures. You will do so using the following methods:

* `OnError( callback )`
* `OnSuccess ( callback )`

```jsx
newMail( to: "lmajano@ortussolutions.com" )
.setSubject( "This is my email confirmation" )
.setBody( "You got in buddy!" )
.send()
  .onSuccess ( () => {

  } )
  .onError( () = > {
  } );
```

#### Utility Methods

The following utility methods can be used for inspecting results and errors.

* `getResults()` : struct. Empty if no results
* `hasErrors()` : boolean. False if no results
* `getResultMessages()` : array. Empty if no results

### Multiple Transmission Mailer Protocols

The configuration allows for a `mailers` structure where you can register extra **named** mailer protocols. You can then use them by name in your mail payloads and set the `defaultProtocol` as well by it's name.

```jsx
cbMailservices : {
	// Default Token marker
	tokenMarker : "@",

	// Default protocol
	defaultProtocol : "file",

	// Mailers
	mailers : {
		file : { class: "", properties : {} },
		postmark : { class: "", properties : {} },
		amazon : { class: "", properties : {} }
	},

  ...

}
```

You can also seed the mailer name in the payload using the `setMailer()` method to register the name of the mailer to use for the payload or use the `mailer` configuration key.

```jsx
// Using constructor
newMail( mailer : "amazon" )

// Using setter
newMail()
	.setMailer( "amazon" )
```

### View Rendering

Allow for the payload to render a view as the body for you instead of doing this manually using the `setView()` method.

```jsx
newMail()
.setView(
	view   : "View",
	module : "view module",
	layout : "layout",
	layoutModule : "layout module",
	args   : {}
)
```

This will tell the mail payload to render the view with or without the layout, pass in the `args` into the view and layout as the `body` of the mailing. If you don't pass a `layout` by convention we will just render the view in isolation.

### Async Sending

Thanks to ColdBox futures you can now send the mailing asynchronously via the sendAsync() method. The return is a ColdBox Future object.

```jsx
future = newMail()
 .setView( "report" )
 .sendAsync()
```

### In Memory Mail Queue

The mail services module will also register a mail scheduler that will iterate and send mail payload asynchronously. The scheduler runs _every minute_ and iterates and sends all mail in the queue. This allows for your application to not wait and block until mail is sent.

All you need to do is use the `queue()` method. That’s it. The mail services schedule will deliver the mail payload to the right mailer asynchronously and at least every minute schedule.

```jsx
var taskId = newMail()
 ...
 .queue();
```

Please note that we are in async land, so you won’t be able to process successes or failures. The scheduler will log all activity to the applications logging facilities for either a success or failed mailing. The return of the `queue()` method is a task ID guid which you can use to track in log statements.
