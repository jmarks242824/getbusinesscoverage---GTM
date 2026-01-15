# GA4: Employees

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Employees |
| Tag ID | 19 |
| Type | GA4 Event (gaawe) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Click |
| Element ID | Regex match |
| Trigger ID | 33 |

---

## Trigger Regex

```
numberOfEmployees_*
```

---

## Event Config

| Property | Value |
|----------|-------|
| Event Name | employees |
| Measurement ID | G-1Z5EC7EX37 |

---

## What It Does

Fires GA4 event when user selects employee count.

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/07_ga4_form_events_code.md |

---

## Replacement Mapping

```javascript
'numberOfEmployees_': 'employees'  // prefix match
```
