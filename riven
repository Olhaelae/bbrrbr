-- champ name detector
if GetObjectName(GetMyHero())~= "Riven" then return end

-- version
local ver = "0.52"

function AutoUpdate(data)
    if tonumber(data) > tonumber(ver) then
        PrintChat("New version found! " .. data)
        PrintChat("Downloading update, please wait...")
        DownloadFileAsync("https://raw.githubusercontent.com/DRoar/Scripts/master/RoarRiven.lua", SCRIPT_PATH ..

"RoarRiven.lua", function() PrintChat("Update Complete, please 2x F6!") return end)
    else
        PrintChat(string.format("<font color='#b756c5'>Roar Riven </font>").."updated ! Version: "..ver)
    end
end

GetWebResultAsync("https://raw.githubusercontent.com/DRoar/Scripts/master/RivenRoar.version", AutoUpdate)

-- lib

require('Inspired')
require('MapPositionGOS')

-- Combo Menu
local RivenMenu = Menu("Riven", "Riven")
RivenMenu:SubMenu("Combo", "Combo")
RivenMenu.Combo:Boolean("Q", "Use Q", true)
RivenMenu.Combo:Boolean("W", "Use W", true)
RivenMenu.Combo:Boolean("E", "Use E", true)
RivenMenu.Combo:Boolean("R", "Use R", true)
RivenMenu.Combo:Slider("RS", "Start R If Enemies Around", 1, 1, 5, 1)
RivenMenu.Combo:Boolean("H", "Use Hydra", true)
RivenMenu.Combo:Boolean("YGB", "Use Ghost Blade", true)

BlockF7OrbWalk(true)

RivenMenu:Menu("Harass", "Harass")
RivenMenu.Harass:Boolean("Q", "Use Q", true)
RivenMenu.Harass:Boolean("W", "Use W", true)
RivenMenu.Harass:Boolean("E", "Use E", true)
RivenMenu.Harass:Boolean("H", "Use Hydra", true)

-- LaneClear Menu
RivenMenu:SubMenu("LaneClear", "LaneClear")
RivenMenu.LaneClear:Boolean("LCQ", "Use Q", true)
RivenMenu.LaneClear:Boolean("LCW", "Use W", true)
RivenMenu.LaneClear:Boolean("LCH", "Use Tiamat/Hydra", true)

-- Jungle Clear
RivenMenu:SubMenu("JungleClear", "JungleClear")
RivenMenu.JungleClear:Boolean("JCQ", "Use Q", true)
RivenMenu.JungleClear:Boolean("JCW", "Use W", true)
RivenMenu.JungleClear:Boolean("JCE", "Use E", true)
RivenMenu.JungleClear:Boolean("JCH", "Use Tiamat/Hydra", true)

RivenMenu:Menu("Killsteal", "Killsteal")
RivenMenu.Killsteal:Boolean("W", "Killsteal with W", true)
RivenMenu.Killsteal:Boolean("R", "Killsteal with R", true)

--Misc
RivenMenu:Menu("Misc", "Misc")
if Ignite ~= nil then RivenMenu.Misc:Boolean("Autoignite", "Auto Ignite", true) end
RivenMenu.Misc:KeyBinding("Flee", "Flee", string.byte("T"))
RivenMenu.Misc:KeyBinding("WallJump", "WallJump", string.byte("G"))
RivenMenu.Misc:Boolean("AutoW", "Auto W", true)
RivenMenu.Misc:Slider("AutoWCount", "if Enemies Around >", 3, 1, 5, 1)
RivenMenu:Menu("Interrupt", "Interrupt (W)")

DelayAction(function()
  local str = {[_Q] = "Q", [_W] = "W", [_E] = "E", [_R] = "R"}
  for i, spell in pairs(CHANELLING_SPELLS) do
    for _,k in pairs(GetEnemyHeroes()) do
      if spell["Name"] == GetObjectName(k) then
      RivenMenu.Interrupt:Boolean(GetObjectName(k).."Inter", "On "..GetObjectName(k).." "..(type(spell.Spellslot) ==

'number' and str[spell.Spellslot]), true)
      end
    end
  end
end, 0.001)

RivenMenu:Menu("Drawings", "Drawings")
RivenMenu.Drawings:Boolean("Q", "Draw Q Range", false)
RivenMenu.Drawings:Boolean("W", "Draw W Range", false)
RivenMenu.Drawings:Boolean("E", "Draw E Range", false)
RivenMenu.Drawings:Boolean("R", "Draw R Range", true)
RivenMenu.Drawings:Boolean("EQ", "Draw EQ Range", true)

