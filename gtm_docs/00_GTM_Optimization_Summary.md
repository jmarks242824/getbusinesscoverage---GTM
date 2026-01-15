# GTM Optimization Summary
## Fast Business Quote - Speed Improvement Plan

**Goal:** Reduce page load time by moving scripts out of GTM and implementing direct integrations where possible.

---

## Executive Summary

| Category | Current | Recommended | Impact |
|----------|---------|-------------|--------|
| GTM Tags | 28 | 8 | -71% tags |
| External Scripts | 7 | 3 | -57% network requests |
| Custom HTML Tags | 10 | 0 | -100% (all moved to page) |
| Estimated Load Reduction | - | ~200-400ms | Significant |

---

## 🗑️ TAGS TO REMOVE (Duplicates/Unnecessary)

### 1. Bright Horizon Media - All in one Meta pixel
**Reason:** DUPLICATE - Fires same Lead event as "Lead Submit Meta Pixel"
- Both fire on `lead_submission` trigger
- Causes double counting of conversions
- Remove one or the other

### 2. Bright horizon Media - Lead Submit Meta Pixel  
**Reason:** DUPLICATE - Keep the All-in-one version OR this one, not both
- Recommend keeping this one (simpler, cleaner)
- Remove the All-in-one version

### 3. Facebook Pixel Lead Submit (Community Template - cvt_5RM3Q)
**Pixel ID:** 1421331895722314
**Reason:** THIRD Meta pixel firing Lead events
- Different pixel ID than Bright Horizon Media (1044730294266508)
- Is this intentional? If not, REMOVE
- If intentional, consolidate into single implementation

### 4. Facebook Pixel Call (Community Template - cvt_5RM3Q)
**Pixel ID:** 1421331895722314
**Reason:** Same as above - different pixel ID
- Review if this pixel is still needed

### 5. Cubadica - All page Pixel
**Pixel ID:** 1936389293471725
**Question:** Is Cubadica still an active partner?
- If NO → REMOVE entirely
- If YES → Consolidate with other Meta pixels

### 6. businessName (GA4 Event)
**Reason:** DUPLICATE of "Name of your Business" 
- Both fire on same trigger (Business Name click)
- Remove one

### 7. Compare Plans (GA4 Event)
**Reason:** No trigger assigned
- Tag has no firing trigger
- Either assign trigger or REMOVE

---

## 📄 TAGS TO MOVE TO PAGE (Direct Integration)

### HIGH PRIORITY (Major Speed Impact)

| Tag | Current Load | Move To | Speed Gain |
|-----|--------------|---------|------------|
| Everflow Click Script | GTM HTML | Page `<head>` | ~50-100ms |
| Everflow Conversion Script | GTM HTML | Page `<body>` | ~30-50ms |
| Meta Base Pixel | GTM HTML | Page `<head>` | ~50-100ms |
| Microsoft Clarity | GTM Template | Page `<head>` | ~30-50ms |

### MEDIUM PRIORITY

| Tag | Current Load | Move To | Speed Gain |
|-----|--------------|---------|------------|
| Meta ViewContent | GTM HTML | Conditional in page | ~10-20ms |
| Meta InitiateCheckout | GTM HTML | Conditional in page | ~10-20ms |
| Meta Contact (Click-to-Call) | GTM HTML | Page event listener | ~10-20ms |
| Meta OutboundClick | GTM HTML | Page event listener | ~10-20ms |
| Taboola Pixel | GTM Template | Page `<head>` | ~20-30ms |
| Outbrain Pixel | GTM Template | Page `<head>` | ~20-30ms |

### LOW PRIORITY (Keep in GTM)

| Tag | Reason to Keep |
|-----|----------------|
| Google Tag | Foundation for Google ecosystem |
| Conversion Linker | GTM-native, cross-domain handling |
| Google Ads Conversion | Enhanced conversions easier in GTM |
| Microsoft UET | Complex event handling |
| GA4 Form Events | Can consolidate, but GTM works |

---

## 🔧 DIRECT INTEGRATION ALTERNATIVES

### Instead of GTM Scripts → Use Server-Side or API Integrations

| Platform | GTM Method | Direct Alternative | Benefit |
|----------|------------|-------------------|---------|
| Everflow | Client JS | Server-side postback | No client script needed |
| Meta | Pixel JS | Conversions API (CAPI) | Better attribution, no pixel needed |
| Google Ads | gtag.js | Enhanced Conversions API | Server-side, more reliable |
| Microsoft | UET JS | Offline Conversions API | No client script |

### Meta Conversions API (CAPI) - Recommended

Instead of client-side Meta Pixel, implement server-side CAPI:

