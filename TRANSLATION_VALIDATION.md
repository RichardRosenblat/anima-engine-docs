# Translation Validation Guide

This document provides scripts and guidelines for validating translation coverage and quality in the ANIMA Engine documentation.

---

## Quick Validation Scripts

### 1. Check for Missing Translations

```bash
#!/bin/bash
# Check which English files are missing translations

for lang in pt-br es-mx; do
    echo "=== Missing in $lang ==="
    find . -type f -name "*.md" ! -path "*/.git/*" ! -path "*/pt-br/*" ! -path "*/es-mx/*" | while read f; do
        translated="./$lang/${f#./}"
        [ ! -f "$translated" ] && echo "  ✗ $f"
    done
    echo ""
done
```

### 2. Check for Orphaned Translations

```bash
#!/bin/bash
# Find translations that don't have corresponding English source

for lang in pt-br es-mx; do
    echo "=== Orphaned in $lang ==="
    find "./$lang" -type f -name "*.md" | while read f; do
        english="./${f#./$lang/}"
        [ ! -f "$english" ] && echo "  ! $f (no English source)"
    done
    echo ""
done
```

### 3. Compare File Counts

```bash
#!/bin/bash
# Count files in each directory

echo "File counts by directory:"
echo ""

count_md_files() {
    find "$1" -type f -name "*.md" 2>/dev/null | wc -l
}

echo "English (root):     $(count_md_files .)"
echo "Portuguese (pt-br): $(count_md_files ./pt-br)"
echo "Spanish (es-mx):    $(count_md_files ./es-mx)"
```

### 4. List Translation Coverage by Directory

```bash
#!/bin/bash
# Show translation status per directory

for dir in adr annoucements architecture foundations governance roadmaps safety specs vision; do
    en_count=$(find "./$dir" -maxdepth 1 -type f -name "*.md" 2>/dev/null | wc -l)
    pt_count=$(find "./pt-br/$dir" -maxdepth 1 -type f -name "*.md" 2>/dev/null | wc -l)
    es_count=$(find "./es-mx/$dir" -maxdepth 1 -type f -name "*.md" 2>/dev/null | wc -l)
    
    [ $en_count -gt 0 ] && echo "$dir: EN=$en_count, PT=$pt_count, ES=$es_count"
done
```

---

## Manual Validation Checklist

When reviewing translations, check:

### Structure
- [ ] Directory structure matches English version
- [ ] Filenames are identical (case-sensitive)
- [ ] No orphaned files (translations without English source)
- [ ] No missing translations (English files without translations)

### Content Quality
- [ ] Headers and titles are translated
- [ ] Body content is translated
- [ ] Code examples remain in English
- [ ] Technical terms are consistently translated
- [ ] Links point to translated versions when available
- [ ] Formatting (markdown) is preserved

### Technical Accuracy
- [ ] URNs and identifiers are NOT translated
- [ ] Schema names are NOT translated
- [ ] API endpoints are NOT translated
- [ ] Code snippets are NOT translated
- [ ] File paths are NOT translated (except in explanatory text)

### Metadata
- [ ] Front matter (if any) is appropriately translated
- [ ] Comments in code are translated
- [ ] Alt text for images is translated

---

## Common Translation Issues

### Issue 1: Inconsistent Technical Terms

**Problem**: The same English term translated differently across files.

**Example**:
- File A: "capability" → "capacidade"
- File B: "capability" → "habilidade"

**Solution**: Create a glossary of standard translations for technical terms.

### Issue 2: Broken Internal Links

**Problem**: Links still point to English files instead of translated versions.

**Example**:
```markdown
<!-- Wrong -->
[Architecture](../architecture/anima-architecture.md)

<!-- Correct in pt-br -->
[Arquitetura](../architecture/anima-architecture.md)
```

**Solution**: Update link text to translated version while keeping path correct.

### Issue 3: Translated Code or Identifiers

