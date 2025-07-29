# NeoSpeedZero Difficulty Settings Explanation  

## Overview  
The "Difficulty" settings in NeoSpeedZero adjust the types and attributes of items players start with (fireworks, elytra, and their durability properties), offering diverse initial conditions to shape gameplay. Different difficulties correspond to unique combinations of starting resources, directly influencing exploration strategies and player experience. Below are the detailed difficulty settings:  

---

## Difficulty List  

| Difficulty ID                          | Description                                                                                     |
|----------------------------------------|-------------------------------------------------------------------------------------------------|
| `neospeedzero:empty`                   | [Start From Zero] Begins with no initial items. Players must rely entirely on naturally generated resources in the game to explore. |
| `neospeedzero:firework`                | [Fireworks Only] Starts with only one set of regular fireworks (consumable), no elytra. Suitable for basic exploration relying on fireworks for propulsion. |
| `neospeedzero:inf_firework`            | [Infinite Fireworks Only] Starts with one set of infinite fireworks (non-consumable), no elytra. Ideal for scenarios requiring sustained progression without flight. |
| `neospeedzero:elytra`                  | [Elytra Travel] Starts with one elytra with limited durability (requires repairing via accumulating 3+ durability or the Mending enchantment). No fireworks. Tests elytra maintenance and flight strategy. |
| `neospeedzero:firework_elytra`         | [Basic Travel] Starts with one elytra with limited durability and one set of regular fireworks (consumable). Balances flight and resource consumption. |
| `neospeedzero:inf_firework_elytra`     | [Advanced Travel] Starts with one elytra with limited durability and one set of infinite fireworks (non-consumable). Suited for long-term flight exploration. |
| `neospeedzero:inf_elytra`              | [Unbreakable Wings] Starts with one elytra with infinite durability (no maintenance required), no fireworks. Perfect for players focused on flight exploration. |
| `neospeedzero:firework_inf_elytra`     | [Infinite Adventure] Starts with one elytra with infinite durability and one set of regular fireworks (consumable). Ideal for long-term flight with managed fireworks usage. |
| `neospeedzero:inf_firework_inf_elytra` | [Classic] Starts with one elytra with infinite durability and infinite fireworks. The classic mode featured in user videos, perfect for unrestricted exploration. |

---

## Item Properties  
- **Fireworks**:  
  - Regular fireworks (consumable): Each use reduces the quantity by one set.  
  - Infinite fireworks (non-consumable): Can be used infinitely for propulsion; note that flight direction control is required.  

- **Elytra**:  
  - Elytra with limited durability: Durability depletes during flight. Repairs require either accumulating 3+ durability through flight or using the Mending enchantment (found on gear).  
  - Elytra with infinite durability: No durability loss; can be used permanently without maintenance.
