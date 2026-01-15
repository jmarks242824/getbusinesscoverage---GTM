# Meta Pixel - Contact (Click-to-Call)

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Bright Horizon Media Meta Pixel - Contact (Click-to-Call) |
| Tag Type | Custom HTML |
| Fires On | All Pages (DOM Ready) |

## What It Does

- Listens for clicks on telephone links (`tel:`)
- Fires Contact event when user clicks to call
- Tracks phone call intent

## Current GTM Code

```html
<script>
document.addEventListener('DOMContentLoaded', function() {
  var callLinks = document.querySelectorAll('a[href^="tel:"]');
  callLinks.forEach(function(link) {
    link.addEventListener('click', function() {
      fbq('track', 'Contact');
    });
  });
});
</script>
```

## Can Move to Page? ✅ YES (RECOMMENDED)

**Recommendation:** MOVE TO PAGE

**Reason:**
- Self-contained event listener
- No GTM dependencies
- Will fire faster when on page directly

**Implementation:**

```html
<script>
document.addEventListener('DOMContentLoaded', function() {
  var callLinks = document.querySelectorAll('a[href^="tel:"]');
  callLinks.forEach(function(link) {
    link.addEventListener('click', function() {
      if (typeof fbq !== 'undefined') {
        fbq('track', 'Contact');
      }
    });
  });
});
</script>
```

## Page Placement

```html
<!-- After Meta base pixel, before closing </body> -->
<script>
document.addEventListener('DOMContentLoaded', function() {
  var callLinks = document.querySelectorAll('a[href^="tel:"]');
  callLinks.forEach(function(link) {
    link.addEventListener('click', function() {
      if (typeof fbq !== 'undefined') {
        fbq('track', 'Contact');
      }
    });
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
- **Recommendation:** Move to page

## Enhanced Version

Consider adding more data to the Contact event:

```javascript
document.addEventListener('DOMContentLoaded', function() {
  var callLinks = document.querySelectorAll('a[href^="tel:"]');
  callLinks.forEach(function(link) {
    link.addEventListener('click', function() {
      var phoneNumber = this.href.replace('tel:', '');
      if (typeof fbq !== 'undefined') {
        fbq('track', 'Contact', {
          content_name: 'Phone Call',
          content_category: 'Click-to-Call',
          value: 50.00,  // Estimated call value
          currency: 'USD'
        });
      }
      
      // Also push to dataLayer for other platforms
      window.dataLayer = window.dataLayer || [];
      dataLayer.push({
        'event': 'click_to_call',
        'phone_number': phoneNumber
      });
    });
  });
});
```

## Notes

- Contact is a standard Meta event
- Tracks phone call intent (not actual calls)
- Useful for mobile-heavy traffic
- Consider also tracking with Google Ads click-to-call conversion
- Current implementation doesn't capture which phone number was clicked
