---
layout: post
title: The case of disappearing output
---

I am seeing this little bug. It is not critical to my work, but
annoying as hell. Moreover, it is sufficiently peculiar to my own
workflow that not many other people ever see it. So I’m writing this
in the hope that it will help me understand the nature of the bug and
possibly fix it.

----

So here’s the setup.

* I have a widescreen monitor, 24″ diagonal.
* I use the `i3` tiling window manager to split it in two stacks, one
  on the left, the other on the right. Each stack may have one or more
  windows. The title bars of all windows in the stack are visible
  above the currently active window — this makes the stacking metaphor
  obvious and greatly helps remembering where each window is.
* At least one window is a terminal emulator. (Sometimes one in each
  stack.)
* Within the terminal emulator runs a `tmux` session, often with
  several `tmux` windows, some of which may be split vertically into
  two panes.
* Every pane runs either Midnight Commander or an `ssh` remote session
  which runs `mc` remotely. (That’s how I roll. I need constant
  visual/spatial aid when navigating a directory structure.)

Midnight Commander has this nifty feature called the subshell.
Basically, when you press `Ctrl+O`, the panels hide and you get a
shell prompt and the output of the commands you ran previously.

And that is where the bug creeps in.

**Initial state:** screen split horizontally into two stacks, one
containing Firefox and Emacs (GTK+ build), the other containing
`xterm`; `xterm` is active, `tmux` running with one window with `mc`
running locally, panels hidden, some output displayed, e.g. that of
`ls -al`.

**Steps to reproduce:** Open a new window (e.g. `gedit`) in that
stack. Switch back to `xterm`.

**Expected behavior:** `xterm` shows the same output as before, except
perhaps for the topmost line which has to make way to the new window’s
title bar.

**Observed behavior:** the output disappears completely, though the
cursor is left at the same position.

----

Now that was an elaborate setup with many variables. Let’s minimize it.

* The number and nature of the windows in the other stack do not matter.
* The nature of windows in `xterm`’s stack does not matter.
* In fact, the only thing that matters is that `xterm` is in a stack
  and that the number of windows in that stack changes.

In fact, what matters is that the `xterm` window is resized. I can
verify it by floating the `xterm` window and resizing it with the
mouse. As soon as the number of lines or columns changes, the screen
clears.

Observation: It only clears the first time after I hide `mc`’s panels.
If I perform another command with hidden panels and resize the window,
everything behaves normally. But when I subsequently toggle panels
back on and off, some other output is restored and the cursor is in
the wrong place.

So here’s the simplified recipe, one variable (`i3`) down.

**Initial state:** `xterm` running `tmux` running a single window with
a single pane running `mc`, panels displayed.

**To reproduce:** hide panels, do a `ls -al`, resize the `xterm`
window.

**Expected behavior:** essentially the same output as before.

**Observed behavior:** screen cleared.

----

Now let’s vary the terminal emulator variable. It turns out the same
thing happens with `gnome-terminal`. And `xfce4-terminal`. And
`stterm`. And `konsole`. Looks like the exact terminal emulator does
not matter, only that it’s capable of being resized.

See if `tmux` is of any importance.

* The problem never occurs without `tmux`, with `mc` running directly
  in `xterm`. Resize the window to your heart’s content, the output
  gets truncated on the right if I make the window too narrow, but
  otherwise is uncorrupted.
* This leaves “the” other terminal multiplexer, `screen`. Initially,
  there is no problem with `screen` either… as long as I don’t enable
  `altscreen on`. Turning the `alternate-screen` option `off` in
  `tmux` also “fixes” the clear-on-resize problem.

Bummer; alternate screen is what feels “right” for a fullscreen
application such as `mc`. Without alternate screen, I can’t even
retain the output across a show-panels/hide-panels cycle.
Clear-on-toggle-panels is a more severe problem than clear-on-resize.

But plain `xterm` does have alternate screen switching, yet it doesn’t
exhibit the problem. While `tmux` and `screen` do. What’s going on?