```javascript
// Server-side (Node.js example)
const bizSdk = require('facebook-nodejs-business-sdk');
const ServerEvent = bizSdk.ServerEvent;
const EventRequest = bizSdk.EventRequest;
const UserData = bizSdk.UserData;

const userData = (new UserData())
  .setEmail('john@example.com')
  .setPhone('+15551234567')
  .setFirstName('john')
  .setLastName('smith')
  .setZip('60601');

const serverEvent = (new ServerEvent())
  .setEventName('Lead')
  .setEventTime(Math.floor(Date.now() / 1000))
  .setUserData(userData)
  .setEventId('your_event_id')
  .setActionSource('website');

const eventsData = [serverEvent];
const eventRequest = (new EventRequest(access_token, pixel_id))
  .setEvents(eventsData);

eventRequest.execute();
```

**Benefits:**
- No fbevents.js needed on client (saves ~50KB)
- Works with ad blockers
- Better iOS 14.5+ attribution
- Event ID deduplication built-in

### Everflow Server-Side Postback

Instead of client-side EF.conversion(), use server postback:

```
https://www.pvm75hjsg.com/sdk/conversion?
  aid=373
  &adv1={first_name}
  &adv2={last_name}
  &adv3={email}
  &adv4={phone}
  &adv5={zip}
  &amount=150
  &transaction_id={ef_tid}
```

**Benefits:**
- No Everflow script needed on client
- Fire from your backend after form submission
- More reliable than client-side

---

## 📊 PIXEL ID REFERENCE

| Owner | Pixel ID | Purpose | Status |
|-------|----------|---------|--------|
| Bright Horizon Media | 1044730294266508 | Primary Meta tracking | KEEP |
| Facebook Template | 1421331895722314 | Unknown | REVIEW |
| Cubadica | 1936389293471725 | Partner | REVIEW |
| Microsoft Clarity | rrejdlholl | Session recording | KEEP |
| Microsoft UET | 97200753 | Bing Ads | KEEP |
| Taboola | 1901590 | Native ads | KEEP if running |
| Outbrain | 0084a3fde60ab652bf9cd5cc0158baee25 | Native ads | KEEP if running |
| Google Tag | GT-5RMBWCN6 | GA4/Ads | KEEP |
| Google Ads | 17097160893 | Conversions | KEEP |

---

## 🚀 RECOMMENDED IMPLEMENTATION ORDER

### Phase 1: Remove Duplicates (Day 1)
1. Remove "Bright Horizon Media - All in one Meta pixel"
2. Remove duplicate "businessName" GA4 event
3. Remove or fix "Compare Plans" (no trigger)
4. Review Facebook Pixel templates (1421331895722314)

### Phase 2: Move Critical Scripts to Page (Day 2-3)
1. Move Everflow Click Script to page `<head>`
2. Move Meta Base Pixel to page `<head>`
3. Move Microsoft Clarity to page `<head>`
4. Consolidate Meta pixels into single init

### Phase 3: Move Event Scripts to Page (Day 4-5)
1. Move Everflow Conversion Script to page
2. Combine all Meta event tracking into page
3. Move Taboola/Outbrain to page (if keeping)

### Phase 4: Server-Side Integration (Week 2)
1. Implement Meta Conversions API (CAPI)
2. Implement Everflow server-side postback
3. Remove client-side scripts where possible

### Phase 5: Cleanup (Week 3)
1. Review GA4 form events - consolidate into single handler
2. Remove Cubadica pixel if not needed
3. Clean up Conversion Linker domains (remove old Vercel previews)
4. Final GTM audit

---

## 📉 EXPECTED RESULTS

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| GTM Container Size | ~150KB | ~50KB | -67% |
| External Script Requests | 7 | 3 | -57% |
| Time to Interactive | +400ms | +150ms | -250ms |
| Lighthouse Score | ~60 | ~80 | +20 points |
| First Contentful Paint | Delayed | Faster | Significant |

---

## ⚠️ CRITICAL NOTES

1. **Event ID Deduplication:** Ensure the same event_id is used across:
   - Client-side Meta Pixel
   - Server-side CAPI (if implementing)
   - Google Ads Enhanced Conversions

2. **Testing:** Before removing any tags:
   - Use GTM Preview mode
   - Verify conversions still track
   - Check Meta Events Manager
   - Verify Google Ads conversions

3. **Consent Mode:** If implementing GDPR consent:
   - Keep consent-dependent tags in GTM
   - Or implement consent checks in page scripts

4. **Three Meta Pixel IDs Found:**
   - 1044730294266508 (Bright Horizon Media)
   - 1421331895722314 (Facebook Template)
   - 1936389293471725 (Cubadica)
   
   This needs clarification - are all three needed?
