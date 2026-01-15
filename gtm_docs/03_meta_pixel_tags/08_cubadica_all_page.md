# Cubadica - All Page Pixel

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Cubadica - All page Pixel |
| Tag ID | 63 |
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
| Pixel ID | 1936389293471725 |
| Event | PageView only |

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

fbq('init', '1936389293471725');
fbq('track', 'PageView');
</script>
```

---

## ⚠️ Questions

1. Is Cubadica still an active affiliate/partner?
2. Should they receive conversion events too?
3. Why only PageView (no Lead event)?

---

## What It Does

1. Initializes SECOND Meta pixel
2. Fires PageView on all pages
3. No conversion tracking

---

## Action

| Decision | Reason |
|----------|--------|
| ❓ REVIEW | Is Cubadica still active? |

---

## If Active

Add their pixel to CAPI calls.

## If Not Active

Delete this tag.
