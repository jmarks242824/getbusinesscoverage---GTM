# Click ID Capture

---

## Purpose

Captures all affiliate and advertising click IDs from URL parameters and stores them in cookies.

---

## Placement

Inside `<head>`, first script

---

## Parameters Captured

| URL Param | Cookie Name |
|-----------|-------------|
| affid | ef_affid |
| oid | ef_oid |
| sub1 | ef_sub1 |
| sub2 | ef_sub2 |
| sub3 | ef_sub3 |
| sub4 | ef_sub4 |
| sub5 | ef_sub5 |
| uid | ef_uid |
| source_id | ef_source_id |
| _ef_transaction_id | ef_tid |
| affid2 | ef_affid2 |
| oid2 | ef_oid2 |
| gclid | gclid |
| msclkid | msclkid |
| fbclid | fbclid |
| tblci | tblci |
| ob_click_id | obclid |

---

## Meta Cookies Generated

| Cookie | Purpose |
|--------|---------|
| _fbc | Facebook click ID |
| _fbp | Facebook browser ID |

---

## Code

```html
<script>
(function(){
  window.dataLayer = window.dataLayer || [];
  var p = new URLSearchParams(location.search);
  var c = function(n,v,d){document.cookie=n+'='+v+';path=/;max-age='+(d||2592000)};
  var g = function(n){var m=document.cookie.match(new RegExp('(^| )'+n+'=([^;]+)'));return m?m[2]:''};
  
  var params = {
    'affid': 'ef_affid',
    'oid': 'ef_oid', 
    'sub1': 'ef_sub1',
    'sub2': 'ef_sub2',
    'sub3': 'ef_sub3',
    'sub4': 'ef_sub4',
    'sub5': 'ef_sub5',
    'uid': 'ef_uid',
    'source_id': 'ef_source_id',
    '_ef_transaction_id': 'ef_tid',
    'affid2': 'ef_affid2',
    'oid2': 'ef_oid2',
    'gclid': 'gclid',
    'msclkid': 'msclkid',
    'fbclid': 'fbclid',
    'tblci': 'tblci',
    'ob_click_id': 'obclid'
  };
  
  for (var key in params) {
    var val = p.get(key);
    if (val) c(params[key], val);
  }
  
  if (!g('_fbc') && p.get('fbclid')) {
    c('_fbc', 'fb.1.' + Date.now() + '.' + p.get('fbclid'), 7776000);
  }
  
  if (!g('_fbp')) {
    c('_fbp', 'fb.1.' + Date.now() + '.' + Math.random().toString(36).substr(2, 12), 7776000);
  }
  
  window.clickIds = {
    ef_tid: g('ef_tid'),
    ef_affid: g('ef_affid'),
    ef_oid: g('ef_oid'),
    ef_sub1: g('ef_sub1'),
    ef_sub2: g('ef_sub2'),
    ef_sub3: g('ef_sub3'),
    ef_sub4: g('ef_sub4'),
    ef_sub5: g('ef_sub5'),
    ef_uid: g('ef_uid'),
    ef_source_id: g('ef_source_id'),
    ef_affid2: g('ef_affid2'),
    ef_oid2: g('ef_oid2'),
    gclid: g('gclid'),
    msclkid: g('msclkid'),
    fbc: g('_fbc'),
    fbp: g('_fbp'),
    tblci: g('tblci'),
    obclid: g('obclid')
  };
})();
</script>
```

---

## Testing

1. Visit: `yoursite.com/?affid=123&oid=456`
2. Open DevTools Console
3. Run: `window.clickIds`
4. Verify values populated
