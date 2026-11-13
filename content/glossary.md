+++
title = "Glossary"
description = "The words this manual uses, in plain language."
+++

The words this manual uses, a sentence or two each. Look one up, then get back
to the good stuff.

[A](@/glossary.md#address) ·
[C](@/glossary.md#conflict-copy) ·
[D](@/glossary.md#deletion-brake) ·
[E](@/glossary.md#encryption) ·
[F](@/glossary.md#folder) ·
[H](@/glossary.md#hole-punching) ·
[I](@/glossary.md#ignore-file) ·
[K](@/glossary.md#key) ·
[L](@/glossary.md#linked-devices) ·
[N](@/glossary.md#network-id) ·
[P](@/glossary.md#pause) ·
[R](@/glossary.md#read-key) ·
[S](@/glossary.md#sealed-key) ·
[T](@/glossary.md#telemetry) ·
[U](@/glossary.md#unlinking) ·
[V](@/glossary.md#version) ·
[W](@/glossary.md#watermark)

## The words, A to Z

### Address

Where a device can be reached on a network, the way a street address says where
a house is. Your public address is the one the rest of the internet sees,
usually your home router's.

### Archive

Where a device keeps the files that changes from other devices replaced or
deleted, 30 days unless you change it, so you can get them back. Each device
has its own. See [Features](@/features.md#the-archive).

### Conflict copy

The second version kept when two changes to one file collide. It sits beside
the file as an ordinary file until you delete it. Nothing is merged and nothing
is lost.

### Coordinator

See [hole-punch coordinator](@/glossary.md#hole-punch-coordinator).

### Deletion brake

A pause for when lots of files vanish from a device's own disk at once, say
from an unplugged drive: the folder stops and tells no other device until you
confirm. Deletions that arrive from other devices aren't braked; the archive
keeps those.

### Device

A computer, phone or web browser running Lickety-Sync. Any device that connects
to it learns its [network ID](@/glossary.md#network-id) and
[device ID](@/glossary.md#device-id); its name, which you choose, shows only to
the devices it opens a folder with.

### Device ID

A lasting number that marks the versions a device writes, so anyone with the
folder's read or write key can see which device changed what. It isn't the network ID, though
both stay the same, and any device that connects to yours learns both.

### DHT

See [public trackers](@/glossary.md#public-trackers-the-dht).

### Direct connection

Two devices connected straight to each other, with nothing in between. Across
the internet, hole punching is one way to make one.

### Encryption

Scrambling data so that only someone with the right key can read it. Every
connection is encrypted, and your files and their names are encrypted before
they leave your device.

### Folder

What you share: a folder of files kept the same on every device that holds one
of its keys. Each folder has its own three keys.

### Folder ID

The number that names a folder, which anyone with any of its keys can work
out. It's how someone who has had or seen a key can recognise the folder on a
local network, or look it up in public trackers.

### Forgetting (old deletions)

Devices remember deletions for a year or more after they arrive, sooner if a
device with the write key has a wrong clock. Then a device with the write key
signs a short message, a watermark, and every device forgets them together. A
device that kept an old copy can then bring a deleted file back.

### Hole punching

A way for two devices behind home routers to reach each other directly. A
helper passes their first few messages, then both knock at the same moment,
and most routers let them through. See
[How devices find each other](@/finding-each-other.md#hole-punching).

### Hole-punch coordinator

The helper in a hole punch: a computer whose person turned on helping with hole
punches, or a public relay, which helps too. It passes a few small messages and
never your files. It learns that two devices are connecting and when, their
addresses and network IDs, but no folder, key or file.

### Ignore file

A list of the things in a folder that you'd rather not sync.

### Index

A folder's list of its files, which every device keeps: for each file, its
latest version, signed with the write key. A storage device keeps it too,
sealed, so it can't read the names.

### Invite

One of a folder's keys, ready to send as a QR code, a link or a file. It
carries your device's addresses only if you add them; an invite meant for the
browser version, and a one-time file link, add them for you.

### Key

The secret that lets a device into a folder. Each folder has three: the
[write key](@/glossary.md#write-key), the [read key](@/glossary.md#read-key)
and the [sealed key](@/glossary.md#sealed-key). A key works for as long as the
folder exists, and can't be taken back.

### Key holder

Anyone with one of a folder's keys, on any of their devices. Nothing in a
folder says which person holds which key, though device names, and which
device made each change, often tell.

### Key store

Where a device keeps its folders' keys, locked by your system's own keychain
or, on a device without one, by a passphrase.

### Linked devices

Your own devices, joined so that a new one gets the keys of the folders you
pick when you scan a code it shows. Coming later.

### Local network

The network your devices share at home or at work, such as your Wi-Fi.

### Local network discovery

Devices announcing themselves on the local network, so the others find them
with nobody typing anything. While it's on, anyone on that network can see
that a device runs Lickety-Sync. See
[Setting up](@/setting-up.md#find-devices-on-your-local-network).

### Network ID

A device's lasting identity: a number it shows to every device it connects
to, the same across networks and folders, so a device yours has met can
recognise it again. It isn't the [device ID](@/glossary.md#device-id).

### Pause

Stopping a folder from syncing on one device for a while. Before you remove
someone, it's what stops more changes arriving from them.

### Peer

Another device you sync a folder with. Peers tell each other where their other
peers are, which is how a device can be found through the ones you already
sync with.

### Peer to peer

Devices connecting to each other directly, rather than through a company's
server. Lickety-Sync is peer to peer.

### Port

A number that goes with an address and says which program on a device a
connection is for.

### Public trackers (the DHT)

A public directory spread across many strangers' computers, not a server
anyone runs, where devices that hold a folder's key can look up each other's
addresses. Setup will suggest turning it on, only to look things up and
announce your device. Anyone who has had or seen one of
the folder's keys can use it to see which addresses offer the folder. See
[Setting up](@/setting-up.md#use-public-trackers).

### Read key

The key to see everything in a folder and change nothing: other devices accept
a change only when it's signed with the write key. A read key holder can pass
on the read key or the sealed key.

### Read-only device

A device that holds a folder's read key: it sees everything in the folder, and
its own changes are never sent to anyone.

### Relay

A go-between that passes encrypted traffic between two devices that can't
reach each other any other way, without being able to read it. Every relay is
public, so it may be a stranger's computer, and none is used unless relays are
on, which setup will suggest. See [Setting up](@/setting-up.md#use-public-relays).

### Router

The box that connects a home or office to the internet. It usually turns away
connections it didn't ask for, which is why devices behind routers need hole
punching, or a relay, to reach each other across the internet.

### Sealed key

The key to keep a full, encrypted copy of a folder without being able to open
it: not the files, not even their names. A device with only this key is a
storage device, and it can pass on only the sealed key.

### Sharing again

Starting a new folder, with new keys, over files you already have. It's how
the people who stay remove someone, since a key can't be taken back.

### Signature

A seal made with a folder's write key. It proves that a change came from
someone holding that key and wasn't altered on the way, and devices refuse any
change without one.

### Storage device

A device that holds only a folder's sealed key, such as a home server or a
friend's spare computer. It keeps and passes on an encrypted copy it can't
open, but it sees how many files there are, roughly how big, when and from
which device each changes, and the devices' names. Someone running it who
knows some of the folder's files, or got files of their choosing into it, can
in time test whether other files they have are in it.

### Sync

Keeping a folder the same on several devices: when a file changes on one, the
change travels to the others.

### Telemetry

Reports a program sends home about how you use it. The Lickety-Sync programs
you install send none.

### Tor

A network run by volunteers that passes connections through several computers
in turn. Lickety-Sync plans to support it later.

### Unlinking

Taking a folder off a device while keeping its files, and its archive, as
ordinary files. The device stops syncing that folder.

### Version

A file's content, or its deletion, as one device saved it. Each device keeps
only the latest versions of each file.

### Watermark

See [forgetting (old deletions)](@/glossary.md#forgetting-old-deletions).

### Write key

The key to read and change everything in a folder, and to give out any of its
keys. It's full control, so showing it will ask for your device's own
password, PIN or fingerprint. It can also be written down as 24 words.
