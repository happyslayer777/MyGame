# Anime Girl Heaven — project context

Adult (18+) browser game. Single-file app: `index.html` (outputTemplate),
`main.pjs` (modelText). Perchance generator, deployed from this repo.

## Hard rules
- Everything in the game is adult. Never add "adult" qualifiers to prompts.
- Prompt constants and negatives are TUNED. Preserve them byte-for-byte unless
  the task is explicitly about them. Do not "improve", reorder, or paraphrase.
- Never summarize a prompt constant into a comment. Comments may describe,
  the constant itself stays literal.
- Preserve existing formatting and indentation exactly. No pretty-printing the
  whole file.
- Output unified diffs or surgical replacements. Not rewritten functions.
- One system per session. Stop after the task.

## Vocabulary — do not rename these
puffy, mound, seal, cleft, labia, gape, nub, hoodie lock, style lock,
identity lock, director, painter, assembly, CLOTHED, PARTIAL, NUDE, CLOSEUP,
heap, edge, ruin, allow, snaps, canon, crowned, traits

## Read first
`src/README.md` — full design doc for every system. Trust it over inference.

## Architecture
CSS 1–700, HTML shell 700–1000, Script 1 engine, Script 2 groups add-on,
Script 3 UI extras, Script 4 agency add-on. Add-ons wrap the engine; prefer
a new add-on over editing Script 1.

## Never
- Rename state keys or storage folders (`root.chatSave`, `c_<id>`).
- Touch the `NEG_*` blocks without being asked.
- Rewrite working pipeline stages.
