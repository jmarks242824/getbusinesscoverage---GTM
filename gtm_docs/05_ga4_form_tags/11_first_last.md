# GA4: First Last

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | First Last |
| Tag ID | 23 |
| Type | GA4 Event (gaawe) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Click |
| Form ID | next_name |
| Trigger ID | 30 |

---

## Event Config

| Property | Value |
|----------|-------|
| Event Name | First_Last |
| Measurement ID | G-1Z5EC7EX37 |

---

## What It Does

Fires GA4 event when user completes name step.

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/07_ga4_form_events_code.md |

---

## Replacement Mapping

```javascript
'next_name': 'First_Last'
```
