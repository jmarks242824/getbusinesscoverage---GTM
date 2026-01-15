# GA4: businessName (DUPLICATE)

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | businessName |
| Tag ID | 34 |
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
| Event Name | businessName |
| Measurement ID | G-1Z5EC7EX37 |

---

## ⚠️ PROBLEM

**Same trigger as Tag ID 13 "Name of your Business"**

Both tags fire on: `next_businessName` click

Result: Double GA4 events for same action

---

## Action

| Decision | Reason |
|----------|--------|
| ❌ DELETE | Duplicate of Tag 13 |
