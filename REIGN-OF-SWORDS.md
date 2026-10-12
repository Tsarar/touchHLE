# Playing Reign of Swords on touchHLE (this fork)

This fork of [touchHLE](https://github.com/touchHLE/touchHLE) runs the original iPhone games **Mobile Battles: Reign
of Swords** (2008) and **Reign of Swords Episode II** (2009) by Punch Entertainment on a Windows PC. The official
touchHLE builds crash when these games start; the fixes are on the `reign-of-swords` branch (see
[What the fork changes](#what-the-fork-changes)).

No game files are included. You need your own copy of each game's `.ipa`.

- [What you need](#what-you-need)
- [1. Get touchHLE (the fork)](#1-get-touchhle-the-fork)
- [2. Add the game](#2-add-the-game)
- [3. Start the game](#3-start-the-game)
- [4. Controls](#4-controls)
- [5. Saves](#5-saves)
- [Known problems](#known-problems)
- [Troubleshooting](#troubleshooting)
- [Building it yourself](#building-it-yourself)
- [What the fork changes](#what-the-fork-changes)

## What you need

- **Windows 10 or 11, 64-bit.** (touchHLE also runs on macOS, Linux and Android, but this fork has only been built
  and tested on Windows.)
- **A graphics card with OpenGL 2.1 or newer.** Any PC from the last ten years has one.
- **The game's `.ipa` file**, unencrypted: an App Store download is encrypted (FairPlay) and won't run in touchHLE.
  These builds were tested and start:

  | Game | Version | Bundle id (shown at start) |
  | --- | --- | --- |
  | Reign of Swords | 1.2 | `CCU8RRT36N.punch-entertainment.com` |
  | Reign of Swords Episode II | 1.0.11 | `CCU8RRT36N.punch-entertainment.ros-ep2` |

## 1. Get touchHLE (the fork)

**Easiest: the ready-made build.**

1. Open this fork's **Releases** page on GitHub.
2. Download `touchHLE_reign-of-swords_Windows_x86_64.zip`.
3. Unpack it anywhere, for example to `C:\Games\touchHLE`. Keep the folder together: `touchHLE.exe` needs the
   `touchHLE_dylibs` and `touchHLE_fonts` folders next to it.

You should now have:

```
C:\Games\touchHLE\
  touchHLE.exe
  touchHLE_apps\          ← your games go here
  touchHLE_dylibs\
  touchHLE_fonts\
  touchHLE_default_options.txt
  touchHLE_options.txt
  OPTIONS_HELP.txt
  README.md
  COPYING.txt
```

Don't use the official touchHLE download for these games: it crashes at launch (see
[Troubleshooting](#troubleshooting)).

**Or build it yourself**: see [Building it yourself](#building-it-yourself).

## 2. Add the game

Copy your `.ipa` file(s) into the `touchHLE_apps` folder. The file name doesn't matter; you can put both episodes
there.

```
C:\Games\touchHLE\touchHLE_apps\
  Reign_v1.2_os20.ipa
  Reign of Swords - Episode II 1.0.11.ipa
```

## 3. Start the game

**From the app picker:** double-click `touchHLE.exe`. A window lists the apps in `touchHLE_apps`; click the game.

**Or straight from a command line** (useful for options and for reading errors):

1. Open the `touchHLE` folder in File Explorer.
2. Click the address bar, type `cmd` and press Enter. A command prompt opens in that folder.
3. Run, with your file name:

   ```
   touchHLE.exe "touchHLE_apps\Reign_v1.2_os20.ipa"
   ```

What you should see:

1. A short dark pause where the Punch Entertainment logo video would be (it doesn't play here, see
   [Known problems](#known-problems)).
2. The game's loading screen, then the main menu, with music.

Useful options (put them before the file name):

| Option | What it does |
| --- | --- |
| `--fullscreen` | full screen instead of a window, the picture scaled to fit (quit with Alt+F4) |
| `--scale-hack=2` | renders at 2× the internal resolution (sharper; `3` or `4` for large screens) |
| `--landscape-left`, `--landscape-right` | rotate the screen if a game ever starts sideways |
| `--print-fps` | prints the frame rate to the console once a second |

For example: `touchHLE.exe --fullscreen --scale-hack=2 "touchHLE_apps\Reign_v1.2_os20.ipa"`.

To make options permanent, add a line to `touchHLE_options.txt` with the game's bundle id and the options:

```
CCU8RRT36N.punch-entertainment.com: --scale-hack=2
CCU8RRT36N.punch-entertainment.ros-ep2: --scale-hack=2
```

All options are explained in `OPTIONS_HELP.txt`.

## 4. Controls

- **Left mouse button = your finger.** Click to tap, hold and drag to drag (scrolling the map, dragging units).
- Close the window to quit. The game saves as it goes, as on the iPhone.
- A game controller can move a virtual cursor (right stick; press the stick or the right shoulder button to tap).

## 5. Saves

Saves live next to `touchHLE.exe`, in `touchHLE_sandbox`, one folder per game:

```
touchHLE_sandbox\CCU8RRT36N.punch-entertainment.com\Documents\      ← Episode I
touchHLE_sandbox\CCU8RRT36N.punch-entertainment.ros-ep2\Documents\  ← Episode II
```

Copy the `touchHLE_sandbox` folder to back them up, or to move them to another PC. Delete a game's folder to start
that game over.

## Known problems

- **The opening logo video doesn't play.** touchHLE can't play video yet; you get a few seconds of black, then the
  game continues.
- **Online features don't work.** The games' mobilebattles.com services (online battles, rivals, army upload) were
  shut down long ago.
- **Only the start of the games has been tested** so far: launch, loading, the main menu and music. If something
  breaks later in a battle or a menu, please report it (see below).

## Troubleshooting

**It crashes at launch with "Attempted null-page access at 0x4".**
You're running the official touchHLE, not this fork. Use the build from this fork's Releases.

**The picture is upside down, or red looks blue.**
Same cause: an older build or the official one. This fork converts the game's frames to the right orientation and
colours.

**"The executable is encrypted. touchHLE can't run encrypted apps!"**
The `.ipa` is an encrypted (FairPlay) App Store copy. touchHLE can only run unencrypted copies.

**The game doesn't appear in the app picker.**
Check that the file ends in `.ipa` and sits directly in `touchHLE_apps`, not in a subfolder.

**Nothing happens when I double-click `touchHLE.exe`.**
Run it from a command prompt (step 3) to see the error. A missing `touchHLE_dylibs` or `touchHLE_fonts` folder
next to the `.exe` is the usual cause: unpack the whole zip.

**After the title screen only part of the picture shows, shifted to a corner, the rest black.**
That's an older build of this fork. Download the latest release.

**Reporting a problem.** Run the game from a command prompt, reproduce the problem, and attach `touchHLE_log.txt`
(written next to `touchHLE.exe`) to an issue on this fork. Don't report problems that only happen in this fork to the
official touchHLE project.

## Building it yourself

Tested on Windows 11 with Visual Studio Community 2026 (version 18). It takes about 30–60 minutes the first time,
most of it downloads.

1. **Install the C++ tools.** In the Visual Studio Installer, add the workload **Desktop development with C++**
   (it includes the MSVC compiler, the Windows SDK, CMake and Ninja).
2. **Install Rust** from <https://rustup.rs> (`rustup-init.exe`, the default options are fine).
3. **Get the source with its submodules:**

   ```
   git clone --branch reign-of-swords https://github.com/Tsarar/touchHLE.git
   cd touchHLE
   git submodule update --init --recursive
   ```

4. **Add Boost.** Download a Boost source zip (1.86.0 works) from <https://www.boost.org/users/download/>, unpack it,
   and move the `boost_1_86_0` folder to `vendor\boost` (so that `vendor\boost\boost\` exists).
5. **Keep the path short.** Windows limits paths to 260 characters and the C++ dependencies nest deeply. Put the
   repository near the root of a drive (for example `C:\src\touchHLE`), or map a drive letter to it:
   `subst T: C:\path\to\touchHLE`.
6. **Build.** Visual Studio 2026 is newer than the build helper touchHLE uses, so build through Ninja. Save this as
   `build.bat` next to the repository folder and adjust the paths:

   ```bat
   @echo off
   call "C:\Program Files\Microsoft Visual Studio\18\Community\VC\Auxiliary\Build\vcvars64.bat" >nul
   set PATH=%USERPROFILE%\.cargo\bin;C:\Program Files\Microsoft Visual Studio\18\Community\Common7\IDE\CommonExtensions\Microsoft\CMake\Ninja;%PATH%
   set CMAKE_GENERATOR=Ninja
   rem CMake 4 refuses the old minimum versions in SDL and OpenAL Soft without this:
   set CMAKE_POLICY_VERSION_MINIMUM=3.5
   set CC=cl
   set CXX=cl
   cd /d C:\src\touchHLE
   cargo build --release
   ```

   Run it. The result is `target\release\touchHLE.exe`.
7. **Run it from the repository folder**, so it finds `touchHLE_dylibs` and `touchHLE_fonts`:

   ```
   cd C:\src\touchHLE
   target\release\touchHLE.exe "touchHLE_apps\Reign_v1.2_os20.ipa"
   ```

With an older Visual Studio (2022) the `CMAKE_GENERATOR` line isn't needed; the rest is the same.

## What the fork changes

Each fix is one commit on the `reign-of-swords` branch:

1. **The main nib's top-level objects stay alive** for the app's lifetime, as on iPhone OS. The game's
   `MainWindow.nib` holds a stray `iCanvas` view; touchHLE released it right after launch, and the game crashed in
   its `-dealloc`.
2. **A view that overrides `drawRect:` is drawn once when it's created**, as on iPhone OS. The game creates its
   renderer in `drawRect:` before its render timer first fires.
3. **Pixels written straight into a layer's bitmap are converted.** The game copies each frame into the memory it
   gets from `CGBitmapContextGetData`, in iPhone OS's layout (BGRA, rows top to bottom); touchHLE keeps layers as
   RGBA with rows bottom to top, so the picture came out upside down with red and blue swapped. The buffer is put
   into iPhone OS's layout when the app takes the pointer and back when the drawing pass ends.
4. **Only the dirty rect is redrawn.** `setNeedsDisplayInRect:` records the rect, and a redraw clears and passes to
   `drawRect:` only that part, rounded out to whole pixels and kept inside the view; the rest of the view keeps what was
   drawn before, as on iPhone OS. After the title
   screen the game redraws only what changed; before this fix the rest of the screen went black.

These changes were made with the help of an AI assistant. touchHLE's contribution rules don't allow that, so they
are not offered to the official project. If you maintain touchHLE and want to fix the same problems, the
descriptions above are all you need; please write your own implementation.

Reign of Swords and its art, music and data belong to their respective owners. This fork contains none of them.
