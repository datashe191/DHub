-- Load Rayfield Library
local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

-- Main Window
local Window = Rayfield:CreateWindow({
    Name = "🌱 Grow A Garden | Seed Injector",
    LoadingTitle = "JuiceVortex + FRANK Interface",
    LoadingSubtitle = "No excuses. No bugs.",
    ConfigurationSaving = {
        Enabled = false
    },
    Discord = {
        Enabled = false
    },
    KeySystem = false
})

-- RARE Seeds for dropdown
local RareSeeds = {
    "Candy blossom",
    "Bone blossom",
    "Cherry blossom",
    "Moon blossom",
    "Venus Fly trap",
    "Sunflower",
    "Dragon pepper",
    "Moon mango"
}

-- Server mapping (lowercase)
local SeedMap = {
    ["candy blossom"] = "CandyBlossom",
    ["bone blossom"] = "BoneBlossom",
    ["cherry blossom"] = "CherryBlossom",
    ["moon blossom"] = "MoonBlossom",
    ["venus fly trap"] = "VenusFlytrap",
    ["sunflower"] = "Sunflower",
    ["dragon pepper"] = "DragonPepper",
    ["moon mango"] = "MoonMango"
}

-- Selection tracker
_G.SelectedSeedRaw = nil
_G.SelectedSeedID = nil

-- Create tab
local Tab = Window:CreateTab("🌾 Rare Seeds", 4483362458)

-- Dropdown
Tab:CreateDropdown({
    Name = "Select a Seed",
    Options = RareSeeds,
    CurrentOption = "",
    Callback = function(selection)
        print("[DEBUG] Dropdown clicked:", selection)
        _G.SelectedSeedRaw = selection
        local cleaned = selection:lower():gsub("^%s*(.-)%s*$", "%1")
        local mapped = SeedMap[cleaned]
        if mapped then
            _G.SelectedSeedID = mapped
            print("[DEBUG] Cleaned:", cleaned)
            print("[DEBUG] Mapped to:", mapped)
        else
            warn("[ERROR] Selection not in map:", cleaned)
            _G.SelectedSeedID = nil
        end
    end
})

-- Remote detector
local function GetSeedRemote()
    for _, obj in ipairs(getgc(true)) do
        if typeof(obj) == "Instance" and obj:IsA("RemoteEvent") and tostring(obj.Name):lower():find("seed") then
            print("[DEBUG] Remote found:", obj.Name)
            return obj
        end
    end
    return nil
end

-- Inject Button
Tab:CreateButton({
    Name = "🌿 Inject Selected Seed",
    Callback = function()
        if not _G.SelectedSeedID then
            Rayfield:Notify({
                Title = "❌ No Seed Selected",
                Content = "Dropdown never fired or mapping failed.",
                Duration = 4
            })
            warn("[ERROR] No Seed Mapped. Raw:", _G.SelectedSeedRaw)
            return
        end

        local remote = GetSeedRemote()
        if not remote then
            Rayfield:Notify({
                Title = "🚫 Remote Missing",
                Content = "Couldn't find RemoteEvent (try near a plot).",
                Duration = 4
            })
            return
        end

        remote:FireServer(_G.SelectedSeedID)

        Rayfield:Notify({
            Title = "✅ Seed Sent",
            Content = "Injected: " .. _G.SelectedSeedID,
            Duration = 4
        })

        print("[SUCCESS] Sent seed to server:", _G.SelectedSeedID)
    end
})
