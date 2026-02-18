# FOREVER: NEON — Book One

A speculative fiction series by Phatti MacHine.

## Overview

This is the complete web infrastructure for **FOREVER: NEON**, featuring:

- **Book One: The Awakening** — Free, fully published
- **Books 2-5** — Coming to Kindle (2024-2025)
- **Technician Portal** — Access-controlled reading system
- **KIDDs Electronics** — Narrative-integrated storefront (est. 1971)
- **Signal Archive** — Supplemental materials and lore

## File Structure

```
/
├── index.html                    # Homepage
├── styles.css                    # Master stylesheet
├── assets/
│   └── brand/
│       └── kidds-seal.svg       # Brand seal graphic
├── read/book-1/
│   ├── index.html               # Book landing page
│   └── chapter-01.html          # Chapter One
├── kidds/
│   ├── portal.html              # Technician login
│   ├── dashboard.html           # Dashboard
│   └── restricted.html          # Level 2 content
├── series/
│   └── index.html               # Series overview
├── signal/
│   └── index.html               # Archive index
└── store/
    └── index.html               # Merch store (placeholder)
```

## Deployment on Netlify

### Option 1: Drag & Drop
1. Download this entire folder to your computer
2. Go to your Netlify site dashboard
3. Find **Deploys** tab
4. Drag the folder into the deploy zone
5. Files go live instantly

### Option 2: GitHub Integration
1. Push this to your GitHub repo
2. Connect repo to Netlify
3. Netlify auto-deploys on every push

## Technician Portal Setup

The portal requires **Supabase** for authentication and email capture.

### Quick Start

1. Go to **https://supabase.com**
2. Create new project (name: `afterthegrid`)
3. In SQL Editor, paste:

```sql
CREATE TABLE technicians (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE NOT NULL,
  access_level INTEGER DEFAULT 1,
  created_at TIMESTAMP DEFAULT now()
);
```

4. Enable RLS on technicians table
5. Add policy:

```sql
CREATE POLICY "allow_public_insert"
ON technicians FOR INSERT
TO anon
WITH CHECK (true);
```

6. Get your API keys from **Project Settings → API**
7. Open `/kidds/portal.html`
8. Replace:
   ```javascript
   const SUPABASE_URL = "YOUR_SUPABASE_URL";
   const SUPABASE_KEY = "YOUR_ANON_PUBLIC_KEY";
   ```

9. Upload updated file to Netlify

### How It Works

- Users login at `/kidds/portal.html`
- Emails containing "1971", "kidds", or "phatti" get Level 2 access
- Everyone else gets Level 1 (can't access restricted node)
- Email records stored in Supabase

### Test It

1. Visit `/kidds/portal.html`
2. Enter any email → redirects to dashboard
3. Try `/kidds/restricted.html` → "Insufficient clearance"
4. Use email with "phatti" in it → Level 2 access

## Content Structure

### Chapter Organization

Each chapter is a standalone `.html` file in `/read/book-1/`

To add new chapters:
1. Copy `chapter-01.html`
2. Update `<h1>` and content
3. Update links in `/read/book-1/index.html`

### Styling

All styling is in `styles.css`. Global color system:

```css
--bg-dark: #0f1110        (Main background)
--accent: #4f6b4f         (Muted green)
--phosphor: #ff7a00       (Orange alerts)
```

## Future Expansions

### Technician ID Badges
Generate downloadable PDFs after login with:
- Seal graphic
- Unique ID number
- Authorization date

### Email Notifications
Track signups and send:
- Book 2 release notices
- Archive updates
- Restricted access elevation

### Signal Archive
Add multimedia content:
- Audio transmissions
- Animated glitch loops
- Faux documentary clips

### Merchandise Integration
Connect to Stripe for direct sales:
- T-shirts
- Mugs
- Prints
- Field manuals

## Security Notes

**DO:**
- Keep Supabase keys in `/kidds/portal.html` only
- Use `.gitignore` if you push to GitHub
- Monitor Supabase for suspicious activity

**DO NOT:**
- Commit service role key to GitHub
- Share anon keys publicly
- Use weak RLS policies

## Author

**Phatti MacHine**

- FOREVER: NEON — Speculative Fiction Series
- KIDDs Electronics — Est. 1971

## License

© Phatti MacHine. All rights reserved.

---

## Support

For questions or issues:
1. Check the SETUP_GUIDE.md
2. Review Supabase documentation
3. Verify file paths and permissions

---

**Status:** Archive Build 1.0
**Last Updated:** 2026-02-18
**Deployment:** Netlify + Supabase
