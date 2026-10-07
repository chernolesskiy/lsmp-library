# AGENTS.md: working with lsmp-library

Written for an LLM agent working on a **copy** of this library. Never edit the original in place.
Keep the credits and the licence (see LICENSE) when you reuse anything. Do not try to find or restore
pupils' names; they were removed deliberately.

## Layout

```
README.md  AGENTS.md  LICENSE  index.json
content/
  chess-com-collections/        76 .pgn files (399 games), sources.md, mapping.json
  lichess-exemplar-sheets/
    levels/<level>/             <level>-<YYYY-MM-DD>.pdf + .json   (2 dates x 4 levels)
    themes/<theme>/             <theme>-<YYYY-MM-DD>.pdf + .json
    tasters/<name>/             <name>.pdf + .json
```
12 PDFs, 12 JSON puzzle files. Release 1, 2026-10-07. Text files are UTF-8, LF line endings.

## index.json

Top level: `name`, `schema` (2), `generated`, `generator`, `item_count`, `items` (list).
Every item has: `id` (unique, stable slug), `title` (unique), `path` (relative to the export root),
`type`, `count` + `count_unit`, `licence`, `sources` (list of credit strings), `size_bytes`,
`sha256`, and `date` where one is known (ISO `YYYY-MM-DD`, else null).

| `type` | Extra fields |
|---|---|
| `pgn-collection` | `count_unit` "games"; `has_comments`, `has_variations`, `has_arrows`, `has_highlights`, `has_clock` (true if any game has them; `has_comments` means written text, not just markers); `games_with_fen` (games that start from a given position, mostly puzzles) |
| `pdf-sheet` | `count_unit` "puzzles"; `group` {kind: levels\|themes\|tasters, name}; `themes` (page headings); `data` (id of its JSON item); `has_hints` (hint text printed on the sheet, read from the PDF) |
| `puzzle-data` | `count_unit` "puzzles"; `group`; `themes`; `sheet` (id of its PDF); `has_hints` (hint text is in *this file*); `hints_on_sheet`; `hints_note` when the file holds only a selection. The taster data files are selections without hint text; their PDFs print the hints. |
| `sources`, `mapping`, `readme`, `license`, `agents-guide` | `count` is the number of entries where it makes sense |

`licence` values: `CC-BY-NC-4.0` (LSMP's own material), `CC0-1.0`, `CC0-1.0 + CC-BY-NC-4.0` (Lichess CC0
puzzle data combined with LSMP hints, selection and layout). Third-party material inside the collections
keeps its owners' rights (see LICENSE). Verify a file with its `sha256`.

## Finding material

**By player.** The index has no player field. Games are PGN: read the `[White "..."]` and `[Black "..."]`
tags. Famous players appear with full names; online games use chess.com usernames. Several collections are
named after one player, but a player can appear in any collection, so search the tags:

```python
import chess.pgn, glob
for path in sorted(glob.glob("content/chess-com-collections/*.pgn")):
    with open(path, encoding="utf-8") as fh:
        while (g := chess.pgn.read_game(fh)):
            if "Lasker" in g.headers.get("White", "") + g.headers.get("Black", ""):
                print(path, g.headers["White"], g.headers["Black"], g.headers.get("Date"))
```

**By theme.** For puzzle sheets use the index: `group.kind == "themes"` with `group.name`, and `themes`
(page headings such as "Tactics: Forks"); levels are `group.kind == "levels"` (rating bands in the name).
The JSON has `pages[].title`, `pages[].description` and `pages[].puzzles[]` (`puzzle_number` is the Lichess
puzzle id, `rating`, `hint`, `hint_tier`). For game collections there is no theme field: use the `title`
(many are themed, e.g. endgames, checkmates, blunders), the PGN `Opening` and `ECO` tags,
`StudyName`/`ChapterName`, and the comment text.

**By date.** Index `date` is the lesson date for the dated collections (null for the others) and is also
in the file name prefix `YYYY-MM-DD-`. Per game, the PGN `Date` tag is the game's own date (`YYYY.MM.DD`,
unknown parts are `?`), and `UTCDate` is when a study chapter was made. Sheets carry their date in the file
name and the JSON `created` field (themes and levels only).

## Things to know before you edit a copy

- Written comments are the text inside `{ }` after you remove `[%...]` commands. Keep `[%clk]` (clock),
  `[%cal]`/`[%c_arrow]` (arrows), `[%csl]`/`[%c_highlight]` (highlights) and `[%timestamp]` when you rewrite
  a PGN; chess.com's `[%c_effect]` markers were already removed.
- Do not reorder or renumber puzzles in a sheet's JSON without regenerating its PDF; they must match.
- `mapping.json` maps each file back to its original name and SHA-256. One entry is withheld on purpose
  (`original_masked: true`); leave it that way.
- Keep the per-game `Source`/`Annotator`/`StudyName`/`ChapterURL`/`GameURL` tags and `sources.md` entries
  with the material they belong to.
- Titles must stay unique and free of pupils' names. If you add an item, give it a distinct
  title, a `licence` and a `sha256`.
