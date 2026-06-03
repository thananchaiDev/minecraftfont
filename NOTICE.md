# Attribution & Licensing

This resource pack is a **fork of [ThaiFontFix](https://modrinth.com/resourcepack/thaifontfix)**
by *max180643* (HewkawAr/ThaiFontFix), which fixes floating Thai characters in Minecraft Java Edition.

## What changed in this fork
- The bundled Thai font was replaced with **Google Sans** (Thai subset, U+0E01–0E5B).
- Latin/English glyphs are intentionally **not** included, so English text falls back to
  Minecraft's built-in (vanilla) bitmap font.

## ⚠️ Font license notice
The bundled font `assets/minecraft/font/thaifontfix.ttf` is a **subset of "Google Sans"**,
a proprietary brand typeface owned by Google. Unlike most fonts on Google Fonts, Google Sans
is **not** released under the SIL Open Font License (OFL). Redistribution of the font binary
(which a resource pack does) may not be permitted by Google's license.

This repository ships it at the maintainer's own discretion and risk. If you need a fully
license-clean alternative, rebuild the pack with an OFL Thai font such as **Noto Sans Thai**,
**IBM Plex Sans Thai**, or **Sarabun** (see `README.md` for the build steps — only the source
font URL changes).
