# messenger-api-helper

This module exposes some functions that allow for sending of messages.
It is heavily based on the example app from Facebook Samples here: https://github.com/fbsamples/messenger-platform-samples/tree/master/node
A callback for receiving messages is specified during the initialization process; the process is detailed below.

## Pre-setup

Follow the Quick start guide to get the configuration information: https://developers.facebook.com/docs/messenger-platform/guides/quick-start  
**Note** You will not be able to set up the Webhook until the server is running, but for the server to be running, you need to have it configured with certain config values.
You will need to get all the config keys and start the server before setting up the webhook. Instructions as to what config things are needed are available here: https://github.com/fbsamples/messenger-platform-samples/tree/master/node  
**Warning** They say it takes 10 minutes, but it took me closer to 10 hours (had to set up SSL, including break times, and had MAJOR issues with authkeys).  

## Setup

Hopefully you have everything configured now.

Begin by requiring the module:
```js
var mah = require('messenger-api-helper');
```

Start the server, optionally specifying a callback:
```js
var callback = function(senderID, timeMessageSent, recipientID, message, isSpecial) {
    if (isSpecial) return; /* is an image, file, etc. */
    mah.sendNormalMessage(senderID, message); /* simple echo back */
};

/* start the server on the port */
mah.start(callback, 3008);
```

## Exposed functions
- sendAudioMessage: send an audio message. Provide it with the recipientID and the HTTP location of the audio file.
- sendVideoMessage: send a video message. Provide it with the recipientID and the HTTP location of the image file.
- sendFileMessage: send a generic file. Provide it with the recipientID and the HTTP location of the file.
- sendNormalMessage: send a generic text message. Provide it with the recipientID and the text.
- sendQuickReply - send a message with quick reply options. Provide it with the recipientID, the text message, and an array of possible replies. The reply text will be the payload. Sample JSON:
```
sendQuickReply(recipientID, "What is your favourite fruit?",
[{
  "content_type":"text",
  "title":"Oranges",
  "payload":"I like Oranges."
},
{
  "content_type":"text",
  "title":"Apples",
  "payload":"I like Apples."
},
{
  "content_type":"text",
  "title":"Other",
  "payload":"I like something else."
}]);
```
- sendReadReceipt - send a read receipt. Provide it with the recipientID.
- sendTypingStatus - send the typing status. Provide it with the recipientID and true = typing, false = not typing.
- callSendAPI - send raw JSON to the api. See code for details.

Readme of the original project below:

---

# Messenger Platform Sample -- node.js

This project is an example server for Messenger Platform built in Node.js. With this app, you can send it messages and it will echo them back to you. You can also see examples of the different types of Structured Messages. 

It contains the following functionality:

* Webhook (specifically for Messenger Platform events)
* Send API 
* Web Plugins
* Messenger Platform v1.1 features

Follow the [walk-through](https://developers.facebook.com/docs/messenger-platform/quickstart) to learn about this project in more detail.

## Setup

Set the values in `config/default.json` before running the sample. Descriptions of each parameter can be found in `app.js`. Alternatively, you can set the corresponding environment variables as defined in `app.js`.

Replace values for `APP_ID` and `PAGE_ID` in `public/index.html`.

## Run

You can start the server by running `npm start`. However, the webhook must be at a public URL that the Facebook servers can reach. Therefore, running the server locally on your machine will not work.

You can run this example on a cloud service provider like Heroku, Google Cloud Platform or AWS. Note that webhooks must have a valid SSL certificate, signed by a certificate authority. Read more about setting up SSL for a [Webhook](https://developers.facebook.com/docs/graph-api/webhooks#setup).

## Webhook

All webhook code is in `app.js`. It is routed to `/webhook`. This project handles callbacks for authentication, messages, delivery confirmation and postbacks. More details are available at the [reference docs](https://developers.facebook.com/docs/messenger-platform/webhook-reference).

## "Send to Messenger" and "Message Us" Plugin

An example of the "Send to Messenger" plugin and "Message Us" plugin are located at `index.html`. The "Send to Messenger" plugin can be used to trigger an authentication event. More details are available at the [reference docs](https://developers.facebook.com/docs/messenger-platform/plugin-reference).

## License

See the LICENSE file in the root directory of this source tree. Feel free to useand modify the code.