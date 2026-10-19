# INES Workshop 2027

Site for the International Network for Engineering Studies workshop,
14–15 January 2027, online.

**Live site:** https://REPLACE-USERNAME.github.io/ines-workshop-2027/

## Files

- `index.html` — the whole public site: hero, about, program, lightning talk call, membership. The program lives in a `PROGRAM` array near the bottom of the file; that array is the only thing you edit week to week.
- `register.html` — registration page with the Google Form embedded.
- `ines_registration_form.gs` — Apps Script that builds the registration form. Run once, then delete or keep for reference. Not served to visitors.
- `.nojekyll` — tells GitHub Pages to serve the files as-is instead of running Jekyll over them. Without it, any folder starting with an underscore is silently dropped.

## Publishing it

```
# from inside this folder
git init
git add .
git commit -m "INES Workshop 2027 site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/ines-workshop-2027.git
git push -u origin main
```

Then on GitHub: **Settings → Pages → Source: Deploy from a branch → Branch: `main`, folder `/ (root)` → Save.**
The site appears at `https://YOUR-USERNAME.github.io/ines-workshop-2027/` within a minute or two.

Every later `git push` republishes. There is no build step.

## Before you publish, replace these

Search the files for each string:

| Placeholder | Where | What to put |
|---|---|---|
| `REPLACE-USERNAME` | `index.html`, `README.md`, `.gs` | your GitHub username |
| `REGISTRATION_FORM_ID` | `register.html` (2 places) | the id from the registration form's live link |
| `REPLACE@example.edu` | `index.html` footer | the contact address |

Check with:

```
grep -rn "REPLACE\|REGISTRATION_FORM_ID" .
```

## Editing the program

Open `index.html`, scroll to `var PROGRAM`. Each session is one object:

```js
{
  start: "2027-01-14T15:00:00Z",   // UTC — January Eastern is UTC-5, so 10:00 ET = 15:00Z
  end:   "2027-01-14T16:00:00Z",
  title: "Lightning talks",
  blurb: "One or two sentences.",
  who:   "Names and affiliations",
  tbd:   false                      // true shows a "Panelists being confirmed" tag
}
```

Times are stored in UTC on purpose: the page converts each session into the
visitor's own time zone in their browser, so someone in Athens or Seoul sees
their own clock without doing arithmetic. If you type local times instead, that
conversion breaks.

## Working on it locally

Open `index.html` in a browser — it works straight off the filesystem, with no
server, because everything is in the one file. If you later split the program
out into `data/program.json` and load it with `fetch`, that stops being true and
you will need a local server:

```
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Where the look comes from

The colors are sampled from the real INES identity rather than chosen freehand —
`#E4643C` and `#326282` are the two inks in the INES logo file, and the
backgrounds and link color are what inesweb.org actually uses. They are defined
once as custom properties at the top of each file's `<style>` block. Change them
there and the whole page follows.

Both files currently carry their own copy of that block. If the site grows past
two or three pages, move it to `assets/ines.css` and link it from each page
instead.
