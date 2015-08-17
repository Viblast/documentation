
## Drm Protection
Viblast Player supports `PlayReady`, `Widevine` or Multi-DRM (`PlayReady` + `Widevine`) protected DASH streams.

### Configuring PlayReady
The required `PlayReady` configuration is carried inside the DASH stream and there is no need to configure anything.

### Configuring Widevine
Just like `PlayReady`, `Widevine` also carries most of its configuration inside the DASH stream. Only the `Widevine Licensing Server` needs to be explicitly specified. To do so, you need to add the `viblast-widevine-licensing-server` property to the video tag and specify the licensing server as the value:

```html
<video 
	data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI="
	data-viblast-widevine-licensing-server="https://mywidevine.licenseserver.com/"
	src="http://myserver/drm-protected-stream.mpd"
	width="640">
</video>
```

### Supported Browsers
Viblast relies on the built-in support for drm in the browser.

`PlayReady` is supported on `IE11` and `Widevine` on `Chrome`.
