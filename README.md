# LibTrinityCore-1.0

LibTrinityCore-1.0 is a library for the World of Warcraft client that integrates with the [TrinityCore](https://github.com/TrinityCore/TrinityCore/) server software.
It provides a Lua interface that allows addon code to interact with server commands in an easy fashion.

## Supported branches
Branch | as of commit | Notes
------ | ------------ | -----
 `master` | `n/a` | Not yet supported
 `3.3.5` | [`508c9d2`](https://github.com/TrinityCore/TrinityCore/commit/508c9d2fc1b20dc2cb40df533e823e1dfe2becc3) | 

## API
### LibTrinityCore::IsTrinityCore
```lua
isTrinityCore = LibStub("LibTrinityCore-1.0"):IsTrinityCore()
```
Returns `true` if the server is a supported TrinityCore build, `false` if not, and `nil` if testing is incomplete

### LibTrinityCore::RegisterLoader
```lua
LibStub("LibTrinityCore-1.0"):RegisterLoader(callback)
```
Registers the passed `callback` to be called once TrinityCore detection finishes. This method might call your loader before returning, or asynchronously at any later point. Once the loader is called, `IsTrinityCore()` is guaranteed to return either `true` or `false` (never `nil`).

### LibTrinityCore::DoCommand
```lua
LibStub("LibTrinityCore-1.0"):DoCommand(cmd, callback)
```
Invokes `cmd` on the server. Commands should be passed without leading command prefix (no `.` or `!`).
When command execution finishes, `callback` will be invoked as `callback(success, output)`. `success` is a boolean indicating whether the command's execution reported any errors, while `output` is an array of lines that the command printed.

## Example usage

```lua
-- get library object - all calls are made through this object
local LibTrinity = LibStub("LibTrinityCore-1.0")
assert(LibTrinity)

-- define a command callback function
local function commandCallback(success, output)
  print("Command status: ", success and "OK" or "Fail")
  if #output > 0 then
    print("Command output:")
    for _,line in ipairs(output) do
      print(line)
    end
  else
    print("Command produced no output")
  end
end
  
-- then invoke the "server motd" and "server info" commands
LibTrinity:DoCommand("server motd", commandCallback)
LibTrinity:DoCommand("server info", commandCallback)
```
