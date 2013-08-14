# AudioContext

Just code for one API by enjoying this polyfill for the **[Web Audio API](http://caniuse.com/audio-api)** at [W3](https://dvcs.w3.org/hg/audio/raw-file/tip/webaudio/specification.html), following the upgrade path outlined at [MDN](https://developer.mozilla.org/en-US/docs/Web_Audio_API/Porting_webkitAudioContext_code_to_standards_based_AudioContext).

## Why this is useful

First, as the **Web Audio API** evolved, many method names were changed from what we find in older Chrome and Safari browsers (e.g. **buffer.start()** was **bufferNode.noteOn()**). Second, older browsers retained a vendor prefixed **Audio Context** method (e.g. **window.AudioContext** was **window.webkitAudioContext**).

## Which browsers this will effect

Including this polyfill will improve your experience coding for Chrome 10-30, Firefox v23-25, Opera 15-16, Safari 6-7, iOS Safari 6-7, and potentially later versions of these browsers as well.

## Examples of the Audio API

Let&rsquo;s load a sound and autoplay it.

```javascript
var
// create the audio context
context = new AudioContext(),
// create the http request
request = new XMLHttpRequest();

// request the MP3
request.open('GET', 'sound.mp3');

// request as an array buffer
request.responseType = 'arraybuffer';

// when the request loads
request.addEventListener('load', function () {
	// decode the array buffer
	context.decodeAudioData(request.response, function (buffer) {
		var
		// create the audio source
		source = context.createBufferSource(),
		// create the audio volume
		volume = context.createGainNode();

		// set the buffer to the audio source
		source.buffer = buffer;

		// set the volume to half
		volume.gain.value = 0.5;

		// connect the audio source to the audio volume
		source.connect(volume);

		// connect the audio volume to the output
		volume.connect(context.destination);

		// play the audio source
		source.start(0);
	});
});

// begin the request
request.send();
```