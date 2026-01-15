# Google Ads Conversion Lead Submit

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Google Ads Conversion Lead Submit |
| Tag Type | awud (Google Ads User Data) |
| Conversion ID | 17097160893 |
| Fires On | lead_submission (Custom Event) |
| Enhanced Conversions | Enabled |

## What It Does

1. Fires Google Ads conversion on lead submission
2. Sends enhanced conversion data (hashed user info)
3. Passes conversion value and currency
4. Uses event_id for deduplication

## GTM Variables Used

| Variable | DataLayer Path | Purpose |
|----------|----------------|---------|
| DL - Event ID | event_id | Transaction ID |
| DL - Value | value | Conversion value |
| DL - Currency | currency | Currency (USD) |
| Google User provided Data | (awec object) | Enhanced conversions |

## Enhanced Conversion Data Structure

The `Google User provided Data` variable maps to:

```javascript
{
  email: {{DL - Email}},
  phone_number: {{DL - Phone}},
  first_name: {{DL - First Name}},
  last_name: {{DL - Last Name}},
  home_address: {
    postal_code: {{DL - Zip}},
    city: {{DL - City}},
    region: {{DL - State}},
    country: {{DL - Country}}
  }
}
```

## Can Move to Page? ⚠️ COMPLEX

**Recommendation:** KEEP IN GTM

**Reason:**
- Enhanced conversions require specific gtag format
- GTM automatically hashes user data
- Easier to manage conversion settings in GTM
- Consent mode integration

**If you must move to page:**

```html
<script>
// Only fire when lead_submission event occurs
window.dataLayer = window.dataLayer || [];

// Listen for lead_submission event
function handleLeadSubmission(event) {
  if (event.event === 'lead_submission') {
    gtag('event', 'conversion', {
      'send_to': 'AW-17097160893/XXXXX',  // Need conversion label
      'value': event.value,
      'currency': event.currency,
      'transaction_id': event.event_id,
      'user_data': {
        'email': event.user_data.email,
        'phone_number': event.user_data.phone,
        'address': {
          'first_name': event.user_data.first_name,
          'last_name': event.user_data.last_name,
          'postal_code': event.user_data.zip,
          'city': event.user_data.city,
          'region': event.user_data.state,
          'country': event.user_data.country
        }
      }
    });
  }
}

// Hook into dataLayer
(function() {
  var originalPush = dataLayer.push;
  dataLayer.push = function() {
    var result = originalPush.apply(this, arguments);
    for (var i = 0; i < arguments.length; i++) {
      if (arguments[i].event === 'lead_submission') {
        handleLeadSubmission(arguments[i]);
      }
    }
    return result;
  };
})();
</script>
```

## Dependencies

- Required by: Nothing
- Requires: Google Tag, Conversion Linker, lead_submission dataLayer event

## Performance Impact

- **Load:** Uses already-loaded gtag.js
- **Timing:** Fires on conversion event only
- **Network:** 1 API call to Google
- **Recommendation:** Keep in GTM for easier management

## Trigger Details

| Setting | Value |
|---------|-------|
| Trigger Type | Custom Event |
| Event Name | lead_submission |
| Trigger Name | Lead Submission Event |

## Notes

- Conversion ID 17097160893 - verify this matches Google Ads account
- Missing conversion label in GTM export - check GTM for actual label
- Enhanced conversions improve match rate significantly
- Transaction ID (event_id) prevents duplicate conversions
- User data is automatically hashed by GTM before sending
