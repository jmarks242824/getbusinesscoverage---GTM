# Everflow Conversion Script

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Everflow Conversion Script |
| Tag ID | 10 |
| Type | Custom HTML |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | All Pages |
| Trigger Type | DOM Ready |
| Trigger ID | 2147479553 |

---

## ⚠️ Current Issue

Tag may have wrong trigger (ID 26 = Click instead of DOM Ready).

**Should fire on:** All Pages (DOM Ready)  
**Why:** Needs to intercept fetch() BEFORE form submits

---

## Conversion Config

| Property | Value |
|----------|-------|
| aid | 373 |
| amount | 150.00 |

---

## Data Captured

| Field | Source |
|-------|--------|
| adv1 | first_name |
| adv2 | last_name |
| adv3 | email |
| adv4 | phone |
| adv5 | zip |
| adv6 | tracking ID |

---

## Code Summary

```javascript
(function() {
  var originalFetch = window.fetch;
  
  window.fetch = function() {
    // Intercept all fetch calls
    // Detect NextInsure API call
    // Extract user data
    // Fire EF.conversion()
    // Push lead_submission to dataLayer
  };
})();
```

---

## What It Does

1. Intercepts fetch() globally
2. Detects NextInsure API submissions
3. Extracts user data from request body
4. Fires Everflow conversion (aid: 373)
5. Pushes `lead_submission` event to dataLayer
6. Generates event_id for deduplication

---

## dataLayer Event Pushed

```javascript
{
  'event': 'lead_submission',
  'event_id': 'conv_123456_abc',
  'value': 150,
  'currency': 'USD',
  'user_data': {
    'email': '...',
    'phone': '...',
    'first_name': '...',
    'last_name': '...',
    'zip': '...',
    'city': '...',
    'state': '...',
    'country': 'US'
  }
}
```

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/09_everflow_conversion_code.md |
