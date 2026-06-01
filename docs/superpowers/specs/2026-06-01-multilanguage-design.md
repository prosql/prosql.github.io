# Multi-Language Site Implementation (EN, BG, RU)

**Date:** 2026-06-01  
**Scope:** Add English and Bulgarian translations to all pages, posts, and book content using jekyll-polyglot

## Overview

Convert the current Russian-only site into a multi-language site with English (default), Bulgarian, and Russian versions. All content (pages, blog posts, book chapters) will be available in all three languages.

## Architecture

**Plugin:** jekyll-polyglot v1.3+
- Purpose-built Jekyll plugin for multi-language sites
- Automatic URL routing (`/en/`, `/bg/`, `/ru/`)
- Language switcher support
- Fallback language handling

**Language Strategy:**
- **Default language:** English (`/` redirects to `/en/`)
- **Other languages:** Russian and Bulgarian at `/ru/` and `/bg/`
- **Content parity:** Same posts/pages in all three languages
- **Fallback:** If a translation is missing, fall back to English

## Folder Structure

### Content Organization

```
_posts/
  en/                           # English blog posts
    2019-02-20-check_inn.md
    2016-10-16-triggers.md
    (all current posts + new translations)
  bg/                           # Bulgarian translations
    2019-02-20-check_inn.md
    2016-10-16-triggers.md
    (all posts translated)
  ru/                           # Original Russian posts
    2019-02-20-check_inn.md
    2016-10-16-triggers.md
    (existing content moved here)

_book/                          # Book chapters collection
  en/
    01-01-select.md
    01-02-langstru.md
    (all chapters)
  bg/
    01-01-select.md
    (all chapters translated)
  ru/
    01-01-select.md
    (existing chapters moved here)
```

### Root Pages

Root-level Markdown pages use language suffixes:
```
about.md          → frontmatter: lang: en (English default)
about-bg.md       → frontmatter: lang: bg (Bulgarian)
about-ru.md       → frontmatter: lang: ru (Russian)

index-bg.md       → frontmatter: lang: bg
index-ru.md       → frontmatter: lang: ru
```

`index.html` remains as the main entry point (English default at `/`).

### Frontmatter Requirements

Every translated file must include language metadata:
```yaml
---
layout: post
title: "Original Title"
lang: en                    # Required: en, bg, or ru
date: 2019-02-20
---
```

## Configuration

### Gemfile
Add jekyll-polyglot dependency:
```ruby
gem "jekyll-polyglot", "~> 1.3"
```

Update the plugins section in Gemfile to include jekyll-polyglot in the jekyll_plugins group.

### _config.yml

Add language configuration:
```yaml
# Language configuration for jekyll-polyglot
languages: ["en", "bg", "ru"]
default_lang: "en"
exclude_from_localization: ["/assets/", "/css/", "/js/", "sitemap.xml", "feed.xml"]

polyglot:
  fallback_languages: ["en"]
```

## Language Switcher UI

### Header Component

Add a language selector dropdown to the site header (in `_layouts/default.html`):

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

### CSS

Basic styling for the language selector:
```css
.language-selector {
  display: inline-block;
}

.lang-dropdown {
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 0.9rem;
  background-color: white;
  cursor: pointer;
}

.lang-dropdown:hover {
  border-color: #999;
}
```

## Content Translation

### Translation Source

- **Russian (original):** Existing blog posts and book content in Russian
- **English & Bulgarian:** AI-generated translations using Claude

### Translation Workflow

1. Read each Russian source file
2. Generate English translation via Claude API
3. Generate Bulgarian translation via Claude API
4. Create corresponding files in `_posts/en/`, `_posts/bg/`, `_book/en/`, `_book/bg/`
5. Include proper frontmatter with `lang: en` or `lang: bg`
6. Commit all translated files

### Translation Quality

- AI translations are initial versions suitable for a database/SQL audience
- User can refine translations later as needed
- Focus on technical accuracy for database/SQL terminology

## Implementation Phases

### Phase 1: Setup (Configuration)
- Update Gemfile with jekyll-polyglot
- Update _config.yml with language configuration
- Add language selector to layout
- Add CSS styling for selector

### Phase 2: Content Organization
- Reorganize existing Russian posts from `_posts/` to `_posts/ru/`
- Reorganize book chapters from `_book/` to `_book/ru/`
- Move root pages to language-suffixed versions

### Phase 3: Content Translation
- Generate English translations for all posts in `_posts/en/`
- Generate Bulgarian translations for all posts in `_posts/bg/`
- Generate English translations for all book chapters in `_book/en/`
- Generate Bulgarian translations for all book chapters in `_book/bg/`

### Phase 4: Root Page Translation
- Translate `about.md`, `index.html` content to English and Bulgarian
- Create `about-bg.md`, `about-ru.md`, `index-bg.md`, `index-ru.md`

### Phase 5: Testing & Verification
- Verify language switching works across all pages
- Check that fallback to English works
- Verify URLs are correct (`/en/`, `/bg/`, `/ru/`)
- Test on GitHub Pages

## Excluded from Localization

These files/paths will NOT be translated:
- Static assets (`/assets/`, `/css/`, `/js/`)
- Sitemap and feed files (handled by jekyll-polyglot)
- Configuration files

## Success Criteria

- [ ] All blog posts available in EN, BG, RU
- [ ] All book chapters available in EN, BG, RU
- [ ] Root pages (about) available in EN, BG, RU
- [ ] Language selector appears in header and switches between versions
- [ ] URLs follow `/en/`, `/bg/`, `/ru/` pattern
- [ ] Default language is English (`/` → `/en/`)
- [ ] Language metadata correct in all files
- [ ] Site builds without errors
- [ ] GitHub Pages deployment works

## Technical Notes

- jekyll-polyglot uses the `lang:` frontmatter field to associate translations
- Same filename in different language folders = same post, different language
- Plugin auto-generates hreflang tags for SEO
- No additional build complexity; works with standard Jekyll/GitHub Pages

## Rollback Plan

If issues arise:
1. Remove jekyll-polyglot from Gemfile
2. Move language-specific folders back to original structure
3. Remove language selector from layout
4. Revert _config.yml changes

All original content preserved in `_posts/ru/` and `_book/ru/`.
