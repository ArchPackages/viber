# AGENTS.md

## Supply-chain checks for AUR updates

`viber` is an AUR fork (`upstream` is `https://aur.archlinux.org/viber.git`).
Before importing an upstream update or pushing packaging changes:

- Review the AUR git history and diffs, not just the latest snapshot. Treat
  unexplained changes to source URLs, checksums, launchers or `prepare()` /
  `package()` logic as a red flag.
- Confirm every source URL points at Viber's official hosts:
  `download.cdn.viber.com` for the deb and `www.viber.com` for the Terms PDF.
  The official download page is <https://www.viber.com/en/download/>.
- Download the deb independently from the official download page and inspect
  its `control` metadata, file list and Debian maintainer scripts. Viber does
  not publish detached checksums or signatures, so do not accept a new AUR
  checksum without this inspection.
- Inspect `viber.sh.in` and any other non-PKGBUILD files for shell hooks,
  network access, obfuscation or unexpected executables.
- Never accept new AUR checksums without repeating the vendor verification.
- Record what was verified in the commit message and update this README.

## Packaging notes

- `pkgver` tracks the `Version:` field in the deb's `control` file.
- The deb URL is unversioned, so the local source name includes `$pkgver`.
  Never remove the checksum: a vendor update must fail the build until the
  package is reviewed and the version and checksums are updated.
- Viber bundles Qt 6, ICU, OpenSSL, curl and FFmpeg under `/opt/viber/lib`.
  Do not add `qt6-multimedia` or similar system Qt dependencies without
  checking the current deb's ELF `NEEDED` entries first.
- Keep the bundled libraries unstripped and unmodified; `options=('!strip')`
  is intentional.
- Bump `pkgrel` for every packaging change and regenerate `.SRCINFO` with
  `makepkg --printsrcinfo > .SRCINFO`.
