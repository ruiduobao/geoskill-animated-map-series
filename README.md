# Animated Map Series (geoskill-animated-map-series)

> Compose multi-temporal rasters into unified rendered frames and a GIF animation

---

## 1. Overview

Renders multi-temporal rasters (each band of a multi-band GeoTIFF is one epoch) into per-frame PNGs using a **unified color scale**, then composites them into a looping GIF. The unified color scale keeps epochs comparable with each other. The synthetic mode generates an NDVI-like seasonal time series (trend + spatial pattern + noise), with each frame carrying a time label and color bar.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-animated-map-series.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `animation.gif` | GIF | Looping animation (primary output) |
| `frames/frame_XXX.png` | PNG | Per-frame images |
| `series_stack.tif` | GeoTIFF | Multi-epoch stack (verifiable output) |
| `frames.json` | JSON | Frame statistics and color-scale information |

Each run also produces `output-manifest.json` (run manifest).


## 6. Technical Principle

unified_scale computes global cross-epoch minmax/percentile endpoints → per-epoch normalization → colormap frame rendering (with labels/color bar) → PIL compositing of the GIF.

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
