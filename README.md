--Usage
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Skinny-yz/Librarys/refs/heads/main/I%20do%20not%20use%20it%20very%20much/EclipseUI.lol"))()

local Window = Library:Start({
  ["Name"] = "SIUUU HUB",
  ["SaveFolder"] = "SIUUU"
})

local Tab = Window:MakeTab("COISAS TOP")

local sectionTab = Tab:Section({
  Title = "VOLUCIDADE",
  Content = "CLIQUE"
})

local selectedSpeed = 1 -- Valor padrão

sectionTab:Slider({
Title = "Velocidade",  -- título
Content = "Ajuste sua velocidade",  -- descrição
Min = 1,  -- valor mínimo
Max = 10,  -- valor máximo
Increment = 1,  -- incremento
Default = 1, 
Callback = function(Value)
    selectedSpeed = Value
    print("Velocidade ajustada para: ", Value)
end})

sectionTab:Button({
Title = "Aplicar Velocidade", -- título
Content = "Clique para aplicar a velocidade", -- descrição
Callback = function()
    local player = game.Players.LocalPlayer
    if player and player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = selectedSpeed * 10 -- Multiplicando por 10 para ajustar a escala
        print("Velocidade aplicada: ", selectedSpeed * 10)
    else
        print("Jogador ou Humanoide não encontrado.")
    end
end})

-- Função de voo
local flying = false
local flySpeed = 1
local flyConnection = nil

local function startFlying(speed)
    local player = game.Players.LocalPlayer
    local character = player.Character
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    local rootPart = character:FindFirstChild("HumanoidRootPart")

    if humanoid and rootPart then
        flying = true
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
        bodyVelocity.Parent = rootPart

        flyConnection = game:GetService("RunService").Stepped:Connect(function()
            if flying then
                local camera = workspace.CurrentCamera
                local moveDirection = camera.CFrame.LookVector
                bodyVelocity.Velocity = moveDirection * speed * 10 -- Multiplicando por 10 para ajustar a escala
            else
                bodyVelocity:Destroy()
                flyConnection:Disconnect()
            end
        end)
    end
end

local function stopFlying()
    flying = false
end

sectionTab:Slider({
Title = "Velocidade de Voo",  -- título
Content = "Ajuste sua velocidade de voo",  -- descrição
Min = 1,  -- valor mínimo
Max = 10,  -- valor máximo
Increment = 1,  -- incremento
Default = 1, 
Callback = function(Value)
    flySpeed = Value
    print("Velocidade de voo ajustada para: ", Value)
end})

sectionTab:Button({
Title = "Aplicar Voo", -- título
Content = "Clique para começar a voar", -- descrição
Callback = function()
    startFlying(flySpeed)
    print("Voo iniciado com velocidade: ", flySpeed * 10)
end})

sectionTab:Button({
Title = "Parar Voo", -- título
Content = "Clique para parar de voar", -- descrição
Callback = function()
    stopFlying()
    print("Voo parado")
end})

local Tab1 = Window:MakeTab("auto farm")

local teleporting = false
local teleportConnection = nil

local function startTeleporting()
    local player = game.Players.LocalPlayer
    if player then
        teleporting = true
        teleportConnection = game:GetService("RunService").Stepped:Connect(function()
            if teleporting then
                player.Character.HumanoidRootPart.CFrame = CFrame.new(-744, -1, -535)
                wait(1)
                player.Character.HumanoidRootPart.CFrame = CFrame.new(-744, -3, 721)
                wait(2)
            else
                teleportConnection:Disconnect()
            end
        end)
    end
end

local function stopTeleporting()
    teleporting = false
end

local autoFarmSection = Tab1:Section({
  Title = "Auto Farm",
  Content = ""
})

autoFarmSection:Toggle({
Title = "Ligar/Desligar Auto Farm",
Content = "Clique para ligar ou desligar o auto farm",
Default = false,
Callback = function(Value)
    if Value then
        startTeleporting()
        print("Auto Farm ligado")
    else
        stopTeleporting()
        print("Auto Farm desligado")
    end
end})

-- Nova aba "tools/items"
local Tab2 = Window:MakeTab("tools/items")

local toolsSection = Tab2:Section({
  Title = "Ferramentas",
  Content = ""
})

toolsSection:Button({
Title = "Pegar TPTOOLS",
Content = "Clique para pegar TPTOOLS",
Callback = function()
    local tool = Instance.new("Tool")
    tool.Name = "TPTOOLS"
    tool.RequiresHandle = false

    tool.Activated:Connect(function()
        local mouse = game.Players.LocalPlayer:GetMouse()
        local player = game.Players.LocalPlayer
        local character = player.Character

        mouse.Button1Down:Connect(function()
            if character and character:FindFirstChild("HumanoidRootPart") then
                character.HumanoidRootPart.CFrame = CFrame.new(mouse.Hit.p)
            end
        end)
    end)

    tool.Parent = game.Players.LocalPlayer.Backpack
    print("TPTOOLS adicionado ao inventário")
end})

toolsSection:Button({
Title = "Pegar FLYTOOLS",
Content = "Clique para pegar FLYTOOLS",
Callback = function()
    local tool = Instance.new("Tool")
    tool.Name = "FLYTOOLS"
    tool.RequiresHandle = false

    tool.Activated:Connect(function()
        local player = game.Players.LocalPlayer
        local character = player.Character
        local humanoid = character:FindFirstChild("Humanoid")

        if humanoid then
            humanoid.PlatformStand = true
            local bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.Velocity = Vector3.new(0, 0, 0)
            bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
            bodyVelocity.Parent = character.HumanoidRootPart

            flyConnection = game:GetService("RunService").Stepped:Connect(function()
                local camera = workspace.CurrentCamera
                local moveDirection = camera.CFrame.LookVector
                bodyVelocity.Velocity = moveDirection * 50
            end)
        end
    end)

    tool.Deactivated:Connect(function()
        if flyConnection then
            flyConnection:Disconnect()
        end
        local player = game.Players.LocalPlayer
        local character = player.Character
        local humanoid = character:FindFirstChild("Humanoid")
        if humanoid then
            humanoid.PlatformStand = false
        end
    end)

    tool.Parent = game.Players.LocalPlayer.Backpack
    print("FLYTOOLS adicionado ao inventário")
end})

Library:Notify({
  Title = "SIUU HUB:",        
  Content = "Notificação",    
  Description = "SIUUU HUB AGRADECE", 
  Color = Color3.fromRGB(255, 0, 0),   
  Time = 0.5,              
  Delay = 5                  
})
