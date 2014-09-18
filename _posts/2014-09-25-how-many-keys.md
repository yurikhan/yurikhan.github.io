---
layout: post
title: How many keys does one need to fit the alphabet?
tags: keyboard

---

Look at your keyboard. How many alphanumeric keys do you have?

On an US ANSI standard keyboard (the kind that are called 104-key), 47
(not counting the space bar and the numeric keypad). On a European ISO
or a Japanese JIS keyboard, 48. Brazilian keyboards have 49.

How did that number come to be? Why do we (and do we really) need so
many?

----

The PC keyboard was designed in the time period when the ASCII table
was already stable but the notion of international compatibility has
not yet entered people’s minds. Therefore, it’s very US-centric. The
47 keys + space bar are necessary and sufficient to represent the 95
printable characters of the ASCII table, using one Shift modifier.

|`~`<br>`` ` ``|`!`<br>`1`|`@`<br>`2`|`#`<br>`3`|`$`<br>`4`|`%`<br>`5`||`^`<br>`6`|`&`<br>`7`|`*`<br>`8`|`(`<br>`9`|`)`<br>`0`|`_`<br>`-`|`=`<br>`+`|
||`Q`|`W`|`E`|`R`|`T`||`Y`|`U`|`I`|`O`|`P`|`{`<br>`[`|`}`<br>`]`|
||`A`|`S`|`D`|`F`|`G`||`H`|`J`|`K`|`L`|`:`<br>`;`|`"`<br>`'`|`|`<br>`\`|
||`Z`|`X`|`C`|`V`|`B`||`N`|`M`|`<`<br>`,`|`>`<br>`.`|`?`<br>`/`|
{: .keyboard}

(The location of the backslash key varies by keyboard model. I will
display it in the home row because this yields the most compact
schematic.)

(Keyboard schematic conventions: I will display a blank column in the
middle, so that hand separation is evident. I also won’t replicate row
staggering because that is a nuisance both technically and
ergonomically.)

**US English**: 26 letters, 95 characters, 1 modifier.


## Going international

Now, what problems do other nations face with this US-centric keyboard
design? How do they solve it?


### United Kingdom of Great Britain and Northern Ireland

The UK uses the same English, so there should be no problem. They
should just be able to use the same 47-key US keyboard. Right?

Yet, the UK uses a different physical layout (ISO). They have one more
key to the left of `Z`. This allows them to fit two more characters:
these are the `£` Pound Sign and the `¬` Not Sign. They also reshuffle
base characters a bit.

|**`¬`**<br>`` ` ``|`!`<br>`1`|**`"`**<br>`2`|**`£`**<br>`3`|`$`<br>`4`|`%`<br>`5`||`^`<br>`6`|`&`<br>`7`|`*`<br>`8`|`(`<br>`9`|`)`<br>`0`|`_`<br>`-`|`=`<br>`+`|
||`Q`|`W`|`E`|`R`|`T`||`Y`|`U`|`I`|`O`|`P`|`{`<br>`[`|`}`<br>`]`|
||`A`|`S`|`D`|`F`|`G`||`H`|`J`|`K`|`L`|`:`<br>`;`|**`@`**<br>`'`|**`~`**<br>**`#`**|
|**`|`**<br>**`\`**|`Z`|`X`|`C`|`V`|`B`||`N`|`M`|`<`<br>`,`|`>`<br>`.`|`?`<br>`/`|
{: .keyboard}

(Keyboard schematic convention: changes from US English are in bold.)

**UK English**: 26 letters, 97 characters, 1 modifier.


### Germany

The German alphabet has three and a half more letters than English
(the umlauted `Ä`, `Ö` and `Ü`, and the small-only ligature `ß`).
Also, the Euro.

The German keyboards generally have the same ISO layout as UK, so they
have just one additional key. In order to fit all needed letters, they
have to start sacrificing ASCII punctuation.

They work around this by converting one modifier key into an
additional `Shift`-like modifier called `AltGr`. Suddenly, 48
additional character slots! (96 if you use `AltGr`+`Shift`.) As a
programmer, though, I’d hate to use the German layout.

Another feature is the dead keys. They are primarily used for
diacritics. You press the dead key, then a letter, you get a letter
with the diacritic. The German layout has dead keys for the
circumflex, acute and grave accents. The circumflex `^` is also
available as a separate character; the grave and acute are not.

|**`°`**<br>**``^``**{:.dead}|`!`<br>`1`|**`"`**<br>`2` `²`|**`§`**<br>`3` `³`|`$`<br>`4`|`%`<br>`5`||`^`<br>`6`|**`/`**<br>`7` `{`|**`(`**<br>`8` `[`|**`)`**<br>`9` `]`|**`=`**<br>`0` `}`|**`?`**<br>**`ß`** `\`|**`` ` ``**{:.dead}<br>**`´`**{:.dead}|
||`Q`<br>` ` `@`|`W`|`E`<br>` ` `€`|`R`|`T`||**`Z`**|`U`|`I`|`O`|`P`|**`Ü`**|**`*`**<br>**`+`** `~`|
||`A`|`S`|`D`|`F`|`G`||`H`|`J`|`K`|`L`|**`Ö`**|**`Ä`**|**`'`**<br>**`#`**|
|**`>`**<br>**`<`** `|`|**`Y`**|`X`|`C`|`V`|`B`||`N`|`M`<br>` ` `µ`|**`;`**<br>`,`|**`:`**<br>`.`|**`_`**<br>**`-`**|
{: .keyboard .with-altgr}

