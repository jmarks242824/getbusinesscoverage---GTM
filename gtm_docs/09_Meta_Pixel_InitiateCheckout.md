# Meta Pixel - InitiateCheckout

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Bright Horizon Media Meta Pixel - InitiateCheckout |
| Tag Type | Custom HTML |
| Fires On | Page - Questions |
| Trigger Conditions | Page Path = /Questions |

## What It Does

- Fires InitiateCheckout event when user starts the quote form
- Signals user intent to complete quote process
- Used for funnel analysis and mid-funnel retargeting

## Current GTM Code

```html
<script>
  fbq('track', 'InitiateCheckout');
</script>
```

## Can Move to Page? ✅ YES (RECOMMENDED)

**Recommendation:** MOVE TO PAGE

**Reason:**
- Simple single-line script
- Page-specific trigger
- No dataLayer dependencies

**Implementation:**

### Option A: Conditional Script

```html
<script>
  if (window.location.pathname === '/Questions') {
    fbq('track', 'InitiateCheckout');
  }
</script>
```

### Option B: On Questions Page Only

```html
<!-- Only on /Questions page -->
<script>fbq('track', 'InitiateCheckout');</script>
```

### Option C: Next.js/React Component

```javascript
// In Questions page component
useEffect(() => {
  if (typeof fbq !== 'undefined') {
    fbq('track', 'InitiateCheckout');
  }
}, []);
```

## Page Placement

```html
<!-- On /Questions page, after Meta base pixel -->
<script>
  fbq('track', 'InitiateCheckout');
</script>
```

## Dependencies

- Required by: Nothing
- Requires: Meta Base Pixel

## Performance Impact

- **Load:** Negligible
- **Timing:** Fires on page load
- **Recommendation:** Move to page

## Trigger Details

| Setting | Value |
|---------|-------|
| Trigger Name | Page - Questions |
| Trigger Type | Page View |
| Conditions | Page Path equals /Questions |

## Notes

- InitiateCheckout is a standard Meta event
- Signals high-intent user (started form)
- Great for retargeting users who didn't complete
- Consider adding value parameter:

```javascript
fbq('track', 'InitiateCheckout', {
  value: 150.00,
  currency: 'USD'
});
```

## Funnel Position

```
PageView (all pages)
    ↓
ViewContent (/ or /Trucking)
    ↓
InitiateCheckout (/Questions) ← THIS EVENT
    ↓
Lead (form submission)
```
