Modes and their extra data can be hardcoded into skull items and blocks by overwriting the **texture data**. The texture data uses the following format, **which must be converted to base 64**:
```lua
<mode id>[;extra data]
```
Here:
- `mode id` is a valid ID for a Mode.
- If you'd like to add extra variant data:
- `;` ends the Mode ID section.
- `extra data` (any remaining text) is passed to the mode's init function.

An example: 
```lua
particle_fountain;end_rod 1 0 1
```
Here:
- `particle_fountain` is used as the Mode ID
- `end_rod 1 0 1` is passed to the Mode's init event.

The Mode ID can be comprised of any character other than `;` (as this would indicate the end of the ID section). The extra data has no character restrictions.
## Usage
1. **Create the texture data string**: Concatenate your `mode_id` with a semicolon and your `extra_data`.
2. **Convert to Base64**: Convert this string to base 64.
3. **Store as an item**: Create an item, using the string as the texture value.
```json
"SkullOwner": {
    "Properties": {
        "textures": [
            {
                "Value": <base 64 string>
            }
        ]
    }
}
```

## 4P5's Heads
For some more examples (and ad-hoc documentation), here are the formats my avatar's heads accept.

**Images**: `images;<byte array>`
- `<byte array>` is a string of the raw bytes. You can get this by decoding a base 64 PNG.

**Jukebox**: `jukebox;<byte array>`
- Same as above, `<byte array>` is a string of the decoded bytes. Decode a base 64 OGG to get this.

**Mini Blocks**: `mini_blocks;<block id>`
- `<block id>` is a valid ID for a block. The block will be used as the texture source for the mini block.

**Lasers**: `tripwire;<hex colour>`
- `<hex colour>` is a hex code for an RGB colour. The laser will use this colour for its beam.