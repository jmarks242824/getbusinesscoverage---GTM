# Everflow Conversion Script

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Everflow Conversion Script |
| Tag Type | Custom HTML |
| Fires On | Lead Submit (Click ID = submit_phone) |
| Firing Option | Once Per Event |

## What It Does

1. Intercepts fetch() API calls globally
2. Detects form submission to nextinsure.com/listingdisplay/listings
3. Extracts user data from request body
4. Fires EF.conversion() to Everflow
5. Pushes `lead_submission` event to dataLayer with full user data
6. Provides fallback if Everflow fails

## Current GTM Code

```html
<script type="text/javascript">
(function() {
  var originalFetch = window.fetch;
  
  window.fetch = function() {
    var args = arguments;
    return originalFetch.apply(this, args).then(function(response) {
      var url = args[0];
      var options = args[1];
      
      // Detect NextInsure API call (form submission)
      if (url && typeof url === 'string' && url.indexOf('nextinsure.com/listingdisplay/listings') > -1) {
        
        if (options && options.body) {
          try {
            var requestData = JSON.parse(options.body);
            var businessInfo = requestData.business_info || {};
            var tracking = requestData.tracking || {};
            var requestedCoverage = requestData.requested_coverage || {};
            
            // Extract user data
            var firstname = businessInfo.first_name || '';
            var lastname = businessInfo.last_name || '';
            var email = businessInfo.email || '';
            var phone = businessInfo.home_phone || '';
            var zipcode = businessInfo.zip || '';
            var city = businessInfo.city || '';
            var state = '';
            var address = businessInfo.address || '';
            var businessName = businessInfo.company_name || '';
            var amount = 150.00;
            
            // Format phone to E.164
            var formattedPhone = phone;
            if (phone && phone.charAt(0) !== '+') {
              formattedPhone = phone.replace(/\D/g, '');
              if (formattedPhone.length === 10) {
                formattedPhone = '+1' + formattedPhone;
              }
            }
            
            // Only track if we have email and phone
            if (email && phone) {
              window.dataLayer = window.dataLayer || [];
              
              var trackConversion = function() {
                window.EF.conversion({
                  aid: 373,
                  adv1: firstname,
                  adv2: lastname,
                  adv3: email,
                  adv4: phone,
                  adv5: zipcode,
                  amount: amount,
                  parameters: {
                    "adv6": tracking.tn_ws_client || 'lead_' + Date.now()
                  }
                }).then(function(conversionData) {
                  
                  // Get event_id from Everflow or generate fallback
                  var eventId = (conversionData && conversionData.transaction_id) 
                    ? conversionData.transaction_id 
                    : 'conv_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9);
                  
                  // Push to dataLayer for Meta/Google
                  window.dataLayer.push({
                    'event': 'lead_submission',
                    'event_id': eventId,
                    'event_time': Math.floor(Date.now() / 1000),
                    'value': amount,
                    'currency': 'USD',
                    'user_data': {
                      'email': email.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'phone': formattedPhone,
                      'first_name': firstname.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'last_name': lastname.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'zip': zipcode.replace(/^\s+|\s+$/g, ''),
                      'city': city.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'state': state.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'country': 'US'
                    },
                    'business_name': businessName,
                    'address': address,
                    'coverage_type': requestedCoverage.coverage_type || ''
                  });
                  
                  console.log('Lead tracked successfully with event_id:', eventId);
                  
                }).catch(function(error) {
                  console.error('Everflow error:', error);
                  
                  // Fallback - still push to dataLayer even if Everflow fails
                  var fallbackEventId = 'conv_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9);
                  
                  window.dataLayer.push({
                    'event': 'lead_submission',
                    'event_id': fallbackEventId,
                    'event_time': Math.floor(Date.now() / 1000),
                    'value': amount,
                    'currency': 'USD',
                    'user_data': {
                      'email': email.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'phone': formattedPhone,
                      'first_name': firstname.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'last_name': lastname.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'zip': zipcode.replace(/^\s+|\s+$/g, ''),
                      'city': city.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'state': state.toLowerCase().replace(/^\s+|\s+$/g, ''),
                      'country': 'US'
                    },
                    'business_name': businessName
                  });
                  
                  console.log('Lead tracked with fallback event_id:', fallbackEventId);
                });
              };
              
              // Load Everflow script if not already loaded
              if (!window.EF) {
                var script = document.createElement('script');
                script.src = 'https://www.bhgtrack1.com/scripts/main.js';
                script.onload = function() {
                  trackConversion();
                };
                document.head.appendChild(script);
              } else {
                trackConversion();
              }
            }
          } catch (e) {
            console.error('Error parsing request data:', e);
          }
        }
      }
      
      return response;
    });
  };
})();
</script>
```

## Can Move to Page? ✅ YES (RECOMMENDED)

**Recommendation:** MOVE TO PAGE

**Reason:**
- Fetch interceptor should be in place before any API calls
- Eliminates GTM processing delay
- Critical for revenue tracking
- Self-contained with fallback handling

**Implementation:**
1. Add to page before form loads
2. Ensure dataLayer is initialized
3. Remove from GTM

## Page Placement

```html
<body>
  <!-- Early in body, before form -->
  <script>
    window.dataLayer = window.dataLayer || [];
  </script>
  <script type="text/javascript">
    // ... (full script from above)
  </script>
  
  <!-- Rest of page content -->
</body>
```

## Everflow Conversion Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| aid | 373 | Advertiser/Offer ID |
| adv1 | firstname | First name |
| adv2 | lastname | Last name |
| adv3 | email | Email address |
| adv4 | phone | Phone number |
| adv5 | zipcode | ZIP code |
| amount | 150.00 | Conversion value |
| adv6 | external_id | NextInsure client ID |

## DataLayer Output

```javascript
{
  'event': 'lead_submission',
  'event_id': 'ef_1234567890_abc123',
  'event_time': 1705276800,
  'value': 150.00,
  'currency': 'USD',
  'user_data': {
    'email': 'john@example.com',
    'phone': '+15551234567',
    'first_name': 'john',
    'last_name': 'smith',
    'zip': '60601',
    'city': 'chicago',
    'state': '',
    'country': 'US'
  },
  'business_name': 'Acme Corp',
  'address': '123 Main St',
  'coverage_type': 'general_liability'
}
```

## Dependencies

- Required by: All conversion tags (Google, Meta, Bing)
- Requires: Everflow main.js (loads dynamically if needed)

## Performance Impact

- **Load:** Minimal (fetch wrapper)
- **Timing:** Intercepts API calls in real-time
- **Network:** 1 API call to Everflow on conversion
- **Recommendation:** Move to page for immediate interception

## Data Formatting Applied

| Field | Formatting |
|-------|------------|
| email | Lowercase, trimmed |
| phone | E.164 format (+1XXXXXXXXXX) |
| first_name | Lowercase, trimmed |
| last_name | Lowercase, trimmed |
| zip | Trimmed |
| city | Lowercase, trimmed |
| state | Lowercase, trimmed |

## Notes

- Script intercepts ALL fetch() calls - monitors for NextInsure API
- Fallback event_id generated if Everflow fails
- bhgtrack1.com is the conversion tracking domain (different from click domain)
- Value is hardcoded at $150.00 - consider making dynamic
- State field is not being captured from form data (empty string)
