# Provenance: projects/sena_nbe

## v1/wsi/cmu-1, v1/wsi/cmu-2 (Deep Zoom pyramids, generated 2026-10-05)

| slide | source (original file) | licence | original SHA-256 | original bytes |
|---|---|---|---|---|
| CMU-1.svs (Aperio, brightfield) | https://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/CMU-1.svs | CC0-1.0 | `00a3d54482cd707abf254fe69dccc8d06b8ff757a1663f1290c23418c480eb30` | 177,552,579 |
| CMU-2.svs (Aperio, brightfield) | https://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/CMU-2.svs | CC0-1.0 | `fb6df83bfd91a252185c9652aebeb00deff84e49329f7ff8f75744f31f475b08` | 390,750,635 |

- Licence and SHA-256 as listed in the source's `index.yaml`
  (https://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/index.yaml); downloaded files matched.
- OpenSlide test data, Carnegie Mellon University. CC0 1.0 (public-domain dedication).
- The original `.svs` files are **not** stored here; only derived Deep Zoom tiles.

### Generation
- Tool: `openslide.deepzoom.DeepZoomGenerator` (openslide-python 1.4.6, OpenSlide 4.0.1), JPEG writing with Pillow 12.2.0.
- Parameters: `tile_size=510`, `overlap=1`, `limit_bounds=False`, format `jpeg`, quality 80.
- Per tile: `DeepZoomGenerator.get_tile(level, (col, row))`, converted to RGB, saved as `<level>/<col>_<row>.jpeg`;
  descriptor from `get_dzi('jpeg')`.
- Date: 2026-10-05.

| | full-resolution size (px) | mpp | levels | files (incl. .dzi) | bytes |
|---|---|---|---|---|---|
| cmu-1 | 46000 x 32914 | 0.499 | 17 (0-16) | 7,986 | 168,780,576 |
| cmu-2 | 78000 x 30462 | 0.499 | 18 (0-17) | 12,301 | 383,378,592 |

- Attribution text used on the page: "CMU-1.svs / CMU-2.svs, OpenSlide test data (Carnegie Mellon University), CC0 1.0."
- Not for clinical or diagnostic use.
