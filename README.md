# Surge SMP resource packs

The three resource packs the Surge SMP server sends on join. This repository is public for one
reason: a Minecraft client downloads a resource pack anonymously, with no token and no auth
header, so the URLs have to be reachable by a stranger. Nothing else lives here — the plugin is
private.

**They only work applied together**, and the order matters.

| # | Pack | Supplies |
| --- | --- | --- |
| 1 | `Shader2.zip` | `ult_effects` fonts (the parry overlay), `ritualvfx`, the `custom.clock` sound, particle shaders |
| 2 | `surgepack.zip` | HUD glyphs, the fullscreen title overlay, the custom sound set, text shaders, the four fragment shard items, the Lunar icon, star and moon |
| 3 | `spherpack.zip` | The raysphere models, the `raysphere_marker` textures, and the core shader that raytraces them |

Lowest priority first. Minecraft merges `sounds.json` and font providers across the whole stack,
so those combine — but models, textures and shaders do not. For those the highest pack that ships
a file wins outright, which is why `spherpack.zip` has to sit on top: `surgepack` ships raysphere
models but not the textures they reference, so with surgepack winning the core renders untextured.

## Hashes

The server sends each pack with its sha1 so clients can reuse a cached copy. These are the hashes
of the zips in this repository at the latest tag. From Lunar onward, surgepack is served straight from the
commit that added it rather than from a release asset, at
`https://raw.githubusercontent.com/seeedey/surge-packs/bb02a742953dd925ff9546eb85d214e900d4b36c/surgepack.zip`.
A commit url never changes underneath the sha1, the same guarantee a release gives:

```
Shader2.zip     55219f777e5a140546ee601b7bc9392a91816aaf
surgepack.zip   bbfc22aa83f7e4b3ab333c3f239bd1a8c823e637
spherpack.zip   8aab0d79816008859f40b1502cffa1fbc1abb503
```

**Re-hash after every pack edit.** A stale hash means either a re-download on every join or a load
failure, and the server is configured to kick on a load failure.

```powershell
certutil -hashfile surgepack.zip SHA1
```

## Updating a pack

Replace the zip, cut a new release, and update both the url and the sha1 in the server's
`config.yml`. The download url changes per release, so updating one without the other is the
usual way to break this.
