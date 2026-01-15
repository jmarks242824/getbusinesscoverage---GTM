# Thank You Page Script

---

## File Name

`04_thankyou_page_script.html`

---

## Purpose

Rewrites links on thank you page to include Everflow transaction ID for downstream tracking.

---

## Placement

Before `</body>`, after other scripts

---

## Pages

**THANK YOU PAGE ONLY**

⚠️ Do NOT add to all pages

---

## Configuration

| Property | Value |
|----------|-------|
| siteUrl | www.pvm75hjsg.com |
| advertiserId | 1 |

---

## What It Does

1. Loads Everflow SDK (everflow.js)
2. Fires EF.click() to get transaction ID
3. Finds all links containing pvm75hjsg.com
4. Rewrites links to include sub1={transactionId}

---

## Code

```html
<script type="text/javascript">
var script = document.createElement('script');
script.src = "https://www.pvm75hjsg.com/scripts/sdk/everflow.js";
script.defer = true;
script.onload = function() {
    var siteUrl = "www.pvm75hjsg.com";
    var advertiserId = 1;
    var elems = document.body.getElementsByTagName("a");
    
    EF.click({
        offer_id: EF.urlParameter('oid'),
        affiliate_id: EF.urlParameter('affid'),
        sub1: EF.urlParameter('sub1'),
        sub2: EF.urlParameter('sub2'),
        sub3: EF.urlParameter('sub3'),
        sub4: EF.urlParameter('sub4'),
        sub5: EF.urlParameter('sub5'),
        uid: EF.urlParameter('uid'),
        source_id: EF.urlParameter('source_id'),
        transaction_id: EF.urlParameter('_ef_transaction_id'),
    }).then(function(transactionId) {
        console.log('Everflow Transaction ID:', transactionId);
        
        if (transactionId) {
            for (var i = 0; i < elems.length; i++) {
                if (elems[i].href.includes(siteUrl)) {
                    var url = new URL(elems[i].href);
                    url.searchParams.set("sub1", transactionId);
                    elems[i].href = url.href;
                }
            }
        } else {
            for (var i = 0; i < elems.length; i++) {
                if (elems[i].href.includes(siteUrl)) {
                    var url = new URL(elems[i].href);
                    url.searchParams.set("sub1", EF.getAdvertiserTransactionId(advertiserId));
                    elems[i].href = url.href;
                }
            }
        }
    });
}
document.body.append(script);
</script>
```

---

## Testing

1. Complete a test form submission
2. Arrive on thank you page
3. Inspect links containing pvm75hjsg.com
4. Verify sub1 parameter contains transaction ID
5. Check console for "Everflow Transaction ID:" log

---

## Notes

- Uses different SDK file: everflow.js (not main.js)
- Falls back to getAdvertiserTransactionId() if no transaction ID
- Only affects links to pvm75hjsg.com domain
