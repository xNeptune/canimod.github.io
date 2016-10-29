---
layout: default
title: Crossplatforming a SMAPI mod
intro: This page explains how to take an existing mod and make it compatible with Linux, Mac, and Windows. This guide assumes you're already familiar with SMAPI development; if not, see <em><a href="/guides/creating-a-smapi-mod">creating a SMAPI mod</a></em> instead.
---

## Making a mod compatible
_If the mod already uses the crossplatform build configuration (e.g. because you used the
[creating a SMAPI mod](creating-a-smapi-mod) guide to make it), you can skip this section._

Making a mod compile on Linux, Mac, and Windows is pretty straightforward now.

1. Download the mod's source code.  
   <small>_Warning: if you're crossplatforming someone else's mod, get their permission before
   modifying and re-releasing their work._</small>
2. Remove any references to `Microsoft.Xna.*`, Stardew Valley, `StardewModdingAPI`, and
   `xTile`.
3. Reference the [`Pathoschild.Stardew.ModBuildConfig` NuGet package](https://www.nuget.org/packages/Pathoschild.Stardew.ModBuildConfig)
  (see [details](https://github.com/Pathoschild/Stardew.ModBuildConfig#readme)).

That's it! Most mods will compile fine on Linux, Mac, and Windows with those changes. (A few mods
might need further tweaks if they do unusual things.)

## Releasing a mod
Packaging a mod for players on Linux, Mac, and Windows is a bit more work since the game is
implemented differently on Linux/Mac than Windows. There's some first-time setup, but afterwards
preparing mod releases is pretty easy.

Essentially you need to create two versions: one for Windows, and one for Linux and Mac. Each
version needs to be compiled for its target platform. You can do that by compiling one version on
your computer, and the other in a virtual machine.

### First-time setup

#### If your main computer is Windows

1. Install [VirtualBox](https://www.virtualbox.org/).
2. Add [this premade Linux virtual machine](https://www.dropbox.com/s/nrq9xsde2afp4ey/StardewValleyLinuxModding.7z)
   (requires a 64-bit computer).  
   _<small>In VirtualBox, click Machine » Add and choose the downloaded `.vbox` file. This is a
   [Manjaro](https://manjaro.org/) virtual machine with Chromium (web browser), Steam, and
   [MonoDevelop](http://www.monodevelop.com/) preinstalled.</small>_
4. Launch the virtual machine, and install Stardew Valley from the Steam client (preinstalled) or GOG website.  
   _<small>Tip: don't change the default install path, or you'll need to customise the mod's build
   configuration.</small>_

#### If your main computer is Linux or Mac

1. Install [VirtualBox](https://www.virtualbox.org/).
2. [Create a VM with Windows](http://www.macworld.co.uk/how-to/mac-software/run-windows-10-on-your-mac-using-virtualbox-3621650/).
3. Install [Visual Studio Community](https://www.visualstudio.com/vs/community/) in your VM.
4. Install Stardew Valley in your VM.

### Preparing a mod release
1. Compile one version on your main computer.
2. Compile another version in your virtual machine.
3. Create three archives with your mod's name, version, and platform. (To reduce confusion, it's
   better to create separate packages for Linux and Mac even though they're identical.)

You should end up with three archive files like this:

```
PineappleMod-1.2-Windows.zip
   PineappleMod/
      PineappleMod.dll
      PineappleMod.pdb
      manifest.json

PineappleMod-1.2-Linux.tar.gz
   PineappleMod/
      PineappleMod.dll
      PineappleMod.mdb
      manifest.json

PineappleMod-1.2-Mac.tar.gz
   PineappleMod/
      PineappleMod.dll
      PineappleMod.mdb
      manifest.json
```

Done! For more information on releasing your mod, see _[creating a SMAPI mod: releasing your mod](creating-a-smapi-mod#releasing-your-mod)_.

## Experimental alternative: cross-compiling
An alternative approach is to compile both versions from the same machine. This works in theory,
but hasn't been extensively tested.

### From Linux or Mac
1. Download the [build scripts](https://github.com/rumangerst/StardewValleyMisc/releases/tag/crosscompile-linux-1.0)
   for cross-compiling on Linux.
2. Open the `README.md` file and follow the instructions.  
   _<small>You'll need to gather some files like the XNA runtime (You can use WINE), SMAPI and
   other libraries. Also do this for Linux. For Mac you can just copy the files for Linux since
   they're nearly identical.</small>_

The build scripts include support for [SilVerPLuM](https://github.com/rumangerst/SilVerPLuM) if you
edit the entries in the Silverplum directory to match your mod. This is optional.

### From Windows
This isn't supported yet. In theory the build scripts just need to be ported to Windows.