One more exercise. How does it survive pane splitting and resizing?
Same problem:

**Initial state:** `tmux` with `alternate-screen on` or `screen` with
`altscreen on` running a single window with one or more panes, one of
them active and running `mc`, panels displayed.

**To reproduce:** hide panels, do a `ls -al`, split the window in any
direction or resize the pane.

**Expected behavior:** output stays on the screen.

**Observed behavior:** pane cleared.

----

Now would be a good time to try composing a bug report… What shall I
say?

**Versions:** Ubuntu 16.04, tmux 2.1-3build1,

```
$ mc -V
GNU Midnight Commander 4.8.15
Built with GLib 2.47.3
Using the S-Lang library with terminfo database
With builtin Editor
With subshell support as default
With support for background operations
With mouse support on xterm and Linux console
With support for X11 events
With internationalization support
With multiple codepages support
Virtual File Systems: cpiofs, tarfs, sfs, extfs, ext2undelfs, ftpfs, sftpfs, fish
Data types: char: 8; int: 32; long: 64; void *: 64; size_t: 64; off_t: 64;
```

**To reproduce:**

1. Start `xterm`.
2. Start `tmux` with default configuration: `tmux -f /dev/null`
3. Start `mc` with default configuration: `mc`
4. Hide panels: `Ctrl+O`
5. Produce some output: `ls -al`
6. Resize the `xterm` window

WTF? It does not reproduce this way!

Clearly, something is wrong with my `tmux` config! What could that be?

```
set -g default-terminal "screen-256color"
```

Hmmm.

* `screen` (default): works
* `tmux`: clears
* `screen-256color`: clears
* `tmux-256color`: clears
* `screen-bce`: clears, though there are only minimal differences from
  `screen`

```
$ infocmp screen | sed s/^screen/myscreen/ | tic -
$ tmux -f /dev/null
$ export TERM=myscreen
$ mc
(Ctrl+O)
$ ls -al
(resize)
```

Clears! Although there are absolutely no differences from `screen`
whatsoever! What. the. hell. is. going. on?

And, just for lulz, because it’s absolutely not the right `$TERM` for
within `tmux`:

```
$ tmux -f /dev/null
$ export TERM=xterm-256color
$ mc
(Ctrl+O)
$ ls -al
(resize)
```

Works!

And, also just for lulz:

```
$ infocmp screen-256color | sed s/^screen-256color/screen/ | tic -
$ tmux
$ export TERM=screen
$ mc
```

Also works!

Something is looking at the value of `$TERM` instead of using
terminfo capabilities!

A recursive search for `screen` in `*.[ch]` over the `mc` source
reveals a couple places where the `$TERM` value is compared
(`lib/tty/key.c:1331`, `lib/tty/tty.c:114`), but they always check for
prefix match, so `screen-256color` would work too. So no, it’s not
`mc`’s fault.

Next suspect is the `S-Lang library with terminfo database`.

```
$ apt-get source libslang2-dev
$ grep -Rn
```

And this place looks highly suspicious!

```
slang2-2.3.0/src/sldisply.c:2600:                  || !strcmp (term, "screen"));
```

It’s in the function `SLtt_initialize` and it checks `$TERM` against a
few values to set the `almost_vtxxx` flag if it matches one of:
`linux*`, `con*`, `vt[1-9]*` except `vt52`, `xterm*`, `rxvt*`,
`Eterm*` or `screen`. See, all other suspects are checked for prefix
match, but `screen` for strict equality.

Further, this variable is used if `_pSLtt_tigetent(term)` returns a
`NULL`, performing some fallback initialization(?) (`src/sldisply.c:2608`):

```c
   if (NULL == (Terminfo = _pSLtt_tigetent (term)))
     {
        if (almost_vtxxx) /* Special cases. */
          {
             int vt102 = 1;
             if (!strcmp (term, "vt100")) vt102 = 0;
             get_color_info ();
             SLtt_set_term_vtxxx (&vt102);
             (void) SLtt_get_screen_size ();
             return 0;
          }
        return -1;
     }
```

