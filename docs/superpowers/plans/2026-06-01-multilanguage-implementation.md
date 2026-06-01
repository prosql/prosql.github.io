# Multi-Language Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add English and Bulgarian translations to all blog posts, book chapters, and pages using jekyll-polyglot, with language switching UI.

**Architecture:** jekyll-polyglot plugin manages language routing, content organization by language folders, language selector dropdown in header.

**Tech Stack:** Jekyll 4.3, jekyll-polyglot 1.3, Ruby/Bundler, SCSS/CSS

---

## Task 1: Update Gemfile with jekyll-polyglot

**Files:**
- Modify: `Gemfile`

- [ ] **Step 1: Read current Gemfile**

```bash
cat Gemfile
```

- [ ] **Step 2: Add jekyll-polyglot to jekyll_plugins group**

Update Gemfile to:
```ruby
source "https://rubygems.org"

gem "jekyll", "~> 4.3"
gem "jekyll-sitemap", "~> 1.4"
gem "jekyll-feed", "~> 0.17"
gem "jekyll-polyglot", "~> 1.3"
gem "github-pages", "~> 230"

group :jekyll_plugins do
  gem "jekyll-sitemap"
  gem "jekyll-feed"
  gem "jekyll-polyglot"
end
```

- [ ] **Step 3: Commit Gemfile change**

```bash
git add Gemfile
git commit -m "Add jekyll-polyglot gem for multi-language support"
```

---

## Task 2: Update _config.yml with language configuration

**Files:**
- Modify: `_config.yml`

- [ ] **Step 1: Read current _config.yml**

```bash
head -50 _config.yml
```

- [ ] **Step 2: Add language configuration to _config.yml**

Add these lines before the `collections:` section:

```yaml
# Language configuration for jekyll-polyglot
languages: ["en", "bg", "ru"]
default_lang: "en"
exclude_from_localization: ["/assets/", "/css/", "/js/", "sitemap.xml", "feed.xml"]

polyglot:
  fallback_languages: ["en"]
```

Full insert point: after the `sass:` section (around line 72), before `exclude:` section.

- [ ] **Step 3: Commit _config.yml change**

```bash
git add _config.yml
git commit -m "Configure jekyll-polyglot with EN, BG, RU languages"
```

---

## Task 3: Create language selector in layout

**Files:**
- Modify: `_layouts/default.html`

- [ ] **Step 1: Read the default layout**

```bash
cat _layouts/default.html
```

- [ ] **Step 2: Find the header/navigation section**

Look for `<nav>` or `<header>` tag. Language selector should go in the header navigation.

- [ ] **Step 3: Add language selector HTML**

Add this code in the header/navigation area (typically after the site title, before closing `</header>` or `</nav>`):

```html
<div class="language-selector">
  <label for="lang-select" class="sr-only">Select Language</label>
  <select id="lang-select" onchange="window.location.href=this.value" class="lang-dropdown">
    <option value="/en/" {% if page.lang == 'en' %}selected{% endif %}>English</option>
    <option value="/bg/" {% if page.lang == 'bg' %}selected{% endif %}>Български</option>
    <option value="/ru/" {% if page.lang == 'ru' %}selected{% endif %}>Русский</option>
  </select>
</div>
```

- [ ] **Step 4: Commit layout change**

```bash
git add _layouts/default.html
git commit -m "Add language selector dropdown to header"
```

---

## Task 4: Add CSS styling for language selector

**Files:**
- Modify: `style.scss` (or `assets/css/style.scss`)

- [ ] **Step 1: Read the current style file**

```bash
cat style.scss
```

- [ ] **Step 2: Add language selector CSS at the end**

Append this to the end of style.scss:

