# The site plan

This is the plan for licketysync.com, for the people and agents who write and
review its pages. The site is LicketySync's manual: friendly enough for
anyone, with enough detail for the people who need it, such as an IT team
checking it against company policy or a reader with privacy concerns of their
own.

The product line, a guide for authors rather than text to quote on the site:
LicketySync is safe, easy and reliable; simple to use, private by default and
trivial to share. Every page should leave it true. Where the facts and the
product line pull apart, the facts win (see the truth rule).

## The pages, in reading order

The order below is the order of `[extra] manual` in `config.toml`, which
drives the menu, the previous and next links at the foot of each page, and
the contents on Home. A new page goes into that list in its place.

| # | Page | File | What it is for | Who it is for |
| --- | --- | --- | --- | --- |
| – | Home | `_index.md` | What LicketySync is, why you'd want it, that your files and their names are private by default and it's easy to share, that it's still being built, and where to go next. | Everyone, on a first visit |
| 1 | How it works | `how-it-works.md` | Folders, keys and devices that sync with each other: no account, and no server you didn't turn on. | Anyone curious, before trying it |
| 2 | Setting up | `setting-up.md` | What the guided setup will suggest, and each choice with what it gives you, what it costs, and what it shows to whom. | Someone setting it up (once there is something to set up) |
| 3 | Sharing | `sharing.md` | Files and their names private by default; sharing with people; the three kinds of key; sharing more widely, when both sides use public trackers; removing someone means making new keys. | Anyone sharing a folder |
| 4 | How devices find each other | `finding-each-other.md` | The local network, public trackers, relays and hole punching, explained simply, with what each one shows and to whom. | Anyone whose devices are not on one network, or who wonders what "hole punching" means |
| 5 | Features | `features.md` | What it does, and what is coming, in plain words. | People comparing it with other tools |
| 6 | Privacy | `privacy.md` | What stays private, what no peer-to-peer program can hide, and the privacy of this website. | Everyone, and privacy-minded readers above all |
| 7 | Risks | `risks.md` | Where your risks are and how they show up, for an average user. Not a log of the decisions behind them. | Anyone about to trust it with their files |
| 8 | For IT and privacy reviewers | `for-reviewers.md` | What it contacts, what it stores and where, the cryptography in brief, the threat model's link, the licences. | IT teams, compliance and privacy reviewers |
| 9 | Questions and answers | `faq.md` | Short answers to common questions, each linking to the page with the details. | Everyone |
| 10 | Glossary | `glossary.md` | Short definitions of the words the manual uses. | Everyone, looked up rather than read |

Home and all ten pages are written, in the voice of the tone guide below.
A change to the site is published only after the owner has seen it.

### What each page covers

- **How it works.** A folder lives on each device that holds one of its keys.
  Devices connect to each other and swap changes. There is no account, and no
  server you didn't turn on: devices talk directly where they can, and a relay,
  once relays are on, passes along encrypted traffic it cannot read. Every
  connection is encrypted. Changes are signed with the folder's write key, so
  devices accept changes only from it. When two changes collide, both are kept
  (a conflict copy). Replaced and deleted files go to an archive.
