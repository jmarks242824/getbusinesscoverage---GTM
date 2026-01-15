# GA4: Partners Owners

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Partners Owners |
| Tag ID | 18 |
| Type | GA4 Event (gaawe) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Click |
| Element ID | Regex match |
| Trigger ID | 40 |

---

## Trigger Regex

```
numberOfOwners_*
```

---

## Event Config

| Property | Value |
|----------|-------|
| Event Name | partners_owners |
| Measurement ID | G-1Z5EC7EX37 |

---

## What It Does

Fires GA4 event when user selects number of partners/owners.

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/07_ga4_form_events_code.md |

---

## Replacement Mapping

```javascript
'numberOfOwners_': 'partners_owners'  // prefix match
```
