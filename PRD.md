# NomadMap PRD — Feature Parity with Nomad List

## Goal
Functionally equivalent to Nomad List (nomads.com) — same core capabilities, distinct visual identity.

## Success Criteria
A nomad can: discover cities with rich data, log their travels, build a profile, see the community, and use discovery tools — all in NomadMap without needing Nomad List.

## Anti-Goals (explicitly out of scope)
- Chat / messaging
- Dating app
- Meetups / events
- Friend finder
- NomadGPT / AI assistant
- FIRE calculator
- Photo voting

---

## Sprint 1 — Richer City Data + Discovery Tools
**Goal:** City profiles match Nomad List's depth. Discovery tools give users more ways to find the right place.

### New data fields per city (15+)
- `rent` — 1BR monthly rent in USD
- `beer` — beer price USD
- `food` — monthly food budget USD
- `airQ` — air quality score 1-10 (10=cleanest)
- `humidity` — avg humidity %
- `walk` — walkability 1-10
- `womenSafe` — female safety 1-10
- `lgbtq` — LGBTQ+ friendliness 1-10
- `english` — english proficiency 1-10
- `cowork` — coworking availability 1-10
- `healthcare` — healthcare quality 1-10
- `pros` — array of 3 curated pros
- `cons` — array of 2 curated cons
- `bestMonths` — array of best months [1-12]
- `avgStay` — average nomad trip length in days

### Sort dimensions (expand from 6 → 15+)
Add: Rent, Beer Price, Walkability, Female Safety, LGBTQ+, English, Coworking, Healthcare, Air Quality

### Detail modal upgrades
- Cost breakdown (total vs rent vs food vs beer)
- Add all new metric bars
- Pros ✅ / Cons ❌ section
- Best months to visit (calendar chips)
- "Avg nomad stay: X days"

### Discovery tools
- **Climate finder** sidebar: filter by temperature range
- **Random good city** button (score ≥ 7)
- **Compare** button: select 2 cities → side-by-side table overlay
- **Trending** tab: cities sorted by recent saves (last 7 days from Supabase)

---

## Sprint 2 — Community
**Goal:** Users have identities. Nomad presence is visible on the map.

### New Supabase tables
```sql
-- nomad_profiles
CREATE TABLE nomad_profiles (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id uuid REFERENCES auth.users NOT NULL UNIQUE,
  username text,
  bio text,
  home_country text,
  current_city_id text,
  remote_work_type text, -- 'freelance'|'employee'|'founder'|'other'
  avatar_color text,
  created_at timestamptz DEFAULT now()
);

-- trips
CREATE TABLE trips (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id uuid REFERENCES auth.users NOT NULL,
  city_id text NOT NULL,
  city_name text,
  start_date date,
  end_date date,
  notes text,
  created_at timestamptz DEFAULT now()
);
```

### Features
- **Profile page** (sidebar tab "Profile"): edit username, bio, home country, remote type, current city
- **Trips log** (sidebar tab or profile): log past + current trips with dates; sortable timeline
- **Members map** layer: show dots of users whose `current_city_id` is set; click dot → mini profile
- **"Nomads here"** counter on city cards and in detail modal (from nomad_profiles.current_city_id)
- **Profile completion nudge** post sign-up

---

## Sprint 3 — Polish + SEO
- Permalink/shareable URLs per city (hash-based: `#city/bali`)
- Page title updates dynamically when city modal opens
- Meta description per city for SEO
- Keyboard navigation (arrow keys through city list)
- Mobile gestures (swipe to close modal)
- Loading states and error states for all Supabase calls
- Performance: lazy-load map markers for off-screen cities
