# shortbread

Shortbread-schema OSM vector-tile generation for OpenCharly images — a
source-built [Tilemaker](https://github.com/systemed/tilemaker) plus the official
[shortbread-tiles/shortbread-tilemaker](https://github.com/shortbread-tiles/shortbread-tilemaker)
config fleet.

The `shortbread` candy builds Tilemaker (C++/Lua) from source — there is no
Arch/Fedora package and no release binary — and installs it at
`/usr/local/bin/tilemaker`. It clones the official Shortbread fleet (the
`process.lua` per-feature classifier + the `config.json` layer/zoom schema) to
`/opt/shortbread-tilemaker` with its `.git` history stripped. With both in place
a notebook DAG can convert an OSM PBF into `monaco-shortbread.pmtiles` under the
versatiles-watched `/workspace/tiles/shortbread` directory.

The [Shortbread schema](https://shortbread-tiles.org) is the de-facto
general-purpose OSM vector-tile schema.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `shortbread` |
| Tool | `/usr/local/bin/tilemaker` (source-built) |
| Config fleet | `/opt/shortbread-tilemaker/{config.json,process.lua,…}` |
| Output dir | `/workspace/tiles/shortbread/` (versatiles serve watches this) |
| Distros | Arch + Fedora |
| Requires | [`layer-supervisord`](https://github.com/opencharly/layer-supervisord) |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list, then invoke
Tilemaker from a notebook DAG cell:

```python
subprocess.run([
    "tilemaker",
    "--input",  "/workspace/tiles/work/monaco.osm.pbf",
    "--output", "/workspace/tiles/shortbread/monaco-shortbread.pmtiles",
    "--config", "/opt/shortbread-tilemaker/config.json",
    "--process", "/opt/shortbread-tilemaker/process.lua",
], check=True)
```

Tilemaker v3.0+ detects the `.pmtiles` extension on `--output` and produces a
PMTiles archive directly. The candy's `plan:` asserts the binary, the config
fleet, the stripped `.git`, and the pre-created output directory.

## Layout

- `charly.yml` — the `shortbread:` candy entity (the `require:`, the
  `distro.{arch,fedora}:` build-dependency sections, the source-build steps, and
  the `check:` probes) and the embedded `shortbread-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-versa:shortbread`
- Image: `/charly-versa:versa`
- Tile server: `/charly-versa:versatiles`, `/charly-versa:versatiles-style`
- DAG: `/charly-versa:notebook-osm`
- Sibling pipeline: `/charly-versa:osm-tools-layer`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
