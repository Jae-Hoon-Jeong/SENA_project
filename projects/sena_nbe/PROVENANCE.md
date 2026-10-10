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
- training_data_44: 52,788 x 40,843 px, 17 levels, 11,313 tiles (+ .dzi, meta); generated 2026-10-06 on server 35.

### `<slide>/overlays/EffRouting/` (semantic-routing view for the page slider; format `nuclei-chunks/v2-routing`)
- Geometry = the final Eff output (same nuclei and contours as `overlays/Eff`). Per nucleus additionally: the class the
  Cls path gives the same detection (nearest Full-Cls candidate of the same tile within 3 px) and the rank of its tile
  in the Stage-2 router order (Router 1, entropy, label-free). At budget f the page shows the Cls class for nuclei in
  the first round(f x eligible) tiles and the Eff class elsewhere. Class-only view: unlike a real Stage-2 run,
  detections/contours are not re-merged.
- Inputs (server 34, read-only): frozen Eff parquet; Stage-2 quality dumps `pass1_cands`, `fullcls_cands`,
  `routing_entropy.npz`. Converter `make_routing_chunks.py` (SHA-256 `b10fdc3a…154f2f`).
- 03: 527,429 nuclei, all mapped to a tile, 527,314 matched to a Cls candidate, class differs for 54,839 at 100%;
  44: 1,135,916 / 1,135,916 / 1,135,711, 145,763.

### `<slide>/pred_area/<Mode>.json` (Predicted tumor area; model prediction, not ground truth)
- Final deploy rule A-simple (TCGA protocol Amendment T-4), Protocol Z, computed from the nuclei of that mode:
  Neoplastic core (100-um bins, t = 0.5, tissue) -> drop components < 0.1 mm2 -> add tissue bins within 500 um with
  (Connective+Dead)/N >= 0.5 -> hole fill -> AND tissue. Exported as contour polygons (native level-0 px).
  Converter `pred_tumor_area.py` (SHA-256 `693dac20…7f9`), frozen Track C / A helpers imported read-only.
- Eff and Cls maps reproduce the PAIP post-hoc A_nodens Protocol-Z Dice of the same slides exactly
  (03: 0.5648 / 0.7084; 44: 0.7375 / 0.9160). Seg maps use the Seg parquets (03: Dice 0.5591, 44: 0.7378).

### `<slide>/pred_area_d100/<Mode>.json` (displayed since 2026-10-06; POST-HOC variant)
- Same A-simple rule with the expansion distance **d = 100 um instead of 500 um** (user request 2026-10-06, made after the
  PAIP confirmation results were known; post-hoc, not a preregistered rule). Converter `pred_tumor_area_d.py`
  (SHA-256 `b24893dc…444`) = `pred_tumor_area.py` with d as a parameter; with d = 500 it reproduces the
  `pred_area/` files above exactly (6/6 regions and Dice identical).
- Dice vs GT (same slides): 03 Eff 0.5803 / Cls 0.6653 / Seg 0.5762; 44 Eff 0.7719 / Cls 0.9336 / Seg 0.7723.
- The d = 500 files in `pred_area/` are kept unchanged.

## v2/fourier (Fourier-epicycle illustration of PanNuke ground-truth nucleus contours, added 2026-10-11)

