+++
title = "Questions and answers"
description = "Quick answers to the questions people ask most, each with a link to the details."
+++

Short answers first. Each one links to the page with the whole story.

## The basics

### What is Lickety-Sync?

It keeps a folder the same on all your devices, and with the people you
choose. Your devices sync with each other, so there's no account and no
company cloud. More in [How it works](@/how-it-works.md).

### Can I download it yet?

Not yet. The design is written down and tested on many simulated devices, and
syncing between computers on one network comes next. There won't be test
versions (betas) before version 1.0, and we won't promise dates.
[Features](@/features.md#where-things-stand) shows the order things arrive in.

### Is there a cloud, or a server?

There's no server you didn't turn on, and no company holds your files. A few
helpers that setup will suggest turning on, such as public trackers and relays,
are other people's computers, and none of them can open your files. The
browser version will be the one exception (see the next question). See
[How it works](@/how-it-works.md#device-to-device).

### What about the browser version?

It will load from licketysync.app each time you open it, so that site will see
your address and when you open it. It isn't sent your keys or files, but a
changed copy could take them, so you'd be trusting the site to serve the real
one; the desktop app or a page-checking browser extension will be able to
check. See
[The browser version will trust its website](@/risks.md#the-browser-version-will-trust-its-website).

### Do I need an account?

No. There's nothing to sign up for. Holding one of a folder's keys is what lets
a device in. See
[How it works](@/how-it-works.md#a-folder-its-keys-and-your-devices).

### Is it open source?

Yes, under the Mozilla Public License 2.0. The code and the design
specifications are on [GitHub](https://github.com/LicketySync), and
[For IT and privacy reviewers](@/for-reviewers.md#the-specifications) points
to the details.

## Sharing

### How will I share a folder?

You'll give someone one of the folder's keys, as a link or a file, and in the
apps as a QR code. The key you pick decides what they can do: change
everything, see everything or keep an encrypted copy they can't open. See
[Sharing](@/sharing.md#three-keys-three-kinds-of-access).

### Is it safe to send a key by email or chat?

Not really. Whoever has a key has the access, so a key sent where others can
read it is as good as public. Hand it over in person (by QR code, once the apps
arrive), or over a channel you already trust. See
[Sending a key safely](@/sharing.md#sending-a-key-safely).

### Can I take back a key?

No. A key works for as long as the folder exists, and whoever has one can pass
it on. To remove someone, everyone who stays moves to a new folder with new
keys. The person removed keeps the old folder, its key and every file they
already had, and any device still on the old folder keeps syncing with them.
See [Removing someone](@/sharing.md#removing-someone).

### Can I share with people I've never met?

Yes: anyone with the folder's key can join, and once public trackers arrive,
their devices can look up yours if both have them on. Finding isn't always
reaching, though, and they'll see what any holder of that key sees: which
device changed each file and when, to the microsecond; your devices' names and
network IDs; and which files each device keeps. They can pass the key on, for
good. See [Sharing more widely](@/sharing.md#sharing-more-widely) and
[Use public trackers?](@/setting-up.md#use-public-trackers)

## Your files

### What happens if two of us edit the same file?

Both versions are kept: one as the file, and the other as a
[conflict copy](@/glossary.md#conflict-copy) beside it. Nothing is merged and
nothing is lost, and you delete the copy once you've sorted it out. See
[When two changes collide](@/how-it-works.md#when-two-changes-collide).

### I deleted a file by mistake. Can I get it back?

Usually, yes. When a deletion reaches your other devices, each one moves its
copy into its [archive](@/features.md#the-archive), where it waits 30 days
unless you change it. So a file you delete on your laptop can still be found on
your other devices.

### Can a deleted file come back?

Sometimes, yes. Devices forget old deletions a year or more after they arrive,
sooner if a device with the write key has a wrong clock. After that, a device
that kept an old copy, one that was away all that time say, can bring the file
back. It never costs you a file. See
[Deleted files can come back](@/risks.md#deleted-files-can-come-back).

### Who can see my files?

Only devices that hold the folder's write key or read key. A
[storage device](@/glossary.md#storage-device), with only the sealed key, keeps
an encrypted copy it can't open, though it sees the folder's shape: how many
files, roughly how big, and when and from which device they change. See
[Who sees what](@/privacy.md#who-sees-what).

### Can anyone tell what I'm sharing?

Nobody can read your files or their names without a write or read key, but
some things show. Anyone watching the network, and any relay you use, sees
which addresses talk, when and how much; with local network discovery on,
people there can see that your device runs Lickety-Sync; and, with the local
network or public trackers on, anyone who has had or seen one of a folder's
keys can tell where it's offered. Someone
running a storage device who knows some of the folder's files, or got files
of their choosing into it, can in time test whether other files they have are
in it. See [Who sees what](@/privacy.md#who-sees-what).

## Devices and networks

### What if I lose a device, or it's stolen?

Whoever has it can read whatever files are on it and, if they get past the
protection on its keys, use those keys for as long as the folders exist. For
each folder it held, the people who stay move to a new folder with new keys,
as they would to remove a person. See
[Removing someone](@/sharing.md#removing-someone) and
[A key is access, for good](@/risks.md#a-key-is-access-for-good).

### Will it work on my phone?

Not yet: the apps for computers and phones come later. A phone can be helped to
connect, but it never helps others as a relay or a hole-punch coordinator. See
[Features](@/features.md#apps).

### Will my company's network block it?

It might. Lickety-Sync connects device to device, and some networks don't
allow that. If your workplace has rules about tools like this,
[For IT and privacy reviewers](@/for-reviewers.md) is written for the people
who check them.

### What's "hole punching"?

A trick that lets two devices behind home routers reach each other directly: a
helper, the hole-punch coordinator, passes their first few messages, then both
knock at once. The coordinator never carries your files. With the recommended
settings it can be a stranger's public relay, which learns that your two
devices are connecting and when, and their addresses. See
[Hole punching](@/finding-each-other.md#hole-punching) and
[Who may help with a hole punch?](@/setting-up.md#who-may-help-with-a-hole-punch)

### Does it phone home?

The programs you install don't: they have no server of their own to call, no
telemetry (reports about how you use it) and no update checks. They reach out
only to other devices, and to the helpers your settings allow. The browser
version will load from licketysync.app each time you open it. See
[Setting up](@/setting-up.md#what-stays-the-same-whatever-you-choose).

### What do all these words mean?

The [Glossary](@/glossary.md) explains each one in a sentence or two.
