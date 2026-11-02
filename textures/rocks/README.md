# textures/rocks — the bedrock set

Eight Poly Haven rocks (CC0), 1024 px each, in one 4096 × 2048 atlas (grid 4 × 2, index = column + 4·row). Same order and
index as `levels/downeast/manifest.json` `rocks`, so every geology class's `tex` still points at the rock it meant.
Built by `tools/assets/rocks.py` from the 4K originals in Matt's `Zodiac Pro 3D/textures/` library; the originals never
enter git.

| i | rock | reads as | used for (geology `tex`) |
|---|---|---|---|
| 0 | marble_rock_01 | pale grey-white, fine black veins, dark lichen blotches | Cadillac Granite |
| 1 | marble_cliff_03 | warm beige-tan ledge, blocky joint sets, iron stain | fine felsic granite / SW Harbor granite / aplite dikes |
| 2 | marble_cliff_06 | pale beige, dense fracture net, grey wash | Somesville, Devonian and state-map granites, unknown |
| 3 | dark_rock | near-black, flat, faint joints | basalt, diabase dikes |
| 4 | dark_rock_02 | dark brown-black, stepped | gabbro and diorite, volcanics |
| 5 | rock_face_04 | dark brown, fractured, pale feldspar specks | Bar Harbor Formation, schist, gneiss |
| 6 | namaqualand_cliff_02 | rust-brown blocky breccia (no height map: flat normal) | shatter zone, Cranberry Island volcanics |
| 7 | lichen_rock | red-brown with grey-green crustose lichen | lichen overlay above the spray zone |

Files:
- `rocks.jpg` — diffuse, **colour as photographed** (the old 2048 × 1024 per-level atlas was grey-balanced for tinting, which
  is why the shore looked grey; the geology tints in the manifest were tuned against that and will need to come down toward
  white for granite classes).
- `rocks_h.jpg` — height, 8-bit, normalised per rock (0 = lowest, 255 = highest).
- `rocks_n.jpg` — tangent-space normal, OpenGL convention, derived from the height map (the Poly Haven EXR normals can't be
  read on the Mac without an EXR library; the derived ones are fine for a 1 m tile seen from a boat).

Scale: a Poly Haven rock tile covers roughly 1–2 m; the terrain shader's two octaves (0.27/m and 0.061/m) put the 1024 px
tile over 3.7 m and 16 m, so 275 px/m up close. That is enough to 30 m or so; closer than that the mesh-level ledge shaping
carries the detail.

## ref/ — reference photos the palettes are sampled from

- `ref/namaqualand_stones_01.jpg` — Matt's reference for Cadillac granite (19 Oct). Poly Haven
  [namaqualand_stones_01](https://polyhaven.com/a/namaqualand_stones_01) diffuse (CC0), a model's UV atlas at 512 px, so it is
  colour reference only, not a tiling texture. `tools/assets/rock_palettes.py` (REFS) takes the granite tops and faces from it:
  pixels with surface detail only (the atlas padding is smeared streaks and is left out), luminance bands p5–25 / p40–60 /
  p75–95 → dark [108, 82, 57], mid [140, 117, 93], light [166, 148, 129].
- `ref/coastal_cliff_04.jpg` — Matt's reference for gabbro-diorite (19 Oct). Poly Haven
  [coastal_cliff_04](https://polyhaven.com/a/coastal_cliff_04) diffuse (CC0), a model's atlas at 512 px. Gabbro-diorite **tops**
  only, detail pixels with the moss left out (green ≥ 0.95 × red and > 1.2 × blue, 13 % of the image): dark [67, 59, 38],
  mid [83, 72, 51], light [100, 88, 66]. The faces stay the Nubble scan (real Maine gabbro-diorite, Cape Neddick), which
  is greyer; the scan's rock is olive-brown, so tops and faces of gabbro now differ in hue. If that reads wrong, sample both
  from one photo with the Ledge panel's eyedropper.
