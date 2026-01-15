# Consolidated Page Implementation
## All Tracking Scripts in One Place

This document provides the complete, consolidated tracking code to add directly to your page, replacing multiple GTM tags.

---

## Page Structure Overview

```html
<head>
  <!-- 1. DataLayer Initialization -->
  <!-- 2. Google Tag (gtag.js) -->
  <!-- 3. Meta Pixel Base (consolidated) -->
  <!-- 4. Microsoft Clarity -->
  <!-- 5. Everflow Click Script -->
  <!-- 6. Taboola Pixel (if needed) -->
  <!-- 7. Outbrain Pixel (if needed) -->
</head>
<body>
  <!-- Page content -->
  
  <!-- 8. Everflow Conversion Script (before </body>) -->
  <!-- 9. Event Listeners (Meta events, GA4 form steps) -->
</body>
```

---

## Complete Implementation

### 1. DataLayer Initialization (FIRST - Required)

```html
<script>
  window.dataLayer = window.dataLayer || [];
</script>
```

### 2. Google Tag (Keep in GTM or add to page)

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GT-5RMBWCN6"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GT-5RMBWCN6');
</script>
```

### 3. Meta Pixel - Consolidated (Replaces 6 GTM Tags)

```html
<!-- Meta Pixel Code - Consolidated -->
<script>
!function(f,b,e,v,n,t,s){
  if(f.fbq)return;n=f.fbq=function(){
    n.callMethod ? n.callMethod.apply(n,arguments) : n.queue.push(arguments)
  };
  if(!f._fbq)f._fbq=n;
  n.push=n;
  n.loaded=!0;
  n.version='2.0';
  n.queue=[];
  t=b.createElement(e);t.async=!0;
  t.src=v;
  s=b.getElementsByTagName(e)[0];
  s.parentNode.insertBefore(t,s)
}(window, document, 'script', 'https://connect.facebook.net/en_US/fbevents.js');

// Initialize PRIMARY pixel (Bright Horizon Media)
fbq('init', '1044730294266508');

// Initialize SECONDARY pixel (Cubadica) - REMOVE if not needed
// fbq('init', '1936389293471725');

// Fire PageView
fbq('track', 'PageView');

// === Conditional Events Based on Page ===
(function() {
  var path = window.location.pathname;
  
  // ViewContent on landing pages
  if (path === '/' || path === '/Trucking') {
    fbq('track', 'ViewContent');
  }
  
  // InitiateCheckout on form page
  if (path === '/Questions') {
    fbq('track', 'InitiateCheckout');
  }
})();

// === Click-to-Call Tracking ===
document.addEventListener('DOMContentLoaded', function() {
  document.querySelectorAll('a[href^="tel:"]').forEach(function(link) {
    link.addEventListener('click', function() {
      fbq('track', 'Contact');
    });
  });
});

// === Outbound Click Tracking (Success page only) ===
document.addEventListener('DOMContentLoaded', function() {
  if (window.location.pathname === '/Success') {
    document.querySelectorAll('a[href^="http"]').forEach(function(link) {
      if (!link.href.includes('fastbusinessquote.com')) {
        link.addEventListener('click', function() {
          fbq('trackCustom', 'OutboundClick', {
            destination_url: this.href
          });
        });
      }
    });
  }
});
</script>
<noscript>
  <img height="1" width="1" style="display:none"
       src="https://www.facebook.com/tr?id=1044730294266508&ev=PageView&noscript=1"/>
</noscript>
<!-- End Meta Pixel Code -->
```

### 4. Microsoft Clarity

```html
<!-- Microsoft Clarity -->
<script type="text/javascript">
  (function(c,l,a,r,i,t,y){
    c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
    t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
    y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
  })(window, document, "clarity", "script", "rrejdlholl");
