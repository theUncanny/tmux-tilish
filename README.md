# Tmux Tilish

This is a plugin that makes [`tmux`][6] behave more like a typical
[dynamic window manager][7]. It is heavily inspired by [`i3wm`][8], and 
most keybindings are taken [directly from there][1]. However, I have made 
some adjustments to make these keybindings more consistent with `vim`: 
using <kbd>h</kbd><kbd>j</kbd><kbd>k</kbd><kbd>l</kbd> instead of
<kbd>j</kbd><kbd>k</kbd><kbd>l</kbd><kbd>;</kbd> for directions, and 
using `vim`'s definitions of "split" and "vsplit". There is also an 
"easy mode" available for non-`vim` users, which uses arrow keys 
instead of <kbd>h</kbd><kbd>j</kbd><kbd>k</kbd><kbd>l</kbd>.

The plugin has been verified to work on `tmux` v1.9, v2.6, v2.7, v2.9, and v3.0. 
Some features are only available on newer versions of `tmux` (currently v2.7+), 
but I hope to provide at least basic support for most `tmux` versions in active use.
If you encounter any problems, please file an issue and I'll try to look into it.

[1]: https://i3wm.org/docs/refcard.html
[6]: https://github.com/tmux/tmux/wiki/Getting-Started
[7]: https://en.wikipedia.org/wiki/Dynamic_window_manager
[8]: https://i3wm.org/docs/

## Why?

Okay, so who is this plugin for anyway? You may be interested in this if:

- You're using or interested in using `tmux`, but find the default keybindings
  a bit clunky. This lets you try out an alternative keybinding paradigm, 
  which uses a modifier key (<kbd>Alt</kbd>) instead of a prefix key 
  (<kbd>Ctrl</kbd> + <kbd>b</kbd>). The plugin also makes it easier to do
  automatic tiling via `tmux` layouts, as opposed to splitting panes manually.
- You use `i3wm`, but also do remote work over `ssh` + `tmux`. This lets 
  you use similar keybindings in both contexts.
- You also use other platforms like Gnome, Mac, or WSL. You want to take 
  your `i3wm` muscle memory with you via `tmux`.
- You're not really using `i3wm` anymore, but you did like how it handled
  terminals and workspaces. You'd like to keep working that way in terminals,
  without using `i3wm` or `sway` for your whole desktop.
- You use a window manager that is similar to `i3wm`, e.g. [`dwm`][9],
  and want to have that workflow in `tmux` too.  
  
[9]: https://dwm.suckless.org/tutorial/

## Quickstart

The easiest way to install this plugin is via the [Tmux Plugin Manager][2].
Just add the following to `~/.tmux.conf`, then press <kbd>Ctrl</kbd> + <kbd>b</kbd>
followed by <kbd>Shift</kbd> + <kbd>i</kbd> to install it (assuming default prefix key):

	set -g @plugin 'jabirali/tmux-tilish'

For `tmux` v2.7+, you can customize which layout is used as default for new workspaces.
To do so, add this to `~/.tmux.conf`:

	set -g @tilish-default 'main-vertical'

Just replace `main-vertical` with one of the layouts from the `tmux` `man` page:

| Description       | Name              |
| ----------------- | ----------------- |
| split then vsplit | `main-horizontal` |
| only split        | `even-vertical`   |
| vsplit then split | `main-vertical`   |
| only vsplit       | `even-horizontal` |
| fully tiled       | `tiled`           |
                                         
The words "split" and "vsplit" refer to the layouts you get in `vim` when
running `:split` and `:vsplit`, respectively. (Unfortunately, what is called 
a "vertical" and "horizontal" split varies between programs.)
If you do not set this option, `tilish` will not autoselect any layout; you
can still choose layouts manually using the keybindings listed below.

