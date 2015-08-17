
## Viblast Player VAST Support with JW Player

Viblast Player does not interfere with the JW Player advertising capabilities. The two can be used together without the need for any additional plugins or configuration. Here is a typical scenario. 

* Add  `viblast` and `JW Player` to your page

```html
<html>
	<head>
		<!-- viblast -->
		<script src="//code.jquery.com/jquery-1.11.3.min.js"></script>

        <!-- the key cannot be embeded in the video tag so we specify it as a global environment -->
		<script>ViblastKey="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI="</script>
		<script async src="path/to/viblast.js"></script>
		
		<!-- jwplayer -->
		<script src="http://jwpsrv.com/library/5V3tOP97EeK2SxIxOUCPzg.js"></script>
		...
	</head>
	...
</html>
```

* Create a container element for the JW Player and initialize it

```html
	...
	<body>
		<div id="my-player">Loading the player...</div>
		<script type="text/javascript">
		    jwplayer("my-player").setup({
		        file: "//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8",
		        image: "http://viblast.com/static/viblast_player/images/hls-demo.png",
		        width: 640,
		        height: 360,
				type: 'mp4',
				autostart: true,
				advertising: {
			      client: "vast",
			      tag: "http://videoads.theonion.com/vast/270.xml",
			      skipoffset: 5
			      
			    }
		    });
		</script>
	</body>
```

Once you have the both libraries in your page, you should be able to play HLS & MPEG-DASH via Viblast Player using the interface and advantced functionality of your JW Player , such as VAST support.
