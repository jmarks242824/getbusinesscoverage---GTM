# BH Media - InitiateCheckout

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Bright Horizon Media Meta Pixel - InitiateCheckout |
| Tag ID | 58 |
| Type | Custom HTML |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Page Path = /Questions |
| Trigger Type | Page View |
| Trigger ID | 57 |

---

## Code

```html
<script>
  fbq('track', 'InitiateCheckout');
</script>
```

---

## What It Does

1. Fires InitiateCheckout event
2. Only on /Questions page
3. Indicates user started form

---

## Dependencies

Requires "BH Media - All Page Pixel" to fire first.

---

## Action

| Decision | Reason |
|----------|--------|
| ❓ REVIEW | Can handle via CAPI or page script |
