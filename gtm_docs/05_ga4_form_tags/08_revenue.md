# GA4: Revenue

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Revenue |
| Tag ID | 20 |
| Type | GA4 Event (gaawe) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Click |
| Element ID | Regex match |
| Trigger ID | 41 |

---

## Trigger Regex

```
annualRevenue_50000|annualRevenue_250000|annualRevenue_750000|annualRevenue_3000000
```

---

## Event Config

| Property | Value |
|----------|-------|
| Event Name | revenue |
| Measurement ID | G-1Z5EC7EX37 |

---

## What It Does

Fires GA4 event when user selects annual revenue range.

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/07_ga4_form_events_code.md |

---

## Replacement Mapping

```javascript
'annualRevenue_': 'revenue'  // prefix match
```
