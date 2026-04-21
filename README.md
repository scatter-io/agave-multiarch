# agave-arm64-build

Multi-arch (`linux/amd64` + `linux/arm64`) Docker image of `solana-test-validator`
from [Agave](https://github.com/anza-xyz/agave).

- `linux/amd64` layer: extracted from the upstream release tarball (byte-identical to upstream).
- `linux/arm64` layer: built from source on a native arm64 runner. This is the point of the repo — upstream does not ship an arm64 Linux release.

Image: `ghcr.io/therealssj/agave-arm64-build:<tag>`

## Tags

- `v<version>` — pinned to Agave release `v<version>` (e.g. `v3.1.11`).
- `latest` — last successful build.

## Rebuilding

Bumping the Agave version:

```
gh workflow run build.yml -f agave_version=3.1.12
gh run watch
```

Or push a change to `Dockerfile.*` or the workflow on `main`.

## Package visibility

New packages publish as private by default. After the first successful run,
flip the package to public in the GitHub UI
(`Packages -> agave-arm64-build -> Package settings -> Change visibility`).
