+++
title = "Privacy"
description = "What stays private, what no peer-to-peer program can hide and what this website collects: nothing of its own."
+++

Your files and their names are private by default: a folder syncs only with
devices that hold one of its keys, every connection is encrypted and there's
no account and no server you didn't turn on. Nothing is turned on before you
answer setup, and the program sends us no telemetry (reports about how you use
it). There's
nothing to download yet, so this page is about the design.

{{ <illustration src="privacy-eyes.svg" alt="Four figures, each with an eye. A key, eye wide open: people with the read or write key see your files. A storage box, eye half open: it sees the size and rhythm of your folder, not what’s in it. A sealed envelope and a magnifying glass, for relays and anyone watching the network, eyes shut to what’s inside, though they see who talks to whom." width={560} height={200} /> }}

## What stays private

- **Your files and their names.** Contents, names, exact sizes, the dates
  stored with each file and the folder's layout can be read only by devices
  with the read or write key. Storage devices, relays and hole-punch
  coordinators (the helpers that let two devices connect when they can't reach
  each other directly), the network and we can't; of the layout, a storage
  device learns only how many files a deleted folder held.
- **Your keys.** A key leaves your device when you share it, and there's no
  server to send it to. The app will show or export a key only when you ask,
  and the write key only after your system's own check that it's you (or,
  where there's none, a passphrase). Links for joining a folder will point to
  licketysync.app, with the key after the `#`, the part of a link that
  browsers never send to any server; this website never carries one. A key can
  still be taken from a lost or stolen device, by malware running as you, or,
  for the browser version, by a broken-into licketysync.app.
- **What you do, from us.** There's no account. The program sends no
  telemetry, analytics, crash reports or update checks; the apps will get
  updates through your package manager or app store. The browser version is
  the exception: it will load from licketysync.app each time you open it, so
  that site will see who loads it.

A few things are private only up to a point:

- Someone running a storage device who knows some of the folder's files, or got
  files of their choosing into it, can in time test whether other files they
  have are in it, from the sizes of their pieces.
- Each device keeps its own archive of replaced and deleted files, 30 days
  unless its person changes that. So a file you delete stays in the archives of
  the other devices that had it, and when someone unlinks a folder, its files
  stay on their disk as ordinary files.
- The people you share a read or write key with see which device made each
  change, and when, to the microsecond.

## What no peer-to-peer program can hide

Your devices talk to each other directly, so some things show. The
recommended settings will turn on finding devices on your local network, an
open port on your router (a numbered door that lets other devices reach
yours), public trackers, hole punching (with any helper your device knows of),
public relays and, on computers, helping the devices you share a folder with
punch through; what follows says what each shows if you take them.

- **That you use it.** Anyone on your local network can see that your device
  runs Lickety-Sync, and anyone who knows a folder can confirm it's offered
  there. Anyone there, even knowing nothing, can replay an announcement to
  learn which devices answer it, and so which hold that folder.
- **An open port.** While your router keeps a port open for your device, anyone
  on the internet can reach it and learn that your device runs Lickety-Sync,
  its network ID and roughly how many folders it holds.
- **Your address**, to every device of your folders, to anyone who gets an
  invite that carries it, to the helpers your device talks to and, if your
  computer helps with hole punches, to everyone the devices it helps share a
  folder or an invite with.
- **Who talks to whom**, when and how much, to anyone watching the network.
  File sizes are blurred a little, but nothing hides how much moves, or when.
- **Your device's network ID**, a name for it on the network that stays the
  same, to any device that connects to it, which can then recognise your device
  on any network and in any folder.
- **Where a folder lives**, through public trackers (the DHT: a public
  directory spread across many strangers' computers). Anyone who has ever had
  or seen one of a folder's keys can then find, every day, the addresses of the
  devices that offer it; people with no key can't tell which folder an address
  offers. But anyone can follow one device there, day after day, by its address
  or by the identifiers it publishes with it, which stay the same.

## Who sees what

If you take the recommended settings:

{% <table_box label="Who sees what"> %}

| Who | What they see |
| --- | --- |
| People with the write or read key | Everything in the folder; which device wrote each version, and when; the devices' names, addresses and hole-punch coordinators; which files each device keeps. Not which person made a change. |
| A storage device (the sealed key) | How many files there are, roughly how big, when they change and from which device; the devices' names, addresses and coordinators. Not contents, names or the dates stored with each file; of the layout, only how many files a deleted folder held. |
| A relay | Both devices' addresses and network IDs, when they talk and how much. Not which folder, nor any file. |
| A hole-punch coordinator, which helps two devices connect directly | That two devices are connecting, and when; their addresses and network IDs. Not which folder, any key or any file. With the recommended settings, it can be a stranger's public relay. |
| Public trackers (the DHT) | The addresses that offer a folder, to anyone who has had or seen one of its keys; to anyone else, one device's announcements day after day, but not which folder. |
| Your local network | That your device runs Lickety-Sync; whether a folder they know is offered there, and, by replaying an announcement, which devices hold it. Not contents, names or keys. |
| Anyone watching the internet | Which addresses talk, when and how much. Not what they say, or which folder. |
| Anyone on the internet, while your router's port is open | That your device runs Lickety-Sync, its network ID and roughly how many folders it holds. Not which folders. |
| A device that connects to yours with no key | Your address and network ID, and roughly how many folders it holds. Not which folders, or your device's name. |
| Us | No telemetry and no account. We run no server for syncing; licketysync.app will see who loads the browser version. |

{% </table_box> %}

[Risks](@/risks.md) says how each of these shows up and what you can do about
it, and [For IT and privacy reviewers](@/for-reviewers.md#what-each-party-sees)
has the precise version, with links to the specifications.

## This website

This site is a set of static pages served by GitHub Pages. The pages contain no
JavaScript, set no cookies, include no analytics and load nothing from any
other site: no scripts, fonts, images or style sheets.

GitHub hosts the site. In its
[GitHub Pages documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages#data-collection),
GitHub says it logs and stores the IP address of every visitor for security
purposes, whether or not they are signed in to GitHub. What GitHub collects and
how it uses it is set out in GitHub's
[General Privacy Statement](https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement).
