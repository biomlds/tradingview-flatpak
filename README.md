# TradingView Flatpak (Self-Built, Signed, Automated)

This repository automatically builds a Flatpak version of TradingView from the **official signed APT repository**:

https://tvd-packages.tradingview.com

It is designed for Fedora Silverblue (and other immutable systems) where Flatpak is the preferred application format.

---

## Security Model

This project follows a strict supply-chain verification process:

1. The official TradingView APT GPG key is downloaded.
2. The key fingerprint is verified against a pinned expected fingerprint.
3. APT verifies:
   - Release file signature
   - Package hashes
4. The verified `.deb` is downloaded via `apt download`.
5. A Flatpak is built deterministically from the verified package.
6. The Flatpak repository is signed with a custom GPG key.
7. The signed repository is published via GitHub Pages.

Trust chain:

TradingView signed APT repo  
→ APT verification  
→ CI reproducible build  
→ Custom GPG signing  
→ Fedora Silverblue Flatpak verification  

No packages are downloaded from arbitrary URLs.  
No binaries are scraped from web pages.

---

## Automation

A GitHub Actions workflow:

- Runs daily
- Detects new TradingView releases
- Builds only when a new version appears
- Signs the Flatpak repository
- Publishes to the `gh-pages` branch

The last built version is stored in the `VERSION` file.

---

## Install on Fedora Silverblue

Add the Flatpak remote:

```bash
flatpak remote-add mytv https://USERNAME.github.io/tradingview-flatpak