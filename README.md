# agave-multiarch

Multi-arch (`linux/amd64` + `linux/arm64`) Docker image of `solana-test-validator`
from [Agave](https://github.com/anza-xyz/agave).

- `linux/amd64` layer: extracted from the upstream release tarball (byte-identical to upstream), SHA256-verified.
- `linux/arm64` layer: built from source on a native arm64 runner, pinned to the upstream tag's commit SHA. This is the point of the repo — upstream does not ship an arm64 Linux release.

Image: `ghcr.io/scatter-io/agave-multiarch:<tag>`

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

## Version bumps

Bumping `AGAVE_VERSION` requires updating two additional pinned values so the
build remains reproducible:

- `AGAVE_AMD64_SHA256` in `Dockerfile.amd64` — SHA256 of the upstream release tarball.
- `AGAVE_COMMIT_SHA` in `Dockerfile.arm64` — commit SHA the upstream tag points at
  (the arm64 build cross-checks that the tag still resolves to this SHA, so a silent
  upstream tag retarget fails the build).

Resolve both in one go:

```bash
VERSION=3.1.12
curl -fsL "https://github.com/anza-xyz/agave/releases/download/v${VERSION}/solana-release-x86_64-unknown-linux-gnu.tar.bz2" | sha256sum
gh api "/repos/anza-xyz/agave/git/refs/tags/v${VERSION}" --jq .object.sha
```

The debian base images are pinned to digests (`debian:bookworm{,-slim}@sha256:...`);
bump these manually (or via Dependabot) when security updates land.

## Package visibility

New packages publish as private by default. After the first successful run,
flip the package to public in the GitHub UI
(`Packages -> agave-multiarch -> Package settings -> Change visibility`).
