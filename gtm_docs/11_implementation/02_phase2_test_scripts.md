# Phase 2: Test Scripts

---

## Timeline

Day 2

---

## Test Click ID Capture

- [ ] Visit site with `?affid=123&oid=456`
- [ ] Open DevTools Console
- [ ] Run `window.clickIds`
- [ ] Verify ef_affid = "123"
- [ ] Verify ef_oid = "456"
- [ ] Check cookies for ef_affid, ef_oid

---

## Test Google Tag

- [ ] Check Network tab for gtag.js request
- [ ] Open GA4 DebugView
- [ ] Verify page_view event fires

---

## Test Microsoft Clarity

- [ ] Check Network tab for clarity.ms request
- [ ] Verify correct project ID in URL

---

## Test Everflow Click

- [ ] Visit with `?affid=123&oid=456`
- [ ] Check Network tab for pvm75hjsg.com request
- [ ] Verify click fires with correct params
- [ ] Check dataLayer for everflow_click event

---

## Test GA4 Form Events

- [ ] Click through each form step
- [ ] Check GA4 DebugView for each event:
  - [ ] name_of_your_business
  - [ ] legal_entity
  - [ ] address
  - [ ] business_description
  - [ ] years_in_business
  - [ ] partners_owners
  - [ ] employees
  - [ ] revenue
  - [ ] payroll
  - [ ] type_of_coverage
  - [ ] First_Last
  - [ ] email
  - [ ] phone

---

## Test Tel Click

- [ ] Click any phone number link
- [ ] Check GA4 DebugView for tel_click event
- [ ] Check dataLayer for tel_click event

---

## Test Everflow Conversion

- [ ] Submit test lead
- [ ] Check Network tab for Everflow conversion
- [ ] Check Console for "✅ Lead tracked" message
- [ ] Check dataLayer for lead_submission event
- [ ] Verify event_id is present

---

## Test Thank You Page

- [ ] Complete test submission
- [ ] Arrive on thank you page
- [ ] Inspect links to pvm75hjsg.com
- [ ] Verify sub1 parameter has transaction ID
- [ ] Check console for transaction ID log
