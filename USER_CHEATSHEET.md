# User cheat sheet

This is the user-facing cheat sheet for this `st` build.

## Modifier names

- `Alt` means `Mod1`.
- `TermMod` means `Alt+Shift` in this build.
- `Mouse wheel up` means button 4.
- `Mouse wheel down` means button 5.

## Launch examples

```sh
st
st -f "DejaVu Sans Mono:pixelsize=24"
st -g 100x30
st -T "terminal"
st -e htop
```

## Command-line options

| Option | Meaning |
|---|---|
| `-a` | Disable alternate screen. |
| `-c class` | Set X window class. |
| `-f font` | Set font for this launch. |
| `-g geometry` | Set X geometry, like `100x30+10+10`. |
| `-i` | Fix position from `-g`. |
| `-n name` | Set X instance name. |
| `-o file` | Write terminal I/O to file. Use `-` for stdout. |
| `-T title` | Set window title. |
| `-t title` | Set window title. |
| `-w windowid` | Embed in given X window. |
| `-l line` | Use tty/serial line instead of pty. |
| `-v` | Print version and exit. |
| `-e cmd ...` | Run command. Must be last option. |

## Included patches

Names follow common suckless patch names where possible.

| Patch / feature | User-visible effect |
|---|---|
| `clipboard` | Adds clipboard copy/paste shortcuts. |
| `scrollback` | Adds terminal scrollback buffer. |
| `scrollback-mouse` | Adds mouse wheel scrollback. |
| `externalpipe` / `externalpipe-eternal` | Pipes screen or scrollback to helper commands. |
| `openurlonclick`-style helpers | Opens or copies URLs through `st-urlhandler` and `dmenu`. |
| `copyurl` helper | Copies selected URL through `st-urlhandler`. |
| `copyout` helper | Copies output of recent command through `st-copyout`. |
| `alpha` | Adds background transparency. |
| `alpha-focus-highlight` | Changes opacity when focused/unfocused. |
| `xresources` | Loads font, colors, alpha, border, and other settings from Xresources. |
| `anysize` | Allows any window size. Pads unused space cleanly. |
| `font2` | Adds configured fallback fonts. Useful for emoji and Nerd Font symbols. |
| `boxdraw` | Draws box/block characters internally for clean alignment. |
| `ligatures` / `harfbuzz` | Enables font ligatures and shaping. |
| `xclearwin` | Clears dirty borders after color/window changes. |
| `osc` / `osc-color-reload` | Supports live color changes from OSC sequences. Useful for pywal. |
| `vim-browse`-style normal mode | Adds keyboard browsing over scrollback. This is a local port, not a clean upstream patch apply. |
| `newterm` is not included | No default keybind launches another terminal. |

## Core keybinds

| Key | Action |
|---|---|
| `Alt+c` | Copy selection to clipboard. |
| `Alt+v` | Paste from clipboard. |
| `Ctrl+Shift+c` | Copy selection to clipboard. |
| `Ctrl+Shift+v` | Paste from clipboard. |
| `Shift+Insert` | Paste from clipboard. |
| `Alt+Shift+c` | Copy selection to clipboard. |
| `Alt+Shift+v` | Paste from clipboard. |
| `Middle click` | Paste primary selection. |
| `Alt+Shift+NumLock` | Toggle numlock mode. |
| `Break` | Send serial break. |

## Scrollback

Patch: `scrollback`, `scrollback-mouse`, `scrollback-mouse-altscreen`.

| Key / mouse | Action |
|---|---|
| `Mouse wheel up` | Scroll back 1 line. |
| `Mouse wheel down` | Scroll forward 1 line. |
| `Alt+k` | Scroll back 1 line. |
| `Alt+j` | Scroll forward 1 line. |
| `Alt+Up` | Scroll back 1 line. |
| `Alt+Down` | Scroll forward 1 line. |
| `Alt+u` | Scroll back 1 page. |
| `Alt+d` | Scroll forward 1 page. |
| `Shift+PageUp` | Scroll back 1 page. |
| `Shift+PageDown` | Scroll forward 1 page. |
| `Alt+PageUp` | Scroll back 1 page. |
| `Alt+PageDown` | Scroll forward 1 page. |

Note: applications can capture mouse wheel in alternate screen. Hold `Shift` to force mouse escape behavior for apps.

## Vim-browse normal mode

Patch: local `vim-browse`-style port. It keeps Luke scrollback instead of replacing history code.

Enter with:

| Key | Action |
|---|---|
| `Alt+Escape` | Toggle normal browse mode. |

Inside normal browse mode:

| Key | Action |
|---|---|
| `h` or `Left` | Move cursor left. |
| `j` or `Down` | Move cursor down. Scrolls forward at bottom. |
| `k` or `Up` | Move cursor up. Scrolls back at top. |
| `l` or `Right` | Move cursor right. |
| `0` or `Home` | Move to line start. |
| `$` or `End` | Move to line end. |
| `Ctrl+d` or `PageDown` | Move down half/page. |
| `Ctrl+u` or `PageUp` | Move up half/page. |
| `gg` | Go to top of scrollback. |
| `G` | Go to live bottom. |
| `v` | Toggle visual selection. |
| `V` | Toggle visual line selection. |
| `y` | Yank selection. Without visual mode, yank current line. |
| `Esc`, `Ctrl+c`, `q`, `i`, or `Enter` | Exit normal browse mode. |

Not included yet: `/` search, `n/N`, text objects, counts, macros.

## Font size

