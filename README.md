# viber — Arch Linux package

Repackaging of the official **Viber for Linux** Debian package for Arch Linux.

Current version: **29.4.0.7** (vendor build dated 2026-11-05).

## Fork provenance

Forked from the AUR package **viber**:

<https://aur.archlinux.org/packages/viber>  
<https://aur.archlinux.org/viber.git>

Originally packaged and maintained by **Sandor Nagy**, later maintained by
**Jason Go** <jasongo@jasongo.net>, with contributions from **Mladen Pejaković**,
**Morealaz**, **Lev Lybin** and **Özgür Sarıer**. Many thanks to them and to the
Viber developers.

This fork is maintained by **Miroslav Pavleski** <miroslav@pavleski.net> and is
published through the [ArchPackages pacman repository](https://archpackages.github.io/archrepo/).

## Changes in this fork

- Updated from 27.3.0.2 to the current vendor build, 29.4.0.7.
- Rebuilt `depends` from the actual ELF `NEEDED` entries instead of inherited
  AUR dependencies. Viber now bundles Qt 6, ICU, OpenSSL, curl and FFmpeg, so
  `qt6-multimedia`, `libxml2-legacy` and `libxslt` are no longer required;
  the remaining system libraries are listed explicitly.
- Replaced the obsolete multimedia/backend `optdepends` with desktop
  integration entries (`gtk3`, `xdg-utils`, `qt6-base`, browsers, audio tools).
- Normalised the 0664/0775 file modes shipped inside the deb to 0644/0755.
- Kept the small `/usr/bin/viber` wrapper that adds `/usr/lib/qt6/bin` to
  `PATH` so `xdg-mime` can find `qtpaths`.

## Supply-chain verification

The 29.4.0.7 update was checked before packaging:

- The complete AUR git history (2015–2026, 35 commits) was reviewed. It has no
  patches, no `.install` file and no build hooks. Every source URL in the
  history points at `download.cdn.viber.com` or `www.viber.com`; the only
  non-PKGBUILD executable file is the five-line `viber.sh.in` launcher.
- The official Linux download page <https://www.viber.com/en/download/>
  links directly to
  <https://download.cdn.viber.com/cdn/desktop/Linux/viber.deb>. The deb was
  downloaded from that URL over verified HTTPS, not from a mirror.
- The deb's `control` metadata reports `Version: 29.4.0.7`, `Architecture:
  amd64`, `Maintainer: Viber Media S.à r.l. <support@viber.com>`, and its
  embedded AppStream metadata reports the same version and a 2026-11-05 build
  date.
- Viber does not publish a detached checksum or signature for this deb. The
  artifact was therefore verified through the official HTTPS download path,
  its internal metadata, its file list, and inspection of its Debian
  maintainer scripts. Those scripts only adjust permissions, refresh the icon
  cache, create a desktop-file symlink and clean stale `/tmp` lock files; they
  do not download or execute anything.
- The archive contains no setuid/setgid files and no symlinks. The only
  executable content is the expected Viber binary, its bundled libraries,
  plugins and helper executables.
- The Viber Terms of Service PDF is fetched from `www.viber.com`; its BLAKE2b
  checksum matches the AUR value.

The package is a proprietary, prebuilt x86_64 binary. Its source code cannot be
audited; only the packaging and the vendor-provided artifact metadata can be
verified.

## Build

```bash
makepkg -f
sudo pacman -U viber-29.4.0.7-1-x86_64.pkg.tar.zst
```

The package installs to `/opt/viber` and provides `/usr/bin/viber`. It is
x86_64 only and is not stripped, because the vendor binary is already stripped
and its bundled libraries must remain untouched.
