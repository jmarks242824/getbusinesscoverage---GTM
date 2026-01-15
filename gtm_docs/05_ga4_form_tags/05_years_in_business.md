# GA4: Years in Business

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Years in Business |
| Tag ID | 17 |
| Type | GA4 Event (gaawe) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Click |
| Element ID | Regex match |
| Trigger ID | 39 |

---

## Trigger Regex

```
yearsInBusiness_0|yearsInBusiness_3|yearsInBusiness_8|yearsInBusiness_12
```

---

## Event Config

| Property | Value |
|----------|-------|
| Event Name | years_in_business |
| Measurement ID | G-1Z5EC7EX37 |

---

## What It Does

Fires GA4 event when user selects years in business.

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/07_ga4_form_events_code.md |

---

## Replacement Mapping

```javascript
'yearsInBusiness_': 'years_in_business'  // prefix match
```
