# Provenance: projects/sena_nbe

## v1/wsi/cmu-1, v1/wsi/cmu-2 (Deep Zoom pyramids, generated 2026-10-05) — REMOVED 2026-10-06

Removed from the current tree on 2026-10-06 when the project page switched to the PAIP2020 demo (no longer
referenced). They remain in this repository's git history (commit 261bb83) for rollback.

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

## v2/paip (PAIP2020 demo; derived assets only, added 2026-10-06)

**Source data.** PAIP2020 challenge dataset (Seoul National University Hospital / Pathology AI Platform),
licensed **CC BY-NC 4.0** (https://creativecommons.org/licenses/by-nc/4.0/) per the challenge Rules page
(https://paip2020.grand-challenge.org/). Used here for a **non-commercial academic project page**.
"De-identified pathology images and annotations used in this research were prepared and provided by the Seoul
National University Hospital by a grant of the Korea Health Technology R&D Project through the Korea Health
Industry Development Institute (KHIDI), funded by the Ministry of Health & Welfare, Republic of Korea (grant number:
HI18C0316)." **Changes were made**: everything below is derived; the original `.svs` slides are not stored here.

| slide | original file | original SHA-256 |
|---|---|---|
| training_data_44 | PAIP2020 `training_data_44.svs` | `1739f2560f653f1eca914c5d646a56648c0553ad742a8b15eb8db47e3d9a7858` |
| training_data_03 | PAIP2020 `training_data_03.svs` | `30982cb66b21c5a244981154853045f95bbcc6b5bd11c6072b790f8451acee06` |

Annotation archive: PAIP2020 `training_data.zip`, SHA-256 `ac2bc8c0452ee898a85a6a71ce0c041db41ec6229cded097cbf9cbb1ff752d3f`.

### `<slide>/gt/whole_tumor_area.json` (reference layer: pathologist ground truth)
- PAIP2020 Whole Tumor Area annotation (`<slide>.xml`, Aperio XML, MicronsPerPixel 0.2522), vertices copied
  unchanged into JSON (level-0 px). No smoothing/simplification. PAIP2020 has no Viable Tumor Area annotation;
  none is provided. Converter `paip_xml_to_json.py` (SHA-256 `5cfd789b…37ca`).
- training_data_44: 1 region, 1,011 vertices; training_data_03: 1 region, 1,209 vertices.

### `<slide>/overlays/<Mode>/` (model layer: nucleus-level predictions; not a tumour segmentation)
- Format `nuclei-chunks/v1` (`index.json`, `coarse/<cx>_<cy>.bin` 16384-px chunks, `fine/<cx>_<cy>.bin` 2048-px
  chunks; see the converter docstring). Converter `make_overlay_chunks.py` (SHA-256 `5f1e5bba…1fd68`).
  Each record is one row of the model output: centroid, type id (0 Neoplastic, 1 Inflammatory, 2 Connective,
  3 Dead, 4 Epithelial — as stored by the release), `confidence` quantised to round(255·conf), contour resampled
  to 12 points (int8 offsets, level-0 px).
- Model: frozen release v1.0.1 package (student `edef2edd…8864`, refiner `fdeaeba8…adb4`, config `a5f80512…358f`,
  system.json `19835c09…6cf02`, `wsi_infer.py` `3b5035b2…288a`), full-slide inference, 256-px tiles / 64-px overlap.
- Eff / Cls (available now): server-34 outputs of 2026-10-04 (`run_paipfull.sh`; Eff batch 1, Cls batch 4).
  Source parquet SHA-256: 44 Eff `3133a356…7e5`, 44 Cls `2fe82f02…5887`, 03 Eff `a6221018…11ce`, 03 Cls `21f71097…e7a`.
- Seg / Full: same package and settings, server 32 (`~/envs/sena34`, pip-freeze identical to the server-34 env),
  `run_segfull.sh` / `run_full_b1.sh`, batch 1 (the release allows batch > 1 only for Eff/Cls). Added per slide as
  each run finishes. Source parquet SHA-256: 03 Seg `06267283…242b5`, 44 Seg `6c19ac75…850c4`.

| slide | mode | nuclei | files | bytes |
|---|---|---|---|---|
| training_data_44 | Eff | 1,135,916 | 1,088 | 40,918,250 |
| training_data_44 | Cls | 1,135,961 | 1,088 | 40,920,016 |
| training_data_44 | Seg | 1,133,610 | 1,088 | 40,835,230 |
| training_data_03 | Eff | 527,429 | 644 | 19,004,355 |
| training_data_03 | Cls | 527,440 | 644 | 19,004,806 |
| training_data_03 | Seg | 526,359 | 644 | 18,965,830 |

### `<slide>/dzi/` (Raw H&E, web-display Deep Zoom)
- Web-display resolution **0.5044 µm/px** (2x native 0.2522 µm/px): the native Deep Zoom pyramid without its top
  level, written directly from the original slide with a single JPEG encoding at **quality 50**
  (`make_dzi_web.py`; openslide-python 1.4.6, OpenSlide 4.0.1, Pillow 12.2.0; tile 510, overlap 1, limit_bounds False).
  Model inference and all overlay/GT coordinates stay in native level-0 pixels; the page scales them by
  web_width / native_width at display time (`<slide>.meta.json`).
- training_data_03: 55,776 x 46,357 px, 17 levels, 13,420 tiles (+ .dzi, meta), 232,188,536 bytes; generated 2026-10-06 on server 35.
