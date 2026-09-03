# Publishing Chronicles #89 on GitHub Pages

## Read this first

GitHub Pages is **public hosting**. A Pages site is reachable by anyone who has the URL,
even when the repository itself is private. There is no password on it.

Real access control on Pages exists only on **GitHub Enterprise Cloud**, where an org can
publish a site privately so that only people with repo read access can open it.

So what this setup gives you is **unlisted, not private**: search engines are told to stay
away, the URL is unguessable, and nothing links to it. Fine for an internal newsletter with
staff holiday photos. Not fine for anything confidential.

Also note: publishing Pages from a **private** repo needs GitHub **Pro / Team / Enterprise**.
On a Free account the repo must be public for Pages to build — the HTML would then be
readable in the repo as well as on the site.

## What's in this folder

| File | Why |
|---|---|
| `summer-c9ba2cf9/index.html` | The issue, at an unguessable folder name |
| `index.html` | Blank "Nothing here." page, so the site root gives nothing away |
| `404.html` | Same, for wrong guesses |
| `robots.txt` | `Disallow: /` for every crawler |
| `.nojekyll` | Stops GitHub's Jekyll build interfering |

The issue's `<head>` also carries `noindex, nofollow, noarchive, nosnippet, noimageindex`
plus `referrer: no-referrer`, so the URL isn't leaked in referrer headers when someone
clicks a link out of the page.

## Steps

1. **Create the repository** — github.com > New repository.
   - Name: something dull, e.g. `internal-notes`
   - Visibility: **Private** (needs Pro/Team for Pages; if you're on Free, see the note above)
   - Don't add a README

2. **Push this folder** (from a terminal, in this folder):

   ```
   git init -b main
   git add .
   git commit -m "Add Chronicles 89"
   git remote add origin https://github.com/<your-account>/internal-notes.git
   git push -u origin main
   ```

   If it asks for a password, use a personal access token, not your account password:
   github.com > Settings > Developer settings > Personal access tokens.

3. **Turn on Pages** — repo > Settings > Pages.
   - Source: **Deploy from a branch**
   - Branch: `main`, folder: `/ (root)`
   - Save. First build takes a minute or two.

4. **Your URL** will be:

   ```
   https://<your-account>.github.io/internal-notes/summer-c9ba2cf9/
   ```

   (the folder name in this directory is the real one — copy it exactly)

5. **Check it before sharing**: open the URL, then open
   `https://<your-account>.github.io/internal-notes/` and confirm it says "Nothing here."

## Keeping it out of search

- Never post the URL anywhere public — no tweets, no public Slack/Discord, no public repos.
- Don't submit it to Google Search Console.
- The `noindex` tag is the thing that actually keeps it out of results. `robots.txt` stops
  polite crawlers reading it at all.
- If it ever does get indexed, use Google's "Remove outdated content" tool, then keep the
  `noindex` in place.

## If you want it genuinely private instead

- **Confluence** (what you already have): authenticated by default, and comments work. Best option.
- **Cloudflare Pages + Cloudflare Access**: free for up to 50 users, gives real email-code
  login in front of the same HTML file. Best free option if you want it outside Confluence.
- **GitHub Enterprise Cloud**: private Pages, restricted to people with repo access.
