# Direct Integrations to Replace Client-Side Scripts
## Eliminate JavaScript, Improve Speed

This document outlines server-side and API integrations that can **completely replace client-side tracking scripts**, dramatically improving page load speed.

---

## Executive Summary

| Platform | Current Method | Direct Integration | Scripts Eliminated | Speed Gain |
|----------|---------------|-------------------|-------------------|------------|
| **Meta** | fbevents.js (50KB) | Conversions API (CAPI) | ✅ Yes | ~100-200ms |
| **Google Ads** | gtag.js (30KB) | Enhanced Conversions API | ⚠️ Partial | ~50-100ms |
| **Everflow** | main.js (15KB) | S2S Postback | ✅ Yes | ~50-100ms |
| **Microsoft/Bing** | bat.js (15KB) | Offline Conversions API | ✅ Yes | ~50ms |
| **Taboola** | tfa.js (20KB) | S2S Conversion API | ✅ Yes | ~50ms |
| **Outbrain** | obtp.js (15KB) | S2S Conversion API | ✅ Yes | ~50ms |
| **Clarity** | clarity.js (20KB) | N/A (keep client-side) | ❌ No | N/A |

**Total Potential Savings:** ~165KB JavaScript, 400-600ms load time

---

## 1. Meta Conversions API (CAPI)

### What It Replaces
- fbevents.js (~50KB)
- All Meta Pixel event tags
- Advanced Matching client-side code

### How It Works
Instead of the browser sending events to Meta, your **server sends events directly to Meta's API**.

```
CURRENT (Client-Side):
User → Browser → fbevents.js → Meta Servers

NEW (Server-Side CAPI):
User → Your Server → Meta API → Meta Servers
```

### Implementation

#### Endpoint
```
POST https://graph.facebook.com/v18.0/{PIXEL_ID}/events
```

#### Server-Side Code (Node.js Example)
```javascript
const crypto = require('crypto');
const https = require('https');

async function sendMetaConversion(eventData, userData) {
  const pixelId = '1044730294266508';
  const accessToken = 'YOUR_ACCESS_TOKEN'; // Generate in Meta Events Manager
  
  // Hash user data (required by Meta)
  const hashData = (data) => {
    if (!data) return null;
    return crypto.createHash('sha256')
      .update(data.toLowerCase().trim())
      .digest('hex');
  };
  
  const payload = {
    data: [{
      event_name: 'Lead',
      event_time: Math.floor(Date.now() / 1000),
      event_id: eventData.event_id, // Same as client-side for deduplication
      event_source_url: eventData.page_url,
      action_source: 'website',
      user_data: {
        em: [hashData(userData.email)],
        ph: [hashData(userData.phone.replace(/\D/g, ''))],
        fn: [hashData(userData.first_name)],
        ln: [hashData(userData.last_name)],
        ct: [hashData(userData.city)],
        st: [hashData(userData.state)],
        zp: [hashData(userData.zip)],
        country: [hashData('us')],
        client_ip_address: userData.ip_address,
        client_user_agent: userData.user_agent,
        fbc: userData.fbc, // Facebook click ID cookie
        fbp: userData.fbp  // Facebook browser ID cookie
      },
      custom_data: {
        value: eventData.value,
        currency: 'USD',
        content_name: 'Insurance Quote'
      }
    }],
    access_token: accessToken
  };
  
  const response = await fetch(
    `https://graph.facebook.com/v18.0/${pixelId}/events`,
    {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    }
  );
  
  return response.json();
}
```

#### What You Still Need Client-Side
```javascript
// Minimal client code to capture fbc/fbp cookies
<script>
function getCookie(name) {
  const match = document.cookie.match(new RegExp('(^| )' + name + '=([^;]+)'));
  return match ? match[2] : '';
}
window._fbp = getCookie('_fbp');
window._fbc = getCookie('_fbc') || (function() {
  const fbclid = new URLSearchParams(window.location.search).get('fbclid');
  return fbclid ? 'fb.1.' + Date.now() + '.' + fbclid : '';
})();
</script>
```

### Setup Steps
1. Go to Meta Events Manager → Settings → Conversions API
2. Generate Access Token
3. Implement server-side code
4. Test with Test Events tool
5. Enable redundant tracking (Pixel + CAPI) initially
6. Once validated, remove client-side Pixel

### Benefits
- ✅ Bypasses ad blockers
- ✅ Bypasses iOS 14.5+ restrictions
- ✅ Better Event Match Quality (EMQ)
- ✅ No fbevents.js (~50KB saved)
- ✅ More reliable tracking

---

## 2. Google Ads Enhanced Conversions API

### What It Replaces
- Can supplement (not fully replace) gtag.js
- Replaces manual Enhanced Conversions in GTM

### How It Works
Send conversion data with hashed user info directly to Google Ads API.

### Implementation

#### Endpoint
```
POST https://googleads.googleapis.com/v15/customers/{customer_id}:uploadConversionAdjustments
```

#### Server-Side Code (Node.js Example)
```javascript
const crypto = require('crypto');
const { GoogleAdsApi } = require('google-ads-api');

