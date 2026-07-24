# Publishing Obsidian Line on GitHub Pages

Everything in this folder is the finished site. Free hosting, HTTPS included, no build step.

---

## Before you start — add your form key

The contact form won't send until you do this. Two minutes.

1. Go to **web3forms.com** and enter `contact@obsidianlinestudio.in`. No account needed — they email you an access key.
2. Open `index.html` in Notepad (right-click → Open with → Notepad).
3. Press `Ctrl+F`, search for `YOUR_WEB3FORMS_ACCESS_KEY`.
4. Replace just that text with your key, keeping the quote marks:

   ```html
   <input type="hidden" name="access_key" value="a1b2c3d4-your-real-key-here">
   ```
5. Save.

Do this now — it's much easier before uploading than after.

---

## Step 1 — Sign in

You're `github.com/agarwaltech`, so nothing to set up here. Just be signed in.

Your site will land at **https://agarwaltech.github.io** — that address is fixed by your username and can't be changed without changing the username itself. If you'd rather the URL read as the brand, that's what Step 5 (the custom domain) is for.

---

## Step 2 — Create the repository

This is the step people get wrong, so read it twice.

1. Click the **+** in the top right → **New repository**.
2. **Repository name** must be exactly your username followed by `.github.io`.

   Your username is `agarwaltech`, so the repo name is `agarwaltech.github.io`. Nothing else. Not `obsidian-line-site`, not `my-website`.

   > This exact naming is what makes GitHub serve it as your main site at `https://agarwaltech.github.io` rather than tucking it in a subfolder.
3. Set it to **Public**. Private repos can't use free Pages.
4. Leave "Add a README file" **unticked**.
5. Click **Create repository**.

---

## Step 3 — Upload the files

On the empty repo page, click **uploading an existing file**.

Now open this folder on your computer and select:

- `index.html`
- the whole `assets` folder
- `robots.txt`
- `sitemap.xml`

Drag all of them into the browser window together. Wait for the assets folder to finish expanding — you should see 5 image files listed under it.

Scroll down, type `Initial site` in the commit box, click **Commit changes**.

**Don't upload `DEPLOY.md`** (this file) — it's just notes for you.

> `.nojekyll` is a hidden file your file explorer probably won't show, and you don't need it. Skip it.

---

## Step 4 — Switch on Pages

1. In your repo, click **Settings** (top row of tabs).
2. Left sidebar → **Pages**.
3. Under "Build and deployment":
   - **Source**: Deploy from a branch
   - **Branch**: `main`, folder `/ (root)`
4. Click **Save**.

Wait 1–3 minutes. Refresh the page and a green banner appears with your live link:

```
https://agarwaltech.github.io
```

Open it. The intro should play.

> Seeing a plain README or a 404? Your `index.html` probably landed inside a subfolder instead of at the top level. Go to the repo's Code tab — you should see `index.html` and `assets` right there, not nested inside another folder.

---

## Step 5 — Point your domain at it

Skip this if you don't own `obsidianlinestudio.in` yet. The `.github.io` address works fine on its own.

### 5a. Tell GitHub about the domain

Settings → Pages → **Custom domain** → type `obsidianlinestudio.in` → **Save**.

Do this *before* touching your DNS. Doing it the other way round leaves a window where someone else could claim your subdomain.

### 5b. Add the DNS records

Log in wherever you bought the domain (GoDaddy, Namecheap, Hostinger, BigRock…) and find **DNS Management**.

Add four **A records**, all with name `@`:

| Type | Name | Value           |
| ---- | ---- | --------------- |
| A    | @    | 185.199.108.153 |
| A    | @    | 185.199.109.153 |
| A    | @    | 185.199.110.153 |
| A    | @    | 185.199.111.153 |

Then one **CNAME record** so `www` works too:

| Type  | Name | Value                    |
| ----- | ---- | ------------------------ |
| CNAME | www  | agarwaltech.github.io  |

Note the CNAME value has **no** `https://` and **no** repo name on the end.

If your provider pre-filled a parking A record pointing somewhere else, delete it.

### 5c. Force HTTPS

DNS can take anywhere from 10 minutes to 24 hours to spread. Once it has, go back to Settings → Pages and tick **Enforce HTTPS**. The tickbox stays greyed out until GitHub has issued your certificate, which is normal — check back later rather than assuming something broke.

---

## Updating the site later

1. Repo → **Code** tab → click `index.html`.
2. Click the pencil icon.
3. Edit, scroll down, **Commit changes**.

Live in about a minute. For images: click into `assets`, then **Add file → Upload files**.

---

## What to change first

Once it's live, these are the things still carrying placeholder content:

| Where | What |
| ----- | ---- |
| Work section | Six tiles with invented film titles — swap for your real releases and link them to YouTube |
| Stats | "40+ films", "2M+ views", "12 worlds" — set these to your actual numbers |
| Process | Written as four generic phases; rewrite in your own words |
| Studio cards | The three pillar descriptions are my draft, not your voice |
| Social links | There are none yet — your YouTube channel should be in the nav and footer |

The stats especially — invented numbers on a real business site are worth fixing before you share the link anywhere.

---

## Two settings you might want

Near the top of the `<script>` block in `index.html`:

```js
var REPLAY = true;   // intro plays on every page load
var TOTAL  = 6900;   // intro length in milliseconds
```

Set `REPLAY = false` and the intro plays once per browser session, so someone returning to check your email address doesn't sit through seven seconds of animation again. Worth considering — a cinematic intro is great the first time and tiresome the fifth.
