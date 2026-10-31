# Unofficial paint.rs packages

Local Flatpak and strict Snap packaging for [paint.rs](https://github.com/daijro/paint.rs) **v0.7.3**, by **daijro**. This is an unofficial downstream package that repackages the upstream x86_64 binary; it does not contain the application source. All credit for the paint.rs application goes to its author, daijro. Upstream is GPL-3.0-or-later; the packages preserve the archive's `LICENSE`, `LICENSE.md`, `NOTICE.md`, and complete `notices/` directory. The packaging repository itself retains the MIT license from its initial setup.

The upstream Linux x86_64 release is used (not an AVX2-specific variant). No ARM build is included. Its published SHA-256 is `1c3e8e5f93a42e54f33dd09182038d84647f350277648f17ee949702df46471e`.

## Flatpak

The Flatpak uses `org.freedesktop.Platform` 26.08. It grants network access, Wayland/X11 and GPU access, and direct access to Desktop, Documents, and Downloads for file-picker compatibility.

```sh
cd flatpak
flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install --user flathub org.freedesktop.Sdk//26.08 org.freedesktop.Platform//26.08
flatpak-builder --user --install --force-clean build-dir io.github.daijro.paintrs.yml
flatpak run io.github.daijro.paintrs
flatpak-builder --repo=repo --force-clean build-dir io.github.daijro.paintrs.yml
flatpak build-bundle repo ../releases/paintrs-0.7.3-x86_64.flatpak io.github.daijro.paintrs master
```

## Snap

The Snap uses core24 and strict confinement. The `home` interface permits direct access to ordinary non-hidden files under the user's home; `removable-media` is declared but requires a manual connection.

```sh
cd snap
snapcraft clean
snapcraft pack --use-lxd
mv paintrs_0.7.3_amd64.snap ../releases/
```

Install a local build only after reviewing its permissions:

```sh
sudo snap install --dangerous ../releases/paintrs_0.6.0_amd64.snap
```

`--dangerous` accepts a locally built unsigned artifact; it does not disable strict confinement.

## Checksums

| Artifact | SHA-256 |
| --- | --- |
| `paintrs-0.7.3-x86_64.flatpak` | `af739e1cf8735b74d981aae66ed7aa811f348cdf51afa6bf0614a5214b45e807` |
| `paintrs_0.7.3_amd64.snap` | `d201fff57d0920fe954973c100d614467e38a1c5e7c0d8e74a07917792678c59` |

Verify both local artifacts from `releases/` with:

```sh
cd releases
sha256sum -c SHA256SUMS
```
