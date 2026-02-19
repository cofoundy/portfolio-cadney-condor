# QA Report: Cadney Veliana Condor Mendez

**Date:** 2026-02-19
**URL:** https://cadney-condor.cofoundy.dev
**Status:** FAIL

## Data Validation
- [x] Name matches source ("Cadney Veliana Condor Mendez" in config, page, and source)
- [x] Email matches source (Cadneycm@gmail.com -- Cloudflare email protection obscures it in HTML but correctly configured)
- [x] LinkedIn matches source (https://www.linkedin.com/in/veliana-condor-mendez-948794314/)
- [x] Job title from source ("Ingeniera Fisica | Investigadora en IA & Sistemas Embebidos" -- consistent with student/researcher profile)
- [x] Companies listed exist in source (Smart Machines Lab CTIC, Hoppnic, APU SPACE UNI, ASME UNI, Facultad de Ciencias UNI)
- [x] Education institution matches source (UNI - Universidad Nacional de Ingenieria)
- [x] Skills match source (Python, C++, MATLAB, SIMULINK, AutoCAD, SolidWorks, Inventor + ML/Signals/Embedded)
- [x] No hallucinated data detected

## Clean Deploy
- [x] No "Powered by" / "Made with" / "Built with" watermarks
- [x] No "Lorem ipsum" / "Your name here" / placeholder text
- [x] No "View source" / "View on GitHub" / template links
- [x] No Astro logo / Vercel badge visible to users
- [x] No "undefined" or "null" visible in text content
- [ ] **FAIL: Empty Twitter and GitHub icon links in footer** (no href, render as dead clickable icons)

## Technical
- [x] CSS loads (HTTP 200) -- /_astro/index.BGRXo_qT.css
- [x] Favicon loads (HTTP 200) -- /favicon.svg (note: href="//favicon.svg" works via server normalization)
- [x] No profile image expected (no photo provided, no img tag rendered -- correct behavior)
- [x] No critical console errors expected (static Astro site, no JS dependencies)

## Issues Found

### Issue 1: EMPTY SOCIAL ICONS IN FOOTER (Severity: Medium)
- **Location:** Footer section, bottom of page
- **Problem:** Twitter (X) and GitHub icons render in the footer without href attributes because `siteConfig.social.twitter` and `siteConfig.social.github` are undefined. The Hero component correctly uses conditional rendering (`siteConfig.social?.twitter &&`) but Footer.astro always renders all 4 social icons unconditionally.
- **Impact:** Users see clickable Twitter and GitHub icons that do nothing when clicked. This looks unprofessional.
- **Fix:** Add conditional rendering in `src/components/Footer.astro` lines 73-119, wrapping Twitter and GitHub links with `{siteConfig.social?.twitter && ( ... )}` and `{siteConfig.social?.github && ( ... )}` respectively, matching the pattern used in Hero.astro.

### Issue 2: FAVICON DOUBLE-SLASH PATH (Severity: Low)
- **Location:** `<link rel="icon" href="//favicon.svg">`
- **Problem:** `index.astro` uses `${import.meta.env.BASE_URL}/favicon.svg` which resolves to `//favicon.svg` when BASE_URL is `/`. Technically this is a protocol-relative URL pointing to the domain `favicon.svg`.
- **Impact:** Currently works because Cloudflare normalizes the double slash. No user-visible issue.
- **Fix:** Optional. Could use a simple `/favicon.svg` or handle the trailing slash in BASE_URL.

## Evidence
- qa-checklist.md (this file)
- No screenshot tool available in this environment; manual visual verification recommended.
