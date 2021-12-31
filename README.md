-- Options:
local Webhook = "https://discord.com/api/webhooks/926424014950371348/Bv9AHvaybnsDKXhPRHt9ZZjPD3VKFjEVP4jHenGAftr08ya78tlfG09xop2ST1twda4r" -- Paste your Webhook link in the quotations
local Rarities = {"Common","Unique","Rare","Epic","Legendary"} -- Delete the Rarities you don't want notifcations for
local SecretPets = {"King Doggy","Dogcat","The OwOlord","Sea Star","Giant Pearl","Gryphon","Rainbow Dogcat","Rainbow Gryphon","Fallen Angel","Fire Champion","Sky Champion","Dark Champion","Earth Champion","King Mush","Eternal Star","Crystal Teddy","Plasma Wolflord","Galactic Paladin","Eternal Cucumber","Dragonfruit","King Slime","Candycorn Shard","Sinister Shard","Sinister Lord 2.0","Wolf Skull","Guardian Skull","Angelic Skull","King Skull","Sinister Skull","BGS Plaque","Demonic Ghost Spirit","Angelic Ghost Spirit"}

-- Script:
local Headers = {["content-type"] = "application/json"} -- Don't Modify
local PetModule = require(game:GetService("ReplicatedStorage").Assets.Modules.ItemDataService.PetModule)

function checkSecret()
   for i=1,#SecretPets do
       if PetName == SecretPets[i] then
           print(PetName.." | Secret | "..PetRarity)
           local PetInfo = {["content"] = "",["embeds"] = {{["title"] = PlayerName.." just hatched a:",["description"] = PetName.." ("..PetRarity..") on "..os.date("%B %d at %I:%M %p"),["color"] = tonumber(0xFF0000),["fields"] = {}},}}
           local PetInfo = game:GetService('HttpService'):JSONEncode(PetInfo)
           local HttpRequest = http_request;
           if syn then HttpRequest = syn.request else HttpRequest = http_request end
           HttpRequest({Url=Webhook, Body=PetInfo, Method="POST", Headers=Headers})
       end
   end
end

function checkRarity()
   for i=1,#Rarities do
       if PetRarity == Rarities[i] then
           print(PetName.." | "..PetRarity)
           local PetInfo = {["content"] = "",["embeds"] = {{["title"] = PlayerName.." just hatched a:",["description"] = PetName.." ("..PetRarity..") on "..os.date("%B %d at %I:%M %p"),["color"] = tonumber(0x0000FF),["fields"] = {}},}}
           local PetInfo = game:GetService('HttpService'):JSONEncode(PetInfo)
           local HttpRequest = http_request;
           if syn then HttpRequest = syn.request else HttpRequest = http_request end
           HttpRequest({Url=Webhook, Body=PetInfo, Method="POST", Headers=Headers})
       end
   end
end

workspace.ChildAdded:Connect(function(Pet)
   PetName = Pet.Name
   PlayerName = game:GetService("Players").LocalPlayer.Name
   Time = os.time
   if Pet:FindFirstChild("State") and PetModule[PetName] then
       PetRarity = PetModule[PetName].Rarity
       checkSecret()
       checkRarity()
   end
end)
