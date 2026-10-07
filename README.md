# lsmp-library

Chess teaching material from LSMP: 76 collections of annotated games (399 games) and
12 printable puzzle homework sheets built from the Lichess puzzle database. Release 1, 2026-10-07.
This repository holds content only.

## What is in it

- `content/chess-com-collections/`: 76 PGN files. Many games have written comments, variations,
  arrows, highlighted squares and clock times. Also here: `sources.md` (credits) and `mapping.json`
  (original file names and hashes; one name is withheld on purpose).
- `content/lichess-exemplar-sheets/`: the puzzle sheets, as 12 PDFs with 12 matching
  puzzle data files (JSON).
  - `levels/`: four rating bands (400-899, 900-1299, 1300-1699, 1700+), two weeks each.
  - `themes/`: two themed sets (forks and pins; skewers and discovered attacks).
  - `tasters/`: two short taster sheets.
  Each sheet has 12 puzzles with a hint under each; the answers are on the back.
- `index.json`: every file with its title, counts, licence and credits (for programs).
- `AGENTS.md`: a guide for an AI agent working on a copy.

## Using the games

Download any `.pgn` file. To read the games with a chess program, open the file in it. To study them online:
- Lichess: Study > add a chapter > Import from PGN (or paste the PGN text), or use the Analysis board's PGN import.
- chess.com: Analysis > open the PGN / Upload PGN option and paste or choose the file.

## Credits and licence

Credits for Lichess, chess.com and the other third-party items are in `content/chess-com-collections/sources.md`;
per-game credits stay in the PGN tags. LSMP's own material is CC BY-NC 4.0 (copyright 2026 LSMP);
Lichess puzzle data is CC0. Details are in `LICENSE`.
