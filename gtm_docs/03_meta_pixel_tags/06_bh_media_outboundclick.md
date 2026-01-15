# BH Media - OutboundClick

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Bright Horizon Media Meta Pixel - OutboundClick |
| Tag ID | 61 |
| Type | Custom HTML |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Success Pages |
| Trigger Type | Page View |
| Trigger ID | 60 |

---

## Code

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

---

## What It Does

1. Only runs on success/thank you pages
2. Finds all external links
3. Fires custom OutboundClick event when clicked

---

## Dependencies

Requires "BH Media - All Page Pixel" to fire first.

---

## Action

| Decision | Reason |
|----------|--------|
| ❓ REVIEW | Only fires on success pages |
