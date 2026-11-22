# Mode: pdf — ATS-Optimised PDF Generation

## Two Modes

### Default Mode (no JD provided)
Generate a clean, general-purpose CV from `cv.md` using `templates/cv-template-minimal.html`.
No keyword injection. No JD required. Use all content from `cv.md` as-is.
Output: `output/cv-vishal-r-default-{YYYY-MM-DD}.pdf`

### Custom Mode (JD provided)
Full JD-tailored pipeline with keyword injection using `templates/cv-template-minimal.html`.
Adapts summary, bullets, and competencies to match the JD — ATS-optimised.
Output: `output/cv-{candidate}-{company}-{YYYY-MM-DD}.pdf`

**Detection:** If the user pastes a JD or URL → Custom Mode. If no JD → Default Mode.

---

## Default Mode Pipeline

1. Read `cv.md` as source of truth (and `article-digest.md` if it exists)
2. Read `config/profile.yml` for name, phone, email, LinkedIn, portfolio, location
3. Build full HTML from `templates/cv-template-minimal.html` — no keyword injection, no JD required
4. Use all content from `cv.md` as-is; section order: Summary → Skills → Work Experience → Education → Notable Achievements
5. Read `name` from `config/profile.yml` → normalise to kebab-case lowercase → `{candidate}`
6. Write HTML to scratchpad as `cv-{candidate}-minimal.html` (use absolute font paths from `fonts/` directory)
7. Run: `node generate-pdf.mjs /path/to/cv-{candidate}-minimal.html output/cv-{candidate}-default-{YYYY-MM-DD}.pdf --format=a4`
8. Report: PDF path, file size

---

## Custom Mode Pipeline

1. Read `cv.md` as source of truth (and `article-digest.md` if it exists)
2. Read `config/profile.yml` for contact details
3. Extract 15-20 keywords from the JD — focus on: role title variants, required skills, tools, domain terms, seniority signals
4. Detect JD language → CV language (EN default)
5. Detect company location → paper format: US/Canada → `letter`, rest of world → `a4`
6. **Rewrite Professional Summary** — inject top 5 JD keywords naturally; keep all metrics real (from `cv.md`)
7. **Reorder experience bullets** by relevance to the JD — most relevant bullets first within each role
8. **Adapt Skills section** — reorder skill categories to surface JD-relevant ones first; expand or contract terms to match JD vocabulary
9. **Inject keywords** into existing bullet points naturally — NEVER invent experience or metrics
10. Build full HTML from `templates/cv-template-minimal.html` with tailored content
11. Read `name` from `config/profile.yml` → normalise to kebab-case → `{candidate}`; extract company name from JD → `{company}`
12. Write HTML to scratchpad as `cv-{candidate}-{company}.html` (use absolute font paths)
13. Run: `node generate-pdf.mjs /path/to/cv-{candidate}-{company}.html output/cv-{candidate}-{company}-{YYYY-MM-DD}.pdf --format={letter|a4}`
14. Report: PDF path, file size, keyword coverage (keywords injected / total extracted)

---

## ATS Rules (clean parsing)

- Single-column layout — no sidebars, no parallel columns
- Standard section headers: "Professional Summary", "Work Experience", "Education", "Skills", "Notable Achievements"
- No text in images or SVGs
- No critical info in PDF headers/footers (ATS ignores them)
- UTF-8, selectable text (not rasterised)
- No nested tables
- Keywords distributed: Summary (top 5), first bullet of each role, Skills section

## Keyword Injection Strategy (ethical, truth-based)

Reframe real experience using the JD's exact vocabulary:
- JD says "AI workflow automation" and CV says "designed features that cut manual effort" → reframe as "AI workflow automation UX reducing manual effort by 50–70%"
- JD says "design systems" and CV says "component library" → reframe as "design system and component library powering 15+ modules"
- JD says "stakeholder management" and CV says "collaborated with cross-functional teams" → reframe as "stakeholder management across product, engineering, and AI strategy teams"

**NEVER add skills the candidate does not have. Only reframe real experience with the JD's exact terminology.**

## Template HTML

Use `templates/cv-template-minimal.html`. Replace `{{...}}` placeholders:

| Placeholder | Content |
|-------------|---------|
| `{{LANG}}` | `en` |
| `{{PAGE_WIDTH}}` | `8.5in` (letter) or `210mm` (A4) |
| `{{NAME}}` | From `config/profile.yml` — ALL CAPS for display |
| `{{HEADLINE}}` | Role title + location (e.g. "Senior UI/UX Designer \| Bangalore") |
| `{{PHONE}}` | From `config/profile.yml` |
| `{{EMAIL}}` | From `config/profile.yml` |
| `{{LINKEDIN_URL}}` | From `config/profile.yml` |
| `{{LINKEDIN_DISPLAY}}` | From `config/profile.yml` (short form e.g. `linkedin.com/in/vishal-r14`) |
| `{{PORTFOLIO_URL}}` | From `config/profile.yml` |
| `{{PORTFOLIO_DISPLAY}}` | From `config/profile.yml` (short form) |
| `{{SUMMARY_TEXT}}` | Summary paragraph (with `<strong>` tags for key terms) |
| `{{SKILLS}}` | `<div class="skill-line"><span class="skill-cat">Category:</span> skill1, skill2...</div>` × N |
| `{{EXPERIENCE}}` | Full work experience HTML using `.job`, `.job-company-row`, `.job-role-title`, `<ul>/<li>` |
| `{{EDUCATION}}` | `<div class="edu-item">` blocks |
| `{{ACHIEVEMENTS}}` | `<ul class="achievements-list"><li>` items |

