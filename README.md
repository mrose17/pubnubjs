# PubNub JS client [![Build Status](https://travis-ci.org/tralamazza/pubnubjs.svg?branch=master)](https://travis-ci.org/tralamazza/pubnubjs) [![NPM version](https://badge.fury.io/js/pubnubjs.svg)](http://badge.fury.io/js/pubnubjs) [![Dependency Status](https://gemnasium.com/tralamazza/pubnubjs.svg)](https://gemnasium.com/tralamazza/pubnubjs)

	npm install pubnubjs

## Usage

Publishes every 100ms:

```js
var PubNubJS = require('pubnubjs');

var client = PubNubJS({
	// port: 443,
	// host: 'pubsub.pubnub.com',
	// insecure: false,
	// pool_max: 50,
	// pool_idletimeout: 20000,
	// secret_key: '', /* required for .grant() */
	subscribe_key: 'sub-c- ...', /* required */
	publish_key: 'pub-c- ...' /* required */
});
setInterval(function() {
	client.publish('my_little_channel', { ts: Date.now() }, {
		// cipher_key: 'my encryption key',
		// params: { auth: 'my auth key' }
	}, function(err, data) {
		console.log('publish', err, data);
	});
}, 100);
```

Subscribes for 10s:

```js
client.subscribe('my_little_channel', {
	// timetoken: '0',
	// cipher_key: 'my encryption key',
	// params: { auth: 'my auth key' }
}, function(err, stream, unsubscribe) {
	setTimeout(unsubscribe, 10000);
	stream.on('data', console.log);
});
```

Grant read & write access to a channel:

```js
client.grant({
	// ttl: 1440,
	w: 1, r: 1,
	channel: 'my_little_channel_with_auth',
	auth: '12345'
}, function(err, data) {
	console.log('grant', err, data);
});
```

## Important

* We keep a socket pool with a default idle timeout of 20s, see `options.pool_idletimeout`
* You can only publish objects or JSON strings.
* `.subscribe()` returns an object stream.
