
## Getting started with Viblast Player
This guide assumes that you have already purchased Viblast Player or downloaded the free version.

1. If you want to self-host Viblast Player, download `viblast-player.zip` from the e-mail we sent you after your request and extract it in a directory suitable for your project. Alternatively, use the library directly from our servers using the link provided in the email.
2. Include `jQuery` and `viblast.js` in you page.

	```html
	<head>
		...
		<script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
		<script async src="path/to/viblast.js"></script>
	</head>
	```
	`jquery-1.7.0` is the minimal supported version.
3. Put a video tag on you page with `src` pointing to your desired HLS or DASH stream

	```html
	<body>
	...
	<video src="//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8" data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI=" controls width="640"></video>
	...
	</body>
	```
	Please copy paste `data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI="` as is and don't modify it. This attributes activates viblast for the specified video tag.
4. Now your HTML5 video player can handle HLS & MPEG-DASH streams. [Tell us]({% url 'vb-player:index' %}#contact) how it went.

	You can continue using the video tag as usual.
	For example, if you want to pause it:
	
	```javascript
	var videoElement = document.getElementById('my-video-tag');
	videoElement.pause();
	```
	
	If you want autoplay then you can specify it as an attribute of the video element
	
	```html
	<video data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI=" autoplay src="//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8" width="640"></video>
	```
	
	and so on.
	
#### HE-AAC support
If the audio stream is coded using `HE-AAC` then you should signal this explicitly to Viblast Player by adding `data-viblast-he-aac="true"` to the video tag.

```html
<video src="http://myserver.com/playlist.m3u8" data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI=" data-viblast-he-aac="true"></video>
```

