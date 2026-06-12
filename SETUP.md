# ChoreQuest — Setup Guide

Follow these steps in order. Takes about 30 minutes total.

---

## Step 1 — Create your Supabase database (10 mins)

1. Go to **supabase.com** and click **Start for free**
2. Sign up with your email
3. Click **New project**
   - Name it: `chorequest`
   - Set a database password (save it somewhere safe)
   - Choose a region closest to you (e.g. West Europe)
   - Click **Create new project** — wait ~2 minutes
4. Once loaded, click **SQL Editor** in the left sidebar
5. Paste this into the editor and click **Run**:

```sql
create table app_state (
  id integer primary key,
  state jsonb not null,
  updated_at timestamptz default now()
);

insert into app_state (id, state) values (1, '{"children":[]}');

alter table app_state enable row level security;

create policy "Public read" on app_state for select using (true);
create policy "Public write" on app_state for all using (true);
```

6. Click **Project Settings** (cog icon, bottom left)
7. Click **API** in the settings menu
8. You'll see two values you need — copy them:
   - **Project URL** — looks like `https://abcdefgh.supabase.co`
   - **anon public key** — a long string starting with `eyJ...`

---

## Step 2 — Add your Supabase details to the app (2 mins)

Open `index.html` in a text editor (on Mac: right-click → Open With → TextEdit, then Format → Make Plain Text).

Find these two lines near the top of the `<script>` section:

```
const SUPABASE_URL = 'YOUR_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';
```

Replace them with your actual values, e.g.:

```
const SUPABASE_URL = 'https://abcdefgh.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

Save the file.

---

## Step 3 — Publish to Netlify (5 mins)

1. Go to **netlify.com** and sign up for free
2. Once logged in, look for a box that says **"drag and drop your site folder here"**
3. Open your file manager / Finder and find the `chore-tracker` folder
4. Drag the entire `chore-tracker` **folder** onto the Netlify page
5. Netlify will give you a URL like `https://cheerful-swan-abc123.netlify.app`
6. Click **Site settings → Change site name** to rename it to something like `chorequest-family`
   - Your URL becomes `https://chorequest-family.netlify.app`

---

## Step 4 — Add to home screen on iPhone (2 mins per phone)

Do this on both phones:

1. Open Safari (must be Safari on iPhone)
2. Go to your Netlify URL
3. Tap the **Share button** (box with arrow at bottom of screen)
4. Scroll down and tap **"Add to Home Screen"**
5. Name it `ChoreQuest` and tap **Add**

The app now appears on your home screen with its own icon, opens full screen, and works like a native app.

**On Android:**
1. Open Chrome
2. Go to your Netlify URL
3. Tap the three-dot menu → **"Add to Home screen"**

---

## Step 5 — Test sync

1. Open the app on one phone and tick a chore
2. Open the app on the other phone
3. Within a second or two it should update automatically

If it doesn't update instantly, a manual refresh will always show the latest state.

---

## Updating the app in future

If you want to make changes to the app:
1. Edit `index.html` locally
2. Go back to Netlify → your site → **Deploys**
3. Drag the updated `chore-tracker` folder onto the deploys area
4. Done — both phones update automatically within seconds

---

## Troubleshooting

**App shows "Loading ChoreQuest…" and doesn't load**
→ Check your SUPABASE_URL and SUPABASE_ANON_KEY are correct in index.html

**Changes on one phone don't appear on the other**
→ Try pulling down to refresh. If still not working, check Supabase is active (free projects pause after 1 week of inactivity — just log in to reactivate)

**Supabase free tier limits**
→ 500MB database, 2GB bandwidth/month. This app uses negligible amounts of both.
