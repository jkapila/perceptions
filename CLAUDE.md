# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **"The AI Architect"** brand website for Jitin Kapila, built with Quarto. The site transforms from a portfolio into a premium coaching/consulting platform featuring:

- Homepage with hero + frameworks (AI-ART Matrix, OLCD Protocol)
- Course sales page (AI Profit OS - ₹19,999/₹34,999)
- Consulting waitlist
- Blog ("The Vault") with Strategy/Engineering tabs
- About page (authority building)

**Live URLs:**
- Primary: https://jitinkapila.com
- Netlify: https://percieveit.com

## Technology Stack

- **Static Site Generator**: Quarto (1.4+)
- **Styling Approach**: SCSS + CSS (hybrid system - see below)
- **Current Theme**: Brite (Bootstrap-based)
- **Analytics**: Umami (includes/umami.html)
- **SEO**: Schema.org JSON-LD (includes/schema.html)
- **Comments**: Utterances (GitHub-based, on blog posts)
- **Publishing**: Netlify

## Architecture Decision: Why SCSS + CSS Hybrid?

### The Stability Problem

The project currently uses **two styling systems** which can cause conflicts:

1. **custom.css** - Direct CSS overrides (current implementation)
2. **styles.scss** - SCSS with Bootstrap variables (backup/original)

### Recommended Approach: SCSS-First Architecture

Based on Quarto best practices and the Lumo theme example in `extras/`, here's why SCSS is superior:

#### SCSS Advantages:
1. **Native Quarto Integration**: Quarto processes SCSS at build time with Bootstrap
2. **Variable System**: Can override Bootstrap variables (`$primary`, `$font-family-sans-serif`)
3. **Theme Inheritance**: Extend themes properly instead of fighting them
4. **Compile-Time Safety**: SCSS errors caught during build, not runtime
5. **Source Order Control**: SCSS compiles BEFORE custom CSS in cascade

#### Current CSS Limitations:
1. **Override Battles**: Must use `!important` everywhere to fight Bootstrap
2. **Specificity Wars**: Constantly battling theme defaults
3. **No Variables**: Can't modify Bootstrap's design tokens cleanly
4. **Runtime Conflicts**: Themes like Cosmo/Brite have their own opinions

### Recommended Migration Path:

```scss
/*-- scss:defaults --*/
// Override Bootstrap variables BEFORE they compile
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=IBM+Plex+Sans:wght@400;500;600&display=swap');

// Typography Variables
$font-family-sans-serif: 'IBM Plex Sans', sans-serif;
$headings-font-family: 'Bebas Neue', sans-serif;
$headings-font-weight: 400;
$headings-text-transform: uppercase;
$headings-letter-spacing: 1px;

// Color Variables
$primary: #0C2B4E;      // Navy Deep
$secondary: #213555;    // Navy Royal
$body-color: #5A6A7A;
$link-color: #0C2B4E;

// Spacing
$spacer: 1rem;

// Border Radius (Blueprint aesthetic - no curves)
$border-radius: 0;
$border-radius-sm: 0;
$border-radius-lg: 0;

/*-- scss:rules --*/
// Custom rules that extend the compiled theme
body {
  background-image:
    linear-gradient(rgba(33, 53, 85, 0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(33, 53, 85, 0.03) 1px, transparent 1px);
  background-size: 20px 20px;
}

h1, h2, h3, h4, h5, h6 {
  font-family: $headings-font-family;
  text-transform: $headings-text-transform;
  letter-spacing: $headings-letter-spacing;
}
```

### Why This Works Better:

1. **Variables in `/*-- scss:defaults --*/`** override Bootstrap BEFORE compilation
2. **Rules in `/*-- scss:rules --*/`** extend the theme cleanly
3. **No `!important` needed** - you're modifying the source, not fighting it
4. **Theme changes are easy** - just change theme name in `_quarto.yml`

## Current Build Commands

```bash
# Preview with live reload
quarto preview

# Production build
quarto render

# Publish to Netlify
quarto publish netlify
```

**Note**: Pre-render hook cleans `_site/*` automatically (see `_quarto.yml`)

## Site Structure

```
/
├── index.qmd              # Homepage (Hero + AI-ART Matrix)
├── about.qmd              # Authority building
├── bootcamp.qmd           # AI Profit OS sales page
├── consulting.qmd         # Consulting waitlist
├── vault/
│   ├── index.qmd          # Blog listing with tabs
│   └── (links to ../posts/)
├── posts/                 # Blog posts
│   ├── _metadata.yml      # Default settings for all posts
│   └── [post_name]/index.qmd
├── includes/
│   ├── umami.html         # Analytics
│   └── schema.html        # SEO structured data
├── extras/                # Reference materials
│   ├── BRAND_MEMORY.md    # Complete brand guide
│   ├── design-tokens.json # Design system tokens
│   └── lumo theme/        # Example Quarto extension
├── _quarto.yml            # Site configuration
├── custom.css             # Current CSS (to migrate to SCSS)
├── styles.scss            # Backup SCSS system
└── backup/                # Original files backup
```

