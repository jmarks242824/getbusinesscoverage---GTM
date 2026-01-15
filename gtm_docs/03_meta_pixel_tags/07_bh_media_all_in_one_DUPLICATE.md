# BH Media - All in One (DUPLICATE)

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Bright Horizon Media - All in one Meta pixel |
| Tag ID | 78 |
| Type | Custom HTML |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | lead_submission |
| Trigger Type | Custom Event |
| Trigger ID | 76 |

---

## ⚠️ PROBLEM

This tag fires on the **SAME trigger** as "Lead Submit Meta Pixel" (Tag ID 62).

Both tags:
- Fire on `lead_submission` event
- Send Lead event to same pixel
- Result: **Double Lead events**

---

## Code Summary

```javascript
// Gets user data from dataLayer
// Manually hashes with SHA-256 (unnecessary)
// Fires fbq('track', 'Lead', {...})
```

---

## Action

| Decision | Reason |
|----------|--------|
| ❌ DELETE | Duplicate of Tag 62 |

---

## Impact of Deletion

None - Tag 62 handles the same functionality.
