# Support Project
I modified UniversalSynSaveInstance and Medal Decompiler, improving both to make them more efficient.

Currently I don’t use a server because I can’t afford to maintain one. If you’d like to support the project, you can donate here:
https://ko-fi.com/dexz0

Crypto donations are also welcome:

Litecoin (LTC) – Native Network
Address: ltc1qls0c3742jwedk2qkhe8nf3fddpp6aa7ytam3k2

Any support helps keep the project going. Thank you! 🚀
----------------------------------------------------------------------------------------------------

# Medal Descompiler

Local Roblox decompiler package built around `saveinstance.lua` and a local HTTP backend.

This release is meant to be used as-is. No source code is required.

## Credits

This package is a modified distribution built on top of these projects:

- `saveinstance.lua` is based on [luau/UniversalSynSaveInstance](https://github.com/luau/UniversalSynSaveInstance)
- the decompiler backend is based on [shrimp-nz/medal](https://github.com/shrimp-nz/medal)

I do not claim authorship of the original projects. This package only adapts and redistributes a modified build for local use.

USSI credit string required by the original project license:

```text
UniversalSynSaveInstance https://discord.gg/wx4ThpAsmw
```

## Included files

- `web-server.exe`
- `luau-lifter.exe`
- `saveinstance.lua`

## Folder layout

Keep all 3 files in the same folder:

```text
MedalDescompiler/
|- web-server.exe
|- luau-lifter.exe
`- saveinstance.lua
```

`web-server.exe` launches `luau-lifter.exe` as a subprocess. If they are not in the same folder, decompilation will fail.

## Requirements

- Windows
- A Roblox executor with:
  - `getscriptbytecode`
  - `request` or equivalent
  - base64 encoding support

## Quick start

1. Open the folder containing the files.
2. Start the local server:

```powershell
.\web-server.exe
```

3. Keep that window open.
4. In your executor, run:

```lua
loadstring(readfile("saveinstance.lua"))()
```

Or, if you prefer loading it from a URL:

```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/Dexz00/DzSaveInstance/refs/heads/main/DZSave.lua"))()
```

Default backend URL:

```text
http://127.0.0.1:3000/decompile
```

## Configuration

### Auto-run with custom options

If you want to keep the direct auto-run flow but change save settings:

```lua
getgenv().USSI_OPTIONS = {
    mode = "scripts",
    ShowStatus = true,
    scriptcache = true,
    SaveBytecode = false,
    NilInstances = false,
    timeout = 120,
}

loadstring(readfile("saveinstance.lua"))()
```

You can do the same with `game:HttpGet(...)` if you prefer:

```lua
getgenv().USSI_OPTIONS = {
    mode = "scripts",
    ShowStatus = true,
    scriptcache = true,
    SaveBytecode = false,
    NilInstances = false,
    timeout = 120,
}

loadstring(game:HttpGet("https://raw.githubusercontent.com/Dexz00/DzSaveInstance/refs/heads/main/DZSave.lua"))()
```

### Manual control

If you want to load the file without auto-running it:

```lua
getgenv().USSI_DIRECT_SOURCE_MODE = false

local saveinstance = loadstring(readfile("saveinstance.lua"))()

saveinstance({
    mode = "full",
    ShowStatus = true,
    SafeMode = false,
    KillAllScripts = false,
    ShutdownWhenDone = false,
    scriptcache = true,
    SaveBytecode = false,
    NilInstances = false,
    timeout = 120,
})
```

Or with `game:HttpGet(...)`:

```lua
getgenv().USSI_DIRECT_SOURCE_MODE = false

local saveinstance = loadstring(game:HttpGet("https://raw.githubusercontent.com/Dexz00/DzSaveInstance/refs/heads/main/DZSave.lua"))()

saveinstance({
    mode = "full",
    ShowStatus = true,
    SafeMode = false,
    KillAllScripts = false,
    ShutdownWhenDone = false,
    scriptcache = true,
    SaveBytecode = false,
    NilInstances = false,
    timeout = 120,
})
```

## Common options

- `mode = "scripts"`: save scripts only
- `mode = "full"`: save the full place
- `mode = "optimized"`: optimized place save mode
- `ShowStatus = true`: show progress/status UI
- `scriptcache = true`: cache decompiled script results
- `SaveBytecode = true`: include bytecode in the output
- `NilInstances = true`: include nil-parented instances
- `timeout = 120`: decompiler timeout in seconds
- `timeout = -1`: disable timeout
- `SafeMode = true`: safer mode, usually kicks before saving
- `KillAllScripts = true`: stop scripts during save
- `ShutdownWhenDone = true`: close the game when finished

## Backend globals

You can change Medal backend behavior without editing the file:

```lua
getgenv().MedalDecompilerUrl = "http://127.0.0.1:3000/decompile"
getgenv().MedalDecompilerTimeout = -1
```

## Logs

To save server output to a log file:

```powershell
.\web-server.exe > web-server.out.log 2>&1
```

## Common errors

### `request failed`

The local server is not running, or the URL is wrong.

### `decompiler subprocess failed`

`luau-lifter.exe` is missing, in the wrong folder, or the backend hit a bytecode case it still does not fully handle.

### Nothing happens

Your executor is likely missing one of the required functions, especially `getscriptbytecode` or `request`.