If you use `vim` or `nvim` as your editor, see 
[how to integrate it with `tilish`](#integration-with-vim-tmux-navigator).
If you use `kak` or `emacs`, you may want to activate [Prefix mode](#prefix-mode).
If you do not use `vim` or `kak`, you may wish to activate [Easy mode](#easy-mode).

It is also recommended that you add the following to the top of your `.tmux.conf`:

	set -s escape-time 0
	set -g base-index 1

This plugin should work fine without these settings. However, without the first one,
you may accidentally trigger e.g. the <kbd>Alt</kbd> + <kbd>h</kbd> binding by pressing
<kbd>Esc</kbd> + <kbd>h</kbd>, something that can happen often if you use `vim` in `tmux`. 
Note that this setting only has to be set manually if you don't use [tmux-sensible][4].
The second one makes the window numbers go from 1-10 instead of 0-9, which IMO
makes more sense on a keyboard where the number row starts at 1. This behavior
is also more similar to how `i3wm` numbers its workspaces. However, the plugin
will check this setting explicitly when mapping keys, and works fine without it.

[2]: https://github.com/tmux-plugins/tpm
[4]: https://github.com/tmux-plugins/tmux-sensible

## Keybindings

Finally, here is a list of the actual keybindings. Most are [taken from `i3wm`][1].
Below, a "workspace" is what `tmux` would call a "window" and `vim` would call a "tab",
while a "pane" is what `i3wm` would call a "window" and `vim` would call a "split".

| Keybinding | Description |
| ---------- | ----------- |
| <kbd>Alt</kbd> + <kbd>0</kbd>-<kbd>9</kbd> | Switch to workspace number 0-9 |
| <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>0</kbd>-<kbd>9</kbd> | Move pane to workspace 0-9 |
| <kbd>Alt</kbd> + <kbd>h</kbd><kbd>j</kbd><kbd>k</kbd><kbd>l</kbd> | Move focus left/down/up/right |
| <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>h</kbd><kbd>j</kbd><kbd>k</kbd><kbd>l</kbd> | Move pane left/down/up/right |
| <kbd>Alt</kbd> + <kbd>Enter</kbd> | Create a new pane at "the end" of the current layout |
| <kbd>Alt</kbd> + <kbd>s</kbd> | Switch to layout: split then vsplit |
| <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>s</kbd> | Switch to layout: only split |
| <kbd>Alt</kbd> + <kbd>v</kbd> | Switch to layout: vsplit then split |
| <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>v</kbd> | Switch to layout: only vsplit |
| <kbd>Alt</kbd> + <kbd>t</kbd> | Switch to layout: fully tiled |
| <kbd>Alt</kbd> + <kbd>f</kbd> | Switch to layout: fullscreen (zoom) |
| <kbd>Alt</kbd> + <kbd>r</kbd> | Refresh current layout |
| <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>q</kbd> | Quit (close) pane |
| <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>e</kbd> | Exit (detach) `tmux` |
| <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>c</kbd> | Reload config |

The <kbd>Alt</kbd> + <kbd>0</kbd> and <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>0</kbd> 
bindings are "smart": depending on `base-index`, they either act on workspace 0 or 10.

The keybindings that move panes between workspaces assume a US keyboard layout.
However, you can configure `tilish` for international keyboards by providing a string
`@tilish-shiftnum` prepared by pressing <kbd>Shift</kbd> + 
<kbd>1</kbd><kbd>2</kbd><kbd>3</kbd><kbd>4</kbd><kbd>5</kbd><kbd>6</kbd><kbd>7</kbd><kbd>8</kbd><kbd>9</kbd><kbd>0</kbd>. 
For instance, for a UK keyboard, you would configure it as follows:

	set -g @tilish-shiftnum '!"£$%^&*()'

Your terminal must support sending keycodes like `M-£` for the above to work.
For instance, a UK keyboard layout works fine on `urxvt`, but does not work 
by default on `kitty` or `alacritty`, which may require additional configuration.

## Easy mode

To make the plugin more accessible for people who do not use `vim` as well,
there is also an "easy mode" available, which uses arrow keys instead of 
the `vim`-style <kbd>h</kbd><kbd>j</kbd><kbd>k</kbd><kbd>l</kbd> keys.
This mode can be activated by putting this in your `.tmux.conf`:

	set -g @tilish-easymode 'on'

The revised keybindings for the pane focus and movement then become:

| Keybinding | Description |
| ---------- | ----------- |
| <kbd>Alt</kbd> + <kbd>&#8592;</kbd><kbd>&#8595;</kbd><kbd>&#8593;</kbd><kbd>&#8594;</kbd> | Move focus left/down/up/right |
| <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>&#8592;</kbd><kbd>&#8595;</kbd><kbd>&#8593;</kbd><kbd>&#8594;</kbd> | Move pane left/down/up/right |

## Prefix mode
Note that this feature is currently only available in `tmux` v2.4+.
The "prefix mode" uses a prefix key instead of <kbd>Alt</kbd>, and 
may be particularly interesting for users of editors like `kak` and 
`emacs` that use <kbd>Alt</kbd> key a lot. To activate this mode, you
define a prefix keybinding in your `tmux.conf`. For instance, to use
<kbd>Alt</kbd> + <kbd>Space</kbd> as your `tilish` prefix, add:

	set -g @tilish-prefix 'M-space'

Actions that would usually be done by <kbd>Alt</kbd> + <kbd>key</kbd>
are now accomplished by pressing the prefix and then <kbd>key</kbd>. 
For example, opening a split is usually <kbd>Alt</kbd> + <kbd>Enter</kbd>,
but with the above prefix this becomes <kbd>Alt</kbd> + <kbd>Space</kbd> 
then <kbd>Enter</kbd>. Note that the `tilish` prefix is different from
the `tmux` prefix, and should generally be bound to a different key.
For the prefix key, you can choose basically any keybinding that `tmux`
supports, e.g. `F12` or `C-s` or anything else you may prefer.

All these keybindings are `repeat`'able, so you do not have to press the
prefix key again if you type multiple commands fast enough. Thus, pressing
<kbd>Alt</kbd> + <kbd>Space</kbd> followed by <kbd>h</kbd><kbd>j</kbd> would 
move to the left and then down, without requiring another prefix activation.
The `tmux` option `repeat-time` can be used to customize this timeout.
Personally, I find the default 500ms timeout somewhat short, and would
recommend that you increase this to at least a second if you use `tilish`:

	set -g repeat-time 1000

## Application launcher

In `i3wm`, the keybinding <kbd>Alt</kbd>+<kbd>d</kbd> is by default mapped to
the application launcher `dmenu`, which can be practical to quickly open apps.
If you have [`fzf`][5] available on your system, `tilish` can offer a similar 
application launcher using the same keyboard shortcut. To enable this 
functionality, add the following to your `~/.tmux.conf`:

	set -g @tilish-dmenu 'on'

Basically, pressing <kbd>Alt</kbd>+<kbd>d</kbd> will then pop up a split
that lets you fuzzy-search through all executables in your system `$PATH`.
Selecting an executable runs the command in that split. When you want 
to start an interactive process, this can be more convenient than
using <kbd>Alt</kbd>+<kbd>Enter</kbd> and typing the command name.
This functionality is currently only available in `tmux` v2.7+.

[5]: https://github.com/junegunn/fzf

## Terminal compatibility

It is worth noting that not all terminals support all keybindings. It 
has been verified to work out-of-the-box on `alacritty`, `kitty`, `urxvt`, 
`terminator`, and `gnome-terminal` on Linux. Note that in `gnome-terminal`,
it only works if you don't open any GUI tabs; if you do so, the terminal 
itself steals the <kbd>Alt</kbd>+<kbd>0</kbd>-<kbd>9</kbd> keybindings.

On `wsltty` (Windows), it works if you disable the keyboard shortcut 
<kbd>Alt</kbd>+<kbd>Enter</kbd> in the terminal settings. Some terminals
that work automatically on Linux, e.g. `alacritty`, also require unbinding
<kbd>Alt</kbd>+<kbd>Enter</kbd> on Windows. This `alacritty.yml` does so:

    key_bindings:
      - { key: Return, mods: Alt, action: ReceiveChar }

If you use `xterm`, almost none of the <kbd>Alt</kbd> keys work by default.
That can be fixed by adding this to `~/.Xresources`:

	XTerm*eightBitControl: false
	XTerm*eightBitInput: false
	XTerm.omitTranslation: fullscreen
	XTerm*fullscreen: never

## Integration with vim-tmux-navigator

There is a great `vim` plugin called [vim-tmux-navigator][3], which allows seamless 
navigation between `vim` splits and `tmux` splits. If you're using that plugin,
you can tell `tilish` about it to make it setup the keybindings for you. (If you
don't tell `tilish`, it uses fallback keybindings that only work in `tmux`.)

The process is quite simple. First install the plugin for `vim` or `neovim`, as
described on the [vim-tmux-navigator website][3]. Then place this in your
`~/.config/nvim/init.vim` (`neovim`) or `~/.vimrc` (`vim`):

	noremap <silent> <m-h> :TmuxNavigateLeft<cr>
	noremap <silent> <m-j> :TmuxNavigateDown<cr>
	noremap <silent> <m-k> :TmuxNavigateUp<cr>
	noremap <silent> <m-l> :TmuxNavigateRight<cr>

You then just have to tell `tilish` that you want the integration:

	set -g @tilish-navigator 'on'

A minimal working  example of a `~/.tmux.conf` with `tpm` would then be:

	# List of plugins.
	set -g @plugin 'tmux-plugins/tpm'
	set -g @plugin 'tmux-plugins/tmux-sensible'
	set -g @plugin 'jabirali/tmux-tilish'
	
	# Plugin options.
	set -g @tilish-navigator 'on'
	
	# Install `tpm` if needed.
	if "test ! -d ~/.tmux/plugins/tpm" \
	   "run 'git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm && ~/.tmux/plugins/tpm/bin/install_plugins'"
	
	# Activate the plugins.
	run -b "~/.tmux/plugins/tpm/tpm"

[3]: https://github.com/christoomey/vim-tmux-navigator

# Related projects

- [3mux](https://github.com/aaronjanse/3mux)
- [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator)
- [vim-i3wm-tmux-navigator](https://github.com/fogine/vim-i3wm-tmux-navigator)
