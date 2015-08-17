
## Accessing Viblast Player ABR Capabilities

ABR (adaptive bitrate) streaming is supported for both HLS and DASH. For ABR streaming, a particular channel (video and/or audio) is encoded using different resolutions and bitrates.
The different encodings of a channel are called `variants` in HLS and `representations` in DASH. Viblast Player simply calls them `qualities`.
The Player monitors the network and automatically chooses the best possible quality under the current network conditions.
This behavior can be modified to suit your application needs. Viblast Player provides ways of obtaining the list of available qualities, manually specifying a quality
and obtaining performance metrics about the network like the current CDN bandwidth. 

### Getting the list of available qualities

Once Viblast Player is initialized for a given video element, it adds a property to the video element called `viblast`.
Through this property, additional functionality not supported by the video element itself is exposed to the developer.


After the metadata for a stream is parsed and the list of available qualities is built, Viblast Player dispatches the `updatedmetadata` event on the `viblast` object.

```js
document.getElementById('myvideo').viblast.addEventListener('updatedmetadata', function(event) {
   // event.target is a shortcut for document.getElementById('myvideo').viblast
   console.log('available qualities', event.target.video.qualities);
});
```

Before the metadata is loaded, the `videoElement.viblast.video` is `undefined`.
After the metadata has been loaded, the list of qualities can be accessed using `videoElement.viblast.video.qualities`.

To obtain the list of available audio qualities just replace `video` with `audio` in the above expressions.

### Getting the current audio/video quality
The current quality is exposed through the `videoElement.viblast.video.quality` property.
The `updatedmetadata` event must have already fired for this property to be populated.

```js
// this code should be run after the metadata has been loaded
var video = document.getElementById('myvideo');
console.log('current video quality', video.viblast.video.quality);
console.log('current audio quality', video.viblast.audio.quality);
```

### Working with the the list of available video qualities

Each item in the list of qualities is a JS object that has the following properties:

 * id - a string that identifies this quality
 * bandwidth - an optional number representing the bitrate of the channel in `kbps`
 * width - an optional number representing the horizontal resolution of a video
 * height - an optional number representing the vertical resolution of a video
 
The list of qualities is sorted by its `bandwidth` in ascending order.
 
The current quality (exposed through `videoElement.viblast.video.quality`) has the same structure. 
 
### Manually changing the quality

To change the quality simply assign to `videoElement.viblast.video.quality` the desired new quality. For example, to choose the highest possible video quality:

```js
var video = document.getElementById('myvideo');
video.viblast.quality = video.viblast.qualities[video.viblast.qualities.length-1];
```

Once a new quality is chosen, Viblast Player will try make the transition to it in the smoothes possible way.
This means that the buffered content from the previous quality will be played until its end and only then the new quality will be fed into the player.
This achieves a smooth video/audio transition. But there is a delay before the new quality becomes "visible" for the viewer.

Once a new quality is chosen `abr streaming` is automatically switched off. If you want to engage it again:

```js
document.getElementById('myvideo').viblast.abr = true;
```

### Receiving notification for quality change
When the current quality is changed (either automatically or manually) the `videoqualitychange` event is dispatched on the `videoElement.viblast` object.

```js
document.getElementById('myvideo').viblast.addEventListener('videoqualitychange', function(event) {
  console.log('Quality Changed. Previous Quality=', event.previousQuality, 'Current Quality=', event.target.video.quality);
});
```

### Don't use videoElement.audioTracks or videoElement.videoTracks
The video element [videoTracks](http://www.w3.org/html/wg/drafts/html/master/semantics.html#dom-media-videotracks) and [audioTracks](http://www.w3.org/html/wg/drafts/html/master/semantics.html#dom-media-audiotracks)
are currently [supported](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement) only by Firefox, only behind a flag and only for ogg.
In addition to their poor support, their semantic is not made clear enough by the specification.
For these reasons we recommend that you use the Viblast Player API when dealing with ABR streams instead of relying on the videoTracks/audioTracks API.  

### Getting information about the network performance
The current bandwidth to the CDN can be acquired using the `cdnBandwidth` property:

```js
window.setTimeout(function() {
   console.log('current bandwidth', document.getElementById('myvideo').viblast.cdnBandwidth, 'kbps');
}, 5000);
```

### Disabling ABR

This can be achieved by setting the `data-viblast-abr` attribute of the video element to `false`. The default value is `true` meaning that abr streaming is enabled.

```html
	<video data-viblast-key="sadQdxx" data-viblast-abr="false" src="stream.mpd" /> 
```