async function sendGoogleConversion(conversionData, userData) {
  const client = new GoogleAdsApi({
    client_id: 'YOUR_CLIENT_ID',
    client_secret: 'YOUR_CLIENT_SECRET',
    developer_token: 'YOUR_DEVELOPER_TOKEN'
  });
  
  const customer = client.Customer({
    customer_id: '17097160893', // Your Google Ads Customer ID
    refresh_token: 'YOUR_REFRESH_TOKEN'
  });
  
  // Hash user data
  const hash = (data) => {
    if (!data) return null;
    return crypto.createHash('sha256')
      .update(data.toLowerCase().trim())
      .digest('hex');
  };
  
  const conversionAdjustment = {
    conversion_action: `customers/17097160893/conversionActions/YOUR_CONVERSION_ACTION_ID`,
    adjustment_type: 'ENHANCEMENT',
    order_id: conversionData.transaction_id,
    adjustment_date_time: new Date().toISOString(),
    user_identifiers: [
      { hashed_email: hash(userData.email) },
      { hashed_phone_number: hash('+1' + userData.phone.replace(/\D/g, '')) },
      { 
        address_info: {
          hashed_first_name: hash(userData.first_name),
          hashed_last_name: hash(userData.last_name),
          postal_code: userData.zip,
          country_code: 'US'
        }
      }
    ]
  };
  
  const response = await customer.conversionAdjustmentUploads.upload({
    customer_id: '17097160893',
    conversion_adjustments: [conversionAdjustment],
    partial_failure: true
  });
  
  return response;
}
```

### Important Notes
- ⚠️ **Cannot fully replace gtag.js** - still need client-side tag for click tracking
- ✅ Can replace Enhanced Conversions GTM tag
- Send within 24 hours of conversion
- Use same `order_id`/`transaction_id` as client-side

### Recommendation
**Keep minimal gtag.js on page, move Enhanced Conversions to server-side**

---

## 3. Everflow S2S Postback (Server-to-Server)

### What It Replaces
- Everflow main.js (~15KB)
- EF.conversion() client-side calls
- Fetch interceptor in GTM

### How It Works
Instead of client-side JavaScript firing conversions, your server makes HTTP request directly to Everflow.

```
CURRENT (Client-Side):
Browser → EF.conversion() → Everflow Servers

