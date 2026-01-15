# Final Testing Checklist

---

## Timeline

Day 4

---

## End-to-End Test

- [ ] Clear all cookies
- [ ] Clear browser cache
- [ ] Visit landing page with affiliate params
- [ ] Complete full form
- [ ] Submit lead
- [ ] Verify thank you page

---

## Verify Each Platform

### Google Analytics 4

- [ ] Check Realtime report
- [ ] Verify page_view events
- [ ] Verify form step events
- [ ] Verify generate_lead event

### Google Ads

- [ ] Check conversion in Google Ads
- [ ] Verify Enhanced Conversions data

### Everflow

- [ ] Check click in Everflow
- [ ] Check conversion in Everflow
- [ ] Verify sub parameters

### Microsoft Clarity

- [ ] Check for new session recording
- [ ] Verify heatmap data collecting

---

## Performance Check

- [ ] Run PageSpeed Insights (before)
- [ ] Run PageSpeed Insights (after)
- [ ] Document JavaScript reduction
- [ ] Document load time improvement

---

## Error Check

- [ ] Check browser Console for errors
- [ ] Check Network tab for failed requests
- [ ] Verify no 404 errors
- [ ] Verify no CORS errors

---

## Sign Off

| Check | Status |
|-------|--------|
| GA4 working | ☐ |
| Google Ads working | ☐ |
| Everflow working | ☐ |
| Clarity working | ☐ |
| No console errors | ☐ |
| Performance improved | ☐ |

---

## Rollback Plan

If issues found:
1. Re-enable deleted GTM tags
2. Remove page scripts
3. Publish previous GTM version