**Source data.** PanNuke (Gamper et al., "PanNuke: an open pan-cancer histology dataset for nuclei instance
segmentation and classification", ECDP 2019; arXiv:2003.10778), Fold 3 `images.npy` / `masks.npy`, licensed
**CC BY-NC-SA 4.0** (https://creativecommons.org/licenses/by-nc-sa/4.0/). The derived videos and posters in this
directory are therefore distributed under **CC BY-NC-SA 4.0**. Non-commercial academic use.

**Nuclei shown** (ground-truth instances; neoplastic channel; 256x256 PanNuke patches):

| file prefix | Fold 3 image idx | instance id | class | tissue | area (px) | contour error K=10 |
|---|---|---|---|---|---|---|
| nucleus1 | 2479 | 312 | Neoplastic | Testis | 4788 | 0.77% |
| nucleus2 | 587 | 168 | Neoplastic | Breast | 4200 | 0.65% |
| nucleus3 | 613 | 30 | Neoplastic | Breast | 3695 | 0.77% |
| nucleus4 | 588 | 80 | Neoplastic | Breast | 3653 | 0.96% |

Selected for illustration only: among 17,697 Fold 3 nuclei, the fourth quintile of K=3 reconstruction error
(0.0305-0.0391; pool 3,556), four large picks. Not a performance result; no model output is shown.

**Generation.** Outer contour (`cv2.findContours`, external, no approximation) resampled to 128 points by arc length;
complex FFT / N; a K-harmonic reconstruction keeps c0 and c_{+-1..+-K}; epicycles chained +1, -1, +2, -2, ...
Rendering: matplotlib FuncAnimation, 120 frames at 20 fps (6 s loop), original H&E colours (no stain normalisation),
crop = nucleus bounding box + 12 px margin, shifted inside the patch (no padding). Style "W2": white circles and radii
(radius lw 1.2) with soft dark shadow, lime (#B6FF3B) reconstructed contour, black dashed ground-truth contour,
red tip. Error = mean distance between corresponding samples of the reconstruction and the 128-point ground-truth contour, divided by the equivalent-circle radius sqrt(area / pi).
`nucleus1_harmonics_K1-10` shows K = 1, 2, 3, 5, 7, 10; `nucleus*_epicycles_K10` shows K = 10.
Encoding: H.264 High, yuv420p, CRF 23, re-muxed with `-movflags +faststart`, no audio. Posters (`.jpg`) are the
final frame (`ffmpeg -sseof -0.05 -frames:v 1 -q:v 3`). GIF versions were not published (about 20x larger).

| file | bytes | SHA-256 |
|---|---|---|
| `v2/fourier/nucleus1_epicycles_K10.jpg` | 70344 | `ba6af9962898d822bf1cf484564114966e90d8803bd4853b5c13c52092f4f16f` |
| `v2/fourier/nucleus1_epicycles_K10.mp4` | 348487 | `eadf015b626ba7aebd9c46b5dae074b9db3aa69e7fb8ead451f60c5ddd47593b` |
| `v2/fourier/nucleus1_harmonics_K1-10.jpg` | 292829 | `018c67d2d8480a7bb640b3dfb41e1da13a9fe00f5e53f17be2be05de7ac5a3f4` |
| `v2/fourier/nucleus1_harmonics_K1-10.mp4` | 1497576 | `4d0417b5763fbd39b33db80994c415a10dbd792dc200d5ad958bd9f5c5f2fb2f` |
| `v2/fourier/nucleus2_epicycles_K10.jpg` | 48838 | `2dd76cd5ec07e53a1bd843a557bce3cdd2ba64542b63519fa615aa002d3eca07` |
| `v2/fourier/nucleus2_epicycles_K10.mp4` | 261463 | `05c0cd770ba2f22b5871fe05eddf7126b0939e96aa53fcd8ddae5f060628d94e` |
| `v2/fourier/nucleus3_epicycles_K10.jpg` | 48690 | `48a9b40de8459521f207615100aa6b554433a27cb405f98e1c98b5d3b35fcba6` |
| `v2/fourier/nucleus3_epicycles_K10.mp4` | 249751 | `ec0a9948e5bc136ca86cb84649b1346221d6b0b164f0eee882d04a66dbd28692` |
| `v2/fourier/nucleus4_epicycles_K10.jpg` | 50747 | `0f97744fa40f2d41c904c778e90945fcb1945c29b3b02c87f68c4bede404477d` |
| `v2/fourier/nucleus4_epicycles_K10.mp4` | 266075 | `fd99ca86db50f50b119756cdb5da9aeddc39fb3c4ae990efd46db84a71623b92` |
