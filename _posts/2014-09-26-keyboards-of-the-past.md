---
layout: post
title: Keyboards of the past
tags: keyboard

---

Before settling with the PC, I have seen and used a couple of other
computers. Their keyboards are worth looking at.

----

## Sinclair ZX Spectrum

ZX Spectrum (hereafter just “Spectrum”) was a machine based on the
8-bit Z80 processor. I will describe the base 1982 model, because
that’s what I had.

Spectrum came with 16K ROM and 48K RAM, in a single 16-bit address
space. The low 16K were ROM which contained the whole OS and a
built-in BASIC interpreter. In the I/O department, Spectrum was
designed to connect to a TV yielding a 256×172 screen (32×24 text with
8×8 character cells) and to an audio cassette recorder used as
external storage.

Spectrum’s keyboard had just 40 keys (in total). Its characteristic
feature was an elaborate system of input modes and modifiers.
Combined, they allowed each key to have up to 8 functions.

Spectrum assigned a character code not only to characters, but also to
each BASIC keyword. This greatly simplified the parser and made for
compact program representation. The keyboard allowed entering every
such keyword and also a number of control characters.

|`BLUE`<br>`EDIT`<br>`▝` `▙`<br>`1` `!`<br>`DEF FN`|`RED`<br>`CAPS LOCK`<br>`▘` `▟`<br>`2` `@`<br>`FN`|`MAGENTA`<br>`TRUE VIDEO`<br>`▀` `▄`<br>`3` `#`<br>`LINE`|`GREEN`<br>`INV.VIDEO`<br>`▗` `▛`<br>`4` `$`<br>`OPEN #`|`CYAN`<br>`←`<br>`▐` `▌`<br>`5` `%`<br>`CLOSE #`|`YELLOW`<br>`↓`<br>`▚` `▞`<br>`6` `&`<br>`MOVE`|`WHITE`<br>`↑`<br>`▜` `▖`<br>`7` `'`<br>`ERASE`|<br>`→`<br>` ` `█`<br>`8` `(`<br>`POINT`|<br>`GRAPHICS`<br><br>`9` `)`<br>`CAT`|`BLACK`<br>`DELETE`<br><br>`0` `_`<br>`FORMAT`|
|`SIN`<br>`Q` `<=`<br>`PLOT`<br>`ASN`|`COS`<br>`W` `>=`<br>`DRAW`<br>`ACS`|`TAN`<br>`E` `<>`<br>`REM`<br>`ATN`|`INT`<br>`R` `<`<br>`RUN`<br>`VERIFY`|`RND`<br>`T` `>`<br>`RANDOMIZE`<br>`MERGE`|`STR$`<br>`Y` `AND`<br>`RETURN`<br>`[`|`CHR$`<br>`U` `OR`<br>`IF`<br>`]`|`CODE`<br>`I` `AT`<br>`INPUT`<br>`IN`|`PEEK`<br>`O` `;`<br>`POKE`<br>`OUT`|`TAB`<br>`P` `"`<br>`PRINT`<br>`©`|
|`READ`<br>`A` `STOP`<br>`NEW`<br>`~`|`RESTORE`<br>`S` `NOT`<br>`SAVE`<br>`|`|`DATA`<br>`D` `STEP`<br>`DIM`<br>`\`|`SGN`<br>`F` `TO`<br>`FOR`<br>`{`|`ABS`<br>`G` `THEN`<br>`GOTO`<br>`}`|`SQR`<br>`H` `↑`<br>`GO SUB`<br>`CIRCLE`|`VAL`<br>`J` `-`<br>`LOAD`<br>`VAL$`|`LEN`<br>`K` `+`<br>`LIST`<br>`SCREEN$`|`USR`<br>`L` `=`<br>`LET`<br>`ATTR`|`ENTER`|
|`CAPS SHIFT`|`LN`<br>`Z` `:`<br>`COPY`<br>`BEEP`|`EXP`<br>`X` `£`<br>`CLEAR`<br>`INK`|`LPRINT`<br>`C` `?`<br>`CONTINUE`<br>`PAPER`|`LLIST`<br>`V` `/`<br>`CLS`<br>`FLASH`|`BIN`<br>`B` `*`<br>`BORDER`<br>`BRIGHT`|`INKEY$`<br>`N` `,`<br>`NEXT`<br>`OVER`|`PI`<br>`M` `.`<br>`PAUSE`<br>`INVERSE`|`SYMBOL SHIFT`|`BREAK`<br>`SPACE`|
{:.keyboard-spectrum}

(Spectrum was a British machine, so it had the `£` pound sign
character in the basic ASCII table, on the place of `` ` ``. The `↑`
up arrow was used as the exponentiation operator and occupied the
place of `^`. The `©` copyright sign was the glyph for the `0x7F`
code.)

### How did it work?

First, there was the normal input mode. The cursor in that mode looked
as a flashing letter `L`. (In Spectrum parlance, “flashing” meant
alternating between normal and reversed display at a rate of about
1Hz.) In this mode, each key produced its primary character (shown in
big font above).

You could also hold `Caps Shift` and press a letter key, and it then
produced a capital letter.

If you pressed `Caps Shift` with a digit, it produced a control
character shown in the second line. Most of these were editing
functions.

Or you could hold `Symbol Shift` and press a key, then it produced the
symbol that is shown to the right of the primary character, in red.

Pressing `Caps Shift`+`2` took you to the Caps Lock mode (and back),
which was signified with a flashing `C` and worked the same as `L` but
produced capital letters no matter if you held `Caps Shift` or not.

