# SCSS Audit & Recommendations

## Current State (Honest Assessment)

### File Size
- **blueprint.scss**: 1,074 lines
- **bootcamp.qmd `<style>` block**: 445 lines
- **Total styling code**: ~1,500 lines

### Problems Identified

#### 1. **Duplication Between Files**
- Bootcamp page has its own 445-line `<style>` block
- Many patterns duplicate what should be in blueprint.scss
- Examples: `.checklist-grid`, `.meta-box`, `.timeline-detailed`
- **Impact**: Maintenance nightmare - changes must be made in 2 places

#### 2. **Too Many Specific Selectors**
Current redundant classes:
- `.section` (generic)
- `.fixed-section` (same as section but with max-width)
- `.hero-section` (specific to hero)
- `.problem-section`, `.transformation-section`, `.curriculum-section` (all basically `.section`)

**Recommendation**: Consolidate to just `.section` with optional modifiers

#### 3. **Overly Deep Nesting**
```scss
.logo-ribbon .logo-scroll-wrapper .logo-container .logo-item img
```
This is 5 levels deep! Makes it:
- Hard to override
- Difficult to debug
- Increases specificity wars

#### 4. **Page-Specific Styles in Global Stylesheet**
- `.bootcamp-hero` - only used once
- `.instructor-section` - only used on bootcamp page
- `.value-stack-section` - only used on bootcamp page

**These should either be:**
- Generalized (e.g., `.hero-section` instead of `.bootcamp-hero`)
- Moved to page-specific CSS file
- Removed if they don't add value

## What I've Fixed Today

### ✅ Completed
1. **Fixed button hover animation** - Simplified from slide-in to clean fill
2. **Added logo ribbon** - Infinite scrolling animation with fade edges
3. **Updated consulting link** - Now uses cal.com
4. **Created utility classes**:
   - `.bg-contrast` - Navy Royal background for subtle contrast
   - `.card-dark` - Dark variant of cards
5. **Added reusable patterns to blueprint.scss**:
   - `.checklist-grid` - For pros/features lists
   - `.disqualify-grid` - For cons/disqualifications
   - `.meta-grid` / `.meta-box` - For stats/info boxes

## Recommendations Going Forward

### Priority 1: Consolidate Bootcamp Styles
**Action**: Remove the `<style>` block from bootcamp.qmd and use global classes
- Move essential patterns to blueprint.scss
- Use existing classes where possible
- Delete page-specific styling

### Priority 2: Simplify Section Classes
**Current**: `.section`, `.fixed-section`, `.problem-section`, `.transformation-section`, etc.
**Proposed**:
```scss
.section {
  padding: 4.5rem 1.5rem;
  margin: 0 auto;

  &.constrained {
    max-width: 1000px; // replaces .fixed-section
  }
}
```

### Priority 3: Reduce Nesting Depth
**Before**:
```scss
.logo-ribbon {
  .logo-scroll-wrapper {
    .logo-container {
      .logo-item {
        img { }
      }
    }
  }
}
```

**After**:
```scss
.logo-ribbon { }
.logo-ribbon-wrapper { }
.logo-ribbon-item {
  img { }
}
```

### Priority 4: Audit "Only Used Once" Classes
Review these selectors that appear only in bootcamp:
- `.guarantee-section` - Could this be `.cta-section.dark`?
- `.instructor-section` - Could this be `.section.bg-warm`?
- `.value-stack-section` - Could this use `.grid` instead?

## Honest Answer: "Do We Need This Many Specific Elements?"

**No, we don't.**

Here's what's actually needed:

### Essential Components (Keep)
- `.hero-content` - Unique hero layout
- `.card` / `.card-dark` - Content containers
- `.btn-primary` / `.btn-secondary` - CTAs
- `.section` - Page sections
- `.timeline` - Step-by-step displays
- `.testimonial-grid` / `.testimonial` - Social proof

### Utility Classes (Keep)
- `.bg-warm`, `.bg-navy`, `.bg-contrast` - Backgrounds
- `.text-center`, `.text-left` - Alignment
- `.mt-*`, `.mb-*` - Spacing

### Can Be Removed or Consolidated
- All the bootcamp-specific sections (8+ classes)
- Duplicate patterns in `<style>` blocks
- Overly-specific modifiers (`.bootcamp-hero` → just use `.hero-content`)

## Target State

**Ideal blueprint.scss size**: ~600-700 lines (40% reduction)
**Bootcamp page-specific CSS**: 0 lines (use global classes)
**Maintenance effort**: 50% less (single source of truth)

## Next Steps

1. Remove `<style>` block from bootcamp.qmd
2. Update bootcamp to use global classes
3. Test that nothing breaks
4. Remove unused selectors from blueprint.scss
5. Document remaining classes in CLAUDE.md

---

*This audit completed: 2025-12-08*
