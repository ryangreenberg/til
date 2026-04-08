# `oxipng` and `pngquant` to compress PNGs

The last time I needed to optimize image sizes was quite some time ago. With this kind of thing, if you wait long enough, a better tool will be available. Last time I remember using `pngcrush`; now there's [`oxipng`](https://github.com/oxipng/oxipng) for lossless compression and [`pngquant`](https://pngquant.org/) for lossy compression.

I tried this with a small screenshot:

```
$ du -h *
 92K    screenshot.png

$ oxipng screenshot.png --out screenshot-oxipng.png
Files processed: 1/1
Input size: 89.7 KiB (91873 bytes)
Output size: 50.3 KiB (51482 bytes)
Total saved: 39.4 KiB (43.96%)

$ pngquant screenshot.png --output screenshot-pngquant.png

$ du -h * | sort -h
 32K    screenshot-pngquant.png
 52K    screenshot-oxipng.png
 92K    screenshot.png
```

The original was 92K, the lossless oxipng version is 52K (57%), and the oxipng version is 32K (35%). I could not identify any visual differences looking at the lossy pngquant version versus the original.
