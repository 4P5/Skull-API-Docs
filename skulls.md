Skulls store data and contain methods to interact with the Skull API's features. The VS Code docs have comments for every method and field, so check them for a full reference of all the features.
## Textures
Never create textures per-Skull with the `textures` API. Figura has a **limit of 128 textures per avatar**, and creating a new texture for every Skull will eventually **error the entire avatar**.

Instead, use `Skull:newTexture` or `Skull:readTexture`. This takes a texture ID from a shared pool. The texture is yours to use for as long as the Skull exists. When the Skull is removed, the texture ID will be released back to the shared pool, for other Skulls to use.