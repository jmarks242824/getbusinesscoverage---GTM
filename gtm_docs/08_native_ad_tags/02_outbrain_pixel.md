# Outbrain Pixel

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Outbrain Pixel |
| Tag ID | 53 |
| Type | Community Template (cvt_221316038_51) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Outbrain PageView Trigger |
| Trigger Type | Page View |
| Trigger ID | 52 |

---

## Configuration

| Property | Value |
|----------|-------|
| Marketer ID | 0084a3fde60ab652bf9cd5cc0158baee25 |
| Pixel Type | page_view_pixel |

---

## External Script

| Property | Value |
|----------|-------|
| Loads | obtp.js |
| Size | ~15KB |

---

## What It Does

1. Loads Outbrain pixel (obtp.js)
2. Fires PageView event
3. Enables Outbrain remarketing

---

## Action

| Decision | Reason |
|----------|--------|
| ❓ REVIEW | Keep if running Outbrain campaigns |

---

## S2S Alternative

If you want to remove client-side pixel:

```
https://tr.outbrain.com/events/conversion/0084a3fde60ab652bf9cd5cc0158baee25?ob_click_id={obclid}&value=150
```
