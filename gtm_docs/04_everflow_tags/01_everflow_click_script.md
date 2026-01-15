# Everflow Click Script

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Everflow Click Script |
| Tag ID | 9 |
| Type | Custom HTML |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | All Pages |
| Trigger Type | DOM Ready |
| Trigger ID | 2147479553 |

---

## External Script

| Property | Value |
|----------|-------|
| URL | https://www.pvm75hjsg.com/scripts/main.js |
| Size | ~15KB |

---

## Parameters Captured

| URL Param | Purpose |
|-----------|---------|
| affid | Affiliate ID |
| oid | Offer ID |
| sub1 | Sub parameter 1 |
| sub2 | Sub parameter 2 |
| sub3 | Sub parameter 3 |
| sub4 | Sub parameter 4 |
| sub5 | Sub parameter 5 |
| uid | User ID |
| source_id | Source ID |
| _ef_transaction_id | Transaction ID |
| affid2 | Secondary affiliate (BH Media) |
| oid2 | Secondary offer |

---

## Code

```html
<script type="text/javascript" src="https://www.pvm75hjsg.com/scripts/main.js"></script>
<script type="text/javascript">
  window.dataLayer = window.dataLayer || [];
  
  if (EF.urlParameter("affid2")) {
    // Dual-affiliate flow (BH Media)
    EF.click({
      tracking_domain: "https://www.bhmediatrack.com",
      offer_id: EF.urlParameter("oid2"),
      affiliate_id: EF.urlParameter("affid2"),
      sub1: EF.urlParameter("sub6")
    }).then(function(transaction_id) {
      dataLayer.push({
        'event': 'everflow_click',
        'event_id': transaction_id
      });
      
      // Chain to primary
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
        source_id: EF.urlParameter("source_id"),
      });
    });
  } else {
    // Standard single flow
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
  }
</script>
```

---

## What It Does

1. Loads Everflow SDK
2. Captures affiliate params from URL
3. Fires EF.click() for attribution
4. Handles dual-affiliate flow
5. Pushes everflow_click to dataLayer

---

## Action

| Decision | Reason |
|----------|--------|
| 🔄 MOVE TO PAGE | See 10_page_scripts/05_everflow_click_code.md |
