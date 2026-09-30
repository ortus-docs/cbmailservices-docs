---
description: Write every email to an HTML file on disk and browse them in the development mail viewer.
icon: file-code
---

# File

Writes each email to a timestamped HTML file instead of sending it. It is the perfect development mailer because nothing leaves your machine, and it powers the [Development Mail Viewer](../essentials/development-mail-viewer.md).

## Configuration

```javascript
mailers : {
    "files" : {
        class      : "File",
        properties : {
            filePath   : "/logs/mail",
            autoExpand : true
        }
    }
}
```

| Property | Required | Default | Description |
| -------- | -------- | ------- | ----------- |
| `filePath` | Yes | | The directory where mail files are stored. It is created if it does not exist. |
| `autoExpand` | No | `true` | Runs `expandPath()` on `filePath`. Set to `false` when you pass an absolute path. |

A missing `filePath` throws `FileProtocol.PropertyNotFound`.

## Output

Files are named `mail.{MM-dd-yyyy}.{HH-mm-ss-L}.html`. Each one contains:

1. A hidden metadata element with the `from`, `to`, `subject` and `sent` values, used by the viewer.
2. A dump of the mail attributes, mail params and mail parts.
3. The mail body. `text` mail is wrapped in `<pre>` and encoded, `html` mail is written as is.

## Development Only Mailer

A common pattern is to use the File mailer in development and your real provider everywhere else:

```javascript
// config/ColdBox.cfc
function configure(){
    moduleSettings = {
        cbmailservices : {
            defaultProtocol : "postmark",
            mailers         : { "postmark" : { class : "Postmark", properties : { apiKey : getSystemSetting( "POSTMARK_KEY" ) } } }
        }
    };
}

function development(){
    moduleSettings.cbmailservices.defaultProtocol = "files";
    moduleSettings.cbmailservices.mailers[ "files" ] = {
        class      : "File",
        properties : { filePath : "/logs/mail" }
    };
}
```
