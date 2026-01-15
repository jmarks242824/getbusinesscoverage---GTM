# Phase 1: Add Page Scripts

---

## Timeline

Day 1

---

## All Pages - Head

- [ ] Add Click ID Capture script to `<head>`
- [ ] Add Google Tag script to `<head>`
- [ ] Add Microsoft Clarity script to `<head>`
- [ ] Update Clarity ID for correct site
- [ ] Add Everflow Click script to `<head>`

---

## All Pages - Body

- [ ] Add GA4 Form Events script before `</body>`
- [ ] Add Tel Click Tracking script before `</body>`
- [ ] Add Everflow Conversion script before `</body>`

---

## Thank You Page Only

- [ ] Add Thank You Page script before `</body>`

---

## Clarity IDs

| Site | Use This ID |
|------|-------------|
| fastbusinessquote.com | rrejdlholl |
| getbusinesscoverage.com | uuqr6vibpo |

---

## File Order in Body

```html
<!-- Before </body> -->
<!-- 1. GA4 Form Events + Tel Click -->
<!-- 2. Everflow Conversion -->
<!-- 3. Thank You Page Script (if thank you page) -->
</body>
```
