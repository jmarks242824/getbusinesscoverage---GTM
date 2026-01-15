# Tags to KEEP in GTM

---

## Must Keep

| Tag Name | Tag ID | Reason |
|----------|--------|--------|
| Conversion Linker | 3 | Required for GCLID |
| Google Ads Conversion Lead Submit | 28 | Enhanced Conversions |

---

## Why These Stay in GTM

### Conversion Linker
- Captures GCLID from URL
- Stores in first-party cookie
- Required for Google Ads attribution
- Cannot replicate outside GTM easily

### Google Ads Conversion Lead Submit
- Uses Enhanced Conversions
- Sends hashed user data to Google
- Integrates with GTM's user data variable
- Best kept in GTM for data handling

---

## Final GTM Container

| Before | After |
|--------|-------|
| 27 tags | 2 tags |
| 20+ triggers | 2 triggers |

**92% reduction**
