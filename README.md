# Unofficial paint.rs packages

Local Flatpak and strict Snap packaging for [paint.rs](https://github.com/daijro/paint.rs) **v0.8.0**, by **daijro**. This is an unofficial downstream package that repackages the upstream x86_64 binary; it does not contain the application source. All credit for the paint.rs application goes to its author, daijro. Upstream is GPL-3.0-or-later; the packages preserve the archive's `LICENSE`, `LICENSE.md`, `NOTICE.md`, and complete `notices/` directory. The packaging repository itself retains the MIT license from its initial setup.

The upstream Linux x86_64 release is used (not an AVX2-specific variant). No ARM build is included. Its published SHA-256 is `6673be9a1197459e612d9dec83d1e1c0346535fe943b6e0feb5592044e850404`.

## Flatpak

The Flatpak uses `org.freedesktop.Platform` 26.08. It grants network access, Wayland/X11 and GPU access, and direct access to Desktop, Documents, and Downloads for file-picker compatibility.

```sh
cd flatpak
flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install --user flathub org.freedesktop.Sdk//26.08 org.freedesktop.Platform//26.08
flatpak-builder --user --install --force-clean build-dir io.github.daijro.paintrs.yml
flatpak run io.github.daijro.paintrs
flatpak-builder --repo=repo --force-clean build-dir io.github.daijro.paintrs.yml
flatpak build-bundle repo ../releases/paintrs-0.8.0-x86_64.flatpak io.github.daijro.paintrs master
```

## Snap

The Snap uses core24 and strict confinement. The `home` interface permits direct access to ordinary non-hidden files under the user's home; `removable-media` is declared but requires a manual connection.

```sh
cd snap
snapcraft clean
snapcraft pack --use-lxd
mv paintrs_0.8.0_amd64.snap ../releases/
```

Install a local build only after reviewing its permissions:

```sh
sudo snap install --dangerous ../releases/paintrs_0.8.0_amd64.snap
```

`--dangerous` accepts a locally built unsigned artifact; it does not disable strict confinement.

## Checksums

| Artifact | SHA-256 |
| --- | --- |
| `paintrs-0.8.0-x86_64.flatpak` | `0817427caf7d861f134a064e17ba4dfdcf51c9b1ca52065f524fce9551342f6a` |
| `paintrs_0.8.0_amd64.snap` | `0c214bcfe30258d09875cc7414f518b59ccc66600ec4167195b1bc075b1edc5a` |

Verify both local artifacts from `releases/` with:

```sh
cd releases
sha256sum -c SHA256SUMS
```
