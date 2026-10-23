# Lickety-Sync — site

The documentation site, built with Zola and published to GitHub Pages. Its
domain is to be **licketysync.com**; until that domain is registered and
verified, the site is served at https://licketysync.github.io/site/. Rules from
the plan: no ads, no registration, no analytics, no cookies, no third-party
scripts or fonts; every page works without JavaScript. The Pages workflow fails
the build if a page contains a script or loads anything from another site. The
browser client is *not* here; it will live at licketysync.dev.

Pages planned: Home, Features, Screenshots (captured in CI from the real app),
Download, Compare, Docs, Limitations, Releases, Contribute. MPL-2.0 for the
site's code; CC BY 4.0 for its prose.

Build locally: `zola serve`.
