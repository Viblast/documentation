
## Troubleshooting common problems

If video playback doesn't start after everything seems to be set up, then you can do the following things:

* Make sure that you are using a [supported browser]({% url 'vb-player:index' %}#platforms).

* See if the video tag has the `data-viblast-key="..."` attribute set.

* Open the development console of your browser and look for any error messages.
	To open the console in Chrome press `Control + Shift + J` (Windows/Linux) or `Command + Option + J` (Mac).
	To open the console in IE11 press `F12`.
	
* Add `data-viblast-log="warning"` to the video tag attributes. This will activate logging of warning messages. Open the development console as described above and look for any warning messages that may point you in the direction of the problem.

* If the video stream originates from a different domain than the website where you are trying to play the video then please make sure that [CORS  is enabled](http://enable-cors.org/server.html). The error that the browser logs when CORS is not enabled usually reads:
> No 'Access-Control-Allow-Origin' header is present on the requested resource.

* If `HLS` is used try setting `data-viblast-he-aac="true"` as an attribute to the video tag and see if that helps. For examle:

```html
<video src="http://myserver.com/playlist.m3u8" data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI=" data-viblast-he-aac="true"></video>
``` 

* If you are unsure whether Viblast Player is being initialized for a particular video element, try adding `data-viblast="true"` to the video element:

```html
<video src="http://myserver.com/stream?format=hls" data-viblast="true" data-viblast-key="N8FjNTQ3NDdhZqZhNGI5NWU5ZTI="></video>
``` 

If normal streams are played just fine but `DRM` protected ones don't start then:

* Make sure that you've specified `viblast-widevine-licensing-server`. More info can be found in our documentation on [Viblast Player and DRM]({% url 'vb-player:doc' article='drm-protection' %})

* If the `widevine licensing server` is not specified or miss-configure then Viblast Player will log an error that says something along the lines of
> viblast-config.widevine.licensing-server is mandatory

* Check if the licensing server is operating properly

* Make sure that Viblast Player is contacting the proper licensing server by opening the development console (as specified above), clicking on the `Network` tab and searching for the url of the licensing server.

If none of these work, feel free to [contact us]({% url 'vb-player:index' %}#contact). We will be happy to assist. It would also help if you could attach some <a href="{% url 'vb-player:doc' article='debug-information' %}">debug information</a>.

