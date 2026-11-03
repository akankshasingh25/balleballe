# BalleBalleTheWeeding.com

A wedding website for Akanksha & Prateek — Puri, 8 & 9 December 2026.

No build step. No npm. No framework. A handful of files and a folder of pictures.
Open `index.html` in a browser and it works; upload the folder and it's live.

---

## Editing it

Almost everything you'll ever want to change lives in **two files**:

| File | What's in it |
|---|---|
| `data/config.js` | Names, dates, events, venues, dress codes, hotels, travel, gift note, contact |
| `data/posts.js`  | The feed — your blog posts |

Change a value, save, refresh. That's the whole workflow.

### The «guillemets»

Anything still wrapped in `«...»` is a placeholder. On the live site it shows up
**highlighted in gold**, so you can't miss it. Delete the `«` and `»` and the
highlight disappears.

What's already real: both your names, all four events (dates, times, venues,
optional dress codes), your email and WhatsApp number (see below — both are
scrambled), both hotels, the airport and the station.

What's still marked unfinished — every one shows up highlighted in gold on the
live site. All of them are in `data/config.js`:

**The four Google links** — see [GOOGLE-SETUP.md](GOOGLE-SETUP.md):

- `collect.itinerary`, `collect.photos`, `collect.quiz`
- `gifts.sheetUrl` — the registry sheet

**The rest:**

- `games.quiz.questions` — four of the ten are templates. Write the question,
  the four options, and point `answer` at the right one.
- `travel.air.note` — «by early morning on 8 December», if that's the advice
  you want to give.
- `photoPlace` for the Vicco Turmeric painting — is Lagos the one in Portugal?
- `games.photos.albumUrl` — optional, a shared album for everything that isn't
  one of the ten.

## The gift registry

The page is live and linked from the nav. **The quickest way to finish it is a
Google Sheet**: make one, share it as "anyone with the link can view", and paste
the link into `gifts.sheetUrl`. See [GOOGLE-SETUP.md](GOOGLE-SETUP.md).

It reads everything from the `gifts` block in `data/config.js`:

- `ways` — the three ways to give. Reword them, or delete one.
- `sheetUrl` — your sheet. Paste the "anyone with the link can view" URL and the
  **Open the full sheet** button appears. This is the quickest way to finish the
  page.
- `items` — optional rows shown on the page itself. Each takes `name`, and
  optionally `price`, `note` and `url`. It ships empty.
- `brands` — the gift-card shops, shown as chips. Empty hides the whole section.

The page works out which of those you've filled in:

| What's in config | What the page shows |
|---|---|
| Nothing yet | "We are still writing it down" |
| `sheetUrl` only | A gold **Open the full sheet** button |
| `items` only | The rows |
| Both | The rows, with the sheet as a second button under them |

## The email address and the phone number

Both are shown on the site, and neither is harvestable by the usual means.

They are stored **scrambled** in `data/config.js`:

```js
email:    "4b414b444159424b5943444d42181f474b5849426a4d474b434604494547",
whatsapp: "01131b0a1d191f181d181f1b1a1a",
```

and — this is the part that matters — they are **not written into the page
when it loads**. A contact link shows a label ("Email us", "WhatsApp") with no
`href` at all. Only when somebody reaches for it — pointer down, a tap, or
keyboard focus — does `site.js` unscramble the value, put it in the `href` and
show it as text.

Addresses get harvested by running a regular expression over page source,
looking for an `@` or a long run of digits. Neither exists here: not in the
HTML, not in `config.js`, not in this public repo. A person gets the address on
the first click, as normal; a scraper gets the word "email".

It is a speed bump, not a lock. Anyone who opens the console and types
`BB.unmask(WEDDING.contact.email)` has it. That's fine — the point is to not be
the easy one.

**To change either**, open the site, press F12 for the console, and run:

```js
BB.mask("new@address.com")
```

Paste what it prints into `config.js`. `BB.unmask("...")` goes the other way, so
you can always check what's in there.

Marked up as `<a data-contact="email">` or `<a data-contact="whatsapp">`. Add
`data-contact-label` to keep your own wording on the link (the home page's
buttons do this) instead of swapping it for the address.

## The missions

`missions.html` holds two missions — ten photographs to find, and ten questions
about the two of you. **Both are forms**, so the answers come back to you.
Everything on the page comes from the `games` block in `data/config.js`.

### Switching them on and off

```js
live: true,                               // up now, linked in the nav
liveFrom:  "2026-12-08T09:00:00+05:30",   // used when live is "auto"
liveUntil: "2026-12-10T12:00:00+05:30",   // delete this and they stay up
```

`live: true` is the current setting — the page is public and "Missions" is in
the nav. `live: "auto"` hides it outside the window above; `live: false` hides
it always. Whatever it says, **`missions.html?preview` always opens the page for
you**, with a banner reminding you guests can't see it.

> This is a static site, so hidden means hidden, not locked, and the quiz
> answers sit in `data/config.js` where anyone can read them. For a wedding game
> that's the right trade — it isn't an exam.

### Mission one — the photographs

