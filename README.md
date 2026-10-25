# Unofficial paint.rs packages

Local Flatpak and strict Snap packaging for [paint.rs](https://github.com/daijro/paint.rs) **v0.1.3**. This project repackages the upstream x86_64 binary; it does not contain the application source. Upstream is GPL-3.0-or-later; see `UPSTREAM-LICENSE`, `UPSTREAM-LICENSE.md`, and `UPSTREAM-NOTICE.md` for bundled upstream notices. The packaging repository itself retains the MIT license from its initial setup.

The upstream release states that Linux builds need glibc 2.35 or newer. The provided binary's ELF version requirements were inspected and include GLIBC_2.35. Flatpak targets Freedesktop 26.08, and the Snap uses core24; actual runtime verification remains necessary on target systems.

The release's plain x86_64 build is used (not the AVX2-specific variant), so it does not impose the AVX2 CPU requirement. No ARM build is included.

## Flatpak

The Flatpak uses `org.freedesktop.Platform` 26.08. It grants network access, Wayland/X11 and GPU access, and direct access to Desktop, Documents, and Downloads for file-picker compatibility.

```sh
cd flatpak
flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak install --user flathub org.freedesktop.Sdk//26.08 org.freedesktop.Platform//26.08
flatpak-builder --user --install --force-clean build-dir io.github.daijro.paintrs.yml
flatpak run io.github.daijro.paintrs
flatpak-builder --repo=repo --force-clean build-dir io.github.daijro.paintrs.yml
flatpak build-bundle repo ../releases/paintrs-0.1.3-x86_64.flatpak io.github.daijro.paintrs master
```

## Snap

The Snap uses core24 and strict confinement. The `home` interface permits direct access to ordinary non-hidden files under the user's home; `removable-media` is declared but requires a manual connection.

```sh
cd snap
snapcraft pack --use-lxd
```

Install a local build only after reviewing its permissions:

```sh
sudo snap install --dangerous paintrs_0.1.3_amd64.snap
```

`--dangerous` accepts a locally built unsigned artifact; it does not disable strict confinement.

## Checksums

When artifact builds complete, record and check the local checksums from `releases/` with `sha256sum -c SHA256SUMS`.