Then there was the Graphics mode (`G` cursor). You got into and out of
it by pressing `Caps Shift`+`9`. In this mode, keys `1`–`8` with or
without `Symbol Shift` produced pseudographical blocks, and letters
`A` through `U` yielded special characters whose bitmaps you could
easily customize. Some games used them for sprites.

Next was the Keyword mode (`K`). It activated itself automagically
when you were at a position where only a keyword made sense — at the
start of a line, or immediately after `:` or `THEN`. (Spectrum BASIC
had no `ELSE`.) In this mode, each letter key produced the keyword
shown below the letter, in black. (Digit keys and `Symbol Shift`
worked normally in this mode; `Caps Shift` was ignored for letters and
editing functions worked normally.)

Pressing `Caps Shift`+`Symbol Shift` took you to and from the Extended
mode (`E`). Then keys produced the character shown at the top of the
key, in green. With any shift, they yielded the character at the
bottom, in red. The color names on the digit keys denote control
characters — you could change the foreground or background color of
the following text, much like ANSI `ESC` sequences in today’s UNIX
terminal emulators. Which color got changed depended on whether you
held `Caps Shift`.

In modern terms, you could say `Caps Shift` was like `Shift` when used
with letters and `Ctrl`, `Cmd` or `Fn` with digits; `Symbol Shift` was
like modern `AltGr` or `Option`.


### What if we modeled a keyboard layout after Spectrum?

* Take the Spectrum keyboard
* Remove BASIC keywords
* Remove editing functions (`Caps Shift`+digits)
* Leave small letters, capital letters, digits and single-character
  symbols
* Move symbols from the extended mode into `Symbol Shift` layer
* Change `↑` to `^` and `£` to `` ` ``

|`1` `!`|`2` `@`|`3` `#`|`4` `$`|`5` `%`|`6` `&`|`7` `'`|`8` `(`|`9` `)`|`0` `_`|
|`Q` `≤`|`W` `≥`|`E` `≠`|`R` `<`|`T` `>`|`Y` `[`|`U` `]`|`I` `©`|`O` `;`|`P` `"`|
|`A` `~`|`S` `|`|`D` `\`|`F` `{`|`G` `}`|`H` `^`|`J` `-`|`K` `+`|`L` `=`||
||`Z` `:`|`X` `` ` ``|`C` `?`|`V` `/`|`B` `*`|`N` `,`|`M` `.`|||
{:.keyboard .with-altgr}

With 26 letter keys, 10 digit keys and 2 modifiers, this can
theoretically represent 36 × 3 = 108 characters = 94 printable ASCII
(not including space) + 14. Enough for German and French.

Key formula: 10 + 10 + 9 + 7.


## Electronica MS 0511 aka UKNC

This was a Soviet clone of the PDP-11, used primarily in education. A
typical computer class had a single teacher machine and around 10 to
15 student machines. The teacher machine had a floppy disk drive or
two, and served as the network controlling host. Student machines only
communicated with the server over network.

The keyboard of a UKNC was laid out after the standard Russian layout,
JCUKEN, with Latin letters following the phonetic principle:

||`+`<br>`;`|`!`<br>`1`|`"`<br>`2`|`#`<br>`3`|`¤`<br>`4`||`%`<br>`5`|`&`<br>`6`|`'`<br>`7`|`(`<br>`8`|`)`<br>`9`|` `<br>`0`|`=`<br>`-`|`?`<br>`/`|
||`J`<br>` ` `Й`|`C`<br>` ` `Ц`|`U`<br>` ` `У`|`K`<br>` ` `К`|`E`<br>` ` `Е`||`N`<br>` ` `Н`|`G`<br>` ` `Г`|`{`<br>`[` `Ш`|`}`<br>`]` `Щ`|`Z`<br>` ` `З`|`H`<br>` ` `Х`|`_`<br>` ` `Ъ`|`*`<br>`:`|
||`F`<br>` ` `Ф`|`Y`<br>` ` `Ы`|`W`<br>` ` `В`|`A`<br>` ` `А`|`P`<br>` ` `П`||`R`<br>` ` `Р`|`O`<br>` ` `О`|`L`<br>` ` `Л`|`D`<br>` ` `Д`|`V`<br>` ` `Ж`|`|`<br>`\` `Э`|`>`<br>`.`|
||`Q`<br>` ` `Я`|`~`<br>`^` `Ч`|`S`<br>` ` `С`|`M`<br>` ` `М`|`I`<br>` ` `И`||`T`<br>` ` `Т`|`X`<br>` ` `Ь`|`B`<br>` ` `Б`|`` ` ``<br>`@` `Ю`|`<`<br>`,`|
{:.keyboard .with-groups}

The keyboard had two Shift keys (Space row, on the sides); one Ctrl
key (home row, left hand, outer column); and in the bottom row on the
left of `Q` were two keys labeled `Alph` and `Graph`. `Alph` normally
served as a modifier to enter a single letter from the other alphabet;
`Graph` was a modifier for box drawing characters (not shown on the
schematic above). There was also a single `Lock` key between left
Shift and the space bar; pressing it while holding any modifier locked
that particular modifier. (It was fun locking `Ctrl` on someone’s
terminal and watching him or her being unable to enter anything.)

Key formula: 13 + 13 + 12 + 10.

With 48 keys, the space bar and three modifiers (not counting `Ctrl`),
this was enough to represent the whole low ASCII table, the 32 Russian
letters (without _yo_), and around 32 pseudographic characters.

A wonderful feature was that the UKNC keyboard had dedicated keys for
essential punctuation characters (`.`, `,`, `;` and `:`); punctuation
stayed on its place when you switched layouts.
