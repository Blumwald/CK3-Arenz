# File Manifest - CK3 Fantasy Arena Mod v1.0.0

Complete list of all mod files with descriptions.

## Root Files

| File | Purpose | Required |
|------|---------|----------|
| [`descriptor.mod`](descriptor.mod) | Mod metadata for game launcher | ✅ Yes |
| [`arena_mod.mod`](arena_mod.mod) | Mod descriptor (copy of descriptor.mod) | ✅ Yes |
| [`README.md`](README.md) | Main documentation and feature overview | 📖 Docs |
| [`INSTALL.md`](INSTALL.md) | Installation and quick start guide | 📖 Docs |
| [`TESTING_CHECKLIST.md`](TESTING_CHECKLIST.md) | Comprehensive testing checklist | 🧪 Testing |
| [`TECHNICAL_DESIGN.md`](TECHNICAL_DESIGN.md) | Technical architecture documentation | 🔧 Dev |
| [`FILE_MANIFEST.md`](FILE_MANIFEST.md) | This file - complete file listing | 📋 Reference |

## Common Files (Game Logic)

### Decisions
| File | Purpose | Lines |
|------|---------|-------|
| [`common/decisions/arena_decisions.txt`](common/decisions/arena_decisions.txt) | Arena construction and entry decisions | ~100 |

**Contains**:
- `arena_construct_decision`: Build arena in county (200 gold)
- `arena_enter_decision`: Enter arena for combat
- AI behavior logic for both decisions

### Traits
| File | Purpose | Lines |
|------|---------|-------|
| [`common/traits/arena_traits.txt`](common/traits/arena_traits.txt) | All arena-related character traits | ~150 |

**Contains**:
- Rank traits: `arena_rank_f`, `arena_rank_e`, `arena_rank_d`
- Class traits: `arena_class_warrior`, `arena_class_mage`, `arena_class_rogue`
- Status traits: `arena_in_match`, `arena_victor`

### Modifiers
| File | Purpose | Lines |
|------|---------|-------|
| [`common/modifiers/arena_modifiers.txt`](common/modifiers/arena_modifiers.txt) | County and character modifiers | ~60 |

**Contains**:
- County: `arena_location` (marks arena presence)
- Combat: Class advantage/disadvantage modifiers
- Consequences: `arena_recent_injury`, `arena_crushing_defeat`
- Fame: `arena_fame_known`, `arena_fame_famous`, `arena_fame_legendary`

### Scripted Triggers
| File | Purpose | Lines |
|------|---------|-------|
| [`common/scripted_triggers/arena_triggers.txt`](common/scripted_triggers/arena_triggers.txt) | Validation and condition checks | ~150 |

**Contains**:
- `arena_valid_participant`: Check if character can fight
- `county_has_arena`: Check if county has arena
- `can_choose_bracket_X`: Bracket eligibility checks
- `qualifies_for_rank_X`: Rank progression checks
- `arena_has_class_advantage/disadvantage`: Matchup checks
- Fame tier checks and safety checks

### Scripted Effects
| File | Purpose | Lines |
|------|---------|-------|
| [`common/scripted_effects/arena_effects.txt`](common/scripted_effects/arena_effects.txt) | Core gameplay mechanics | ~300 |

**Contains**:
- `arena_initialize_participant`: First-time setup
- `arena_update_rank`: Rank progression logic
- `arena_set_class_X`: Class selection
- `arena_set_bracket_X`: Bracket selection
- `arena_generate_opponent`: NPC creation
- `arena_apply_class_bonuses`: Combat modifiers
- `arena_handle_victory`: Victory rewards
- `arena_handle_defeat`: Defeat consequences
- `arena_cleanup_match`: Post-match cleanup
- `arena_update_fame_modifiers`: Fame system

### Scripted Values
| File | Purpose | Lines |
|------|---------|-------|
| [`common/scripted_values/arena_values.txt`](common/scripted_values/arena_values.txt) | Configuration and calculations | ~150 |

**Contains**:
- Configuration constants (costs, chances, rewards)
- Bracket multipliers
- Rank thresholds
- Calculated values (rewards, penalties, opponent prowess)

### On Actions
| File | Purpose | Lines |
|------|---------|-------|
| [`common/on_actions/arena_on_actions.txt`](common/on_actions/arena_on_actions.txt) | Event hooks and triggers | ~80 |

**Contains**:
- `arena_duel_end_handler`: Hooks into vanilla duel system
- `arena_ai_participation_check`: Yearly AI participation pulse

## Event Files

### Construction Events
| File | Purpose | Lines |
|------|---------|-------|
| [`events/arena_construction_events.txt`](events/arena_construction_events.txt) | Arena construction completion | ~30 |

**Contains**:
- `arena_construction.001`: Construction complete event

### Entry Events
| File | Purpose | Lines |
|------|---------|-------|
| [`events/arena_entry_events.txt`](events/arena_entry_events.txt) | Class and bracket selection | ~100 |

**Contains**:
- `arena_entry.001`: Class selection event
- `arena_entry.002`: Bracket selection event

### Combat Events
| File | Purpose | Lines |
|------|---------|-------|
| [`events/arena_combat_events.txt`](events/arena_combat_events.txt) | Match creation and duel start | ~80 |

