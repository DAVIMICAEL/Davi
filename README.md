-- ESP Box Script (Script Hub Ready)
pcall(function()
    local Players = game:GetService("Players")

    local function criarCaixaESP(character)
        local rootPart = character:WaitForChild("HumanoidRootPart", 5)
        if not rootPart then return end

        if rootPart:FindFirstChild("ESPBox") then return end

        local box = Instance.new("BoxHandleAdornment")
        box.Name = "ESPBox"
        box.Adornee = rootPart
        box.AlwaysOnTop = true
        box.ZIndex = 10
        box.Size = Vector3.new(4, 6, 2)
        box.Color3 = Color3.new(1, 0, 0)
        box.Transparency = 0.5
        box.Parent = rootPart
    end

    local function limparCaixaESP(character)
        local rootPart = character:FindFirstChild("HumanoidRootPart")
        if rootPart then
            local box = rootPart:FindFirstChild("ESPBox")
            if box then
                box:Destroy()
            end
        end
    end

    local function onCharacterAdded(character)
        criarCaixaESP(character)

        local humanoid = character:WaitForChild("Humanoid", 5)
        if humanoid then
            humanoid.Died:Connect(function()
                limparCaixaESP(character)
            end)
        end
    end

    for _, player in pairs(Players:GetPlayers()) do
        if player.Character then
            onCharacterAdded(player.Character)
        end
        player.CharacterAdded:Connect(onCharacterAdded)
    end

    Players.PlayerAdded:Connect(function(player)
        player.CharacterAdded:Connect(onCharacterAdded)
    end)
end)
