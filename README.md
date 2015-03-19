![Color Spectrum](http://neilbartlett.github.io/color-temperature/images/color-temperature-spectrum.png)

Converts a color temperature in Kelvin to an RGB color space.

More details on color temperature and the algorithm can be found at [here](http://zombieprototypes.com).

# Usage

Install color-temparture as a local module:

`$ npm install --save color-temperture`


Then include `color-temperature`:

```js
var ct = require('color-temperature');

//convert kelvin to RGB
// eg take convert typical candlelight (1850K )
var rgb = ct.colorTemperature2rgb(1850);

var red = rgb.red;
var green = rgb.green;
var blue = rgb.blue;
```

Typically this is used with image processing. The following uses streams to generate a PNG similar to that at the top of this README file.

```js
var fs = require('fs');
var png_encoder = require('png-stream');
var ct = require('color-temperature');
var width = 500;
var height = 100;
var kelvinStart = 10;
var kelvinEnd = 40000;

var pixels = new Buffer(width * height * 3);
for (var w = 0; w < width; w += 1) {
  for (var h = 0; h < height; h += 1) {
      var i = ((h*width)+w)*3;

      kelvin = ((kelvinEnd-kelvinStart)/width)* w + kelvinStart;
      var rgb = ct.colorTemperature2rgb(kelvin);

      pixels[i] = rgb.red;
      pixels[i + 1] = rgb.green;
      pixels[i + 2] = rgb.blue;
  }
}

var enc = new png_encoder.Encoder(width, height);
enc.pipe(fs.createWriteStream('color-temperature-'+kelvinStart+'-'+kelvinEnd+'.png'));
enc.end(pixels);```


# API

API of color-temperature provides two methods

```js
require('color-temperature').colorTemperature2rgb(kelvin);
```

```js
require('color-temperature').colorTemperature2rgbOriginalVersion(kelvin);
```

These both provide the same functionality to convert from Kelvin to RGB. The
colorTemperature2rgbOriginalVersion method is a JavaScript port of code from Tanner Helland. The colorTemperature2rgb method is
refitting of the original data and provides a slightly more accurate version of
code.

# License
