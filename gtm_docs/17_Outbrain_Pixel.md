# Outbrain Pixel

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Outbrain Pixel |
| Tag Type | cvt_221316038_51 (Custom Template) |
| Fires On | Outbrain PageView Trigger (All Pages) |

## What It Does

- Loads Outbrain tracking pixel
- Enables Outbrain native advertising attribution
- Supports remarketing audiences

## Can Move to Page? ✅ YES

**Recommendation:** MOVE TO PAGE

**Reason:**
- Simple standalone pixel
- No GTM dependencies
- Standard implementation

**Implementation:**

```html
<script type="text/javascript">
  !function(_window, _document) {
    var OB_ADV_ID = 'YOUR_OUTBRAIN_PIXEL_ID';
    if (_window.obApi) {
      var toArray = function(object) {
        return Object.prototype.toString.call(object) === '[object Array]' ? object : [object];
      };
      _window.obApi.marketerId = toArray(_window.obApi.marketerId).concat(toArray(OB_ADV_ID));
      return;
    }
    var api = _window.obApi = function() {
      api.dispatch ? api.dispatch.apply(api, arguments) : api.queue.push(arguments);
    };
    api.version = '1.1';
    api.loaded = true;
    api.marketerId = OB_ADV_ID;
    api.queue = [];
    var tag = _document.createElement('script');
    tag.async = true;
    tag.src = '//amplify.outbrain.com/cp/obtp.js';
    tag.type = 'text/javascript';
    var script = _document.getElementsByTagName('script')[0];
    script.parentNode.insertBefore(tag, script);
  }(window, document);

  obApi('track', 'PAGE_VIEW');
</script>
```

## Page Placement

```html
<head>
  <!-- Outbrain Pixel -->
  <script type="text/javascript">
    !function(_window, _document) {
      var OB_ADV_ID = 'YOUR_OUTBRAIN_PIXEL_ID';
      if (_window.obApi) {
        var toArray = function(object) {
          return Object.prototype.toString.call(object) === '[object Array]' ? object : [object];
        };
        _window.obApi.marketerId = toArray(_window.obApi.marketerId).concat(toArray(OB_ADV_ID));
        return;
      }
      var api = _window.obApi = function() {
        api.dispatch ? api.dispatch.apply(api, arguments) : api.queue.push(arguments);
      };
      api.version = '1.1';
      api.loaded = true;
      api.marketerId = OB_ADV_ID;
      api.queue = [];
      var tag = _document.createElement('script');
      tag.async = true;
      tag.src = '//amplify.outbrain.com/cp/obtp.js';
      tag.type = 'text/javascript';
      var script = _document.getElementsByTagName('script')[0];
      script.parentNode.insertBefore(tag, script);
    }(window, document);

    obApi('track', 'PAGE_VIEW');
  </script>
</head>
```

## Conversion Tracking

To track lead conversions with Outbrain:

```javascript
// Fire on lead submission
obApi('track', 'Lead', {
  value: 150.00,
  currency: 'USD'
});
```

## Dependencies

- Required by: Nothing
- Requires: Nothing (standalone)

## Performance Impact

- **Load:** ~15KB (obtp.js)
- **Timing:** Async, non-blocking
- **Recommendation:** Move to page

## Notes

- Pixel ID needs to be extracted from GTM template configuration
- Currently only tracks PageView, no conversion events
- Consider adding lead conversion tracking
- Outbrain is a native advertising platform (content recommendation)
- Similar to Taboola - if not running campaigns, consider removing
