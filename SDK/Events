local BindedEvents = {}
local CustomEvents = {}

function BindEvent(BindName,Object,Event,Callback)
    if BindedEvents[Object.Name] and BindedEvents[Object.Name][Event] then
        table.insert( BindedEvents[Object.Name][Event], {BindName,Callback} )
    else
        BindedEvents[Object.Name] = {}
        BindedEvents[Object.Name][Event] = {{BindName,Callback}}
    end
    BindedEvents[Object.Name][Event] = {{BindName,Callback}}
    Object[Event]:Connect(function(...)
        local args = {...}
        table.foreach(BindedEvents[Object.Name][Event],function(_,tbl)
            tbl[2](unpack(args))
        end)
    end)
end

function UnbindEvent(BindName,Object,Event)
    if BindedEvents[Object.Name] and BindedEvents[Object.Name][Event] then
        table.foreach(BindedEvents[Object.Name][Event],function(i,bind)
            if bind[1] == BindName then
                table.remove( BindedEvents[Object.Name][Event] , i )
            end
        end)
    else
        return warn("Failed to find binded event!")
    end
end

function RemoveEventBind(Object,Event)
    if BindedEvents[Object.Name] and BindedEvents[Object.Name][Event] then
        BindedEvents[Object.Name][Event] = nil
    end
end

function CreateCustomEvent(EventCat,EventName)
    if not CustomEvents[EventCat] then
        CustomEvents[EventCat] = {}
    end
    CustomEvents[EventCat][EventName] = {Bind=function(BindName,Callback)
        table.insert( CustomEvents[EventCat][EventName].Binds, {BindName,Callback})
    end,
    Unbind=function(BindName)
        table.foreach(CustomEvents[EventCat][EventName].Binds,function(i,bind)
            if bind[1] == BindName then
                table.remove( CustomEvents[EventCat][EventName].Binds , i )
            end
        end)
    end,Binds={}}
end

function RemoveCustomEvent(EventCat,EventName)
    if CustomEvents[EventCat] and CustomEvents[EventCat][EventName] then
        CustomEvents[EventCat][EventName] = nil
    else
        return warn("Failed to find custom event!")
    end
end

function GetCustomBinds(EventCat,EventName)
    if CustomEvents[EventCat] and CustomEvents[EventCat][EventName] then
        return CustomEvents[EventCat][EventName].Binds
    else
        return warn("Failed to find custom event!")
    end
end

getgenv().BindedEvents = BindedEvents
getgenv().CustomEvents = CustomEvents

getgenv().BindEvent = BindEvent
getgenv().UnbindEvent = UnbindEvent
getgenv().RemoveEventBind = RemoveEventBind
getgenv().RemoveCustomEvent = RemoveCustomEvent
getgenv().GetCustomBinds = GetCustomBinds
