# Microsoft Clarity Code

---

## Purpose

Loads Microsoft Clarity for session recording and heatmaps.

---

## Placement

Inside `<head>`, after Google Tag

---

## Project IDs

| Site | Project ID |
|------|------------|
| fastbusinessquote.com | rrejdlholl |
| getbusinesscoverage.com | uuqr6vibpo |

---

## Code (FBQ)

```html
<script>
(function(c,l,a,r,i,t,y){
  c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
  t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
  y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
})(window, document, "clarity", "script", "rrejdlholl");
</script>
```

---

## Code (GBC)

```html
<script>
(function(c,l,a,r,i,t,y){
  c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
  t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
  y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
})(window, document, "clarity", "script", "uuqr6vibpo");
</script>
```

---

## ⚠️ Important

Use correct Project ID for each site!

---

## Replaces GTM Tag

| Tag Name | Tag ID |
|----------|--------|
| Microsoft Clarity - Official | 7 |
