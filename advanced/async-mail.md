# Async Mail

The module allows you to either send mail asynchronously or queue it in an in-memory queue so it can be delivered by the mail services scheduler.  So let's explore the asynchronous nature of cbmailservices.

## Async Mail

You can easily send mail asynchronously via the [ColdBox Async Manager](https://coldbox.ortusbooks.com/digging-deeper/promises-async-programming) using the `sendAsync()` method. This will return to you a ColdBox Future object, which then you can use to setup an async pipeline to p

{% embed url="https://coldbox.ortusbooks.com/digging-deeper/promises-async-programming" %}

```javascript
newMail( 
	to         : "email@email.com",
	from       : "no_reply@mydomain.com",
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
.sendAsync()
.then( function( mail ){
	// Async pipeline that can process the mail once it is sent.

})
```

## Mail Queue

You can also detach the mail and let the cbmailservices Mail Queue send it for you. The module's mail scheduler runs on a one-minute interval and will send any mail found in the processing queue. All you need to do is use the `queue()` method and be done!

```javascript
var mailId = newMail( 
	to         : "email@email.com",
	from       : "no_reply@mydomain.com",
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
.queue();
```

The `queue`method will return back a task ID guid, which you can use to track the task down in your logs or via the Mail Service.



The mail scheduler is on by default for convenience but is only needed when using the asynchronous mail feature. It can be turned off, if desired, by these steps:

1. Open config/coldbox.cfc
2. In the modulesSettings section, add a key for cbmailServices with the property `runQueueTask` set to `false`. See the [Configuration ](../essentials/configuration.md#runqueuetask)section for more details.

```
moduleSettings = {
	cbmailServices : {
		runQueueTask: false
	}
}
```
