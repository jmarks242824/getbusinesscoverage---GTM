# GA4: Name of your Business

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Name of your Business |
| Tag ID | 13 |
| Type | GA4 Event (gaawe) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Click |
| Element ID | next_businessName |
| Trigger ID | 36 |

---

## Event Config

| Property | Value |
|----------|-------|
| Event Name | name_of_your_business |
| Measurement ID | G-1Z5EC7EX37 |

---

## What It Does

Fires GA4 event when user clicks "Next" after entering business name.

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/07_ga4_form_events_code.md |

---

## Replacement Mapping

```javascript
'next_businessName': 'name_of_your_business'
```
