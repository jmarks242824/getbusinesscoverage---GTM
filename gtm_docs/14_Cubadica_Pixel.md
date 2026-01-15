# Cubadica Meta Pixel (Partner)

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Cubadica - All page Pixel |
| Tag Type | Custom HTML |
| Pixel ID | 1936389293471725 |
| Fires On | All Pages (DOM Ready) |

## What It Does

- Loads secondary Meta Pixel for partner (Cubadica)
- Fires PageView on all pages
- Separate from primary Bright Horizon Media pixel

## Current GTM Code

```html
<!-- Meta Pixel Code -->
<script>
!function(f,b,e,v,n,t,s)
{if(f.fbq)return;n=f.fbq=function(){n.callMethod?
 n.callMethod.apply(n,arguments):n.queue.push(arguments)};
 if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
 n.queue=[];t=b.createElement(e);t.async=!0;
 t.src=v;s=b.getElementsByTagName(e)[0];
 s.parentNode.insertBefore(t,s)}(window, document,'script',
 'https://connect.facebook.net/en_US/fbevents.js');
 
fbq('init', '1936389293471725');
fbq('track', 'PageView');
</script>
<noscript>
  <img height="1" width="1" style="display:none"
       src="https://www.facebook.com/tr?id=1936389293471725&ev=PageView&noscript=1"/>
</noscript>
<!-- End Meta Pixel Code -->
```

## Can Move to Page? ✅ YES

**Recommendation:** MOVE TO PAGE (if keeping)

**Consideration:** Is this pixel still needed? If Cubadica is no longer a partner, remove entirely.

**Implementation:**

```html
<head>
  <!-- Cubadica Meta Pixel -->
  <script>
  !function(f,b,e,v,n,t,s)
  {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
   n.callMethod.apply(n,arguments):n.queue.push(arguments)};
   if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
   n.queue=[];t=b.createElement(e);t.async=!0;
   t.src=v;s=b.getElementsByTagName(e)[0];
   s.parentNode.insertBefore(t,s)}(window, document,'script',
   'https://connect.facebook.net/en_US/fbevents.js');
   
  fbq('init', '1936389293471725');
  fbq('track', 'PageView');
  </script>
  <!-- End Cubadica Meta Pixel -->
</head>
```

## Multiple Pixel Handling

Since there are TWO Meta pixels (Bright Horizon Media + Cubadica), use `fbq.push` or `fbq('trackSingle', ...)` to target specific pixels:

```javascript
// Fire event to specific pixel only
fbq('trackSingle', '1936389293471725', 'Lead', {
  value: 150.00,
  currency: 'USD'
});

// Or fire to all pixels
fbq('track', 'Lead', {
  value: 150.00,
  currency: 'USD'
});
```

## Dependencies

- Required by: Nothing
- Requires: Nothing (standalone)

## Performance Impact

- **Load:** ~50KB (fbevents.js) - BUT already loaded by primary pixel
- **Timing:** Async, non-blocking
- **Issue:** Loading fbevents.js twice is wasteful

## Optimized Multi-Pixel Implementation

If keeping both pixels, use single fbevents.js load:

```html
<script>
!function(f,b,e,v,n,t,s)
{if(f.fbq)return;n=f.fbq=function(){n.callMethod?
 n.callMethod.apply(n,arguments):n.queue.push(arguments)};
 if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
 n.queue=[];t=b.createElement(e);t.async=!0;
 t.src=v;s=b.getElementsByTagName(e)[0];
 s.parentNode.insertBefore(t,s)}(window, document,'script',
 'https://connect.facebook.net/en_US/fbevents.js');

// Initialize BOTH pixels
fbq('init', 'PRIMARY_PIXEL_ID');      // Bright Horizon Media
fbq('init', '1936389293471725');       // Cubadica

// Fire PageView to both
fbq('track', 'PageView');
</script>
```

## Notes

- Pixel ID: 1936389293471725
- Only fires PageView - no conversion tracking
- Consider if this pixel is still needed
- If partner relationship ended, remove to reduce page weight
- Current implementation loads fbevents.js twice (inefficient)
