color-temperature
=================

![Color Spectrum](http://neilbartlett.github.io/color-temperature/images/color-temperature-spectrum.png)

Converts a color temperature in Kelvin to an RGB color space.

More details on color temperature and the algorithm can be found  [here](http://zombieprototypes.com).
## Installation

`$ npm install --save color-temperture`

## Usage

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
enc.end(pixels);
```


## API

The API provides two methods. Both convert from Kelvin to RGB. The first method provides a more accurate conversion. The performance of the two methods is approximately the same. The second method is provided for comparison purposes. The first method is preferred.

NOTE The conversions use approximations and are suitable for photo-mainpulation and other non-critical uses. They are not suitable for medical or other high accuracy use cases.

Accuracy is best between 1000K and 40000K.


```js
require('color-temperature').colorTemperature2rgb(kelvin);
```
This is a refitting of the original data and provides a slightly more accurate conversion.

```js
require('color-temperature').colorTemperature2rgbOriginalVersion(kelvin);
```
Ths is version is a JavaScript port of code from Tanner Helland.

## Examples

There are examples in the examples directory.

## License

The code is released under an MIT license.

## Release History

* 0.1.0 Initial release

[![NPM](https://nodei.co/npm/color-temperature.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/color-temperature/)