- **Setting up.** What the guided setup will suggest, and the settings behind
  it, one per connection choice. Describe the choices, not the screens: the
  specifications set out the settings, not how setup shows them. Every choice
  has a device-wide setting, and a new folder starts from the device-wide
  settings and may override them. One section per choice a folder can
  override: finding devices on the local network, direct connections (hole
  punching among them), whom a folder may use to coordinate a hole punch ("Who
  may help with a hole punch?"), public trackers (the DHT), using public
  relays, and Tor; and one on asking the router to open a port. Then the
  choices that are device-wide only and off before setup: acting as a relay
  for others and acting as a hole-punch coordinator, both on computers only,
  never on a phone (a computer that relays also coordinates), and helping run
  the public trackers (server mode). Coordinating comes in two kinds: for the
  devices of the person's own shared folders only, and for anyone who reaches
  the device. For each: what it gives you, what it costs, what it shows and to
  whom, and what setup will suggest. Setup's defaults are these: nothing is
  on before setup's answer; setup asks once and
  suggests the recommended settings, which are finding devices on the local
  network, asking the router to open a port, public trackers (client mode
  only), hole punching at the fourth "Who may help with a hole punch?" level
  (also any device yours knows of, and any public relay you've turned on), on
  computers coordinating punches for the devices of the person's own shared
  folders, and using public relays; saying no opens the settings, and any of
  these can be turned off; setup never suggests acting as a relay,
  coordinating for anyone or helping run the public trackers. Say, where hole
  punching is explained, that at that level every helper used, a stranger's
  public relay among them, learns that two devices are connecting, when and
  their addresses, and that a helper never carries file data. Never promise
  that a punch works: it needs a helper both ends allow, and most home routers
  let it through, not all. The narrower coordinating mode is not in the
  specifications yet: write it as how setup will behave, in the future tense.
  Call them "the recommended settings" on every page. Where coordinating is
  explained (Setting up, Sharing, For IT and privacy reviewers), say that a
  device your computer helps names your computer, by its address and network
  ID, to its own folders' peers and in its invites, so strangers can learn
  both and ask it for a punch. For IT and privacy reviewers says that these
  defaults are decided but not yet in the specifications. Say which of these
  arrive later.
- **Sharing.** Files and their names are private by default: a folder syncs
  only with devices that hold one of its keys. The three keys, what each
  holder can do, and who can pass on which key. Sending a key safely (in
  person by QR code, or over a channel
  you already trust), and why a key sent by email or chat is as good as
  public. Sharing more widely: anyone who holds a key can join, so a key can
  go to people you have never met (a read key for files you mean to give away,
  say). Say what such a person gets and learns (threat-model.md §5). With a
  read or write key ("To a key holder"): everything in the folder, now and
  later; which device wrote each version and when, to the microsecond; and
  which files each device keeps. With a sealed key, never the contents or
  names, but what threat-model.md §5 "To a sealed peer" lists in full, among
  it: how many files there are and roughly how big they are; copies and
  renames, since identical pieces are visible; where a file's history split
  and joined again; how many files a deleted folder held; when each version
  arrives and from which device; which encrypted pieces each device holds;
  each device's reported status, free disk space included; and, for someone
  who knows some of the folder's files, a test of whether other files they
  have are in it. With any key ("To the peers of a
  device's folders"): the names and IDs of the devices that share it, and,
  through them, their addresses and the hole-punch coordinators they use; and
  the key itself, which they can pass on, for good. With public trackers on at
  both ends, their devices can find the addresses of the devices that offer
  the folder without being told them. Finding is not connecting: public
  trackers carry addresses only, never the hole-punch coordinators (keys.md
  §2.2), so a device behind a home router may stay out of reach of a newcomer
  who has met none of the folder's devices directly and holds no invite with
  addresses. What public trackers show is in the truth rule's example below.
  Removing someone, or a lost device: there is no taking a key back, so the
  people who stay move to a new folder with new keys; the steps, in plain
  words, saying that restoring from the archive comes with the archive,
  later. Say what it does not do (keys.md §7.4): the removed person keeps the
  old folder, its key and every file they already had, and a device that
  misses the move keeps syncing with them.
- **How devices find each other.** On the local network. Through public
  trackers. By an address you type in. Hole punching, explained with a simple
  picture: two devices behind home routers, a helper that passes a first
  note, then a direct line. Relays: a go-between that passes sealed envelopes
  it cannot open. For each, what it shows and to whom, and what is off until
  you turn it on.
- **Features.** What it does, and what is on the way, grouped plainly:
  syncing, sharing, the three keys, storage devices, conflict copies, the
  archive, apps for computers and phones, the browser version, linked
  devices, choosing what to keep on a device. Say clearly which exist and
  which are coming; today none ships.
- **Privacy.** What stays private (file contents and names, from everyone
  without a read or write key), with this exception: someone holding only the
  sealed key who knows some of the folder's files can, in time, test whether
  other files they have are in it (threat-model.md §5, "To a sealed peer";
  keys.md §3.2). What a peer-to-peer program cannot hide (that your device
  runs LicketySync, its address, who talks to whom and when). What each kind
  of party sees, in a short table. That deleted and replaced files stay in the
  archive of every device that can read the folder and had them, for as long
  as that device's archive keeps them (30 days unless its person changes it),
  and stay as ordinary files on a device that unlinks the folder (keys.md
  §7.3). Then this website's own privacy.
- **Risks.** For an average reader: each risk as
  what can happen, how you would notice, and what to do. The full detail
  stays in the threat model, linked at the end.
- **For IT and privacy reviewers.** What the program contacts: no host you did
  not turn on. `core/spec/allowed-hosts.txt` lists the fixed hostnames and
  addresses built into the program, and it is empty today; peers, DHT nodes,
  relays and hole-punch coordinators are reached at addresses learned while it
  runs, and only through features that are on. Say that nothing is on before
  setup's answer, and what setup will suggest, as the Setting up entry above
  states it. What it stores
  on each device and where, which settings exist for each connection type and
  their defaults, the cryptography in brief (`core/spec/suites.md`), what each
  party can see (`threat-model.md` §5), links to the threat model and
  specifications, how vulnerabilities are reported, and the licences. This is
  the one page where the threat model's TM numbers may appear.
- **Questions and answers.** Short questions in a reader's words ("Is there a
  cloud?", "Can I take back a key?", "What happens if two of us edit the same
  file?", "Does my company's network block it?"), each answered in two or
  three sentences with a link.
- **Glossary.** One or two sentences per word: archive, conflict copy,
  device, folder, hole punching, hole-punch coordinator, key (write, read,
  sealed), local network, peer to peer, public trackers (the DHT), relay,
  storage device, sync.

### Later, once there is something to download

Download, Screenshots (captured in CI from the real app), Releases, Compare,
Limitations, Contribute: the pages the site's README listed before this plan.
Each joins the reading order when it has something true to say. "Docs" is
this manual.

## The tone guide

- **Friendly, playful, succinct.** Write to the reader as "you". Short
  paragraphs, short sentences, one idea per section. A light touch of play is
  welcome, one or two per page, and never about risks or lost files.
- **Lead with what it means for you.** Say what happens and what to do first.
  Put the detail after, for those who want it, or on the page for reviewers.
- **Keep the essentials visible.** Everything a reader needs to decide or act
  stays on the page. Reviewer-level numbers and edge cases go behind a `more`
  (see "Building blocks") or on the page for IT and privacy reviewers.
- **No quoted or dated decisions, no "reviewed by" notes.** The site
  says what is true, not who decided it or when. The history lives in the
  repositories and the plan.
- **Never techy for its own sake.** Use a technical word only where the reader
  needs it, explain it the first time it appears on each page, and add it to
  the glossary. TM numbers, crate names and protocol message names stay on
  the page for reviewers.
- **Honest.** Say what it cannot do as plainly as what it can, and never
  oversell. A reader should never find out from someone else.
- **The same words everywhere.** Use the words of the glossary, and the words
  the app will use: write key, read key, sealed key, storage device (a device
  that holds only the sealed key), conflict copy, archive, relay, hole-punch
  coordinator, public trackers, the recommended settings (what setup will
  suggest). "Public trackers" is the everyday name for internet discovery
  through the DHT, a public directory spread across many
  strangers' computers rather than a server anyone runs. On the pages that
  explain it (How devices find each other, Setting up, For IT and privacy
  reviewers, the glossary), say "public trackers (the DHT)" the first time;
  Home and short mentions elsewhere need not.
- **British spelling** (licence, colour, recognise, behaviour), as the rest of
  the project writes.
- **No serial comma**: "red, white and blue", unless the comma prevents a
  misreading.

## The truth rule

- **Every claim is true to the specifications on core's main branch**:
  chiefly `core/spec/protocol.md` §9, "The index in plain words", and
  `core/spec/threat-model.md` §5, "What stays visible"; for the cryptography,
  `core/spec/suites.md`; for keys and invites, `core/spec/keys.md`. Check
  every sentence against them before it ships, and again when they change.
  Where a page and the specifications disagree, raise it; never quietly pick
  one. Where they are silent, ask; never guess.
- **Nothing ships yet. Say so plainly.** Never describe a feature as
  available when it is not. Describe how the design works in the present
  tense ("every connection is encrypted"); describe anything a person would
  do or see in an app, and anything planned for later, in the future tense
  ("setup will ask", "coming later"). The ribbon on every page but Home, and
  the status on Home, say that it is still being built; they do not excuse a
  sentence that reads as if it shipped.
- **The facts win.** Where the facts and the product line (at the top of this
  plan) pull apart, say the facts, plainly, and let the product line give way.
- **Where the specifications differ, take the weaker promise and raise it.**
  For example, keys.md §1.3 says a sealed key holder cannot test whether the
  folder holds a known file; threat-model.md §5 says published attacks let
  someone who knows some of the folder's files do it. Write the second.
- **Privacy promises stay true for every party** in threat-model.md §5. Never
  "nobody will know what you're sharing" without the exceptions. For example:
  with public trackers on, anyone who has ever had or seen any of the folder's
  keys can find, every day, the internet addresses of the devices that offer
  it; people with no key cannot tell which folder an address offers. And with
  finding devices on the local network on, anyone on your network can see that
  your device runs LicketySync, anyone who knows a folder can confirm it is
  there, and anyone there can replay a device's announcement to see which
  devices answer it, and so which of them hold that folder.
- **Defaults and settings are named.** When a claim depends on a setting, say
  which one and where it stands: nothing is on before setup's answer; setup
  asks once and suggests the recommended settings (as in the Setting up entry
  above), and saying no opens the settings; setup never suggests acting as a
  relay, coordinating hole punches for anyone or helping run the public
  trackers; acting as a relay or as a hole-punch coordinator is device-wide
  only and on computers only; no page promises that punches work. Each folder
  starts from the device-wide settings and may override them.
- **Numbers come from the specifications.** Round them for readers without
  changing what they mean: a deletion is forgotten "a year or more" after it
  arrived (sooner if a device with the write key has a wrong clock), never
  "about a year"; the archive keeps files "30 days unless you change it".
- **Link rather than restate.** Each page links to the specification section
  that backs it, for readers who want the full version.

### What is coming, for authors

Plain words for the site, from the specifications and the roadmap; check the
plan before naming a phase on a page, and prefer "next" and "later" to phase
numbers:

- now: the design and its specifications, and a simulator that tests the
  sync engine on many simulated devices; nothing to download;
- next (phase 1): syncing between computers on one local network, from the
  command line, which can export keys as key strings, links and files;
- phase 2: hole punching, public trackers (the DHT) and storage devices (the
  sealed key) in real use;
- phases 3 and 4: the apps for computers and phones, with keys shown as QR
  codes in the apps from phase 3;
- phase 5: linked devices, choosing what to keep on a device, and the archive;
- phase 6: the browser version, at licketysync.app;
- phase 7: public relays and Tor; then a manual testing pass before 1.0, with
  no betas before it.

## Illustrations

- **SVG only**, in `static/images/`, drawn in the house style: ink outlines
  about 3 to 4 units wide with round ends and joins, cream and white paper,
  coral, sunshine, mint and sky fills, small sparkles and dots. Screenshots of
  the app, when there are some, are the one exception.
- **Light and dark.** Each SVG carries a `<style>` with a
  `prefers-color-scheme: dark` block that swaps the ink and paper colours, as
  the existing files do. Ink drawn on a fill that does not change in dark
  (sunshine, coral, mint, sky) keeps its dark ink, so it never fades (the face
  in `lost-folder.svg`, the keyhole in `home-devices.svg`); only ink on the
  page or on paper turns light. Check both. The one exception is `favicon.svg`
  (and the logo, which is the same drawing): a coral tile with dark arrows,
  fixed colours that read on light and dark alike, so it has no dark block.
- **Words stay out of the drawings** where they can, so they can be read,
  searched and translated. A drawing that needs a label repeats it in its alt
  text.
- **Alt text** says what the picture shows and why it is there. A purely
  decorative picture gets `alt=""`.
- **Self-contained and safe**: no scripts, no external references, no fonts,
  no `http:` except the SVG namespace in `xmlns`. Each file must parse as
  well-formed XML.

What each page needs:

| Page | Illustrations |
| --- | --- |
| Home | Devices in sync, `home-devices.svg` (done); the three keys, `key-write.svg`, `key-read.svg`, `key-sealed.svg` (done) |
| How it works | A folder on three devices, a key beside each, device to device, with nothing in between by default and a crossed-out cloud in the middle, `how-it-works-folder.svg` (done); a conflict copy kept beside the file, `how-it-works-conflict.svg` (done) |
| Setting up | A device's settings passed to a folder that changes one, `setting-up-switches.svg` (done); one small icon per question: `setting-up-local-network.svg`, `setting-up-direct.svg`, `setting-up-router-port.svg`, `setting-up-hole-punch.svg`, `setting-up-trackers.svg`, `setting-up-relay.svg`, `setting-up-tor.svg` (done) |
| Sharing | Handing over a key by QR code in person, `sharing-qr.svg` (done); the three keys (Home's, reused, done); a folder offered widely through public trackers, `sharing-widely.svg` (done); moving to a new folder with new keys, `sharing-new-folder.svg` (done) |
| How devices find each other | Devices around a home router, `finding-local.svg` (done); public trackers as a directory spread across many computers, `finding-trackers.svg` (done); hole punching: two routers, a helper passing a note, then a direct line, `finding-hole-punch.svg` (done); a relay passing a sealed envelope, `finding-relay.svg` (done) |
| Features | A small icon per feature: `features-conflict-copy.svg`, `features-archive.svg`, `features-storage-device.svg`, `features-apps.svg`, `features-browser.svg` (done) |
| Privacy | Who sees what: a key holder (open eye), a storage device (half-open eye: it sees the envelope, not the letter), a relay and a network watcher (closed eye on the contents, but they see who talks to whom), `privacy-eyes.svg` (done) |
| Risks | One gentle opening picture (an umbrella: it's got your back), `risks-umbrella.svg` (done); an icon per group of risks (later) |
| For IT and privacy reviewers | What talks to what: devices, the local network, public trackers, relays and coordinators, marked "off until you turn it on" where that is true, `for-reviewers-map.svg` (done) |
| Questions and answers, Glossary | None needed |
| Everywhere | The logo (inline in the header and footer, done), `favicon.svg` (done), `coming-soon.svg` (done), `lost-folder.svg` on the 404 page (done) |

## Building blocks

Every page is a Markdown file under `content/` with a TOML front matter:

```toml
+++
title = "Sharing"
description = "One line, shown under the title and used as the page's search description."
+++
```

Content is also a Tera template, so pages can call the components in
`templates/components.html`:

- `{{ <illustration src="key-write.svg" alt="What it shows" width={132} height={84} /> }}`
  for a picture from `static/images/`, with an optional `caption="..."`.
  `alt` and `caption` are plain text (no Markdown or HTML; quotes are fine).
  With a caption the picture is a `<figure>`; without one, it is a plain
  block;
- `{% <note> %}Markdown text{% </note> %}` for a highlighted note;
- `{{ <lead_in text="How bad:" /> }}` at the start of a line, for the short
  labels that let a list of risks or choices scan quickly ("How bad:", "What
  to do:", "Default:"); it shows as a small bold pill. Plain text only;
- `{% <more summary="The details"> %}` and `{% </more> %}` around Markdown,
  each on a line of its own, for depth a reader can open if they want it: the
  detail stays one click away, behind a bold line ("How the year is counted",
  "What a storage device sees, in full"). Put no headings inside it: a heading
  would show in "On this page" while it is hidden;
- `{% <table_box label="Settings and their defaults"> %}` before a Markdown
  table and `{% </table_box> %}` after it, each on a line of its own with a
  blank line between them and the table (below);
- `{{ <coming_soon /> }}` on a page that is not written yet; delete it when
  the page is.

Text passed to a component (`alt`, `caption`, `text`, `label`, `summary`) is
plain text and is not given curly quotes: type ’ for an apostrophe and “ ” for
quotes yourself ("What’s next:").

The ribbon on every page but Home points at Features ("See what's coming", in
`templates/base.html`), and Home's main button at How it works ("See how it
works", in `content/_index.md`).

HTML in Markdown works where a layout needs it (`<div class="cards">` with
`<div class="card">` inside, as on Home); leave a blank line between the HTML
and the Markdown inside it. In the 44rem column of a manual page, use
`<div class="cards cards-3">` for three compact cards across (the three keys,
say); they stack on a phone.

Put every Markdown table in a `table_box`. The box scrolls sideways on its
own when the table is wider than the column, so the page never does, and the
cells wrap between words. The box is a named region a keyboard can reach, so
its `label` says what the table holds, differently for each table on a page
("Settings and their defaults", "Who sees what"). Keep tables to a few short
columns all the same; on a phone a list often reads better:

```
{% <table_box label="Who sees what"> %}

| Who | Sees |
| --- | --- |
| A relay | Both devices’ addresses, not the folder |

{% </table_box> %}
```

Links to the specifications may point at a section on GitHub
(`…/spec/threat-model.md#5-what-stays-visible`): `zola check` skips anchor
checks on github.com, whose anchors only exist once its scripts run, so check
such an anchor by hand. A page with more than two `##` headings gets an
"On this page" list by itself.

## The site's fixed rules

- HTTPS only; no plain-HTTP URLs anywhere.
- No scripts, ads, analytics or cookies; every page works without JavaScript
  (the phone menu is a `<details>` element).
- Nothing third-party is loaded: no fonts, CDNs or embeds. The theme uses
  system fonts. A font, if one is ever wanted, is self-hosted under an open
  licence and downloaded from its official source with its checksum.
- Colours meet WCAG AA in both themes (the pairs are listed at the top of
  `static/style.css`).
- **No invites on the site** (threat-model.md TM-18): never a real or
  decodable key string, word list or join link, not even as an example. Use a
  plainly fake placeholder such as `<your key>`.
- **What padding does not hide is stated** (TM-15): the number of pieces, the
  total amount of data, when changes happen and each version's approximate
  size. Before a release turns on compression, the site states the length
  channel it opens.
- **The browser version's checks are documented** (TM-28): when the browser
  version ships, the site explains how to verify the bundle it serves (WEBCAT
  and Code Verify-style checks, and the native app's manifest check).
- MPL-2.0 for the site's code; CC BY 4.0 for its prose and its illustrations
  (the SVGs in `static/images/`). The logo and `favicon.svg` are not
  illustrations: they stay under the repository's MPL-2.0, like the
  templates.
- **Every table is in a `table_box`.** The page check fails on a table
  outside one.

## Checks before a change

- `zola build` and `zola check` (Zola 0.23.6, the version the workflow pins).
- The page check in `.github/workflows/pages.yml`, run by hand over `public/`.
- `typos` over the repository.
- Each SVG parses as well-formed XML.
- Look at every changed page in a light and a dark theme, on a phone width
  (about 375 pixels) and a desktop width.
