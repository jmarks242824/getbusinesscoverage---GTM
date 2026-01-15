# Microsoft Advertising UET (Bing Ads)

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Microsoft Advertising Universal Event Tracking |
| Tag Type | baut (Bing Ads UET) |
| UET Tag ID | 97200753 |
| Fires On | Lead Submit (Click ID = submit_phone) |

## What It Does

- Fires Bing Ads conversion on lead submission
- Enables Microsoft Advertising conversion tracking
- Supports remarketing audiences in Bing

## Can Move to Page? ⚠️ PARTIAL

**Recommendation:** KEEP IN GTM (or use UET tag helper)

**Reason:**
- UET requires specific initialization
- GTM handles UET complexities automatically
- Consent mode integration

**If you must move to page:**

### Base UET Tag (All Pages)

```html
<script>
(function(w,d,t,r,u){
  var f,n,i;
  w[u]=w[u]||[],f=function(){
    var o={ti:"97200753"};
    o.q=w[u],w[u]=new UET(o),w[u].push("pageLoad")
  },
  n=d.createElement(t),n.src=r,n.async=1,n.onload=n.onreadystatechange=function(){
    var s=this.readyState;
    s&&s!=="loaded"&&s!=="complete"||(f(),n.onload=n.onreadystatechange=null)
  },
  i=d.getElementsByTagName(t)[0],i.parentNode.insertBefore(n,i)
})(window,document,"script","//bat.bing.com/bat.js","uetq");
</script>
```

### Conversion Event (On Lead Submit)

```html
<script>
window.uetq = window.uetq || [];
window.uetq.push('event', 'submit_lead_form', {
  'event_category': 'Lead',
  'event_label': 'Insurance Quote',
  'event_value': 150
});
</script>
```

## Page Placement

### Base Tag (in head)

```html
<head>
  <!-- Microsoft UET Tag -->
  <script>
  (function(w,d,t,r,u){
    var f,n,i;
    w[u]=w[u]||[],f=function(){
      var o={ti:"97200753"};
      o.q=w[u],w[u]=new UET(o),w[u].push("pageLoad")
    },
    n=d.createElement(t),n.src=r,n.async=1,n.onload=n.onreadystatechange=function(){
      var s=this.readyState;
      s&&s!=="loaded"&&s!=="complete"||(f(),n.onload=n.onreadystatechange=null)
    },
    i=d.getElementsByTagName(t)[0],i.parentNode.insertBefore(n,i)
  })(window,document,"script","//bat.bing.com/bat.js","uetq");
  </script>
</head>
```

### Conversion (tied to lead_submission event)

```javascript
// Listen for lead_submission and fire UET conversion
(function() {
  window.dataLayer = window.dataLayer || [];
  
  var originalPush = dataLayer.push;
  dataLayer.push = function() {
    var result = originalPush.apply(this, arguments);
    
    for (var i = 0; i < arguments.length; i++) {
      var event = arguments[i];
      if (event.event === 'lead_submission') {
        // Fire UET conversion
        window.uetq = window.uetq || [];
        window.uetq.push('event', 'submit_lead_form', {
          'event_category': 'Lead',
          'event_label': 'Insurance Quote',
          'event_value': event.value || 150
        });
      }
    }
    
    return result;
  };
})();
```

## Dependencies

- Required by: Nothing
- Requires: UET base tag

## Performance Impact

- **Load:** ~15KB (bat.js)
- **Timing:** Async, non-blocking
- **Recommendation:** Keep in GTM unless consolidating everything

## Trigger Details

| Setting | Value |
|---------|-------|
| Trigger Name | Lead Submit |
| Trigger Type | Click |
| Condition | Click ID = submit_phone |

## Notes

- UET Tag ID: 97200753
- Currently fires on click trigger, not dataLayer event
- Consider changing to fire on lead_submission event for consistency
- UET supports enhanced conversions (similar to Google)
- Microsoft Advertising requires UET for conversion tracking and remarketing
