# DataLayer Variables

## Current GTM Configuration

| Variable Name | Type | DataLayer Path |
|---------------|------|----------------|
| DL - Event ID | Data Layer Variable | event_id |
| DL - Value | Data Layer Variable | value |
| DL - Currency | Data Layer Variable | currency |
| DL - Email | Data Layer Variable | user_data.email |
| DL - Phone | Data Layer Variable | user_data.phone |
| DL - First Name | Data Layer Variable | user_data.first_name |
| DL - Last Name | Data Layer Variable | user_data.last_name |
| DL - Zip | Data Layer Variable | user_data.zip |
| DL - City | Data Layer Variable | user_data.city |
| DL - State | Data Layer Variable | user_data.state |
| DL - Country | Data Layer Variable | user_data.country |
| DL - Event Type | Data Layer Variable | event_type |
| leadValue | Data Layer Variable | value |
| Google User provided Data | Google Ads User Data | (Enhanced Conversions) |

## What They Do

These variables extract data from the dataLayer for use in GTM tags:
- **Event ID:** Used for conversion deduplication (Meta + Google)
- **Value/Currency:** Conversion value tracking
- **User Data:** Enhanced conversions (Google Ads) and Advanced Matching (Meta)
- **Event Type:** Custom event classification

## DataLayer Structure (Source)

The Everflow Conversion Script pushes this structure:

```javascript
dataLayer.push({
  'event': 'lead_submission',
  'event_id': 'conv_1234567890_abc',
  'event_time': 1705276800,
  'value': 150.00,
  'currency': 'USD',
  'user_data': {
    'email': 'john@example.com',
    'phone': '+15551234567',
    'first_name': 'john',
    'last_name': 'smith',
    'zip': '60601',
    'city': 'chicago',
    'state': 'il',
    'country': 'US'
  },
  'business_name': 'Acme Corp',
  'coverage_type': 'general_liability'
});
```

## Can Remove from GTM? ⚠️ CONDITIONAL

**If moving all tags to page:** Yes, remove all variables

**If keeping Google Ads Conversion in GTM:** Keep these variables:
- DL - Event ID
- DL - Value
- DL - Currency
- DL - Email
- DL - Phone
- DL - First Name
- DL - Last Name
- DL - Zip
- DL - City
- DL - State
- DL - Country
- Google User provided Data

**Variables to remove regardless:**
- `leadValue` (duplicate of DL - Value)
- `DL - Event Type` (not used by essential tags)

## Google User provided Data Variable

This special variable type maps user data for Google Ads Enhanced Conversions:

```javascript
// GTM Variable Configuration
{
  email: {{DL - Email}},
  phone_number: {{DL - Phone}},
  first_name: {{DL - First Name}},
  last_name: {{DL - Last Name}},
  home_address: {
    postal_code: {{DL - Zip}},
    city: {{DL - City}},
    region: {{DL - State}},
    country: {{DL - Country}}
  }
}
```

## Notes

- All user data should be lowercase and trimmed before pushing to dataLayer
- Phone must be E.164 format (+1XXXXXXXXXX)
- GTM automatically hashes data for Google Ads Enhanced Conversions
- Meta Pixel also auto-hashes Advanced Matching data
- Event ID is critical - same ID must be used across all platforms
