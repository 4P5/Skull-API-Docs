## Explanation

Figura limits what scripts can do when you're not the host. APIs like `keybinds` only work when you're the one running the avatar.

However, there's a workaround. Using `avatar:store("keybinds", keybinds)`, you store *your own* keybinds API in your avatar, where my script can access it.
> NB: The above code is unsafe, as it allows *anyone* to register keybinds on your behalf.

## Workaround

Use this code to pass your keybinds API to my script. Only I will be able to access it and register keybinds for you.  **You can run the code with `/figura run`**.

The DOOM skull mode will use this to register WASD movement keys. You can reload your or my avatar at any time to remove the keybinds.

```lua
local vars = world.avatarVars()["584fb77d-5c02-468b-a5ba-4d62ce8eabe2"]
_ = vars and vars.doom(keybinds)
```

This ensures my avatar is loaded before attempting to access its variables.