## Design System (The Blueprint)

### Brand Identity

- **Brand Name**: The AI Architect
- **Tagline**: "Stop Guessing. Start Architecting."
- **Visual Motif**: Engineering blueprint aesthetic
- **Core Principle**: Sharp corners, no gradients, geometric precision

### Color Palette (Design Tokens)

```css
:root {
  /* Primary */
  --navy-deep: #0C2B4E;      /* Headers, buttons, primary text */
  --navy-royal: #213555;     /* Hover states, secondary */
  --slate-blue: #3E5879;     /* Accent text */

  /* Accent */
  --cream-gold: #D8C4B6;     /* Dividers, highlights */
  --gold-hover: #C4B09E;     /* Gold hover state */

  /* Backgrounds */
  --bg-light: #FAFBFC;       /* Default background */
  --bg-warm: #F8F6F3;        /* Alternate sections */
  --bg-grid: rgba(33, 53, 85, 0.03);  /* Blueprint grid */

  /* Text */
  --text-primary: #0C2B4E;   /* Headlines */
  --text-body: #5A6A7A;      /* Paragraphs */
  --text-muted: #8896A6;     /* Meta info */
}
```

### Typography System

```css
/* Headers: Bebas Neue - Bold, Uppercase, Tight */
h1, h2, h3 {
  font-family: 'Bebas Neue', sans-serif;
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* Body: IBM Plex Sans - Clean, Professional */
body {
  font-family: 'IBM Plex Sans', sans-serif;
  font-weight: 400;
  line-height: 1.7;
}

/* Responsive Sizing */
h1 { font-size: clamp(48px, 8vw, 80px); line-height: 0.95; }
h2 { font-size: clamp(32px, 5vw, 48px); line-height: 1.1; }
h3 { font-size: clamp(24px, 3vw, 32px); line-height: 1.2; }

/* Eyebrow Text */
.eyebrow {
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: var(--text-muted);
}
```

### Component Standards

All components follow **border-radius: 0** (no rounded corners):

- **Buttons**: Sharp, uppercase, slide-in hover effect
- **Cards**: 1-2px border, subtle shadow on hover
- **Dividers**: 60px horizontal lines in cream-gold
- **Grid Background**: 20px × 20px blueprint grid at 3% opacity

## Configuration Guide

### _quarto.yml Structure

```yaml
project:
  type: website
  output-dir: _site
  pre-render: ["rm -rf _site/*"]  # Clean build
  resources:
    - CNAME
    - favicon-32x32.png
    - favicon.ico
    - apple-touch-icon.png

website:
  title: "Jitin Kapila | The AI Architect"
  site-url: https://jitinkapila.com

  navbar:
    background: transparent  # Let CSS control this
    left:
      - text: " "  # Blank for logo/brand
        href: index.qmd
    right:
      - text: "The Protocol"
        href: bootcamp.qmd
      # ... other nav items

  page-footer:
    left: "© 2025 Jitin Kapila | The AI Architect"
    right:
      - icon: linkedin
        href: https://linkedin.com/in/jitinkapila
      # ... other social icons

format:
  html:
    theme: brite  # Can also use: cosmo, flatly, litera
    css: custom.css  # Or use SCSS with theme: [custom.scss]
    include-in-header:
      - includes/umami.html
      - includes/schema.html
    toc: false  # Disable by default, enable per-page
    code-fold: true
    highlight-style: github
    smooth-scroll: true

execute:
  freeze: auto  # Cache computational output
  cache: true

google-analytics: "G-XXXXXXXXXX"  # Replace with actual ID
```

### Key Configuration Patterns

1. **Theme Selection**: Use Bootstrap-based themes (cosmo, brite, flatly, litera)
2. **CSS Loading Order**: Theme → CSS → Page-specific styles
3. **Freeze Strategy**: `freeze: auto` prevents re-running code unnecessarily
4. **Resources**: Always include CNAME, favicons for deployment

## Working with Blog Posts

### Post Frontmatter Template

```yaml
---
title: "Post Title"
description: "Brief description for listings"
date: "YYYY-MM-DD"
categories: [strategy, code]  # For tab filtering
tags: [ai, ml, enterprise]
image: "img/feature.jpg"
image-alt: "Alt text"
draft: false  # Set true to hide
author: "Jitin Kapila"  # Inherited from posts/_metadata.yml
---
```

