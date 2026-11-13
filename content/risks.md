+++
title = "Risks of using Lickety-Sync"
+++

Lickety-Sync is in development, and there is nothing to download yet. This page
lists the risks of its design, as its
[specifications](https://github.com/LicketySync/core/tree/main/spec) state them
today, so that you can judge them before you trust it with your files. The
current code does not do all of it yet: until the change that forgets deletions
by a signed message lands, it still refuses to sync with devices whose clocks
are more than a year apart. Each risk says what it means for you, why the design
accepts it, and what you can do about it. Some of these are choices the
project's owner made on purpose; each one gives the reason and the cost. These
are the main risks the design knows of; the threat model, linked at the end,
lists every one.

**Words used on this page.** A **version** is a file's content, or its
deletion, as one device saved it. A **record** is the signed entry that
describes one version in a folder's **index**, the list of records each device
keeps. A **fingerprint** is a short hash that identifies one record. A
**storage device** is a device that holds only a folder's sealed key. A
**cleanup** is a signed list, sent by a device with the write key, of records
that storage devices should forget. A **relay** is a device that passes
encrypted traffic between two devices that cannot reach each other, without
being able to read it. Every relay is a public one, which may belong to anyone.
No relay is used, and your devices relay for no one, unless you turn that on.
Any of your computers can be set up as a public relay, with limits on rate and
open connections, and a minimum transfer rate below which a connection can be
dropped (the owner's decision, 2026-11-03). The **archive** is where a device
keeps the files that other devices' changes replaced or deleted. A
**hole-punch coordinator** is a computer that helps two devices connect
directly (see "Relays and hole-punch coordinators are strangers").

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

Holders of the write or read key also see every other device's ID, name and
the key level it reports; which files each device keeps locally; and they can
test whether a file they guess at is in the folder.

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
  (the owner's decision, 2026-11-02);
- which records were cleaned up together, and so how many files a deleted
  folder held; and, from some cleanups, that certain records were folder
  deletions, or deletions that a watermark has passed (see "Deleted files can
  come back"), and, from a cleanup that names nothing, when a device with the
  write key signed a watermark (the owner's decision, 2026-11-02);
- when each record arrives and from which device, which usually shows who
  wrote it, and so the times and rate of activity;
- the folder's ID, and the device IDs and device names of the devices it
  connects to, with the key level each one reports;
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
  the year (see "Deleted files can come back");
- make a folder deletion take a renamed file that the deleting device never
  saw, by holding back the file's new name while passing on the removal of its
  old name (the file goes to the archive);
- slow down the forgetting of deletions, if it is a device's only link to the
  others (see "Forgetting can come late"), which costs storage and never a
  file;
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
  any folder), and roughly how many folders it holds. It also sees your
  address.
- Anyone on your local network can see that your device runs Lickety-Sync.
  Anyone there who knows a folder's ID, which any past or present key holder
  does, can confirm that the folder is announced there. And anyone there who
  replays a device's announcement can see which devices answer it, and so
  which of them hold that folder.
- With the opt-in internet discovery (the DHT), anyone who ever knew the
  folder's ID, past key holders included, can watch which addresses announce it,
  day by day. Relays never learn the folder's ID, and the DHT never carries
  which hole-punch coordinators your device uses.
- A relay or a hole-punch coordinator, which may be anyone's computer, sees both
  devices' addresses, but not the folder and not what passes through it (see
  "Relays and hole-punch coordinators are strangers").
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

## Relays and hole-punch coordinators are strangers

Two devices that cannot reach each other directly can use two kinds of helper.
Both can be anyone's computer, a stranger's included, and neither is used
unless a person turned it on.

**A public relay** passes the encrypted traffic of the two devices. It sees both
devices' addresses and stable network identities, when they talk and how much,
and the sizes of the encrypted data it passes. It does not see the folder: it is
never told one, because a relay learns only the two devices, so it cannot use a
folder's ID to look for the folder elsewhere (TM-25, 2026-11-04). It cannot read
or change what passes, but it can slow, drop or cut a connection. There are no
private relays: a device cannot tell your own computers from a friend's, so
every relay is treated as a stranger's and serves whoever reaches it. One
exception to the encryption: when a browser client is one end of a relayed
connection or of a coordinator's connection, that connection uses classical
encryption, so a relay or coordinator that records it could read the folder's ID
from it later, if someone gets a quantum computer.

**A hole-punch coordinator** helps two devices behind home routers find each
other. It passes their first few messages, so that each learns the other's
public address and both try to connect at the same moment. It learns that the
two are connecting, when, their public addresses and stable network identities,
and which devices keep a standing connection with it. It learns no folder, no
key and no file, and no file data goes through it: the two devices never sync
through a coordinator, so it never becomes a relay, and it passes at most 64 KiB
and 60 seconds of set-up for each punch. A coordinator that lies or stays silent
only makes the punch fail.

What you control:

- Using relays, and acting as a relay for others, are off unless you turn them
  on. Any computer can be set up as a public relay, never a phone, with limits
  on rate and open connections and a minimum transfer rate (the owner's
  decisions, 2026-11-03 and 2026-11-04). Such a computer also coordinates hole
  punches, within the same limits. Others use it for hole punching only through
  a folder set to the fourth level with its public-relays switch on.
- Acting as a hole-punch coordinator is off unless you turn it on, on computers
  only (the owner's decisions, 2026-11-08 and 2026-11-10). A phone can be helped
  by a coordinator but never helps.
- Asking for punches, and being reached through them, is on unless you turn it
  off, but only through devices that share a folder with yours. Coordinating for
  others, and using or serving relays, are off unless you turn them on. Every
  way of connecting has a setting for the whole device, which each folder can
  override. For hole punching a folder can choose never; only devices it shares
  a folder with (the default); also devices it is directly connected to; or also
  any device it knows and any public relay you turned on. A punch serves a
  folder only if both devices' settings for that folder allow it; otherwise that
  folder does not sync over that connection and the others do.

Who can reach you through a coordinator, and what that costs:

- Your device lists the coordinators it uses only to the devices it shares a
  folder with, and for each coordinator only if that folder's own settings allow
  a punch through it, and in invites to such a folder that carry addresses. It
  never puts them, or its network identity, in the DHT. So the devices you share
  folders with, and anyone an invite reached, learn which coordinators you use.
- Only a device that shares a folder with yours, or a new one that first proves,
  with a few small messages and no files, that it holds a key to such a folder,
  can punch through to your device. Anyone else gets no punch and no address;
  they learn at most that your device is online and which device it is. That
  holds for a person you removed from a folder only once the old folder is
  unlinked on your device, and unless they still share another folder with it.
  Until then, a key proof, or the record of them as a peer, which your device
  keeps for up to 90 days, lets them punch through (see step 3 under "Holding a
  key is the access").
- A punch tries only a few public internet addresses, so it cannot be aimed at
  your home network's private addresses, with stated exceptions: behind a
  network translator whose prefix or mapping your device cannot learn, or a
  public-looking network reached by a route other than the default one, a
  split-tunnel VPN's say. In those cases a punch's packets can reach a private
  network.
- A device that your device admitted, or that admitted it, can still make it
  send a few short bursts of packets to public addresses of its choosing, its
  own network's among them, at most 160 a minute. That is all a punch can be
  used to send at someone else, and it costs a little of your connection.
- Someone your device refuses who already has its network identity and
  coordinators, a former peer or anyone an invite reached, can keep your device
  from being reached by punches through each of those coordinators for as long
  as they keep asking. Your own punches through other coordinators, and your
  direct, local network and relay connections, are unaffected.

The reason: a device cannot tell your own machines from a friend's when both
share the same folders, so every helper is treated as a stranger and is given as
little as it needs. In the owner's words of 2026-11-08, "I don't want random
connected clients used as relays without the operator's consent."

What to do: leave relays and coordinating off unless you want them. Turn on
relaying only on a computer whose connection you are content to share, with
limits you set; it then coordinates punches too. If you do not want your device
to ask for punches, set a folder's hole punching to "never"; asking is on by
default, through devices that share a folder.

## Write times are recorded to the microsecond

Each version of a file records when it was written, to the millionth of a
second, and which device wrote it. Every holder of the read or write key can
see this for every version, including edits made offline, and which devices
wrote the file before, and, from the watermarks, roughly when deletions were
forgotten. Storage devices cannot read these times, though they see when records
arrive.

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

## Deleted files can come back

A deletion is forgotten a year or more after it arrived, and a file whose
deletion has been forgotten can come back. The year is counted by the devices
that have the write key, each by its own calendar, from when a deletion reached
it. Once such a device has held deletions for 365 days, it signs a short
message, a **watermark**: "forget the deletions up to this point". The point is
the newest deletion it has held that long, but never past one it has held for
less (the owner's decision, 2026-11-12: "Wait, never early"). The device does
not need to have been switched on all year, only when it signs. Every device
that receives the watermark forgets the same deletions, whatever its own clock
says, and ignores any of them that it is sent again. So every device that holds
the same records forgets the same deletions. Cleanups are forgotten the same
way. Once deletions stop, devices stop signing, so the last year's deletion
records stay until the next deletion brings a new watermark (a provisional
ruling by the project's lead, not yet reviewed by the owner). Storage devices
read no watermark: they forget only what a device with the write key names in a
cleanup, and keep a cleanup until a later one replaces it.

After a deletion is forgotten:

- A device that comes back still holding an old copy of a deleted file brings
  it back as a live file, or as a conflict copy beside a newer file with the
  same name.
- A file you delete on a device that then stays offline for more than a year
  comes back on that device when it reconnects, if a watermark has passed its
  deletion by then (the owner's decision, 2026-11-12: "OK, file comes back").
- Any device that kept an old copy, whatever key it holds, can bring it back
  whenever it likes.

**Forgetting can come late.** Nothing promises that a deletion is forgotten
after exactly a year. A deletion that reaches a device late holds back that
device's forgetting of the later ones until the late one, too, has been there a
year. And someone who wants deletions kept can hold forgetting back: a device
with the write key that keeps signing deletions dated just past the last
watermark; a storage device, or a device with the read key, that is a device's
only link to the others and hands it the folder's deletions slowly; or someone
on the network who cuts a device off and lets the deletions through late. It
costs storage, never a file (the owner's decisions, 2026-11-12: "Wait, never
early" and "Accept, documented"). What it costs in storage: the cleanups for a
folder of 100 000 deleted files come to about 3.4 MB, kept a year or more on
every device that can read the folder; and each device with the write key can
sign a watermark a day, about 400 bytes, which in a folder with 64 such devices
is about 9 MB a year.

### Wrong clocks

In the specified design, no device refuses another, or a single version, for its
clock: the clock rule of 2026-10-25, which refused to sync with devices whose
clocks were more than a year apart, is dropped (the owner's decision,
2026-11-08), and with it the remedy of stopping the folder, fixing the clock and
adding the folder again. The current code still has the rule. A wrong clock
costs at most deletions forgotten early, and the files then come back; none is
lost.

- **The worst case** (the owner's decisions, 2026-11-12: "Accept, documented"
  and "Still accept, documented"). A device with the write key whose clock jumps
  a year or more ahead finds every deletion it holds a year old, and signs a
  watermark that makes every device forget every deletion in the folder at once.
  Deletions still on their way when it does are undone when they arrive. Every
  deleted file of which a copy survives somewhere comes back. That can happen
  through a mistyped year; through a new device that first synced with a wrong
  clock and was then corrected; through a machine back after a year or more
  whose clock is wrong when it starts, before it has set its time from the
  network; and through someone with no key at all who fakes the time a device
  takes from the network (network time that is not authenticated, or a fake
  mobile phone mast). There is no guard against it, by the owner's choice.
- A device with the write key whose clock runs fast, or is set forward, signs
  the watermark early, by as much as the clock gained, and every device then
  forgets that deletion early. The file comes back only from a copy that
  survived: a device that was offline and never saw the deletion, a storage
  device that kept it, or someone with a key who saved the old version and sends
  it again.
- A clock that is ahead or behind by the same amount all year counts the year
  right. A clock set back makes forgetting late, because the year is counted
  again by the slower clock.
- A device whose clock reads far behind, such as a Raspberry Pi that started
  without a network and has no battery for its clock, and that has seen little
  of the folder, dates its deletions in the past. They are undone when they
  reach the others, and the files come back to it. Once it has synced, its dates
  follow the others', whatever its clock says.
- A device whose clock is ahead pushes every later version number in the folder
  ahead with it, for as long as it keeps writing and for as long again after it
  stops. The watermarks move ahead too, so a deletion made by a device that has
  not synced since can be undone the same way. It cuts no one off.
- The time a device shows for a version is the writer's own clock reading, so a
  wrong clock shows wrong times for its own versions and changes nothing else.

The reason: the owner's decisions (2026-11-08 and 2026-11-12) are a year, the
same on every device, forgotten together by one signed message, and old files
that come back rather than being treated as deleted. If each device forgot by
its own clock, two devices could disagree for ever about which deletions exist.
If they forgot by the folder's own time, one device's clock far ahead would make
every device forget everything at once, which would need the clock rule, and the
rule costs refused devices, cut-off devices and a remedy that starts a device's
index afresh. And in the owner's words, "I can't let indexes grow indefinitely":
a deletion that is never forgotten would stay in the index for good. Treating
returning files as deleted would need to know when each pair of devices last
met, and could send live files to the archive by mistake. The cost is a file you
deleted reappearing. It is never a loss.

What to do: delete it again. Do not leave a device or a backup of a folder
unsynced for more than a year and then bring it back without checking what
reappears. Let every device set its clock from the network, and give the write
key only to devices whose clock you trust.

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

The reason: the rename case is the price of one record per deleted folder
rather than one per file, and the others break what the design assumes about
a device: one machine per device ID, and a clock that never reads earlier than
the device's last write. The threat model states each one.

What to do: never run two machines from one copy of a device's Lickety-Sync
data. Check a restored device's clock, and let it sync with the others before
anyone edits files on it. Keep the archive on.

## A version your device cannot read

A newer release of Lickety-Sync may write a kind of entry that your release does
not know, and a device with the write key may write a broken one. When such a
version replaces every version of a file your device has, the device can neither
show it nor tell whether it is newer, but it knows its own versions were
replaced: every other device on its release that holds the same entries has
dropped them. So it takes its copy of the file off its disk, into the archive,
and shows a warning that names the entry it cannot read (the owner's decision,
2026-11-03: "Yes, remove the old copy"). When such a version replaces only some
of the versions your device has, the file stays, showing one that is left. On a
desktop the warning also shows in the status bar and as a system notification.

The copy is kept until the problem is fixed (the owner's decisions, 2026-11-10:
"Until fixed" and "30 days should be configurable"). That is past the archive's
usual 30 days, which are a setting, and even where versioning is off. It ends
when a save replaces that old version, which the next save of the file on that
device does, and so does one on any device that had the old version, or when the
device is updated to a release that reads the entry. An edit made elsewhere
without the old version brings the file back, but leaves the copy and the
warning. Saving the file on the device ends the keeping: the copy then goes as
the archive's 30 days say, or at once where versioning is off, so restore from
the archive first whatever you want of the old file.

What it means for you: a file can vanish from one device's folder, with a
warning, until it is fixed. A device with only the read key cannot save, and
waits for one with the write key. An entry of an unknown kind may be a newer
release's, which updating shows, or a faulty one that no release reads, which
stays beside the saved file. Every device on one release that holds the same
entries shows the same thing.

What to do: keep every device on the same release, restore what you want from
the archive when the warning appears, and save the file again from a device with
the write key.

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

- **Old deletion records linger.** Some deletion records stay until they are
  forgotten (see "Deleted files can come back") or a cleanup removes them, for
  example one of two deletions of the same folder made at the same time on two
  devices. They take a little space. Nothing to do.
- **A broken record stays until the file is saved again.** If a write key holder
  sends a record that devices cannot read or that breaks a rule, every device
  keeps it, and shows a warning that saving the file again clears it. At a file
  nobody saves again it stays for good, and so does a broken cleanup. What to
  do: save the file again. Devices that had the version it replaced keep their
  copy meanwhile (see "A version your device cannot read").
- **A newer release's versions.** A device on an older release keeps and passes
  on a newer release's version that it cannot read, and shows it as an update it
  cannot read (see "A version your device cannot read" for what happens to the
  file). What to do: keep every device on the same release.
- **Storage devices keep old copies.** A storage device replaces a version only
  when a newer record names it, and otherwise keeps both. Its old copies stay
  until a device with the write key meets it and sends a cleanup. A storage
  device that only read-only devices visit keeps them. Its status shows how many
  extra versions it holds. What to do: let a device with the write key meet it
  now and then.
- **A storage device can be filled with old copies.** Someone with the sealed or
  read key who kept old versions can send them all to another storage device,
  which keeps them until a device with the write key meets it. What to do: let a
  device with the write key meet it now and then.
- **A storage device can lose a returned file for a while.** Someone with the
  sealed or read key can make another storage device drop a file that came back
  after its deletion was forgotten, by sending it the old deletion. The file
  returns there when a device with the write key next meets it, and devices that
  can read the folder keep it throughout. Nothing to do.
- **Freed space can wait** (an open design question). Pieces that no file uses
  any more are deleted at the end of the next round of syncing. While every
  connection is already up to date, that round may not come until the next
  reconnect. Nothing to do.
- **Checking at start.** When a folder opens, every saved record is checked
  again, and the folder shows Checking meanwhile. This takes time in proportion
  to the number of records the folder holds. The owner chose this (2026-10-25)
  because it catches records damaged or changed on disk with no extra secret to
  keep. Nothing to do.
- **Damage on disk.** A device drops saved records that are damaged and fetches
  them again from other devices. A change that only that device had is lost.
  What to do: let each device sync with another soon after you change files on
  it.
- **At most 256 folders on a device** (a limit of the current engine). What to
  do: keep to 256 folders on one device.
- **At most 64 folders with one device.** The protocol opens at most 64 folders
  on one connection, and the current engine makes one connection to each device.
  If you share more than 64 folders with one device, only 64 of them sync with
  it, and a message says so. What to do: share at most 64 folders with one
  device.
- **A write key holder can lock the folder.** Version numbers stop at a fixed
  date, 10 000 years after 1970, the same on every device, and a version with a
  later number is refused everywhere. A write key holder can sign a record
  numbered just before that date, which makes the folder unwritable for everyone
  until it is shared again as a new folder. Only a write key holder can do this,
  and one can already delete everything (the owner accepted this risk on
  2026-11-12: "Accept, documented"). A write key holder can also sign a
  watermark that forgets every deletion at once, which brings deleted files back
  wherever a copy survives (see "Wrong clocks"). What to do: if the folder is
  locked, move to a new folder as described under "Holding a key is the access".

## Where this comes from

The points above are stated, with more detail, in the [threat
model](https://github.com/LicketySync/core/blob/main/spec/threat-model.md) and
the [protocol
specification](https://github.com/LicketySync/core/blob/main/spec/protocol.md)
(its last section explains the index in plain words). The limits and open
questions of the current engine marked above are in the project's
[changelog](https://github.com/LicketySync/core/blob/main/CHANGELOG.md). This
page changes when the design does.