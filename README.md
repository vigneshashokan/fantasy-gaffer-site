# fantasy-gaffer-site

The public site for [Fantasy Gaffer](https://github.com/vigneshashokan/fantasy-gaffer) —
served at **https://fantasy-gaffer.com** via GitHub Pages.

Exists to satisfy the App Store's required Privacy Policy / Support / Marketing
URLs, and to give the in-app share link (`APP_STORE_URL`) somewhere real to land.

## Layout

```
public/           # everything here is published as-is at the domain root
  index.html      # landing page
.github/workflows/deploy.yml
```

Add new pages, images, or `.well-known/` files under `public/` — the workflow
copies the whole directory, so nothing else needs editing.

## The legal pages are not authored here

`/privacy` and `/terms` are **generated in the app repo** from
`src/content/legal/*.ts` (the same typed source the in-app legal screens render)
and guarded there by a parity test. This site pulls the built HTML from that
repo's `main` at deploy time, so the hosted policy can't drift from what users
see in the app.

**To change legal copy:** edit it in the app repo, run `npm run legal:html`,
merge — then run this repo's workflow (it also rebuilds weekly on its own).

## Deploying

Pushes to `main` deploy automatically. Manual: Actions → *Deploy site* → *Run
workflow*. It also rebuilds weekly to pick up legal-content edits merged in the
app repo.

## Custom domain

Currently live at the repo subpath:
**https://vigneshashokan.github.io/fantasy-gaffer-site/**

`fantasy-gaffer.com` is not attached yet — its DNS still points at a parked
redirect. To attach it:

1. At the registrar, drop the parking redirect and add
   `A @ 185.199.108.153 / .109.153 / .110.153 / .111.153` (plus the matching
   `AAAA` records), and `CNAME www vigneshashokan.github.io`. Confirm the current
   IPs against Settings → Pages rather than trusting this list.
2. Settings → Pages → Custom domain → `fantasy-gaffer.com`.
3. Once the certificate issues, tick **Enforce HTTPS**.

A `CNAME` file in the published artifact does **not** work here — Actions-based
Pages deploys ignore it. The domain has to be set in settings.

Links in `public/index.html` are deliberately **relative** so they resolve both
at the subpath and at the apex once the domain is attached.
