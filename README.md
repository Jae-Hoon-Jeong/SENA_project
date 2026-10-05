# Project-page static asset warehouse

Public static assets used by the project pages of **Jaehoon Jeong**
(https://jae-hoon-jeong.github.io/). Served as plain files by GitHub Pages
(`.nojekyll` at the root) at:

```
https://jae-hoon-jeong.github.io/projectpage_essets/<path>
```

## Layout: one namespace per project, one sub-directory per version

```text
projects/
  <project>/
    PROVENANCE.md      source, licence and generation record for every asset of this project
    v1/
      wsi/<slide>/     Deep Zoom (DZI) pyramids: <slide>.dzi + <slide>_files/<level>/<col>_<row>.jpeg
      overlays/        public, precomputed overlays (JSON)
      images/
      videos/
      data/            public JSON / tables used by the page
    v2/ ...
```

- A version directory is never modified in place once a page references it; new or changed assets go to a
  new version (`v2/`, ...), so existing URLs stay stable and can be cached for a long time.
- Current projects: `projects/sena_nbe/` (SENA / NBE project page).

## What may be stored here
Only assets that are public and redistributable and are used by a public project page: images, videos,
DZI/WSI tiles, public JSON/overlays, and other static files whose licence allows redistribution and web hosting.

## What is never stored here
- credentials, tokens, keys or any other secret
- model checkpoints or embeddings
- private datasets, access-restricted or DUA-governed data, and original slides from restricted cohorts
  (e.g. TCGA source slides)
- any asset whose redistribution rights are unclear

## Provenance and licences
Each project directory carries its own `PROVENANCE.md` with the source URL, licence, original file hashes,
the generation tool/version, parameters and date for every asset. Licences of the assets are those stated
there; they are not covered by any licence of this repository's text.