(Keyboard schematic conventions: Dead keys are shown with dotted border. `AltGr` layer in green.)

**German**: 29 letters, 105 direct characters, 3 dead keys, 2 modifiers.


### France

The French alphabet has vastly more diacritics than German. As a
result, they mostly don’t bother putting all letters with diacritics
on the keyboard, utilizing dead keys instead. A few exceptions are
made for the most commonly used small letters `à`, `è`, `é`, `ù` and
`ç` which are given easily accessible keys.

Interestingly, the French layout puts digits in the `Shift` layer.

|<br>**`²`**|**`1`**<br>**`&`**|**`2`**<br>**`é`** `~`{:.dead}|**`3`**<br>**`"`** `#`|**`4`**<br>**`'`** `{`|**`5`**<br>**`(`** `[`||**`6`**<br>**`-`** `|`|**`7`**<br>**`è`** `` ` ``{:.dead}|**`8`**<br>**`_`** `\`|**`9`**<br>**`ç`** `^`|**`0`**<br>**`à`** `@`|**`°`**<br>**`)`** `]`|`+`<br>`=` `}`|
||**`A`**|**`Z`**|`E`<br>` ` `€`|`R`|`T`||`Y`|`U`|`I`|`O`|`P`|**`¨`{:.dead}**<br>**`^`{:.dead}**|**`£`**<br>**`$`** `¤`|
||**`Q`**|`S`|`D`|`F`|`G`||`H`|`J`|`K`|`L`|**`M`**|**`%`**<br>**`ù`**|**`µ`**<br>**`*`**|
|**`>`**<br>**`<`** `|`|**`W`**|`X`|`C`|`V`|`B`||`N`|**`?`**<br>**`,`**|**`.`**<br>**`;`**|**`/`**<br>**`:`**|**`§`**<br>**`!`**|
{: .keyboard .with-altgr}

**French**: 26 first-class letters, 5 small-only letters, 105 direct
characters, 4 dead keys, 2 modifiers.


### Russia

Above, we only looked at countries using languages based on the Latin
script. Let’s see what other scripts bring to the table.

The Russian alphabet consists of 33 letters, none of which coincide
with Latin. (There are letters homologic or homographic to Latin
letters, but they are separate Unicode characters.) This makes it
necessary to provide both Cyrillic and Latin input.

This is solved by having two layouts installed. One is usually US
English, the other is Russian. Switching is usually done with a
combination of modifiers (`Alt`+`Shift` is the default, `Ctrl`+`Shift`
is also common. Many Linux users prefer `Caps Lock`.)

Because the computer is virtually unusable unless *both* layouts are
installed, the Russian layout does not try very hard to preserve all
punctuation characters. Along with 26 Latin letters, 15 punctuation
characters ``#$&'<>@[]^`{|}~`` are sacrificed; to enter those, you
have to switch to the Latin layout.

