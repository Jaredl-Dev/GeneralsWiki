# Visual Studio 2022 and 2026

This guide uses Visual Studio's built-in CMake support to build GeneralsGameCode with a modern compiler. The shared
presets, options, targets, output paths, and installation steps are documented in the
[Building with CMake guide](cmake_guide).

Visual Studio 2022 and 2026 use the same steps.

## Requirements

- Windows
- [Visual Studio 2022 or 2026](https://visualstudio.microsoft.com/downloads/)
- [Git](https://git-scm.com/downloads)

In Visual Studio Installer, select **Desktop development with C++** and enable:

- **Windows 11 SDK (10.0.26100.x)**
- **C++ CMake tools for Windows**

For Visual Studio 2022, also enable:

- **MSVC v143 - VS 2022 C++ x64/x86 build tools (Latest)**
- **C++ MFC for latest v143 build tools (x86 & x64)**

For Visual Studio 2026, enable these v143 compatibility components:

- **MSVC v143 - VS 2022 C++ x64/x86 build tools (v14.44)**
- **C++ v14.44 (17.14) MFC for v143 build tools (x86 & x64)**

Standalone CMake and Ninja installations are not required when building through the IDE.

## Clone and open the source

Clone the repository:

```batch
git clone https://github.com/TheSuperHackers/GeneralsGameCode.git
```

In Visual Studio, select **File > Open > Folder** and open the cloned `GeneralsGameCode` directory. Visual Studio reads
`CMakePresets.json` and starts configuring the project. The first configure downloads several dependencies, so it
requires an internet connection.

## Select a preset

On the Visual Studio toolbar, select **Local Machine**, then choose matching configure and build presets:

| Build   | Configure preset      | Build preset                |
| ------- | --------------------- | --------------------------- |
| Release | Windows 32bit Release | Build Windows 32bit Release |
| Debug   | Windows 32bit Debug   | Build Windows 32bit Debug   |
| Profile | Windows 32bit Profile | Build Windows 32bit Profile |

Wait for the CMake output to report that generation finished before building.

> **Retail compatibility:** Win32 builds are not compatible with retail multiplayer or replays.

## Build

Select **Build > Build All** to build every enabled target.

To build one target, switch Solution Explorer to **CMake Targets View**, right-click a target such as `g_generals` or
`z_generals`, and select **Build**.

Release game executables are written to:

- `build/win32/Generals/Release/generalsv.exe`
- `build/win32/GeneralsMD/Release/generalszh.exe`

## Install

Run each installed game at least once so CMake can find its directory from the Windows registry. In **CMake Targets
View**, right-click the top-level `install` target and select **Build**. This builds the active configuration if needed,
then copies the enabled executables and debug symbols into the detected game directories.

See [Install](cmake_guide#install) to set the game directories manually. Administrator permission may be
required when a game is installed under `Program Files`.

## Run

On the Visual Studio toolbar, open **Select Startup Item** and choose `g_generals` or `z_generals`. Select the green
**Start** button or press **F5** to build and run it with the debugger. To run without the debugger, select **Debug >
Start Without Debugging** or press **Ctrl+F5**.

See the [Building with CMake guide](cmake_guide) for Debug and Profile output, game and tool selection, and
individual targets.

## Troubleshooting

### A preset is missing

Confirm that Visual Studio opened the repository root containing `CMakePresets.json`. If needed, enable CMake Presets
under **Tools > Options > CMake > General**, close the folder, and open it again.

### MFC headers or libraries are missing

Open Visual Studio Installer, modify the installation, and add the MFC component matching your Visual Studio version
from the [requirements](#requirements). In Visual Studio 2026, confirm that the v143 compatibility MFC component is
enabled.

### Configuration or dependency download fails

Check the CMake output for the first error. Confirm that Git can access GitHub, then select **Project > Delete Cache and
Reconfigure**.

### A path is too long

Move the repository closer to the drive root, such as `C:\GeneralsGameCode`, then delete the CMake cache and
reconfigure.
