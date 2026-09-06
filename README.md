# JS13K 2026 Rainbow Platformer

```powershell
$env:Path = "C:\Users\tyler\.cache\codex-runtimes\codex-primary-runtime\dependencies\node\bin;$env:Path"
& ".\node_modules\.bin\terser.cmd" keys.js utility.js tile.js song.js sound.js level.js entity.js particles.js hero.js camera.js game.js --compress "passes=3,top_retain=startGame" --mangle "reserved=[startGame]" --toplevel --output build/game.min.js
& ".\node_modules\.bin\roadroller.cmd" -O2 build/game.min.js -o build/game.js
```

``` powershell
$env:Path = "C:\Users\tyler\.cache\codex-runtimes\codex-primary-runtime\dependencies\node\bin;$env:Path"

& ".\node_modules\.bin\terser.cmd" keys.js utility.js tile.js song.js sound.js level.js entity.js particles.js hero.js camera.js game.js --compress "passes=3,top_retain=startGame" --mangle "reserved=[startGame]" --mangle-props "reserved=[red,orange,yellow,green,blue,indigo,violet]" --toplevel --output build/game.min.js
& ".\node_modules\.bin\roadroller.cmd" -O2 build/game.min.js -o build/game.js
```

A fast, momentum-focused browser platformer about restoring the colours of the rainbow. Collect each level's crystals to activate its colour mechanics, then reach the door.

The game is written in plain JavaScript and renders with a fixed `800 × 480` logical coordinate system at the browser's current display resolution. It includes keyboard and touch controls, responsive scaling, wall jumps, a double jump, moving platforms, hazards, generated music and Web Audio effects, camera effects, a run timer and death counter, and seven hand-authored levels.

## Playing the game

Collect the crystals in the order shown by the HUD. The exit door opens after every colour listed for the current level has been collected.

Falling onto spikes restarts the level. Completed levels advance automatically after a short delay.

### Controls

| Action | Keyboard | Touch |
| --- | --- | --- |
| Move | `←` / `→` or `A` / `D` | Direction pad |
| Jump | `Space`, `↑`, or `W` | **JUMP** button |
| Double jump | Press jump again while airborne | Press **JUMP** again |
| Wall jump | Press jump while sliding against a wall | Press **JUMP** while against a wall |

## Colour mechanics

| Colour | Crystal | Map block | Behavior |
| --- | --- | --- | --- |
| Red | `C` | `r` | Red blocks begin ghosted and non-solid, then become solid. |
| Orange | `O` | `o` / `M` | Orange blocks become solid and orange moving platforms begin moving. |
| Yellow | `Y` | `y` | Yellow blocks begin solid, then disappear and become non-solid—the inverse of red. |
| Green | `G` | `g` | Green launch pads become solid. Landing from above produces an automatic `850 px/s` super-bounce and refreshes the double jump. |
| Blue | `B` | `u`, `v`, `<`, `>` | Linked portals activate. Entering one preserves speed and redirects it through its partner. |
| Indigo | `I` | `i` | Indigo blocks become solid. Stepping on one warns for `0.3s`, hides it for `1.5s`, then safely restores it. |
| Violet | `V` | — | The unstable final crystal starts a five-second countdown. Reach the door before it creates a large rainbow explosion. |

The player, trail, particles, HUD, and exit door gain each restored colour as the level progresses.

### Level properties

#### `colours`

The ordered list of crystals required to open the door. It also controls the HUD's crystal prompts.

```js
colors: ["red", "orange", "yellow", "green", "blue", "indigo", "violet"]
```

Every listed colour must have its corresponding crystal in `rows`; otherwise the door can never open. Colours may be omitted when a level is intended to focus on a smaller set of mechanics.
