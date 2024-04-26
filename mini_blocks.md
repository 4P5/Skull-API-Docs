Mini Blocks are an optional library which make it easy to add shapes that look hand-textured.

They can be used as the icon for a Mode with `Mode:setMiniItemBlock`, or created in-world with `Skull:newMiniBlock`. For example:
```lua
function Mode:init(skull)
	skull:newMiniBlock("lodestone")
end
```
This creates an 8x8x8 mini block with the texture of a lodestone. The texture is sampled in a way that doesn't distort it while keeping the edges.

You can control the scale of mini blocks, up to one full block (2x2x2). Scales are in 1/8ths, so it's common to do the following:
```lua
function Mode:init(skull)
	skull:newMiniBlock("oak_planks", vec(8/8, 4/8, 8/8))
end
```
This creates a mini slab, with half the vertical size.