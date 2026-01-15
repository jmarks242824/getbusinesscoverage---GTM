# Facebook Pixel Lead Submit (Template)

---

## Basic Info

| Property | Value |
|----------|-------|
| Tag Name | Facebook Pixel Lead Submit |
| Tag ID | 44 |
| Type | Community Template (cvt_5RM3Q) |

---

## Trigger

| Property | Value |
|----------|-------|
| Fires On | Lead Submit (Click) |
| Trigger Type | Click |
| Trigger ID | 26 |

---

## Configuration

| Property | Value |
|----------|-------|
| Pixel ID | 1421331895722314 |
| Event | Purchase |
| Advanced Matching | false |

---

## ⚠️ PROBLEMS

### Problem 1: Wrong Pixel ID

| Current | Should Be |
|---------|-----------|
| 1421331895722314 | 1044730294266508 |

### Problem 2: Wrong Event Type

| Current | Should Be |
|---------|-----------|
| Purchase | Lead |

### Problem 3: No Advanced Matching

User data not being sent.

---

## Action

| Decision | Reason |
|----------|--------|
| ❌ DELETE | Wrong pixel ID, wrong event |

---

## Alternative

Use "BH Media - Lead Submit" tag instead (correct pixel).
