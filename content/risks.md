+++
title = "Risks"
description = "Where your risks are, how they would show up, how much they matter and what you can do."
+++

We'd rather you hear about the trade-offs from us. Here are the risks in
LicketySync's design, the big ones first, each with how much it matters and
what to do. There's nothing to download yet, so this is about the design.

{{ <illustration src="risks-umbrella.svg" alt="An umbrella keeping the rain off a folder, with a few drops falling around it: LicketySync is built to have your files’ back." width={360} height={240} /> }}

## The short version

- Give each key only to people and devices you'd trust with that folder for
  good, and hand it over in person or over a channel you already trust.
- Keep the archive on.
- Let every device set its clock from the network.
- Keep every device on the same release.
- When a device or a backup comes back after more than a year away, check
  what reappears.

## The big ones

### A key is access, for good

Whoever holds one of a folder's keys can use it on any number of devices, for
as long as the folder exists, and pass it on without asking. Keys never expire,
and a key sent by email or chat is as good as public. No key can be taken back:
to leave someone behind, or after losing a device, you all move to a new
folder, and they keep the old one.

{{ <lead_in text="How much it matters:" /> }} a key is the folder, for good.
{{ <lead_in text="What to do:" /> }} share keys only by QR code in person or
over a channel you already trust
([Sending a key safely](@/sharing.md#sending-a-key-safely)). To move,
follow [Removing someone](@/sharing.md#removing-someone).

{% <more summary="What a key lets its holder do, and what moving costs"> %}

- Nothing records who holds a key. The write key can give out any of the three
  keys, the read key the read and sealed keys, and the sealed key only itself.
- Changes made on a device that holds only the read key stay on it: every other
  device rejects a change the write key didn’t sign.
- Any key gives away the folder’s ID, a code that names the folder. With it,
  someone can check whether the folder is on a local network they join, and
  find, through public trackers, the addresses of the devices that offer it.
- An invite that carries your addresses gives them away, along with the
  hole-punch coordinators your device uses (the helpers that introduce two
  devices so they can connect directly). Invites for the browser version, and
  one-time links for sending a file, will always carry them.
- The write key can also be shown as 24 words that look just like a
  cryptocurrency wallet’s recovery phrase, so programs that hunt for wallet
  phrases will take them too. Keep them as secret as a wallet phrase, and type
  them nowhere else.
- Whoever has a lost or stolen device can read the files it held, and, past its
  key store’s protection (your system’s keychain, or a passphrase), use every
  folder key it held, for good. They get no other folder’s keys. Any program
  running as you, malware included, can read what you can, keys too.
- Moving to a new folder, step by step, and what it doesn’t do, are in
  [Removing someone](@/sharing.md#removing-someone). Do it for every folder a
  lost device held. Restoring from the archive comes with the archive, later.
- A device that misses the move keeps syncing with them, and until your device
  unlinks the old folder, they can still reach it through hole punches.

{% </more> %}

### The people you share with see more than your files

Everyone with the read or write key sees which device wrote each version and
when, to the microsecond, offline edits included, and which files each device
keeps. Every device in the folder learns the others' names and addresses, and
keeps what it already had, archive included.

{{ <lead_in text="How much it matters:" /> }} someone with a read key could see
that you edit late at night. {{ <lead_in text="What to do:" /> }} keep files
whose timing you want private in a folder of their own.

{% <more summary="More of what they see"> %}

- Every device in the folder, whatever its key, learns the device IDs and names
  of the others, and your device’s hole-punch coordinators.
- Key holders can test whether a file they guess at is in the folder, and see
  roughly when deletions were forgotten.
- A change names a device, never a person, and a write key holder can sign a
  change in any device’s name, so nothing shows who made a change.
- Each device keeps its own archive, for as long as its person chooses, so a
  file you change or delete stays in the archive of every other device that
  had it. On a device that unlinks the folder, its files stay as ordinary
  files.
- Keep keys to a private folder on your own devices only, and give your devices
  names you don’t mind others seeing.

{% </more> %}

### Storage devices see the shape of your folder

A storage device holds only the sealed key: a home server, say, that keeps an
encrypted copy. It can't read your files or their names, but it sees:

- how many files there are, and roughly how big;
- when they change, and from which device;
- the names and addresses of the devices it meets.

Someone running one who knows some of the folder's files, or got files of
their choosing into it, can in time test whether other files they have are in
it.

{{ <lead_in text="How much it matters:" /> }} the pattern of your activity
shows. {{ <lead_in text="What to do:" /> }} give a sealed key only to storage
you're content to let see
[everything on this list](@/for-reviewers.md#what-a-storage-device-sees).

### Someone with the write key can delete everything

Deletions from other devices skip the deletion brake, the pause that asks
first when many files vanish at once from your disk. So one write key holder,
or a stolen write key, can delete the whole folder everywhere. The archive
keeps what other devices' changes replace or delete, 30 days unless you change
it.

{{ <lead_in text="How much it matters:" /> }} serious, but the archive has the
files. {{ <lead_in text="What to do:" /> }} keep the archive on, and give the
write key only to people you'd trust with every file.

{% <more summary="What else a write key holder can do"> %}

- Fill the folder with changes, clashing versions and pieces of files, which
  every device keeps until someone writes over them.
- Lock the folder for everyone, by numbering a change just before the point
  where version numbers stop, 10 000 years after 1970. The folder then has to
  be shared again as a new one.
- Make every device forget every deletion at once, which brings deleted files
  back wherever a copy survives.
- Sign changes that no release can read, which moves those files into each
  device’s archive with a warning ([Rarer things](#rarer-things)).

{% </more> %}

### Deleted files can come back

A deletion is remembered for a year or more, sooner if a device with the write
key has a wrong clock, then every device forgets it together. After that, any
device that kept an old copy, such as one back from a long time away, can bring
the file back. If a device with the write key has its clock set a year or more
ahead, every device forgets every deletion at once. Files come back; none is
lost.

{{ <lead_in text="How much it matters:" /> }} a surprise, never a loss.
{{ <lead_in text="What to do:" /> }} let every device set its clock from the
network, delete again what comes back and check what reappears when a device
returns after a year away.

{% <more summary="How forgetting works"> %}

Each device with the write key counts the year by its own calendar, from when
the deletion reached it, then signs a short note, and every device that gets
the note forgets the same deletions. Storage devices can bring such a file back
too. A returning file comes back as itself, or as a conflict copy beside a
newer file of the same name. A file you deleted on a device that then stayed
offline for more than a year comes back on that device when it reconnects, if
forgetting has passed its deletion by then. A deletion that arrives late holds
back the forgetting of later ones, and someone who wants deletions kept can
hold forgetting back, which costs only storage. A clock that jumps ahead can
come from a mistyped year, a machine whose clock is wrong when it starts or
someone with no key who fakes the time a device takes from the network; nothing
guards against it. The mechanism, and what it costs, is in
[Forgetting deletions in detail](@/for-reviewers.md#forgetting-deletions-in-detail).

{% </more> %}

### Files that belong together sync one by one

A git repository, a database or a password manager's vault syncs file by file,
so another device can see a mix of old and new files that no device ever had.

{{ <lead_in text="How much it matters:" /> }} it can leave the set broken on
the other device. {{ <lead_in text="What to do:" /> }} don't use such a set on
two devices at once. Close it on one, and open it on another only once that
device shows the folder up to date with the first.

### The browser version will trust its website

The browser version, when it comes, will load from licketysync.app each time
you open it. If that site were ever broken into, it could hand out a changed
version that leaks keys and files, and keys taken that way stay usable for
good.

{{ <lead_in text="How much it matters:" /> }} serious, for the browser version
only. {{ <lead_in text="What to do:" /> }} for the folders that matter most,
use the app on your own computer.

{% <more summary="How you’ll be able to check"> %}

A broken-into site could serve the right version to people it expects to
check, and the changed one to everyone else; the folders whose keys it took
would have to move to new ones. Each release will publish a signed list of the
browser version’s fingerprints, which the desktop app and page-checking
browser extensions will be able to check.

{% </more> %}

## Who can see that you use it

Anyone watching the network sees which addresses talk, and if you take the
recommended settings, anyone on your local network, or who reaches the port
your router opens, can see that your device runs LicketySync, and anyone who has had or
seen a folder's key can find the addresses that offer it.
[Privacy](@/privacy.md#what-no-peer-to-peer-program-can-hide) says what else
shows, and [How devices find each other](@/finding-each-other.md) what each way
of connecting gives away.

## Rarer things

Two edits made at once leave a conflict copy, and copies can pile up where many
devices write one file.

{% <more summary="Conflict copies"> %}

When two devices change a file before either sees the other’s change, both
versions are kept: one as the file, the other as a conflict copy beside it.
Nothing is merged, and an edit beats a delete. An edit made while a newer
version is still downloading is a conflict too. Saving the file again turns
each copy into an ordinary file, which stays until someone deletes it; a log
that four machines add to at the same moment every hour can leave about 26 000
copies a year. Each version remembers only the 32 devices that wrote the file
last, so a device away while more than 32 others changed it comes back with a
conflict copy. Only the latest versions are kept for syncing; older ones
survive only in the archive. {{ <lead_in text="What to do:" /> }} settle and
delete conflict copies, and give each machine its own file to write
([When two changes collide](@/how-it-works.md#when-two-changes-collide)).

{% </more> %}

Relays and hole-punch coordinators, the helpers setup will suggest using, see
both devices' addresses, never your files.

{% <more summary="What helpers see, and what hole punching costs"> %}

- A relay passes the encrypted traffic between two devices that can’t reach
  each other. It sees how much they send, and can slow, drop or cut the
  connection. Every relay is treated as a stranger’s, your devices use relays
  only once you turn them on, and setup will never make your device a relay
  for others.
- A hole-punch coordinator passes the first few messages of a punch, never
  your files; one that lies only makes the punch fail. Punches need a helper: a
  computer whose person turned on helping others with them (a relay also
  helps), and that the folder's “Who may help with a hole punch?” setting
  allows. Setup will suggest the widest setting, so a stranger's public relay
  can help, and on computers it will suggest helping the devices you share a
  folder with, never anyone else; the devices it helps then pass your
  computer's address and network ID on to the devices they share folders
  with. Every helper used learns that your two
  devices are connecting and when, and their addresses, never file data.
- The devices you share folders with, and anyone who gets an invite with your
  addresses, learn which coordinators your device uses.
- A person you left behind can still punch through to your device until the old
  folder is unlinked on it, or while they share another folder with it.
- In a few unusual setups a punch can reach a private network; a device yours
  admitted can make it send a few short bursts of packets to addresses it
  picks; and someone your device refuses can keep it from being reached
  through the coordinators they know.

{{ <lead_in text="What to do:" /> }} say no to relays and hole punching in
setup if you don’t want them, or set
[Who may help with a hole punch?](@/setting-up.md#who-may-help-with-a-hole-punch)
to Nobody for a folder. The numbers are in
[Hole punching in detail](@/for-reviewers.md#hole-punching-in-detail).

{% </more> %}

On networks that block ordinary connections, devices can fall back to older
encryption, which a future quantum computer could open.

{% <more summary="Older encryption"> %}

It is a web-style connection (WebSocket), and anyone who blocks the other ways
between two devices can force them onto it. A recording of it would show a
future quantum computer what a storage device sees, never your files or their
names. A connection through a relay or a hole-punch coordinator with the
browser version at one end has the same weakness, and could show the folder’s
ID. A fix for the web-style connection is planned.

{% </more> %}

A storage device can hold back data, keep old copies or bring back forgotten
files, though it can't read or sign anything.

{% <more summary="What a storage device can do"> %}

- Go offline, or serve slowly.
- Show an out-of-date copy to a device on its first sync, until that device
  meets an up-to-date one.
- Bring back a file whose deletion was forgotten, or slow forgetting down if
  it’s a device’s only link to the others.
- Make a folder deletion take a renamed file, by holding back its new name.
- Keep old copies until a device with the write key meets it and tidies up;
  its status will show how many extra versions it holds.
- Send another storage device old versions it kept, which that one keeps too,
  or an old deletion, which makes it drop a file that came back, until a device
  with the write key meets it. Anyone with the read key can do the same.
- Send an old broken change again, which makes a file that came back leave the
  disks of the devices that read the folder, into their archives, with a
  warning.

None of it loses a file while the archive is on.
{{ <lead_in text="What to do:" /> }} let a device with the write key meet each
storage device now and then.

{% </more> %}

A newer release, or a broken change, can move a file into your device's
archive, with a warning, until it's fixed.

{% <more summary="A version your device can’t read"> %}

It happens when such a version, from a newer release or a write key holder,
replaces every version of the file your device has; on a computer the warning
will also show in the status bar and as a notification. The archived copy is
kept until it’s fixed, past the archive’s usual time and even with the archive
off: until a save from a device that had the old version, or an update that
reads the change. An edit made elsewhere brings the file back, but leaves the
warning. Once you save on that device, the copy goes when the archive’s time is
up, at once with the archive off, so restore from it first. A device with only
the read key can’t save, and waits for one with the write key. A change of a
kind no release knows stays beside the file for good.
{{ <lead_in text="What to do:" /> }} keep every device on the same release,
restore what you want from the archive and save the file again from a device
with the write key.

{% </more> %}

In a few rare cases, one version replaces another as if it were newer, and the
old one goes to the archive.

{% <more summary="A version replaced by mistake"> %}

- Two machines that run from one copy of a device’s LicketySync data, such as
  clones of one disk image.
- A device restored from a backup while its clock reads earlier than its last
  change before the backup.
- A folder deleted on one device that also takes a file it hadn’t received yet,
  usually after a rename inside it, when the old name’s removal arrived before
  the new name.

{{ <lead_in text="What to do:" /> }} never run two machines from one copy of a
device’s data, check a restored device’s clock and let it sync before anyone
edits on it, and keep the archive on.

{% </more> %}

Smaller clock slips can bring a file back early, or keep a device from being
heard on the local network or found through public trackers.

{% <more summary="Clock edge cases"> %}

- A write key device whose clock runs fast forgets deletions early, by as much
  as it gained. A clock off by the same amount all year counts the year right,
  and one set back forgets late.
- A device whose clock reads far behind, and that has seen little of the
  folder, has its own deletions undone.
- A device whose announcements on the local network are more than a few
  minutes off is ignored there, though it can still connect by address or
  through other devices.
- A device whose clock is more than a day off can't find others, or be found,
  through public trackers.
- A version’s time is its writer’s clock reading, so a wrong clock shows wrong
  times for its own versions only.

More is in [Clocks in detail](@/for-reviewers.md#clocks-in-detail).

{% </more> %}

A damaged entry on a device's disk is fetched again from other devices, but a
change only that device had is lost.

{% <more summary="Damage on disk"> %}

Each time a folder opens, every saved entry is checked again, and the folder
will show Checking meanwhile, for longer the more it holds.
{{ <lead_in text="What to do:" /> }} let each device sync soon after you change
files on it.

{% </more> %}

And a few small things you might notice, none of which loses a file.

{% <more summary="Smaller things"> %}

- Some old deletion entries linger until forgotten, such as two deletions of
  one folder made at once on two devices.
- A broken entry from a write key holder stays on every device, with a
  warning, until someone saves that file again; at a file nobody saves again,
  or in a broken clean-up note, it stays for good.
- A device on an older release keeps and passes on a newer release’s version,
  and will show it as an update it can’t read.
- Freed space may wait for the next round of syncing, which can mean until
  devices reconnect. This may change.
- Today’s engine holds at most 256 folders on a device, and syncs at most 64
  folders with any one device; past that, a message will say which are left
  out.

{% </more> %}

## Where the details live

The [threat model](https://github.com/LicketySync/core/blob/main/spec/threat-model.md)
has these risks in full, and the
[changelog](https://github.com/LicketySync/core/blob/main/CHANGELOG.md) the
limits of today's engine;
[For IT and privacy reviewers](@/for-reviewers.md#the-main-residual-risks)
explains them precisely. This page changes when the design does.
