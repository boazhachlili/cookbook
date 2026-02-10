# Buziel Cookbook – Data & Workflow

This repository is the **single source of truth** for my personal cookbook:  
recipes, structure, and the rules for how AI assistants (GPTs) should use and extend it.

There are **two layers**:

1. **Data** – `cookbook.json`  
   The actual recipes and metadata.
2. **Rules & workflows** – `README.md` (this file)  
   How to read, write, extend, and use the data.

Any GPT that works with this repo should treat this README as the **canonical guide**.

---

## 1. Repository structure

Current files:

- `cookbook.json`  
  Main structured data file. Contains:
  - `project_name`, `exported_at`, `timezone`
  - `user_prefs`
  - `knowledge_and_preferences`
  - `management_notes`
  - `cookbook`: array of recipe objects (the important part) 

More files may be added in the future (e.g. `scripts/`, `schema/`, etc.), but **`cookbook.json` is the authoritative database of recipes**.

---

## 2. Top-level JSON structure

`cookbook.json` is a JSON object with at least these keys: 

```jsonc
{
  "project_name": "בוזיאל מבשל – ספר מתכונים מסודר",
  "exported_at": "2025-11-15T00:00:00",
  "timezone": "Asia/Jerusalem",

  "user_prefs": {
    "units_default": "metric",
    "languages_policy": "Keep content in its original language; no translations.",
    "categorization": {
      "rule": "Recipes containing eggs or dairy are marked as vegetarian, not vegan."
    },
    "equipment_preferences": {
      "default_for_meats": "מחבת פסים",
      "default_for_stir_fry": "מחבת רגילה גדולה או ווק",
      "note": "בשרים תמיד על מחבת פסים; מוקפץ/סגנון אסיאתי על מחבת רגילה/ווק."
    }
  },

  "knowledge_and_preferences": [
    // free-form notes about user preferences & history
  ],

  "management_notes": "High-level rules for how the book is managed.",

  "cookbook": [
    // array of recipe objects (see schema below)
  ]
}
```

**What matters for GPTs?**

- `cookbook` is the primary array to read from and write to.
- `user_prefs`, `knowledge_and_preferences`, and `management_notes` give context and rules:
  - **Units**: metric.
  - **Keep recipes in original language** (Hebrew/English mix), do not auto-translate.
  - **Recipes with eggs/dairy** = vegetarian, not vegan.
  - Preferred equipment defaults for meats and stir-fry.

---

## 3. Recipe object schema (`cookbook[]`)

Each entry in `cookbook` is a recipe object. Fields can be:

### Required

- **`section`**  
  High-level category / section, for example:
  - `"מנות עיקריות"` (main dishes)
  - `"כבישה טבעית"` (natural pickling)
  - `"כבישה מהירה"` (quick pickling)
  - `"דגים"` (fish)
  - `"vegan"`, `"vegetarian"`, etc.

- **`title`**  
  Unique(ish) human title of the recipe, e.g.  
  `"שניצלים מושלמים בצ'יפסר – חזה עוף"`  
  `"פילה סלמון במחבת – קריספי מבחוץ, עסיסי מבפנים"`.

### Common optional fields

Not every recipe has all fields, but GPTs should prefer adding them for new or expanded recipes.

- **`language`**  
  e.g. `"he"`, `"en"`, `"he + en (original mix)"`.

- **`diet`**  
  e.g. `"בשרי (עוף)"`, `"בשרי (בקר)"`, `"טבעוני"`, `"צמחוני"`, `"vegetarian"`, `"vegan"`, `"דגי"`.

- **`contains`**  
  For allergens/constraints. Example: `["eggs"]`.

- **`serving_suggestion`**  
  Free-text suggested serving, e.g.  
  `"ציזיקי קר וטרי (Cold Fresh Tzatziki)"`.

- **`body`**  
  For older/imported recipes: a free-form block of text (full recipe, partially truncated for some).

- **`description`**  
  Short 1–3 line description of the dish.

- **`equipment`**  
  Array of tools used, e.g.:
  ```json
  "equipment": ["מחבת פסים", "תבנית תנור", "צ'יפסר"]
  ```

- **`ingredients`**  
  Either:
  - A single long string with line breaks, or
  - An array of strings, each an ingredient line.

- **`steps`**  
  Array of strings, each a clear step in order.

- **`tips`**  
  Optional array of short "chef notes" / tips about doneness, serving, or variations.

- **`status`**  
  For internal management, e.g.:
  - `"summary_only"` – only description, not a full recipe yet.
  - `"as_is_from_project_file"` – original imported content.
  - `"complete_generated_in_conversation"` – already expanded to a full recipe.

- **`source_note`**  
  Provenance metadata from previous tools/context.

---

## 4. Conventions & rules for GPTs

Any GPT or tool working with this repository should follow these rules:

