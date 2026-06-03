# ShaderInterop Documentation

This will explain how Shader/Mod/Modpack Developers can interact with this mod to add easier Mod~Shader cross-compatibility.

Shaderpacks will use IRIS_TAG_SUPPORT to add the block/item/entity tags provided by this mod to their block/item/entity properties files. Mods/Modpacks will use easy to write yaml/yml files to assign their blocks/items/entities to these tags.

This documentation will show examples for each tag for both sides. Some tags may already exist in vanilla or modded environments. This is not a showcase of new tags, but a proof of concept what could be possible with a shader-focused tag system instead of a gameplay-focused one.

<hr>

# General

## Parent/Child tags

Most tags will be in a parent/child relationship `foliage/leaves` where parent tags will always include every entry from their childs.

So the tag `foliage` will include all block IDs from these childs:
- `foliage/leaves`
- `foliage/grass`
- `foliage/saplings`
- ...

This could be useful if a shader decides to base light color on texture color, in which case they can use the combined tag `emissive/light:color` to get all colored lights.

## Filename (YAML)

The filename has to contain the tag category and modid/namespace.

`block.minecraft.yaml` will define the modid/namespace to `minecraft` and the tag category to `block`.

For easier writing the modid/namespace in the filename will be used as a prefix for every ID inside the YAML file.

```
foliage:
  leaves:
    - oak_leaves
```

In this example `oak_leaves` will be added to the `block` tag `foliage/leaves` and changed to `minecraft:oak_leaves` using the modid/namespace in the filename when added to the tag so it properly references the mod that ID came from.

<hr>

# Available Tags (subject to change - not a complete list)

## Foliage

### Leaves

Shader:

`%shaderinterop:foliage/leaves`

Yaml:

```
foliage:
  leaves:
    - oak_leaves
    - birch_leaves
    - ...
```

### Grass/Short

Shader:

`%shaderinterop:foliage/grass/short`

Yaml:

```
foliage:
  grass:
    short:
      - short_grass
      - fern
      - ...
```

### Grass/Tall

Shader:

`%shaderinterop:foliage/grass/tall:half=upper`

`%shaderinterop:foliage/grass/tall:half=lower`

Yaml:

```
foliage:
  grass:
    tall:
      - tall_grass
      - large_fern
```

## Ground

### Grass

Shader:

`%shaderinterop:ground/grass:snowy=true`

`%shaderinterop:ground/grass:snowy=false`

Yaml:

```
ground:
  grass:
    - grass_block
```

### Path

Shader:

`%shaderinterop:ground/path`

Yaml:

```
ground:
  path:
    - dirt_path
```

## Translucent

### Glass

Shader:

`%shaderinterop:translucent/glass`

Yaml:

```
translucent:
  glass:
    - glass
```

### Ice

Shader:

`%shaderinterop:translucent/ice`

Yaml:

```
translucent:
  ice:
    - ice
```

## Emissive

### Light

Shader:

`%shaderinterop:emissive/light`

Yaml:

```
emissive:
  light:
    - glowstone
```

### Glowing

Shader:

`%shaderinterop:emissive/glowing`

Yaml:

```
emissive:
  glowing:
    - magma_block
```

## Other

### Color

Shader:

`%shaderinterop:color/yellow`

Yaml:

```
color:
  yellow:
    - yellow_wool
```

### Shape

Shader:

`%shaderinterop:shape/stairs`

`%shaderinterop:shape/door`

`%shaderinterop:shape/fence`

Yaml:

```
shape:
  stairs:
    - oak_stairs
  door:
    - oak_door
  fence:
    - oak_fence
```

# Advanced/Combining Tags

Combining tags works by adding IDs to multiple tags and then chaining them together.

Shaders will have to chain the tags together. Mods/Modpacks will have to add block/item/entity IDs to all applicable tag mappings.

## Colored glass

Shader:

`%shaderinterop:translucent/glass:color/red`

Yaml:

```
translucent:
  glass:
    - red_stained_glass

color:
  red:
    - red_stained_glass
```

## Colored light with specific light level

Shader:

`%shaderinterop:emissive/light:color/green:level/9`

Yaml:

```
emissive:
  light:
    - ochre_froglight

color:
  green:
    - ochre_froglight

level:
  9:
    - ochre_froglight
```

## Combining tags with blockstates (shader only)

Tags and Blockstates can be chained together in the same way. Let's take candles as an example.

`%shaderinterop:emissive/candle:color/blue:candles=3:lit=true`