The ten prompts live in `games.photos.prompts`. Reword, reorder or cut them
freely; the page follows whatever is there.

They go to a **Google Form with one file-upload question per prompt**, which is
the only arrangement that gives you a folder per photograph:

```
Balle Balle — ten photographs (File responses)/
  Someone you have only ever met in a family WhatsApp group/
  A pair of hands straight after Daler Mehandi…/
  …
```

Open a folder, see everyone's attempt at that one prompt side by side. Set
`collect.photos` and the attach buttons on the page are replaced by a button
through to the form. **The ten prompts stay listed on your page either way** —
people still need to know what they are looking for.

Two things to tell guests, both on the form's description:

- **File uploads require a Google account.** There is no way round it, and it is
  the price of the folders.
- Uploads land in **your** Drive and count against your 15 GB.

Until `collect.photos` is set, the page shows its own upload form. That one
posts to Netlify, which caps a whole submission at 8 MB and takes one file per
field — so it manages two or three photographs at a time and warns you when
you're over. Fine for a look; not the real thing.

### Mission two — the quiz

Ten questions, marked in the browser the moment it's sent so the guest gets
their score straight away, and posted to you with the score and every answer
written out in words (not option numbers).

**Four of the ten are still templates.** A question with «guillemets» left in it
is one you haven't written, so guests don't see it at all — right now the quiz
shows them the six finished ones and scores out of six. Open
`missions.html?preview` and you see all ten, gold marks and all, so you can tell
what is still outstanding. Fill one in and it appears for everyone, keeping its
original slot, so answers you've already collected stay lined up.

### If you add or remove a prompt or a question

On the website itself: nothing. Both missions are drawn from `config.js`, so
adding a prompt adds a row and adding a question adds a question.

Two places to keep in step:

- **Your Google Form.** Add the matching file-upload question (titled the same
  as the prompt) or multiple-choice question. The site doesn't do this for you.
- **`netlify-forms.html`**, only if you're still on the fallback forms. It
  declares the field names Netlify would otherwise not see (`shot_01…`,
  `q0…`). Once the Google links are in, it does nothing.

## The feed

Removed. `feed.html` is gone and nothing loads `data/posts.js`. The post file is
still there with your posts in it, so bringing it back is a small job rather than
a rewrite — ask and I'll put the page back.

## Changing the events

Edit the `events` array in `data/config.js` — that's the only place. Add an entry
and a new panel appears on the events page, a new card on the home page and a new
entry in the jump bar. Delete one and it all disappears again. `slug` is just
the anchor in the URL (`events.html#sangeet`), so keep it lowercase and hyphen-free.

`city` is optional — all four events are in Puri and the page already says so, so
it's left out and the "Where" row shows the venue alone.

Panels alternate sides automatically — odd ones put the painting on the left,
even ones on the right.

### Artwork and colours

Each event carries three things in `config.js`:

- `art` — its postcard, lifted out of `Weddin Invite Postcards.pdf`
- `photo`, `photoPlace`, `photoNote` — the photograph it was painted from
- `sky` — a palette name, defined in `assets/css/style.css` under `[data-sky]`

The palettes were sampled pixel-by-pixel from the paintings themselves, and they
drive the text colour and the wash that keeps words readable over the picture.
To retheme an event, change the five colours on its `[data-sky]` line.

The site used to draw its own Van Gogh skies in SVG, because the real paintings
weren't available yet. They are now, so that engine is gone.

---

## Deploying

The site is plain files — 34 of them, 22 MB, 19 MB of which is
`save-the-date.mp4`. There is no build step and, once the Google links are in,
**nothing posts to a server**. So any free static host will do, and you should
pick the one with the most generous bandwidth, because of the video.

### Cloudflare Pages — what this is set up for

The repo is **github.com/akankshasingh25/balleballe**, private. Cloudflare Pages is
free, serves static files with unmetered bandwidth, and — unlike GitHub Pages
on a free account — deploys from a private repo, so nothing about the wedding
becomes searchable.

**One-time setup:**

