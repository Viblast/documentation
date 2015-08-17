
## Integrating Viblast Player with JW Player

Viblast Player leverages the existing HTML5 video tag and its API. This allows for easy integration with other HTML5 players. A popular decision is to use Viblast Player together with [JWPlayer](http://www.jwplayer.com/), combining Viblast playback with the beautiful JWPlayer user interface.

* The first step is to add `Viblast Player` and `JW Player` to your page

```html
<html>
	<head>
		<script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
		<script src="http://jwpsrv.com/library/w4i+cCIEEeS0jiIACyaB8g.js"></script>
		
		<!-- the key cannot be embeded in the video tag so we specify it as a global environment -->
		<script>ViblastKey="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI="</script>
		<script src="path/to/viblast.js"></script>
		....
	</head>
</html>
```
* After that, the only thing you have to do is initialize JW Player, giving it your HLS or DASH stream as the file attribute:

```html
	<body>
		<div id="jwplayer-container">Loading the player...</div>
		<script type="text/javascript">
		    jwplayer("jwplayer-container").setup({
		        file: "//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8",
		        image: "http://viblast.com/static/viblast_player/images/hls-demo.png",
		        width: 640,
		        height: 360,
				type: 'mp4'
		    });
		</script>
	</body>
```

And here is the result:

<script src="http://jwpsrv.com/library/w4i+cCIEEeS0jiIACyaB8g.js"></script>
<script src="{% url 'vb-player:free-version' client_id='qy2fdwajo1' path='viblast.js' %}"></script>
<script>ViblastKey="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI="</script>

<div id="jwplayer-container">Loading the player...</div>
<script type="text/javascript">
    jwplayer("jwplayer-container").setup({
        file: "//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8",
        image: "http://viblast.com/static/viblast_player/images/hls-demo.png",
        width: 640,
        height: 360,
		type: 'mp4'
    });
</script>



### Passing configuration options to Viblast Player

Options can be passed to Viblast Player by providing them in the JW Player `setup` method. For example, to force the use of [HE-AAC](https://en.wikipedia.org/wiki/High-Efficiency_Advanced_Audio_Coding) for the audio and to specify a `widevine` licensing server:

```javascript
jwplayer("jwplayer-container").setup({
  ....
  viblast: {
  	heAac: true,
  	widevine: {
		"licensing-server": "http://example.com/licensing"
	}
  }
});
```
