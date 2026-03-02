# NomadMap — Full Feature Upgrade

Free alternative to Nomad List (nomadlist.com). Currently at https://nomads.antmachine.com

## Current State
Single HTML file (431 lines). Has: map view, list view, 58 cities with scores, filters, search, tags.

## What to Build: Full-Featured Nomad Platform

### Supabase Config
- URL: https://grxvqkaokerjrsklqubr.supabase.co
- Anon Key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdyeHZxa2Fva2VyanJza2xxdWJyIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzI0Mzg4OTAsImV4cCI6MjA4ODAxNDg5MH0.UYg_98cYSfxV9egaoDyuM1LIntTn_qzEvBm5xUUVw6A
- Load SDK: <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

### Database Tables (already created)
- **city_reviews**: id, user_id, city_id, city_name, rating(1-5), review, cost_actual, internet_actual, visited_at
- **saved_cities**: id, user_id, city_id, city_name, status('wishlist'|'visited'|'living')
- **city_tips**: id, user_id, city_id, tip, category('general'|'food'|'coworking'|'housing'|'transport'|'safety'), likes
- **nomad_profiles**: id, user_id, username, bio, current_city, home_country, remote_work

### Features to Add (keep all existing map/list/filter features)

#### 1. Auth (email/password via Supabase)
- Clean modal: Sign Up / Sign In tabs
- Top bar: show avatar initial + username when logged in, "Join Free" button when not
- Dark theme modal, red (#ff2d2d) accent

#### 2. City Detail Panel (slide-in from right when city clicked)
- City name, flag, country, region
- Overall score + metric bars (cost, internet, safety, weather, fun)
- Tags
- Visa info
- **Community Reviews**: list of user reviews with star ratings
- **Add Review**: form with rating (1-5 stars), text review, actual cost/internet they experienced, when visited
- **Tips**: list of tips by category (food, coworking, housing, transport, safety)
- **Add Tip**: textarea + category dropdown
- **Save buttons**: "Wishlist ♡" / "Visited ✓" / "Living 🏠" — toggle states

#### 3. My Cities Panel (sidebar tab)
- Tabs: Wishlist | Visited | Living Here
- List of saved cities per tab with remove button
- Only visible when logged in

#### 4. Community Reviews on Map Markers
- Marker size scales with number of reviews (more reviewed = bigger marker)
- Color still based on score

#### 5. Leaderboard / Most Popular Cities
- Section showing top 10 most saved cities by community
- Updated from saved_cities table

### UI Layout
- Keep existing: dark bg, red accent, left sidebar (filters + city list), right map
- Add to topbar: user menu on right side
- Add city detail: slide-in panel over map (300px wide, full height, right side)
- Add bottom of sidebar: "My Cities" tab toggle

### Design Rules
- Dark (#080808 bg, #0f0f0f surface, #1a1a1a border)
- Red accent: #ff2d2d
- Green score: #2ecc71, Yellow: #f39c12, Red score: #e74c3c
- Smooth transitions on panel open/close
- Mobile responsive

### Everything in one index.html file
No build step. CDN only. Pure HTML/CSS/JS.

When done:
1. git add -A && git commit -m "feat: full community features with Supabase auth"
2. git push origin main