### Post Metadata Inheritance

`posts/_metadata.yml` sets defaults for ALL posts:

```yaml
freeze: true
author: 'Jitin Kapila'
toc: true
toc-depth: 3
toc-location: left
page-layout: article
reference-location: margin  # Tufte-style margin notes
citation-location: margin
lightbox: true
license: "CC BY"
code-line-numbers: true
code-fold: true
comments:
  utterances:
    repo: jkapila/perceptions
    theme: github-light
```

### Category-Based Tab Filtering

The Vault uses categories to separate posts:

- **Strategy Tab**: `categories: [strategy, business, roi]`
- **Engineering Tab**: `categories: [code, llm, agents]`

**Note**: Posts can appear in multiple tabs if they have multiple categories.

## Advanced Quarto Features

### Custom Layouts with Divs

Quarto uses fenced divs for layout control:

```markdown
::: {.hero-section}
Content here
:::

::: {.column-body-outset}
Wide content that extends beyond normal margins
:::

::: {.panel-tabset}
## Tab 1
Content
## Tab 2
More content
:::
```

### Available Layout Classes

- `.column-body` - Normal width (default)
- `.column-body-outset` - Slightly wider
- `.column-page` - Page width
- `.column-screen` - Full screen width
- `.column-margin` - Margin notes (Tufte style)

### Template Partials (Advanced)

For deep customization, use template partials (see `extras/lumo theme/`):

```yaml
format:
  html:
    template-partials:
      - title-block.html  # Custom title block
      - footer.html       # Custom footer
```

## Brand.yml Implementation (Recommended Future)

Quarto 1.4+ supports `brand.yml` for centralized design tokens. This is the FUTURE direction:

### brand.yml Structure

```yaml
meta:
  name: "The AI Architect"
  link: "https://jitinkapila.com"

color:
  palette:
    navy-deep: "#0C2B4E"
    navy-royal: "#213555"
    cream-gold: "#D8C4B6"
  background: "#FAFBFC"
  foreground: "#0C2B4E"
  primary: "#0C2B4E"
  secondary: "#213555"

typography:
  fonts:
    - family: "Bebas Neue"
      source: google
    - family: "IBM Plex Sans"
      source: google
      weight: [400, 500, 600]
  base-size: "16px"
  headings:
    family: "Bebas Neue"
    weight: 400
  body:
    family: "IBM Plex Sans"
    weight: 400
    line-height: 1.7
```

### Why Brand.yml?

1. **Single Source of Truth**: All design tokens in one file
2. **Cross-Format**: Works across HTML, PDF, DOCX outputs
3. **Tooling Integration**: IDEs can autocomplete brand values
4. **Team Collaboration**: Designers can edit without touching code

**Status**: Not yet implemented. Consider for Phase 2 refactor.

## SEO & Analytics

### Schema.org Structured Data

Located in `includes/schema.html`:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "ProfessionalService",
      "name": "Jitin Kapila - The AI Architect",
      "founder": {
        "@type": "Person",
        "name": "Jitin Kapila",
        "jobTitle": "AI Strategy Consultant"
      }
    },
    {
      "@type": "Course",
      "name": "The AI Architect Protocol",
      "provider": { "@type": "Person", "name": "Jitin Kapila" }
    }
  ]
}
</script>
```

### Umami Analytics

Located in `includes/umami.html` - lightweight, privacy-focused alternative to Google Analytics.

### Open Graph & Twitter Cards

Configured in `_quarto.yml`:

```yaml
website:
  open-graph: true
  twitter-card:
    creator: "@jitinkapila"
    card-style: summary_large_image
```

## Performance Optimizations

### Freeze Strategy

```yaml
execute:
  freeze: auto  # Only re-run when source changes
  cache: true   # Cache results
```

**When to Clear Cache**:
```bash
rm -rf _freeze/
quarto render
```

### Image Optimization

- Use thumbnails for listings (`-thumb.jpg` suffix)
- Compress images before adding to repo
- Enable lightbox for blog images (`lightbox: true`)

### Code Highlighting

- Use `highlight-style: github` for consistency
- Enable `code-fold: true` for long code blocks
- Add `code-line-numbers: true` for readability

## Common Tasks

### Change Navigation

Edit `_quarto.yml` → `website.navbar.right`:

```yaml
navbar:
  right:
    - text: "New Page"
      href: newpage.qmd
```

### Add Social Icon

Edit `_quarto.yml` → `page-footer.right`:

```yaml
page-footer:
  right:
    - icon: youtube
      href: https://youtube.com/@channel
```

### Create New Page

```bash
# Create file
touch newpage.qmd

