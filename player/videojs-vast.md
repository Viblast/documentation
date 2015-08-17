
## Viblast Player Vast Plugin for Video.js

When would you need to use the [Video.js Viblast VAST plugin](https://github.com/Viblast/videojs-vast-plugin)?
If you're using Viblast Player together with [Video.js](https://github.com/videojs/video.js) and find yourself in need of a [VAST](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vast) solution.

* Add  `viblast`, `videojs` and `videojs.viblast-vast` to your page

```html
<html>
	<head>
		<script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
		
		<!-- videojs -->
		<link href="http://vjs.zencdn.net/4.12/video-js.css" rel="stylesheet">
		<script src="http://vjs.zencdn.net/4.12/video.js"></script>
		
		<!-- videojs.viblast-vast -->
		<link href="https://cdn.rawgit.com/Viblast/videojs-vast-plugin/7fa2183cc8c1ec3a23f94a6034748d7335e052fa/dist/videojs.viblast-vast.css" rel="stylesheet" type="text/css">
		<script src="https://cdn.rawgit.com/Viblast/videojs-vast-plugin/7fa2183cc8c1ec3a23f94a6034748d7335e052fa/dist/videojs.viblast-vast.js"></script>
		
		<!-- viblast -->
		<script async src="path/to/viblast.js"></script>
		...
	</head>
	...
</html>
```

* Place the video tag that's going to be used for `video.js` on your page

```html
	...
	<body>
		<video id="vid1" class="video-js vjs-default-skin" controls
	       poster="http://video-js.zencoder.com/oceans-clip.png"
	       data-setup='{}'
	       width='640'
	       height='400'
	       data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI="
	     >
	     <source src="//cdn3.viblast.com/streams/dash/vod-bunny/SNE_DASH_CASE3B_SD_REVISED.mpd" type='application/x-mpegURL'>
	   </video>
	</body>
```

* Initialize the Video.js Viblast VAST plugin

```html
	...
	<body>
		<video ....> <!-- as before -->
		<script>
	    var vid1 = videojs('vid1');
	    vid1.vast({
	      url: 'http://videoads.theonion.com/vast/270.xml'
	    });	
	  </script>
	</body>
```