| Key | Action |
|---|---|
| `Alt+Shift+PageUp` | Increase font size. |
| `Alt+Shift+PageDown` | Decrease font size. |
| `Alt+Shift+Up` | Increase font size. |
| `Alt+Shift+Down` | Decrease font size. |
| `Alt+Shift+K` | Increase font size. |
| `Alt+Shift+J` | Decrease font size. |
| `Alt+Shift+U` | Increase font size by 2. |
| `Alt+Shift+D` | Decrease font size by 2. |
| `Alt+Shift+Home` | Reset font size. |

## Transparency

Patch: `alpha`, plus focus alpha support.

| Key | Action |
|---|---|
| `Alt+a` | Increase opacity. Make less transparent. |
| `Alt+s` | Decrease opacity. Make more transparent. |

Config values:

```c
float alpha = 0.8;
float alphaOffset = 0.0;
float alphaUnfocus;
```

`alpha` ranges roughly from `0.0` to `1.0`.

## URL and output helpers

Patch: `externalpipe`, `externalpipe-eternal`, URL helpers, and copyout helper.

These use external scripts.

| Key | Action | Needs |
|---|---|---|
| `Alt+l` | Pick URL with `dmenu`, then open it. | `st-urlhandler`, `dmenu`, `xdg-open` |
| `Alt+y` | Pick URL with `dmenu`, then copy it. | `st-urlhandler`, `dmenu`, `xclip` |
| `Alt+o` | Pick recent command and copy output. | `st-copyout`, `dmenu`, `xclip` |

Install scripts with:

```sh
sudo make install
```

or keep `st-urlhandler` and `st-copyout` in your `PATH`.

## Print / I/O capture

These use the `-o` option.

| Key | Action |
|---|---|
| `Ctrl+PrintScreen` | Toggle printing to I/O file. |
| `Shift+PrintScreen` | Print full screen to I/O file. |
| `PrintScreen` | Print selection to I/O file. |

Example:

```sh
st -o session.log
```

## Fonts

Patch: `font2` and `ligatures` / Harfbuzz.

Main font lives in `config.def.h`:

```c
static char *font = "mono:pixelsize=12:antialias=true:autohint=true";
```

Fallback fonts live in `font2`:

```c
static char *font2[] = {
    "NotoColorEmoji:pixelsize=10:antialias=true:autohint=true"
};
```

Use a monospace font as main font. Use `font2` for emoji and symbols.

Test font names:

```sh
fc-match "DejaVu Sans Mono:pixelsize=24"
fc-list | grep -i droid
fc-scan /path/to/font.ttf | grep family
```

Test glyphs:

```sh
printf 'emoji: 🙂\n'
printf 'arch: \uf303\n'
```

## Xresources

Patch: `xresources`.

This build reads Xresources at startup.

Example `~/.Xresources`:

```text
st.font: DejaVu Sans Mono:pixelsize=24:antialias=true:autohint=true
st.fontalt0: Noto Color Emoji:pixelsize=20:antialias=true:autohint=true
st.alpha: 0.95
st.borderpx: 4
st.background: #282828
st.foreground: #ebdbb2
st.cursorColor: #add8e6
```

Load it:

```sh
xrdb ~/.Xresources
```

Start a new `st` after loading.

Supported Xresources names include:

- `font`
- `fontalt0`
- `color0` through `color15`
- `background`
- `foreground`
- `cursorColor`
- `termname`
- `shell`
- `minlatency`
- `maxlatency`
- `blinktimeout`
- `bellvolume`
- `tabspaces`
- `borderpx`
- `cwscale`
- `chscale`
- `alpha`
- `alphaOffset`

## Source config knobs

These knobs come from base `st` plus patches like `alpha`, `font2`, `boxdraw`, `xresources`, and `anysize`.

Common knobs in `config.def.h`:

| Name | Meaning |
|---|---|
| `font` | Main font. |
| `font2` | Fallback fonts. |
| `borderpx` | Inner border width. |
| `shell` | Fallback shell. |
| `cwscale` | Character width scale. |
| `chscale` | Character height scale. |
| `worddelimiters` | Characters used for word selection. |
| `allowaltscreen` | Allow alternate screen. |
| `allowwindowops` | Allow clipboard-setting escape sequences. |
| `minlatency` | Minimum draw latency. |
| `maxlatency` | Maximum draw latency. |
| `blinktimeout` | Cursor blink timeout. |
| `cursorthickness` | Bar/underline cursor thickness. |
| `boxdraw` | Draw box characters internally. |
| `boxdraw_bold` | Make bold box lines thicker. |
| `boxdraw_braille` | Draw braille internally. |
| `bellvolume` | Bell volume. `0` disables. |
| `termname` | TERM value. |
| `tabspaces` | Spaces per tab. |
| `alpha` | Background opacity. |
| `alphaOffset` | Alpha offset when focus changes. |
| `defaultfg` | Default foreground color index. |
| `defaultbg` | Default background color index. |
| `defaultcs` | Cursor color index. |
| `cursorshape` | Cursor shape. |
| `cols`, `rows` | Default terminal size. |

## Rebuild after config changes

Recommended flow:

```sh
rm -f config.h
make clean
make
```

Run local binary:

```sh
./st
```

Install:

```sh
sudo make install
```

## Notes

- `config.h` is generated from `config.def.h` when missing.
- Edit `config.def.h` for tracked defaults.
- Edit `config.h` only for local throwaway testing.
- Some desktop environments bind `Alt` shortcuts first. Change `MODKEY` if needed.
