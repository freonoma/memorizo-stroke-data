# Memorizo stroke data

Stroke order and stroke shapes for Japanese and Chinese characters, one SQLite file per language. They are the data behind the writing practice in Memorizo, a flashcard app for language learners that isn't released yet. When it's live, I will link it here.

| Folder | File | Characters | Source | License |
| --- | --- | --- | --- | --- |
| `ja/` | `strokes-ja.db` (3.1 MB) | 6,617: 184 kana, 6,431 kanji, 々 and 〆 | KanjiVG r20260714 | CC BY-SA 4.0 (`ja/LICENSE`) |
| `zh/` | `strokes-zh.db` (2.8 MB) | 9,565 simplified and traditional hanzi, in mainland (PRC) stroke order | Make Me a Hanzi `graphics.txt` | Arphic Public License (`zh/ARPHICPL.TXT`) |

Each file is under its own license, which is in its folder.

## Japanese: `ja/strokes-ja.db`

Adapted from [KanjiVG](https://kanjivg.tagaini.net), copyright (C) 2009-2013 Ulrich Apel, release r20260714, licensed under [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/).

What changed from KanjiVG:

- Only kana and kanji are kept. KanjiVG's Latin letters, digits, punctuation and radical forms are left out.
- Each stroke path is rewritten as absolute cubic Bézier curves in KanjiVG's 109 by 109 box, with coordinates rounded to a tenth of a unit and stored as 16-bit integers.
- Element groups, radicals and stroke number positions are left out. KanjiVG's stroke types are kept.

This adaptation is licensed under [Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/) (`ja/LICENSE`). If you share it or something made from it, credit KanjiVG and Ulrich Apel as above and share it under the same license.

## Chinese: `zh/strokes-zh.db`

Adapted from [Make Me a Hanzi](https://github.com/skishore/makemeahanzi) by Shaunak Kishore (`graphics.txt` at commit bddc96d), which is derived from the fonts AR PL KaitiM GB and AR PL UKai, copyright (C) 1999 Arphic Technology Co., Ltd., under the Arphic Public License.

What changed, by Memorizo on 2026-11-18:

- Converted from `graphics.txt` to the SQLite format below. Each stroke is its median line, moved from Make Me a Hanzi's 1024 box (y pointing up) into a 109 by 109 box with y pointing down, coordinates rounded to a tenth of a unit.
- The stroke outlines are left out, and so are the 9 entries outside the CJK ideograph blocks (radical forms).
- Memorizo smooths each median into cubic curves when it reads it, keeping turns sharper than 60 degrees as corners.

This modified version is distributed under the Arphic Public License. Its text is `zh/ARPHICPL.TXT`, unaltered, and is also stored in the file itself (the `license_text` row of `meta`). The same notice of changes is in the file's `changes` row.

## Format

Both files have two tables:

- `meta (key, value)`: the format, the box size (`109`), the source file and its SHA-256, the licenses, the credit, the changes and the number of characters. The Chinese file also holds `license_text`.
- `stroke (char, lang, count, types, data)`: one row per character and language (`ja` or `zh`), with its number of strokes.
  - `types`: KanjiVG's stroke type for each stroke (such as `㇐` or `㇆/㇚`), separated by spaces, or NULL where the source has none (kana, and all Chinese rows).
  - `data`: byte 0 is the format and byte 1 the number of strokes. Then come the strokes in writing order. Every point is two little-endian signed 16-bit integers, x then y, in tenths of a unit (divide by 10).
    - Format 1 (Japanese): for each stroke, one byte with its number of curve segments, then its start point, then each segment's first control point, second control point and end point.
    - Format 2 (Chinese): for each stroke, one byte with its number of points, then the points of its median line.

## Contact

I will add a contact email here once it's launched.
