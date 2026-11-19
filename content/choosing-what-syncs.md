+++
title = "Choosing what syncs"
description = "Leave files out with an ignore file, and see what never syncs and what travels with each file."
+++

Everything in a shared folder syncs, unless you say otherwise. To leave
things out, such as caches, build output or your editor's scratch files,
you'll list them in an **ignore file**. Ignore files are being built now,
with syncing on one network from the command line.

{{ <illustration src="choosing-ignore.svg" alt="Three file cards travel along a dotted path towards a yellow folder. A paper list stands in the way like a gate: two cards pass through into the folder, and the third, marked with a cross, stays outside." width={440} height={220} /> }}

## The ignore file

An ignore file is a plain text file named `.syncignore`. It can sit in any
folder inside the shared folder, and it covers that folder and everything
below it; a deeper one overrides a shallower one. It lists one pattern per line, like these (the rules are the same as
git's `.gitignore`):

```
# Lines starting with # are notes
*.tmp
build/
/todo.txt
**/cache/
!keep.tmp
```

- `*.tmp` leaves out every file ending in `.tmp`, at any depth.
- `build/` leaves out every folder named `build`, with everything in it.
- `/todo.txt` leaves out `todo.txt` only beside the ignore file, not deeper
  down.
- `**/cache/` leaves out a `cache` folder at any depth.
- `!keep.tmp` brings back a file an earlier line left out.

The app will open these files for you to edit, show the rules that apply to
any file and say why a file is left out. A starter list will cover the usual
clutter: `.DS_Store`, `Thumbs.db`, `desktop.ini` and editors' swap files.

### It syncs, so everyone shares it

A `.syncignore` file is an ordinary file in the folder, so every device that
holds the folder follows the same rules. That also means anyone with the
write key can change it, and so decide what syncs for everyone.

### Your device's own list

For junk that only one device makes, each device will also have a list of
its own that doesn't sync. LicketySync will follow both: the folder's files and
your device's list, added together.

## When a synced file becomes ignored

Adding a file that already syncs to an ignore list doesn't delete it
anywhere: every device keeps its copy, and changes simply stop travelling.
To get rid of such files everywhere, delete them first, wait until every
device has caught up, then add them to the list.

## What never syncs

- **LicketySync's own hidden folder**, `.licketysync`, at the top of each
  shared folder. It holds a marker, the folder's ID (never a key), this
  device's settings for the folder, its archive and downloads still in
  progress. The list of files that LicketySync keeps lives elsewhere, in the
  app's own data folder.
- **A shared folder inside another.** The outer one leaves it alone, and the
  app will show it as synced separately, so the two can have different keys
  and people.
- **A drive plugged in inside the folder.** LicketySync won't sync into it,
  and will say so.
- **Shortcuts to other places (symbolic links).** At first they're skipped,
  with a notice. Before version 1.0 they'll sync as links wherever the system
  supports them, and LicketySync will never follow one.
- **A file whose name your system can't hold**, such as a name with a `:` on
  Windows, can't be written there. LicketySync keeps the entry, shows it as “cannot be stored here” and never
  treats it as deleted. An optional escaping scheme, per device and off by
  default, will be able to store such names under another spelling.

## What travels with a file

Its contents and its modified time, always. Its permissions, where the disk
supports them (on Windows, just “read-only”), unless you turn that off.

{% <more summary="The rest of a file’s details"> %}

- **Only if you turn them on:** the file's owner and group, by name, and its
  extended attributes from a short allowed list (macOS resource forks among
  them, but not the attributes macOS adds by itself).
- **Never:** folders' own times, Windows' access lists and NTFS alternate data
  streams. Hard links arrive as separate files.

{% </more> %}

## Choosing what each device keeps

Later, you'll be able to keep only some of a folder's files on a device. The
device will still list every file, and fetch any of them when you ask, ahead
of everything else. “Free up space” will remove a file only while another
connected device holds a full copy, so the copy you free up is never the
only one around. The
devices you share the folder with will be able to tell which files you keep.
[Features](@/features.md#devices) shows when it comes.

{% <note> %}
**For the curious.** The threat model's
[TM-4](https://github.com/LicketySync/core/blob/main/spec/threat-model.md#4-requirements)
says how every file on the disk is reached, ignore files included: through
one path checker that never follows a link.
{% </note> %}
