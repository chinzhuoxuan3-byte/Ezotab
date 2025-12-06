local a=game:GetService("Players")
local b=game:GetService("UserInputService")
local c=game:GetService("TweenService")
local d=game:GetService("RunService")
local e=a.LocalPlayer
local f=e:WaitForChild("PlayerGui")
local g=game:GetService("ReplicatedStorage").Events.GeneralEvent
local h=false
local i="Draco"
local j={}

local k=Instance.new("ScreenGui")
k.ResetOnSpawn=false
k.Parent=f

local l=Instance.new("TextButton")
l.Size=UDim2.new(0,95,0,95)
l.Position=UDim2.new(0,20,0.5,-47)
l.BackgroundColor3=Color3.fromRGB(255,50,50)
l.Text="关"
l.TextScaled=true
l.Font=Enum.Font.GothamBold
l.Parent=k
Instance.new("UICorner",l).CornerRadius=UDim.new(1,0)
local m=Instance.new("UIStroke",l)
m.Color=Color3.new(1,1,1)
m.Thickness=4

task.spawn(function()
while l.Parent do
c:Create(m,TweenInfo.new(1,Enum.EasingStyle.Sine,Enum.EasingDirection.InOut,-1,true),{Transparency=0.4}):Play()
task.wait(1)
end
end)

local n=false
l.InputBegan:Connect(function(o)if o.UserInputType==Enum.UserInputType.Touch or o.UserInputType==Enum.UserInputType.MouseButton1 then n=true end end)
l.InputEnded:Connect(function()n=false end)
b.InputChanged:Connect(function(o)if n then l.Position=UDim2.new(0,o.Position.X-47,0,o.Position.Y-47)end end)

l.Activated:Connect(function()
h=not h
l.Text=h and"开"or"关"
l.BackgroundColor3=h and Color3.fromRGB(0,255,0)or Color3.fromRGB(255,50,50)
end)

local p=0
l.MouseButton1Down:Connect(function()
if tick()-p<0.4 then k.Enabled=not k.Enabled end
p=tick()
end)

local function q()
local r=e.Character
if not r or not r:FindFirstChild("HumanoidRootPart")then return nil end
local s=r.HumanoidRootPart.Position
local t,u=nil,math.huge
for _,v in a:GetPlayers()do
if v~=e and v.Character and v.Character:FindFirstChild("Head")and v.Character:FindFirstChild("Humanoid")then
local w=v.Character.Humanoid
if w.Health>1 then
local x=(s-v.Character.HumanoidRootPart.Position).Magnitude
if x<u and x<=5000 then
t=v
u=x
end
end
end
end
return t
end

local function y(z,A)
local B=Drawing.new("Line")
B.Color=Color3.fromRGB(255,0,0)
B.Thickness=2
B.Transparency=1
B.From=workspace.CurrentCamera:WorldToScreenPoint(z)
B.To=workspace.CurrentCamera:WorldToScreenPoint(A)
table.insert(j,B)
c:Create(B,TweenInfo.new(0.3),{Transparency=0}):Play()
task.wait(0.2)
c:Create(B,TweenInfo.new(0.3),{Transparency=1}):Play()
task.wait(0.3)
B:Remove()
end

local function C(D)
pcall(function()
local r=e.Character
if r and r:FindFirstChild("HumanoidRootPart")then
local E={[1]="FireGun",[2]={["Position1"]=r.HumanoidRootPart.Position,["GunName"]=i,["Hit1"]=D}}
g:FireServer(unpack(E))
y(r.HumanoidRootPart.Position+Vector3.new(0,2,0),D.Position)
end
end)
end

d.Heartbeat:Connect(function()
if not h then return end
local F=q()
if F and F.Character and F.Character:FindFirstChild("Head")then
C(F.Character.Head)
task.wait(0.05)
end
end)
