# GA4: Payroll

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Payroll |
| Tag ID | 21 |
| Type | GA4 Event (gaawe) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Click |
| Form ID | Regex match |
| Trigger ID | 32 |

---

## Trigger Regex

```
annualPayroll_*
```

---

## Event Config

| Property | Value |
|----------|-------|
| Event Name | payroll |
| Measurement ID | G-1Z5EC7EX37 |

---

## What It Does

Fires GA4 event when user selects annual payroll range.

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/07_ga4_form_events_code.md |

---

## Replacement Mapping

```javascript
'annualPayroll_': 'payroll'  // prefix match
```
