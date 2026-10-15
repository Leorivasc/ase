
# ASE = Atari System Emulator

![Logo.](Images/logo.png "Logo.")

**Atari System Emulator** or **ASE** is an **Atari ST** emulator that emulates the **ST F/FM**, the **Mega ST** and the **STE**. It is written in C# and utilizes the great **Motorola 68000 Moira** emulator created by **Dr. Dirk Hoffmann**.

ASE is designed to deliver a fresh Atari ST emulation experience, allowing you to enhance your favorite ST games and applications beyond the capabilities of the original hardware, without requiring any modifications to the software itself.

ASE makes it easier than ever to rediscover the games you grew up playing, with support for **cover art, videos, and screenshots** to bring your collection to life.

It also features a **save state system**, allowing you to pick up exactly where you left off at any time.

📖 **[Check out the Wiki](https://github.com/thebitculture/ase/wiki)** to explore all the features ASE has to offer!

This emulator has its roots in an old project I started developing in C and assembly language back in the late 1990s. However, the current version is a mix of old and new code. Most of it has been rewritten from scratch, while some parts have been adapted from **Hatari** and the **MiSTer Atari ST core**.

The original idea behind ASE was to create an educational project that would help people understand how emulators actually work. I explore this topic on my YouTube channel, ***The Bit Culture***, where I walk through the fundamentals of emulation and share insights into the development process.

If you're curious about what goes on behind the scenes of an emulator, I invite you to check out the video and follow the project's progress.

And there's more to the channel than just emulators! We also explore retro hardware, classic games, and the fascinating world of vintage computing.

👉 https://youtube.com/@thebitculture?si=2s4M5Iu4QbIdq_hn


## Minimum Requirements

It runs on **Windows**, **Mac**, and should also work on **Linux**, although I haven't tested it. **ASE** will work with any **TOS** version from 1.00 to 1.04 on the ST/Mega models, and from 1.06 to 2.06 on the STE.

## Downloads

You can download the latest release ready for use from the **[releases page](https://github.com/thebitculture/ase/releases/)**, or better yet, you can download the source code to compile and experiment yourself. To compile the source code, you will need to download **Moira** from **Dr. Dirk Hoffmann's** repository and copy only the 68k emulator into that directory, excluding the `MoiraConfig.h` configuration file. In the **ASE** **Moira** directory, you will find `Moira_dotnet.cpp` and `Moira_dotnet.h`, which are responsible for exposing Moira to the C# wrapper, as well as the files to compile **Moira** (`build.cmd` and `build.sh`), depending on the OS you use.

To compile **Moira** on **Windows**, you will need to have `cmake.exe` in your system path or open a **Visual Studio** developer console.

### Native libraries layout

The native libraries live under `ASE/native/`, organized **per .NET RID** (operating system + architecture). When publishing, `ASE.csproj` copies **only** the library matching the target RID next to the executable:

```
ASE/native/
  win-x64/      moira.dll, SDL2.dll, mt32emu-2.dll
  osx-x64/      moira.dylib, libSDL2.dylib, libmt32emu.2.dylib
  osx-arm64/    moira.dylib, libSDL2.dylib, libmt32emu.2.dylib
  linux-x64/    moira.so, libmt32emu.so.2
  linux-arm64/  moira.so, libmt32emu.so.2, libtinyfiledialogs.so
```

`SDL2` is bundled only on Windows and macOS; on **Linux** the distribution's own SDL2 is used. `libmt32emu` (see below) is committed **already compiled** for every RID, so it never has to be built as part of ASE. The build scripts place the compiled **Moira** library straight into the matching `native/<rid>/` folder:

- `build.cmd` / `build.sh` with no argument build for the current machine.
- To target another platform, pass its RID: `./build.sh osx-x64`, `./build.sh linux-arm64`, or `build.cmd ARM64` (cross-compiling requires the corresponding toolchain). See `ASE/native/README.md` for details.

Then publish for that RID, e.g. `dotnet publish -c Release -r linux-arm64 --self-contained`.

### Roland MT-32 emulation

The **Roland MT-32** (and its CM-32L / LAPC-I relatives) is the module a good number of Atari ST games and demos were composed for, through the MIDI ports every ST shipped with. ASE emulates the ST's MIDI interface and can either bridge it to the host's real MIDI ports (*System managed MIDI*) or play it through a built-in MT-32, whose synthesis comes from **Munt**'s `libmt32emu`:

👉 https://github.com/munt/munt

You don’t need to build this library; the binary is already included in this repository. However, if you still want to build it yourself, take a look at `ASE/native/README.md`.

`libmt32emu` is distributed under the **LGPL v2.1 or later**. The **MT-32 ROM images are not included**: just like TOS, they are copyrighted (by Roland) and have to be supplied by the user.

### EmuTOS

ASE ships with **EmuTOS** 1.4 (`ASE/emutos.rom`, the 256 KB image in English), the free replacement for TOS written by the EmuTOS development team, so the emulator boots out of the box without a ROM of your own. EmuTOS is distributed under the **GNU GPL**: its licence text and the link to its source code travel next to it (`emutos-license.txt`, `emutos.txt`). For the best compatibility use an original TOS dumped from your own machine.

👉 ASE Releases: https://github.com/thebitculture/ase/releases/

👉 Moira Releases: https://github.com/dirkwhoffmann/Moira/releases

👉 Munt (libmt32emu): https://github.com/munt/munt

👉 EmuTOS: https://emutos.sourceforge.io

---

(C) _The Bit Culture_ - Public License Version 3
