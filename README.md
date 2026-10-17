# stems

Local stem factory. Queue tracks on your Mac → Rekordbox vocal/instrumental files + a Traktor `.stem.m4a` next to the mix. No upload step. Built for a **35k+** library where GUIs tap out after a dozen songs.

[NUO-STEMS](https://nuo-stems.com) is the inspirational piece. Derived from the underlying algorithms leveraging GPU we batch CLI crates of music at a time [ixamal/ix](https://github.com/ixamal/ix) **STEMIT**. [ixamal](https://github.com/ixamal) · [alkalurops.org](https://www.alkalurops.org)

Grab tracks → `python3 -m py.exec.separate` → `{name}.stem.m4a` next to the MP3 (plus Rekordbox vocals/instrumental when there is voice). Same machine as Traktor. Audio stays on disk. Apple Music stays in its own tree. Tried and parked ideas live in the docs — this is the shop log, not only the tools that stuck.

`mel_vocals_bs_others` generates parallel NI STEM files with Rekordbox pairing. **Generator:** local Mel + BS-RoFormer CLI.

We also take a look from [Fadr](https://fadr.com) to host a black box with an HTTP API and no local CLI (`docs/PROGRESS.md`).

## `py/` tools

See [`py/README.md`](py/README.md). From the repo root (`ffmpeg`, `MP4Box`, `pip install -r requirements.txt`, Homebrew Python 3.12 `.venv`):

```bash
python3 -m py
.venv/bin/python -m py.exec.separate --path m3u/nuo_queue_test4.m3u
.venv/bin/python -m py.exec.extract_stems --path ~/path/to/stem-containers --execute
```

Dry-run is the default. Mute mixes print `we found none` and skip the Rekordbox pair (the original *is* the instrumental). Every CLI tees to `log/` (JSON + matplotlib overlay) unless `--quiet`. `--execute` opens an Aqua progress window (stays until Close) and mails the local notify address.

| Module | Class | What |
|--------|-------|------|
| `py.exec.separate` | `LocalSeparator` | Mel pair + BS-RoFormer `.stem.m4a` |
| `py.exec.extract_stems` | `StemExtractor` | NI STEM → named M4As |
| `py.exec.vocals_instrumental` | `VocalsInstrumental` | Pair from STEM; else `m3u/` queue |
| `py.utils.apple` | `AppleFriendly` | Apple-friendly video/audio — not Music.app |
| `py.tools.simple_mp3` | `SimpleMp3` | 320k MP3 for the H2O headset |
| `py.tools.tag_updater` | `TagUpdater` | Tags from `Artist - Title` filenames |
| `py.tools.trak_extract` | `TrakExtractor` | `.trak` → folder + M3U |
| `py.exec.cleanup_unknown` | `UnknownRoleCleanup` | Nameless role WAVs → quarantine |
| `py.exec.validate_meta` | `MetaValidator` | QA / repair source tags on generated files |

Library remaps stay in [ixamal/music_migration](https://github.com/ixamal/music_migration). The mixer cable is [ixamal/blackhole](https://github.com/ixamal/blackhole).

License: [Apache-2.0](LICENSE).

## Docs

| Doc | What |
|-----|------|
| [`docs/RUNBOOK.md`](docs/RUNBOOK.md) | Local CLI, then extract |
| [`docs/MASTER_CONTEXT.md`](docs/MASTER_CONTEXT.md) | Paths, package map, what this repo is not |
| [`docs/PROGRESS.md`](docs/PROGRESS.md) | The log |
| [`docs/35k-stem-throughput.md`](docs/35k-stem-throughput.md) | RunPod vs GCP/AWS; 35k in a few days, not 92 |
| [`docs/settings/nuo-stems-4/NOTES.md`](docs/settings/nuo-stems-4/NOTES.md) | NUO app notes |
| [`docs/settings/py/NOTES.md`](docs/settings/py/NOTES.md) | Tool / class map |

GitHub is docs + `py/` + `m3u/README.md` + `log/` examples. No stems audio, no NML, no collection dumps, no `m3u/*.m3u`.
