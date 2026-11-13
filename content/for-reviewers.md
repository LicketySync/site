+++
title = "For IT and privacy reviewers"
description = "What Lickety-Sync contacts, what it stores and where, its cryptography in brief and its licences: the details a review needs."
+++

This page is for people checking Lickety-Sync against a policy: IT teams,
compliance and privacy reviewers. It's more precise than the rest of the
manual, holds the numbers and mechanisms the other pages leave out, and cites
the threat model's numbered requirements (TM-1 to TM-43) so you can look each
one up. Nothing ships yet: this describes the design in the specifications on
the core repository's main branch, and says where today's code still differs.
One part is decided but not yet in the specifications: what setup turns on
(see [What it contacts](#what-it-contacts)).

{{ <illustration src="for-reviewers-map.svg" alt="Two devices joined directly by a line with a padlock on it, and a home router below linking them on the local network. Above them, three helpers, each with its switch off: public trackers, drawn as a ring of small computers; a relay, a box passing a sealed envelope along; and a helper computer that coordinates hole punches for others. Nothing is on before you answer setup, which will suggest turning on public trackers, relays and hole punching, and, on computers, coordinating punches for the devices you share a folder with." width={560} height={300} /> }}

## In brief

- No account, no telemetry (reports about how it's used) and no server you
  didn't turn on; nothing is on before the person answers setup. The programs
  contact no fixed host that `spec/allowed-hosts.txt` doesn't list, and the
  list is empty today (TM-31). They check for no updates (TM-29).
- Devices connect to each other directly. Every connection is encrypted and
  authenticated; there is no plaintext mode (TM-1).
- Each folder has three shared keys, and holding one is the access. There is no
  owner key, no per-person key and no revocation; leaving someone behind means
  moving to a new folder (TM-42).
- Contents, names and structure are encrypted under the folder's keys before
  they leave a device (TM-33).
- Open source, under the Mozilla Public License 2.0.

## What it contacts

**Fixed hosts.** `spec/allowed-hosts.txt` lists every fixed hostname and
address built into the programs, and it is empty today, because the code
contacts nothing. A host is added only in the change that adds the feature
which contacts it, and only for a feature that is off until the person turns it
on; the multicast and broadcast addresses that local network discovery will use
are to be listed there too, each with a note. A check in the build and in CI
fails on any hostname in the code's string literals that the list lacks. It
catches accidents, not adversaries, and states its own limits; the reviewers of
each change cover the rest (TM-31). Join links, which the programs read and
write, begin with `licketysync.app/join#`; the programs never request that
address.

**Everything else** is reached at addresses learned while running, and only
through features that are on: other devices that hold a folder's key (found on
the local network, through peers, in an invite or from an address the person
types), the nodes of the public trackers (the DHT, the Mainline DHT that
BitTorrent also uses), relays and hole-punch coordinators (devices that pass
the first messages between two devices so they can connect directly).

**The home router's port-mapping service** (Universal Plug and Play, NAT-PMP
or PCP), when the person takes the recommended settings or turns it on. The
device
maps only its one listening port, for a bounded lease, and removes the mapping
at exit (TM-26). While the port is open, anyone on the internet can reach it
and learn that the device runs Lickety-Sync, and what anyone who completes a
connection learns ([What each party sees](#what-each-party-sees)).

**What is on, before and after setup.** Nothing is on before the person answers
setup. Setup will ask once and suggest the recommended settings below; saying
no opens the settings screen. Setup never suggests acting as a relay for others
or helping run the public trackers, and never suggests coordinating punches for
devices the device shares no folder with. These defaults (nothing on before
setup, the recommended settings, the fourth helper level and the narrower
coordinating mode) are decided but not yet in the specifications, which still
state the earlier defaults; a change to them is under way.

{% <table_box label="Connection settings before setup, and the recommended settings"> %}

| Setting | Before setup | The recommended settings |
| --- | --- | --- |
| Finding devices on the local network | Off | On |
| Asking the home router to open a port | Off | On |
| Public trackers (the DHT) | Off | On, in client mode |
| Direct connections, with hole punching | Off | On |
| Using public relays | Off | On |
| Who may help with a hole punch? | Nobody | The fourth level: also any device yours knows of, and any public relay you've turned on |
| Acting as a hole-punch coordinator, for the devices of the person's own shared folders (computers only) | Off | On |
| Acting as a hole-punch coordinator, for anyone (computers only) | Off | Never suggested |
| Acting as a relay for others (computers only) | Off | Never suggested |
| Helping run the public trackers (server mode) | Off | Never suggested |
| Tor (later) | Off | Not suggested |

{% </table_box> %}

Finding devices on the local network, public trackers, direct connections, the
"Who may help with a hole punch?" level, using public relays and, later, Tor
are each a default for the whole device that each folder can override; asking
the router to open a port and the ways of helping others are set for the whole
device only. A new folder starts from
the device-wide settings, and a connection serves a folder only as that
folder's settings allow (TM-25). A hole punch needs a coordinator: a device the
folder's "Who may help with a hole punch?" level allows, whose person turned on
coordinating, or a public relay. With the recommended settings, every
coordinator a device uses, a stranger's public relay among them, learns that
two devices are connecting and when, their addresses and network IDs; none
carries file data. A device that a computer helps names that computer, by its
address and network ID, to its own folders' peers and in its invites, so
strangers can learn both, and can ask that computer for a punch (TM-25,
`keys.md` §6.3).

**Listening.** A device listens for other devices on its network port. The
service also takes commands from its own command line and web interface, on a
socket or named pipe that only its user can open and on a localhost address
that checks the Host and Origin headers and needs a token on every request
(TM-21). Names and other text from other devices are only ever shown as text
(TM-20).

**On locked-down networks** devices can fall back to WebSocket. A corporate
proxy there sees the hostname dialled, timing and volume, and only encrypted
frames, since the inner handshake runs inside the WebSocket (threat model §5).

## What it stores, and where

On each device:

- **The folder's files**, as ordinary files, on devices with the read or write
  key. Writes land in a temporary file beneath the folder, are checked against
  their signed record and are then renamed into place (TM-35). Every path goes
  through one resolver that never follows a symlink (TM-4).
- **The archive**: files that changes from other devices replaced or deleted,
  kept 30 days unless that device's person changes it (TM-35). Copies archived
  because their versions became unreadable stay past that, even with the
  archive off, until a version the device reads covers them. Each device keeps
  its own archive, so a deleted file stays in other devices' archives as long
  as their people choose. Unlinking a folder keeps its files and its archive as
  ordinary files (`keys.md` §7.3).
- **The index**: the latest versions of each file as signed records, typically
  under 1 KB per file and at most about 2.3 KB for a small file many devices
  wrote; deletion records, kept a year or more; and, for each device it synced
  with in the last 90 days, where it stopped, about 60 bytes each and at most
  4 096 per folder (`protocol.md` §9). Conflict copies are live files with
  records of their own, so they add up: four machines that append to one 10 MB
  log at the same moment every hour leave about 26 000 copies and about 300 MB
  of index a year, until someone deletes them. Every stored record is checked
  again each time its folder opens, so a store changed at rest can't make a device
  apply a change the write key didn't sign (TM-9). Today the index lives in
  memory; from the next phase it is an SQLite database on disk.
- **Encrypted pieces of files** (chunks) that the device holds and serves to
  other devices of the folder. A storage device, which holds only the sealed
  key, keeps the folder as these pieces and its records, and can open neither.
- **The key store**: the device's seed and one key per folder, at that device's
  level; nothing derived from them is stored. It is a file encrypted with
  XChaCha20-Poly1305 under a key held in the platform keystore (Keychain,
  Credential Manager, Secret Service, Android Keystore), or, where there is
  none, derived with Argon2id from a passphrase the person sets. A folder's key
  is erased when the folder is unlinked (`keys.md` §8.2). It arrives with the
  next phase; today the code stores no key on disk.

Exact paths on disk are not fixed yet. Secrets are held in memory in types that
are wiped when dropped, and never written to logs; logs leave out folder IDs
and addresses by default (TM-6). Two exceptions are recorded in `suites.md`
§4.1: beneath the network layer, the device's libp2p identity key is held in
types that are not wiped, and is copied into the TLS and QUIC configurations;
and the optional WebTransport listener's certificate key passes through types
that are not wiped. Nothing anywhere records which person holds a key or made
a change.

## No telemetry

The programs send no telemetry, analytics, crash reports or update checks
(TM-29, TM-31). The apps will get updates only through package managers and
app stores, with their signatures checked before anything is unpacked, and
never to a lower version. The browser version is the exception: it loads its
code from licketysync.app at each visit (TM-28). Releases, when there are some,
will be built in CI, signed and published with a software bill of materials
and build provenance (TM-29).

## The key model

- A folder is a random 32-byte write secret, from which everything else derives
  one way: the folder's Ed25519 signing key, its data key and its ID. The read
  key carries the verification key and the data key; the sealed key carries the
  verification key and an access key derived from the data key. No lower key
  yields a higher one (`keys.md` §1).
- Any holder can give out its own key or a lower one, to anyone, any number of
  times. Keys never expire. Holders of one key look alike: nothing records who
  holds which key or who changed what.
- Every update is signed with the folder's write key, and every device rejects
  one that isn't, so read and sealed holders can't change anything (TM-9).
- Each side of a connection proves it holds a key to a folder without sending
  it; knowing a folder's ID isn't enough to connect or to learn whether a
  device holds it (TM-32). It does let its holder confirm the folder on a local
  network and find, through public trackers, the addresses that announce it.
- A key is written as text with a checksum, and the write key also as 24 words
  from the BIP39 list. A key carries no address unless the sender adds one, or,
  for invites to the browser version and one-time links for sending a file,
  always (TM-37); an invite with addresses also names the device's hole-punch
  coordinators (TM-25). In a link the key sits after the `#`, which browsers
  don't send to servers (TM-18). The app will show or export a key only on the
  person's explicit action, and the write key only after the system's own user
  check, or the key store's passphrase where there is none (TM-39).
- A device has three lasting identifiers, fixed for the life of its seed
  (`keys.md` §8.1): its network ID (the libp2p peer ID, which the transport
  authenticates) and its device ID (the hash of its device key, which signs
  only the handshake's device proof), both shown to whoever completes a
  connection; and, from the phase after next, its DHT key, which signs only
  its address records in public trackers and is published in them, never
  shown at connection. The DHT node ID it persists and publishes there is
  stable too (TM-24).
- Leaving someone behind, or a lost device, means the people who stay unlink
  the folder and share their copy again as a new folder with new keys, given
  to the others outside the app; nothing in the protocol supports it, and the
  person left behind keeps what they had (TM-42, `keys.md` §7.4).

## Cryptography in brief

Suite 1, from `suites.md`, is one fixed set for keys and records: its parts are
never negotiated, and a folder's suite is fixed by its keys (TM-30). The
transport's TLS 1.3 chooses its key-exchange group and cipher suite from the
fixed lists the libp2p TLS stack offers, as TLS does.

{% <table_box label="Suite 1 algorithms"> %}

| Role | Algorithm |
| --- | --- |
| Signatures | Ed25519 (RFC 8032), verified strictly |
| Data encryption | XChaCha20-Poly1305, 256-bit keys |
| Hashing and key derivation | BLAKE3: labelled hash, keyed hash and key derivation |
| Passphrases | Argon2id, 64 MiB, 3 passes, 4 lanes |
| Transport | TLS 1.3 over QUIC and TCP, preferring the hybrid post-quantum group X25519MLKEM768; Noise XX inside WebSocket, WebTransport and WebRTC |

{% </table_box> %}

- Each transport offers exactly one security protocol, and the suite lists are
  bound into the handshake, so a downgrade within a transport fails (TM-1,
  TM-30). Blocking UDP and raw TCP can still force a device onto the WebSocket
  path, which is classical today.
- Chunk nonces are derived from the content, so that storage devices can check
  and de-duplicate what they hold; identical pieces therefore look identical
  (TM-12). Every other nonce is random.
- Chunks are padded with Padmé, at most 12 % and on average 3 % extra (TM-15),
  so each size shows only roughly. Padding doesn't hide the number of pieces,
  the total volume or when changes happen. Nothing is compressed today; a
  release that turns on compression will state its length channel first.
- Cryptography comes only from the approved crates `suites.md` lists; the
  project writes none of its own beyond the two padding schemes (TM-5).
- The 256-bit symmetric parts withstand a future quantum computer; Ed25519
  doesn't. Recorded traffic would give such a computer at most what a storage
  device sees, but it could let anyone with a folder's verification key forge
  updates (TM-33).

## What each party sees {#what-each-party-sees}

The full list is
[threat model §5](https://github.com/LicketySync/core/blob/main/spec/threat-model.md#5-what-stays-visible).
In short:

- **Write and read key holders**: everything in the folder; for every version,
  the device ID its writer claims and when it was written, to the microsecond,
  offline edits included; every device's ID, name and reported key; which
  files each device keeps, through chunk requests; and roughly when deletions
  were forgotten.
  They can test whether a guessed file is in the folder.
- **Every device of a folder, whatever its key, and anyone who gets an invite
  with addresses**: the device's addresses, its network ID and the hole-punch
  coordinators it uses, through peer exchange (TM-25).
- **A storage device**: see
  [What a storage device sees](#what-a-storage-device-sees).
- **The local network**, once finding devices there is on, as setup will
  suggest: that a device runs Lickety-Sync and its port; a confirmation that a known folder is announced
  there, or that a device already met is announcing. Anyone there, knowing
  nothing, can replay a captured announcement from their own address and learn
  from who dials back which devices hold the folder it names, along with their
  network IDs. Announcements more than five minutes off the listener's clock
  are ignored, and a listener that hears only such announcements from one
  address raises "clock differs from LAN peers" (`keys.md` §2.2).
- **Public trackers (the DHT)**, once on, as setup will suggest: which
  addresses announce a folder's hash for the day. The hash changes daily and doesn't give away the folder,
  but anyone who has had or seen a key can compute every day's hash. And anyone,
  with no key, can follow one device across days and folders by its stable
  address, or by the stable identifiers it publishes with its announcements:
  the key that signs its address record and a persisted node ID (TM-24).
- **Anyone on the internet, while the router's port is open**: that the device
  runs Lickety-Sync, by connecting to the port, and so what anyone who
  completes a connection learns (next).
- **Anyone who completes a connection**, with no key: the device's address,
  network ID and device ID, the protocols and limits it offers, and, from how
  long a refused folder proof takes, roughly how many folders it holds; not
  which folders, and not its name, which goes only to a device that opens a
  folder with it. As the network layer is built today, a device dialling an
  address also shows its network ID to whoever answers there, before checking
  who answered; fixing that is next-phase work (§3.1).
- **A network observer**: which addresses talk, when, for how long and how
  much, and the sizes of encrypted frames; not contents, names, keys, folder
  IDs or network IDs.
- **A future quantum computer with recorded traffic**: what a storage device
  sees, from WebSocket sessions and from circuits with a browser client at one
  end; nothing from QUIC or TCP between two native ends, which use the hybrid
  group.

### What a storage device sees {#what-a-storage-device-sees}

A storage device holds the sealed key. It can't read contents, names, the
folder's layout, dates or exact sizes, but it sees the shape of the folder and
of its activity. Someone running a storage device who knows some of the
folder's files, or got files of their choosing into it, can in time test
whether other files they have are in it.

{% <more summary="The full list"> %}

It can't read contents, names, the folder's layout, dates, exact sizes, modes, the device IDs or counters inside
records, or which records are deletions, short of the cases below. It sees:

- each record's file ID, a keyed tag that names no path, so how many files
  there are and how often each changes;
- the fingerprints of the versions each record replaces, so where a file's
  history split into concurrent versions and joined again;
- each record's piece IDs and padded sizes, so each version's size to within
  about 3 %, which pieces are the same across files and versions, and from that
  a copy or a rename;
- that a record with no pieces is a deletion, a deleted folder, a directory or
  an empty file, without telling which;
- the size class of each record's sealed part (256, 512, 1 024 or 2 048 bytes
  and up), which shows only whether its path, pieces and writers together are
  small or large;
- which records a clean-up named together, and so how many files a deleted
  folder held; from some clean-ups, which records were folder deletions, or
  deletions old enough to have been forgotten; and, from a clean-up that names
  nothing, when a device with the write key signed a watermark;
- when each record arrives and from which device, which usually shows who
  wrote it;
- the folder ID and verification key; the device IDs, names, network IDs,
  addresses, reported keys and hole-punch coordinators of the devices it
  connects to;
- what each device reports about the folder: records and extra versions held,
  pieces stored, bytes still to fetch, whether it is paused and its free disk
  space; which pieces each device holds; and the folder's approximate total
  size.

Pieces are cut at places derived from the folder's key, which stops a storage
device cutting a file it already has and matching the piece sizes. But
published attacks recover such keys from the piece sizes of files an attacker
knows or chose, after which known files can be confirmed in that folder for as
long as it exists (TM-14, `keys.md` §3.2). Which chunker the next phase adopts
is still open; today's code cuts fixed-size pieces, whose sizes show only a
file's size.

{% </more> %}

## Hole punching in detail {#hole-punching-in-detail}

Hole punching arrives in the phase after next, and relays later still. A
coordinator carries only a punch's set-up, never files, and only a device that
shares a folder with yours, or proves a key to one, can punch through to it.

{% <more summary="How it works, its numbers and its stated residuals"> %}

From the phase after next (TM-25):

- **Who coordinates.** Only a computer whose person turned on coordinating,
  never a phone; a computer that relays also coordinates, within its relay
  limits. Coordinating will come in two modes: for the devices of the person's
  own shared folders only, which setup will suggest on computers, and for
  anyone who reaches the device, which it never suggests. A coordinator needs
  no key to any folder. Setup will suggest the fourth level of "Who may help
  with a hole punch?", so a stranger's public relay can coordinate a folder's
  punches. A device a computer helps lists that computer, by its address and
  network ID, to its folders' peers and in its invites with addresses, so
  strangers can learn both and ask that computer for a punch.
- **Who may help with a hole punch?** One of four levels, each including the
  ones before: nobody; devices you share a folder with (where the
  specifications start); also devices yours is connected to directly; also any
  device yours knows of, and any public relay you've turned on. A punch serves
  a folder only if both devices' settings for that folder allow it; on a
  connection they don't allow, the folder is treated as paused.
- **How the two find a coordinator.** The device that wants to be reachable
  keeps a standing connection with one to three coordinators its settings
  allow, and lists them only to the devices of its folders whose settings allow
  them, and in invites to such folders that carry addresses; never in the DHT.
  A device that wants to reach it tries at most three of them, two at a time.
- **What a coordinator learns.** That two devices are connecting, when, their
  public addresses and network IDs, and which devices keep a standing
  connection with it. It passes at most 64 KiB and 60 seconds of set-up for
  each punch, takes requests only over direct connections, and never carries
  the sync protocol, so it never becomes a relay.
- **Limits, by side.** On the coordinator: at most 64 KiB and 60 seconds for
  each punch, at most 64 punches at once and 120 opened in any minute, so about
  7.5 MiB a minute over a long run and at most about 11.5 MiB in any one
  minute. On each device being punched to or from: at most 2 punches at once
  and 6 a minute through one coordinator, and 4 at once and 20 a minute in
  all.
- **Who may punch through.** Only a device that shares a folder with yours
  whose settings allow that coordinator, or a new one that first proves it
  holds a key to such a folder. Anyone else learns at most that your device is
  online and which device it is. A person left behind still qualifies through
  the old folder until it is unlinked, with their key or through the per-peer
  record kept for up to 90 days, and through any other folder they share.
- **Stated residuals.** A punch tries only a few public addresses, but can reach
  a private network behind a translator whose prefix or mapping the device
  can't learn, or a public-looking network reached by a non-default route, as
  with a split-tunnel VPN. A device yours admitted, or that admitted yours, can
  make it send punch attempts to up to 8 public addresses of its choosing, its
  own network's among them, at most 20 attempts and 160 short bursts a minute.
  A requester your device refuses that knows its network ID and coordinators
  can keep it from being reached through those coordinators for as long as it
  keeps asking; its other connections are unaffected. A coordinator that
  ignores its own caps can make the device run at most 6 admission handshakes
  a minute through it.

**Relays**, from the last phase before 1.0, are public only: any computer whose
person turned relaying on, never a phone, treated as a stranger by every
device. A relay is told no folder, never ends the end-to-end channel, and sees
both devices' addresses and network IDs, timing, volume and frame sizes. It
enforces its person's limits: a weekly rate schedule, a cap on open circuits
with a queue, a minimum transfer rate, caps for each requester counted by
network ID and by source address, a fair share between circuits, and caps on
reservations (starting at 2 for each network ID, 8 for each source address and
256 in all). These numbers are starting values, fixed with the code.

{% </more> %}

## Forgetting deletions in detail {#forgetting-deletions-in-detail}

A deletion is forgotten only by a watermark that a device with the write key
signs, a year or more after the deletion reached it, sooner if its clock is
wrong. Forgetting can come late, which costs storage, never a file.

{% <more summary="The mechanism, and what it costs"> %}

A deletion, a deleted folder's record and a clean-up note are forgotten only by
a watermark, a short note a device with the write key signs (TM-40, TM-43):

- Each device with the write key notes, by its own calendar, when each deletion
  reached it. Once it has held deletions for 365 days, it signs a watermark up
  to the newest deletion it has held that long, never past one it has held for
  less. It needs to be on only when it signs.
- Every device that holds the watermark forgets the same deletions, whatever
  its own clock says, and ignores them if they are sent again. Storage devices
  read no watermark: they forget only what a clean-up from a device with the
  write key names.
- Forgetting comes a year or more after a deletion arrived, sooner where that
  device's clock is wrong, and nothing promises when. A late deletion holds
  back the forgetting of later ones until it has been held a year. A write key
  holder who keeps signing deletions just past the last watermark, or a storage
  or read-only device that is a write key device's only link and passes
  deletions on slowly, or someone on the network who cuts a device off and lets
  deletions through late, can hold forgetting back without bound. That costs
  storage, never a file.
- Once nobody deletes any more, devices stop signing, so the last year's
  watermarks, and the clean-ups that tell storage devices to forget deletions
  already forgotten, stay on every device until the next deletion brings a new
  watermark.
- What it costs: deleting a folder of 100 000 files sends about 3.4 MB of
  clean-up notes, 34 bytes per file, kept a year or more on each device that
  can read the folder. Each device with the write key can sign one watermark a
  day, about 400 bytes, so about 9 MB a year in a folder with 64 such devices.
- After forgetting, a device or storage device that kept an old copy brings the
  file back, as the file or as a conflict copy beside a newer one. A file
  deleted on a device that then stayed offline for more than a year comes back
  on that device when it reconnects, if a watermark has passed its deletion.

{% </more> %}

## Clocks in detail {#clocks-in-detail}

No ordering decision depends on a clock, and a wrong clock costs the index at
most deletions forgotten early, whose files come back.

{% <more summary="Each case"> %}

No ordering decision depends on a clock: versions are ordered by version
vectors, and no device or version is refused for its clock (TM-40). A device's
clock enters the index in two places only: the counter of each version it
writes, which follows its clock or runs just above the greatest counter it has
seen; and, on a device with the write key, when each deletion reached it, for
the watermark. One door-keeping check reads the clock: listeners ignore LAN
announcements more than five minutes off their own clock, though the device
can still connect by address or through peers. Everything else the clock
serves is advisory, such as lease lengths, protocol timeouts and the daily
hash for public trackers. That hash is looked up for the day before and the
day after too, so a device whose clock is more than a day off can't find
others, or be found, through public trackers (`keys.md` §2.2).

- **The worst case.** A device with the write key whose clock jumps a year or
  more ahead signs a watermark at the greatest counter it holds, so every
  device forgets every deletion at once, and deletions still on their way are
  undone when they arrive. A mistyped year, a new device that first synced
  with a wrong clock and was then corrected, a machine back after a year away
  whose clock is wrong at start, or someone with no key who fakes network time
  that isn't authenticated (NTP, or a fake mobile phone mast's time) can cause
  it. There is no guard; every file comes back wherever a copy survives.
- A write key device's clock that runs fast, or is set forward, signs early by
  as much. A clock wrong by the same amount all year counts the year right; one
  set back forgets late.
- A device whose clock reads far behind, such as a Raspberry Pi with no clock
  battery that started without a network, and that has seen little of the
  folder, dates its deletions below the watermark, so they are undone. Once
  synced, its counters follow the others'.
- A device whose clock is ahead pushes every later counter ahead with it, for
  as long as it writes and as long again after, so deletions by devices that
  haven't caught up can be forgotten early. It cuts no one off.
- Counters stop at a fixed ceiling, 10 000 years after 1970, the same on every
  device; a clock that reads later than 5 000 years after 1970 gives counters
  no later than that. A write key holder can sign a counter just below the
  ceiling and lock the folder until it is shared again (§3.6).

{% </more> %}

## The main residual risks

The threat model states these as accepted, or as work still to do;
[Risks](@/risks.md) explains each in plain words.

- **Keys**: no revocation; a person left behind keeps what they had (TM-42,
  §3.2, §3.5). A lost device gives up, past its key store, every key it held
  (§3.4).
- **A write key holder** can delete everything past the brake, which the
  archive limits (TM-35), flood the folder, lock it at the counter ceiling and
  forget every deletion at once (§3.6).
- **Clocks and forgetting**: [Clocks in detail](#clocks-in-detail),
  [Forgetting deletions in detail](#forgetting-deletions-in-detail) (TM-40,
  TM-43).
- **Storage devices**: [what they see](#what-a-storage-device-sees); they can
  also replay old copies and broken records (§3.3, TM-41).
- **Visibility**: stable identifiers, the local network, an open port and
  public trackers, as in [What each party sees](#what-each-party-sees) (§5,
  TM-24, TM-26).
- **Hole punching**: the residuals in
  [Hole punching in detail](#hole-punching-in-detail) (TM-25).
- **WebSocket** is classical end to end today and can be forced by blocking UDP
  and raw TCP; a fix is planned for the phase after next (TM-1). Circuits with
  a browser client at one end use classical Noise.
- **Transport limits** on QUIC and TCP resources are required before 1.0
  (TM-22's residuals (a) to (c) and (h)).
- **The browser version** trusts licketysync.app at each load; a broken-into
  host can serve a changed version to those it expects won't check, and keys
  taken stay usable until the folders move. A signed manifest and verifying
  extensions are the checks (§3.8, TM-28).
- **Out of scope**: the operating system and its keystore, malware running as
  the user, sets of files that must change together, denial of service by
  traffic volume, and coercion; there is no duress mode (§3.12).

## Where today's code differs

The specifications, a simulator that runs the sync engine on many simulated
devices, and the `keys`, `protocol`, `index` and `engine` crates exist; nothing
syncs on a real disk or network yet. The index is in memory, files are cut into
fixed-size pieces rather than at key-derived places, and the code still forgets
deletions by the folder's own time and refuses devices whose clocks are more
than a year apart. The next phase replaces those with the signed watermark this
manual describes. Today's engine also holds at most 256 folders on a device and
syncs at most 64 with any one device, and the space freed by dropped pieces can
wait for the next round of syncing, an open question. The
[changelog](https://github.com/LicketySync/core/blob/main/CHANGELOG.md) records
each change.

## Reporting a vulnerability

Report it privately with GitHub's "Report a vulnerability" button on the core
repository; its [security policy](https://github.com/LicketySync/core/blob/main/SECURITY.md)
has the details. A report that shows a threat-model requirement unmet is a
vulnerability.

## Licences

The code of Lickety-Sync and of this site, the logo included, is under the
[Mozilla Public License 2.0](https://www.mozilla.org/en-US/MPL/2.0/). This
site's words and illustrations are under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

## The specifications

- [Threat model](https://github.com/LicketySync/core/blob/main/spec/threat-model.md):
  the adversaries, the numbered requirements and, in §5, what stays visible to
  whom.
- [Protocol](https://github.com/LicketySync/core/blob/main/spec/protocol.md):
  the wire protocol; its last section, §9, explains the index in plain words.
- [Keys](https://github.com/LicketySync/core/blob/main/spec/keys.md): folder
  keys, derivations, key strings and what each device keeps.
- [Suites](https://github.com/LicketySync/core/blob/main/spec/suites.md): every
  algorithm, parameter and approved crate.
- [Allowed hosts](https://github.com/LicketySync/core/blob/main/spec/allowed-hosts.txt):
  the complete list of fixed hosts the programs may contact.
