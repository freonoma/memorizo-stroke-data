# Memorizo stroke data

Stroke order and stroke shapes for 6,617 Japanese characters (184 kana, 6,431 kanji, 々 and 〆) in one SQLite file, `strokes-ja.db` (3.1 MB). It is the data behind the writing practice in Memorizo, a flashcard app for language learners that isn't released yet. When it's live, I will link it here.

## Source and credit

Adapted from [KanjiVG](https://kanjivg.tagaini.net), copyright (C) 2009-2013 Ulrich Apel, release r20260714, licensed under [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/).

## What changed from KanjiVG

- Only kana and kanji are kept. KanjiVG's Latin letters, digits, punctuation and radical forms are left out.
- Each stroke path is rewritten as absolute cubic Bézier curves in KanjiVG's 109 by 109 box, with coordinates rounded to a tenth of a unit and stored as 16-bit integers.
- Element groups, radicals and stroke number positions are left out. KanjiVG's stroke types are kept.

## License

This adaptation is licensed under [Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/) (see LICENSE). If you share it or something made from it, credit KanjiVG and Ulrich Apel as above and share it under the same license.

## Format

Two tables:

- `meta (key, value)`: the source file and its SHA-256, the licenses, the credit, the format version (`1`), the box size (`109`) and the number of characters.
- `stroke (char, lang, count, types, data)`: one row per character and language (`ja` for all of them), with its number of strokes.
  - `types`: KanjiVG's stroke type for each stroke (such as `㇐` or `㇆/㇚`), separated by spaces, or NULL where KanjiVG has none (kana).
  - `data`: byte 0 is the format version (1) and byte 1 the number of strokes. Then, for each stroke in writing order: one byte with its number of curve segments, then its start point, then each segment's first control point, second control point and end point. Each point is two little-endian signed 16-bit integers, x then y, in tenths of a unit (divide by 10 for KanjiVG's coordinates).

## Contact

I will add a contact email here once it's launched.
