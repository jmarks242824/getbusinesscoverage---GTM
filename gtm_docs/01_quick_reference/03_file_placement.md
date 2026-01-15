# File Placement Guide

---

## All Pages

| File | Location | Order |
|------|----------|-------|
| 01_head_scripts.html | Inside `<head>` | 1st |
| 02_body_scripts.html | Before `</body>` | 2nd |
| 03_everflow_conversion.html | Before `</body>` | 3rd |

---

## Thank You Page Only

| File | Location | Order |
|------|----------|-------|
| 04_thankyou_page_script.html | Before `</body>` | After others |

---

## Visual

```
<html>
<head>
  <!-- 01_head_scripts.html HERE -->
</head>
<body>
  
  <!-- Your page content -->
  
  <!-- 02_body_scripts.html HERE -->
  <!-- 03_everflow_conversion.html HERE -->
  <!-- 04_thankyou_page_script.html HERE (thank you page only) -->
</body>
</html>
```
