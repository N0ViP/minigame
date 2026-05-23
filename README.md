# minigame

`cub3D` is a small raycasting maze renderer written in C using MiniLibX (X11). It loads a `.cub` configuration file (textures, floor/ceiling colors, and a map grid) and renders a textured 3D view with a minimap, doors, and collectible sprites.

## Features

- Parses `.cub` files with `NO/SO/WE/EA` wall textures and `F/C` RGB colors
- Validates maps (single player spawn, allowed characters, closed-by-walls flood fill)
- Textured raycasting renderer with z-buffered sprite rendering
- Door tiles that can be toggled open/closed in-game
- Collectible `C` sprites (removed when the player steps on them)
- Circular minimap overlay with player marker, FOV lines, and compass labels
- Keyboard + mouse controls, with a capped frame loop (`FRAME_CAP` = 120)

## Tech Stack

- C (compiled with `cc`, `-Wall -Wextra -Werror`)
- MiniLibX for windowing and drawing ([minilibx-linux/](minilibx-linux/))
- X11 libraries (`-lX11 -lXext`) and math library (`-lm`)
- Project-local utilities (`stdfcts/`, `utils/`) and allocation tracking (`memtrack/`)

## Build / Installation

Build the `cub3D` binary:

```bash
make
```

Other Make targets:

```bash
make clean
make fclean
make re
```

## Usage

Run with a single `.cub` file:

```bash
./cub3D maps/map0.cub
```

### Controls

- `W/A/S/D`: move
- `Left/Right Arrow`: rotate
- Mouse movement: rotate
- `e`: toggle the door tile in front of the player
- `Esc`: exit

### `.cub` format (as implemented)

Required directives before the map:

- `NO <path>` / `SO <path>` / `WE <path>` / `EA <path>`: wall textures (loaded via `mlx_xpm_file_to_image`)
- `F <r>,<g>,<b>`: floor color (0–255 each)
- `C <r>,<g>,<b>`: ceiling color (0–255 each)

Map characters accepted by the parser:

- `1`: wall
- `0`: empty space
- `N/S/E/W`: player spawn + initial facing (exactly one required)
- `D`: closed door (blocks movement and raycasts)
- `d`: open door
- `C`: sprite/coin tile (rendered as a sprite and collected on contact)
