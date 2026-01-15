# Google Tag (GT-5RMBWCN6)

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Google Tag |
| Tag Type | googtag |
| Tag ID | GT-5RMBWCN6 |
| Fires On | All Pages |
| Firing Option | Once Per Event |

## What It Does

- Loads the Google tag (gtag.js) library
- Initializes GA4 and Google Ads tracking
- Required for all other Google tags to function

## Current GTM Code (Equivalent)

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GT-5RMBWCN6"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GT-5RMBWCN6');
</script>
```

## Can Move to Page? ⚠️ PARTIAL

**Recommendation:** KEEP IN GTM

**Reason:** 
- This is the foundation for all Google tracking
- Moving to page would require also moving Conversion Linker logic
- GTM provides easier management of Google configurations
- Consent mode integration is simpler in GTM

**If you must move to page:**
- Place in `<head>` as high as possible
- Ensure it loads before any gtag() calls
- You'll need to manually handle consent mode

## Dependencies

- Required by: Google Ads Conversion, GA4 Events
- Requires: Nothing (base tag)

## Performance Impact

- **Load:** ~30KB (gtag.js)
- **Timing:** Async, non-blocking
- **Recommendation:** Keep as-is, async loading is optimal
