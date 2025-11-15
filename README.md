# 🍳 Personal Cookbook - LLM Instructions

## Overview

This repository contains a personal recipe collection in JSON format. The primary data source is:

**https://raw.githubusercontent.com/boazhachlili/cookbook/refs/heads/main/cookbook.json**

## Important Instructions for LLMs (ChatGPT, Claude, etc.)

### Core Rules

1. **ALWAYS load the JSON from the URL above** at the beginning of each session
2. **ONLY use recipes that exist in the cookbook.json file** - do not invent or suggest recipes not in the collection
3. **When adding new recipes**, format them to match the existing structure exactly

### JSON Structure

The cookbook.json file contains:

- `project_name`: Name of the cookbook
- `exported_at`: Last export timestamp
- `user_prefs`: User preferences (units, languages, categorization rules)
- `knowledge_and_preferences`: Historical notes about user preferences
- `management_notes`: Rules for managing the cookbook
- `cookbook`: **Array of recipe objects** (this is the main data)

### Recipe Object Schema

Each recipe in the `cookbook` array may contain:

**Required/Common Fields:**
- `section`: Category (e.g., "מנות עיקריות", "vegan", "סלטים טריים", "רטבים וממרחים", "כבישה טבעית")
- `title`: Recipe name
- `language`: "he" (Hebrew) or "en" (English)
- `diet`: Dietary classification (e.g., "טבעוני", "vegan", "בשרי (עוף)", "צמחוני", "vegetarian")

**Optional Fields:**
- `description`: Brief description of the dish
- `ingredients`: List of ingredients (can be array or text string)
- `steps`: Preparation steps (array)
- `tips`: Cooking tips (array or text)
- `equipment`: Required equipment (array)
- `contains`: Special dietary notes
- `serving_suggestion`: Serving recommendations
- `body`: Full recipe text (for older/unstructured entries)
- `source_note`: Origin/reference
- `status`: Recipe status

### User Preferences

**Key Rules from user_prefs:**
1. **Units**: Default to metric
2. **Languages**: Keep content in original language - no translations
3. **Categorization**: Recipes with eggs or dairy are **vegetarian**, not vegan
4. **Equipment for meats**: Always use "מחבת פסים" (grill pan)
5. **Equipment for stir-fry/Asian**: Use "מחבת רגילה גדולה או ווק"

## Common Use Cases

### 1. Finding a Recipe

When user asks for a recipe:
- Search in `cookbook` array by `title` or `section`
- Return exact recipe from the JSON
- Do not modify or "improve" the recipe

### 2. Suggesting a Menu

When user asks for meal ideas:
- **ONLY use recipes from the cookbook**
- Combine recipes from different sections
- Consider the `diet` field for dietary restrictions

### 3. Adding a New Recipe

When user provides a new recipe:
- Create a JSON object matching the schema above
- Include all relevant fields: `section`, `title`, `language`, `diet`, `ingredients`, `steps`, `tips`, `equipment`
- Format consistently with existing recipes
- Present the JSON so user can add it to the file

### 4. Searching by Criteria

Support searches by:
- **Category** (`section`)
- **Title**
- **Ingredient** (search in `ingredients` field)
- **Equipment** (search in `equipment` field)
- **Diet type** (`diet` field)

## Example Queries & Responses

**User**: "תן לי מתכון לקציצות עוף"
**Response**: Search for recipes with title containing "קציצות עוף" in the cookbook array, return the exact recipe.

**User**: "מה יש לי טבעוני?"
**Response**: Filter `cookbook` where `diet` is "טבעוני" or "vegan", list titles.

**User**: "תכנן לי ארוחה"
**Response**: Suggest combination of recipes (main + side + salad) from the existing cookbook only.

**User**: "הנה מתכון חדש: [recipe details]"
**Response**: Create properly formatted JSON object matching the schema, ready to be added to the cookbook array.

## Important Reminders

✅ **DO:**
- Load the JSON at session start
- Use only existing recipes
- Match the exact structure when adding recipes
- Respect user preferences (language, equipment, categorization)
- Search and filter accurately

❌ **DO NOT:**
- Invent recipes not in the JSON
- Suggest recipes from external sources
- Translate recipes (keep original language)
- Modify or "improve" existing recipes without permission
- Ignore the diet categorization rules (eggs/dairy = vegetarian, not vegan)

---

## Hebrew Instructions (הוראות בעברית)

יש לי ספר מתכונים אישי בפורמט JSON, שנגיש ב-URL הזה:

**https://raw.githubusercontent.com/boazhachlili/cookbook/refs/heads/main/cookbook.json**

### כללים עיקריים

הקובץ הוא אובייקט JSON שמכיל לפחות את השדה:
- `"cookbook"`: מערך של מתכונים

כל מתכון ב-`"cookbook"` כולל שדות כמו:
- `section` (קטגוריה)
- `title` (שם המנה)
- `description`
- `ingredients`
- `steps`
- `tips`
- `equipment`
- `diet`
ועוד שדות עזר.

### מה לעשות

1. **תטען את ה-JSON מהכתובת הזו** בתחילת כל שיחה
2. **תשתמש בו כבסיס הידע היחיד** לספר המתכונים שלי
3. כשאני מבקש מתכון – **תשלוף אותו מתוך cookbook** לפי הכותרת או לפי הקטגוריה
4. כשאני מבקש רעיון/תפריט – **תשתמש רק במתכונים מתוך ה-cookbook**
5. כשאני נותן מתכון חדש – **תבנה לי אובייקט JSON חדש** בפורמט שתואם למבנה של ה-cookbook, כדי שאוכל להוסיף אותו לקובץ

### מה לא לעשות

❌ **נא לא להמציא מתכונים** שלא קיימים ב-JSON, אלא אם אני מוסיף אותם במפורש
❌ לא לתרגם מתכונים (לשמור על השפה המקורית)
❌ לא לשנות מתכונים קיימים בלי אישור

---

*This README is designed to be read by LLMs at the start of each session to understand how to work with this personal cookbook.*
