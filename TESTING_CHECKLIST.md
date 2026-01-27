# CK3 Fantasy Arena Mod - Testing Checklist

## Pre-Testing Setup

- [ ] Mod files placed in correct directory
- [ ] Mod enabled in CK3 launcher
- [ ] Game launches without errors
- [ ] No console errors on main menu

## Core Functionality Tests

### Arena Construction
- [ ] "Construct Arena" decision appears for landed rulers
- [ ] Decision requires 200 gold
- [ ] Decision blocked when at war
- [ ] Decision blocked if county already has arena
- [ ] Construction completes successfully
- [ ] County receives `arena_location` modifier
- [ ] Construction event fires (arena_construction.001)
- [ ] Player receives prestige (+100)
- [ ] Vassals gain opinion bonus

### Arena Entry
- [ ] "Enter the Arena" decision appears in counties with arenas
- [ ] Decision blocked when character is imprisoned
- [ ] Decision blocked when character is incapable
- [ ] Decision blocked when character has prowess < 6
- [ ] Decision blocked when character is injured
- [ ] Decision blocked after 3 consecutive losses
- [ ] Entry event fires (arena_entry.001)

### Class Selection
- [ ] First-time entry shows all 3 class options
- [ ] Warrior class grants +2 prowess
- [ ] Mage class grants +1 prowess
- [ ] Rogue class grants +1 prowess
- [ ] Class traits are mutually exclusive
- [ ] Subsequent entries show "Keep current style" option
- [ ] Class selection proceeds to bracket selection

### Bracket Selection
- [ ] F bracket always available
- [ ] E bracket only available at E rank or higher
- [ ] D bracket only available at D rank
- [ ] Bracket selection proceeds to match creation
- [ ] Cancel option works correctly

### Match Creation
- [ ] Opponent is generated successfully
- [ ] Opponent has appropriate prowess (based on bracket)
- [ ] Opponent has random class assigned
- [ ] Class bonuses applied correctly:
  - [ ] Warrior vs Warrior: +2 prowess
  - [ ] Mage vs Warrior: +2 prowess
  - [ ] Mage vs Rogue: -1 prowess
  - [ ] Rogue vs Mage: +2 prowess
  - [ ] Rogue vs Warrior: -1 prowess
- [ ] `arena_in_match` trait added
- [ ] Prowess difference calculated and stored
- [ ] Duel starts successfully

### Duel Integration
- [ ] Vanilla duel interface appears
- [ ] Duel resolves normally
- [ ] Winner/loser determined correctly
- [ ] `on_duel_end` hook triggers
- [ ] Arena flags set correctly (arena_won_duel / arena_lost_duel)
- [ ] Resolution events fire after 1 day

### Victory Resolution
- [ ] Victory event fires (arena_resolution.001)
- [ ] Win count increments by 1
- [ ] Consecutive losses reset to 0
- [ ] Fame increases (10 × rank)
- [ ] Gold reward calculated correctly (50 × rank²)
- [ ] Prestige reward calculated correctly (25 × rank²)
- [ ] `arena_victor` trait added
- [ ] Victor trait removed after 30 days
- [ ] Stress relief for Brave/Wrathful/Arrogant (-10)
- [ ] Rank update check runs
- [ ] Opponent NPC cleaned up
- [ ] Match flags cleared

### Defeat Resolution
- [ ] Defeat event fires (arena_resolution.002)
- [ ] Loss count increments by 1
- [ ] Consecutive losses tracked correctly
- [ ] Fame decreases (-20 × rank)
- [ ] Prestige loss calculated correctly (-50 × rank²)
- [ ] Crushing defeat modifier applied at rank 2+ (1 year duration)
- [ ] Stress gain for Ambitious/Proud (+10)
- [ ] Injury chance calculated correctly
- [ ] Injury applied when triggered (wounded trait + modifier)
- [ ] Death chance is 2% (constant)
- [ ] Death occurs when triggered
- [ ] Opponent NPC cleaned up
- [ ] Match flags cleared

### Rank Progression
- [ ] F rank assigned on first entry
- [ ] E rank unlocked at 10 wins
- [ ] D rank unlocked at 25 wins
- [ ] Rank up event fires (arena_resolution.100)
- [ ] Old rank trait removed
- [ ] New rank trait added
- [ ] `arena_rank` variable updated
- [ ] Prestige bonus for rank up (100 × rank)
- [ ] Fame bonus for rank up (50 × rank)

