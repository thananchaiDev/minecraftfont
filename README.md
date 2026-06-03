# minecraftfont — Custom Thai font resource pack

A Minecraft resource pack that renders **Thai text with Prompt** while leaving
**English/Latin text in Minecraft's built-in (vanilla) bitmap font**.

Fork of [ThaiFontFix](https://modrinth.com/resourcepack/thaifontfix) by max180643.

## How it works

`assets/minecraft/font/default.json` adds a single TTF provider to the game's
`minecraft:default` font. Minecraft **merges** font providers across packs, and a provider
added by a pack takes priority over vanilla for the glyphs it contains. Because the bundled
font is subset to **Thai glyphs only**, it overrides just Thai characters — Latin text falls
back to the vanilla `ascii` bitmap font.

Prompt's Thai vowels and tone marks are drawn at the correct vertical position inside
the glyph outlines themselves (advance = 0, negative xMin so the mark sits over the preceding
consonant), so they render correctly in Minecraft even though it does not perform OpenType
GPOS shaping.

## Structure

```
pack/
  pack.mcmeta                              # pack_format 34, supported 34-84 (covers MC 26.1.2)
  pack.png                                 # icon
  assets/minecraft/font/default.json       # TTF provider
  assets/minecraft/font/thaifontfix.ttf    # Prompt (OFL), Thai subset (~34 KB)
ThaiFontFix-GoogleSans.zip                 # built pack (attached to the GitHub release)
src/                                       # source fonts (not committed / not in the zip)
```

## Rebuilding

```powershell
# 1) Subset Prompt Thai (woff2 from Google Fonts) to a Thai-only TTF
python -m fontTools.subset src/Prompt-thai-full.ttf `
  --unicodes="U+0E01-0E5B,U+200C-200D" --layout-features="*" --glyph-names `
  --no-prune-unicode-ranges --output-file="pack/assets/minecraft/font/thaifontfix.ttf"

# 2) Zip with forward-slash entries (do NOT use PowerShell Compress-Archive, which writes
#    backslashes that Minecraft cannot resolve). pack.mcmeta must sit at the zip root.
python -c "import zipfile,os; z=zipfile.ZipFile('ThaiFontFix-GoogleSans.zip','w',zipfile.ZIP_DEFLATED,compresslevel=9); [z.write(os.path.join(d,f), os.path.relpath(os.path.join(d,f),'pack').replace(os.sep,'/')) for d,_,fs in os.walk('pack') for f in fs]; z.close()"
```

## Installing on a server

Point `server/server.properties` at the public release URL with its SHA-1:

```
require-resource-pack=true
resource-pack=https://github.com/thananchaiDev/minecraftfont/releases/download/v1.0.0/ThaiFontFix-GoogleSans.zip
resource-pack-sha1=7627f3852690cfae050893506b17cec874a0495c
```

> The SHA-1 stays the same as long as the zip is not rebuilt. If you rebuild it, recompute the
> SHA-1 and upload a new release.

Download: <https://github.com/thananchaiDev/minecraftfont/releases/latest>
