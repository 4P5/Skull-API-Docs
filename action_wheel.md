The Skull API comes with an action wheel page you can add to your avatar's wheel. The page contains every Mode in the registry. Simply click an icon to get an item for that mode!

If you want to customize your mode with different variants, you can use the `Mode:getVariants` function. If this is defined, full page will be generated for the mode. Every time the page is opened, this function will be called to generate a list of variants.

Variants can be:
- A list of strings, which will be used for both the display name and the variant data.
- A table mapping strings (names) to strings (data)
- A table mapping strings (names) to functions that return strings (data). These functions will be called when their variant is clicked, and can be used to delay creation if said data is expensive to generate.

```lua
function Mode:getVariants()
	return { "variant1", "variant2", "variant3" }
end

function Mode:getVariants()
	return {
	    ["The First Variant"] = "variant1",
	    ["The Second Variant"] = "variant2",
	    ["The Third Variant"] = "variant3"
	}
end

function Mode:getVariants()
	return {
	    ["The First Variant"] = getVariant1,
	    ["The Second Variant"] = function() return veryLaggyFunction(123) end,
	    ["The Third Variant"] = function() return "variant3" end
	}
end
```
The data will be stored in the Skull's item/blockstate as a string, and will be used as the second parameter in the `Mode:init(skull, extra)` event.

You can use this to create variants of your Mode without creating a new Mode for each of them. For example, you might have a fountain that shoots out particles. You can have it default to a specific particle, but accept extra data to force it to use a specific particle.

Or your Mode might have variations randomly based on its coordinates. You can explicitly list the variations here to make it easier to debug or use specific ones.