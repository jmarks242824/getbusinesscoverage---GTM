# Everflow Click Script

## Current GTM Configuration

| Setting | Value |
|---------|-------|
| Tag Name | Everflow Click Script |
| Tag Type | Custom HTML |
| Fires On | All Pages (DOM Ready) |
| Firing Option | Once Per Event |

## Tracking Domains

| Domain | Purpose |
|--------|---------|
| pvm75hjsg.com | Primary Switchboard tracking |
| bhmediatrack.com | Bright Horizon Media (affiliate) |

## What It Does

1. Loads Everflow main.js script
2. Captures URL parameters (affid, oid, sub1-5, uid, source_id)
3. Handles dual-affiliate tracking (affid2 flow)
4. Fires EF.click() to register click attribution
5. Pushes `everflow_click` event to dataLayer

## Current GTM Code

```html
<script type="text/javascript" src="https://www.pvm75hjsg.com/scripts/main.js"></script>
<script type="text/javascript">
  window.dataLayer = window.dataLayer || [];
  
  if (EF.urlParameter("affid2")) {
    // Dual affiliate flow - BH Media first, then Switchboard
    EF.click({
      tracking_domain: "https://www.bhmediatrack.com",
      offer_id: EF.urlParameter("oid2"),
      affiliate_id: EF.urlParameter("affid2"),
      sub1: EF.urlParameter("sub6")
    }).then(function(transaction_id) {
      
      // Push to dataLayer for Meta/Google
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
        sub5: transaction_id,  // Links BH Media transaction
        uid: EF.urlParameter("uid"),
        source_id: EF.urlParameter("source_id"),
      });
    });
  } else {
    // Standard single affiliate flow
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
      transaction_id: efTransactionId,
    });
    
    // Push to dataLayer
    if (efTransactionId) {
      dataLayer.push({
        'event': 'everflow_click',
        'event_id': efTransactionId,
        'affiliate_id': EF.urlParameter("affid"),
        'offer_id': EF.urlParameter("oid")
      });
    }
  }
</script>
```

## Can Move to Page? ✅ YES (RECOMMENDED)

**Recommendation:** MOVE TO PAGE

**Reason:**
- Click attribution should fire as early as possible
- Reduces GTM processing overhead
- No dependency on other GTM tags
- Critical for affiliate revenue tracking

**Implementation:**
1. Add to page `<head>` or early in `<body>`
2. Ensure dataLayer is initialized first
3. Remove from GTM

## Page Placement

```html
<head>
  <!-- Initialize dataLayer first -->
  <script>
    window.dataLayer = window.dataLayer || [];
  </script>
  
  <!-- Everflow Click Script -->
  <script type="text/javascript" src="https://www.pvm75hjsg.com/scripts/main.js"></script>
  <script type="text/javascript">
    // ... (full script from above)
  </script>
</head>
```

## URL Parameters Captured

| Parameter | Purpose | Example |
|-----------|---------|---------|
| affid | Affiliate ID | 12345 |
| oid | Offer ID | 373 |
| sub1-sub5 | Sub-affiliate tracking | campaign_name |
| uid | User ID | user_abc123 |
| source_id | Traffic source | google_cpc |
| _ef_transaction_id | Pre-existing transaction | ef_123456789 |
| affid2 | Secondary affiliate | 67890 |
| oid2 | Secondary offer | 456 |

## Dependencies

- Required by: Everflow Conversion Script (for transaction linking)
- Requires: Everflow main.js CDN

## Performance Impact

- **Load:** ~15KB (main.js)
- **Timing:** Should fire immediately on page load
- **Network:** 1-2 API calls to Everflow
- **Recommendation:** Move to page for faster attribution

## DataLayer Output

```javascript
// On successful click registration
{
  'event': 'everflow_click',
  'event_id': 'ef_1234567890',
  'affiliate_id': '12345',
  'offer_id': '373'
}
```

## Notes

- Dual-affiliate flow (affid2) links BH Media transaction to Switchboard via sub5
- Transaction ID is critical for conversion attribution
- If no _ef_transaction_id in URL, Everflow generates one