OnDraw(function(myHero)
local pos = GetOrigin(myHero)
if RivenMenu.Drawings.Q:Value() then DrawCircle(pos,275,1,25,GoS.Pink) end
if RivenMenu.Drawings.W:Value() then DrawCircle(pos,260,1,25,GoS.Yellow) end
if RivenMenu.Drawings.E:Value() then DrawCircle(pos,250,1,50,GoS.Blue) end
if RivenMenu.Drawings.R:Value() then DrawCircle(pos,1100,1,0,GoS.Green) end
if RivenMenu.Drawings.EQ:Value() then DrawCircle(pos,525,1,0,GoS.Red) end
end)

local QCast = 0
local lastE = 0


--Mode
function Mode() --Deftsu
    if IOW_Loaded then
        return IOW:Mode()
    elseif DAC_Loaded then
        return DAC:Mode()
    elseif PW_Loaded then
        return PW:Mode()
    elseif GoSWalkLoaded and GoSWalk.CurrentMode then
        return ({"Combo", "Harass", "LaneClear", "LastHit"})[GoSWalk.CurrentMode+1]
    elseif AutoCarry_Loaded then
        return DACR:Mode()
    elseif _G.SLW_Loaded then
        return SLW:Mode()
    elseif EOW_Loaded then
        return EOW:Mode()
    end
    return ""
end

OnTick(function ()
    local target=GetCurrentTarget()
    mousePos=GetMousePos()
	Tiamat = GetItemSlot(myHero, 3077)
    Hydra = GetItemSlot(myHero,3074)
    YGB = GetItemSlot(myHero,3142)
    TO=GetOrigin(target)


	if Mode()== "Combo" then
        if RivenMenu.Combo.R:Value() and IsReady(_R) and GetCastName(myHero, _R) ~= "rivenizunablade" and EnemiesAround

(myHero, 325)>=RivenMenu.Combo.RS:Value() then
            CastSpell(_R)
        end

        if RivenMenu.Combo.YGB:Value() and YGB> 0 and Ready(YGB) and ValidTarget(target,1500)then
        CastSpell(YGB)
        end


		if IsReady(_Q) and IsReady(_E) and IsReady(_R) and RivenMenu.Combo.Q:Value() and RivenMenu.Combo.E:Value

()and RivenMenu.Combo.Q:Value() and ValidTarget(target, 715) then
	 	 	CastSkillShot(_E, TO) DelayAction(function () CastSkillShot(_R,TO) CastSkillShot(_Q,TO)end, 0.1 )

	 	elseif IsReady(_Q) and IsReady(_E) and RivenMenu.Combo.Q:Value() and RivenMenu.Combo.E:Value() and

ValidTarget(target, 715) and GetDistance(target) > GetRange(myHero)+GetHitBox(target) then
	 	 	CastSkillShot(_E, TO)
        DelayAction(function() CastSkillShot(_Q, TO) end, 0.267)

        elseif IsReady(_E) and RivenMenu.Combo.E:Value() and ValidTarget(target, 440) and GetDistance(target) > GetRange

(myHero)+GetHitBox(myHero) then
	 		CastSkillShot(_E, TO)
	  	end
	end

	if Mode() == "Harass" then

	  	if IsReady(_E) and RivenMenu.Harass.E:Value() and ValidTarget(target, 440) and GetDistance(target) >

GetRange(myHero)+GetHitBox(myHero) then
	  	CastSkillShot(_E, TO)
	  	end

	  	if IsReady(_Q) and IsReady(_E) and RivenMenu.Harass.Q:Value() and RivenMenu.Harass.E:Value() and

ValidTarget(target, 715) and GetDistance(target) > GetRange(myHero)+GetHitBox(myHero)*3 then
	 	 CastSkillShot(_E, TO)
	  	DelayAction(function() CastSkillShot(_Q, TO) end, 0.267)
	  	end
	end

	if IsReady(_W) and RivenMenu.Misc.AutoW:Value() and EnemiesAround(GetOrigin(myHero),260,267) >=

RivenMenu.Misc.AutoWCount:Value() then
		CastSpell(_W)
	end

	for i,enemy in pairs(GetEnemyHeroes()) do

	  if Ignite and RivenMenu.Misc.Autoignite:Value() then
            if IsReady(Ignite) and 25*GetLevel(myHero)+55 > GetHP(enemy)+GetHPRegen(enemy)*3 and ValidTarget(enemy, 600)

then
            CastTargetSpell(enemy, Ignite)
            end
          end

      local DMGREQ= GetCurrentHP(enemy) - (GetArmorPenFlat(myHero)* (0.6 + (GetLevel(myHero)*0.4)/18) - GetArmor

(enemy)/GetArmorPenPercent(enemy))
      local WDMG= (GetCastLevel(myHero, _W)*30+25)*GetBonusDmg(myHero)
      local MissingHealthP=(((GetMaxHP(enemy)-GetCurrentHP(enemy))/GetMaxHP(enemy)))*2.666
      if MissingHealthP>2 then
	 			MissingHealthP=2
	  end
      local RDMG=	MissingHealthP*((GetCastLevel(myHero, _R)*50+100)+GetBonusDmg(myHero)*0.6)

  	  if IsReady(_W) and ValidTarget(enemy, 260) and RivenMenu.Killsteal.W:Value() and DMGREQ < WDMG then
  	  CastSpell(_W)

	  elseif IsReady(_R) and GetCastName(myHero, _R) ~= "RivenFengShuiEngine" and ValidTarget(enemy, 1100) and

RivenMenu.Killsteal.R:Value() and DMGREQ<RDMG then
        CastSkillShot(_R,enemy.pos)
      end

	 if RivenMenu.Misc.Flee:Value() then
          MoveToXYZ(mousePos)
          if IsReady(_E) then
          CastSkillShot(_E, mousePos)
          end
          if not IsReady(_E) and IsReady(_Q) and lastE + 350 < GetTickCount() then
          CastSkillShot(_Q, mousePos)
          end
     end

	if RivenMenu.Misc.WallJump:Value() then
          local movePos1  = GetOrigin(myHero) + (Vector(mousePos) - GetOrigin(myHero)):normalized() * 75
          local movePos2 =  GetOrigin(myHero) + (Vector(mousePos) - GetOrigin(myHero)):normalized() * 450
          if QCast < 2 and CanUseSpell(myHero, _Q) ~= ONCOOLDOWN then
          CastSkillShot(_Q, mousePos)
          end
          if not MapPosition:inWall(movePos1) then
            MoveToXYZ(mousePos)
          else
            if not MapPosition:inWall(movePos2) and CanUseSpell(myHero, _Q) ~= ONCOOLDOWN then
            CastSkillShot(_Q, movePos2)
            end
          end
	end

        end


end)

