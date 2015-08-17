
## Viblast Player Release History

### Viblast Player 5.97
 * Fixed video playback under Firefox ver >= 42
 * Fixed autoplay under Firefox
 * Fixed JW Player & Flowplayer integration under Firefox
 * Fixed player freezes when seeking backwards in encrypted HLS streams
 * License is now checked over HTTPS, fixing bogus error messages when a page is loaded over HTTPS
 * Removed some misleading and ill-formated warning messages
 * Fixed downloading the same chunks twice when using the Viblast Player Dynamic API and the target video tag has autoplay enabled
 * Buffered content clean-up after pausing for a long time. This guards against playback getting behind if paused multiple times. Timeout can be specified using long-pause-timeout.
 * viblast.msp.js renamed viblast.remux.js
 
### Viblast Player 5.96
* Improved HLS encryption support (Support for explicit initialization vectors)
* Fixed regression that prevented playback from continuing after pausing video for longer periods
* Fixed a multitude of issues when using Viblast Player with Microsoft Azure
* Support for explicitly turning Viblast Player on/off using data-viblast="true"

### Viblast Player 5.94
 * VAST support for video.js and JW Player users
 * Enhanced HLS VOD support - catch-up TV handling
 * Bug fixes

### Viblast Player 5.93
 * JwPlayer integration
 * Support for video elements that get added dynamically to the DOM tree

### Viblast Player 5.92
 * Various improvements of HLS VOD support, including seek functionality
 * Bug fixes

### Viblast Player 5.91
 * Flowplayer and Video.js integration
 * Optimizations for very long VOD playlists

### Viblast Player 5.89
 * DASH-related bug fixes
 * Support for 4K streams
 * Fixed signaling and detection of end stream

### Viblast Player 5.88
 * Major refactoring of Viblast Player and especially its DASH support.
 * Smarter downloading of audio segments
 * Fixed some minor bugs

### Viblast Player 5.86
 * Multi-DRM support - both Widevine and PlayReady are supported

### Viblast Player 5.85
 * Support for DASH video-on-demand (VOD) streams
 * Support for seek in DASH VOD streams

### Viblast Player 5.84
 * Support for DASH dynamic streams

### Viblast Player 5.83
 * Fixed detection of screen size on HTML5

### Viblast Player 5.80
 * Fixed handling of missing chunks

### Viblast Player 5.79
 * HTTP Progressive Streaming for native players
 * Publicly available Android version
 * Fixed secure web sockets
 * Public API that checks for WebRTC support on the current platform

### Viblast Player 5.78
 * Reworked Android API
 * Packaging for Android removing the need for clients to do any JNI themselves
 