**Problem**: Code examples or technical identifiers are translated when they shouldn't be.

**Example**:
```markdown
<!-- Wrong -->
Use the `$raiz` scope instead of `$root`

<!-- Correct -->
Use the `$root` scope (raiz)
```

**Solution**: Keep all code, URNs, and identifiers in English.

### Issue 4: Missing Files

**Problem**: Translation directories missing files present in English.

**Solution**: Use the scripts above to identify and create missing translations.

---

## Automation Recommendations

### CI/CD Integration

Consider adding automated checks to your CI/CD pipeline:

```yaml
# Example GitHub Actions workflow snippet
- name: Check Translation Coverage
  run: |
    ./scripts/check-translations.sh
    if [ $? -ne 0 ]; then
      echo "Translation coverage check failed"
      exit 1
    fi
```

### Pre-commit Hooks

Add a pre-commit hook to remind about translations:

```bash
#!/bin/bash
# .git/hooks/pre-commit

# Check if any English .md files were modified
modified_md=$(git diff --cached --name-only --diff-filter=AM | grep -E '\.md$' | grep -v '^pt-br/' | grep -v '^es-mx/')

if [ -n "$modified_md" ]; then
    echo ""
    echo "⚠️  English documentation modified. Remember to update translations:"
    echo "$modified_md" | sed 's/^/  - /'
    echo ""
    echo "Translation files to update:"
    echo "$modified_md" | sed 's|^|  - pt-br/|'
    echo "$modified_md" | sed 's|^|  - es-mx/|'
    echo ""
fi
```

---

## Translation Workflow

### For New Documentation

1. **Write** English version first
2. **Review** English version for technical accuracy
3. **Translate** to pt-br and es-mx
4. **Review** translations for accuracy and consistency
5. **Commit** all three versions together

### For Updated Documentation

1. **Update** English version
2. **Note** what changed (for translators)
3. **Update** translations with the same changes
4. **Review** to ensure consistency
5. **Commit** all updates together

### For New Languages

1. **Create** new language directory (e.g., `fr-fr/`)
2. **Copy** directory structure from `pt-br/` or `es-mx/`
3. **Translate** all files following this guide
4. **Update** root README.md to include new language
5. **Document** language-specific terminology choices

---

## Quality Metrics

### Coverage Metrics

Track these metrics for each language:

- **File Coverage**: % of English files that have translations
- **Directory Coverage**: % of directories that are complete
- **Priority Coverage**: % of high-priority files translated

### Current Status (as of last check)

| Metric | pt-br | es-mx |
|--------|-------|-------|
| File Coverage | 88% | 88% |
| High Priority Files | 0/2 | 0/2 |
| Medium Priority Files | 0/1 | 0/1 |

---

## Tools and Resources

### Recommended Tools

- **DeepL**: High-quality machine translation (post-edit required)
- **Google Translate**: Quick reference (post-edit required)
- **CAT Tools**: Translation memory tools for consistency
  - OmegaT (free, open-source)
  - Poedit (for gettext projects)
  - Weblate (web-based, collaborative)

### Best Practices

1. **Never use raw machine translation** - always post-edit
2. **Maintain glossary** of technical terms
3. **Have native speakers review** when possible
4. **Test internal links** after translation
5. **Keep commits atomic** - one language per commit when possible

---

## Appendix: Current Missing Translations

As of the last check (see TRANSLATION_REPORT.md for details):

### High Priority
- `README.md` (646 lines)
- `specs/json_reference_system.md` (411 lines)

### Medium Priority
- `architecture/directry-overview.md` (214 lines) - Note: typo in filename

### Review Needed
- `future-posts.md` (61 lines) - Internal draft, may not need translation

---

## Contact

For questions about translation standards or to report issues:
- Open an issue in the repository
- Tag with `documentation` and `translation` labels
- Reference this guide in your report

---

**Last Updated**: January 11, 2026