OnProcessSpell(function(unit,spell)
-- W cancel event
  if GetObjectType(unit) == Obj_AI_Hero and GetTeam(unit) ~= GetTeam(myHero) and IsReady(_W) then
    if CHANELLING_SPELLS[spell.name] then
      if ValidTarget(unit, 240) and GetObjectName(unit) == CHANELLING_SPELLS[spell.name].Name and RivenMenu.Interrupt

[GetObjectName(unit).."Inter"]:Value() then
      CastSpell(_W)
      end
    end
  end

  if unit == myHero then
    if spell.name == "RivenFeint" then  --E
    lastE = GetTickCount()
    end

    local target = GetCurrentTarget()
--------------------------------------------------------AACanceling
    if spell.name:lower():find("attack") then -- after attacking using spells
       AAWind = GetWindUp(myHero)
    DelayAction(function()
-- Combo
      if Mode() == "Combo" and ValidTarget(target) then
        if IsReady(_W) and RivenMenu.Combo.W:Value() then
        CastSpell(_W)
        elseif Hydra > 0 and IsReady(Hydra) and RivenMenu.Combo.H:Value() then
        CastSpell(Hydra)
        elseif Tiamat > 0 and IsReady(Tiamat) and RivenMenu.Combo.H:Value() then
        CastSpell(Tiamat)
        elseif IsReady(_Q) and RivenMenu.Combo.Q:Value() then
        CastSkillShot(_Q, GetOrigin(target))
        end
      end
-- Harass
      if Mode() == "Harass" and ValidTarget(target) then
      	if IsReady(_W) and RivenMenu.Harass.W:Value() then
        CastSpell(_W)
        elseif Hydra > 0 and IsReady(Hydra) and RivenMenu.Harass.H:Value() then
        CastSpell(Hydra)
        elseif Tiamat > 0 and IsReady(Tiamat) and RivenMenu.Harass.H:Value() then
        CastSpell(Tiamat)
        elseif IsReady(_Q) and RivenMenu.Harass.Q:Value() then
        CastSkillShot(_Q, mousePos)
        end
      end
-- Lane Clear
      if  Mode()=="LaneClear" then
    		if RivenMenu.LaneClear.LCH:Value() and Ready(_Q) and MinionsAround(myHero,260, MINION_ENIME)>0 then
       		CastSpell(_Q)
            elseif RivenMenu.LaneClear.LCW:Value() and Ready(_W) and MinionsAround(myHero,250,MINION_ENIME)>0 then
        	CastSpell(_W)
            elseif RivenMenu.LaneClear.LCH:Value() and Tiamat>0 and MinionsAround(myHero,350,MINION_ENIME)>0 then
        	CastSpell(Tiamat)
            elseif RivenMenu.LaneClear.LCH:Value() and Hydra>0 and MinionsAround(myHero,350,MINION_ENIME)>0 then
       		CastSpell(Hydra)
-- Jungle Clear
        elseif RivenMenu.JungleClear.JCQ:Value() and Ready(_Q) and MinionsAround(myHero,260,MINION_JUNGLE)>0 then
		CastSpell(_Q)
        elseif RivenMenu.JungleClear.JCW:Value() and Ready(_W) and MinionsAround(myHero,250,MINION_JUNGLE)>0 then
       	CastSpell(_W)
        elseif RivenMenu.JungleClear.JCH:Value() and Tiamat>0 and MinionsAround(myHero,350,MINION_JUNGLE)>0 then
        CastSpell(Tiamat)
    	elseif RivenMenu.JungleClear.JCH:Value() and Hydra>0 and MinionsAround(myHero,350,MINION_JUNGLE)>0 then
        CastSpell(Hydra)
        elseif RivenMenu.JungleClear.JCE:Value() and Ready(_E) and MinionsAround(myHero,350,MINION_JUNGLE)>0 then
        CastSpell(_E)
        end
      end
    end, AAWind )

    end
--------------------------------------------------- SpellCanceling
    if spell.name == "RivenMartyr" then --W
        DelayAction(function()
      if Mode() == "Combo" and ValidTarget(target) then
        if IsReady(_Q) and RivenMenu.Combo.Q:Value() then
        CastSkillShot(_Q, GetOrigin(target))
        elseif Hydra > 0 and IsReady(Hydra) and RivenMenu.Combo.H:Value() then
        CastSpell(Hydra)
        elseif Tiamat > 0 and IsReady(Tiamat) and RivenMenu.Combo.H:Value() then
        CastSpell(Tiamat)
        end
      end

      if Mode() == "Harass" and ValidTarget(target) then
        if IsReady(_Q) and RivenMenu.Combo.Q:Value() then
        CastSkillShot(_Q, GetOrigin(target))
        elseif Hydra > 0 and IsReady(Hydra) and RivenMenu.Combo.H:Value() then
        CastSpell(Hydra)
        elseif Tiamat > 0 and IsReady(Tiamat) and RivenMenu.Combo.H:Value() then
        CastSpell(Tiamat)
        end
      end

    end, 0.1)
    end

    if spell.name == "RivenTriCleave" then --Q
    	DelayAction(function ()MoveToXYZ(mousePos)

    	DelayAction(function ()IOW:ResetAA()end, 0.1)
                 end, 0.25)
    end

   if spell.name == "ItemTiamatCleave" then
    IOW:ResetAA()
    DelayAction(function()
      if (Mode() == "Combo" and ValidTarget(target) and IsReady(_Q) and RivenMenu.Combo.Q:Value()) or(Mode() == "Harass"

and ValidTarget(target)) then

	    CastSkillShot(_Q, GetOrigin(target))
      end
     end, spell.windUpTime )
   end


    if spell.name == "RivenFengShuiEngine" then --R first Cast
    IOW:ResetAA()
    DelayAction(function()
	  if Mode() == "Combo" and ValidTarget(target) then
	    if IsReady(_Q) and RivenMenu.Combo.Q:Value() then
	    CastSkillShot(_Q, GetOrigin(target))
	    end
	  end

	  if Mode() == "Harass" and ValidTarget(target) then
	    if IsReady(_Q) and RivenMenu.Harass.Q:Value() then
	    CastSkillShot(_Q, GetOrigin(target))
	    end
	  end
    end, 0.25 )
    end

    if spell.name == "rivenizunablade" then  --R 2nd Cast
    IOW:ResetAA()
    DelayAction(function()
      if (Mode() == "Combo" and ValidTarget(target) and IsReady(_Q) and RivenMenu.Combo.Q:Value()) or (Mode() == "Harass"

and ValidTarget(target)) then
        CastSkillShot(_Q, GetOrigin(target))
      end
    end, AAWind)

    end

  end

end)

OnUpdateBuff(function(unit,buff)
  if unit == myHero and buff.Name == "RivenTriCleave" then
    QCast = buff.Count
  end
end)

OnRemoveBuff(function(unit,buff)
  if unit == myHero and buff.Name == "RivenTriCleave" then
  QCast = 0
  end
end)

AddGapcloseEvent(_W, 240, false, RivenMenu)
