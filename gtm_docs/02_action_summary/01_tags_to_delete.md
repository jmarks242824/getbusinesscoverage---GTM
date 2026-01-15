# Tags to DELETE from GTM

---

## Delete Immediately

| Tag Name | Tag ID | Reason |
|----------|--------|--------|
| Bright Horizon Media - All in one Meta pixel | 78 | Duplicate |
| businessName | 34 | Duplicate |
| Compare Plans | 12 | No trigger |

---

## Review Then Delete

| Tag Name | Tag ID | Reason |
|----------|--------|--------|
| Facebook Pixel Lead Submit | 44 | Wrong pixel ID |
| Facebook Pixel Call | 47 | Wrong pixel ID |

---

## Details

### Bright Horizon Media - All in one Meta pixel
- **Why:** Fires same event as "Lead Submit Meta Pixel"
- **Impact:** Causes double Lead events to Meta

### businessName
- **Why:** Duplicate of "Name of your Business"
- **Impact:** Double GA4 events

### Compare Plans
- **Why:** No trigger assigned
- **Impact:** Never fires, dead code

### Facebook Pixel Lead Submit (Template)
- **Why:** Uses pixel ID 1421331895722314
- **Should be:** 1044730294266508

### Facebook Pixel Call (Template)
- **Why:** Uses pixel ID 1421331895722314
- **Should be:** 1044730294266508
