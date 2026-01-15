# Meta Pixel - Base (Bright Horizon Media)

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Bright Horizon Media Meta all page Pixel |
| Tag Type | Custom HTML |
| Fires On | All Pages (DOM Ready) |
| Firing Option | Once Per Event |

## What It Does

1. Loads Meta Pixel (fbevents.js) library
2. Initializes pixel with ID
3. Fires PageView event on every page
4. Required for all other Meta events

## Current GTM Code

```html
<!-- Meta Base Pixel Code with Debug Logging -->
<script>
!function(f,b,e,v,n,t,s){
  if(f.fbq)return;n=f.fbq=function(){
    n.callMethod ? n.callMethod.apply(n,arguments) : n.queue.push(arguments)
  };
  if(!f._fbq)f._fbq=n;
  n.push=n;
  n.loaded=!0;
  n.version='2.0';
  n.queue=[];
  t=b.createElement(e);t.async=!0;
  t.src=v;
  s=b.getElementsByTagName(e)[0];
  s.parentNode.insertBefore(t,s)
}(window, document,'script','https://connect.facebook.net/en_US/fbevents.js');

fbq('init', 'PIXEL_ID_HERE');
fbq('track', 'PageView');
</script>
<noscript>
  <img height="1" width="1" style="display:none"
       src="https://www.facebook.com/tr?id=PIXEL_ID_HERE&ev=PageView&noscript=1"/>
</noscript>
```

## Can Move to Page? ✅ YES (RECOMMENDED)

**Recommendation:** MOVE TO PAGE

**Reason:**
- Base pixel should load as early as possible
- Simple, standalone script
- No GTM dependencies
- Improves PageView tracking accuracy

**Implementation:**
1. Add to `<head>` section
2. Place high in head for early loading
3. Remove from GTM

## Page Placement

```html
<head>
  <!-- Meta Pixel Code - Place early in head -->
  <script>
  !function(f,b,e,v,n,t,s){
    if(f.fbq)return;n=f.fbq=function(){
      n.callMethod ? n.callMethod.apply(n,arguments) : n.queue.push(arguments)
    };
    if(!f._fbq)f._fbq=n;
    n.push=n;
    n.loaded=!0;
    n.version='2.0';
    n.queue=[];
    t=b.createElement(e);t.async=!0;
    t.src=v;
    s=b.getElementsByTagName(e)[0];
    s.parentNode.insertBefore(t,s)
  }(window, document,'script','https://connect.facebook.net/en_US/fbevents.js');

  fbq('init', 'YOUR_PIXEL_ID');
  fbq('track', 'PageView');
  </script>
  <noscript>
    <img height="1" width="1" style="display:none"
         src="https://www.facebook.com/tr?id=YOUR_PIXEL_ID&ev=PageView&noscript=1"/>
  </noscript>
  <!-- End Meta Pixel Code -->
</head>
```

## Dependencies

- Required by: All other Meta Pixel event tags
- Requires: Nothing (base tag)

## Performance Impact

- **Load:** ~50KB (fbevents.js)
- **Timing:** Async, non-blocking
- **Network:** 1-2 requests per page
- **Recommendation:** Move to page for faster initialization

## Notes

- Pixel ID needs to be extracted from full GTM export
- Consider adding Advanced Matching during init for better attribution
- noscript fallback important for users with JS disabled

## Advanced Matching (Optional Enhancement)

If moving to page, consider adding Advanced Matching:

```javascript
fbq('init', 'YOUR_PIXEL_ID', {
  em: 'hashed_email',  // If available at page load
  ph: 'hashed_phone',
  fn: 'hashed_first_name',
  ln: 'hashed_last_name',
  ct: 'hashed_city',
  st: 'hashed_state',
  zp: 'hashed_zip',
  country: 'us'
});
```