NEW (Server-Side S2S):
Your Server → HTTP GET/POST → Everflow Servers
```

### Implementation

#### Postback URL Format
```
https://www.pvm75hjsg.com/?nid=YOUR_NID&transaction_id={transaction_id}&amount={amount}&adv1={first_name}&adv2={last_name}&adv3={email}&adv4={phone}&adv5={zip}
```

#### Server-Side Code (Node.js Example)
```javascript
async function sendEverflowConversion(transactionId, leadData) {
  const params = new URLSearchParams({
    nid: 'YOUR_NETWORK_ID',  // Get from Everflow dashboard
    transaction_id: transactionId,
    amount: '150.00',
    adv1: leadData.first_name,
    adv2: leadData.last_name,
    adv3: leadData.email,
    adv4: leadData.phone,
    adv5: leadData.zip,
    adv6: leadData.external_id || ''
  });
  
  const response = await fetch(
    `https://www.pvm75hjsg.com/?${params.toString()}`,
    { method: 'GET' }
  );
  
  return response.ok;
}
```

#### For Click Tracking (Still Needed)
You still need to capture the transaction_id from the initial click. Options:

**Option A: Server-side redirect (best)**
```javascript
// On your server, when user lands with affiliate params
app.get('/landing', async (req, res) => {
  const { affid, oid, sub1, sub2, sub3, sub4, sub5, uid, source_id } = req.query;
  
  // Call Everflow Click API
  const clickResponse = await fetch(
    `https://www.pvm75hjsg.com/sdk/click?` + 
    `offer_id=${oid}&affiliate_id=${affid}&sub1=${sub1}&sub2=${sub2}&sub3=${sub3}&sub4=${sub4}&sub5=${sub5}`,
    { method: 'GET' }
  );
  
  const { transaction_id } = await clickResponse.json();
  
  // Store transaction_id in session/database
  req.session.ef_transaction_id = transaction_id;
  
  // Continue to page
  res.render('landing');
});
```

**Option B: Minimal client script (simpler)**
Keep a tiny script to capture transaction_id on click, store in cookie:
```javascript
<script>
(function() {
  const params = new URLSearchParams(window.location.search);
  const efTid = params.get('_ef_transaction_id');
  if (efTid) {
    document.cookie = 'ef_tid=' + efTid + '; path=/; max-age=86400';
  }
})();
</script>
```

### Benefits
- ✅ No main.js script needed (~15KB saved)
- ✅ More reliable than client-side
- ✅ Works with ad blockers
- ✅ Faster conversion tracking

---

## 4. Microsoft/Bing Offline Conversions API

### What It Replaces
- bat.js UET tag (~15KB)
- Microsoft UET conversion events

### Implementation

#### Endpoint
```
POST https://bingads.microsoft.com/Customer/v13/OfflineConversions
```

#### Server-Side Upload (via API or bulk file)
```javascript
async function sendBingConversion(conversionData) {
  // Microsoft Advertising API requires OAuth
  const accessToken = await getMicrosoftAccessToken();
  
  const conversion = {
    conversionName: 'Lead Submission',
    conversionTime: new Date().toISOString(),
    conversionValue: 150.00,
    conversionCurrencyCode: 'USD',
    microsoftClickId: conversionData.msclkid, // Capture from URL
    hashedEmailAddress: hashSha256(conversionData.email),
    hashedPhoneNumber: hashSha256(conversionData.phone)
  };
  
  const response = await fetch(
    'https://bingads.microsoft.com/api/v13/offlineconversions',
    {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${accessToken}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ conversions: [conversion] })
    }
  );
  
  return response.json();
}
```

### Note
- Requires capturing `msclkid` from URL on landing
- Upload within 90 days of click
- Can fully replace client-side UET for conversions

---

## 5. Taboola S2S Conversion API

### What It Replaces
- tfa.js (~20KB)
- Taboola pixel conversion events

### Implementation

#### Endpoint
```
GET https://trc.taboola.com/actions-handler/log/3/s2s-action
```

#### Server-Side Code
```javascript
async function sendTaboolaConversion(clickId, eventName, revenue) {
  const params = new URLSearchParams({
    'click-id': clickId,     // Captured from initial click
    'name': eventName,       // e.g., 'lead', 'purchase'
    'revenue': revenue,
    'currency': 'USD'
  });
  
  const response = await fetch(
    `https://trc.taboola.com/actions-handler/log/3/s2s-action?${params.toString()}`,
    { method: 'GET' }
  );
  
  return response.ok;
}
```

### Required
- Capture `tblci` (Taboola click ID) from URL on landing page
- Store in session/cookie for conversion

---

## 6. Outbrain S2S Conversion API

### What It Replaces
- obtp.js (~15KB)
- Outbrain pixel conversion events

### Implementation

#### Endpoint
```
GET https://tr.outbrain.com/events/conversion/your-marketer-id
```

#### Server-Side Code
```javascript
async function sendOutbrainConversion(clickId, conversionValue) {
  const params = new URLSearchParams({
    'ob_click_id': clickId,
    'name': 'Lead',
    'value': conversionValue,
    'currency': 'USD',
    'orderId': generateOrderId()
  });
  
  const response = await fetch(
    `https://tr.outbrain.com/events/conversion/0084a3fde60ab652bf9cd5cc0158baee25?${params.toString()}`,
    { method: 'GET' }
  );
  
  return response.ok;
}
```

### Required
- Capture `obOrigUrl` or `outbrain_click_id` from URL
- Store in session/cookie for conversion

---

## Implementation Architecture

### Recommended Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER JOURNEY                              │
└─────────────────────────────────────────────────────────────────┘

1. USER CLICKS AD
   │
   ├─── URL contains: gclid, fbclid, msclkid, tblci, ef_tid
   │
   ▼
2. LANDING PAGE (Minimal Client JS)
   │
   ├─── Capture all click IDs from URL
   ├─── Store in first-party cookie or session
   ├─── Capture _fbp, _fbc cookies
   │
   ▼
3. FORM SUBMISSION → YOUR SERVER
   │
   ├─── Receive form data + click IDs
   ├─── Process lead (save to DB, send to CRM)
   │
   ▼
4. SERVER FIRES ALL CONVERSIONS (Parallel)
   │
   ├─── Meta CAPI ────────► Meta Servers
   ├─── Google Enhanced ──► Google Ads API
   ├─── Everflow S2S ─────► Everflow Servers
   ├─── Bing Offline ─────► Microsoft API
   ├─── Taboola S2S ──────► Taboola Servers
   └─── Outbrain S2S ─────► Outbrain Servers
```

