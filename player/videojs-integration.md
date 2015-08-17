
## Integrating Viblast Player with Video.js

Please Note that the Viblast Player Integration with Video.js has been changed in Viblast Player v6.0. If you're using an older version then please <a href="{% url 'vb-player-dashboard:dashboard' %}">upgrade</a>.

Viblast Player leverages the existing HTML5 video tag and its API. This allows for easy integration with other HTML5 players. A popular decision is to use Viblast Player together with [Video.js](http://www.videojs.com/), combining Viblast playback with the beautiful Video.js user interface. Viblast Player also transparently integrates with the Flash Version of Video.js allowing for seamless fallback to flash on platforms that don't support HTML5 video and <a href="https://w3c.github.io/media-source/">MSE</a>.

First you need to include `video.js` and `viblast.js` in this order. The ORDER is important so be careful:

```html
<html>
	<head>
		<script src="//code.jquery.com/jquery-1.7.0.js"></script>
	
		<!-- videojs must be included first -->
		<script src="http://vjs.zencdn.net/4.12/video.js"></script>
		
		<!-- and only then viblast.js -->  
		<script async src="path/to/viblast.js"></script>
	</head>
</htm>
```

Then to [get started with Video.js](https://github.com/videojs/video.js/blob/stable/docs/guides/setup.md) you have to add a video tag to your webpage. For example:

```html
<video id="example_video_1" class="video-js vjs-default-skin"
  controls preload="auto" width="640" height="264"
  poster="http://video-js.zencoder.com/oceans-clip.png"
  data-setup='{"example_option":true}'>
 <source src="http://video-js.zencoder.com/oceans-clip.mp4" type='video/mp4' />
 <p class="vjs-no-js">To view this video please enable JavaScript, and consider upgrading to a web browser that <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a></p>
</video>
```

To integrate Viblast Player, you need to set `src=` to the url of you HLS/DASH stream, set `type` to `application/x-mpegURL` or `application/dash+xml`, and add `data-viblast-key`. Like so:

```html
<video id="example_video_1" class="video-js vjs-default-skin"
  controls preload="auto" width="640" height="264"
  poster="http://video-js.zencoder.com/oceans-clip.png"
  data-setup='{"example_option":true}'
  data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI=">
 <source src="//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8" type='application/x-mpegURL' />
 <p class="vjs-no-js">To view this video please enable JavaScript, and consider upgrading to a web browser that <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a></p>
</video>
```

### Forcing the use of Flash
If you want to use only the flash version of Video.js then you have to set `data-setup='{"techOrder": ["viblastflash", "html5"]}'`. Here is an example:

```html
<video id="example_video_2" class="video-js vjs-default-skin"
  controls preload="auto" width="640" height="264"
  poster="http://video-js.zencoder.com/oceans-clip.png"
  data-setup='{"techOrder": ["viblastflash", "html5"]}'
  data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI=">
 <source src="//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8" type='application/x-mpegURL' />
 <p class="vjs-no-js">To view this video please enable JavaScript, and consider upgrading to a web browser that <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a></p>
</video>
```