|`~` `Ё`<br>`` ` ``|`!`<br>`1`|`@` `"`<br>`2`|`#` `№`<br>`3`|`$` `;`<br>`4`|`%`<br>`5`||`^` `:`<br>`6`|`&` `?`<br>`7`|`*`<br>`8`|`(`<br>`9`|`)`<br>`0`|`_`<br>`-`|`=`<br>`+`|
||`Q`<br>` ` `Й`|`W`<br>` ` `Ц`|`E`<br>` ` `У`|`R`<br>` ` `К`|`T`<br>` ` `Е`||`Y`<br>` ` `Н`|`U`<br>` ` `Г`|`I`<br>` ` `Ш`|`O`<br>` ` `Щ`|`P`<br>` ` `З`|`{`<br>`[` `Х`|`}`<br>`]` `Ъ`|
||`A`<br>` ` `Ф`|`S`<br>` ` `Ы`|`D`<br>` ` `В`|`F`<br>` ` `А`|`G`<br>` ` `П`||`H`<br>` ` `Р`|`J`<br>` ` `О`|`K`<br>` ` `Л`|`L`<br>` ` `Д`|`:`<br>`;` `Ж`|`"`<br>`'` `Э`|`|` `/`<br>`\`|
||`Z`<br>` ` `Я`|`X`<br>` ` `Ч`|`C`<br>` ` `С`|`V`<br>` ` `М`|`B`<br>` ` `И`||`N`<br>` ` `Т`|`M`<br>` ` `Ь`|`<`<br>`,` `Б`|`>`<br>`.` `Ю`|`?` `,`<br>`/` `.`|
{: .keyboard .with-groups}

(Keyboard schematic convention: the secondary group is displayed in
red, if and only if different from the primary.)

**Russian**: 33 letters, 95 characters.

**Russian + US English**: 33 + 26 = 59 letters, 161 characters, 2
groups, 1 modifier.


### Japan

The Japanese language utilizes three writing systems: Hiragana,
Katakana and Kanji. Together with Latin, it makes four.

Latin works pretty much as we are used to, but uses a different
layout.

Hiragana and Katakana are syllabaries. Collectively, they are known as
kana. Hiragana is used for native Japanese words and especially for
word forms. Katakana is for foreign imports (like we use *italics* for
Latin words in English text) and for emphasizing (like we use **bold**
or CAPS). Each syllabary has 46 commonly used characters, and a couple
rare ones. Nine characters also have a small form which is a distinct
character. There are two diacritics.

The traditional Japanese keyboard layout assigns each kana syllable to
a key. This takes up almost all alphanumeric keys, leaving just 3
available for the two diacritics and the yen sign. (Small characters
and the syllable `wo` are placed in the `Shift` layer which is
otherwise almost empty.) An additional key between `Space` and right
`Alt` switches between Hiragana and Katakana. Digits are not available
in kana mode; you have to switch to Latin mode or use the numeric
keypad.

Kanji are characters that represent whole roots of words. There are
thousands of them, and no hope of putting them on a keyboard. So they
are entered by using an *input method* — software that runs in
background, analyzes your kana input, and suggests words from a
dictionary. Additional keys at the sides of the Space bar control the
input method.

||`!`<br>`1` `ぬ`|**`"`**<br>`2` `ふ`|`#` `ぁ`<br>`3` `あ`|`$` `ぅ`<br>`4` `う`|`%` `ぇ`<br>`5` `え`||**`&`** `ぉ`<br>`6` `お`|**`'`** `ゃ`<br>`7` `や`|**`(`** `ゅ`<br>`8` `ゆ`|**`)`** `ょ`<br>`9` `よ`|` ` `を`<br>`0` `わ`|**`=`**<br>`-` `ほ`|**`~`**<br>**`^`** `へ`|**`|`**<br>**`¥`** `ー`|
||`Q`<br>` ` `た`|`W`<br>` ` `て`|`E` `ぃ`<br>` ` `い`|`R`<br>` ` `す`|`T`<br>` ` `か`||`Y`<br>` ` `ん`|`U`<br>` ` `な`|`I`<br>` ` `に`|`O`<br>` ` `ら`|`P`<br>` ` `せ`|**`` ` ``**<br>**`@`** `゛`|**`{`** `「`<br>**`[`** `゜`|
||`A`<br>` ` `ち`|`S`<br>` ` `と`|`D`<br>` ` `し`|`F`<br>` ` `は`|`G`<br>` ` `き`||`H`<br>` ` `く`|`J`<br>` ` `ま`|`K`<br>` ` `の`|`L`<br>` ` `り`|**`+`**<br>`;` `れ`|**`*`**<br>**`:`** `け`|**`}`** `」`<br>**`]`** `む`|
||`Z` `っ`<br>` ` `つ`|`X`<br>` ` `さ`|`C`<br>` ` `そ`|`V`<br>` ` `ひ`|`B`<br>` ` `こ`||`N`<br>` ` `み`|`M`<br>` ` `も`|`<` `、`<br>`,` `ね`|`>` `。`<br>`.` `る`|`?` `・`<br>`/` `め`|**`_`**<br>**`\`** `ろ`|
{: .keyboard .with-groups}

**Japanese**: 45 letter-like entities, too many characters to count, 5
modes, 1 modifier.


## Summary of problems and solutions

So far, we have seen a problem and a few solutions. The problem
is, depending on who you ask:

* Those darn languages have way too many letters!
* Those short-sighted American keyboard designers gave us too few
  keys!

The solutions are:

* Add more keys. US ANSI has 47, European ISO, Japanese JIS and Korean
  Dubeolsik have 48, Brazilian ABNT has 49.
  * Does not scale. There is only so many places you can stick a new
    key and keep it relatively accessible. Even the ANSI 47 are
    already pushing the limit.
* Add a modifier. With `Shift` and 47 keys, can enter 94 characters.
  With `Shift` and `AltGr` and the same 47 keys, 141 characters. If
  you’re ok with pressing `Shift`+`AltGr`+key, 188 characters.
  * To get an `AltGr` on a classic keyboard, you have to give up one
    other modifer (commonly right `Alt`) and/or a combination of
    modifiers (on Windows, `Ctrl`+`Alt` also works as `AltGr`).
* Add modes. Russian has two modes (Latin and Cyrillic). Japanese has
  five or so (Latin halfwidth, Latin fullwidth, Hiragana, Katakana,
  Kanji conversion).
  * When your active mode doesn’t have some character you need once,
    you have to switch twice.


## How many keys can we actually press conveniently?

* A human hand has five fingers. Four, if not counting the thumb.
* This gives us four columns of convenient keys and two columns of
  slightly less convenient keys in the middle.
* Three rows are OK. The digit row is already a bit too far.
* Therefore, we have the zone of comfort spanning 30 keys.

|`Q`|`W`|`E`|`R`|`T`||`Y`|`U`|`I`|`O`|`P`|
|`A`|`S`|`D`|`F`|`G`||`H`|`J`|`K`|`L`|`:`<br>`;`|
|`Z`|`X`|`C`|`V`|`B`||`N`|`M`|`<`<br>`,`|`>`<br>`.`|`?`<br>`/`|
{: .keyboard}


### What if we tried to design a keyboard layout on these 30 keys?

* US English: 30 keys × (1 base + 2 modifiers) = 90 characters. Not
  enough for base ASCII.
* French, Russian: 30 unshifted keys are insufficient to cover all
  letters.
* Japanese: 30 unshifted keys are insufficient to cover all primary
  kana syllables. (However, a dead-key-like system could be devised,
  so that you first press the consonant and then the vowel, and they
  produce a syllable. That would require just 9 consonant keys, 5
  vowel keys, and 2 diacritic keys.)


### So what’s your point?

The 47 key layout is based on technological limitations of the past.
Namely, it was chosen because it is the minimum number of keys that
are sufficient for representing the 95 printable ASCII characters with
just one modifier.

In the Unicode age, both numbers (47 and 95) should be viewed as
arbitrary. We should admit that there is real need to represent more
than just ASCII, and design keyboards and usage patterns that are up
for the job.

The number and locations of keys are constrained by several factors:

* Lower bound: the number of letters or letter-like characters in the
  target language(s). For example, if a keyboard design wants to
  support the Japanese kana layout, it has to offer at least 48 keys
  in the alphanumeric zone, and they pretty much have to be in the
  standard places so as not to break the users’ muscle memory.
* Upper bound: the number of keys that can be pressed conveniently.
  That is, (the number of fingers per hand + [0…2] additional columns)
  × 3 rows × the number of target user’s hands. For normal healthy
  humans, this means around 36 keys in the main letter area.
* Modifier and editing keys: On conventional PC keyboards, the first
  and only left outer column is dedicated to Tab, Caps Lock and Shift,
  and there are editing keys as far as the third or fourth right outer
  column (Backspace). If we want to use outer columns for character
  input and avoid the proliferation of outer columns, we need to
  devise alternate locations for these. (Truly Ergonomic moves some
  editing keys to the center column, which corresponds to the second
  inner column on both hands. Axios provides a lot of thumb keys which
  can be used to offload outer columns.)
