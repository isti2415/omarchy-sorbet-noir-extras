# Sorbet Noir extras

Optional extras for the
[Sorbet Noir](https://github.com/isti2415/omarchy-sorbet-noir-theme) Omarchy
theme. Omarchy themes can't contain code, so the theme repository carries only
the palette, backgrounds and the files Omarchy reads, and everything that runs
lives here.

None of this is needed to use the theme. Install the theme first:

```bash
omarchy theme install https://github.com/isti2415/omarchy-sorbet-noir-theme
omarchy theme set sorbet-noir
```

Then clone this repository:

```bash
git clone https://github.com/isti2415/omarchy-sorbet-noir-extras.git
cd omarchy-sorbet-noir-extras
```

| Path | What it is |
|---|---|
| [`hyprland/glow.lua`](hyprland/glow.lua) | mint bloom on the focused window, mint-tinted groupbar |
| [`hyprland/looknfeel.lua`](hyprland/looknfeel.lua) | a complete look and feel: rounded, airy, dimmed, flowy |
| [`bar/Workspaces.qml`](bar/Workspaces.qml) | workspace numbers, each in its own pastel hue |
| [`tools/`](tools/) | the scripts that build the theme's backgrounds and unlock screen |

## Mint glow

The theme's border gradient lives in `colors.toml`, so it comes through
`omarchy theme install`. The soft mint shadow under the focused window and the
groupbar tint are Hyprland Lua, which a theme can't ship. Append them to your
own config:

```bash
cat hyprland/glow.lua >> ~/.config/hypr/looknfeel.lua
hyprctl reload
```

Tune it with the alpha in `rgba(a8f0dc20)` and with `range`.

## Look and feel: rounded, airy, flowy

`hyprland/looknfeel.lua` is a complete `~/.config/hypr/looknfeel.lua`:
rounding 10, gaps 4/6, border 2, dimmed inactive windows, blur tuned so popups
don't crush an all-black desktop into a void, and slower easing on three
custom bezier curves. It replaces your file, so back it up, and add the glow
after it if you want that too:

```bash
cp ~/.config/hypr/looknfeel.lua ~/.config/hypr/looknfeel.lua.bak
cp hyprland/looknfeel.lua ~/.config/hypr/looknfeel.lua
cat hyprland/glow.lua >> ~/.config/hypr/looknfeel.lua
hyprctl reload && hyprctl configerrors
```

For tighter or airier windows, change `gaps_in` and `gaps_out`.

## Pastel workspace indicators

Stock workspace numbers use a single bar colour. `bar/Workspaces.qml` gives
each workspace its own pastel: 1 mint, 2 sky, 3 lilac, 4 rose, 5 peach, and on
through the palette.

Clone the built-in widget first so the plugin id matches your username, then
replace just its QML:

```bash
omarchy plugin clone omarchy.workspaces
cp bar/Workspaces.qml ~/.config/omarchy/plugins/"$USER".workspaces/
omarchy restart shell
```

## Tools

These rebuild the images the theme ships. They need ImageMagick (`magick`);
the Python scripts have no other dependencies.

Regenerate the four backgrounds at any resolution:

```bash
python3 tools/generate-backgrounds.py 3840 2160 ~/.config/omarchy/themes/sorbet-noir/backgrounds
```

Rebuild the pastel Omarchy wordmark for the boot and unlock screen, then its
preview:

```bash
python3 tools/make-unlock.py /usr/share/omarchy/themes/catppuccin/unlock.png unlock.png
omarchy plymouth preview 000000 a8f0dc unlock.png preview-unlock.png
```

Download four community wallpapers from [Wallhaven](https://wallhaven.cc/)
that pair well with the theme: [4xz98z](https://wallhaven.cc/w/4xz98z),
[573kd1](https://wallhaven.cc/w/573kd1),
[k88m2m](https://wallhaven.cc/w/k88m2m) and
[ymxr2d](https://wallhaven.cc/w/ymxr2d). They're third-party uploads with
their own rights holders, so they aren't redistributed anywhere; the script
fetches them into the installed theme's `backgrounds/`:

```bash
./tools/fetch-extra-backgrounds.sh
```

## What this runs

- `hyprland/*.lua` is executed by Hyprland at every config load. It only sets
  options (decoration, gaps, blur, groupbar colours), layer rules, curves and
  animations; it starts no processes and reads no files.
- `bar/Workspaces.qml` runs inside omarchy-shell as your copy of the stock
  workspaces widget. The only command it runs is `hyprctl dispatch` to focus
  the workspace you click.
- `tools/generate-backgrounds.py` and `tools/make-unlock.py` call `magick`
  and write image files where you tell them.
- `tools/fetch-extra-backgrounds.sh` downloads four images from
  `w.wallhaven.cc` with `curl`.

Read each file before you use it. They're all short.

## License

MIT, see [LICENSE](LICENSE). It doesn't cover the Wallhaven wallpapers, which
belong to their authors.
