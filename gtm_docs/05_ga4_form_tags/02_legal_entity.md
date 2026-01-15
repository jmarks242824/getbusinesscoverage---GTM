# GA4: Legal Entity

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Legal Entity |
| Tag ID | 14 |
| Type | GA4 Event (gaawe) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Click |
| Element ID | Regex match |
| Trigger ID | 37 |

---

## Trigger Regex

```
legalEntity_Sole Proprietorship|legalEntity_Partnership|legalEntity_LLC|legalEntity_S Corporation|legalEntity_Other
```

---

## Event Config

| Property | Value |
|----------|-------|
| Event Name | legal_entity |
| Measurement ID | G-1Z5EC7EX37 |

---

## What It Does

Fires GA4 event when user selects a legal entity type.

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/07_ga4_form_events_code.md |

---

## Replacement Mapping

```javascript
'legalEntity_': 'legal_entity'  // prefix match
```