This happens if `$TERM` names a terminal for which we don’t have a
terminfo file, and several other cases when one can’t be loaded.
Debugging shows that does not happen in my case.

Otherwise (`_pSLtt_tigetent(term)` returns non-`NULL`) `almost_vtxxx`
is used here (`src/sldisply.c:2670`):

```c
   /* If I do this for vtxxx terminals, arrow keys start sending ESC O A,
    * which I do not want.  This is mainly for HP terminals.
    */
   Keypad_Init_Str = tt_tgetstr ("ks");
   Keypad_Reset_Str = tt_tgetstr ("ke");
   if ((almost_vtxxx == 0) && (SLtt_Force_Keypad_Init == -1))
     SLtt_Force_Keypad_Init = 1;
```

So, in my case, with `TERM=screen` `SLtt_Force_Keypad_Init` stays
negative, but with `TERM=screen-256color` or `TERM=tmux` it is forced
to 1.

That global variable is only used in two places, and those are
function `SLtt_init_keypad` and `SLtt_deinit_keypad`, which are no-ops
if `SLtt_Force_Keypad_Init` is negative. No other code in `libslang2`
changes this variable, and neither does `mc`.

Sticking a `return;` at the beginning of both `SLtt_init_keypad` and
`SLtt_deinit_keypad` and recompiling `libslang2` does indeed solve the
problem. (As does fixing the `"screen"` comparison.)

----

So I reported these findings to the author and maintainer of
`libslang2`, suggesting that `screen*` and `tmux*` should be treated
uniformly with `screen`. He seems unconvinced, and suggests that (lack
of) keypad initialization only masks a problem elsewhere. He is at
least partly right.

Well, it’s time for the chemical protection suit and the debugger.

In `mc`, the most interesting function is `toggle_panels`
(`src/execute.c:453`). At first glance, it seems to perform several
shutdown tasks, then call `invoke_subshell` (which blocks until
`Ctrl+O` is pressed to return to panels *or* the subshell exits
cleanly), then performs several initialization tasks again:

```c
void
toggle_panels (void)
{
    […]
    channels_down ();
    disable_mouse ();
    disable_bracketed_paste ();
    if (clear_before_exec)
        clr_scr ();
    if (mc_global.tty.alternate_plus_minus)
        numeric_keypad_mode ();
    […]
    tty_noecho ();
    tty_keypad (FALSE);
    tty_reset_screen ();
    do_exit_ca_mode ();
    tty_raw_mode ();
    […]
        invoke_subshell (NULL, VISIBLY, new_dir_p);
    […]
    do_enter_ca_mode ();

    tty_reset_prog_mode ();
    tty_keypad (TRUE);
    […]
    enable_mouse ();
    enable_bracketed_paste ();
    channels_up ();
    if (mc_global.tty.alternate_plus_minus)
        application_keypad_mode ();
    […]
        repaint_screen ();
}
```

At 115 lines, this is one pretty involved function, and that’s why I
was reluctant to dig into it initially. A deeper inspection, however,
uncovers a bug.

The `do_exit_ca_mode` and `do_enter_ca_mode` functions’ names hint
that they are the ones that switch from and to the alternate screen,
with all the saving and restoring activities. They are also simple as
a cork:

```c
void
do_enter_ca_mode (void)
{
    if (mc_global.tty.xterm_flag && smcup != NULL)
    {
        fprintf (stdout, /* ESC_STR ")0" */ ESC_STR "7" ESC_STR "[?47h");
        fflush (stdout);
    }
}
```

```c
void
do_exit_ca_mode (void)
{
    if (mc_global.tty.xterm_flag && rmcup != NULL)
    {
        fprintf (stdout, ESC_STR "[?47l" ESC_STR "8" ESC_STR "[m");
        fflush (stdout);
    }
}
```

<!-- ] -->

Basically, invoke the terminal’s Save Cursor, Use Alternate Screen
Buffer, Use Normal Screen Buffer, Restore Cursor sequences; and also
reset the character attributes.

