# rules/ — species and community rules for the Critical Zone Engine

One JSON file per species or community (docs/critical-zone.md §3). A rule is data: a naturalist writes it, `tools/cze/grow.py`
runs it against the derived fields (`tools/cze/derive.py`) and bakes a cover grid the sim reads.

```
{
  "id": "xanthoria",  "name": "...",  "kind": "lichen | moss | alga | plant | tree | sessile | community",
  "affinity": {                       // product of response curves, 0..1; the species cannot exist where it is 0
    "<field>": [[x, y], ...],          // piecewise-linear response to a derived field (0..1 fields, m for height)
    "substrate": { "<class>": weight } // per substrate class (derive.py substrate_classes), missing = 0
  },
  "seed": "paint" | "natural",         // propagules only where painted (authored brushes kind "paint"), or by affinity
  "spread": { "rate": per step, "reach_m": metres, "steps": N, "min_affinity": 0.2 },
  "form": "rosette | cushion | blanket | canopy | meadow | band",
  "represent": { "cover": "cover_<id>.png", "shader": "<how the sim draws it>" },
  "sources": [ { "cite": "...", "note": "what in the rule rests on it" } ]
}
```
Curves are over the field's stored range (0..1 for sun/wet/salt/slope). `height_m` is the NAVD height. Keep the sources
honest: a rule that cites nothing is a guess and says so in `"confidence"`.

## Generator rules (`"kind": "generator"`, 20 Oct; docs/coast-generator.md)

A generator rule says where the coast generator lays one of Matt's brushes and how. Same curve format as above.

```
{
  "id": "beach_stones", "name": "...", "kind": "generator",
  "paints": "ledge | fill | weed | trees | paint",      // which brush it lays (the existing brush formats)
  "plain": "one or two sentences in plain terms: what grows or lies where, and why",
  "where": { "<field>": [[x, y], ...], "substrate": { "<class>": weight } },   // 0..1 suitability, a product, as "affinity"
  "params": { "<brush param>": number | text | { "field": "<field>", "curve": [[x, y], ...] } },   // a brush setting, fixed or following a field
  "seed": "natural", "confidence": "...", "sources": [ { "cite": "...", "note": "..." } ]
}
```

Fields: `sun`, `wet`, `salt`, `slope` (0..1 as stored by derive.py), `substrate` (classes), `fetch_m` (open water to windward,
metres), `height_mllw_m` (height above mean lower low water at Bar Harbor, metres: MHW ≈ 3.3, MHHW ≈ 3.5), `height_m` (NAVD 88).
`rules/index.json` lists every file. The editor's Rules panel (Vegetation → Rules) edits any rule in plain terms and saves the
change as `authored.json` → `rules: { <id>: <whole rule> }`, which the generator reads over the file; Matt's corrections reach
the files when he wants them to (the Publisher or the Editor copies them in).

The six first generator rules (`ledge`, `beach_stones`, `rockweed`, `kelp`, `black_lichen`, `spruce_fringe`) are drafts from
the plan's one-line descriptions: their numbers are placeholders, and each says so in `confidence`.
