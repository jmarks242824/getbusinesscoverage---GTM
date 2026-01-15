# Everflow Conversion Code

---

## File Name

`03_everflow_conversion.html`

---

## Purpose

Intercepts form submission to fire Everflow conversion and push lead_submission to dataLayer.

---

## Placement

Before `</body>`, after 02_body_scripts.html

---

## Pages

ALL PAGES

---

## Conversion Config

| Property | Value |
|----------|-------|
| aid | 373 |
| amount | 150.00 |

---

## Data Captured

| Everflow Field | Source |
|----------------|--------|
| adv1 | first_name |
| adv2 | last_name |
| adv3 | email |
| adv4 | phone |
| adv5 | zip |
| adv6 | tracking ID |

---

## dataLayer Event Pushed

```javascript
{
  'event': 'lead_submission',
  'event_id': 'conv_123456_abc',
  'event_time': 1234567890,
  'value': 150,
  'currency': 'USD',
  'user_data': {
    'email': 'user@example.com',
    'phone': '+11234567890',
    'first_name': 'john',
    'last_name': 'doe',
    'zip': '12345',
    'city': 'anytown',
    'state': 'ca',
    'country': 'US'
  }
}
```

---

## Code

```html
<script>
(function() {
  var originalFetch = window.fetch;
  
  window.fetch = function() {
    var args = arguments;
    return originalFetch.apply(this, args).then(function(response) {
      var url = args[0];
      var options = args[1];
      
      if (url && typeof url === 'string' && url.indexOf('nextinsure.com/listingdisplay/listings') > -1) {
        
        if (options && options.body) {
          try {
            var requestData = JSON.parse(options.body);
            var businessInfo = requestData.business_info || {};
            var tracking = requestData.tracking || {};
            
            var firstname = businessInfo.first_name || '';
            var lastname = businessInfo.last_name || '';
            var email = businessInfo.email || '';
            var phone = businessInfo.home_phone || '';
            var zipcode = businessInfo.zip || '';
            var city = businessInfo.city || '';
            var state = businessInfo.state || '';
            var amount = 150.00;
            
            var formattedPhone = phone;
            if (phone && phone.charAt(0) !== '+') {
              formattedPhone = phone.replace(/\D/g, '');
              if (formattedPhone.length === 10) {
                formattedPhone = '+1' + formattedPhone;
              }
            }
            
            if (email && phone) {
              window.dataLayer = window.dataLayer || [];
              
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
                
                var eventId = (conversionData && conversionData.transaction_id) 
                  ? conversionData.transaction_id 
                  : 'conv_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9);
                
                window.dataLayer.push({
                  'event': 'lead_submission',
                  'event_id': eventId,
                  'event_time': Math.floor(Date.now() / 1000),
                  'value': amount,
                  'currency': 'USD',
                  'user_data': {
                    'email': email.toLowerCase().trim(),
                    'phone': formattedPhone,
                    'first_name': firstname.toLowerCase().trim(),
                    'last_name': lastname.toLowerCase().trim(),
                    'zip': zipcode.trim(),
                    'city': city.toLowerCase().trim(),
                    'state': state.toLowerCase().trim(),
                    'country': 'US'
                  }
                });
                
                if (typeof gtag !== 'undefined') {
                  gtag('event', 'generate_lead', {
                    'value': amount,
                    'currency': 'USD',
                    'transaction_id': eventId
                  });
                }
                
                console.log('✅ Lead tracked with event_id:', eventId);
                
              }).catch(function(error) {
                console.error('Everflow error:', error);
                
                var fallbackEventId = 'conv_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9);
                
                window.dataLayer.push({
                  'event': 'lead_submission',
                  'event_id': fallbackEventId,
                  'value': amount,
                  'currency': 'USD',
                  'user_data': {
                    'email': email.toLowerCase().trim(),
                    'phone': formattedPhone,
                    'first_name': firstname.toLowerCase().trim(),
                    'last_name': lastname.toLowerCase().trim(),
                    'zip': zipcode.trim(),
                    'country': 'US'
                  }
                });
              });
            }
          } catch (e) {
            console.error('Error processing lead data:', e);
          }
        }
      }
      return response;
    });
  };
})();
</script>
```

---

## Testing

1. Submit test lead
2. Check Network tab for Everflow conversion
3. Check Console for "✅ Lead tracked" message
4. Check dataLayer for lead_submission event

---

## Replaces GTM Tag

| Tag Name | Tag ID |
|----------|--------|
| Everflow Conversion Script | 10 |
