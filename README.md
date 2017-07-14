# gifencoder

Streaming server-side animated (and non-animated) gif generation for node.js

## Installation

This module is installed via npm:

``` bash
$ npm install gifencoder
```

## Streaming API - Duplex Piping with Writes

You can also stream writes of pixel data (or canvas contexts) to the encoder:

``` js
var GIFEncoder = require('gifencoder');
var encoder = new GIFEncoder(854, 480);
var pngFileStream = require('png-file-stream');
var fs = require('fs');

pngFileStream('test/**/frame?.png')
  .pipe(encoder.createWriteStream({ repeat: -1, delay: 500, quality: 10 }))
  .pipe(fs.createWriteStream('myanimated.gif'));
```

NB: The chunks that get emitted by your read stream must either by a 1-dimensional bitmap of RGBA
data (either an array or Buffer), or a canvas 2D `context`.

## Example: Streaming API - Reads

You can also use a streaming API to receive data:

``` js
var GIFEncoder = require('gifencoder');
var Canvas = require('canvas');
var fs = require('fs');

var encoder = new GIFEncoder(320, 240);
// stream the results as they are available into myanimated.gif
encoder.createReadStream().pipe(fs.createWriteStream('myanimated.gif'));

encoder.start();
encoder.setRepeat(0);   // 0 for repeat, -1 for no-repeat
encoder.setDelay(500);  // frame delay in ms
encoder.setQuality(10); // image quality. 10 is default.

// use node-canvas
var canvas = new Canvas(320, 240);
var ctx = canvas.getContext('2d');

// red rectangle
ctx.fillStyle = '#ff0000';
ctx.fillRect(0, 0, 320, 240);
encoder.addFrame(ctx);

// green rectangle
ctx.fillStyle = '#00ff00';
ctx.fillRect(0, 0, 320, 240);
encoder.addFrame(ctx);

// blue rectangle
ctx.fillStyle = '#0000ff';
ctx.fillRect(0, 0, 320, 240);
encoder.addFrame(ctx);

encoder.finish();
```
The above code will generate the following animated GIF:

![Animated GIF](https://raw.github.com/eugeneware/gifencoder/master/examples/myanimated.gif)

## Contributing

gifencoder is an **OPEN Open Source Project**. This means that:

> Individuals making significant and valuable contributions are given commit-access to the project to contribute as they see fit. This project is more like an open wiki than a standard guarded open source project.

See the [CONTRIBUTING.md](https://github.com/eugeneware/gifencoder/blob/master/CONTRIBUTING.md) file for more details.

### Contributors

gifencoder is only possible due to the excellent work of the following contributors:

<table><tbody>
<tr><th align="left">Kevin Weiner</th><td><a href="mailto:kweiner@fmsware.com">kweiner@fmsware.com</a></td></tr>
<tr><th align="left">Thibault Imbert</th><td><a href="http://www.bytearray.org/">http://www.bytearray.org/</a></td></tr>
<tr><th align="left">Eugene Ware</th><td><a href="https://github.com/eugeneware">GitHub/eugeneware</a></td></tr>
<tr><th align="left">Raine Virta</th><td><a href="https://github.com/raine">GitHub/raine</a></td></tr>
<tr><th align="left">Paul Ochoa</th><td><a href="https://github.com/rochoa">GitHub/rochoa</a></td></tr>
<tr><th align="left">Heikki Pora</th><td><a href="https://github.com/heikkipora">GitHub/heikkipora</a></td></tr>
</tbody></table>
