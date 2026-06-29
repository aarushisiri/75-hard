====================================
  75 Hard Tracker — Setup Guide
====================================

STEP 1: HOST ON GITHUB PAGES
------------------------------
1. Go to github.com → sign in → click "+" → New repository
2. Name it: 75-hard  (must be public)
3. Upload ALL files from this folder:
     index.html, manifest.json, sw.js, icon-192.png, icon-512.png
4. Go to Settings → Pages → Source: "main" branch → Save
5. Your URL: https://YOUR-USERNAME.github.io/75-hard/

Share this URL with your friends — everyone opens the same URL.


STEP 2: SET UP THE LEADERBOARD (free, ~5 mins)
------------------------------------------------
The leaderboard uses Supabase (free Postgres database).

1. Go to https://supabase.com → Sign up (free)

2. Click "New Project" → pick a name → set a password → Create

3. Once ready, go to the SQL Editor and run this query:

   CREATE TABLE hard75_leaderboard (
     id TEXT PRIMARY KEY,
     name TEXT NOT NULL,
     days_complete INTEGER DEFAULT 0,
     streak INTEGER DEFAULT 0,
     updated_at TIMESTAMPTZ DEFAULT NOW()
   );

   ALTER TABLE hard75_leaderboard ENABLE ROW LEVEL SECURITY;

   CREATE POLICY "Anyone can read" ON hard75_leaderboard
     FOR SELECT USING (true);

   CREATE POLICY "Users insert own row" ON hard75_leaderboard
     FOR INSERT WITH CHECK (true);

   CREATE POLICY "Users update own row" ON hard75_leaderboard
     FOR UPDATE USING (true);

4. Go to Settings → API. Copy:
   - Project URL  (looks like https://xxxx.supabase.co)
   - anon public key  (long string starting with eyJ...)

5. Open index.html in any text editor (Notepad is fine).
   Find these two lines near the top of the <script> tag:

     const SUPABASE_URL = 'YOUR_SUPABASE_URL';
     const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';

   Replace with your actual values:

     const SUPABASE_URL = 'https://xxxx.supabase.co';
     const SUPABASE_ANON_KEY = 'eyJhbGciOi...';

6. Save index.html and re-upload it to GitHub Pages.

7. Done! Everyone who visits the site will appear on the leaderboard
   when they log their first day.


INSTALL AS AN APP ON iPHONE
-----------------------------
1. Open your GitHub Pages URL in Safari on iPhone
2. Tap the Share button (box with arrow at bottom)
3. Tap "Add to Home Screen"
4. Tap "Add"
The app now lives on your home screen. Works offline.


HOW THE LEADERBOARD WORKS
---------------------------
- Each person enters their name when they first open the app
- Progress syncs to the leaderboard automatically when they save a day
- If someone misses a task, their streak resets per the 75 Hard rules
- The leaderboard ranks by days complete, then by streak
- Each person's data is private — only their name, days complete,
  and streak are shared publicly

====================================
