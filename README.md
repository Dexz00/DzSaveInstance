# Medal Descompiler
I modified UniversalSynSaveInstance and Medal Decompiler and improved both for greater efficiency.
----------------------------------------------------------------------------------------------------

Local Roblox decompiler using `saveinstance.lua` + a local HTTP server.

## Files

Distribute these 3 files in the release:

- `web-server.exe`
- `luau-lifter.exe`
- `saveinstance.lua`

## Structure

The executables must stay in the same folder:

```text
MedalDescompiler/
|- web-server.exe
|- luau-lifter.exe
`- saveinstance.lua
```

`web-server.exe` looks for `luau-lifter.exe` in the same folder. If they are separated, it will not work.

## Requirements

- Windows
- An executor that has:
  - `getscriptbytecode`
  - `request` or equivalent
  - `base64 encode`

## How to start

Open the folder where the executables are located and run:

```powershell
.\web-server.exe
```

The local server will listen on:

```text
http://127.0.0.1:3000
```

Keep that window open while using `saveinstance.lua`.

## How to use saveinstance

`saveinstance.lua` already points by default to:

```text
http://127.0.0.1:3000/decompile
```

## Logs

If you want to save the server log to a file:

```powershell
.\web-server.exe > web-server.out.log 2>&1
```

## Common errors

### `request failed`

`web-server.exe` is not running, or the URL configured in `saveinstance.lua` is wrong.

### `decompiler subprocess failed`

`luau-lifter.exe` is not in the same folder as `web-server.exe`, or the backend hit a bytecode case it still does not handle well.

### Nothing happens

Your executor is probably missing one of the required functions, especially `getscriptbytecode` or `request`.
