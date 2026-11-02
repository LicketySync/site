+++
title = "Risks of using Lickety-Sync"
+++

Lickety-Sync is in development, and there is nothing to download yet. This page
lists the risks of its design, as its
[specifications](https://github.com/LicketySync/core/tree/main/spec) state them
today, so that you can judge them before you trust it with your files. Each
risk says what it means for you, why the design accepts it, and what you can do
about it. Some of these are choices the project's owner made on purpose; each
one gives the reason and the cost. These are the main risks the design knows
of; the threat model, linked at the end, lists every one.

**Words used on this page.** A **version** is a file's content, or its
deletion, as one device saved it. A **record** is the signed entry that
describes one version in a folder's **index**, the list of records each device
keeps. A **fingerprint** is a short hash that identifies one record. A
**storage device** is a device that holds only a folder's sealed key. A
**cleanup** is a signed list, sent by a device with the write key, of records
that storage devices should forget. A **relay** is a device that passes
encrypted traffic between two devices that cannot reach each other. Your own
devices may relay for each other; no other relay is used unless you turn it
on. The **archive** is where a device keeps the files that other devices'
changes replaced or deleted.

## Holding a key is the access

Every folder has three keys: a write key (read and change the folder), a read
key (only read it) and a sealed key (store it without being able to read it).
Whoever has a key has that access, on as many devices as they like, for as long
as the folder exists.

- Keys never expire. Nobody issues them, and nothing records who holds one.
- A write key holder can give out any of the three keys. A read key holder can
  give out the read and sealed keys. A sealed key holder can give out the
  sealed key. None of this needs anyone's approval.
- Each record names a device, not a person, and a write key holder can name
  any device.

**There is no way to take a key back** (the owner's decision, 2026-10-18). To
remove someone, or a lost or stolen device, the people who stay move to a new
folder with new keys (the owner's answer of 2026-10-22):

1. Pause the folder on every device that stays, so that nothing more arrives
   from the person being removed. If they are leaving on good terms, you can
   first let every device finish syncing, so that their copies match.
2. Nothing shows which changes were theirs. On the device that will share the
   new folder (step 4), check what changed since you last trusted them, and
   restore from its archive what they changed or deleted.
3. On every device that stays, stop syncing the folder and keep its files.
4. On one of them, share that copy as a new folder. Give its new key to the
   others yourself, outside the app.
5. Each of the others joins the new folder. Joining over its own copy keeps
   the files that match; any file that differs becomes a conflict copy, and a
   file the shared copy no longer has comes back on every device. To avoid
   that, join into an empty directory instead and let the files download.

For a lost device, do this for every folder it held. What it means for you:

- The removed person keeps the old folder, its key and every file they already
  had.
- A device that misses step 3 keeps syncing with them.
- The old folder's history does not move to the new one.
- Storage devices store the new folder from scratch. Their copy of the old
  folder goes only when the old folder is unlinked on each of them.

The reason: it is the model Resilio Sync uses, and it replaced an earlier
design in which keys were issued to each person and could be revoked. In the
owner's words, "Simpler is better", and someone who had access to the files
"could still cause a plethora of problems" even if their key were revoked:
revoking a key would not take back the files a person already has. The cost is
that you cannot cut someone off from what they already have, and removing them
takes the steps above on every device.

What to do: give a key only to people and devices you would trust with that
folder for good.

## A key sent by email or chat is a leaked key

A key is all an invite is. Whoever reads a key, in an email, a chat or a photo
of a screen, can join at that key's level, from any number of devices, for as
long as the folder exists. There is no expiry, no use count and no approval
step. With the folder's ID, which any of its keys gives, they can also check
whether the folder is on any local network they join and, if you use the
opt-in internet discovery (the DHT), watch which addresses announce it.

The reason: the key is the access, with nothing in between (above). The program
cannot protect a key after a person sends it.

What to do: pass keys by QR code in person, or over a channel you already
trust. If a key leaks, move to a new folder as above.

## What each key holder can see and do

### The write key

A write key holder sees everything in the folder: every file, every name, every
directory. It also sees when each version was written, to the microsecond, and
by which device (see "Write times are recorded to the microsecond").

It can change or delete anything. Deletions that arrive from other devices are
applied without the deletion brake (the pause that asks you first when many
files vanish at once from your own disk), however many there are. So one write
key holder can delete the whole folder on every device. The reason is the
owner's answer of 2026-10-22: the archive is what keeps such files. The design
keeps them there for 30 days by default.

It can also sign a record under any device ID, its own or another's. So the
device a record names does not prove which device wrote it.

What to do: keep the archive on, and give the write key only to people you
would trust with every file in the folder.

### The read key

A read key holder sees everything a write key holder sees, write times and
device IDs included: it can read everything in the folder, now and later,
until you move to a new folder. It cannot change anything: every device
rejects a record that the write key did not sign. Edits made on a read-only
device are not sent to anyone; what the app shows for them is still being
designed.

The reason: the read key carries the folder's data key, which stays the same
for the folder's whole life, so there is nothing to take back short of a new
folder.

### What write and read key holders also see

Holders of the write or read key also see every other device's ID, name, the
key level it reports and its clock, to the day; which files each device keeps
locally; and they can test whether a file they guess at is in the folder.

What to do: treat a read key like the folder itself. Anyone who has it can
read everything in the folder, now and later, until you move to a new folder.

### The sealed key

A sealed key is for a storage device: a NAS or a friend's server that holds a
full encrypted copy, checks it and serves it to others without being able to
read it. It cannot read file contents, names, the folder's layout, dates or
exact sizes. By the owner's decision (2026-10-25), storage devices see almost
nothing; this is everything they can see:

- an opaque number for each file, so how many files there are and how often
  each one changes;
- for each record, the fingerprints of the records it replaces, so where a
  file's history split into conflicting versions and joined again;
- the IDs of each version's pieces and their rounded sizes, so each version's
  size to within a few percent, which pieces are the same across files and
  versions, and from that a copy or a rename;
- that a version with no pieces is a deletion, a deleted folder, a directory
  or an empty file, without telling which;
- the size class of each record's encrypted details, rounded up to a power of
  two (256, 512, 1 024 or 2 048 bytes and up), which says only whether its
  name, its pieces and the devices that wrote it together are small or large
  (a design choice the owner has yet to review);
- which records were cleaned up together, and so how many files a deleted
  folder held; and, from some cleanups, that certain records were folder
  deletions, or deletions more than a year old (a design choice the owner has
  yet to review);
- when each record arrives and from which device, which usually shows who
  wrote it, and so the times and rate of activity;
- the folder's ID, and the device IDs, device names and clocks (to the day) of
  the devices it connects to, with the key level each one reports;
- what each device reports about the folder: how many records and extra
  versions it holds, how many pieces it stores, how much it still has to fetch,
  whether it is paused and how much free disk space it has; which pieces each
  device holds; and the folder's approximate total size.

**Known files.** Pieces are cut at places only key holders can work out. But
someone who knows a few of the folder's files, or can get you to save one, can
work out where pieces are cut, and from then on test, from the sizes of the
pieces, whether any file they have is in the folder, for as long as the folder
exists. Which way of cutting pieces to use is still an open design question.

**What a storage device can do.** Anyone with the sealed key can:

- bring back, on every device, a file whose deletion has been forgotten after
  the year (see "Deleted files can come back after a year");
- make a folder deletion take a renamed file that the deleting device never
  saw, by holding back the file's new name while passing on the removal of its
  old name (the file goes to the archive);
- keep versions dated more than a year ahead in circulation, which devices
  that can read the folder refuse until the real date comes within a year of
  them;
- hold back data, go offline or serve slowly;
- show an out-of-date copy of the folder to a device on its first sync, which
  that device notices as soon as it meets an up-to-date one;
- keep old copies, and fill other storage devices with them (see "Smaller
  things you might notice").

It cannot read anything, sign anything, or make a device that can read the
folder drop a version on its own say-so.

The reason: a storage device must check, deduplicate and serve your data
without reading it, and to do that it has to see the list above.

What to do: give a sealed key only to storage you are content to let see the
list above, and let a device with the write key meet each storage device now
and then.

### Anyone who connects, or watches

- A device that connects to yours, even with no key, learns a stable identity
  for your device (it can recognise your device again on any network and in
  any folder), its clock to the day, and roughly how many folders it holds. It
  also sees your address.
- Anyone on your local network can see that your device runs Lickety-Sync.
  Anyone there who knows a folder's ID, which any past or present key holder
  does, can confirm that the folder is announced there. And anyone there who
  replays a device's announcement can see which devices answer it, and so
  which of them hold that folder.
- With the opt-in internet discovery (the DHT), anyone who ever knew the
  folder's ID, past key holders and relays the folder used included, can watch
  which addresses announce it, day by day.
- A relay sees both devices' addresses and the folder's ID, but not what
  passes through it. Your own devices may relay for each other; no other relay
  is used unless you turn it on.
- Someone watching the network sees which addresses talk, when and how much,
  but not what.
- Connections over the WebSocket fallback, used on locked-down networks, use
  classical encryption; a fix is planned for phase 2. Anyone who blocks UDP
  and raw TCP between two devices can force them onto that path. Someone who
  records such connections now and later has a quantum computer could read
  from them what a storage device sees.

The reason: a system with no servers cannot hide that it runs, or who it talks
to.

What to do: leave internet discovery off unless you need it; it is off by
default.

## Write times are recorded to the microsecond

Each version of a file records when it was written, to the millionth of a
second, and which device wrote it. Every holder of the read or write key can
see this for every version, including edits made offline, and which devices
wrote the file before. Storage devices cannot read these times, though they see
when records arrive.

This is the owner's choice (2026-10-25). In the owner's words: "I chose this
because I can work on files in multiple locations nearly simultaneously and the
per-minute version resolution is not accurate enough." And: "Make sure this is
documented - do not hide this detail."

What it means for you: the contents of your files stay private to key holders,
but when you work on them, and on which device, does not. A person you give a
read key to can see, for example, that you edit files late at night, and from
which of your devices.

What to do: keep files whose timing you want private in a folder of their own,
and give its keys only to devices of yours.

## The one-year clock rule

The owner's rule (2026-10-25), in the owner's words: "Refuse to sync with
clients that are more than a year out of date. Document this decision as it has
real impact for people's deleted files and for trying to settle the sync'd
copies. A solution would be to stop synchronizing a folder, fix the time, and
then start synchronizing again."

What the rule does:

- Two devices whose clocks differ by more than a year do not sync a folder
  together. Each shows a message for that folder.
- A device that can read the folder also refuses any single version dated more
  than a year after its own clock, even when another device passes it along.
  (This check of single versions is how the design applies the owner's rule;
  the owner was shown it and kept the one-year limit.)
- A folder whose saved records are dated more than a year ahead of the
  device's clock stays closed and syncs nothing. That happens when the clock
  was set back, or was ahead when the records were written or arrived. The
  folder opens by itself once the clock is within a year of those records, or
  when you add the folder again. It is kept closed rather than throwing those
  records away, because one of them could be your own edit that no other
  device has yet.
- None of these messages can tell which clock is wrong. Compare your device's
  clock with a trusted one.

**If the clock was only behind,** set it right. The folder opens by itself, and
nothing below applies.

**If the clock was ahead, and the device wrote while it was,** the remedy is on
that device: stop syncing the folder there (remove it and keep its files), set
the clock right, then add the folder again with the same key.

What adding the folder again costs. Added again over its old copy, the device
starts its index afresh and treats every file there that differs from what the
other devices hold as a new version beside theirs:

- files edited on that device while its clock was ahead show up as conflict
  copies, to settle by hand;
- so do the files the other devices changed while it was cut off, since its old
  copies differ from their newer versions;
- files the other devices deleted meanwhile come back on every device, because
  an edit beats a delete;
- files deleted on that device while its clock was ahead come back from the
  other devices.

Nothing is overwritten then. Later, versions that device wrote while its
clock was ahead, and that other devices kept, can come back once the real date
reaches them. One of them can even replace a later edit of the same file,
which then goes to the archive.

What to do instead: let the other devices finish syncing, move the old copy
aside, and add the folder again into an empty directory, so that it syncs fresh
from the others. Then compare with the old copy by hand: copy back any edit of
yours that the others never received, and check what reappears. This does not
stop the later return of versions the device wrote while its clock was ahead.

What the rule lets through:

- A clock up to a year ahead is allowed. Its records move the folder's own time
  forward (see "Deleted files can come back after a year"), so deleted files
  can come back up to that much early. They come back; they are not lost. A
  warning shows when a folder's time is more than a day ahead of your device's
  clock.
- A clock up to a year behind is allowed too. A device whose clock is behind,
  and that has seen little of the folder, dates its first deletions in the
  past, so they are forgotten up to that much early, and the files come back.
- A fast clock can cut off a slow one through a correct one. Once a device with
  the right time takes a version from the fast device, everything it writes is
  dated ahead too, and a device whose clock is behind may refuse it for up to a
  year.
- A device with no clock set, such as a Raspberry Pi that started without a
  network and has no battery for its clock, cannot sync until its clock is set.

Why the rule exists: a version dated far in the future would make every device
forget every deletion at once.

What to do: let every device set its clock from the network. If you see the
clock message, find out which clock is wrong and follow the steps above for it.

## Deleted files can come back after a year

A deletion is remembered for 365 days, or less if a clock was off. The year
is counted in the folder's own time: the latest time stamped on any record a
device holds, not any one device's clock. So every device that holds the same
records forgets the same deletions. A folder nobody writes to forgets nothing.

After that year:

- A device that comes back still holding an old copy of a deleted file brings
  it back as a live file, or as a conflict copy beside a newer file with the
  same name.
- A file you delete on a device that then stays offline for more than a year is
  not deleted elsewhere: the deletion arrives too late, and the file comes back
  on that device.
- Any device that kept an old copy, whatever key it holds, can bring it back
  whenever it likes.

This is the owner's decision (2026-10-25): the horizon is fixed at a year, the
same on every device, and old files come back rather than being treated as
deleted. The reasons: the index must not grow for ever, and in the owner's
words, "I can't let indexes grow indefinitely"; a deletion that is never
forgotten would stay in it for good. A horizon that each device set for itself
would leave devices disagreeing about which deletions exist. And treating
returning files as deleted would need to know when each pair of devices last
met, and could send live files to the archive by mistake. The cost is a file
you deleted reappearing. It is never a loss.

What to do: delete it again. Do not leave a device or a backup of a folder
unsynced for more than a year and then bring it back without checking what
reappears.

## Concurrent edits become conflict copies

When two devices change the same file before either has seen the other's
version, both versions are kept. The file shows one of them, and the other
appears beside it as a conflict copy. In the current design a copy is named
like `report.txt (conflict 1a2b3c4d)`. Contents are never merged (the owner's
decision, 2026-10-25), so each version stays exactly as it was saved.

- An edit beats a delete. If one device deletes a file or a folder while
  another edits a file in it, the edit survives.
- An edit made while a newer version is still downloading is a conflict, not
  an overwrite.
- When someone saves the file again, each copy becomes an ordinary file of its
  own, which stays until someone deletes it. Copies pile up where many devices
  write one file at once: a log that four machines append to at the same moment
  every hour can leave up to three copies an hour, about 26 000 a year, and,
  for a 10 MB log, about 300 MB of index a year until they are deleted.
- The index keeps only the latest versions of each file, not their history.
  Older versions survive only in each device's archive.
- Each version remembers at most the 32 devices that wrote the file most
  recently. A device that was away while more than 32 others changed a file,
  or fewer when conflicting versions filled the 32 places, comes back with a
  version that nothing marks as old, so it becomes a conflict copy. Nothing is
  lost.

The reasons: keeping only the latest versions, and at most 32 devices for each,
keeps the index from growing for ever. The owner chose a count over a time
window. The smaller count considered, 16, would already fill up with two
people's devices and a small team; 32 entries take at most about 1.5 KB.

What to do: delete conflict copies once you have settled them. Give each
machine its own file to write rather than one file that many write at once.

## Rare cases where a version is replaced by mistake

In a few cases one version can replace another as if it were newer. The
replaced file goes to the archive.

- Two machines that share one device ID, such as machines cloned from one disk
  image.
- A device restored from a backup while its clock reads earlier than its last
  write before the backup.
- A folder deleted on one device that also takes a file it had not yet
  received. The usual case is a rename inside that folder: the new name and the
  removal of the old name travel separately, and only the removal had arrived.
- Versions written while a clock was more than a year ahead (see "The one-year
  clock rule").

The reason: the rename case is the price of one record per deleted folder
rather than one per file, and the others break what the design assumes about
a device: one machine per device ID, and a clock that never reads earlier than
the device's last write. The threat model states each one.

What to do: never run two machines from one copy of a device's Lickety-Sync
data. Check a restored device's clock, and let it sync with the others before
anyone edits files on it. Keep the archive on.

## Files that belong together sync one by one

A git repository, a database such as SQLite, a password manager's vault, or any
set of files that must change together syncs file by file. Another device can
see a mix of old and new files that no device ever had on its disk.

The reason: Lickety-Sync syncs files, and nothing ties several files' changes
together.

What to do: do not use such a set on two devices at once. Close the program on
one device, and open it on another only once that device shows the folder up
to date, having synced with the device you closed it on.

## Smaller things you might notice

- **Old deletion records linger.** Some deletion records stay until their year
  is up or a cleanup removes them, for example one of two deletions of the same
  folder made at the same time on two devices. They take a little space.
  Nothing to do.
- **A broken record stays until the file is saved again.** If a write key
  holder sends a record that devices cannot read or that breaks a rule, every
  device keeps it, and shows a warning that saving the file again clears it.
  At a file nobody saves again it stays for good, and so does a broken cleanup.
  What to do: save the file again.
- **A newer release's versions.** A device on an older release keeps and
  passes on a newer release's version that it cannot read, and shows it as an
  update it cannot read. If every version of a file is of that kind, it leaves
  the file on its disk as it is, so two such devices can show different
  content there. What to do: keep every device on the same release.
- **Storage devices keep old copies.** A storage device replaces a version
  only when a newer record names it, and otherwise keeps both. Its old copies
  stay until a device with the write key meets it and sends a cleanup. A
  storage device that only read-only devices visit keeps them. Its status shows
  how many extra versions it holds. What to do: let a device with the write key
  meet it now and then.
- **A storage device can be filled with old copies.** Someone with the sealed
  or read key who kept old versions can send them all to another storage
  device, which keeps them until a device with the write key meets it. What to
  do: let a device with the write key meet it now and then.
- **A storage device can lose a returned file for a while.** Someone with the
  sealed or read key can make another storage device drop a file that came
  back after its deletion was forgotten, by sending it the old deletion. The
  file returns there when a device with the write key next meets it, and
  devices that can read the folder keep it throughout. Nothing to do.
- **Freed space can wait** (an open design question). Pieces that no file
  uses any more are deleted at the end of the next round of syncing. While
  every connection is already up to date, that round may not come until the
  next reconnect. Nothing to do.
- **Checking at start.** When a folder opens, every saved record is checked
  again, and the folder shows Checking meanwhile. This takes time in proportion
  to the number of records the folder holds. The owner chose this (2026-10-25)
  because it catches records damaged or changed on disk with no extra secret to
  keep. Nothing to do.
- **Damage on disk.** A device drops saved records that are damaged and fetches
  them again from other devices. A change that only that device had is lost.
  What to do: let each device sync with another soon after you change files
  on it.
- **At most 256 folders on a device** (a limit of the current engine). What
  to do: keep to 256 folders on one device.
- **At most 64 folders with one device.** The protocol opens at most 64
  folders on one connection, and the current engine makes one connection to
  each device. If you share more than 64 folders with one device, only 64 of
  them sync with it, and a message says so. What to do: share at most 64
  folders with one device.
- **A folder that reopens can wait for a reconnect** (an open design
  question). A folder kept closed by the clock rule that opens again may sync
  with a device it is already connected to only after they next reconnect.
  Nothing to do.
- **A write key holder can lock the folder.** A write key holder can sign a
  record dated near the end of the range the design allows, which makes the
  folder unwritable for everyone until it is shared again as a new folder. Only
  a write key holder can do this, and one can already delete everything. What
  to do: if it happens, move to a new folder as described under "Holding a key
  is the access".

## Where this comes from

The points above are stated, with more detail, in the
[threat model](https://github.com/LicketySync/core/blob/main/spec/threat-model.md)
and the
[protocol specification](https://github.com/LicketySync/core/blob/main/spec/protocol.md)
(its last section explains the index in plain words). The limits and open
questions of the current engine marked above are in the project's
[changelog](https://github.com/LicketySync/core/blob/main/CHANGELOG.md). This
page changes when the design does.