# Add frontmatter
---
title: "Page Title"
page-layout: full  # or article, custom
---
```

### Modify Theme Colors (SCSS Method)

Edit `styles.scss` → `/*-- scss:defaults --*/`:

```scss
$primary: #0C2B4E;
$secondary: #213555;
```

Then in `_quarto.yml`:

```yaml
format:
  html:
    theme: [cosmo, styles.scss]  # Theme + custom SCSS
```

## Troubleshooting

### Navbar Not Showing

**Cause**: Using `theme: none` removes Bootstrap navbar structure.
**Fix**: Use a Bootstrap theme (cosmo, brite, flatly) + CSS overrides.

### Styles Not Applying

**Check Order**:
1. Is SCSS in `/*-- scss:defaults --*/` section?
2. Is CSS loaded AFTER theme in `_quarto.yml`?
3. Are you using `!important` (symptom of wrong approach)?

**Debug**:
```bash
quarto preview --log-level debug
```

### Freeze Issues

**Symptom**: Old content showing despite changes.
**Fix**:
```bash
rm -rf _freeze/
quarto render
```

### Preview Not Updating

**Fix**:
```bash
# Kill all Quarto processes
pkill -f quarto

# Restart preview
quarto preview
```

## Best Practices

### Styling Best Practices

1. **Use SCSS variables** instead of CSS custom properties for Bootstrap integration
2. **Prefer theme extension** over complete override
3. **Avoid `!important`** - it indicates fighting the cascade
4. **Test mobile-first** - use responsive breakpoints
5. **Keep specificity low** - let cascade work naturally

### Content Best Practices

1. **Use divs for layout**, not CSS classes in markdown
2. **Enable freeze for computational posts** - save build time
3. **Add alt text to all images** - SEO + accessibility
4. **Use relative links** for internal navigation
5. **Test locally before pushing** - `quarto preview`

### Performance Best Practices

1. **Optimize images** before committing (use WebP when possible)
2. **Use code-fold** for long code blocks
3. **Enable caching** for expensive computations
4. **Minimize custom fonts** (currently 2 families - good)
5. **Lazy load images** in listings

## Future Roadmap

### Phase 1: Stability (Current)
- [x] Migrate to Bootstrap theme + CSS overrides
- [x] Fix navbar visibility
- [ ] Consolidate to SCSS-only approach
- [ ] Remove `!important` from CSS

### Phase 2: Enhancement
- [ ] Implement `brand.yml` for design tokens
- [ ] Create Quarto extension for Blueprint theme
- [ ] Add custom template partials
- [ ] Implement custom listing layouts

### Phase 3: Optimization
- [ ] Add service worker for offline
- [ ] Implement lazy loading for images
- [ ] Optimize build time (currently ~20s)
- [ ] Add progressive enhancement features

## Resources

### Official Documentation
- [Quarto Websites](https://quarto.org/docs/websites/)
- [HTML Themes](https://quarto.org/docs/output-formats/html-themes.html)
- [HTML Theme Customization](https://quarto.org/docs/output-formats/html-themes-more.html)
- [Brand Guide](https://quarto.org/docs/authoring/brand.html)
- [HTML Basics](https://quarto.org/docs/output-formats/html-basics.html)

### Reference Files
- `extras/BRAND_MEMORY.md` - Complete brand guide
- `extras/design-tokens.json` - Design system reference
- `extras/lumo theme/` - Example custom Quarto extension
- `backup/` - Original files before rebuild

### Community
- [Quarto Discussions](https://github.com/quarto-dev/quarto-cli/discussions)
- [Awesome Quarto](https://github.com/mcanouil/awesome-quarto)

## Quick Reference

### File Purposes

| File | Purpose | Edit Frequency |
|------|---------|----------------|
| `_quarto.yml` | Site config, nav, theme | Occasionally |
| `custom.css` | Style overrides | Often (to migrate to SCSS) |
| `styles.scss` | SCSS variables/rules | Recommended approach |
| `includes/schema.html` | SEO structured data | Rarely |
| `includes/umami.html` | Analytics script | Never (unless changing provider) |
| `posts/_metadata.yml` | Blog post defaults | Rarely |
| `index.qmd` | Homepage | Occasionally |
| `vault/index.qmd` | Blog listing | Rarely |

### Common Frontmatter Options

```yaml
---
title: "Page Title"
pagetitle: "Browser Tab Title"  # Overrides title for <title> tag
page-layout: full  # full, article, custom
toc: true  # Enable table of contents
toc-location: left  # left, right, body
description: "Meta description for SEO"
image: "path/to/social-share.jpg"
draft: false  # Hide page if true
---
```

---

*This document should be referenced when making ANY changes to the site architecture, styling, or content structure.*