```scss
// Language selector styles
.language-selector {
  display: inline-block;
  margin-left: 1rem;
}

.lang-dropdown {
  padding: 0.5rem 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 0.9rem;
  background-color: white;
  cursor: pointer;
  transition: border-color 0.2s ease;
}

.lang-dropdown:hover {
  border-color: #999;
}

.lang-dropdown:focus {
  outline: none;
  border-color: #0077aa;
  box-shadow: 0 0 0 2px rgba(0, 119, 170, 0.1);
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

- [ ] **Step 3: Commit style changes**

```bash
git add style.scss
git commit -m "Add CSS styling for language selector"
```

---

## Task 5: Create language-specific post directories and move Russian posts

**Files:**
- Move: `_posts/*.md` → `_posts/ru/`
- Create: `_posts/en/`, `_posts/bg/`

- [ ] **Step 1: List all posts in _posts/**

```bash
ls -la _posts/ | grep "\.md$"
```

Note: Save this list to know which posts exist.

- [ ] **Step 2: Create language directories**

```bash
mkdir -p _posts/en _posts/bg _posts/ru
```

- [ ] **Step 3: Move Russian posts to _posts/ru/**

```bash
mv _posts/20*.md _posts/ru/
```

- [ ] **Step 4: Verify Russian posts moved correctly**

```bash
ls -la _posts/ru/ | grep "\.md$"
ls _posts/*.md 2>/dev/null | wc -l  # Should show 0
```

- [ ] **Step 5: Update Russian post frontmatter to add lang field**

For each file in `_posts/ru/`, add `lang: ru` to the frontmatter. Example:

If current frontmatter is:
```yaml
---
layout: post
title: "Post Title"
date: 2019-02-20 00:00:00
---
```

Update to:
```yaml
---
layout: post
title: "Post Title"
date: 2019-02-20 00:00:00
lang: ru
---
```

Process all files:
```bash
# For each file, you can use sed or manually edit
for file in _posts/ru/*.md; do
  if ! grep -q "lang:" "$file"; then
    sed -i '/^---$/a lang: ru' "$file"  # Add after first ---
  fi
done
```

Then verify one file looks correct:
```bash
head -10 _posts/ru/2019-02-20-check_inn.md
```

- [ ] **Step 6: Commit post reorganization**

```bash
git add _posts/
git commit -m "Reorganize Russian blog posts to _posts/ru/ with lang metadata"
```

---

## Task 6: Create language-specific book directories and move Russian chapters

**Files:**
- Move: `_book/*.md` → `_book/ru/`
- Create: `_book/en/`, `_book/bg/`

- [ ] **Step 1: List all book chapters**

```bash
ls -la _book/ | grep "\.md$"
```

- [ ] **Step 2: Create language directories**

```bash
mkdir -p _book/en _book/bg _book/ru
```

- [ ] **Step 3: Move Russian book chapters to _book/ru/**

```bash
mv _book/*.md _book/ru/ 2>/dev/null || echo "No .md files to move"
```

- [ ] **Step 4: Verify book chapters moved**

```bash
ls -la _book/ru/ | grep "\.md$"
```

- [ ] **Step 5: Update Russian book chapter frontmatter to add lang field**

For each file in `_book/ru/`, add `lang: ru` to the frontmatter (same pattern as Task 5):

```bash
for file in _book/ru/*.md; do
  if ! grep -q "lang:" "$file"; then
    sed -i '/^---$/a lang: ru' "$file"
  fi
done
```

Verify:
```bash
head -10 _book/ru/01-01-select.md
```

- [ ] **Step 6: Commit book reorganization**

```bash
git add _book/
git commit -m "Reorganize Russian book chapters to _book/ru/ with lang metadata"
```

---

## Task 7: Create language versions of root pages (about.md)

**Files:**
- Create: `about-bg.md`, `about-ru.md`
- Modify: `about.md` (rename conceptually to about-en.md but keep as about.md for jekyll-polyglot)

- [ ] **Step 1: Read current about.md**

```bash
cat about.md
```

- [ ] **Step 2: Create about-ru.md (Russian version)**

Copy current about.md content to about-ru.md and add `lang: ru`:

```bash
cp about.md about-ru.md
```

Edit about-ru.md to add `lang: ru` to frontmatter:
```yaml
---
layout: page
title: About
lang: ru
---
```

- [ ] **Step 3: Add lang: en to about.md**

Edit about.md frontmatter to:
```yaml
---
layout: page
title: About
lang: en
---
```

- [ ] **Step 4: Create about-bg.md (Bulgarian placeholder)**

Create about-bg.md with Bulgarian translation (initial version from AI, shown below):

```markdown
---
layout: page
title: За сайта
lang: bg
---

# Про SQL

Блог за всичко, което се отнася до СУБД.

Тази страница е превод на български език.
```

- [ ] **Step 5: Commit root page versions**

```bash
git add about.md about-bg.md about-ru.md
git commit -m "Create language versions of about page (EN, BG, RU)"
```

---

## Task 8: Translate all Russian blog posts to English

**Files:**
- Create: `_posts/en/*.md` (all translated posts)

- [ ] **Step 1: Get list of Russian posts to translate**

```bash
ls _posts/ru/ | grep "\.md$"
```

Count them:
```bash
ls _posts/ru/*.md | wc -l
```

- [ ] **Step 2: For each Russian post, create English translation**

For each file in `_posts/ru/`, you will:
1. Read the Russian content
2. Generate English translation (AI-assisted)
3. Create new file in `_posts/en/` with same filename
4. Include `lang: en` in frontmatter

**Script to help process files:**

```bash
for ru_file in _posts/ru/*.md; do
  filename=$(basename "$ru_file")
  en_file="_posts/en/$filename"
  
  # If English version doesn't exist yet, create it
  if [ ! -f "$en_file" ]; then
    echo "Need to translate: $filename"
    # Copy structure, will manually translate content
    cp "$ru_file" "$en_file"
    sed -i 's/lang: ru/lang: en/' "$en_file"
  fi
done
```

- [ ] **Step 3: Manually translate post content (or use Claude API)**

For each post in `_posts/en/`:
- Read the Russian original from `_posts/ru/`
- Translate title, content, and any metadata to English
- Update the frontmatter to have proper English title and `lang: en`

**Example:**
If `_posts/ru/2019-02-20-check_inn.md` has:
```yaml
---
title: "Проверка ИНН"
lang: ru
---
# Проверка ИНН

Как правильно проверить ИНН...
```

Then `_posts/en/2019-02-20-check_inn.md` should have:
```yaml
---
title: "Check INN"
lang: en
---
# Check INN

How to properly check INN...
```

**Note:** This step requires reading Russian posts and translating. You can batch this using Claude API or manual translation. For efficiency, process all posts together.

- [ ] **Step 4: Verify all English posts are in place**

```bash
ls _posts/en/*.md | wc -l
ls _posts/ru/*.md | wc -l
```

Should be equal.

- [ ] **Step 5: Commit English translations**

```bash
git add _posts/en/
git commit -m "Add English translations of all blog posts"
```

---

## Task 9: Translate all Russian blog posts to Bulgarian

**Files:**
- Create: `_posts/bg/*.md` (all translated posts)

- [ ] **Step 1: Create Bulgarian post structure**

```bash
for ru_file in _posts/ru/*.md; do
  filename=$(basename "$ru_file")
  bg_file="_posts/bg/$filename"
  
  if [ ! -f "$bg_file" ]; then
    cp "$ru_file" "$bg_file"
    sed -i 's/lang: ru/lang: bg/' "$bg_file"
  fi
done
```

- [ ] **Step 2: Translate post content to Bulgarian**

For each post in `_posts/bg/`:
- Read the Russian original
- Translate title and content to Bulgarian
- Update frontmatter with Bulgarian title and `lang: bg`

**Example:**
If original is "Проверка ИНН", Bulgarian would be "Проверка на ИНН" (similar structure but Bulgarian language).

- [ ] **Step 3: Verify all Bulgarian posts are in place**

```bash
ls _posts/bg/*.md | wc -l
```

Should match English and Russian counts.

- [ ] **Step 4: Commit Bulgarian translations**

```bash
git add _posts/bg/
git commit -m "Add Bulgarian translations of all blog posts"
```

---

## Task 10: Translate all Russian book chapters to English

**Files:**
- Create: `_book/en/*.md` (all translated chapters)

- [ ] **Step 1: Create English chapter structure**

```bash
for ru_file in _book/ru/*.md; do
  filename=$(basename "$ru_file")
  en_file="_book/en/$filename"
  
  if [ ! -f "$en_file" ]; then
    cp "$ru_file" "$en_file"
    sed -i 's/lang: ru/lang: en/' "$en_file"
  fi
done
```

- [ ] **Step 2: Translate chapter content to English**

For each chapter in `_book/en/`:
- Read Russian original
- Translate title and content to English
- Update frontmatter with `lang: en`

This may require multiple chapters, process all.

- [ ] **Step 3: Verify all English chapters are in place**

```bash
ls _book/en/*.md | wc -l
ls _book/ru/*.md | wc -l
```

Should be equal.

- [ ] **Step 4: Commit English chapter translations**

```bash
git add _book/en/
git commit -m "Add English translations of all book chapters"
```

---

## Task 11: Translate all Russian book chapters to Bulgarian

**Files:**
- Create: `_book/bg/*.md` (all translated chapters)

- [ ] **Step 1: Create Bulgarian chapter structure**

```bash
for ru_file in _book/ru/*.md; do
  filename=$(basename "$ru_file")
  bg_file="_book/bg/$filename"
  
  if [ ! -f "$bg_file" ]; then
    cp "$ru_file" "$bg_file"
    sed -i 's/lang: ru/lang: bg/' "$bg_file"
  fi
done
```

- [ ] **Step 2: Translate chapter content to Bulgarian**

For each chapter in `_book/bg/`:
- Read Russian original
- Translate to Bulgarian
- Update frontmatter with `lang: bg`

- [ ] **Step 3: Verify all Bulgarian chapters are in place**

```bash
ls _book/bg/*.md | wc -l
```

Should match English and Russian counts.

- [ ] **Step 4: Commit Bulgarian chapter translations**

```bash
git add _book/bg/
git commit -m "Add Bulgarian translations of all book chapters"
```

---

## Task 12: Test Jekyll build and language routing

**Files:**
- Test: Verify site structure and Jekyll build

- [ ] **Step 1: Run Jekyll build locally**

```bash
jekyll build --config _config.yml
```

Check for build errors. Expected output should show site built to `_site/` directory.

- [ ] **Step 2: Verify language-specific directories were created**

```bash
ls -d _site/en/ _site/bg/ _site/ru/ 2>/dev/null && echo "Language dirs created" || echo "Check failed"
```

- [ ] **Step 3: Verify posts are in language directories**

```bash
ls _site/en/ | grep -E "20[0-9]{2}" | wc -l  # Count English posts
ls _site/bg/ | grep -E "20[0-9]{2}" | wc -l  # Count Bulgarian posts
ls _site/ru/ | grep -E "20[0-9]{2}" | wc -l  # Count Russian posts
```

Should all be roughly equal.

- [ ] **Step 4: Check language switcher HTML rendered**

```bash
grep -l "lang-dropdown" _site/en/index.html _site/bg/index.html 2>/dev/null && echo "Language selector present" || echo "Check failed"
```

- [ ] **Step 5: Verify URLs follow pattern**

Test a specific post:
```bash
ls _site/en/check-inn/ 2>/dev/null && echo "Post routing works" || echo "Check post URL"
ls _site/bg/check-inn/ 2>/dev/null && echo "Bulgarian post exists" || echo "Check Bulgarian post"
ls _site/ru/check-inn/ 2>/dev/null && echo "Russian post exists" || echo "Check Russian post"
```

- [ ] **Step 6: Commit build verification (no code changes needed)**

Just verify and report status:
```bash
echo "Build verification complete"
```

---

## Task 13: Test language switching functionality

**Files:**
- Test: Verify language switcher works

- [ ] **Step 1: Check language selector in generated HTML**

```bash
cat _site/en/index.html | grep -A 5 "language-selector"
```

Should show dropdown with English selected.

- [ ] **Step 2: Spot-check hreflang tags (SEO)**

```bash
cat _site/en/index.html | grep "hreflang" | head -3
```

Should show links to `/bg/`, `/ru/` versions if jekyll-polyglot added them.

- [ ] **Step 3: Test fallback language**

If any English translation is missing, jekyll-polyglot should serve English content. This is configured but difficult to test without a live server.

- [ ] **Step 4: Manual smoke test (optional)**

If Jekyll is running locally:
- Visit `http://localhost:4000/en/`
- Click language selector
- Verify it navigates to `http://localhost:4000/bg/`

---

## Task 14: Final cleanup and push

**Files:**
- Cleanup any temporary files, verify all commits

- [ ] **Step 1: Check git status**

```bash
git status
```

Should be clean.

- [ ] **Step 2: Review commit log**

```bash
git log --oneline | head -15
```

Should see commits from all previous tasks.

- [ ] **Step 3: Verify no broken files or incomplete edits**

```bash
find . -name "*.md" -type f -exec grep -l "TODO\|FIXME\|TBD" {} \;
```

Should return nothing.

- [ ] **Step 4: Final push to remote**

```bash
git push origin master
```

---

## Self-Review Against Spec

**Spec Coverage:**
- ✅ Architecture: jekyll-polyglot setup (Tasks 1-2)
- ✅ Configuration: Gemfile, _config.yml, layout (Tasks 1-4)
- ✅ Folder Structure: Language-specific posts, book, pages (Tasks 5-7)
- ✅ Content Translation: All posts, chapters, pages in EN, BG, RU (Tasks 8-11)
- ✅ Language Switcher UI: Dropdown in header with routing (Tasks 3-4)
- ✅ Testing: Build, routing, language switching (Tasks 12-13)
- ✅ Final Deployment: Git push (Task 14)

**All spec requirements covered by implementation tasks.**

---

## Execution Handoff

Plan complete and saved to `docs/superpowers/plans/2026-06-01-multilanguage-implementation.md`.

**Two execution options:**

1. **Subagent-Driven (recommended)** — I dispatch a fresh subagent per task, review between tasks, fast iteration

2. **Inline Execution** — Execute tasks in this session using executing-plans, batch execution with checkpoints

Which approach?
