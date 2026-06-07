# OM4ATC Groundstation Netlify Proxy

This repository keeps the Netlify URL while serving the real OM4ATC Groundstation website from:

`https://om4atc-server.tailb28193.ts.net/groundstation-om4atc`

Netlify uses the root `_redirects` file. The current rule proxies every Netlify path to the matching path under `/groundstation-om4atc`:

```text
/groundstation-om4atc/* https://om4atc-server.tailb28193.ts.net/groundstation-om4atc/:splat 200!
/* https://om4atc-server.tailb28193.ts.net/groundstation-om4atc/:splat 200!
```

The first rule handles the real site's injected `/groundstation-om4atc/` base path without duplicating it. The catch-all rule proxies normal Netlify paths.

- A rule ending in `200!` is a forced rewrite/proxy. It keeps the Netlify URL visible and overrides the fallback `index.html`.
- A rule ending in `302` is a temporary visible redirect. It changes the browser URL to the target.
- Do not use `301` while testing because browsers cache permanent redirects aggressively.

For proxy/masking mode, relative asset and API paths are usually best. For example, `/api/status`, `/app.js`, `/style.css`, `/images/photo.jpg`, `/camera`, and `/stream` are proxied beneath the real site's `/groundstation-om4atc` mount.

If proxy mode breaks, replace the `_redirects` rule with this visible redirect fallback:

```text
/* https://om4atc-server.tailb28193.ts.net/groundstation-om4atc/:splat 302
```

Netlify proxy masking can break when the real site uses WebSockets, video streams, cookies, CORS restrictions, backend redirects, or absolute asset URLs. If that happens, use the `302` redirect or redesign the frontend so Netlify hosts the static page and only selected API/video endpoints come from the Funnel.
