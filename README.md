# Lickety-Sync — site

The documentation site, built with Zola and published with GitHub Pages at
https://licketysync.com. The site is HTTPS-only: Pages enforces HTTPS, the
build uses an https:// base URL, and the page check fails on any plain-HTTP URL
except an XML namespace name, within the limits listed beside it. Rules from the
plan: no ads, no registration, no analytics, no cookies, no third-party scripts
or fonts; every page works without JavaScript. The page check, in
`.github/workflows/pages.yml`, greps the pages after each build and also fails
on a `<script` tag, an `on…=` event handler, a `javascript:` attribute value, or
an off-site URL in `src` or `srcset`, in a stylesheet, icon, preload, prefetch,
preconnect or manifest `<link>`, or in a CSS `url()` or `@import`. It catches
accidents, not every way a page can load something. The browser client is *not*
here; it is published from the `web` repository at https://licketysync.app.

Pages planned: Home, Features, Screenshots (captured in CI from the real app),
Download, Compare, Docs, Limitations, Releases, Contribute. MPL-2.0 for the
site's code; CC BY 4.0 for its prose.

Build locally: `zola serve`.
