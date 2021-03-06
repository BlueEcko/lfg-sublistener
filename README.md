#lfg-sublistener
This is a [NodeCG](http://github.com/nodecg/nodecg) bundle.

Listens to one or many Twitch chat channels via IRC for "subscription" events.
Upon hearing one, it emits a NodeCG API message that other bundles can listen to and use.
Also displays recent subscriptions on the dashboard.

If [lfg-twitchapi](https://github.com/SupportClass/lfg-twitchapi) is present, sublistener will also poll the API every 60 seconds to fill in any gaps left by TwitchNotify.

## Installation
- Install to `nodecg/bundles/lfg-sublistener`
- Create `nodecg/cfg/lfg-sublistener.json` with a valid [twitch-irc configuration](https://github.com/Schmoopiie/twitch-irc/wiki#configuration).
- NOTE: There is a custom boolean parameter called "chatevents". This is used only for testing. When enabled, it lets the broadcaster use `!sendsub <name>` and `!sendsubforce <name>` to simulate subsription events. 

### Config Example
```json
{
  "twitch-irc": {
    "identity": {
      "username": "YOUR_USERNAME",
      "password": "oauth:YOUR_OAUTH"
    },
    "channels": ["ANY_CHANNEL"]
  },
  "chatevents": false
}
```

## Usage
### As a dashboard panel
If you simply want a list of recent subs on your dashboard, you are done.

### In other bundles' view pages and dashboard panels
If you would like to use this data in another bundle, add the following code to your view/panel:
```javascript
nodecg.listenFor('subscription', 'lfg-sublistener', callback);
```
... where 'callback' is the name of a function with the signature `function callback(data)`

### In other bundles' extensions
If you want to use subscription events in another bundle's extension,
add `lfg-sublistener` as a `bundleDependency` in your bundle's [`nodecg.json`](https://github.com/nodecg/nodecg/wiki/nodecg.json)

Then add the following code:
```javascript
var sublistener = nodecg.extensions['lfg-sublistener'];

sublistener.on('subscription', function subscription(data) {
    // do work
    // data.name = Twitch username of subscription
    // data.channel = What channel was subscribed to
    // data.resub = Boolean, whether or not this is a resub
    // data.months = If this is a resub, this will be the number of months they have been subscribed for
    // data.ts = Unix timestamp (in milliseconds)
});
```

### License
lfg-sublistener is provided under the MIT license, which is available to read in the [LICENSE][] file.
[license]: LICENSE
