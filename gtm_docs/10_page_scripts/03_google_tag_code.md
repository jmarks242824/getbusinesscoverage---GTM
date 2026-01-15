# Google Tag Code

---

## Purpose

Loads Google Tag (gtag.js) for GA4 and Google Ads.

---

## Placement

Inside `<head>`, after Click ID Capture

---

## IDs Used

| ID | Purpose |
|----|---------|
| GT-5RMBWCN6 | Google Tag |
| G-1Z5EC7EX37 | GA4 Measurement |

---

## Code

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=GT-5RMBWCN6"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GT-5RMBWCN6');
  gtag('config', 'G-1Z5EC7EX37');
</script>
```

---

## What It Does

1. Loads gtag.js asynchronously
2. Initializes Google Tag
3. Initializes GA4 property
4. Fires page_view automatically

---

## Replaces GTM Tag

| Tag Name | Tag ID |
|----------|--------|
| Google Tag | 4 |
