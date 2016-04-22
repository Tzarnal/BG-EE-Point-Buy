To install normally place the UI.menu for your version of the game in the override folder. 
If that fails, or you wish to combine a number of UI modifications you can manually make the following edits to UI.menu. If you don't have a UI.menu in your override folder yet follow these instructions: https://forums.beamdog.com/discussion/comment/723397/#Comment_723397

Open UI.menu in a file editor. Now find this section, somewhere around line number 12600

    menu
    {
        name 'CHARGEN_ABILITIES' 
  
Copy the following code just above it        

    --[[ POINT BUY SYSTEM VARIABLES]]
    chargen.pointBuyTable = {
        75,
        80,
        85,
        90
    }

    chargen.pointBuyIndex = 3
    chargen.pointBuyTotal = 4
    --[[ END POINT BUY SYSTEM VARIABLES]]
    
Next find this section, somewhere around line number 12800

    button
    {
        on '8'
        action "createCharScreen:OnCheatyMcCheaterson()"
        
        
    }    

Copy the following code just above it
    
    --[[ POINT BUY SYSTEM BUTTON]]
        button
        {
            area 570 636 210 46
            bam GUIOSTUM
            sequence 0
            text "Point Buy"
            text style "button"		
            action "			
                -- Get a totalRoll of the value we need
                -- I'd prefer to set it directly but it looks like the chargen values are purely 
                -- for display purposes
                while chargen.totalRoll ~= chargen.pointBuyTable[chargen.pointBuyIndex] do
                    createCharScreen:OnAbilityReRollButtonClick()
                end

                -- Reduce all Ability scores to minimum
                local oldPoints
                for i = 1,6 do
                    oldPoints = chargen.extraAbilityPoints + 1
                    while oldPoints ~= chargen.extraAbilityPoints do
                        oldPoints = chargen.extraAbilityPoints
                        createCharScreen:OnAbilityPlusMinusButtonClick(i,false)						
                    end		
                end
                
                -- Shift to the next 
                chargen.pointBuyIndex = chargen.pointBuyIndex + 1
                if(chargen.pointBuyIndex > chargen.pointBuyTotal) then
                    chargen.pointBuyIndex = 1
                end
                
            "
        }
    --[[ POINT BUY SYSTEM BUTTON]]    
    
Save and start the game.