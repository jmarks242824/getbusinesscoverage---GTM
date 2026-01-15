# Meta Pixel - OutboundClick

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Bright Horizon Media Meta Pixel - OutboundClick |
| Tag Type | Custom HTML |
| Fires On | Success Pages (Page Path = /Success) |

## What It Does

- Listens for clicks on external links (not fastbusinessquote.com)
- Fires custom OutboundClick event
- Tracks when users click on carrier/partner links on success page

## Current GTM Code

```html
<script>
document.addEventListener('DOMContentLoaded', function() {
  var links = document.querySelectorAll('a[href^="http"]');
  links.forEach(function(link) {
    if (!link.href.includes('fastbusinessquote.com')) {
      link.addEventListener('click', function(e) {
        fbq('trackCustom', 'OutboundClick');
      });
    }
  });
});
</script>
```

## Can Move to Page? ✅ YES (RECOMMENDED)

**Recommendation:** MOVE TO PAGE (Success page only)

**Reason:**
- Self-contained event listener
- Only needed on success page
- No GTM dependencies

**Implementation:**

```html
<!-- Only on /Success page -->
<script>
document.addEventListener('DOMContentLoaded', function() {
  var links = document.querySelectorAll('a[href^="http"]');
  links.forEach(function(link) {
    if (!link.href.includes('fastbusinessquote.com')) {
      link.addEventListener('click', function(e) {
        if (typeof fbq !== 'undefined') {
          fbq('trackCustom', 'OutboundClick', {
            destination_url: this.href
          });
        }
      });
    }
  });
});
</script>
```

## Page Placement

```html
<!-- On /Success page, before closing </body> -->
<script>
document.addEventListener('DOMContentLoaded', function() {
  var links = document.querySelectorAll('a[href^="http"]');
  links.forEach(function(link) {
    if (!link.href.includes('fastbusinessquote.com')) {
      link.addEventListener('click', function(e) {
        if (typeof fbq !== 'undefined') {
          fbq('trackCustom', 'OutboundClick', {
            destination_url: this.href
          });
        }
      });
    }
  });
});
</script>
```

## Dependencies

- Required by: Nothing
- Requires: Meta Base Pixel

## Performance Impact

- **Load:** Negligible
- **Timing:** Event listener attached on DOM ready
- **Recommendation:** Move to Success page only

## Enhanced Version

Track more details about outbound clicks:

```javascript
document.addEventListener('DOMContentLoaded', function() {
  var links = document.querySelectorAll('a[href^="http"]');
  links.forEach(function(link) {
    if (!link.href.includes('fastbusinessquote.com')) {
      link.addEventListener('click', function(e) {
        var destination = this.href;
        var linkText = this.innerText || this.textContent || '';
        
        // Determine carrier from URL
        var carrier = 'unknown';
        if (destination.includes('progressive')) carrier = 'Progressive';
        else if (destination.includes('geico')) carrier = 'GEICO';
        else if (destination.includes('statefarm')) carrier = 'State Farm';
        // Add more carriers as needed
        
        if (typeof fbq !== 'undefined') {
          fbq('trackCustom', 'OutboundClick', {
            destination_url: destination,
            carrier_name: carrier,
            link_text: linkText.substring(0, 100)
          });
        }
        
        // Also push to dataLayer
        window.dataLayer = window.dataLayer || [];
        dataLayer.push({
          'event': 'outbound_click',
          'destination_url': destination,
          'carrier_name': carrier
        });
      });
    }
  });
});
```

## Notes

- OutboundClick is a CUSTOM event (not standard)
- Uses `trackCustom` instead of `track`
- Tracks clicks to insurance carriers/partners on success page
- Useful for understanding which carriers get the most engagement
- Consider adding carrier identification for better reporting
