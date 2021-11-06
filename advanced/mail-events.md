# Mail Events

The module will register two [interception points.](https://coldbox.ortusbooks.com/the-basics/interceptors) `PreMailSend` and `PostMailSend`. These interception points are useful to alter the mail object before it gets sent out, and/or perform any functions after the mail gets sent out. An example interceptor would be:

```javascript
component extends="coldbox.system.Interceptor"{

    void function configure(){
    }

    boolean function preMailSend( event, data, buffer, rc, prc ){
        var environment = getSetting('environment');
        var appName = getSetting('appName');

        if(environment eq 'development'){
            //change recipient if we are on development
            data.mail.setTo('johndoe@example.com');  
            //prefix the subject if we are on development
            data.mail.setSubject('<DEV-#appName#> #data.mail.getSubject()#');
        }       
    }

    boolean function postMailSend( event, data, buffer, rc, prc ){
        if( data.result.error ){
            //log mail failure here...
        }
    }

}
```

### preMailSend

Announced before the mail payload is sent to the chosen mailer protocol for sending.  This is your last change to influence the payload.

#### Data Passed

| Key    | Type | Description             |
| ------ | ---- | ----------------------- |
| `mail` | Mail | The mail payload object |

### postMailSend

Announced after the mail payload has been sent via the mailer protocol of choice.&#x20;

#### Data Passed

| Key      | Type   | Description                                                                                                            |
| -------- | ------ | ---------------------------------------------------------------------------------------------------------------------- |
| `mail`   | Mail   | The mail payload object                                                                                                |
| `result` | struct | The result structure from the mailer protocol.  At most it will contain an `error` boolean key and a `messages` array. |

