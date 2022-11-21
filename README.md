# LibChatHandler-1.0 (WoW AddOn Library)

## Description
A MVC for chat event handling. Bring compatibility between chat related addons.

## Example
In the following example, a whisper is received, it is not delivered to the Default Blizzard UI, but it is delivered to all listening delegates.

```lua
local lib = LibStub("LibChatHandler-1.0");
local delegate = lib:NewDelegate();

delegate:RegisterChatEvent("CHAT_MSG_WHISPER")

-- this function gets called first, the controller can be used to block messages, delay them etc.
function delegate:CHAT_MSG_WHISPER_CONTROLLER(controller, arg1, ..., argN)
  controller:BlockFromChatFrame()
end

-- once the even has been processed (and not blocked, it will be delivered here and to other delegates)
function delegate:CHAT_MSG_WHISPER(arg1, arg2, ..., argN)
  -- do something with event
end
```

## Documentation

To use the library, you must first create a delegate. A delegate can be created or your existing object can be promoted to a delegate.
```lua
local lib = LibStub("LibChatHandler-1.0");
local delegate = lib:NewDelegate();
```
or
```lua
local MyAddon = {}
LibStub("LibChatHandler-1.0"):Embed(MyAddon);
```

### Delegate
A delegate listens for and manages whether or not an event is delivered, delayed, blocked, etc.
#### RegisterChatEvent (eventName, [priority])
Register a chat event to listen for and manage.
```lua
delegate:RegisterChatEvent("CHAT_MSG_WHISPER")
```
When an event is received it will first see if a controller is in place:
```lua
function delegate:CHAT_MSG_WHISPER_CONTROLLER(controller, arg1, ..., argN)

end
```
This method is similar to a basic event with the exception that a controller is provided. This controller can be used to control the delivery of the event. By default, an event is delivered unless a controller specifies otherwise.

Once the event clears a controller it is delivered (conditionally) to:
```lua
function delegate:CHAT_MSG_WHISPER(arg1, ..., argN)

end
```

#### UnregisterChatEvent (eventName)
Unregister / no longer listen for specified event.

### Controller