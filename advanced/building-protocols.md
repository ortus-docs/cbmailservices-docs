# Building Protocols

If you want to build your own protocol you will have to do the following

1. Create a CFC that inherits from `cbmailservices.models.AbstractProtocol`
2. Create the `init()` and `send()` methods
3. Give your protocol a name in the `init()` via the `variables.name` variable.
4. Register it in the `mailers` section of the [configuration](../essentials/configuration.md)

Here are the method signatures of the two methods to implement:

```javascript
/**
 * Constructor
 *
 * @properties The protocol properties to instantiate
 */
function init( struct properties = {} ){
	variables.properties = arguments.properties;
	variables.name = "MyProtocol";
	return this;
}

/**
 * Implemented by concrete protocols to send a message.
 *
 * The return is a struct with a minimum of the following two keys
 * - `error` - A boolean flag if the message was sent or not
 * - `messages` - An array of messages the protocol stored if any when sending the payload
 *
 * @payload The paylod object to send the message with
 * @payload.doc_generic cbmailservices.models.Mail
 *
 * @return struct of { "error" : boolean, "messages" : [] }
 */
struct function send( required cbmailservices.models.Mail payload ){

}
```

That's it!
