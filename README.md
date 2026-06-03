# Pathology Mod — v12

A Minecraft Forge mod adding a virus / disease simulation system with corpse entities, loot, body bags, and fire destruction.

---

## What's New in v12

### Corpse Pose Change
Corpses now lie **flat on their back, face up toward the sky** (supine) instead of on their side. The renderer applies a -90° ZP rotation to achieve this.

### Fire Destruction
Right-click any `CorpseEntity` with **Flint & Steel** or a **Fire Charge** to ignite and destroy it. The corpse will:
- Display an orange fire tint while burning.
- Emit large smoke particles.
- Fully despawn after ~3 seconds, removing all record and loot.

This is the only way to permanently eliminate a corpse before its 30-minute natural decay.

### Decay Disease – Guaranteed Zombie Spawn
All zombie variants (zombie, zombie_villager, husk, drowned, zombified_piglin) **always spawn pre-infected** with `decay_disease`. This guarantees every zombie produces a corpse entity on death.

### Corpse Airborne Infection
Players who stand too close to a corpse will gradually accumulate **exposure ticks**. After enough exposure the player is infected with `decay_disease` and `airbornehuskflu`.

| Parameter | Value |
|---|---|
| Detection radius | 5 blocks |
| Base infection chance | 10% per check |
| Max infection chance | 30% per check (after 60 s continuous exposure) |
| Check interval | 40 ticks (2 s) |

The chance scales linearly from 10% to 30% as the player accumulates up to 1,200 ticks (60 seconds) of exposure. Stepping away decays the counter.

### Corpse Inventory (Loot Chest)
Every corpse spawns with a randomised **9-slot inventory** accessible by right-clicking the corpse entity (opens a 3×3 chest-style GUI).

Loot is configured in `pathologymod-corpse-loot.toml`:
```toml
[corpseLoot]
minSlots = 2
maxSlots = 5

# Flat pool: [itemId, weight, minCount, maxCount, ...]
# Supports any modded item via the Forge item registry
pool = [
    "minecraft:rotten_flesh", "60", "1", "3",
    "minecraft:bone",         "50", "1", "4",
    "mod:shotgun",             "5", "1", "1",
    ...
]
```

Higher `weight` = more common. The `itemId` is a Forge registry key (namespace:path) so any loaded mod's items work automatically.

### Body Bag
A new **Body Bag** item (`pathologymod:body_bag`) appears in the Pathology creative tab. It allows players to:
1. **Pick up** a corpse: right-click near a corpse to store it. The CorpseEntity despawns.
2. **Place it** somewhere else: right-click again (not near a corpse) to spawn it at your feet. All loot, decay stage, and data are preserved.

The bag holds exactly one corpse at a time. The item tooltip shows what's stored inside.

---

## Config Files

| File | Purpose |
|---|---|
| `pathologymod-viruses.toml` | All virus parameters (airborne radius, stages, etc.) |
| `pathologymod-protection.toml` | Armour/gear infection resistance |
| `pathologymod-corpse-loot.toml` | Corpse loot pool and slot counts |

---

## Corpse Entity Summary

| Feature | Detail |
|---|---|
| Pose | Supine (on back, face up) |
| Decay stages | 0 Fresh → 1 Rotting (5 min) → 2 Skeletal (15 min) → despawn (30 min) |
| Infection | Nearby players accumulate exposure; 10–30% chance of decay_disease + airbornehuskflu |
| Loot | 9-slot inventory, configurable weighted pool |
| Fire | Flint & Steel / Fire Charge ignites; despawns in ~3 s |
| Body Bag | Pick up + relocate; preserves all data |
| Merge | Corpses within 2 blocks merge into a pile (body count increments) |

---

## Virus Overview

| Virus | Spread | Source |
|---|---|---|
| `decay_disease` | Corpse proximity (airborne) | All zombie deaths; corpse exposure |
| `airbornehuskflu` | Airborne (zombie breath + corpse) | All zombie spawns |
| `bloodbornehuskvirus` | Bloodborne (melee hit / death cloud) | All zombie spawns |
| `influenza` | Airborne | Config |


NOTES-
Made with the assistance of AI. I've made a general use system for pathology based events. Right now I've only specificed on zombies esc infections but it has the ground work to make custom viruses. Lots of customization. 
I dont know its late. 
