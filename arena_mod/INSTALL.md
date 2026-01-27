# Installation & Quick Start Guide

## Installation

### Method 1: Manual Installation

1. **Locate your CK3 mod folder**:
   - **Windows**: `Documents/Paradox Interactive/Crusader Kings III/mod/`
   - **Linux**: `~/.local/share/Paradox Interactive/Crusader Kings III/mod/`
   - **Mac**: `~/Documents/Paradox Interactive/Crusader Kings III/mod/`

2. **Copy the mod files**:
   - Copy the entire `arena_mod` folder to your mod directory
   - Copy `arena_mod.mod` to your mod directory
   
   Your structure should look like:
   ```
   mod/
   ├── arena_mod/           (folder with all mod files)
   └── arena_mod.mod        (descriptor file)
   ```

3. **Launch CK3**:
   - Open the CK3 launcher
   - Go to "Playsets" or "Mods"
   - Enable "CK3 Fantasy Arena Mod"
   - Click "Play"

### Method 2: Steam Workshop (if published)

1. Subscribe to the mod on Steam Workshop
2. Launch CK3
3. Enable the mod in the launcher
4. Play!

## Quick Start Guide

### 5-Minute Tutorial

#### Step 1: Build an Arena (2 minutes)
1. Start or load a game as any landed ruler
2. Make sure you have at least 200 gold
3. Open the Decisions menu (hotkey: `D`)
4. Find "Construct Arena" decision
5. Click to build (costs 200 gold)
6. Arena is now built in your capital county!

#### Step 2: Enter the Arena (1 minute)
1. Make sure you're in a county with an arena
2. Open the Decisions menu
3. Find "Enter the Arena" decision
4. Click to enter

#### Step 3: Choose Your Class (30 seconds)
First time only:
- **Warrior**: Best for high prowess characters (+2 prowess)
- **Mage**: Balanced, good vs Warriors (+1 prowess)
- **Rogue**: Tricky, good vs Mages (+1 prowess)

#### Step 4: Choose Difficulty (30 seconds)
- **F Bracket**: Easy opponents (60% of your prowess) - Good for beginners
- **E Bracket**: Medium opponents (80% of your prowess) - Unlocks at E rank
- **D Bracket**: Hard opponents (100% of your prowess) - Unlocks at D rank

#### Step 5: Fight! (1 minute)
- The vanilla duel interface appears
- Fight normally
- Win or lose, you'll get an event with results

### Your First Match

**Recommended Setup**:
- Character with prowess 10+
- Choose Warrior class (simplest)
- Choose F bracket (easiest)
- Win to get your first victory!

**What to Expect**:
- **If you win**: Gold, prestige, fame, and progress toward next rank
- **If you lose**: Small prestige loss, small injury chance (5%), tiny death chance (2%)

### Progression Path

1. **Start**: F Rank (0 wins)
2. **10 wins**: Advance to E Rank
3. **25 wins**: Advance to D Rank
4. **Each rank**: Better rewards, but harsher penalties for losses

### Tips for Success

✅ **DO**:
- Start with F bracket until you're comfortable
- Fight when your prowess is high
- Use class advantages (Mage > Warrior > Rogue > Mage)
- Build multiple arenas across your realm
- Fight when you're healthy

❌ **DON'T**:
- Fight when injured or wounded
- Fight after 3 consecutive losses
- Choose brackets above your rank
- Ignore your character's prowess stat
- Fight if you're the only heir (risky!)

## Troubleshooting

### Text shows as "arena_construct_decision" instead of proper names
**This is the most common issue!** It means the localization file isn't loading.

**Solution**:
1. **Verify file structure**: Check that `mod/arena_mod/localization/english/arena_l_english.yml` exists
2. **Complete reinstall**:
   - Close CK3 completely
   - Delete BOTH `mod/arena_mod/` folder AND `mod/arena_mod.mod` file
   - Extract the zip fresh and copy both files again
   - Make sure you copy the ENTIRE `arena_mod` folder, not just some files
3. **Restart CK3 completely** (not just to main menu - close the entire game)
4. If still not working, check your CK3 `error.log` file at:
   - Windows: `Documents\Paradox Interactive\Crusader Kings III\logs\error.log`
   - Look for errors mentioning "arena" or "localization"

### Decision does nothing when clicked
This is usually caused by the same localization issue above. Follow the same solution.

### Mod doesn't appear in launcher
- Check that both `arena_mod` folder and `arena_mod.mod` file are in the mod directory
- Restart the CK3 launcher
- Check file permissions

### Decisions don't appear
- Make sure you're a landed ruler (for construction)
- Make sure you're in a county with an arena (for entry)
- Check that your character meets requirements (adult, not imprisoned, prowess 6+)

### Events don't fire
- Check the console for errors (open with `` ` `` key)
- Make sure mod is enabled
- Try reloading the save

### Opponent doesn't appear
- This is a rare bug - if it happens, the match should auto-cancel
- Try entering the arena again

### Game crashes
- Check CK3 version compatibility (1.12.x+)
- Disable other mods to test for conflicts
- Report the issue with error logs

## Configuration

Want to tweak the mod? Edit [`common/scripted_values/arena_values.txt`](common/scripted_values/arena_values.txt):

```
# Make arenas cheaper
arena_construction_cost = 100  # Default: 200

# Make combat safer
arena_base_injury_chance = 0.02  # Default: 0.05
arena_death_chance = 0.01        # Default: 0.02

# Increase rewards
arena_base_gold = 100      # Default: 50
arena_base_prestige = 50   # Default: 25

# Make opponents easier
bracket_f_mult = 0.4  # Default: 0.6
bracket_e_mult = 0.6  # Default: 0.8
bracket_d_mult = 0.8  # Default: 1.0
```

## Uninstallation

1. Disable the mod in the CK3 launcher
2. Load your save (arena features will be inactive)
3. Delete `arena_mod` folder and `arena_mod.mod` file from mod directory

**Note**: Characters will keep arena traits, but they won't do anything. This is safe.

## Getting Help

- Read the [README.md](README.md) for full documentation
- Check the [TESTING_CHECKLIST.md](TESTING_CHECKLIST.md) for known issues
- Report bugs with:
  - CK3 version
  - Steps to reproduce
  - Console errors (if any)

## Next Steps

Once you're comfortable with the basics:
- Try different classes and find your favorite
- Challenge yourself with higher brackets
- Build arenas across your realm
- Watch AI characters participate
- Aim for D rank (25+ wins)!

---

**Have fun and may your blade stay sharp!** ⚔️
