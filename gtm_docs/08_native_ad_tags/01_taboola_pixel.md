# Taboola Pixel

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Taboola Pixel |
| Tag ID | 50 |
| Type | Community Template (cvt_PLW64) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | All Pages |
| Trigger Type | DOM Ready |
| Trigger ID | 2147479553 |

---

## Configuration

| Property | Value |
|----------|-------|
| Account ID | 1901590 |
| Pixel Type | page_view |
| Add Revenue | false |

---

## External Script

| Property | Value |
|----------|-------|
| Loads | tfa.js |
| Size | ~20KB |

---

## What It Does

1. Loads Taboola pixel (tfa.js)
2. Fires PageView on every page
3. Enables Taboola remarketing

---

## Action

| Decision | Reason |
|----------|--------|
| ❓ REVIEW | Keep if running Taboola campaigns |

---

## S2S Alternative

If you want to remove client-side pixel:

```
https://trc.taboola.com/actions-handler/log/3/s2s-action?click-id={tblci}&name=lead&revenue=150
```
