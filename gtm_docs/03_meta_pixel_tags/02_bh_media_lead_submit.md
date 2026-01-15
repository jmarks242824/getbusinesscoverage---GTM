# BH Media - Lead Submit

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Bright horizon Media - Lead Submit Meta Pixel |
| Tag ID | 62 |
| Type | Custom HTML |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | lead_submission |
| Trigger Type | Custom Event |
| Trigger ID | 76 |

---

## Configuration

| Property | Value |
|----------|-------|
| Pixel ID | 1044730294266508 |
| Event | Lead |
| Event ID | From dataLayer |
| Advanced Matching | Yes |

---

## Code

```javascript
var fb_event_id = {{DL - Event ID}};
var userEmail = {{DL - Email}};
var userPhone = {{DL - Phone}};
var userFirstName = {{DL - First Name}};
var userLastName = {{DL - Last Name}};
var userZip = {{DL - Zip}};
var userCity = {{DL - City}};
var userState = {{DL - State}};
var userCountry = {{DL - Country}};

if (userEmail && userPhone && fb_event_id) {
  fbq('track', 'Lead', {
    value: {{DL - Value}},
    currency: {{DL - Currency}}
  }, {
    eventID: fb_event_id,
    em: userEmail,
    ph: userPhone,
    fn: userFirstName,
    ln: userLastName,
    zp: userZip,
    ct: userCity,
    st: userState,
    country: userCountry
  });
}
```

---

## What It Does

1. Gets user data from dataLayer
2. Gets event_id for deduplication
3. Fires Lead event with value
4. Sends Advanced Matching data

---

## Action

| Decision | Reason |
|----------|--------|
| ❓ REVIEW | Can replace with Meta CAPI |
