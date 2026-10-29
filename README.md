# Unofficial paint.rs packages

Local Flatpak and strict Snap packaging for [paint.rs](https://github.com/daijro/paint.rs) **v0.6.0**, by **daijro**. This is an unofficial downstream package that repackages the upstream x86_64 binary; it does not contain the application source. All credit for the paint.rs application goes to its author, daijro. Upstream is GPL-3.0-or-later; see `UPSTREAM-LICENSE`, `UPSTREAM-LICENSE.md`, and `UPSTREAM-NOTICE.md` for bundled upstream notices. The v0.6.0 archive also includes `NOTICE.md` and `notices/NOTICE.md`, included in both packages. The packaging repository itself retains the MIT license from its initial setup.

The upstream Linux x86_64 release is used (not an AVX2-specific variant). No ARM build is included. Its published SHA-256 is `d8ee902faaa07f4b3f108034c9412115b85947b8d1f7d9de7c1b297a8bcf0359`.

## Flatpak

The Flatpak uses `org.freedesktop.Platform` 26.08. It grants network access, Wayland/X11 and GPU access, and direct access to Desktop, Documents, and Downloads for file-picker compatibility.

```sh
cd flatpak
flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install --user flathub org.freedesktop.Sdk//26.08 org.freedesktop.Platform//26.08
flatpak-builder --user --install --force-clean build-dir io.github.daijro.paintrs.yml
flatpak run io.github.daijro.paintrs
flatpak-builder --repo=repo --force-clean build-dir io.github.daijro.paintrs.yml
flatpak build-bundle repo ../releases/paintrs-0.6.0-x86_64.flatpak io.github.daijro.paintrs master
```

## Snap

The Snap uses core24 and strict confinement. The `home` interface permits direct access to ordinary non-hidden files under the user's home; `removable-media` is declared but requires a manual connection.

```sh
cd snap
snapcraft clean
snapcraft pack --use-lxd
mv paintrs_0.6.0_amd64.snap ../releases/
```

Install a local build only after reviewing its permissions:

```sh
sudo snap install --dangerous ../releases/paintrs_0.6.0_amd64.snap
```

`--dangerous` accepts a locally built unsigned artifact; it does not disable strict confinement.

## Checksums

| Artifact | SHA-256 |
| --- | --- |
| `paintrs-0.6.0-x86_64.flatpak` | `6ab02355082b7cbdf79082d86f01914a0567d718afb703c5fee164a42d138066` |
| `paintrs_0.6.0_amd64.snap` | `3bdf42daee8f287649a6ba63a077b5a19f896e94c0ef26373258b4dbb0f32234` |

Verify both local artifacts from `releases/` with:

```sh
cd releases
sha256sum -c SHA256SUMS
```
