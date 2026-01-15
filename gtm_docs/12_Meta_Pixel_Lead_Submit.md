# Meta Pixel - Lead Submit (Primary Conversion)

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Bright horizon Media - Lead Submit Meta Pixel |
| Tag Type | Custom HTML |
| Fires On | lead_submission (Custom Event) |

## What It Does

1. Fires Lead conversion event to Meta
2. Passes event_id for deduplication
3. Sends Advanced Matching user data
4. Includes conversion value

## Current GTM Code

```html
<script>
// Get event_id and user data from dataLayer
var fb_event_id = {{DL - Event ID}};
var userEmail = {{DL - Email}};
var userPhone = {{DL - Phone}};
var userFirstName = {{DL - First Name}};
var userLastName = {{DL - Last Name}};
var userZip = {{DL - Zip}};
var userCity = {{DL - City}};
var userState = {{DL - State}};
var userCountry = {{DL - Country}};

// Debug log to verify data
console.log('Meta Lead Event - Event ID:', fb_event_id);
console.log('Meta Lead Event - User Data:', {
  email: userEmail,
  phone: userPhone,
  firstName: userFirstName,
  lastName: userLastName,
  zip: userZip
});

// Fire Lead event with Advanced Matching
fbq('track', 'Lead', {
  value: {{DL - Value}},
  currency: {{DL - Currency}},
  content_name: 'Insurance Quote'
}, {
  eventID: fb_event_id
});

// Set user data for Advanced Matching
fbq('init', 'PIXEL_ID', {
  em: userEmail,
  ph: userPhone,
  fn: userFirstName,
  ln: userLastName,
  zp: userZip,
  ct: userCity,
  st: userState,
  country: userCountry
});
</script>
```

## Can Move to Page? ⚠️ COMPLEX

**Recommendation:** KEEP IN GTM (or integrate into Everflow Conversion Script)

**Reason:**
- Depends on dataLayer variables
- Must fire AFTER lead_submission event
- Timing is critical for accurate tracking

**Alternative: Integrate into Everflow Conversion Script**

Instead of separate tag, add Meta tracking directly in the conversion script:

```javascript
// Inside the Everflow Conversion Script, after dataLayer.push
window.dataLayer.push({
  'event': 'lead_submission',
  // ... existing data
});

// Fire Meta Lead event immediately
if (typeof fbq !== 'undefined') {
  fbq('track', 'Lead', {
    value: amount,
    currency: 'USD',
    content_name: 'Insurance Quote'
  }, {
    eventID: eventId
  });
}
```

## Standalone Page Version

If implementing separately on page:

```html
<script>
// Listen for lead_submission event in dataLayer
(function() {
  window.dataLayer = window.dataLayer || [];
  
  var originalPush = dataLayer.push;
  dataLayer.push = function() {
    var result = originalPush.apply(this, arguments);
    
    for (var i = 0; i < arguments.length; i++) {
      var event = arguments[i];
      if (event.event === 'lead_submission') {
        fireMetaLead(event);
      }
    }
    
    return result;
  };
  
  function fireMetaLead(data) {
    if (typeof fbq === 'undefined') return;
    
    var userData = data.user_data || {};
    
    // Fire Lead event with event_id for deduplication
    fbq('track', 'Lead', {
      value: data.value || 150.00,
      currency: data.currency || 'USD',
      content_name: 'Insurance Quote'
    }, {
      eventID: data.event_id
    });
    
    console.log('Meta Lead fired with event_id:', data.event_id);
  }
})();
</script>
```

## Dependencies

- Required by: Nothing
- Requires: Meta Base Pixel, lead_submission dataLayer event

## Performance Impact

- **Load:** Negligible
- **Timing:** Must fire after dataLayer event
- **Recommendation:** Integrate into conversion script

## DataLayer Variables Required

| Variable | DataLayer Path |
|----------|----------------|
| DL - Event ID | event_id |
| DL - Value | value |
| DL - Currency | currency |
| DL - Email | user_data.email |
| DL - Phone | user_data.phone |
| DL - First Name | user_data.first_name |
| DL - Last Name | user_data.last_name |
| DL - Zip | user_data.zip |
| DL - City | user_data.city |
| DL - State | user_data.state |
| DL - Country | user_data.country |

## Notes

- Event ID is CRITICAL for deduplication with CAPI
- Advanced Matching data should be lowercase and trimmed
- Phone should be E.164 format (+1XXXXXXXXXX)
- Meta auto-hashes Advanced Matching data
- Debug console.log statements should be removed in production
