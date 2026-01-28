# Technical Design Document

## Architecture Overview

The CK3 Fantasy Arena Mod is built using only vanilla CK3 systems with no custom animations or unstable features. This document explains the technical implementation for developers and modders.

## Core Design Principles

1. **Vanilla-Only**: No custom animations, skeletons, or undocumented features
2. **Patch-Resilient**: Uses stable systems that survive game updates
3. **County-Based**: Arenas are county modifiers, not buildings (more stable)
4. **Duel Integration**: Wraps vanilla duel system with arena narrative
5. **Variable Tracking**: Uses character variables for progression
6. **Modular Effects**: Scripted effects for reusable logic

## System Architecture

### Data Flow

```
Player Decision (Enter Arena)
    ↓
Class Selection Event (arena_entry.001)
    ↓
Bracket Selection Event (arena_entry.002)
    ↓
Match Creation Event (arena_combat.001)
    ├─ Generate Opponent NPC
    ├─ Apply Class Bonuses
    └─ Custom Duel System (prowess-based)
        ├─ Compare prowess values
        ├─ Apply modifiers
        └─ Determine winner/loser
    ↓
Resolution Event (arena_resolution.001 or .002)
    ├─ Calculate Rewards/Penalties
    ├─ Update Variables
    ├─ Apply Modifiers
    └─ Cleanup (remove opponent NPC)
```

### Key Components

#### 1. County Modifiers (Stable)
```
arena_location = {
    monthly_county_control_growth_add = 0.1
    county_opinion_add = 5
}
```
- **Why**: County modifiers are more stable than buildings across patches
- **Persistence**: Permanent until removed
- **Detection**: Used by triggers to enable arena entry

#### 2. Character Variables (Integer Only)
```
arena_wins = 0..∞          # Total victories
arena_losses = 0..∞        # Total defeats
arena_rank = 1..3          # Current rank (MVP: 3 ranks)
arena_fame = 0..∞          # Cumulative fame
arena_consecutive_losses   # Safety tracking
arena_chosen_bracket       # Temporary: difficulty multiplier
arena_target_prowess       # Temporary: opponent prowess
arena_prowess_difference   # Temporary: for injury calc
```
- **Why**: Integers are stable and performant
- **Scope**: Character-scoped, persist across saves
- **Cleanup**: Temporary variables removed after match

#### 3. Traits (Visible & Hidden)

**Rank Traits** (Visible):
- `arena_rank_f`, `arena_rank_e`, `arena_rank_d`
- Mutually exclusive
- Provide fame/prestige context

**Class Traits** (Hidden):
- `arena_class_warrior` (+2 prowess)
- `arena_class_mage` (+1 prowess)
- `arena_class_rogue` (+1 prowess)
- Mutually exclusive
- Permanent once chosen

**Status Traits** (Temporary):
- `arena_in_match`: Flags active match for duel hook
- `arena_victor`: +10 opinion for 30 days

#### 4. Scripted Effects (Modular)

**Core Effects**:
- `arena_initialize_participant`: First-time setup
- `arena_generate_opponent`: NPC creation with calculated prowess
- `arena_apply_class_bonuses`: Class advantage modifiers
- `arena_handle_victory`: Rewards, fame, rank progression
- `arena_handle_defeat`: Penalties, injuries, death chance
- `arena_cleanup_match`: Remove flags, modifiers, variables
- `arena_update_rank`: Check and apply rank progression

**Design Pattern**:
```
effect_name = {
    # Validation
    if = { limit = { ... } }
    
    # Core logic
    ...
    
    # Cleanup
    ...
}
```

#### 5. Duel Integration (Critical)

**Hook Mechanism**:
```
on_duel_end = {
    on_actions = {
        arena_duel_end_handler
    }
}

arena_duel_end_handler = {
    trigger = {
        scope:winner = { has_trait = arena_in_match }
    }
    effect = {
        # Set flags and trigger resolution
    }
}
```

**Why This Works**:
- `arena_in_match` trait distinguishes arena duels from vanilla duels
- `on_duel_end` is a stable hook that fires after any duel
- Flags (`arena_won_duel`, `arena_lost_duel`) persist for 1 day
- Resolution events fire 1 day later to ensure clean scope

**Failure Modes**:
- If player dies during duel: Cleanup happens via character death
- If opponent dies: NPC cleaned up in resolution event
- If duel cancelled: Match flags expire after 1 day

## Balance Calculations

### Opponent Prowess
```
target_prowess = player_prowess × bracket_multiplier
bracket_multiplier = {
    F: 0.6 (easy)
    E: 0.8 (medium)
    D: 1.0 (hard)
}
Clamped to [6, 30]
```

### Rewards (Scale with Rank²)
```
gold = 50 × rank² × bracket_multiplier
prestige = 25 × rank² × bracket_multiplier

Examples:
- F rank, F bracket: 50 × 1² × 0.6 = 30 gold
- E rank, E bracket: 50 × 2² × 0.8 = 160 gold
- D rank, D bracket: 50 × 3² × 1.0 = 450 gold
```

### Penalties (Scale with Rank²)
```
prestige_loss = -50 × rank²
fame_loss = -20 × rank

Examples:
- F rank loss: -50 prestige, -20 fame
- E rank loss: -200 prestige, -40 fame
- D rank loss: -450 prestige, -60 fame
```

### Injury Chance (Decreases with Prowess)
```
base = 5%
modifier = prowess_difference / 20

If player has +5 prowess advantage:
    injury_chance = 5% - (5/20) = 2.5%

If player has -5 prowess disadvantage:
    injury_chance = 5% + (5/15) = 8.3%

Clamped to [1%, 25%]
```

