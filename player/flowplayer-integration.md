
## Integrating Viblast Player with Flowplayer HTML5

Viblast Player leverages the existing HTML5 video tag and its API. This allows for easy integration with other HTML5 players. A popular decision is to use Viblast Player together with [Flowplayer](https://flowplayer.org/), combining Viblast playback with the beautiful Flowplayer user interface.

To [get started with Flowplayer](https://github.com/flowplayer/flowplayer#minimal-setup) you have to include `flowplayer.min.js` and add a video tag to your webpage. For example:

```html
<head>
	<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>
	<script type="text/javascript" src="flowplayer.min.js"></script>
	<link rel="stylesheet" type="text/css" href="flowplayer/minimalist.css">
	....
</head>
<body>
	...
	<div class="flowplayer">
      <video>
         <source type="video/mp4" src="my-video2.mp4">
      </video>
   </div>
<body>
```

To integrate Viblast Player, you need to

 * include `viblast.js` *after* `flowplayer.min.js` 
 * set `src=` to the url of you HLS/DASH stream and add `data-viblast-key`
 * add `type="video/mp4"` to the video tag
 * add `data-live="true"` to the `<div class="flowplayer">` element if the stream is live
 * add `preload="none"` to the `video` element in order to prevent Flowplayer from pulling content it's not supposed to do.
Like so:

```html
<head>
	...
	<script type="text/javascript" src="flowplayer.min.js"></script>
	<script src="path/to/viblast.js"></script>
	...
</head>
<body>
	...
	<div class="flowplayer" data-ratio="0.562" data-live="true">
	   <video data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI=" preload="none" controls src="//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8" type="video/mp4" poster="images/video-poster.png">
	   </video>
	</div>
</body>
```
Please, copy paste `data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI="` as is and don't modify it. This attributes activates Viblast Player for the specified video tag.