---

## Canva CV Generation (optional)

If `config/profile.yml` has `cv.canva_resume_design_id` set, offer the user a choice before generating:
- **"HTML/PDF (fast, ATS-optimized)"** — existing flow above
- **"Canva CV (visual, design-preserving)"** — new flow below

If the user has no `cv.canva_resume_design_id`, skip this prompt and use the HTML/PDF flow.

### Canva workflow

#### Step 1 — Duplicate the base design

a. `export-design` the base design (using `cv.canva_resume_design_id`) as PDF → get download URL
b. `import-design-from-url` using that download URL → creates a new editable design (the duplicate)
c. Note the new `design_id` for the duplicate

#### Step 2 — Read the design structure

a. `get-design-content` on the new design → returns all text elements (richtexts) with their content
b. Map text elements to CV sections by content matching:
   - Look for the candidate's name → header section
   - Look for "Summary" or "Professional Summary" → summary section
   - Look for company names from cv.md → experience sections
   - Look for degree/school names → education section
   - Look for skill keywords → skills section
c. If mapping fails, show the user what was found and ask for guidance

#### Step 3 — Generate tailored content

Same content generation as the HTML flow (Steps 1-11 above):
- Rewrite Professional Summary with JD keywords + exit narrative
- Reorder experience bullets by JD relevance
- Select top competencies from JD requirements
- Inject keywords naturally (NEVER invent)

**IMPORTANT — Character budget rule:** Each replacement text MUST be approximately the same length as the original text it replaces (within ±15% character count). If tailored content is longer, condense it. The Canva design has fixed-size text boxes — longer text causes overlapping with adjacent elements. Count the characters in each original element from Step 2 and enforce this budget when generating replacements.

#### Step 4 — Apply edits

a. `start-editing-transaction` on the duplicate design
b. `perform-editing-operations` with `find_and_replace_text` for each section:
   - Replace summary text with tailored summary
   - Replace each experience bullet with reordered/rewritten bullets
   - Replace competency/skills text with JD-matched terms
   - Replace project descriptions with top relevant projects
c. **Reflow layout after text replacement:**
   After applying all text replacements, the text boxes auto-resize but neighboring elements stay in place. This causes uneven spacing between work experience sections. Fix this:
   1. Read the updated element positions and dimensions from the `perform-editing-operations` response
   2. For each work experience section (top to bottom), calculate where the bullets text box ends: `end_y = top + height`
   3. The next section's header should start at `end_y + consistent_gap` (use the original gap from the template, typically ~30px)
   4. Use `position_element` to move the next section's date, company name, role title, and bullets elements to maintain even spacing
   5. Repeat for all work experience sections
d. **Verify layout before commit:**
   - `get-design-thumbnail` with the transaction_id and page_index=1
   - Visually inspect the thumbnail for: text overlapping, uneven spacing, text cut off, text too small
   - If issues remain, adjust with `position_element`, `resize_element`, or `format_text`
   - Repeat until layout is clean
d. Show the user the final preview and ask for approval
e. `commit-editing-transaction` to save (ONLY after user approval)

#### Step 5 — Export and download PDF

a. `export-design` the duplicate as PDF (format: a4 or letter based on JD location)
b. **IMMEDIATELY** download the PDF using Bash:
   ```bash
   curl -sL -o "output/cv-{candidate}-{company}-canva-{YYYY-MM-DD}.pdf" "{download_url}"
   ```
   The export URL is a pre-signed S3 link that expires in ~2 hours. Download it right away.
c. Verify the download:
   ```bash
   file output/cv-{candidate}-{company}-canva-{YYYY-MM-DD}.pdf
   ```
   Must show "PDF document". If it shows XML or HTML, the URL expired — re-export and retry.
d. Report: PDF path, file size, Canva design URL (for manual tweaking)

#### Error handling

- If `import-design-from-url` fails → fall back to HTML/PDF pipeline with message
- If text elements can't be mapped → warn user, show what was found, ask for manual mapping
- If `find_and_replace_text` finds no matches → try broader substring matching
- Always provide the Canva design URL so the user can edit manually if auto-edit fails

## Post-generation

If the offer is already in the tracker (`data/applications.md`), update its PDF column from ❌ to ✅.
