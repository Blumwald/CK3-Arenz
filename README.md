# CK3 Fantasy Arena Mod - MVP Phase 1

A Crusader Kings 3 mod that adds a fantasy arena combat system where characters can fight as warriors, mages, or rogues, progressing through ranks and earning fame and rewards.

## Features

### Core Gameplay
- **Arena Construction**: Build arenas in your counties (200 gold)
- **Character Classes**: Choose from Warrior, Mage, or Rogue fighting styles
- **Rank Progression**: Advance through 3 ranks (F, E, D) based on victories
- **Bracket System**: Choose opponent difficulty (F, E, D brackets)
- **Rewards**: Earn gold, prestige, and fame based on rank and bracket
- **Risk & Consequences**: Face injuries and prestige loss on defeat
- **Fame System**: Build reputation through victories

### Combat System
- Uses vanilla CK3 duel mechanics (no custom animations)
- Class advantages: Warrior vs Warrior (neutral), Mage > Warrior, Rogue > Mage, Warrior > Rogue
- Difficulty scales with player prowess
- **Balanced lethality**: Death risk is constant (2%), does not increase with rank
- High-rank losses hurt prestige/fame, not survival

### Ranks (MVP)
1. **F Rank** - Novice (0-9 wins)
2. **E Rank** - Fighter (10-24 wins)
3. **D Rank** - Veteran (25+ wins)

### Classes
- **Warrior**: +2 Prowess, bonus vs Warriors
- **Mage**: +1 Prowess, strong vs Warriors, weak vs Rogues
- **Rogue**: +1 Prowess, strong vs Mages, weak vs Warriors

### Brackets
- **F Bracket**: Opponents at 60% of your prowess (easy)
- **E Bracket**: Opponents at 80% of your prowess (medium)
- **D Bracket**: Opponents at 100% of your prowess (hard)

## Installation

1. Download the mod
2. Extract to your CK3 mod folder:
   - **Windows**: `Documents/Paradox Interactive/Crusader Kings III/mod/`
   - **Linux**: `~/.local/share/Paradox Interactive/Crusader Kings III/mod/`
   - **Mac**: `~/Documents/Paradox Interactive/Crusader Kings III/mod/`
3. Launch CK3 and enable "CK3 Fantasy Arena Mod" in the launcher
4. Start or load a game

## How to Play

### Building an Arena
1. As a landed ruler, use the "Construct Arena" decision
2. Pay 200 gold to build the arena in your capital county
3. The arena becomes available immediately

### Entering the Arena
1. Travel to a county with an arena
2. Use the "Enter the Arena" decision
3. Choose your fighting class (first time only)
4. Select opponent bracket (difficulty)
5. Fight in a vanilla duel
6. Receive rewards or consequences based on outcome

### Progression
- Win matches to increase your win count
- Reach 10 wins to advance to E Rank
- Reach 25 wins to advance to D Rank
- Higher ranks provide better rewards but more severe consequences for losses

### Risk Management
- **Injury chance**: 5% base, modified by prowess difference
- **Death chance**: 2% constant (does not scale with rank)
- **Prestige loss on defeat**: Scales with rank (more painful at higher ranks)
- **Avoid fighting when**: Injured, on 3+ loss streak, or low prowess

## Configuration

Edit values in [`common/scripted_values/arena_values.txt`](common/scripted_values/arena_values.txt):

```
arena_construction_cost = 200          # Gold cost to build arena
arena_base_injury_chance = 0.05        # 5% base injury chance on loss
arena_death_chance = 0.02              # 2% death chance (constant)
arena_base_gold = 50                   # Base gold reward (multiplied by rank²)
arena_base_prestige = 25               # Base prestige reward (multiplied by rank²)
bracket_f_mult = 0.6                   # F bracket difficulty
bracket_e_mult = 0.8                   # E bracket difficulty
bracket_d_mult = 1.0                   # D bracket difficulty
```

## AI Behavior

- AI characters have a 10% yearly chance to participate if eligible
- Modified by traits (Brave +50%, Craven -75%)
- AI avoids arena when:
  - Prowess < 8
  - Currently injured
  - On 3+ loss streak
  - Is heir and risk-averse

## Compatibility

- **CK3 Version**: 1.12.x or later
- **Ironman Compatible**: Yes
- **Save Game Compatible**: Yes (can be added to existing saves)
- **Multiplayer**: Should work, but untested
- **Other Mods**: Compatible with most mods (no vanilla file overwrites)

## Technical Details

### File Structure
```
arena_mod/
├── descriptor.mod
├── arena_mod.mod
├── common/
│   ├── decisions/          # Arena construction and entry
│   ├── traits/             # Ranks and classes
│   ├── modifiers/          # Combat bonuses and penalties
│   ├── scripted_triggers/  # Validation logic
│   ├── scripted_effects/   # Core mechanics
│   ├── scripted_values/    # Configuration values
│   └── on_actions/         # Duel integration hooks
├── events/
│   ├── arena_construction_events.txt
│   ├── arena_entry_events.txt
│   ├── arena_combat_events.txt
│   └── arena_resolution_events.txt
└── localization/english/
    └── arena_l_english.yml
```

### Key Systems
- **County-based arenas**: Uses county modifiers (stable across patches)
- **Vanilla duel integration**: Hooks into `on_duel_end` to detect arena matches
- **Character variables**: Tracks wins, losses, rank, fame
- **Scripted effects**: Modular, reusable combat logic
- **No custom animations**: 100% vanilla-compatible

## Known Issues

None currently. Please report issues on the mod page.

## Future Phases

### Phase 2 (Planned)
- 9 rank tiers (F to SSS)
- Expanded classes (Swordsman, Knight, Archer, Necromancer, etc.)
- Multiple arena types
- Rivalry system
- Enhanced fame/reputation mechanics

### Phase 3 (Planned)
- Arena hosting (organize tournaments)
- Spectator events
- Arena betting
- Team battles

## Credits

- **Design**: Based on fantasy arena combat systems
- **Implementation**: Vanilla CK3 mechanics only
- **Compatibility**: Designed for long-term stability

## License

This mod is free to use and modify. Credit appreciated but not required.

## Support

For bug reports, suggestions, or questions, please visit the mod page.

---

**Version**: 1.0.0 (MVP Phase 1)  
**Last Updated**: 2026-01-27  
**CK3 Version**: 1.12.x+
