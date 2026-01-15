# Conversion Linker

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Conversion Linker |
| Tag Type | gclidw |
| Fires On | All Pages |
| Cross-Domain | Enabled |
| URL Passthrough | Disabled |
| Cookie Overrides | Disabled |

## Cross-Domain Domains (58 total)

```
fastbusinessquote.com
fast-business-quote-business.vercel.app
fast-business-quote-business-gq4z43qg9.vercel.app
fast-business-quote-business-n0srndpic.vercel.app
... (54 more Vercel preview domains)
```

## What It Does

- Captures GCLID (Google Click ID) from URL parameters
- Stores GCLID in first-party cookie for conversion attribution
- Enables cross-domain tracking between listed domains
- Critical for Google Ads conversion accuracy

## Can Move to Page? ❌ NO

**Recommendation:** KEEP IN GTM

**Reason:**
- Conversion Linker is a GTM-native tag type
- No direct JavaScript equivalent
- Cross-domain configuration is complex
- Google manages this functionality within GTM ecosystem

**Alternative if needed:**
- Use gtag.js linker configuration (complex)
- Would require manual cookie management

```javascript
// This is NOT a direct replacement - just shows complexity
gtag('set', 'linker', {
  'domains': ['fastbusinessquote.com', 'fast-business-quote-business.vercel.app'],
  'decorate_forms': true,
  'accept_incoming': true
});
```

## Dependencies

- Required by: Google Ads Conversion tracking
- Requires: Google Tag

## Performance Impact

- **Load:** Minimal (uses gtag.js already loaded)
- **Timing:** Fires on page load
- **Recommendation:** Essential, cannot remove

## Notes

- The large number of Vercel preview domains suggests active development
- Consider cleaning up old preview domains periodically
- Only production domain (fastbusinessquote.com) is truly necessary for live tracking
