# GA4 Form Events Code

---

## Purpose

Tracks all form step interactions in GA4.

---

## Placement

Before `</body>`, inside 02_body_scripts.html

---

## Event Mappings - Direct

| Element ID | GA4 Event |
|------------|-----------|
| next_businessName | name_of_your_business |
| next_businessAddress | address |
| next_businessDescription | business_description |
| next_linesOfBusiness | type_of_coverage |
| next_name | First_Last |
| next_email | email |
| submit_phone | phone |

---

## Event Mappings - Prefix

| ID Prefix | GA4 Event |
|-----------|-----------|
| legalEntity_ | legal_entity |
| yearsInBusiness_ | years_in_business |
| numberOfOwners_ | partners_owners |
| numberOfEmployees_ | employees |
| annualRevenue_ | revenue |
| annualPayroll_ | payroll |

---

## Code

```html
<script>
document.addEventListener('DOMContentLoaded', function() {
  
  var directEvents = {
    'next_businessName': 'name_of_your_business',
    'next_businessAddress': 'address',
    'next_businessDescription': 'business_description',
    'next_linesOfBusiness': 'type_of_coverage',
    'next_name': 'First_Last',
    'next_email': 'email',
    'submit_phone': 'phone'
  };
  
  var prefixEvents = {
    'legalEntity_': 'legal_entity',
    'yearsInBusiness_': 'years_in_business',
    'numberOfOwners_': 'partners_owners',
    'numberOfEmployees_': 'employees',
    'annualRevenue_': 'revenue',
    'annualPayroll_': 'payroll'
  };
  
  document.addEventListener('click', function(e) {
    var target = e.target.closest('[id]');
    if (!target) return;
    
    var id = target.id;
    var eventName = null;
    
    if (directEvents[id]) {
      eventName = directEvents[id];
    } else {
      for (var prefix in prefixEvents) {
        if (id.startsWith(prefix)) {
          eventName = prefixEvents[prefix];
          break;
        }
      }
    }
    
    if (eventName && typeof gtag !== 'undefined') {
      gtag('event', eventName, {
        'event_category': 'form_interaction',
        'event_label': id
      });
    }
  });
  
});
</script>
```

---

## Testing

1. Click through form steps
2. Open GA4 DebugView
3. Verify each event fires with correct name

---

## Replaces 13 GTM Tags

Tags 13-25 (except Tel click)
