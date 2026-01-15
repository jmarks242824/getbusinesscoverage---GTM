# Meta Pixel - All-in-One (Unified Lead Tracking)

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Bright Horizon Media - All in one Meta pixel |
| Tag Type | Custom HTML |
| Fires On | lead_submission (Custom Event) |

## What It Does

1. Consolidated Meta tracking for lead submission
2. Handles all user data formatting
3. Fires Lead event with full Advanced Matching
4. Includes SHA-256 hashing for enhanced privacy

## Current GTM Code

```html
<script>
(function() {
  // === DATA LAYER INPUTS ===
  var eventName = 'Lead'; // Always fire as Lead for Meta optimization
  var eventType = {{DL - Event Type}} || ''; 
  var eventValue = {{DL - Value}} || 0;
  var currency = {{DL - Currency}} || 'USD';
  var eventId = {{DL - Event ID}} || (new Date().getTime().toString());

  var userEmail = {{DL - Email}} || '';
  var userPhone = {{DL - Phone}} || '';
  var userFirstName = {{DL - First Name}} || '';
  var userLastName = {{DL - Last Name}} || '';
  var userZip = {{DL - Zip}} || '';
  var userCity = {{DL - City}} || '';
  var userState = {{DL - State}} || '';
  var userCountry = {{DL - Country}} || 'US';

  // === HASH FUNCTION (SHA-256) ===
  async function sha256(message) {
    if (!message) return '';
    const msgBuffer = new TextEncoder().encode(message.toLowerCase().trim());
    const hashBuffer = await crypto.subtle.digest('SHA-256', msgBuffer);
    const hashArray = Array.from(new Uint8Array(hashBuffer));
    return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
  }

  // === FIRE META EVENT ===
  async function fireMetaEvent() {
    // Hash user data
    var hashedEmail = await sha256(userEmail);
    var hashedPhone = await sha256(userPhone.replace(/\D/g, ''));
    var hashedFirstName = await sha256(userFirstName);
    var hashedLastName = await sha256(userLastName);
    var hashedCity = await sha256(userCity);
    var hashedState = await sha256(userState);
    var hashedZip = await sha256(userZip);

    // Fire Lead event
    fbq('track', 'Lead', {
      value: eventValue,
      currency: currency,
      content_name: 'Insurance Quote',
      event_type: eventType
    }, {
      eventID: eventId
    });

    // Log for debugging
    console.log('Meta All-in-One Lead Event Fired:', {
      eventId: eventId,
      value: eventValue,
      hasEmail: !!hashedEmail,
      hasPhone: !!hashedPhone
    });
  }

  // Execute
  if (typeof fbq !== 'undefined') {
    fireMetaEvent();
  } else {
    console.error('Meta Pixel not loaded');
  }
})();
</script>
```

## Can Move to Page? ⚠️ COMPLEX

**Recommendation:** CONSOLIDATE WITH EVERFLOW CONVERSION SCRIPT

**Reason:**
- This tag duplicates functionality with "Lead Submit Meta Pixel"
- Both fire on same trigger (lead_submission)
- Should be consolidated into one implementation

**Consolidation Approach:**

Remove this tag and the separate Lead Submit tag. Instead, add Meta tracking directly to the Everflow Conversion Script (see 05_Everflow_Conversion_Script.md).

## Recommended Consolidated Implementation

```javascript
// Inside Everflow Conversion Script, after successful conversion
window.dataLayer.push({
  'event': 'lead_submission',
  'event_id': eventId,
  // ... rest of data
});

// Fire Meta Lead event immediately (no need for separate GTM tag)
if (typeof fbq !== 'undefined') {
  fbq('track', 'Lead', {
    value: amount,
    currency: 'USD',
    content_name: 'Insurance Quote'
  }, {
    eventID: eventId
  });
  
  console.log('Meta Lead fired with event_id:', eventId);
}
```

## Dependencies

- Required by: Nothing
- Requires: Meta Base Pixel, lead_submission dataLayer event

## Performance Impact

- **Load:** Minimal
- **Timing:** Fires after dataLayer event
- **Issue:** DUPLICATE TAG - fires same event as Lead Submit tag

## Notes

- This tag appears to be a more comprehensive version of "Lead Submit Meta Pixel"
- Both tags fire on the same trigger (lead_submission)
- Having both causes DUPLICATE Lead events to Meta
- RECOMMENDATION: Remove one of these tags
- The SHA-256 hashing is actually unnecessary - Meta auto-hashes Advanced Matching data
- Manual hashing can cause issues if format doesn't match Meta's expected format