### Death Chance (Constant)
```
death_chance = 2% (always)
```
**Critical**: Death chance does NOT scale with rank. High-rank losses are punishing via prestige/fame, not lethality.

## Class Advantage System

### Matchup Matrix
```
           vs Warrior  vs Mage  vs Rogue
Warrior    +2 prowess  -        -1 prowess
Mage       +2 prowess  -        -1 prowess
Rogue      -1 prowess  +2       -
```

### Implementation
Applied via temporary character modifiers (1 day duration):
- `arena_warrior_advantage`: +2 prowess
- `arena_mage_vs_warrior`: +2 prowess
- `arena_mage_vs_rogue`: -1 prowess
- `arena_rogue_vs_mage`: +2 prowess
- `arena_rogue_vs_warrior`: -1 prowess

## NPC Opponent Generation

### Creation Process
```
create_character = {
    template = arena_fighter_template
    dynasty = none
    age = { 25 35 }
    gender = random
    prowess = calculated_target_prowess
    
    # Random class assignment
    random_list = {
        33 = { add_trait = arena_class_warrior }
        33 = { add_trait = arena_class_mage }
        34 = { add_trait = arena_class_rogue }
    }
    
    save_scope_as = opponent
}
```

### Cleanup
```
scope:opponent = {
    death = { death_reason = death_vanished }
}
```
- Executed in resolution events
- Prevents NPC accumulation
- Uses `death_vanished` to avoid death notifications

## AI Participation

### Yearly Pulse
```
yearly_global_pulse = {
    on_actions = {
        arena_ai_participation_check
    }
}
```

### AI Decision Logic
```
Base chance: 10%
Modifiers:
- Brave: ×2
- Craven: ×0.5
- High prowess (15+): ×1.5

Safety checks:
- Prowess < 8: Never
- Injured: Never
- 3+ consecutive losses: Never
- Is heir + risk-averse: Never
```

## Performance Considerations

### Memory Management
- **NPC Cleanup**: All opponent NPCs killed after match
- **Variable Cleanup**: Temporary variables removed
- **Modifier Cleanup**: Temporary modifiers expire

### Optimization
- **Event Delays**: 1-day delays prevent scope issues
- **Trigger Caching**: Scripted triggers reused
- **Value Caching**: Scripted values calculated once

### Scalability
- **Per-Character**: ~10 variables max
- **Per-County**: 1 modifier max
- **Global**: No global variables
- **NPC Count**: 0 (all cleaned up)

## Error Handling

### Validation Triggers
```
arena_valid_participant = {
    is_adult = yes
    is_alive = yes
    is_imprisoned = no
    is_incapable = no
    prowess >= 6
    NOT = { has_trait = arena_in_match }
}
```

### Failure Modes

**No Opponent Generated**:
- Fallback: Create NPC with exact target prowess
- Never fails due to random_list

**Player Dies During Match**:
- Cleanup: Character death removes all flags/modifiers
- Opponent: Cleaned up via death event

**County Loses Arena**:
- Entry decision becomes unavailable
- Active matches complete normally

**Save/Load During Match**:
- Flags persist across saves
- Match completes normally

## Future Expansion (Phase 2+)

### Planned Features
1. **9 Rank Tiers**: F, E, D, C, B, A, S, SS, SSS
2. **Expanded Classes**: 15+ classes with unique abilities
3. **Arena Types**: Different arenas with special rules
4. **Rivalry System**: Track opponents and rematches
5. **Spectator Events**: Audience reactions and betting

### Technical Considerations
- **Rank Expansion**: Add more traits, update thresholds
- **Class Expansion**: More hidden traits, more matchup modifiers
- **Arena Types**: Additional county modifiers with different effects
- **Rivalries**: Character relations + variables
- **Spectators**: Event chains triggered by matches

### Compatibility
- All Phase 2+ features will be additive
- No breaking changes to Phase 1 saves
- Backward compatible with existing arenas

## Debugging

### Console Commands
```
# Add arena to current county
effect = { capital_county = { add_county_modifier = arena_location } }

# Set wins
effect = { set_variable = { name = arena_wins value = 25 } }

# Set rank
effect = { add_trait = arena_rank_d }

# Add fame
effect = { set_variable = { name = arena_fame value = 1000 } }

# Test opponent generation
effect = { arena_generate_opponent = yes }
```

### Common Issues

**Events not firing**:
- Check `arena_in_match` trait is present
- Check console for script errors
- Verify on_actions file loaded

**Opponent too strong/weak**:
- Check `arena_chosen_bracket` variable
- Verify prowess calculation in scripted_values
- Check class bonuses applied

**Variables not updating**:
- Check scope (character vs county)
- Verify variable names match exactly
- Check for typos in scripted_effects

## Code Style Guidelines

### Naming Conventions
- **Prefix**: All identifiers start with `arena_`
- **Traits**: `arena_rank_X`, `arena_class_X`
- **Events**: `arena_category.###`
- **Effects**: `arena_verb_noun`
- **Triggers**: `arena_adjective_noun`

### File Organization
- **One system per file**: Don't mix unrelated code
- **Logical grouping**: Related effects in same file
- **Comments**: Explain complex logic
- **Whitespace**: Blank lines between major sections

### Best Practices
- **Validate inputs**: Always check scopes exist
- **Clean up**: Remove temporary data
- **Fail gracefully**: Handle edge cases
- **Test thoroughly**: Use checklist

## Version History

### v1.0.0 (MVP Phase 1)
- Initial release
- 3 ranks, 3 classes, 3 brackets
- County-based arenas
- Vanilla duel integration
- Balanced lethality system

---

**For questions or contributions, see README.md**
