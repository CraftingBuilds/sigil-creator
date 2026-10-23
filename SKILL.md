---
name: sigil-creator
description: Create personal magical sigils from user intentions using methods from classical grimoires and modern chaos magic — Austin Osman Spare letter reduction, Agrippa planetary kameas, Golden Dawn Rose Cross, and grimoire-style seals. Supports Sigil Profiles (visual styles) such as Antiquarian Calligraphic. For every sigil also emit JSON metadata and a Markdown grimoire entry. Trigger on requests to create a sigil, generate sigil for an intent, design a magical symbol, make a chaos magic glyph, or similar occult symbol work. Includes charging and activation guidance.
---

# Sigil Creator

Create authentic-feeling personal sigils grounded in historical Western occult traditions and chaos magic practice. Output a clear design description, optional generated image, reduction steps, and practical charging instructions. Never claim the result is a historically authentic spirit seal from a specific grimoire unless it is a known public-domain form; distinguish personal creation from traditional seals.

## Core Workflow

1. **Elicit and refine the Statement of Intent (SoI)**  
   - Present tense, positive, specific, already-true framing ("I am financially secure" or "It is my will to find steady creative work I enjoy").  
   - Reject or rephrase negatives, vague wishes, and future tense.  
   - Confirm the final SoI with the user before proceeding if ambiguous.

2. **Select method** (ask user preference or recommend by context)  
   - **Spare / Chaos letter method** — default for most personal desires; fast, creative, psychological model.  
   - **Planetary kamea method** — when planetary correspondence fits (protection→Saturn, love→Venus, wealth→Jupiter, etc.); more structured and traditional.  
   - **Rose Cross method** — ceremonial / Golden Dawn flavor; good for names, short phrases, or Qabalistic work.  
   - **Grimoire-style composite** — angular seals combining geometry, planetary symbols, and reduced letters for a classical look.  
   - Hybrid or freehand artistic if user requests.

3. **Select Sigil Profile** (visual style)  
   - Default: `antiquarian_calligraphic` (bold archaic calligraphic, black ink on ivory parchment).  
   - Load the matching YAML from `profiles/` and apply its aesthetic, stroke, curve, geometry, lettering, composition, and arrow rules when describing or rendering the sigil.  
   - Profiles are independent of method; any method can be rendered in any profile.

4. **Generate the sigil**  
   - Perform letter/number reduction using the scripts in `scripts/` when helpful (`reduce_letters.py`, `letters_to_numbers.py`).  
   - For Spare: strip vowels (optional) + unique letters → abstract combined glyph. Describe the composition precisely (which strokes share lines, rotations, overall silhouette).  
   - For kamea: choose planet → convert reduced letters to numbers (A=1…Z=26, digital-root reduce to fit square) → path across the square from `references/kameas.md`. Start path with open circle, end with bar.  
   - For Rose Cross: sequence letters on the 22-petal rose and connect; note that exact petal layout is approximate unless a reference image is available.  
   - Always produce a textual description of the final form that is detailed enough for the user (or image model) to redraw it, written in the language of the chosen profile.

5. **Visualize**  
   - Prefer generating an image via the available image-generation capability.  
   - Craft the prompt strictly from the active Sigil Profile: use its `aesthetic.finish`, `canvas` / `background` description, `stroke` character & weights, `curves`, `geometry`, `lettering`, `composition`, and optional `arrow`. Respect the profile’s “avoid” list.  
   - Center the mark, high contrast, no text labels unless requested.  
   - Offer SVG-style path description or ASCII art as fallback when image gen is unavailable or undesired.  
   - Do not output copyrighted grimoire plates; generate original designs only.

6. **Provide charging and release instructions**  
   - Summarize gnosis methods, gazing, physical destruction or concealment, and the deliberate forgetting step.  
   - Note planetary timing or lunar phase when relevant to the chosen method.  
   - Keep psychological framing available (subconscious encoding) alongside traditional magical framing; let the user choose the model.

7. **Write companion files**  
   - Always produce a Markdown grimoire entry (`.md`) and JSON metadata (`.json`) for the finished sigil (see Output Format).  
   - Place them in the working directory (prefer `artifacts/` or a dedicated `output/` folder).  
   - Use a stable filename slug derived from the SoI + date/id so the pair can be found later.

8. **Safety and framing**  
   - Treat the practice as psychological technology, artistic practice, or sincere occult work according to the user’s expressed belief.  
   - Do not promise results, diagnose, or encourage harm.  
   - For spirit seals or Goetic work, clarify that authentic seals come from source texts and this skill creates personal analogs only.

## Sigil Profiles (Style & Aesthetic)

Sigil Profiles define the visual language, stroke character, composition rules, and rendering guidelines for the finished sigil. They sit orthogonal to the *method* (Spare, kamea, Rose Cross, etc.): the method determines the underlying geometry or letter reduction; the profile determines how that geometry is rendered.

**Current profiles** (load from `profiles/`):

