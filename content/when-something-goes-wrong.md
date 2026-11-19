+++
title = "When something goes wrong"
description = "A full disk, a file it can’t open, a limit your system sets: how LicketySync will tell you, keep going and help you fix it."
+++

Sometimes your device gets in the way: the disk fills up, a file can't be
opened, or the system runs short of something LicketySync needs. When that
happens, LicketySync will keep syncing what it can, tell you once in plain
words, try again by itself and show you how to fix the rest. This is being
built now, with syncing on one network from the command line; the apps will
show the same notices when they come.

{{ <illustration src="problems-notice.svg" alt="A laptop showing a single notice card with an exclamation mark; beside it, a round arrow that means trying again and a sheet of steps with a tick, for the page that explains the fix." width={400} height={220} /> }}

## What it will do

- **Keep syncing.** A problem costs one thing, such as watching for changes
  as they happen, never a whole folder in silence and never your files.
- **Tell you once, plainly.** One notice per problem, not a hundred: what
  happened, what it means for you and how to fix it, with the steps for your
  system. It will go away by itself once the problem does.
- **Try again by itself.** It will retry, more slowly each time, and check at
  once when something changes: space freed, a limit raised, the network back,
  the device awake.
- **Change nothing behind your back.** It will never raise a system limit,
  change a system setting or ask for administrator rights by itself. When a
  fix needs one, it will show you the exact command, and you choose whether to
  run it.
- **One problem, one page.** Each problem will have a short code and a page on
  this site with the steps for each system, written for people who don't use a
  terminal. The same advice will be built into the program, so it works
  offline: the program shows the page's address and contacts nothing.

## Check before it bites

`licketysync doctor`, on the command line, will look for these problems
before they happen, such as a folder too big to watch, and print the same
advice.

## The first one: too many folders to watch

On Linux, the system limits how many folders one person's programs may watch
for changes, all together (the watch limit). Editors, other sync programs and LicketySync all
draw from the same allowance, and LicketySync needs one for every folder
inside a shared folder (ignored ones don't count).

- **Before it opens a folder**, LicketySync will count what it needs, read the
  limit and work out what your other programs already use. If the folder
  won't fit, it will say so first.
- **It will leave room for your other programs.** It takes only part of
  what's free, so even a folder with a huge number of subfolders never starves
  your editor.
- **If the allowance runs out**, it will keep every watch it has and check the
  rest of the folder on a timer instead, such as every 10 minutes. The notice
  will say so, give the command to raise the limit and link to its page here.
- **Once there's room again**, it will watch the rest, and the notice will
  clear by itself.

## Other problems it will handle

- {{ <lead_in text="Disk full:" /> }} pauses downloads into that folder, keeps
  serving what it has and carries on when space returns. You free some space.
- {{ <lead_in text="A file it isn’t allowed to read or write:" /> }} skips
  that file, shows it in the folder's status and tries again later. You fix
  its permissions.
- {{ <lead_in text="Too many files open at once:" /> }} raises its own limit as far as
  the system allows, uses fewer files and slows down rather than fails.
- {{ <lead_in text="A drive that never says when a file changes:" /> }}
  network drives and some others don't tell programs about changes, so
  LicketySync checks them on a timer, and says so.
- {{ <lead_in text="Another program using LicketySync’s port:" /> }} it takes
  another port by itself. If a firewall (the part of your system that blocks
  unwanted connections) stops devices on your network from finding yours, it
  shows the steps for your system.
- {{ <lead_in text="Sleep and wake:" /> }} checks the folder again after the
  device wakes.

On a Linux computer these come first. Problems that only Macs, Windows
computers and phones have come with their apps.

## Help that's on its way

Each problem's page appears on this site together with the change that
detects it, starting with the watch limit. Until then, nothing here is
something you need to fix: there's nothing to download yet.
