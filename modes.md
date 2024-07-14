## Data
Often when using Skulls, you'll want to store some data that persists between events. You can do this with a special table included with every Skull:
```lua
function Mode:init(skull)
	skull.data.offset = math.random() * 100
	skull.data.part = skull:newPart()
end

function Mode:render(skull, delta)
	local offset = math.sin(world.getTime(delta) + skull.data.offset)
	local pos = skull.render_pos + offset
	skull.data.part:pos(pos)
end
```
`skull.data` is a regular table, but you're required to use this instead of storing data directly in the Skull object. This is because Skulls can change modes throughout their lives, and arbitrary data can't be cleaned up properly. `Skull.data` is wiped whenever the Skull changes mode.
# Events
Modes work with the concept of *events*. These are functions that are called at specific times. All events are optional; you should only define events you actually use. See the **VS Code docs** for a list of all events.

**Init** events are run once, typically before most other events.
- `Mode:init(skull)` runs once for every Skull added to a Mode.
- `Mode:globalInit()` runs every time the Mode goes from *0 Skulls* to *1 Skull*. This event can run multiple times. You can use this if you want to do stuff independent of any one Skull.
- `Mode:globalSetup()` runs once, when the Mode goes from *0 Skulls* to *1 Skull*. It will only run again when the avatar is reloaded. You can use this to delay laggy calculations to when the data is actually needed.
- `Mode:debugInit()` runs when the Mode is placed into Debug Mode. You can optionally expose some extra data here with the [debugger](debugger).

**Tick** events run once per world tick. They run in the following order:
- `Mode:preTick(skulls)` runs first, with a list of all Skulls tracked by the Mode. This will only run if the Mode has at least once Skull.
- `Mode:tick(skull)` runs next, for every Skull tracked by the mode.
- `Mode:postTick(skulls)` runs last, after all the Skulls have been ticked. This list may be different to the one in `preTick`.

**Render** events are similar to the above, running once per world render. They run in the following order:
- `Mode:preRender(skulls, delta)` is first, with a list of all Skulls.
- `Mode:render(skull, delta)` is called for every Skull in the Mode.
- `Mode:postRender(skulls, delta)` is last, with a list of all Skulls. The list is guaranteed to be the same as in `preRender`.

**Exit** events are finalizers, letting you clean up stuff. Note: Skulls will automatically clean up everything created through `Skull:newXxxx` or `Skull:addXxxx` methods.
- `Mode:exit(skull)` runs when a Skull leaves the mode, either by changing its mode to a new mode, or by being removed.
- `Mode:globalExit()` runs when the total number of Skulls in a mode drops to 0. This can run multiple times. `globalInit` and `globalExit` will always alternate—one will never be called twice in a row.
- `Mode:debugExit()` runs when the Skull exits Debug Mode. You can clean up optional debug visualizations here.

There are also some miscellaneous events:
- `Mode:punch(skull, puncher)` runs when the Skull's block is punched. A punch is counted as a player swinging their arm while looking at the Skull. It is conventional for Modes to use this as a form of "redstone", by calling the event on other Skulls. For example, you might have a tripwire which punches the Skull behind it with the entity that trips it.
- `Mode:itemRender(itemstack, extra, context)` runs when the Mode renders in item form. This is very close to the `SKULL_RENDER` event.
- `Mode:heldRender(entity, itemstack, left_hand)` runs when the Mode is rendered in the hand of an entity.