1. **Do not translate recipes by default.**  
   Keep the original recipe language (Hebrew / English / mixed) as is, unless explicitly asked to translate.

2. **Diet labeling rule:**
   - If a recipe contains eggs or dairy → mark as `"vegetarian"` / `"צמחוני"`, **not** `"vegan"`.
   - Vegan = no animal products at all.

3. **Equipment defaults (when missing):**
   - Meats by default: `"מחבת פסים"` (grill pan).
   - Stir-fry / "Asian style": `"מחבת רגילה גדולה"` or `"ווק"`.

4. **The cookbook is the source of truth.**
   - When asked for a recipe that exists in `cookbook`, use that version.
   - Do not silently override the user's data with a completely new version unless asked to "rewrite" or "refine".

5. **`"summary_only"` recipes must not be invented.**
   - If `status == "summary_only"`, GPTs should **not** hallucinate full details unless the user explicitly asks to expand that specific recipe.
   - In that case, the expansion should respect `user_prefs` and existing `description`.

6. **No destructive edits.**
   - When suggesting JSON changes, GPTs should output the new/updated recipe object(s), not rewrite the entire file unless explicitly requested.

7. **Merge conflict resolution in `cookbook.json`:**
   - Conflicts in `cookbook.json` almost always happen because two branches each append a new recipe to the end of the `cookbook` array.
   - The correct resolution is **always to keep both recipes**. Never discard one side.
   - After resolving, ensure the result is valid JSON: proper commas between objects, no trailing commas, and no leftover conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
   - Validate the file with a JSON parser after resolution.

---

## 5. How GPTs should use this repo

### 5.1. Loading the cookbook

Given the raw URL:

```
https://raw.githubusercontent.com/boazhachlili/cookbook/refs/heads/main/cookbook.json
```

A GPT with browsing can:

1. Fetch the JSON.
2. Parse it.
3. Use:
   - `cookbook[]` as the recipe database.
   - `user_prefs` + `management_notes` as behavioral rules.

### 5.2. Answering questions

When user asks:

- **"Give me the recipe for X"** →  
  Find the recipe in `cookbook` by title (or close match) and format it nicely.

- **"Show me only vegan mains"** →  
  Filter `cookbook` by `diet` and/or `section`.

- **"Build a Friday dinner menu"** →  
  Use existing recipes, group by sections (main, sides, salads, pickles, sauces).

### 5.3. Adding a new recipe (workflow)

1. User describes a new recipe in natural language.
2. GPT creates a new recipe object matching this schema:

   ```json
   {
     "section": "מנות עיקריות",
     "title": "Example Dish Title",
     "language": "he",
     "diet": "בשרי (עוף)",
     "equipment": ["מחבת פסים"],
     "description": "Short description of the dish.",
     "ingredients": [
       "Line 1",
       "Line 2"
     ],
     "steps": [
       "Step 1",
       "Step 2"
     ],
     "tips": [
       "Optional tip 1",
       "Optional tip 2"
     ],
     "status": "complete_generated_in_conversation"
   }
   ```

3. User copies this object and adds it as a new element to the `cookbook` array in `cookbook.json` (manually via GitHub edit).

### 5.4. Updating an existing recipe

1. GPT identifies the recipe by `title` (and optionally `section`).
2. GPT outputs the updated JSON object (same structure, modified fields).
3. User replaces the original object inside `cookbook[]` with the updated one via GitHub.

---

## 6. Versioning & updates

### Single file, stable URL
`cookbook.json` is served via `raw.githubusercontent.com`, so its URL is stable. Only the content changes over time.

### Update pattern
When recipes are frequently updated:

1. Edit `cookbook.json` on GitHub (in browser).
2. Commit changes with a short message, e.g. `add salmon pan-fry recipe`.

### Backups / exports
If needed, the data can be exported back to CSV/Sheets or other formats using scripts, but **this repo is the canonical JSON definition**.

---

## 7. Future extensions (optional ideas)

Possible future additions (not required now):

- `schema/` directory with a formal JSON Schema for recipe objects.
- `scripts/` to:
  - Sync with Google Sheets.
  - Validate `cookbook.json` structure.
- `index` fields:
  - `id` per recipe.
  - `tags` (e.g. `"spicy"`, `"Moroccan"`, `"weeknight"`, `"air-fryer"`).
- Time metadata:
  - `prep_time_minutes`, `cook_time_minutes`, `difficulty`, `spice_level`.

---

## Summary

- **`cookbook.json`** = data (recipes + preferences).
- **`README.md`** = rules & workflows (how to use and extend that data).

GPTs should:

1. Load `cookbook.json`.
2. Respect `user_prefs` and the conventions above.
3. Use the `cookbook` array as the only recipe source.
4. Generate new/updated recipe objects that match this schema for manual commit.
