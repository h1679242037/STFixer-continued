# STFixer - SteamTools Save and Offline Setup Repair Utility

Community continuation of STFixer, originally created by @Selectively11.

STFixer is a Windows utility for repairing common SteamTools-related issues, including broken saves in some Capcom games, first-time SteamTools setup when backend services are unavailable, and missing or mismatched SteamTools DLLs.

> Disable Steam Cloud for affected non-owned games and manually back up saves before applying patches.

## Download

Download `STFixer.exe` from the latest release:

https://github.com/h1679242037/STFixer-continued/releases/latest

## What It Fixes

- Capcom games that cannot create or write saves because of SteamTools cloud behavior.
- SteamTools first-time setup when its backend is down.
- SteamTools Desktop overwriting patched DLLs on startup.
- Missing, outdated, or mismatched `xinput1_4.dll` and `dwmapi.dll`.

## Usage

1. Close Steam and SteamTools.
2. Run `STFixer.exe`.
3. Confirm the detected Steam install path, or enter the correct one.
4. Choose the patch you need.
5. Restart Steam and SteamTools when finished.

To undo changes, run STFixer again and select **Disable Everything**.

## Menu Options

### 1. Setup SteamTools Offline

Patches SteamTools setup behavior so a new or repaired SteamTools installation can work even when its backend server is unavailable.

### 2. Capcom Game Save Fix

Disables the SteamTools cloud behavior that can prevent some Capcom games from creating saves. If saves still fail after this patch, disable Steam Cloud for the affected game, clear that game's userdata folder, restart Steam, and test again.

Userdata path:

```text
<Steam install path>\userdata\<steamid>\<appid>
```

### 3. Patch SteamTools App

Patches `SteamTools.exe` so SteamTools Desktop does not overwrite STFixer-patched DLLs when it starts.

### 4. Repair SteamTools DLLs

Downloads fresh SteamTools DLLs and replaces existing copies. Use this when DLLs are missing, mismatched, or corrupted. If you already applied option 2, avoid running option 4 afterward unless you plan to re-apply option 2.

### 5. Disable Everything

Restores original files from backups created by STFixer.

## Notes

- Backups are created before patching.
- STFixer auto-detects Steam from the Windows registry, but the path can be overridden.
- SteamTools or Steam updates may require rerunning STFixer.
- This fork is maintained as a continuation after the upstream repository was archived. Authorization/license clarification is pending.

## Building

Requirements:

- .NET 9 SDK
- Windows x64 build environment
- Visual Studio Build Tools or MinGW for `Stella\stella_fallback.dll`

Build the Stella fallback DLL first, then publish:

```powershell
cd Stella
.\build.bat
cd ..
dotnet publish .\CloudFix.csproj -c Release
```

If you are using MinGW instead of Visual Studio Build Tools:

```powershell
cd Stella
x86_64-w64-mingw32-gcc -shared -O1 -Wall -Wextra -o stella_fallback.dll stella_fallback.c stella_fallback.def -lwinhttp '-Wl,--subsystem,windows' '-Wl,--out-implib,stella_fallback.lib'
cd ..
dotnet publish .\CloudFix.csproj -c Release
```

The published executable is written to:

```text
bin\Release\net9.0\win-x64\publish\STFixer.exe
```