But see the two adjacent function calls, to `tty_reset_screen` and
`tty_reset_prog_mode`. These lead into `SLsmg_reset_smg` and
`SLsmg_init_smg` respectively, which are `libslang2` functions that
*also* do a bunch of shutdown and initialization tasks.

Including switching from and to the alternate screen. But they do it
by sending the terminfo-specific escape sequences `smcup` and `rmcup`.
Which for me translate into `ESC [ ? 1 0 4 9 h`<!-- ] --> and `ESC
[ ? 1 0 4 9 l`,<!-- ] --> respectively. These save/restore the screen
contents and the cursor position in one go.

Now what does that mean for `tmux`? `tmux` sees an application do six
actions. On startup:

* Save cursor position using `ESC 7`.
* Switch to the alternate screen without saving the cursor position
  using `ESC [ ? 4 7 h`<!-- ] -->. (#1)
* Switch to the alternate screen, saving the cursor position along the
  way, using `ESC [ ? 1 0 4 9 h`<!-- ] -->. *This uses a different
  variable to store the cursor position. And because the alternate
  screen is already active, this is a no-op.*

When hiding panels:

* Switch back to the main screen and restore cursor position, using
  `ESC [ ? 1 0 4 9 l`<!-- ] --> (#2). *Since the last switch to the
  alternate screen at #1 did not save the cursor position, the cursor
  position is now garbage.*
* Switch back to the main screen without restoring cursor position,
  using `ESC [ ? 4 7 l`<!-- ] -->. *The main screen is already active,
  so this is a no-op too.*
* Restore cursor position using `ESC 8`.

Now, the garbage cursor position affects the way the screen contents
is restored, causing further garbage.(?) TODO: re-check this

So, the fix includes removing the calls to `do_exit_ca_mode` and
`do_enter_ca_mode` from `toggle_panels` or possibly neutering these
functions in the `slang` build.

----

But even then the output continues to disappear.

Further observation: It is not even necessary to resize the window or
the pane. Sufficient to send a `SIGWINCH` to the `mc` process while
its panels are hidden. The screen clears, while the cursor stays in
place. (Now that I patched out duplicate alternate screen switching, a
`Ctrl+O Ctrl+O` restores output.)

`mc`’s `WINCH` handler just sets a global flag and re-establishes
itself for the next signal. However, that flag is noticed in the
`feed_subshell` function (`src/subshell.c:498`):

```c
/* Despite using SA_RESTART, we still have to check for this */
if (errno == EINTR)
{
    if (mc_global.tty.winch_flag != 0)
        tty_change_screen_size ();

    continue;       /* try all over again */
}
```

And `tty_change_screen_size` calls `SLsmg_reinit_smg`, which sees that
`smg` is currently not initialized (because of the `SLsmg_reset_smg`
call when the panels were toggled off) and calls `SLsmg_init_smg`.
Which leads to a switch into the alternate screen, even though it’s
not appropriate with panels hidden. And a switch to the alternate
screen involves clearing the alternate screen, which explains the
blackout. And `SLsmg_init_smg` also calls `SLtt_init_keypad`, which,
if not disabled by `almost_vtxxx`, flushes output, forcing the clear.
(Does that mean that in the `almost_vtxxx` configuration the screen is
not cleared only because output is not flushed? 😨)

Anyway, adding this check fixes the problem for me:

```diff
-    if (mc_global.tty.winch_flag != 0)
+    if (how == QUIETLY && mc_global.tty.winch_flag != 0)
```

----

I have filed a couple of tickets ([#3639][1], [#3640][2]) and attached
patches against Midnight Commander.

[1]: http://www.midnight-commander.org/ticket/3639
[2]: http://www.midnight-commander.org/ticket/3640

Additionally, I created an [Ubuntu PPA][3] to host packages patched
against this bug.

[3]: https://launchpad.net/~yurivkhan/+archive/ubuntu/mc
