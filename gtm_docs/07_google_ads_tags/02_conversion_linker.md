# Conversion Linker

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Conversion Linker |
| Tag ID | 3 |
| Type | gclidw |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | All Pages |
| Trigger Type | Page View |
| Trigger ID | 2147479573 |

---

## Configuration

| Property | Value |
|----------|-------|
| Enable Cross Domain | true |
| Enable URL Passthrough | false |
| Enable Cookie Overrides | false |

---

## What It Does

1. Captures GCLID from URL parameters
2. Stores GCLID in first-party cookies
3. Enables cross-domain tracking
4. Links clicks to conversions

---

## Why Keep in GTM

- Required for Google Ads conversion tracking
- Works with Google Ads Conversion tag
- Cannot easily replicate outside GTM

---

## Action

| Decision | Reason |
|----------|--------|
| ✅ KEEP IN GTM | Required for GCLID tracking |
