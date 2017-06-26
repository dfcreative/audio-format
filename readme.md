# audio-format [![Build Status](https://travis-ci.org/audiojs/audio-format.svg?branch=master)](https://travis-ci.org/audiojs/audio-format) [![unstable](https://img.shields.io/badge/stability-unstable-green.svg)](http://github.com/badges/stability-badges) [![Greenkeeper badge](https://badges.greenkeeper.io/audiojs/audio-format.svg)](https://greenkeeper.io/)

Audio formats are used to identify universally any audio data parameters, such as number of channels, sample rate, data type, data container and data layout.

## Usage

[![npm install audio-format](https://nodei.co/npm/audio-format.png?mini=true)](https://npmjs.org/package/audio-format/)

### obj = format.parse(string|obj)

Parse format properties from a string or data container. Returns only guaranteed properties and does not try to guess them.

```js
format.parse('interleaved uint8 le stereo 44100')
// {interleaved: true, type: 'uint8', endianness: 'le', channels: 2, sampleRate: 44100}

format.parse('stereo audiobuffer 96000')
// {channels: 2, type: 'audiobuffer', sampleRate: 96000, interleaved: false, endianness: 'le'}

format.parse(new AudioBuffer(null, {length: 10, numberOfChannels: 2}))
// {channels: 2, type: 'audiobuffer', sampleRate: 44100, endianness: 'le', interleaved: false}

format.parse(new Uint8ClampedArray([0, 255, 0, 255]))
// {type: 'uint8_clamped'}
```

#### `interleaved` property marker

| Value | Meaning |
|---|---|
| `'interleaved'` | `interleaved` is `true`, `channels` is `2` or more. |
| `'planar'` | `interleaved` is `false`, `channels` is `2` or more. |

#### `endianness` property marker

| Value | Meaning |
|---|---|
| `'le'` | `endianness` is `'le'` (little endian), `type` is not `'int8'` or `'uint8'` |
| `'be'` | `endianness` is `'be'` (big endian), `type` is not `'int8'` or `'uint8'` |

#### `channels` property marker

| Value | Meaning |
|---|---|
| `'mono'` | 1 channel |
| `'stereo'` | 2 channels |
| `'2.1'` | 3 channels |
| `'quad'` | 4 channels |
| `'5.1'` | 5 channels |
| `'*-channel'` | N channels |

#### `sampleRate` property marker

| Value | Meaning |
|---|---|
| `Number` | Any number, primarily [default sample-rates](https://github.com/audiojs/sample-rate) |

#### `type` property marker

| Value | Meaning |
|---|---|
| `'uint8'` | _Uint8Array_ |
| `'uint8_clamped'` | _Uint8ClampedArray_ |
| `'uint16'` | _Uint16Array_ |
| `'uint32'` | _Uint32Array_ |
| `'int8'` | _Int8Array_ |
| `'int16'` | _Int16Array_ |
| `'int32'` | _Int32Array_ |
| `'float32'` | _Float32Array_ |
| `'float64'` | _Float64Array_ |
| `'array'` | _Array_ |
| `'arraybuffer'` | _ArrayBuffer_ |
| `'buffer'` | _Buffer_ |
| `'audiobuffer'` | _AudioBuffer_ |
| `'ndarray'` | [_ndarray_](https://github.com/scijs/ndarray) |
| `'ndsamples'` | [_ndsamples_](https://github.com/livejs/ndsamples) |


### str = format.stringify(obj, defaults?)

Get string identifying the format object. Optional `defaults` object can indicate properties to skip if format value matches them.

```js
format.stringify({channels: 2, interleaved: false})
// 'stereo planar'

format.stringify(new AudioBuffer(null, {length: 10}))
// 'mono audiobuffer 44100'

format.stringify(
	{type: 'float32', endianness: 'le', interleaved: false, channels: 2},
	{endianness: 'le', type: 'float32'}
)
// 'stereo planar'
```

## See also

* [audio-convert](https://github.com/audiojs/pcm-convert) convert, remix or resample audio data
* [pcm-convert](https://github.com/audiojs/pcm-convert) converts low-level pcm data from one format to another
