# BH Media - Contact (Click-to-Call)

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Bright Horizon Media Meta Pixel - Contact (Click-to-Call) |
| Tag ID | 59 |
| Type | Custom HTML |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | All Pages |
| Trigger Type | DOM Ready |
| Trigger ID | 2147479553 |

---

## Code

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

---

## What It Does

1. Finds all tel: links on page
2. Adds click listener to each
3. Fires Contact event on click

---

## Dependencies

Requires "BH Media - All Page Pixel" to fire first.

---

## Action

| Decision | Reason |
|----------|--------|
| ❓ REVIEW | Depends on keeping base pixel |
