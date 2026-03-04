# cub3D 🎮

A 3D first-person maze game built in C using raycasting, inspired by the legendary **Wolfenstein 3D**. Explore textured dungeon environments rendered entirely in software using the DDA (Digital Differential Analysis) algorithm.

---

## 📸 Preview

The engine renders a 3D perspective view of a 2D map, complete with textured walls, colored floors and ceilings, and a minimap overlay.

---

## ✨ Features

- **Raycasting engine** — Real-time 3D rendering using the DDA algorithm
- **Textured walls** — Each cardinal direction (North, South, East, West) has its own XPM texture
- **Configurable colors** — Floor and ceiling colors defined per map in RGB
- **Smooth movement** — Frame-rate-independent movement and rotation
- **Collision detection** — Prevents walking through walls
- **Minimap** — Overhead view rendered in the corner of the screen
- **Custom map format** — Define your own levels with `.cub` files

---

## 🛠️ Dependencies

| Library | Purpose |
|---------|---------|
| **minilibx-linux** | Window management and pixel rendering (X11) |
| **libft** | Custom C standard library |
| **libftprintf** | Custom `ft_printf` implementation |
| **X11 / Xext** | Linux graphics system |
| **libm** | Math (trigonometry for ray calculations) |
| **libz** | Compression |

> **Note:** This project is designed for **Linux** with X11. macOS builds would require the macOS variant of minilibx.

---

## 🚀 Building

```bash
# Clone the repository
git clone <repo-url>
cd cub3d

# Build everything
make

# Clean object files
make clean

# Full clean (objects + binary)
make fclean

# Rebuild from scratch
make re
```

---

## 🎮 Running

```bash
./cub3d <path-to-map.cub>
```

**Example:**
```bash
./cub3d maps/map.cub
```

---

## 🕹️ Controls

| Key | Action |
|-----|--------|
| **W** | Move forward |
| **S** | Move backward |
| **A** | Strafe left |
| **D** | Strafe right |
| **← Arrow** | Rotate view left |
| **→ Arrow** | Rotate view right |
| **ESC** | Quit the game |

---

## 🗺️ Map Format (`.cub`)

Map files are plain text with two sections: **configuration** and **map grid**.

### Configuration

```
NO ./textures/wall1.xpm   # North wall texture (XPM)
SO ./textures/wall2.xpm   # South wall texture (XPM)
WE ./textures/wall3.xpm   # West wall texture (XPM)
EA ./textures/wall4.xpm   # East wall texture (XPM)
F  135,62,35              # Floor color (R,G,B)
C  3,70,123               # Ceiling color (R,G,B)
```

### Map Grid

After the configuration block, the map is defined as a 2D grid:

| Character | Meaning |
|-----------|---------|
| `0` | Empty / walkable floor |
| `1` | Wall |
| `N` | Player start, facing **North** |
| `S` | Player start, facing **South** |
| `E` | Player start, facing **East** |
| `W` | Player start, facing **West** |

**Rules:**
- The map must be **fully enclosed** by walls (`1`).
- There must be **exactly one** player start position.
- Only the characters `0`, `1`, `N`, `S`, `E`, `W`, and spaces are valid.
- Configuration identifiers (`NO`, `SO`, `WE`, `EA`, `F`, `C`) must all appear before the map grid.

### Example Map

```
NO ./textures/wall1.xpm
SO ./textures/wall2.xpm
WE ./textures/wall3.xpm
EA ./textures/wall4.xpm
F 135,62,35
C 3,70,123

        1111111111111111111111111
        1000000000110000000000001
        1011000001110000000000001
        1001000000000000000000001
111111111011000001110000000000001
100000000011000001110111110111111
11110111111111011100000010001
11110111111111011101010010001
11000000110101011100000010001
10000000000000000000000010001
10000000000000001101010010001
11000001110101011111011110N011
11110111 1110101 101111010001
11111111 1111111 111111111111
```

---

## 📁 Project Structure

```
cub3d/
├── sources/
│   ├── cub3d.c          # Entry point
│   ├── parsing.c        # Map file parsing and validation
│   ├── init.c           # Game and player initialization
│   ├── raycasting1.c    # DDA raycasting algorithm
│   ├── raycasting2.c    # Wall rendering and texture projection
│   ├── movement1.c      # WASD movement with collision detection
│   ├── movement2.c      # Arrow-key rotation
│   ├── textures1.c      # Texture loading
│   ├── textures2.c      # Texture rendering helpers
│   ├── minimap.c        # Minimap overlay rendering
│   ├── check_map.c      # Map validation (structure/bounds)
│   ├── check_map2.c     # Map character validation
│   ├── mlx_fts1.c       # MLX window/event wrappers
│   ├── mlx_fts2.c       # MLX image helpers
│   ├── inputs.c         # RGB/color input parsing
│   ├── map_maker.c      # Map grid construction
│   ├── clean_exit.c     # Memory cleanup and exit
│   ├── utils.c          # General utility functions
│   ├── utils2.c         # Additional utilities
│   ├── cub3d.h          # Main game structures and prototypes
│   └── parsing.h        # Parsing structures and prototypes
├── maps/
│   └── map.cub          # Example map
├── textures/
│   ├── wall1.xpm        # North wall texture
│   ├── wall2.xpm        # South wall texture
│   ├── wall3.xpm        # West wall texture
│   ├── wall4.xpm        # East wall texture
│   └── minimap.xpm      # Minimap tile texture
├── libft/               # Custom C library
├── libftprintf/         # Custom printf
├── minilibx-linux/      # Graphics library
└── Makefile
```

---

## 🔧 How It Works

1. **Parsing** — The `.cub` file is read and validated. Textures are loaded and the map grid is stored.
2. **Initialization** — The MLX window is created (800×600), textures are converted to pixel arrays, and the player is placed on the map.
3. **Game Loop** — On every frame, a ray is cast for each vertical column of the screen. The DDA algorithm determines which wall was hit and at what distance.
4. **Rendering** — Wall height is computed from the perpendicular ray distance. The correct texture column is sampled and drawn. Floor and ceiling are filled with flat colors.
5. **Input** — Keyboard hooks update player position/direction each frame, with collision detection preventing wall clipping.

---

## 📜 License

This project was developed as part of the **42 School** curriculum.
