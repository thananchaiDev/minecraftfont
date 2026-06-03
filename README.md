# fontfix — Custom Thai font resource pack

Fork ของ [ThaiFontFix](https://modrinth.com/resourcepack/thaifontfix) (max180643)

- **ภาษาไทย** → ฟอนต์ **Google Sans** (subset เฉพาะบล็อกไทย U+0E01–0E5B + ZWJ/ZWNJ)
- **ภาษาอังกฤษ/Latin** → ใช้ฟอนต์ pixel **vanilla ของ Minecraft** (ไม่ถูกแตะ)

## วิธีทำงาน

`assets/minecraft/font/default.json` แทรก TTF provider 1 ตัวเข้าไปใน font `minecraft:default`
ของเกม Minecraft จะ **merge** providers กับ vanilla และ provider ที่เติมทีหลังมี priority สูงกว่า
สำหรับ glyph ที่มันมี เนื่องจากฟอนต์ subset ไว้ให้มี **เฉพาะ glyph ไทย** → จึง override เฉพาะ
ตัวอักษรไทย ส่วน Latin ยังตกไปที่ bitmap ascii ของ vanilla

สระบน/ล่าง/วรรณยุกต์ของ Google Sans Thai ถูกวาดด้วยตำแหน่งแนวตั้งที่ถูกต้องใน glyph outline เอง
(advance = 0, xMin ติดลบเพื่อถอยกลับไปทับพยัญชนะหน้า) จึงไม่ลอยใน Minecraft ซึ่งไม่ทำ
OpenType GPOS shaping

## โครงสร้าง

```
pack/
  pack.mcmeta                              # pack_format 34, supported 34–84 (รองรับ MC 26.1.2)
  pack.png                                 # ไอคอน (จาก ThaiFontFix)
  assets/minecraft/font/default.json       # TTF provider
  assets/minecraft/font/thaifontfix.ttf    # Google Sans subset ไทย (24 KB)
ThaiFontFix-GoogleSans.zip                 # pack ที่ build แล้ว (พร้อมอัปโหลด)
src/                                       # ไฟล์ฟอนต์ต้นทาง (ไม่อยู่ใน zip)
```

## Build ใหม่

```powershell
# 1) subset Google Sans Thai (woff2 จาก Google Fonts) -> TTF เฉพาะไทย
python -m fontTools.subset src/GoogleSans-thai.ttf `
  --unicodes="U+0E01-0E5B,U+200C-200D" --layout-features="*" --glyph-names `
  --no-prune-unicode-ranges --output-file="pack/assets/minecraft/font/thaifontfix.ttf"

# 2) zip (ต้องใช้ forward slash ใน entry — ห้ามใช้ Compress-Archive ของ PowerShell)
python -c "import zipfile,os; z=zipfile.ZipFile('ThaiFontFix-GoogleSans.zip','w',zipfile.ZIP_DEFLATED,compresslevel=9); [z.write(os.path.join(d,f), os.path.relpath(os.path.join(d,f),'pack').replace(os.sep,'/')) for d,_,fs in os.walk('pack') for f in fs]; z.close()"
```

## การติดตั้งบน server

`server/server.properties` ต้องชี้ `resource-pack` ไปที่ **URL สาธารณะ** ของ zip นี้ พร้อม sha1:

```
require-resource-pack=true
resource-pack=<public-url>/ThaiFontFix-GoogleSans.zip
resource-pack-sha1=abd07edcc6a432d4d63b46b4b25bd784f8f2a257
```

> sha1 จะคงเดิมตราบใดที่ไม่ build zip ใหม่ (ถ้า build ใหม่ต้องคำนวณ sha1 ใหม่)

## ⚠️ License

"Google Sans" เป็น brand font ของ Google (ไม่ใช่ OFL เหมือนฟอนต์ทั่วไปบน Google Fonts)
การแจกจ่ายฟอนต์นี้ในแพ็คสาธารณะอาจติดข้อจำกัดด้านลิขสิทธิ์ — ใช้ในวงจำกัด/ส่วนตัวควรพิจารณาเอง
