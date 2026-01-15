# Taboola Pixel

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Taboola Pixel |
| Tag Type | cvt_PLW64 (Community Template) |
| Fires On | All Pages (DOM Ready) |

## What It Does

- Loads Taboola tracking pixel
- Enables Taboola native advertising attribution
- Supports remarketing audiences

## Can Move to Page? ✅ YES

**Recommendation:** MOVE TO PAGE

**Reason:**
- Simple standalone pixel
- No GTM dependencies
- Standard implementation

**Implementation:**

```html
<script type='text/javascript'>
  window._tfa = window._tfa || [];
  window._tfa.push({notify: 'event', name: 'page_view', id: YOUR_TABOOLA_PIXEL_ID});
  !function (t, f, a, x) {
    if (!document.getElementById(x)) {
      t.async = 1;t.src = a;t.id=x;f.parentNode.insertBefore(t, f);
    }
  }(document.createElement('script'),
    document.getElementsByTagName('script')[0],
    '//cdn.taboola.com/libtrc/unip/YOUR_ACCOUNT_ID/tfa.js',
    'tb_tfa_script');
</script>
```

## Page Placement

```html
<head>
  <!-- Taboola Pixel -->
  <script type='text/javascript'>
    window._tfa = window._tfa || [];
    window._tfa.push({notify: 'event', name: 'page_view', id: YOUR_PIXEL_ID});
    !function (t, f, a, x) {
      if (!document.getElementById(x)) {
        t.async = 1;t.src = a;t.id=x;f.parentNode.insertBefore(t, f);
      }
    }(document.createElement('script'),
      document.getElementsByTagName('script')[0],
      '//cdn.taboola.com/libtrc/unip/YOUR_ACCOUNT_ID/tfa.js',
      'tb_tfa_script');
  </script>
</head>
```

## Conversion Tracking

To track lead conversions with Taboola:

```javascript
// Fire on lead submission
window._tfa = window._tfa || [];
window._tfa.push({
  notify: 'event',
  name: 'lead',
  id: YOUR_PIXEL_ID,
  revenue: 150.00,
  currency: 'USD'
});
```

## Dependencies

- Required by: Nothing
- Requires: Nothing (standalone)

## Performance Impact

- **Load:** ~20KB (tfa.js)
- **Timing:** Async, non-blocking
- **Recommendation:** Move to page

## Notes

- Pixel ID needs to be extracted from GTM template configuration
- Currently only tracks PageView, no conversion events
- Consider adding lead conversion tracking
- Taboola is a native advertising platform (content recommendation)
- If not running Taboola campaigns, consider removing
