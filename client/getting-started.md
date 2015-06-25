# Getting Started! (Draft)

## Overview

Essentially, Viblast PDN is a smart transport layer for video content streaming from a **server** (***CDN*** or an in-house alternative) to a **client**. Under the hood, it uses a decentralized approach for fetching the stream. The server is the primary source, but if there are other **clients** watching the same stream (also known as ***peers***), it will try fetching the stream from them and thus offload the server.

Viblast PDN supports the following **server** streaming protocols:

 * HTTP Live Streaming (HLS)
 * Dynamic Adaptive Streaming over HTTP (MPEG-DASH)

Once a chunk of the stream is obtained by Viblast PDN, it needs to be fed into a video player. This is done via a player plugin. The currently supported players are:

 * Web-based

   * Viblast Player
   * Flowplayer Flash
   * JWPlayer 6 Flash (commercial editions only)
 
  Support for the following can be easily added:

   * Any other HTML5 player supporting playback via Media Source Extensions (MSE)
   * Any other Adobe Flash-based player supporting playback via plugin (or just a NetStream class)

 * Mobile devices

   * iOS 8
   * Android 4.2+

## Integration with web-based players

When integrating with web-based players, Viblast PDN provides a JavaScript library that relies on WebRTC browser support for peer-to-peer communication. If WebRTC support is not present, Viblast PDN gracefully falls back to fetching the stream from the CDN only, utilizing the standard client-server communication model.

It is important to note that Viblast does **not** require any client-side plugins to operate.

### Server-side configuration

#### Viblast PDN installation

