# Real-ESRGAN

**Real-ESRGAN (image super-resolution / restoration)** — General-purpose 4x super-resolution and restoration for real-world images.

An **azphalt** AI-model plugin, packaged as a `.azp` (the azphalt analogue of a VS Code `.vsix`). It is
named for the *model*, not a single feature — the same model powers many tools, and it is **host-neutral**:
any azphalt host that understands its role can use it, not just one app. Install it from any host's
**Azphalt Storefront**.

## What it can do

- upscaling low-res footage
- detail restoration
- de-compression / artifact cleanup

## Roles (host-neutral routing)

This plugin contributes the role(s): `superres`. A host routes the model by role — it carries no
`targetApps`, so it is not tied to any single application.

**Example host — [Guillotine](https://github.com/HereLiesAz/Guillotine):** Desktop `effect_superres` — `apply_image_effect effect=superres`.

## Model file(s)

- **`realesrgan.onnx`** (role `superres`) — [upstream](https://huggingface.co/Xenova/real-esrgan-x4/resolve/main/onnx/model.onnx)

Model license: **BSD-3-Clause (Real-ESRGAN, Xintao Wang et al.)**. This plugin's manifest/packaging is `BSD-3-Clause`.

## How it works — the VSCode Header Pattern

The `.azp` does **not** bundle the weights. The manifest declares each model as a *remote asset*
(`"path": ""` + `remoteUrl` + `checksum` + `byteSize`); the host downloads the weights on install and
verifies them against the pinned SHA-256 — exactly how a large VS Code extension fetches its language
server instead of shipping it inside the `.vsix`. `remoteUrl` points at this repo's own GitHub **Release**
asset (named the exact filename the host expects); the `release` workflow fetches the upstream model,
renames it, checksums it, and publishes it beside the packed `.azp`.

## Build / release

```sh
npm install && npm run build     # packs com.hereliesaz.azphalt.real-esrgan-1.0.0.azp
git tag v1.0.0 && git push --tags   # runs the release workflow: hosts the model + .azp
```