| Profile ID                    | Name                          | Character                                      | Default use case                          |
|-------------------------------|-------------------------------|------------------------------------------------|-------------------------------------------|
| `antiquarian_calligraphic`    | Antiquarian Calligraphic Sigil| Bold, archaic, interwoven, calligraphic        | Default classical / grimoire look         |
| `grimoire_engraved`           | Grimoire Engraved Sigil       | Severe, angular, authoritative, plate-like     | Solomonic / Goetic / ceremonial seals     |
| `chaos_minimal`               | Chaos Minimal Sigil           | Clean, abstract, psychological, decisive       | Modern Spare / chaos magic aesthetic      |
| `alchemical_manuscript`       | Alchemical Manuscript Sigil   | Ornate yet restrained, symbolic, manuscript    | Alchemical / illuminated manuscript feel  |

### How to apply a profile

1. Ask the user if they have a preferred style, or recommend one (default: `antiquarian_calligraphic`).
2. Load the corresponding YAML from `profiles/<id>.yaml`.
3. When crafting the **image-generation prompt** or the **detailed visual description**, incorporate the profile’s:
   - `aesthetic` (influences, character, finish)
   - `stroke` (weights, terminals, pressure, color)
   - `curves` & `geometry` (loop styles, axes, negative space)
   - `lettering` & `composition` rules
   - `arrow` settings if the profile enables a directional arrow
   - `vector_guidelines` and “avoid” list
4. Keep the canvas description consistent with the profile (`canvas` + `background` sections).
5. Do **not** invent new profile parameters; stay faithful to the loaded YAML.

Future profiles can be added simply by dropping a new YAML into `profiles/` and listing it in the table above.

## Resource Map

- `profiles/` — visual style definitions (YAML): `antiquarian_calligraphic`, `grimoire_engraved`, `chaos_minimal`, `alchemical_manuscript`.
- `references/methods.md` — detailed steps for each tradition, charging notes, historical context.  
- `references/kameas.md` — exact Agrippa planetary squares and reduction rules.  
- `scripts/reduce_letters.py` — CLI helper for Spare-style unique-letter extraction.  
- `scripts/letters_to_numbers.py` — convert letters to kamea path numbers.

Load references and profiles on demand. Keep SKILL.md itself concise; expand only when the active method or profile requires it.

## Output Format

Structure every chat response:
- Confirmed SoI (and note if it was normalized from plain speech)
- Method chosen and brief rationale
- Sigil Profile applied (name + one-line character)
- Reduction steps (letters or numbers)
- Detailed visual description of the finished sigil (written in the language of the active profile)
- Generated image (if produced)
- Charging / activation / forgetting guidance
- Optional variations (different method or different profile)

**Always** also write two companion files for every completed sigil (use the sandbox / working directory; prefer `artifacts/` or a per-session `output/` folder). Name them with a short slug derived from the SoI plus a timestamp or sequential id, e.g. `focus-calm-20261020-01.md` and matching `.json`.

### 1. Markdown grimoire entry (`.md`)

Write a self-contained grimoire log sheet containing at minimum:

```markdown
# Sigil Grimoire Entry

**Date:** YYYY-MM-DD  
**Statement of Intent:** "…"  
**Normalized from:** (original plain speech, if different)

## Method & Profile
- Method: Spare / Kamea (planet) / Rose Cross / Grimoire-style / Hybrid
- Profile: `profile_id` — short character description
- Rationale: one or two sentences

## Reduction
- Original letters / numbers sequence
- Unique reduced form
- Any notes on vowel removal, digital-root reduction, or path construction

## Visual Description
(Full profile-aware description of the finished mark — same text used for image generation)

## Charging & Activation
- Recommended gnosis methods
- Planetary day/hour or lunar phase (if relevant)
- Physical handling (draw, burn, bury, carry, destroy)
- Deliberate forgetting / banishing step

## Release & Disposal
How and when to release the sigil from conscious attention.

## Notes
Optional variations, user preferences, or follow-up actions.
```

### 2. JSON metadata (`.json`)

Write a machine-readable companion file with this schema (extend only with additional optional fields; never remove required ones):

```json
{
  "schema_version": "1.0",
  "created": "ISO-8601 datetime",
  "statement_of_intent": "exact final SoI string",
  "original_request": "user’s plain-speech request if different",
  "method": "spare | kamea | rose_cross | grimoire | hybrid",
  "planet": "saturn | jupiter | mars | sun | venus | mercury | moon | null",
  "profile_id": "antiquarian_calligraphic | grimoire_engraved | chaos_minimal | alchemical_manuscript | …",
  "profile_name": "human-readable profile name",
  "reduction": {
    "raw_letters": "…",
    "unique_letters": "…",
    "numbers": [1, 2, 3],
    "notes": "optional free text"
  },
  "visual_description": "the detailed profile-aware description",
  "charging": {
    "gnosis_methods": ["gazing", "mantra", "…"],
    "timing": "planetary day/hour or lunar note, or null",
    "disposal": "burn | bury | carry | destroy | conceal"
  },
  "image_prompt": "exact prompt used for image generation (if any)",
  "files": {
    "markdown": "filename.md",
    "json": "filename.json",
    "image": "optional image filename or null"
  }
}
```

When an SVG or other vector is also produced, list it under `files` as well. Keep the JSON strictly valid and UTF-8.

### Plain-speech → Statement of Intent

Accept natural language (“I want to be focused and calm”) and rewrite it into a proper present-tense, positive Statement of Intent (“I am focused and calm”). Confirm the rewritten form with the user when the change is significant.

Iterate with the user: refine design, try another method or profile, or adjust style until satisfied. After the final version is accepted, write (or update) the `.md` and `.json` files.
