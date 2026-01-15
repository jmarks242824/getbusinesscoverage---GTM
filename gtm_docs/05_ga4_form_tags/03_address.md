# GA4: Address

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Address |
| Tag ID | 15 |
| Type | GA4 Event (gaawe) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Click |
| Element ID | next_businessAddress |
| Trigger ID | 38 |

---

## Event Config

| Property | Value |
|----------|-------|
| Event Name | address |
| Measurement ID | G-1Z5EC7EX37 |

---

## What It Does

Fires GA4 event when user completes address step.

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/07_ga4_form_events_code.md |

---

## Replacement Mapping

```javascript
'next_businessAddress': 'address'
```
