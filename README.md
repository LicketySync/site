# Lickety-Sync — site

The documentation site, built with Zola and published with GitHub Pages at
https://licketysync.com. Rules from the plan: no ads, no registration, no
analytics, no cookies, no third-party scripts or fonts; every page works without
JavaScript. After each build the Pages workflow greps the pages and fails on a
`<script` tag, an `on…=` event handler, a `javascript:` attribute value, or an
off-site URL in `src` or `srcset`, in a stylesheet, icon, preload, prefetch,
preconnect or manifest `<link>`, or in a CSS `url()` or `@import`. It catches
accidents, not every way a page can load something; its known limits are listed
beside it in `.github/workflows/pages.yml`. The browser client is *not* here; it
is published from the `web` repository at https://licketysync.app.

Pages planned: Home, Features, Screenshots (captured in CI from the real app),
Download, Compare, Docs, Limitations, Releases, Contribute. MPL-2.0 for the
site's code; CC BY 4.0 for its prose.

Build locally: `zola serve`.
