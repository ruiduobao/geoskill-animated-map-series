# 动态地图序列 / 动画 | Animated Map Series

把多期栅格（多波段 GeoTIFF 每波段一期）用**统一色标**渲染成逐帧 PNG，再合成为循环 GIF。统一色标保证各期之间可比较。

合成模式生成 NDVI 式季节时序（趋势 + 空间格局 + 噪声），每帧带时间标签与色标条。

## 核心算法

unified_scale 计算跨期全局 minmax/percentile 端点 → 逐期归一化 → colormap 渲染帧（含标签/色标）→ PIL 合成 GIF。

## 依赖

```bash
pip install numpy rasterio scipy matplotlib geopandas shapely pillow
```

## 使用方法

### 示例 1（合成数据，离线）

```bash
python geoskill-animated-map-series.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### 示例 2（指定帧时长）

```bash
python geoskill-animated-map-series.py --input series.tif --duration 300 --cmap viridis
```

### 示例 3（百分位统一色标）

```bash
python geoskill-animated-map-series.py --input series.tif --scale percentile --pct 2
```

### 示例 4（合成 12 期）

```bash
python geoskill-animated-map-series.py --bbox 116 39 117 40 --synthetic --periods 12
```

### 示例 5（地形配色）

```bash
python geoskill-animated-map-series.py --input series.tif --cmap terrain
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `animation.gif` | GIF | 循环动画（主产物） |
| `frames/frame_XXX.png` | PNG | 逐帧图像 |
| `series_stack.tif` | GeoTIFF | 多期 stack（可验证产物） |
| `frames.json` | JSON | 帧统计与色标信息 |

每次运行还会产出 `output-manifest.json`（运行清单）。

## 数据源 / Source

本地 GeoTIFF / 矢量文件；`--synthetic` 模式生成物理一致的模拟数据，完全离线。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传用户数据。

## License

MIT
