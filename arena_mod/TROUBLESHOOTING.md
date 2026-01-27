# Troubleshooting Guide

## Critical Issue: Text Shows as Keys (e.g., "arena_construct_decision")

This is the #1 issue users face. It means CK3 isn't loading the localization file.

### Why This Happens

CK3 is very picky about:
1. File structure (must be exact)
2. File encoding (must be UTF-8 with BOM)
3. Mod installation (both .mod file and folder needed)

### Solution Steps

#### Step 1: Verify Your Installation

Your `mod/` folder should look EXACTLY like this:

```
mod/
├── arena_mod.mod          ← This file must be here
└── arena_mod/             ← This folder must be here
    ├── common/
    ├── events/
    ├── localization/
    │   └── english/
    │       └── arena_l_english.yml  ← This file is critical!
    ├── descriptor.mod
    └── arena_mod.mod
```

#### Step 2: Check the Localization File

1. Navigate to: `mod/arena_mod/localization/english/`
2. Verify `arena_l_english.yml` exists
3. Open it in Notepad - first line should be: `l_english:`
4. File size should be around 5-6 KB

#### Step 3: Complete Reinstall

If the file exists but still doesn't work:

1. **Close CK3 completely** (not just to main menu)
2. **Delete everything**:
   - Delete `mod/arena_mod/` folder
   - Delete `mod/arena_mod.mod` file
3. **Extract fresh from zip**:
   - Extract `arena_mod.zip`
   - Copy the `arena_mod` folder to `mod/`
   - Copy `arena_mod.mod` (from inside the arena_mod folder) to `mod/`
4. **Restart CK3 completely**
5. Enable the mod in the launcher
6. Start a game

#### Step 4: Check Error Log

If still not working, check for errors:

1. Go to: `Documents\Paradox Interactive\Crusader Kings III\logs\`
2. Open `error.log`
3. Search for "arena" or "localization"
4. Look for any error messages

Common errors:
- `Failed to load localization file` - File encoding issue
- `File not found` - Installation structure wrong
- `Parse error` - File corruption

### Still Not Working?

Try these:

1. **Verify game version**: Must be CK3 1.12 or later
2. **Disable other mods**: Test with ONLY this mod enabled
3. **Verify game files**: Steam → Right-click CK3 → Properties → Verify Integrity
4. **Check file permissions**: Make sure you can read/write to the mod folder

## Other Common Issues

### Decision Does Nothing When Clicked

**Cause**: Same as above - localization not loading
**Solution**: Follow the steps above

### Mod Doesn't Appear in Launcher

**Cause**: Missing files or wrong location

**Solution**:
1. Verify both `arena_mod.mod` AND `arena_mod/` folder exist in `mod/`
2. Restart the CK3 launcher
3. Check that `arena_mod.mod` contains:
   ```
   name="CK3 Fantasy Arena Mod"
   version="1.0.0"
   supported_version="1.12.*"
   ```

### Decisions Don't Appear In-Game

**For "Construct Arena"**:
- Must be a landed ruler
- Must have a capital county
- Capital must NOT already have an arena
- Must not be at war
- Must have 200+ gold

**For "Enter the Arena"**:
- Must be in a county with an arena
- Must be an adult
- Must not be imprisoned
- Must have prowess 6+
- Must not be injured
- Must not have 3+ consecutive losses

### Events Don't Fire

1. Open console with `` ` `` key
2. Look for error messages
3. Try: `event arena_construction.001` to test events
4. If that works, the issue is with the decision trigger

### Game Crashes

1. Check CK3 version (must be 1.12+)
2. Disable all other mods
3. Verify game files
4. Check error.log for crash details

## Getting Help

If none of this works:

1. Collect this information:
   - CK3 version
   - Operating system
   - Contents of `error.log`
   - Screenshot of your `mod/` folder structure
   - Screenshot of the in-game issue

2. Report on GitHub with all the above information

## Quick Diagnostic Commands

Open the CK3 console (`` ` `` key) and try:

```
# Test if events work
event arena_construction.001

# Test if localization loaded
# (If you see proper text, localization works)

# Check if mod is loaded
# (Look for "arena_mod" in the mods list)
```

If `event arena_construction.001` shows proper text, your localization IS working and the issue is elsewhere.