</script>
```

### 5. Everflow Click Script

```html
<!-- Everflow Click Attribution -->
<script type="text/javascript" src="https://www.pvm75hjsg.com/scripts/main.js"></script>
<script type="text/javascript">
(function() {
  window.dataLayer = window.dataLayer || [];
  
  if (EF.urlParameter("affid2")) {
    // Dual affiliate flow
    EF.click({
      tracking_domain: "https://www.bhmediatrack.com",
      offer_id: EF.urlParameter("oid2"),
      affiliate_id: EF.urlParameter("affid2"),
      sub1: EF.urlParameter("sub6")
    }).then(function(transaction_id) {
      dataLayer.push({
        'event': 'everflow_click',
        'event_id': transaction_id,
        'affiliate_id': EF.urlParameter("affid"),
        'offer_id': EF.urlParameter("oid")
      });
      
      EF.click({
        tracking_domain: "https://www.pvm75hjsg.com",
        offer_id: EF.urlParameter("oid"),
        affiliate_id: EF.urlParameter("affid"),
        sub1: EF.urlParameter("sub1"),
        sub2: EF.urlParameter("sub2"),
        sub3: EF.urlParameter("sub3"),
        sub4: EF.urlParameter("sub4"),
        sub5: transaction_id,
        uid: EF.urlParameter("uid"),
        source_id: EF.urlParameter("source_id")
      });
    });
  } else {
    // Standard flow
    var efTransactionId = EF.urlParameter("_ef_transaction_id") || '';
    
    EF.click({
      tracking_domain: "https://www.pvm75hjsg.com",
      offer_id: EF.urlParameter("oid"),
      affiliate_id: EF.urlParameter("affid"),
      sub1: EF.urlParameter("sub1"),
      sub2: EF.urlParameter("sub2"),
      sub3: EF.urlParameter("sub3"),
      sub4: EF.urlParameter("sub4"),
      sub5: EF.urlParameter("sub5"),
      uid: EF.urlParameter("uid"),
      source_id: EF.urlParameter("source_id"),
      transaction_id: efTransactionId
    });
    
    if (efTransactionId) {
      dataLayer.push({
        'event': 'everflow_click',
        'event_id': efTransactionId,
        'affiliate_id': EF.urlParameter("affid"),
        'offer_id': EF.urlParameter("oid")
      });
    }
  }
})();
</script>
```

### 6. Taboola Pixel (Optional - only if running campaigns)

```html
<!-- Taboola Pixel -->
<script type='text/javascript'>
  window._tfa = window._tfa || [];
  window._tfa.push({notify: 'event', name: 'page_view', id: 1901590});
  !function (t, f, a, x) {
    if (!document.getElementById(x)) {
      t.async = 1;t.src = a;t.id=x;f.parentNode.insertBefore(t, f);
    }
  }(document.createElement('script'),
    document.getElementsByTagName('script')[0],
    '//cdn.taboola.com/libtrc/unip/1901590/tfa.js',
    'tb_tfa_script');
</script>
```

### 7. Outbrain Pixel (Optional - only if running campaigns)

```html
<!-- Outbrain Pixel -->
<script type="text/javascript">
  !function(_window, _document) {
    var OB_ADV_ID = '0084a3fde60ab652bf9cd5cc0158baee25';
    if (_window.obApi) {
      var toArray = function(object) {
        return Object.prototype.toString.call(object) === '[object Array]' ? object : [object];
      };
      _window.obApi.marketerId = toArray(_window.obApi.marketerId).concat(toArray(OB_ADV_ID));
      return;
    }
    var api = _window.obApi = function() {
      api.dispatch ? api.dispatch.apply(api, arguments) : api.queue.push(arguments);
    };
    api.version = '1.1';
    api.loaded = true;
    api.marketerId = OB_ADV_ID;
    api.queue = [];
    var tag = _document.createElement('script');
    tag.async = true;
    tag.src = '//amplify.outbrain.com/cp/obtp.js';
    tag.type = 'text/javascript';
    var script = _document.getElementsByTagName('script')[0];
    script.parentNode.insertBefore(tag, script);
  }(window, document);
  obApi('track', 'PAGE_VIEW');
