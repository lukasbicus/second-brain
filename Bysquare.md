---
title: What do I like on bysquare project 
summary: Project bysquare is an example of working with bytes, providing cli and more.
slug: bysquare
project-tags: 
  - bysquare
  - self education
thematic-tags:
  - bysquare
  - pay by square
  - qr code
  - cli example
  - binary
  - writing command line utils
---

# What do I like on bysquare project

*Project bysquare is an example of working with bytes, providing cli and more.*

---


## Project bysquare[^1]
While thinking, how can I contribute to community and improve my coding skills, I found project bysquare[^1]. At glance, I realized, this is a high quality project and I can learn a lot just by contributing to it. I decided to implement some features here and learn by the way.

## Bytes
What is new for me is working on the project on level of bytes.
Data in project are represented in binary format (f.e.: `0b0000_0001`) and in hexadecimal format (f.e.: `0x01`).
With data in binary format, there are bite operations like
- `bitwise AND` (`&`)
- `bitwise OR` (`|`)
- `right shift` (`>>`)
- `left shift` (`<<`)

It uses some libraries, that work with binary data:
- `lzma1` - implements compression / decompression of Lempel-Ziv-Markov (LZMA) chain compression algorithm.
- `rfc4648` - implements encoding and decoding for the data formats specified in [rfc4648 specification](https://datatracker.ietf.org/doc/html/rfc4648)
- `crc-32` - Standard CRC-32 algorithm implementation in JS

## Formatting
Some things are done non-traditional way for me. F.e. formatting by `dprint` package. I added custom file watcher, that will run dprint on all files.

In comparison to eslint, `dprint`:
- is more performant (written in Rust)
- has more configuration options
- is newer
- community is smaller, but grows continuously


## cli included
The bysquare package can be used as a command line tool. It's achieved by adding a `bin` entry into `package.json`

```package.json
  "bin": "./dist/cli.js",
```

On every execution in terminal, the file `cli.js` is executed.

Arguments are captured using [parseArgs](https://nodejs.org/docs/v20.16.0/api/util.html#utilparseargsconfig)

Based on captured arguments, encode/decode is executed or help is displayed.

## Local development of utils

For local development it's handy to be able to execute file without installing the package.

Add `#!/usr/bin/env node` to the top of a javascript file or `#!/usr/bin/env node-ts` to your typescript file.
Make file executable by adding rights

```bash
chmod +x yourscript.js
```
Now it can be executed directly from terminal
```bash
./yourscript.js --help
```

---
### Sources

[^1]: [bysquare project](https://github.com/xseman/bysquare)
