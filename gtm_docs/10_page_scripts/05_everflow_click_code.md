# Everflow Click Code

---

## Purpose

Fires Everflow click tracking for affiliate attribution.

---

## Placement

Inside `<head>`, after Microsoft Clarity

---

## Tracking Domains

| Domain | Owner |
|--------|-------|
| pvm75hjsg.com | The Switchboard |
| bhmediatrack.com | Bright Horizon Media |

---

## Dependencies

Requires Click ID Capture to run first (uses `window.clickIds`)

---

## Code

```html
<script type="text/javascript" src="https://www.pvm75hjsg.com/scripts/main.js"></script>
<script>
(function(){
  window.dataLayer = window.dataLayer || [];
  
  if (!window.clickIds.ef_affid) return;
  
  if (window.clickIds.ef_affid2) {
    // Dual-affiliate flow (BH Media)
    EF.click({
      tracking_domain: "https://www.bhmediatrack.com",
      offer_id: window.clickIds.ef_oid2,
      affiliate_id: window.clickIds.ef_affid2,
      sub1: window.clickIds.ef_sub1
    }).then(function(transaction_id) {
      dataLayer.push({
        'event': 'everflow_click',
        'event_id': transaction_id
      });
      
      EF.click({
        tracking_domain: "https://www.pvm75hjsg.com",
        offer_id: window.clickIds.ef_oid,
        affiliate_id: window.clickIds.ef_affid,
        sub1: window.clickIds.ef_sub1,
        sub2: window.clickIds.ef_sub2,
        sub3: window.clickIds.ef_sub3,
        sub4: window.clickIds.ef_sub4,
        sub5: transaction_id,
        uid: window.clickIds.ef_uid,
        source_id: window.clickIds.ef_source_id
      });
    });
  } else {
    // Standard single flow
    EF.click({
      tracking_domain: "https://www.pvm75hjsg.com",
      offer_id: window.clickIds.ef_oid,
      affiliate_id: window.clickIds.ef_affid,
      sub1: window.clickIds.ef_sub1,
      sub2: window.clickIds.ef_sub2,
      sub3: window.clickIds.ef_sub3,
      sub4: window.clickIds.ef_sub4,
      sub5: window.clickIds.ef_sub5,
      uid: window.clickIds.ef_uid,
      source_id: window.clickIds.ef_source_id,
      transaction_id: window.clickIds.ef_tid || ''
    }).then(function(tid) {
      if (tid) {
        dataLayer.push({
          'event': 'everflow_click',
          'event_id': tid
        });
      }
    });
  }
})();
</script>
```

---

## Testing

1. Visit with params: `?affid=123&oid=456`
2. Check Network tab for request to pvm75hjsg.com
3. Verify click fires with correct parameters

---

## Replaces GTM Tag

| Tag Name | Tag ID |
|----------|--------|
| Everflow Click Script | 9 |