</script>
```

### 8. Everflow Conversion + Meta Lead (Consolidated)

Place this before `</body>`:

```html
<!-- Everflow Conversion + Meta Lead Tracking -->
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
            var state = businessInfo.state || '';
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
                  
                  var eventId = (conversionData && conversionData.transaction_id) 
                    ? conversionData.transaction_id 
                    : 'conv_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9);
                  
                  // Prepare user data (lowercase, trimmed)
                  var userData = {
                    'email': email.toLowerCase().trim(),
                    'phone': formattedPhone,
                    'first_name': firstname.toLowerCase().trim(),
                    'last_name': lastname.toLowerCase().trim(),
                    'zip': zipcode.trim(),
                    'city': city.toLowerCase().trim(),
                    'state': state.toLowerCase().trim(),
                    'country': 'US'
                  };
                  
                  // Push to dataLayer for Google
                  window.dataLayer = window.dataLayer || [];
                  window.dataLayer.push({
                    'event': 'lead_submission',
                    'event_id': eventId,
                    'event_time': Math.floor(Date.now() / 1000),
                    'value': amount,
                    'currency': 'USD',
                    'user_data': userData,
                    'business_name': businessName,
                    'address': address,
                    'coverage_type': requestedCoverage.coverage_type || ''
                  });
                  
                  // Fire Meta Lead event directly (no GTM needed)
                  if (typeof fbq !== 'undefined') {
                    fbq('track', 'Lead', {
                      value: amount,
                      currency: 'USD',
                      content_name: 'Insurance Quote'
                    }, {
                      eventID: eventId
                    });
                    console.log('✅ Meta Lead fired with event_id:', eventId);
                  }
                  
                  // Fire Taboola conversion (if pixel loaded)
                  if (typeof window._tfa !== 'undefined') {
                    window._tfa.push({
                      notify: 'event',
                      name: 'lead',
                      id: 1901590,
                      revenue: amount,
                      currency: 'USD'
                    });
                  }
                  
                  // Fire Outbrain conversion (if pixel loaded)
                  if (typeof obApi !== 'undefined') {
                    obApi('track', 'Lead', {
                      value: amount,
                      currency: 'USD'
                    });
                  }
                  
                  console.log('✅ Lead tracked with event_id:', eventId);
                  
                }).catch(function(error) {
                  console.error('Everflow error:', error);
                  
                  // Fallback - still fire conversions
                  var fallbackEventId = 'conv_' + Date.now() + '_' + Math.random().toString(36).substr(2, 9);
                  
                  window.dataLayer = window.dataLayer || [];
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
                      'city': city.toLowerCase().trim(),
                      'state': state.toLowerCase().trim(),
                      'country': 'US'
                    }
                  });
                  
                  if (typeof fbq !== 'undefined') {
                    fbq('track', 'Lead', {
                      value: amount,
                      currency: 'USD',
                      content_name: 'Insurance Quote'
                    }, {
                      eventID: fallbackEventId
                    });
                  }
                  
                  console.log('⚠️ Lead tracked with fallback event_id:', fallbackEventId);
                });
              };
              
              // Load Everflow script if not already loaded
              if (!window.EF) {
                var script = document.createElement('script');
                script.src = 'https://www.bhgtrack1.com/scripts/main.js';
                script.onload = trackConversion;
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

### 9. GA4 Form Step Events (Consolidated)

```html
<!-- GA4 Form Step Tracking - Consolidated -->
<script>
document.addEventListener('DOMContentLoaded', function() {
  
  // Form step event mapping
  var formStepEvents = {
    'next_businessName': 'name_of_your_business',
    'next_businessAddress': 'address',
    'next_businessDescription': 'business_description',
    'next_linesOfBusiness': 'type_of_coverage',
    'next_name': 'First_Last',
    'next_email': 'email',
    'submit_phone': 'phone'
  };
  
  // Prefix-based events
  var prefixEvents = {
    'legalEntity_': 'legal_entity',
    'yearsInBusiness_': 'years_in_business',
    'numberOfOwners_': 'partners_owners',
    'numberOfEmployees_': 'employees',
    'annualRevenue_': 'revenue',
    'annualPayroll_': 'payroll'
  };
  
  // Click handler
  document.addEventListener('click', function(e) {
    var target = e.target.closest('[id]');
    if (!target) return;
    
    var clickId = target.id;
    var eventName = null;
    
    // Check exact matches
    if (formStepEvents[clickId]) {
      eventName = formStepEvents[clickId];
    } else {
      // Check prefix matches
      for (var prefix in prefixEvents) {
        if (clickId.indexOf(prefix) === 0) {
          eventName = prefixEvents[prefix];
          break;
        }
      }
    }
    
    // Fire GA4 event
    if (eventName && typeof gtag !== 'undefined') {
      gtag('event', eventName, {
        'event_category': 'Form',
        'event_label': clickId
      });
    }
  });
  
  // Tel click tracking
  document.querySelectorAll('a[href^="tel:"]').forEach(function(link) {
    link.addEventListener('click', function() {
      if (typeof gtag !== 'undefined') {
        gtag('event', 'tel_click', {
          'event_category': 'Phone',
          'event_label': 'Click to Call'
        });
      }
    });
  });
});
</script>
```

---

## What Remains in GTM (Minimal)

After moving all above to page, keep only these in GTM:

1. **Conversion Linker** - Required for cross-domain
2. **Google Ads Conversion** - Enhanced conversions handling
3. **Microsoft UET Conversion** - If needed

That's it - 3 tags instead of 28!

---

## File Sizes Comparison

| Script | Size | Required? |
|--------|------|-----------|
| gtag.js | ~30KB | Yes (Google) |
| fbevents.js | ~50KB | Yes (Meta) |
| clarity.js | ~20KB | Optional |
| Everflow main.js | ~15KB | Yes (Attribution) |
| Taboola tfa.js | ~20KB | Only if running ads |
| Outbrain obtp.js | ~15KB | Only if running ads |

**Total (required):** ~115KB
**Total (with optional):** ~150KB
**GTM container:** Will be much smaller (~20KB vs ~150KB)