**Contains**:
- `arena_combat.001`: Match creation and duel initiation
- `arena_combat.002`: Pre-duel flavor event (optional)

### Resolution Events
| File | Purpose | Lines |
|------|---------|-------|
| [`events/arena_resolution_events.txt`](events/arena_resolution_events.txt) | Post-combat outcomes | ~150 |

**Contains**:
- `arena_resolution.001`: Victory event
- `arena_resolution.002`: Defeat event
- `arena_resolution.100`: Rank up event
- `arena_resolution.200`: Victor trait removal (hidden)
- `arena_resolution.300`: Injury recovery event
- `arena_resolution.400`: Crushing defeat recovery event

## Localization Files

| File | Purpose | Lines |
|------|---------|-------|
| [`localization/english/arena_l_english.yml`](localization/english/arena_l_english.yml) | All English text | ~100 |

**Contains**:
- Trait names and descriptions
- Modifier names
- Decision text
- Event titles and descriptions
- All player-facing text

## Graphics Files

| Directory | Purpose | Status |
|-----------|---------|--------|
| `gfx/interface/icons/arena_traits/` | Trait icons (optional) | 📁 Empty |

**Note**: Trait icons are optional. The mod uses vanilla icons by default.

## File Statistics

### Total Files
- **Core Logic**: 7 files (decisions, traits, modifiers, triggers, effects, values, on_actions)
- **Events**: 4 files (construction, entry, combat, resolution)
- **Localization**: 1 file (English)
- **Documentation**: 5 files (README, INSTALL, TESTING, TECHNICAL, MANIFEST)
- **Descriptors**: 2 files (descriptor.mod, arena_mod.mod)

**Total**: 19 files

### Lines of Code
- **Core Logic**: ~890 lines
- **Events**: ~360 lines
- **Localization**: ~100 lines
- **Documentation**: ~1500 lines

**Total**: ~2850 lines

### File Size
- **Total mod size**: ~150 KB (text only)
- **With documentation**: ~200 KB

## File Dependencies

### Load Order
CK3 loads files in this order:
1. `common/` files (all loaded simultaneously)
2. `events/` files (all loaded simultaneously)
3. `localization/` files (loaded on demand)

### Internal Dependencies
```
Decisions → Triggers, Effects, Values
Events → Triggers, Effects, Values, Traits, Modifiers
On Actions → Events, Triggers
Effects → Triggers, Values, Traits, Modifiers
Triggers → Traits, Modifiers
```

## Modification Guide

### To Change Balance
Edit: [`common/scripted_values/arena_values.txt`](common/scripted_values/arena_values.txt)

### To Add New Ranks
1. Add traits in [`common/traits/arena_traits.txt`](common/traits/arena_traits.txt)
2. Add thresholds in [`common/scripted_values/arena_values.txt`](common/scripted_values/arena_values.txt)
3. Add triggers in [`common/scripted_triggers/arena_triggers.txt`](common/scripted_triggers/arena_triggers.txt)
4. Update rank logic in [`common/scripted_effects/arena_effects.txt`](common/scripted_effects/arena_effects.txt)
5. Add localization in [`localization/english/arena_l_english.yml`](localization/english/arena_l_english.yml)

### To Add New Classes
1. Add trait in [`common/traits/arena_traits.txt`](common/traits/arena_traits.txt)
2. Add modifiers in [`common/modifiers/arena_modifiers.txt`](common/modifiers/arena_modifiers.txt)
3. Add selection effect in [`common/scripted_effects/arena_effects.txt`](common/scripted_effects/arena_effects.txt)
4. Update class bonus logic in [`common/scripted_effects/arena_effects.txt`](common/scripted_effects/arena_effects.txt)
5. Add option in [`events/arena_entry_events.txt`](events/arena_entry_events.txt)
6. Add localization in [`localization/english/arena_l_english.yml`](localization/english/arena_l_english.yml)

### To Add New Events
1. Create event in appropriate file in `events/`
2. Add localization in [`localization/english/arena_l_english.yml`](localization/english/arena_l_english.yml)
3. Trigger from existing events or decisions

## Version Control

### Current Version: 1.0.0 (MVP Phase 1)

**Included Features**:
- ✅ 3 ranks (F, E, D)
- ✅ 3 classes (Warrior, Mage, Rogue)
- ✅ 3 brackets (F, E, D)
- ✅ County-based arenas
- ✅ Vanilla duel integration
- ✅ Balanced lethality system
- ✅ Fame and reputation
- ✅ AI participation

**Planned for Phase 2**:
- 🔜 9 ranks (F to SSS)
- 🔜 15+ classes
- 🔜 Multiple arena types
- 🔜 Rivalry system
- 🔜 Enhanced fame mechanics

## Compatibility Notes

### CK3 Version
- **Minimum**: 1.12.x
- **Tested**: 1.12.x - 1.16.x
- **Expected**: Compatible with future versions (uses stable systems)

### Other Mods
- **No vanilla file overwrites**: 100% compatible
- **Potential conflicts**: Mods that modify duel system
- **Safe with**: Most gameplay, graphics, and UI mods

### Save Compatibility
- **Can be added**: To existing saves
- **Can be removed**: From saves (arena features become inactive)
- **No corruption**: Safe to enable/disable

---

**Last Updated**: 2026-01-27  
**Mod Version**: 1.0.0  
**Manifest Version**: 1.0
