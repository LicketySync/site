+++
title = "LicketySync: your folders, in sync"
description = "LicketySync keeps your folders in sync across your devices and with the people you choose: device to device, encrypted, with no account and no company server. In development; nothing to download yet."
+++

<section class="hero">
<div class="hero-text">

<p class="status">Still being built: there’s nothing to download yet</p>

# Your folders, in sync. <em>Lickety-split.</em>

LicketySync keeps a folder the same on all your devices, and with the people
you choose. Your devices sync with each other, so there's no account to make,
no cloud to trust and no server you didn't turn on.

<div class="actions">

[See how it works](@/how-it-works.md)
[Read the risks](@/risks.md)

</div>
</div>

{{ <illustration src="home-devices.svg" alt="A laptop, a phone and a desktop computer, each showing the same folder, linked to each other directly by dotted lines, with a padlock on one line and a folder zipping along another." width={480} height={360} /> }}

</section>

## Why you'll like it

<div class="cards">
<div class="card">

### Private by default

Your files and their names go only to devices that hold one of the folder's
keys, and every connection is encrypted. Nothing is on until you answer setup.
It will suggest helpers such as public trackers and relays: computers you
don't know, which see your addresses but can't open your files.

</div>
<div class="card">

### Easy to share

To share a folder, you'll give someone a key, as a QR code, a link or a
file. The kind of key decides what they can do.

</div>
<div class="card">

### Nothing to sign up for

There are no accounts, and no company holds your files. Your devices sync
with each other, not through someone else's cloud.

</div>
<div class="card">

### It's got your back

When two people change the same file at once, both versions are kept: one as
the file, one as a conflict copy beside it. When a change from another device
replaces or deletes a file, the old one waits in an archive for 30 days
(you'll be able to change that).

</div>
</div>

<section class="panel">

## Three keys, three kinds of access

Every folder has three keys. Whoever holds one can join the folder with that
key's access, on as many devices as they like.

<div class="cards">
<div class="card">

{{ <illustration src="key-write.svg" alt="" width={132} height={84} /> }}

### Write key

Read and change everything. For your own devices, and for people you'd trust
with every file in the folder.

</div>
<div class="card">

{{ <illustration src="key-read.svg" alt="" width={132} height={84} /> }}

### Read key

See everything, change nothing. Other devices take changes only from the
write key.

</div>
<div class="card">

{{ <illustration src="key-sealed.svg" alt="" width={132} height={84} /> }}

### Sealed key

Keep a full copy without being able to open it: not the files, not even their
names. A device with this key still sees how many files there are, roughly
how big each one is, when they change and the names of the devices it syncs
with. Handy for a home server, or a friend's spare computer, that keeps a
copy for you.

</div>
</div>

A key works for as long as the folder exists, and whoever has one can pass it
on. So share keys the way you'd share a house key. There's no taking a key
back: to leave someone out from then on, everyone else moves to a new folder
with new keys, and that person keeps the old folder and every file they
already had.

</section>

## Still being built

LicketySync is in development, and there's nothing to download yet. Here's
where things stand:

- **Now:** the design is written down in detail, and the heart of it is
  tested on many simulated devices.
- **Next:** syncing between computers on the same network, from the command
  line.
- **After that:** finding each other across the internet, apps for computers
  and phones, and a version that runs in your web browser.

LicketySync is open source, under the Mozilla Public License 2.0. The code
and the design specifications are on
[GitHub](https://github.com/LicketySync).

## Before you trust it with your files

Every sync tool makes trade-offs, and we'd rather you hear about ours from us.
The [Risks](@/risks.md) page explains what each key holder can see, what
helpers on the internet such as relays learn and when a deleted file can come
back.