### Server-Side Unified Conversion Handler

```javascript
// Node.js/Next.js API Route Example
export async function POST(req) {
  const formData = await req.json();
  const cookies = parseCookies(req);
  
  // Get all click IDs
  const clickIds = {
    gclid: cookies.gclid || formData.gclid,
    fbclid: cookies.fbclid,
    fbc: cookies._fbc,
    fbp: cookies._fbp,
    msclkid: cookies.msclkid,
    tblci: cookies.tblci,
    ef_tid: cookies.ef_tid
  };
  
  // Generate unified event ID
  const eventId = `conv_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
  
  // Fire all conversions in parallel
  const results = await Promise.allSettled([
    sendMetaConversion(eventId, formData, clickIds),
    sendGoogleEnhancedConversion(eventId, formData, clickIds),
    sendEverflowConversion(clickIds.ef_tid, formData),
    sendBingConversion(formData, clickIds.msclkid),
    sendTaboolaConversion(clickIds.tblci, 'lead', 150),
    sendOutbrainConversion(clickIds.obclid, 150)
  ]);
  
  // Log results
  console.log('Conversion results:', results);
  
  return Response.json({ success: true, eventId });
}
```

---

## Minimal Client-Side Code Required

Even with server-side integrations, you need minimal JS to:

1. **Capture click IDs from URL**
2. **Capture Meta cookies (_fbp, _fbc)**
3. **Store in first-party cookies**

```html
<!-- Minimal Tracking Bootstrap (~500 bytes) -->
<script>
(function() {
  var p = new URLSearchParams(location.search);
  var c = function(n,v,d) { document.cookie = n+'='+v+';path=/;max-age='+(d||86400); };
  var g = function(n) { var m = document.cookie.match(new RegExp('(^| )'+n+'=([^;]+)')); return m ? m[2] : ''; };
  
  // Capture click IDs
  ['gclid','msclkid','tblci','fbclid','_ef_transaction_id'].forEach(function(k) {
    var v = p.get(k); if (v) c(k, v, 2592000);
  });
  
  // Capture/generate fbc
  if (!g('_fbc') && p.get('fbclid')) {
    c('_fbc', 'fb.1.'+Date.now()+'.'+p.get('fbclid'), 7776000);
  }
  
  // Expose for form submission
  window.trackingIds = {
    gclid: g('gclid'),
    msclkid: g('msclkid'),
    tblci: g('tblci'),
    ef_tid: g('_ef_transaction_id'),
    fbc: g('_fbc'),
    fbp: g('_fbp')
  };
})();
</script>
```

---

## Summary: What Changes

### REMOVE from GTM (10 Tags)
1. ❌ Everflow Click Script
2. ❌ Everflow Conversion Script
3. ❌ Bright Horizon Media Meta all page Pixel
4. ❌ Bright Horizon Media- Meta Pixel - ViewContent
5. ❌ Bright Horizon Media Meta Pixel - InitiateCheckout
6. ❌ Bright Horizon Media Meta Pixel - Contact
7. ❌ Bright Horizon Media Meta Pixel - OutboundClick
8. ❌ Bright horizon Media - Lead Submit Meta Pixel
9. ❌ Taboola Pixel
10. ❌ Outbrain Pixel

### KEEP in GTM (3 Tags)
1. ✅ Google Tag (minimal, for click tracking)
2. ✅ Conversion Linker (for GCLID)
3. ✅ Microsoft Clarity (no server-side alternative)

### ADD to Server
1. ➕ Meta CAPI endpoint
2. ➕ Google Enhanced Conversions upload
3. ➕ Everflow S2S postback
4. ➕ Bing Offline Conversions upload
5. ➕ Taboola S2S conversion
6. ➕ Outbrain S2S conversion

### Scripts Eliminated
| Script | Size | Status |
|--------|------|--------|
| fbevents.js | ~50KB | ❌ Removed |
| Everflow main.js x2 | ~30KB | ❌ Removed |
| tfa.js (Taboola) | ~20KB | ❌ Removed |
| obtp.js (Outbrain) | ~15KB | ❌ Removed |
| bat.js (Bing) | ~15KB | ❌ Removed |
| **Total Saved** | **~130KB** | |

### Estimated Speed Improvement
- **Initial Page Load:** 300-500ms faster
- **Time to Interactive:** 200-400ms faster
- **Lighthouse Score:** +15-25 points