1. [dash.cloudflare.com](https://dash.cloudflare.com) → sign up (free, no card).
2. **Compute (Workers & Pages) → Create → Pages → Connect to Git.**
3. Authorise GitHub. When it asks which repositories, pick **balleballe** —
   it has to be granted explicitly because the repo is private.
4. Framework preset: **None**. Build command: **leave empty**. Build output
   directory: **`/`**. Save and Deploy.

It goes live at `balleballe.pages.dev` in about a minute. Every `git push` to
`main` redeploys it.

**The custom domain**, once you're happy with it: Pages project → **Custom
domains → Set up a domain** → `balleballetheweeding.com`. For a bare domain
Cloudflare needs your nameservers pointed at it — it walks you through that at
your registrar. HTTPS provisions itself.

**Two Cloudflare limits worth knowing:** 500 builds a month (you'll use a
handful) and **25 MiB per file**. `save-the-date.mp4` is 19.4 MiB, so it fits,
but a longer cut would not.

`_headers` and `_redirects` in the repo root are the Cloudflare equivalents of
`netlify.toml` — caching rules and the two redirects from the retired
`photos.html` / `quiz.html`. All three files are kept so the site behaves the
same wherever it lands; if you change one, change the others.

### Netlify — also fine

Same shape: **Add new site → Import an existing project**, build command empty,
publish directory `.` Or, for a first look with no accounts at all, drag the
folder onto [app.netlify.com/drop](https://app.netlify.com/drop) and it is live
in a minute.

Worth knowing if you go this way: since April 2026 Netlify's free plan runs on a
monthly credit pool rather than fixed quotas, and **when the credits run out the
site pauses until the next billing month** — there is no overage to absorb a
spike. Sources disagree on how far the free pool stretches for bandwidth, so
check their current pricing page. With a 19 MB video on the home page that is a
real consideration; on Cloudflare it isn't.

`netlify.toml` is ignored by every other host, so it does no harm wherever you
land.

### GitHub Pages, Vercel, or any old web host

All work. Upload the folder. Nothing needs configuring.

### About that video

22 MB of the site's 22 MB is basically `save-the-date.mp4`. It is set to
`preload="none"`, so it costs nothing until somebody presses play — but a few
hundred guests each pressing play is several GB. On Cloudflare that is fine. If
you want it smaller anyway, 30 seconds at 1080×1920 should re-encode to 4–6 MB
with no visible loss — ask and I'll do it.

---

## Where the answers go

Everything the site collects goes to **Google** — free, and on an account you
already have. Nothing posts to a server, which is why the site can be hosted
anywhere.

| What | Goes to | Config key |
|---|---|---|
| Travel itinerary | A Google Form → Sheet | `collect.itinerary` |
| Mission one, the photographs | A Google Form → **one Drive folder per prompt** | `collect.photos` |
| Mission two, the scores | A Google Form → Sheet | `collect.quiz` |
| The gift registry | A Google Sheet people read | `gifts.sheetUrl` |

**[GOOGLE-SETUP.md](GOOGLE-SETUP.md) has the questions to put in each form, in
order, with the settings that matter.** It takes about twenty minutes.

Fill them in one at a time — while a link is still in «guillemets» the site
falls back rather than breaking:

- **No `collect.itinerary`?** `travel.html` shows its own built-in form instead.
- **No `collect.photos`?** Mission one shows its own upload form instead. That
  one is a stopgap: Netlify caps a submission at 8 MB, so it only carries two or
  three photographs at a time.
- **No `collect.quiz`?** The quiz still marks itself; it just doesn't offer
  anywhere to record the score.

The quiz is worth understanding: **the questions and the marking live on the
website**, because that needs no server at all and it's the nicest thing on the
page. The Google Form only records the result — name and score — so it's two
boxes, not ten. If you'd rather have every answer in a Sheet, rebuild all ten
questions in the form and point `collect.quiz` at it; the on-site quiz carries
on working as the practice round.

### The fallback forms

`netlify-forms.html` and the built-in itinerary form are still in the project.
They cost nothing, nothing links to them once the Google links are in, and they
are what shows if a link is ever missing. If you want them gone once Google is
set up, say so.

## Working on it locally

```bash
python3 -m http.server 8788 --directory .
```

Then open http://localhost:8788. You *can* just double-click `index.html`, but
serving it over http is closer to the real thing.

---

## Deliberate choices

- **No webfonts.** System fonts only. The hand-lettering from the invitation
  carries the character, so there was nothing here worth 300 KB of Google Fonts.
- **The cover is the closing frame of the save-the-date film**, pulled straight
  out of the video rather than redrawn.
- **The paintings are the design.** Each event shows its own postcard whole, at
  its own proportions, with a soft blown-up copy of the same file behind the text
  for colour. One download, used twice.
- **No analytics, no tracking, no cookies.** Hence no cookie banner.
- **`robots.txt` blocks search engines.** A wedding isn't for the open internet.
  Delete it if you disagree.
- **Dark mode** follows the reader's system setting, on every page.
- **Works without JavaScript**, degraded: backdrops fall back to flat colour and
  the forms post normally instead of in-place.

### About the weight

The site is about **21 MB**, and 19 MB of that is `save-the-date.mp4`. The video
is set to `preload="none"`, so it costs a visitor nothing until they press play —
the page itself is roughly 700 KB, and most of that is lazy-loaded artwork.

If you want it lighter, re-encode the video (30 seconds at 1080×1920 should
compress to 4–6 MB without visible loss) or host it on YouTube/Vimeo and embed it.

## Files

```
index.html  events.html  travel.html  gifts.html  404.html
missions.html            ← the two missions; see `games` in config
netlify-forms.html       ← fallback form declarations. Unlinked. Harmless.
GOOGLE-SETUP.md          ← how to build the three Forms and the Sheet
data/config.js      ← you edit this
data/posts.js       ← the feed's posts; nothing loads it while the feed is off
assets/css/style.css
assets/js/site.js   ← wiring
assets/art/         ← the paintings, plus the save-the-date card used as the cover
assets/img/         ← logo, hand-lettering, the Jagannath eyes
assets/photos/      ← the photographs each painting was made from
assets/video/       ← the save the date
netlify.toml  robots.txt
```
