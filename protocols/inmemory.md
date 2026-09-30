---
description: Keep sent mail in an array so your tests can assert on it.
icon: memory
---

# InMemory

Stores each payload configuration in an in-memory array instead of sending it. Use it in your test suite to assert that mail was sent, without a mail server.

## Configuration

```javascript
mailers : {
    "memory" : { class : "InMemory" }
}
```

It has no properties.

## Methods

| Method | Description |
| ------ | ----------- |
| `getMail()` | Returns the array of stored mail configurations |
| `hasMessage( callback )` | Returns `true` if any stored message makes the closure return `true` |
| `reset()` | Empties the array and returns the protocol |

## Testing Example

```javascript
it( "sends the welcome mail", function(){
    var mailService = getInstance( "MailService@cbmailservices" );
    var memory = mailService.getMailer( "memory" ).transit;
    memory.reset();

    getInstance( "UserService" ).register( { email : "luis@example.com" } );

    expect(
        memory.hasMessage( ( mail ) => mail.to == "luis@example.com" && mail.subject contains "Welcome" )
    ).toBeTrue();
} );
```
