+++
title = "What’s ready and what’s next"
description = "Where LicketySync stands today and what comes in what order, in plain words. No dates, no betas."
+++

There's nothing to download yet, so no need to hold your breath. LicketySync
is being built in steps, and each step ends with a check it has to pass before
it counts as done. We build and test on Linux and in the Chrome browser first.
We won't promise dates, and there won't be test versions (betas) before
version 1.0.

{{ <illustration src="whats-next-path.svg" alt="A winding dotted path with flags along it. The first flag is ticked; Zippy, LicketySync’s smiling folder, walks towards the second; more flags lead on to a burst of sparkles at the end." width={560} height={220} /> }}

## Ready today

The groundwork, with nothing for you to run yet:

- the design, written down in detailed specifications, with a threat model
  that says what an attacker could do and what stops them;
- a practice arena where pretend devices lose messages, crash, lose their
  connections and fill their disks, so the sync engine learns to cope;
- the building blocks: the keys, encrypted connections, the list of files each
  device keeps, and safe file handling.

Nothing syncs on a real disk or network yet.

## Now: syncing on one network

From the command line, between computers on the same home or office network:

- folders and their keys, which you'll be able to export as key strings,
  links and files, and the write key as 24 words;
- [finding devices on your local network](@/finding-each-other.md#on-your-local-network);
- noticing changes on the disk as they happen;
- [conflict copies, the archive and the deletion brake](@/conflicts-and-the-archive.md);
- [ignore files](@/choosing-what-syncs.md#the-ignore-file);
- where each file stands: in sync, on its way or stuck, and why;
- [the setup](@/setting-up.md#how-setup-will-ask) that asks once before
  anything connects;
- [calm handling of problems](@/when-something-goes-wrong.md) on your device,
  and `licketysync doctor`.

Its check: two Linux computers keep a folder in sync through a set of tests
that break things on purpose.

## Next: across the internet

[Public trackers](@/finding-each-other.md#through-public-trackers-the-dht),
[hole punching](@/finding-each-other.md#hole-punching), asking your router to
open a port, invites that can carry your addresses, and
[storage devices](@/sharing.md#three-keys-three-kinds-of-access) that keep a
copy they can't open. Its check: two home networks sync, and a recording of
every packet shows LicketySync contacted nothing it shouldn't.

## Then: the apps

- **For computers**, Linux first, then Windows and macOS: a friendly window,
  keys shown as QR codes, notices in the corner of your screen, sync status
  on files in your file manager, and installers.
- **For phones**, Android and iOS, built alongside the computer app.

## After that: the rest of the toolbox

[Linked devices](@/features.md#devices), choosing what each device keeps,
more ways to keep old versions, a weekly schedule for when to sync, proxies
and a background check that the files on disk are still sound.

## The browser version

A shared folder in Chrome, with nothing to install, at licketysync.app, and
one-time links for sending a single file. It needs only the internet step
before it, so it may arrive before the apps are done.

## Last: relays and Tor

[Public relays](@/finding-each-other.md#relays), Tor, and files your system
shows without keeping them on the device. Then a careful testing pass by
hand, and version 1.0 after it.

{% <note> %}
**For the curious.** The
[changelog](https://github.com/LicketySync/core/blob/main/CHANGELOG.md)
records every change as it lands, and
[Features](@/features.md) says what each feature will do.
{% </note> %}
