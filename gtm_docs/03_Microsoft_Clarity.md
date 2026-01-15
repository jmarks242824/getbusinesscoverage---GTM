# Microsoft Clarity

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Microsoft Clarity - Official |
| Tag Type | cvt_MQDKZ (Community Template) |
| Project ID | rrejdlholl |
| Fires On | All Pages + Clarity Custom Event (.* wildcard) |
| Firing Option | Once Per Event |

## What It Does

- Session recording (video replay of user sessions)
- Heatmaps (click, scroll, movement)
- Rage click detection
- Dead click detection
- Scroll depth tracking
- Form analytics

## Current GTM Code (Equivalent)

```html
<script type="text/javascript">
    (function(c,l,a,r,i,t,y){
        c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
        t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
        y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
    })(window, document, "clarity", "script", "rrejdlholl");
</script>
```

## Can Move to Page? ✅ YES

**Recommendation:** MOVE TO PAGE

**Reason:**
- Simple standalone script
- No dependencies on other GTM tags
- No dataLayer requirements
- Faster initialization when on page directly
- One less GTM tag to process

**Implementation:**
1. Add script to `<head>` section
2. Place after any critical resources
3. Remove tag from GTM

## Page Placement

```html
<head>
  <!-- Other critical resources -->
  
  <!-- Microsoft Clarity -->
  <script type="text/javascript">
    (function(c,l,a,r,i,t,y){
        c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
        t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/rrejdlholl";
        y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
    })(window, document, "clarity", "script", "rrejdlholl");
  </script>
</head>
```

## Dependencies

- Required by: Nothing
- Requires: Nothing (standalone)

## Performance Impact

- **Load:** ~20KB
- **Timing:** Async, non-blocking
- **Recording:** Minimal CPU impact
- **Recommendation:** Move to page for faster initialization

## Custom Events (Optional)

If you want to track custom events in Clarity:

```javascript
// Track custom event
clarity("set", "custom_tag", "value");

// Track conversion
clarity("set", "conversion", "lead_submitted");
```

## Notes

- Clarity is free (no usage limits)
- Data retained for 30 days
- Integrates with Microsoft Advertising for audience building
- Consider adding custom tags for key user actions
