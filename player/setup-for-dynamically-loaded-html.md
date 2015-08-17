
## Setup for Dynamically Loaded HTML
If your web page or application loads the video tag dynamically (AJAX, appendChild, etc.), it may not exist when the page loads. In such a scenario, you'll want to manually set up the player instead of relying on the `data-viblast-key` attribute. To do this, run the following JavaScript some time after the `viblast.js` JavaScript library has loaded and after the video tag has been loaded into the DOM.

```javascript
viblast("#target-video-tag").setup({
	key: 'N8FjNTQ3NDdhZqZhNGI5NWU5ZTI=',
	stream: '//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8',
	autoplay: true
});
```
The first argument in the `viblast` function is a jquery selector of your video tag.

The second argument is an options object.

* The `key` attribute must have the same value as the `data-viblast-key` attribute you would add to a statically loaded video tag.
* The `stream` attribute is the url of the HLS/DASH stream
* `autoplay` is self explanatory. If not specified, the value of the `autoplay` attribute of the video tag will be used instead.

`key` and `stream` are the only mandatory attributes.

Instead of using a selector, you can also pass a reference to the element itself.

```javascript
videojs(document.getElementById('target-video-tag'), /* as before */);
```

To stop and completely unload the Viblast Player and its entire state (allocated resources, opened HTTP connections and so on):

```javascript
viblast("#target-video-tag").stop();
```

And here is a tiny example to get you started:

```html
<video id="my-cool-video-tag" controls width="800"></video>
<button onclick="start_viblast();">Start</button>
<button onclick="stop_viblast();">Stop</button>
<script>
function start_viblast() {
	viblast('#my-cool-video-tag').setup({
		key: 'N8FjNTQ3NDdhZqZhNGI5NWU5ZTI=',
		stream: '//cdn3.viblast.com/streams/hls/airshow/playlist.m3u8',
		autoplay: true
	});
}
function stop_viblast() {
	viblast('#my-cool-video-tag').stop();
}
</script>
```
