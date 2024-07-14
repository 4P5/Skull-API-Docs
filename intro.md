Welcome to the documentation for my Skull API!

The Skull API makes it easy to create custom functionality with player heads. It handles complicated stuff like lifetimes, state, and cleanup, letting you focus on making cool stuff.

The two main things you'll be working with are **Skulls** and **Modes**.
## Skulls
A Skull is created for every one of your placed player heads. It contains information about its block (`Skull.is_wall_head`, `Skull.rot`, `Skull.pos`), and contains many methods for easily creating things (`Skull:newTexture`, `Skull:newPart`, `Skull:addPart`).

On their own, Skulls are relatively useful. You can do some cool stuff with them, but it gets hard to manage all the different Skulls at once. This is where Modes come in.

Skulls handle the life cycle of parts, textures, animations, etc., and will automatically clean these up when they exit. If you use the correct methods, you'll never have to worry about accidentally leaving ModelParts behind, or using up all the textures.

[Read more about Skulls here](skulls)
## Modes
Modes are primarily an event system, with many different events to help you manage Skulls. Skulls can have a single Mode, and Modes can have many Skulls.

Modes are decided when a Skull is first placed. The default behaviour is for each Mode to match the blocks below them against a list of blocks, then decide if they should apply for that block.

Events are the main way you'll interact with Skulls. For example, the `Mode:tick` event is run for every Skull controlled by a given Mode, every tick. `Mode:init` is run before a Skull ticks for the first time, and can be used to setup things that only need to run once.

There are also Mode-focused events, like `Mode:preTick`. If the Mode has at least one Skull, this event will run before ticking its Skulls. This lets you treat the Skulls as a list of positions or data, so you're not limited to per-Skull features.

[Read more about Modes here](modes)
## Using the Skull API
Typically, you'll use the Skull API by creating a Mode, adding some events, then performing stuff in those events. Take this Mode as an example:
```lua
local SkullAPI = require(...) --[[@as SkullAPI]]

local mode = SkullAPI.registry:newMode(({...})[2])
    :setName("Fountain")
    :setBlocks("stone_bricks")
    :setMiniItemBlock("chiseled_stone_bricks")

---@param skull Skull
function mode:init(skull)
    skull:newMiniBlock("chiseled_stone_bricks")
end

---@param skull Skull
function mode:tick(skull)
    local pos = skull.pos + vec(0.5, 0.5, 0.5)
    particles["rain"]:pos(pos):spawn()
end
```
We're doing a few things here:
- The first three lines can be ignored. We're just requiring the Skull API and creating a new mode with the file name as its ID.
- We set the name of the mode to Fountain. This will show up in the [action wheel](action_wheel).
- We set the valid blocks to `"stone_bricks"`, which matches any blocks with that string in their name (including mossy, cracked, stair/slab variants).
- We set the mode's mini item block to `"chiseled_stone_bricks"`. [mini blocks](/mini_blocks) are an optional library, but make it easier to set fancy custom icons.

Then, we register the init event.
- We take a single Skull as an argument. This event runs every time a Skull is added to this mode.
- We create a mini block. This is automatically positioned and rotated to be at the Skull.

Finally, we register the tick event.
- We take a Skull again. This runs every tick, for every Skull in this mode.
- We spawn a simple particle above the Skull. We offset by 0.5 to use the center of the block, instead of the floored coordinates.

And that's it! The Skull will automatically remove its mini block when broken.