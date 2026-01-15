# BH Media - All Page Pixel

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Bright Horizon Media Meta all page Pixel |
| Tag ID | 54 |
| Type | Custom HTML |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | All Pages |
| Trigger Type | DOM Ready |
| Trigger ID | 2147479553 |

---

## Configuration

| Property | Value |
|----------|-------|
| Pixel ID | 1044730294266508 |
| Event | PageView |
| Advanced Matching | Yes |

---

## Code

```html
<script>
!function(f,b,e,v,n,t,s){
  if(f.fbq)return;n=f.fbq=function(){
    n.callMethod?n.callMethod.apply(n,arguments):n.queue.push(arguments)
  };
  if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
  n.queue=[];t=b.createElement(e);t.async=!0;t.src=v;
  s=b.getElementsByTagName(e)[0];s.parentNode.insertBefore(t,s)
}(window, document, 'script', 'https://connect.facebook.net/en_US/fbevents.js');

fbq('init', '1044730294266508', {
  em: {{DL - Email}} || '',
  ph: {{DL - Phone}} || '',
  fn: {{DL - First Name}} || '',
  ln: {{DL - Last Name}} || '',
  zp: {{DL - Zip}} || '',
  ct: {{DL - City}} || '',
  st: {{DL - State}} || '',
  country: {{DL - Country}} || 'US'
});

fbq('track', 'PageView');
</script>
```

---

## What It Does

1. Loads fbevents.js (~50KB)
2. Initializes pixel with ID 1044730294266508
3. Sets up Advanced Matching from dataLayer
4. Fires PageView on every page

---

## Action

| Decision | Reason |
|----------|--------|
| ❓ REVIEW | Can replace with Meta CAPI |
