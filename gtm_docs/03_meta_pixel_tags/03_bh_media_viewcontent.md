# BH Media - ViewContent

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Bright Horizon Media- Meta Pixel - ViewContent |
| Tag ID | 56 |
| Type | Custom HTML |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Page Path = /Trucking OR / |
| Trigger Type | Page View |
| Trigger ID | 55 |

---

## Code

```html
<script>
  fbq('track', 'ViewContent');
</script>
```

---

## What It Does

1. Fires ViewContent event
2. Only on landing pages (/ or /Trucking)
3. Depends on base pixel being loaded

---

## Dependencies

Requires "BH Media - All Page Pixel" to fire first.

---

## Action

| Decision | Reason |
|----------|--------|
| ❓ REVIEW | Can handle via CAPI or page script |
