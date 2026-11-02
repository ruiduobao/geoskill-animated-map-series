---
name: geoskill-animated-map-series
description: 'Compose multi-temporal rasters into unified rendered frames and a GIF animation'
---

# 动态地图序列 / 动画 | Animated Map Series

Renders multi-temporal rasters (one epoch per band of a multi-band GeoTIFF) into per-frame PNGs using a **unified color scale**, then composes them into a looping GIF. The unified color scale keeps epochs comparable with each other.

Synthetic mode generates an NDVI-like seasonal time series (trend + spatial pattern + noise), with each frame carrying a time label and color bar.

## Core Algorithm / 核心算法

unified_scale computes cross-epoch global minmax/percentile endpoints → per-epoch normalization → colormap rendering of frames (with labels/color bar) → GIF composition with PIL.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy matplotlib geopandas shapely pillow
```

## Usage / 使用方法

### Example 1 (Synthetic Data, Offline)

```bash
python geoskill-animated-map-series.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2 (Custom Frame Duration)

```bash
python geoskill-animated-map-series.py --input series.tif --duration 300 --cmap viridis
```

### Example 3 (Uniform Percentile Color Scale)

```bash
python geoskill-animated-map-series.py --input series.tif --scale percentile --pct 2
```

### Example 4 (12 Synthetic Periods)

```bash
python geoskill-animated-map-series.py --bbox 116 39 117 40 --synthetic --periods 12
```

### Example 5 (Terrain Color Scheme)

```bash
python geoskill-animated-map-series.py --input series.tif --cmap terrain
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `animation.gif` | GIF | Looping animation (primary output) |
| `frames/frame_XXX.png` | PNG | Per-frame images |
| `series_stack.tif` | GeoTIFF | Multi-epoch stack (verifiable output) |
| `frames.json` | JSON | Frame statistics and color scale info |

Each run also produces `output-manifest.json` (run manifest).

## Data Source / 数据源 / Source

Local GeoTIFF / vector files; `--synthetic` mode generates physically consistent simulated data, fully offline.

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is done locally; user data is never uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
