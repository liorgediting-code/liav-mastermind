# Design Spec: Webinar Transformation + Registration Page

**Date:** 2026-04-20  
**Topic:** Transform landing page from in-person event → free Google Meet webinar; add registration page with webhook; add calendar links.

---

## Context

The existing `index.html` is a Hebrew RTL single-file landing page for a paid (97₪) in-person sales mastermind event. This spec covers transforming it into a free online webinar with a separate registration flow.

---

## New Event Details

| Field | Value |
|---|---|
| Date | 12.05.2026 (Tuesday) |
| Time | 19:00 |
| Duration | 90 minutes |
| Format | Google Meet (online) |
| Price | Free |
| Spots | 100 (fake scarcity) |
| Hosts | ליאב (sales), ליאור |
| Language | Hebrew RTL |

---

## What We Learn (updated, sales-only)

1. **תבנית מכירה נכונה + 8 השלבים** — the complete framework that closes deals
2. **מכירה על ידי השלילה** — getting the customer to reach the answer themselves (the "כמו עכשיו" technique)

Marketing content (ליאור's "הכפלת לידים מהממומן") is removed entirely.

---

## Part 1: `index.html` Changes

### Spots Bar
- New copy: `🔥 100 מקומות בלבד — אם אתה רואה את הדף הזה, עדיין יש מקום`

### Hero
- Badge: `וובינר חינמי · 12.05 · Google Meet`
- H1: `למה אנשים לא קונים ממך — ואיך לשנות את זה ב-90 דקות`
- Subtext: `רוב בעלי העסקים מוכרים לא נכון. לא בגלל שהם לא טובים — בגלל שאף אחד לא לימד אותם את 8 השלבים. בוא ללמוד אותם חינם, מהסלון שלך.`
- Event meta items: 📅 12.05.2026 | 🕑 19:00 | 💻 Google Meet | 👥 100 מקומות | ⏱️ 90 דקות | 🎁 חינם לגמרי
- CTA: links to `register.html` (not external payment link)

### Problem Section
- Keep all 3 bullets, tighten copy slightly
- CTA links to `register.html`

### Testimonials
- No changes

### Agitate Section
- Keep all 3 bubbles
- Pivot line updated: `...מגיע לוובינר, 90 דקות מהסלון שלך, בחינם — ויוצא עם תכנית פעולה שמיישמים מחר בבוקר.`

### What Is It (3 cards, rewritten)
- 🎯 **סדנה חיה עם ליאב** — תוכן מכירות שמיישמים מחר בבוקר
- 💻 **מהנוחות של הבית** — בלי נסיעות, בלי חניה, ב-Google Meet
- 🎁 **חומרים מוכנים** — תבנית מכירה + 8 השלבים — שלך לתמיד

### What We Learn (2 items, replacing 3)
1. תבנית מכירה נכונה ו-8 השלבים למכירה
2. איך למכור על ידי דרך השלילה

### Benefits (updated)
- 🔥 תבנית מכירה מוכנה לשימוש מהיום הראשון
- 💻 ללא נסיעות — 90 דקות מהסלון שלך
- 📋 תכנית פעולה ברורה לשיחת המכירה הבאה שלך

### About Section
- Keep ליאב card as-is (5M+ ₪ stat, 1.5 years coaching stat)
- Keep ליאור card but remove paid-ads/marketing references from description and stats. New description: "שנתיים בניהול מכירות ועסקים. מביא את הפרספקטיבה האסטרטגית שגורמת ללקוחות לבוא אליך."

### Price Section
- "חינם" replaces 97₪ animated number
- Price includes list: סדנה חיה עם ליאב | 2 טכניקות מכירה מוכחות | תבנית מוכנה לשימוש | 90 דקות אינטנסיביות
- Remove food/drink item
- CTA → `register.html`

### FAQ (updated questions)
- מתאים לי גם אם אני בתחילת הדרך? → keep answer
- למה חינם? → replaces "למה רק 97₪?" — answer: "המטרה היא למלא את החדר באנשים הנכונים"
- מה צריך כדי להשתתף? → Google Meet (חינמי), קישור ישלח לפני האירוע
- מה המבנה? → 19:00, 90 דקות, Google Meet
- מה אם אני לא יכול להגיע? → "ההרשמה חינמית, אין צורך לבטל"
- שאלות נוספות? → keep WhatsApp link

### Legal Section
- **Remove entirely** — free event, no consumer protection law obligation

### Footer
- Update: date 12.05.2026, remove physical address, keep email + WhatsApp

---

## Part 2: `register.html` (new file)

Same design system as `index.html` — import same Google Font, use identical CSS variables. No navigation bar.

### Structure

**Header block:**
- Badge: `וובינר חינמי · ליאב · 12.05.2026 · 19:00`
- H1: `שמור את המקום שלך`
- Subtext: `90 דקות שישנו את כל שיחות המכירה שלך. חינם לגמרי.`
- Scarcity line: `⚡ נותרו מקומות — הירשם עכשיו לפני שייגמרו`

**Form (3 fields):**
- שם מלא (required, text)
- מספר טלפון (required, tel)
- אימייל (required, email)

**Submit button:** `אני רוצה את המקום שלי →` (full orange CTA style)

**States:**
- Default: form visible
- Loading: button shows spinner + "שולח...", form inputs disabled
- Success: form hidden, replaced by success block (see below)
- Error: inline error message below button, "משהו השתבש — נסה שוב", form re-enabled

**Success block:**
```
✅ נרשמת בהצלחה!
קישור Google Meet ישלח אליך לפני האירוע.

[הוסף ליומן Google] [הורד קובץ iCal (.ics)]
```

**Calendar links:**
- Google Calendar: pre-filled URL with event name, date (2026-05-12 19:00 Asia/Jerusalem), duration 90min, description with Google Meet note
- iCal: downloadable `.ics` file served as a data URI (no server needed), same event details

### Webhook Integration

```js
const WEBHOOK_URL = 'PASTE_YOUR_WEBHOOK_URL_HERE'; // Make.com or Google Apps Script

fetch(WEBHOOK_URL, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name, phone, email, timestamp: new Date().toISOString() })
})
```

The webhook receives JSON: `{ name, phone, email, timestamp }`.

**Make.com setup:** Create scenario → Webhook trigger → parse JSON → send email / add to Google Sheet.  
**Google Apps Script setup:** `doPost(e)` function reads `e.postData.contents`, parse JSON, append to Sheet.

---

## Scarcity Mechanism

The "fake scarcity" works at the file level:
- `index.html` is live = spots available
- When full: replace `register.html` with a static "המקומות אזלו" page
- No JS or backend needed

---

## Files Changed

| File | Action |
|---|---|
| `index.html` | Modify in-place — all sections updated |
| `register.html` | Create new |

No new dependencies, no build step, no framework.