### Fame System
- [ ] Fame tracked correctly in `arena_fame` variable
- [ ] Fame increases on victory
- [ ] Fame decreases on defeat
- [ ] Fame modifiers applied at correct thresholds:
  - [ ] Known (101-500): +5 opinion
  - [ ] Famous (501-1500): +10 opinion
  - [ ] Legendary (1501+): +20 opinion
- [ ] Fame modifiers update after each match

## Balance Tests

### Difficulty Scaling
- [ ] F bracket opponents are noticeably easier
- [ ] E bracket opponents are moderately challenging
- [ ] D bracket opponents are evenly matched
- [ ] Opponent prowess scales with player prowess
- [ ] Class advantages/disadvantages are noticeable

### Risk vs Reward
- [ ] Higher brackets provide better rewards
- [ ] Injury chance decreases with prowess advantage
- [ ] Injury chance increases with prowess disadvantage
- [ ] Death chance remains constant at 2%
- [ ] High-rank defeats are more punishing (prestige/fame)
- [ ] Low-rank defeats are less punishing

### Progression Pacing
- [ ] Reaching E rank (10 wins) feels achievable
- [ ] Reaching D rank (25 wins) requires commitment
- [ ] Rewards scale appropriately with rank
- [ ] Losses at high ranks are catastrophic (prestige-wise)

## AI Behavior Tests

### AI Participation
- [ ] AI characters with high prowess participate
- [ ] AI characters with Brave trait participate more
- [ ] AI characters with Craven trait participate less
- [ ] AI avoids arena when prowess < 8
- [ ] AI avoids arena when injured
- [ ] AI avoids arena after 3 consecutive losses
- [ ] AI heirs participate less frequently

### AI Safety
- [ ] AI doesn't participate when it would be suicidal
- [ ] AI stops after multiple losses
- [ ] AI doesn't break the game with excessive participation

## Error Handling Tests

### Edge Cases
- [ ] No crash if opponent generation fails
- [ ] No crash if player dies during match
- [ ] No crash if county loses arena modifier mid-match
- [ ] No crash if character becomes imprisoned mid-match
- [ ] Variables clean up correctly on character death
- [ ] Modifiers clean up correctly on character death

### Save/Load
- [ ] Arena state persists across save/load
- [ ] Win/loss counts persist
- [ ] Rank persists
- [ ] Fame persists
- [ ] Active matches handle save/load correctly

### Multiplayer (if applicable)
- [ ] Both players can construct arenas
- [ ] Both players can participate
- [ ] No desync issues
- [ ] Events fire correctly for both players

## Performance Tests

### Memory
- [ ] No memory leaks from NPC generation
- [ ] Opponent NPCs cleaned up properly
- [ ] No accumulation of dead characters

### Performance
- [ ] No lag when entering arena
- [ ] No lag during match creation
- [ ] No lag from AI participation checks
- [ ] Game runs smoothly with multiple arenas

## Compatibility Tests

### Vanilla Compatibility
- [ ] No conflicts with vanilla duels
- [ ] No conflicts with vanilla traits
- [ ] No conflicts with vanilla events
- [ ] Works with all vanilla cultures/religions

### Ironman Mode
- [ ] Mod works in Ironman mode
- [ ] Achievements still available (if applicable)
- [ ] No save corruption

### Existing Saves
- [ ] Can be added to existing saves
- [ ] Doesn't break existing characters
- [ ] Doesn't break existing counties

## Localization Tests

- [ ] All decision text displays correctly
- [ ] All event text displays correctly
- [ ] All trait descriptions display correctly
- [ ] All modifier descriptions display correctly
- [ ] No missing localization keys (check for "arena_" in game)

## Final Checks

- [ ] No console errors during gameplay
- [ ] No crashes during extended play session
- [ ] No save corruption
- [ ] All features work as documented in README
- [ ] Balance feels appropriate
- [ ] Fun to play!

## Bug Reporting Template

If you find a bug, please report with:
- CK3 version
- Mod version
- Steps to reproduce
- Expected behavior
- Actual behavior
- Console errors (if any)
- Save file (if possible)

---

## Testing Notes

**Tester Name**: _______________  
**Date**: _______________  
**CK3 Version**: _______________  
**Mod Version**: _______________  

**Overall Assessment**:
- [ ] Ready for release
- [ ] Needs minor fixes
- [ ] Needs major fixes
- [ ] Not ready

**Additional Comments**:
