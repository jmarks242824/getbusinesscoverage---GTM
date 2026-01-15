# Google Ads Conversion Lead Submit

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Google Ads Conversion Lead Submit |
| Tag ID | 28 |
| Type | awud (Google Ads User Data) |

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
| Conversion ID | 17097160893 |
| Enable Conversion Linker | true |

---

## User Data Variable

| Field | dataLayer Variable |
|-------|-------------------|
| Email | {{DL - Email}} |
| Phone | {{DL - Phone}} |
| First Name | {{DL - First Name}} |
| Last Name | {{DL - Last Name}} |
| City | {{DL - City}} |
| State/Region | {{DL - State}} |
| Postal Code | {{DL - Zip}} |
| Country | {{DL - Country}} |

---

## What It Does

1. Fires Google Ads conversion on lead submission
2. Sends hashed user data (Enhanced Conversions)
3. Links to click via Conversion Linker

---

## Dependencies

- Requires `lead_submission` dataLayer event
- Requires Conversion Linker tag
- User data pushed by Everflow Conversion Script

---

## Why Keep in GTM

- Enhanced Conversions requires GTM integration
- User data hashing handled automatically
- Best practice for Google Ads tracking

---

## Action

| Decision | Reason |
|----------|--------|
| ✅ KEEP IN GTM | Enhanced Conversions |
