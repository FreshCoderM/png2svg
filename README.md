# png2svg [![GoDoc](https://godoc.org/github.com/xyproto/png2svg?status.svg)](http://godoc.org/github.com/xyproto/png2svg) [![Go Report Card](https://goreportcard.com/badge/github.com/xyproto/png2svg)](https://goreportcard.com/report/github.com/xyproto/png2svg)

Go module and command line utility for converting small PNG images to SVG Tiny 1.2.

## Features and limitations

* Draws rectangles for each region in the PNG image that can be covered by a rectangle.
* The remaining pixels are drawn with a rectangle for each pixel.
* This is not an efficient representation of PNG images!
* The conversion may be useful if you have a small PNG image or icons at sizes around 32x32, and wish to scale it up and print it out without artifacts.
* The utility is fast for small images, but larger images will take an unreasonable amount of time to convert, creating SVG files many megabytes in size.
* The resulting SVG images can be opened directly in a browser like Firefox or Chromium, and may look sharper and crisper than small PNG or JPEG images that are smoothed/blurred by the browser, by default (this can be configured with CSS, though).
* The default crispiness of how SVG images are displayed may be useful for displaying "pixel art" style graphics in the browser.
* Written in pure Go, with no runtime dependencies on any external library or utility.
* Handles transparent PNG images by not drawing SVG elements for the transparent regions.
* For creating SVG images that draws a rectangle for each and every pixel, instead of also using larger rectangles, use the `-p` flag.

## Image Comparison

| 192x192 PNG image (16 colors) | 192x192 SVG image (16 colors) | 192x192 SVG image (optimized with [svgo](https://github.com/svg/svgo)) |
| ----------------------------- | ----------------------------- | ----------------------------------------------------------------------|
| 8 KB                          | 188 KB                        | 61 KB                                                                 |
| ![png](img/spaceships.png)    | ![png](img/spaceships.svg)    | ![png](img/spaceships_opt.svg)                                        |

The spaceships are drawn by [wuhu](https://opengameart.org/content/spaceships-1) (CC-BY 3.0).

Try zooming in on both images. Most browsers will keep the SVG image crisp when zooming in, but blur the PNG image.

For keeping PNG images crisp, this CSS can be used, but this is not normally needed for SVG images:

```css
image-rendering: -moz-crisp-edges; /* Firefox */
image-rendering: -o-crisp-edges; /* Opera */
image-rendering: -webkit-optimize-contrast; /* Webkit (non-standard naming) */
image-rendering: crisp-edges;
-ms-interpolation-mode: nearest-neighbor; /* IE (non-standard property) */
```

Other comparisons:

| 302x240 PNG image          | 302x240 SVG image (limited to 4096 colors)  |
| -------------------------- | ------------------------------------------- |
| 171 KB                     | 2.98 MB                                     |
| ![png](img/rainforest.png) | ![png](img/rainforest4096.svg)              |

The rainforest image is from [Wikipedia](https://en.wikipedia.org/wiki/Landscape).

| 64x64 PNG image      | 64x64 SVG image (one rectangle per pixel) | 64x64 SVG image (optimized) | 64x64 SVG image (4096 colors) | 64x64 SVG image (rectangles >1px are colored pink) |
| -------------------- | ----------------------------------------- | --------------------------- | ----------------------------- | -------------------------------------- |
| 2.22 KB              | 231 KB                                    | 71.2 KB                     | 66.7 KB                       |                                        |
| ![png](img/acme.png) | ![png](img/acme_singlepixel.svg)          | ![png](img/acme.svg)        | ![png](img/acme4096.svg)      | ![png](img/acmecolor.svg)              |

The Glenda bunny is from [9p.io](https://9p.io/plan9/glenda.html).

## Q&A

* Q: Why 4096 colors?
* A: Because representing colors on the short form (`#000` as opposed to `#000000`) makes it possible to express 4096 unique colors.

## Installation

Development version:

    go get -u github.com/xyproto/png2svg/cmd/png2svg

## Example usage

Generate an SVG image with one rectangle per pixel:

    png2svg -p -o output.svg input.png

Generate an SVG image with as few rectangles as possible (optimized):

    png2svg -o output.svg input.png

Generate an SVG image with as few rectangles as possible (4096 colors):

    png2svg -q -o output.svg input.png

Like above, but with verbose/progress output:

    png2svg -v -q -o output.svg input.png

## General information

* Version: 1.3.2
* Author: Alexander F. Rødseth &lt;xyproto@archlinux.org&gt;
* License: MIT
