+++
title = "Features"
description = "What LicketySync will do, in plain words, and whether each part is being built now or comes later."
+++

Nothing on this page ships yet: LicketySync is still being built, and there's
nothing to download. Here's what it will do. Each feature says whether it's
being built now, coming next or coming later;
[What's ready and what's next](@/whats-next.md) has the order, without dates.

## Syncing

<div class="cards">
<div class="card">

### Folders that stay the same

Every device that holds one of a folder's keys will keep its own copy, with
changes travelling from device to device. No account, and nothing in between
but a relay, when one is needed and relays are on.

<p class="status">Being built now</p>

</div>
<div class="card">

### On your network

With finding devices on your local network on, as setup will suggest,
computers on the same home or office network will find each other and sync,
with no internet needed.
LicketySync will keep track of where every file stands: in sync, or still on
its way.

<p class="status">Being built now</p>

</div>
<div class="card">

### Anywhere on the internet

Devices will find each other through public trackers (the DHT). Where a
helper is available, they'll try hole punching, which gets through most home
routers, not all, and when nothing else works, they'll fall back to public
relays. Setup will
suggest all three, and each is your choice:
[Setting up](@/setting-up.md#how-setup-will-ask) explains them.

<p class="status">Coming next, relays later</p>

</div>
<div class="card">

### Leave things out

An ignore file will list the things in a folder you'd rather not sync, with
the same rules as git's: [Choosing what syncs](@/choosing-what-syncs.md).

<p class="status">Being built now</p>

</div>
</div>

## Sharing

<div class="cards">
<div class="card">

### Three keys

A write key to read and change everything, a read key to see everything and
change nothing, and a sealed key to keep an encrypted copy without opening it
(storage devices that use it come next). Whoever holds a key can pass on that
key or a lower one.

<p class="status">Being built now</p>

</div>
<div class="card">

### Links and files

A key will travel as a link or a small file, from the command line. The write
key can also be written down as 24 words, and showing it will ask for your
device's own password, PIN or fingerprint.

<p class="status">Being built now</p>

</div>
<div class="card">

### Moving to new keys

There's no taking a key back. To remove someone, the people who stay will move
to a new folder with new keys, over the files they already have. The person
removed keeps the old folder and everything they had, any device still on the
old folder keeps syncing with them, and storage devices fetch the new folder
from scratch.

<p class="status">Being built now</p>

</div>
<div class="card">

### Sharing widely

With public trackers on at both ends, a read key can go to people you've never
met, and you'll be able to add your addresses to an invite. Whoever holds the
key will see what any holder sees, such as which device changed each file and
when, to the microsecond, and can pass it on for good.
[Sharing](@/sharing.md#sharing-more-widely) has the rest.

<p class="status">Coming next</p>

</div>
</div>

## It's got your back

<div class="cards cards-3">
<div class="card">

{{ <illustration src="features-conflict-copy.svg" alt="" width={120} height={90} /> }}

### Conflict copies

When two people change the same file at once, both versions are kept, one as a
[conflict copy](@/conflicts-and-the-archive.md) beside the file. Nothing is
merged or lost.

<p class="status">Being built now</p>

</div>
<div class="card">

{{ <illustration src="features-archive.svg" alt="" width={120} height={90} /> }}

### The archive

When a change from another device replaces or deletes a file, the old one waits
in this device's archive, 30 days unless you change it. Phones keep one too.
Other ways to keep old versions come later.

<p class="status">Being built now</p>

</div>
<div class="card">

### The deletion brake

If lots of files vanish from your disk at once, say from an unplugged drive,
the folder will pause and ask you before telling your other devices.

<p class="status">Being built now</p>

</div>
</div>

## Devices

<div class="cards cards-3">
<div class="card">

{{ <illustration src="features-storage-device.svg" alt="" width={120} height={90} /> }}

### Storage devices

A device with only the sealed key keeps and passes on an encrypted copy it
can't open. It still sees the folder's shape: how many files, roughly how big,
and when and from which device each changes. Someone running it who knows some
of the folder's files, or got files of their choosing into it, can in time test
whether other files they have are in it.

<p class="status">Coming next</p>

</div>
<div class="card">

### Linked devices

You'll add a device of your own by scanning a code it shows, and your other
device will send it the keys of the folders you pick.

<p class="status">Coming later</p>

</div>
<div class="card">

### Choose what each device keeps

You'll be able to keep only some of a folder's files on a device, and fetch
the others when you need them. The devices you share it with can tell which
ones ([more](@/choosing-what-syncs.md#choosing-what-each-device-keeps)).

<p class="status">Coming later</p>

</div>
</div>

## Apps

<div class="cards cards-3">
<div class="card">

### The command line

First comes a program you run from the command line on your computers. It will
export keys as links and files.

<p class="status">Being built now</p>

</div>
<div class="card">

{{ <illustration src="features-apps.svg" alt="" width={120} height={90} /> }}

### Apps for computers and phones

Apps with a friendly window, and keys shown as QR codes. On a computer, some
warnings will also show as a notification.

<p class="status">Coming later</p>

</div>
<div class="card">

{{ <illustration src="features-browser.svg" alt="" width={120} height={90} /> }}

### The browser version

A shared folder in your web browser, with nothing to install. It will load from
licketysync.app each time, so you'll be trusting that site. An invite meant for
the browser version, and a one-time file link, will carry your addresses.

<p class="status">Coming later</p>

</div>
</div>

## Built in from the start

These are how LicketySync is designed, so every feature above has them.

- **Encrypted everywhere.** Every connection is encrypted, and files and their
  names are encrypted before they leave a device.
- **Signed changes.** Every change is signed with the folder's write key, and
  devices accept a change only when it is.
- **No half-written files.** A file arrives in a temporary place, is checked
  and only then takes the old one's place.
- **Nothing to sign up for.** The programs you install have no server of their
  own to call: no account, no telemetry (reports about how you use it), no
  update checks.
- **Plain notices.** When something goes wrong, the device will say once what
  happened, why and how to fix it
  ([When something goes wrong](@/when-something-goes-wrong.md)).
- **Open source.** The code and the design specifications are on
  [GitHub](https://github.com/LicketySync), under the Mozilla Public License
  2.0.

Curious about the trade-offs? [Risks](@/risks.md) explains where your risks are
and how they'd show up.
