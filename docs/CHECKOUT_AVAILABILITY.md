> Public export: availability below describes the source base. Check `PUBLICATION.json` for excluded native databases and private authoring files.

# Published checkout availability

The table below is derived from the remote base tree used for this documentation
update. It makes newer maintainer-local source selections visible without claiming
that they are already in this clone. A documentation commit does not change
firmware sources, original binaries, toolchain pins or selected submodule commits.

| Entry point | Available in remote base |
| --- | --- |
| `original/` | Yes |
| `src/baseline-exact/` | Yes |
| `src/overlay/` | Yes |
| `build-context/f363/` | Yes |
| `evidence/rea/public-export-20261022T132100Z-r2/` | Yes |
| `evidence/reconstruction-exact-paired.json` | Yes |
| `docs/RECONSTRUCTION.md` | Yes |
| `SOURCE_ORIGIN.json` | Yes |

## Submodule handoffs

Source and analysis submodule revisions stay pinned. Use the direct child
repository links in [the repository map](REPOSITORY_MAP.md) to read newer handoff
documentation; a parent clone intentionally retains the selected child revision.

## Integrity manifests

Where an artifact manifest includes README/documentation bytes, this update
refreshes only those members and aggregate counts. Existing non-document source
and evidence rows remain unchanged. Historical reports are not rewritten to
pretend that new builds or physical checks were run.
