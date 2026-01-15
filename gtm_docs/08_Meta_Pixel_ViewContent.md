# Meta Pixel - ViewContent

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Bright Horizon Media- Meta Pixel - ViewContent |
| Tag Type | Custom HTML |
| Fires On | Page - Trucking or Home |
| Trigger Conditions | Page Path = /Trucking OR / |

## What It Does

- Fires ViewContent event on landing pages
- Signals user is viewing key content
- Used for funnel analysis and audience building

## Current GTM Code

```html
<script>
  fbq('track', 'ViewContent');
</script>
```

## Can Move to Page? ✅ YES (RECOMMENDED)

**Recommendation:** MOVE TO PAGE (Conditional)

**Reason:**
- Simple single-line script
- Can be conditionally loaded based on page path
- No dataLayer dependencies

**Implementation Options:**

### Option A: Inline Conditional (Simple)

```html
<script>
  // Fire ViewContent on landing pages
  if (window.location.pathname === '/' || window.location.pathname === '/Trucking') {
    fbq('track', 'ViewContent');
  }
</script>
```

### Option B: Page-Specific Script

On `/` (homepage):
```html
<script>fbq('track', 'ViewContent');</script>
```

On `/Trucking`:
```html
<script>fbq('track', 'ViewContent');</script>
```

### Option C: Next.js/React Component

```javascript
// In your page component
useEffect(() => {
  if (typeof fbq !== 'undefined') {
    fbq('track', 'ViewContent');
  }
}, []);
```

## Page Placement

```html
<!-- After Meta base pixel, in page body or component -->
<script>
  if (window.location.pathname === '/' || window.location.pathname === '/Trucking') {
    fbq('track', 'ViewContent');
  }
</script>
```

## Dependencies

- Required by: Nothing
- Requires: Meta Base Pixel

## Performance Impact

- **Load:** Negligible (single function call)
- **Timing:** Fires on page load
- **Recommendation:** Move to page

## Trigger Details

| Setting | Value |
|---------|-------|
| Trigger Name | Page - Trucking or Home |
| Trigger Type | Page View |
| Conditions | Page Path equals /Trucking OR Page Path equals / |

## Notes

- ViewContent is a standard Meta event
- Used to build audiences of users who viewed landing pages
- Can be used for retargeting campaigns
- Consider adding content_name parameter for more detail:

```javascript
fbq('track', 'ViewContent', {
  content_name: window.location.pathname === '/' ? 'Homepage' : 'Trucking Landing'
});
```