If you have downloaded Viblast as a standalone archive from our [portal](http://portal.viblast.com), you must unpack it somewhere on your website (i.e. `/js/viblast/`), preserving the directory structure. The archive contains free versions of Flowplayer, JWPlayer 6 and Viblast's HTML5 player for demo purposes. An unpacked archive has the following structure:

```text
.
`-- flow
|   `-- flowplayer.controls.swf
|   `-- flowplayer.swf
|   `-- ViblastFlowPlayer.swf
`-- jw6
|   `-- jwplayer6.swf
|   `-- ViblastJWPlayer6.swf
`-- flash-escape.js
`-- Viblast.asp.js
`-- Viblast.crypto.js
`-- Viblast.js
`-- Viblast.msp.js
```

#### Cross-origin resource sharing

Most commonly, the streaming server and the Viblast JavaScript files reside on different domains. Due to security concerns, modern web browsers refuse requesting resources via AJAX hosted on domains different from the origin domain of the request. To solve this issue, one must enable [Cross-origin resource sharing (CORS)](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) on the server side. Unfortunately, the configuration depends on the server type. Information on how to configure CORS for most common web servers is available [here](http://enable-cors.org/server.html). In addition here is how to configure

 * [Wowza Streaming Engine]()
 * [Nimble Streamer]()

### Client-side configuration

*In the examples that follow, we assume that:*

 * *Your web site's domain is `example.com/`*
 * *Viblast's files reside under `example.com/js/viblast/`*
 * *An HLS stream is being served on `stream.example.com/hls/playlist.m3u8`*

*If you are using Viblast through our cloud service, replace `example.com/js/viblast/` with `portal.viblast.com/static/resources/xxxxx/`.*

Viblast PDN's API consists of:

 * `Viblast.play(config)` - where *config* is a JSON object holding the Viblast PDN configuration. The return value is an Object representing the just-created Viblast instance. It is used for subsequent API calls. See [Viblast Configuration Options Reference]() for all available options.
 * `Viblast.stop(context)` - stops and removes a Viblast instance specified by *context*.

Viblast PDN configuration is logically split into two sections - player options and common options. Player options are passed to the underlying player without (or with minimal) modification. The common options affect how Viblast PDN works and specify general settings such as the stream's URL and the player container.

A simple example using the Viblast HTML5 player:

```html
...
<script type='text/javascript' src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script> 
<script type='text/javascript' src="http://example.com/js/viblast/viblast.js"></script>
...

...
<!-- the player will be embedded in this container -->
<div id="player-container" style="width: 848px; height: 640px;" />

<script type="text/javascript">
	Viblast.play({
		container: 'player-container',
		channel: {
			cdnStream: 'http://stream.example.com/hls/playlist.m3u8'
		}
	});
</script>
...

```

By default the player used is Viblast's HTML5 Player.

#### Using Flowplayer

When using Viblast PDN with Flowplayer, a Flowplayer configuration must be provided so that Viblast can initialize the player. Your existing configuration can be used with two minor changes:

 * Add our Flowplayer plugin to the Flowplayer configuration
 * Replace the stream source with the one provided by the Viblast plugin.

The plugin is a standard Flowplayer plugin - to add it, append a new entry in the `'plugins'` section of your configuration:

```javascript
{
	...
	plugins: {
		...
		viblast: {
			url: 'http://example.com/js/viblast/flow/ViblastFlowPlayer.swf',
		}
		
	}
	...
}

```

Replacing the stream source can be done in two ways depending on your configuration:

 * If you use [clip]() properties you need to change the URL to point to `'http://viblast.com'` and add a `'provider'` property with value `'viblast'`

   For example, if your current configuration is:

	```javascript
	{
		...
		clip: {
			url: 'rtmp://domain.com/some-stream',
			autoplay: true
		}
		...
	}
	```

	it needs to be changed to:

	```javascript
	{
		...
		clip: {
			url: 'http://viblast.com',
			provider: 'viblast'
		}
		...
	}
		
	```

 * If you use the [playlist object]() you need to either replace your existing entry or append a new one with `'url'` set to `'http://viblast.com'` and `'provider'` set to `'viblast'`.

	For example, if your current configuration is:

	```javascript
	{
		...
		playlist: [
			{
				url: 'http://domain.com/preroll-ad.flv',
			},
			{
				url: 'rtmp://domain.com/live-stream',
			}
			...
		]
		...
	}
	```

	it needs to be changed to:
	
	```javascript
	{
		playlist: [
			{
				url: 'http://domain.com/preroll-ad.flv',
			},
			{
				url: 'http://viblast.com',
				provider: 'viblast'
			}
		]
	}
	```

To pass the configuration to Viblast PDN, an Object with two properties - `'config'` and `'params'` must be created. The `'config'` property contains the Flowplayer configuration and the `'params'` property contains the Flash embedding parameters (if any). This Object must then be assigned to the `'flowplayer'` property of the Viblast configuration.

Finally, to instruct Viblast to use Flowplayer the `'player'` property  must be set to `'flowplayer'`.

A simple example of Viblast using Flowplayer:


```html
...
<script type="text/javascript">
	Viblast.play({
		container: 'player-container',
		channel: {
			cdnStream: 'http://stream.example.com/hls/playlist.m3u8'
		},
		player: 'flowplayer', # use Flowplayer as the player
		flowplayer: {
			config: { # the modified Flowplayer configuration
				{
					plugins: {
						viblast: {
							url: 'http://example.com/js/viblast/flow/ViblastFlowPlayer.swf'
						}
					},
					clip: {
						url: 'http://viblast.com',
						provider: 'viblast'
					}
				}
			},
			params: { # flash embedding parameters
				wmode: 'direct'
			}
		}
	});
</script>
...
```

#### Using JWPlayer 6

When using Viblast PDN with JWPlayer 6, a JWPlayer configuration must be provided so that Viblast can initialize the player. Your existing configuration can be used with three changes:

 * Replace the stream source with the one provided by the Viblast plugin
 * Add our JWPlayer 6 plugin to the JWPlayer configuration
 * Force Flash as the primary JWPlayer variant to be used. Currently, we do not support the HTML5 variant of JWPlayer.

Replacing the stream can be done in two ways, depending on your configuration:

 * If you use the `'file'` property directly to specify your stream without a [playlist configuration block](), you need to migrate to using such a block. To do this, you need to create a property `'playlist'` with an array containing a JSON object describing the stream. In the case of migrating, you will need to just set the `'file'` property of the object to use the correct stream. When using Viblast, `'file'` must point to `'http://viblast.com'`.

	For example, if your current configuration is:

	```javascript
	{
		...
		file: 'rtmp://example.com/live/flv:myFolder/myLiveStream'
		width: 640,
		height: 360,
		...
	}
	```

	It needs to be changed to:

	```javascript
	{
		...
		playlist: [
			{
				file: 'http://viblast.com'
			}
		],
		width: 640,
		height: 360,
		...
	}
	```

 * If you use a `'playlist'` configuration block, you need to set the `'file'` property of the stream source to `'http://viblast.com'`. For example:
 
	```javascript
	{
		...
		playlist: [
			{
				file: 'http://viblast.com'
			}
		],
		width: 640,
		height: 360,
		...
	}
	```

To load the Viblast plugin, two additional properties must be added to the Viblast playlist entry - `'provider'` and `'type'`. The `'provider'` property contains the URL of the Viblast plugin and the `'type'` property must be set to `'hls'`. For example:

```javascript
{
	...
	playlist: [
		{
			file: 'http://viblast.com',
			type: 'hls',
			provider: 'http://example.com/js/viblast/flow/ViblastJWPlayer6.swf'
		}
	],
	width: 640,
	height: 360,
	...
}
```

To force JWPlayer to use its Flash player by default, you must set the `'primary'` property to `'flash'`. For example:

```javascript
{
	...
	width: 640,
	height: 360,
	primary: 'flash',
	...
}
```

Once the configuration is modified, it must be passed to Viblast via the `'jwplayer6'` configuration property.

The last step is to instruct Viblast that we want to use JWPlayer. This is done via setting the `'player'` property to `'jwplayer6'`.

A simple example showing Viblast working with JWPlayer:

```html
...
<script type="text/javascript">
	Viblast.play({
		container: 'player-container',
		channel: {
			cdnStream: 'http://stream.example.com/hls/playlist.m3u8'
		},
		player: 'jwplayer6', # use JWPlayer 6 as the player
		jwplayer6: { # the modified JWPlayer 6 configuration
			playlist: [
				{
					file: 'http://viblast.com',
					type: 'hls',
					provider: 'http://example.com/js/viblast/flow/ViblastJWPlayer6.swf'
				}
			],
			width: 640,
			height: 360,
			primary: 'flash',
		}
	});
</script>
...
```
