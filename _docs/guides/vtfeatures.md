---
title: Supported VT features and terminal sequences
category: Guides
---




xterm.js version: 4.3.0

## Table of Contents

- [General notes](#general-notes)
- [C0](#c0)
- [C1](#c1)
- [CSI](#csi)
- [DCS](#dcs)
- [ESC](#esc)
- [OSC](#osc)

## General notes

This document lists xterm.js' support of terminal sequences. The sequences are grouped by their sequence type:

- C0: single byte command (7bit control codes, byte range \x00 .. \x1F, )
- C1: single byte command (8bit control codes, byte range \x80 .. \x9F)
- ESC: sequence starting with `ESC` (`\x1B`)
- CSI - Control Sequence Introducer: sequence starting with `ESC [` (7bit) or CSI (`\x9B` 8bit)
- DCS - Device Control String: sequence starting with `ESC P` (7bit) or DCS (`\x90` 8bit)
- OSC - Operating System Command: sequence starting with `ESC ]` (7bit) or OSC (`\x9D` 8bit)

Application Program Command (APC), Privacy Message (PM) and Start of String (SOS) are recognized but not supported,
any sequence of these types will be ignored. They are also not hookable by the API.

Note that the list only contains sequences implemented in xterm.js' core codebase. Missing sequences are either
not supported or unstable/experimental. Furthermore addons or integrations can provide additional custom sequences.

To denote the sequences the tables use the same abbreviations as xterm does:
- `Ps`: A single (usually optional) numeric parameter, composed of one or more decimal digits.
- `Pm`: A multiple numeric parameter composed of any number of single numeric parameters, separated by ; character(s),
  e.g. ` Ps ; Ps ; ... `.
- `Pt`: A text parameter composed of printable characters. Note that for most commands with `Pt` only
  ASCII printables are specified to work. Additionally the parser will let pass any codepoint greater than C1 as printable.


## C0

| Mnemonic | Name | Sequence | Short Description | Support |
| -------- | ---- | -------- | ----------------- | ------- |
| NUL | Null | `\0, \x00` | NUL is ignored.  | <span title="supported">✓</span> |
| BEL | Bell | `\a, \x07` | Ring the bell. _[more](#bell)_ | <span title="supported">✓</span> |
| BS | Backspace | `\b, \x08` | Move the cursor one position to the left.  | <span title="supported">✓</span> |
| HT | Horizontal Tabulation | `\t, \x09` | Move the cursor to the next character tab stop.  | <span title="supported">✓</span> |
| LF | Line Feed | `\n, \x0A` | Move the cursor one row down, scrolling if needed. _[more](#line-feed)_ | <span title="supported">✓</span> |
| VT | Vertical Tabulation | `\v, \x0B` | Treated as LF.  | <span title="supported">✓</span> |
| FF | Form Feed | `\f, \x0C` | Treated as LF.  | <span title="supported">✓</span> |
| CR | Carriage Return | `\r, \x0D` | Move the cursor to the beginning of the row.  | <span title="supported">✓</span> |
| SO | Shift Out | `\x0E` | Switch to an alternative character set.  | <span title="Only limited ISO-2022 charset support.">Partial</span> |
| SI | Shift In | `\x0F` | Return to regular character set after Shift Out.  | <span title="supported">✓</span> |
| ESC | Escape | `\e, \x1B` | Start of a sequence. Cancels any other sequence.  | <span title="supported">✓</span> |

### Bell
The behavior of the bell is further customizable with `ITerminalOptions.bellStyle`
and `ITerminalOptions.bellSound`.

### Line Feed
Scrolling is restricted to scroll margins and will only happen on the bottom line.




## C1

| Mnemonic | Name | Sequence | Short Description | Support |
| -------- | ---- | -------- | ----------------- | ------- |
| IND | Index | `\x84` | Move the cursor one line down scrolling if needed.  | <span title="supported">✓</span> |
| NEL | Next Line | `\x85` | Move the cursor to the beginning of the next row.  | <span title="supported">✓</span> |
| HTS | Horizontal Tabulation Set | `\x88` | Places a tab stop at the current cursor position.  | <span title="supported">✓</span> |
| DCS | Device Control String | `\x90` | Start of a DCS sequence.  | <span title="supported">✓</span> |
| CSI | Control Sequence Introducer | `\x9B` | Start of a CSI sequence.  | <span title="supported">✓</span> |
| ST | String Terminator | `\x9C` | Terminator used for string type sequences.  | <span title="supported">✓</span> |
| OSC | Operating System Command | `\x9D` | Start of an OSC sequence.  | <span title="supported">✓</span> |
| PM | Privacy Message | `\x9E` | Start of a privacy message.  | <span title="supported">✓</span> |
| APC | Application Program Command | `\x9F` | Start of an APC sequence.  | <span title="supported">✓</span> |




## CSI

| Mnemonic | Name | Sequence | Short Description | Support |
| -------- | ---- | -------- | ----------------- | ------- |
| ICH | Insert Characters | ``CSI Ps @`` | Insert `Ps` (blank) characters (default = 1). _[more](#insert-characters)_ | <span title="supported">✓</span> |
| SL | Scroll Left | ``CSI Ps SP @`` | Scroll viewport `Ps` times to the left. _[more](#scroll-left)_ | <span title="supported">✓</span> |
| CUU | Cursor Up | ``CSI Ps A`` | Move cursor `Ps` times up (default=1). _[more](#cursor-up)_ | <span title="supported">✓</span> |
| SR | Scroll Right | ``CSI Ps SP A`` | Scroll viewport `Ps` times to the right. _[more](#scroll-right)_ | <span title="supported">✓</span> |
| CUD | Cursor Down | ``CSI Ps B`` | Move cursor `Ps` times down (default=1). _[more](#cursor-down)_ | <span title="supported">✓</span> |
| CUF | Cursor Forward | ``CSI Ps C`` | Move cursor `Ps` times forward (default=1).  | <span title="supported">✓</span> |
| CUB | Cursor Backward | ``CSI Ps D`` | Move cursor `Ps` times backward (default=1).  | <span title="supported">✓</span> |
| CNL | Cursor Next Line | ``CSI Ps E`` | Move cursor `Ps` times down (default=1) and to the first column. _[more](#cursor-next-line)_ | <span title="supported">✓</span> |
| CPL | Cursor Backward | ``CSI Ps F`` | Move cursor `Ps` times up (default=1) and to the first column. _[more](#cursor-backward)_ | <span title="supported">✓</span> |
| CHA | Cursor Horizontal Absolute | ``CSI Ps G`` | Move cursor to `Ps`-th column of the active row (default=1).  | <span title="supported">✓</span> |
| CUP | Cursor Position | ``CSI Ps ; Ps H`` | Set cursor to position [`Ps`, `Ps`] (default = [1, 1]). _[more](#cursor-position)_ | <span title="supported">✓</span> |
| CHT | Cursor Horizontal Tabulation | ``CSI Ps I`` | Move cursor `Ps` times tabs forward (default=1).  | <span title="supported">✓</span> |
| DECSED | Selective Erase In Display | ``CSI ? Ps J`` | Currently the same as ED.  | <span title="Protection attributes are not supported.">Partial</span> |
| ED | Erase In Display | ``CSI Ps J`` | Erase various parts of the viewport. _[more](#erase-in-display)_ | <span title="supported">✓</span> |
| DECSEL | Selective Erase In Line | ``CSI ? Ps K`` | Currently the same as EL.  | <span title="Protection attributes are not supported.">Partial</span> |
| EL | Erase In Line | ``CSI Ps K`` | Erase various parts of the active row. _[more](#erase-in-line)_ | <span title="supported">✓</span> |
| IL | Insert Line | ``CSI Ps L`` | Insert `Ps` blank lines at active row (default=1). _[more](#insert-line)_ | <span title="supported">✓</span> |
| DL | Delete Line | ``CSI Ps M`` | Delete `Ps` lines at active row (default=1). _[more](#delete-line)_ | <span title="supported">✓</span> |
| DCH | Delete Character | ``CSI Ps P`` | Delete `Ps` characters (default=1). _[more](#delete-character)_ | <span title="supported">✓</span> |
| SU | Scroll Up | ``CSI Ps S`` | Scroll `Ps` lines up (default=1).  | <span title="supported">✓</span> |
| SD | Scroll Down | ``CSI Ps T`` | Scroll `Ps` lines down (default=1).  | <span title="supported">✓</span> |
| ECH | Erase Character | ``CSI Ps X`` | Erase `Ps` characters from current cursor position to the right (default=1). _[more](#erase-character)_ | <span title="supported">✓</span> |
| CBT | Cursor Backward Tabulation | ``CSI Ps Z`` | Move cursor `Ps` tabs backward (default=1).  | <span title="supported">✓</span> |
| HPA | Horizontal Position Absolute | ``CSI Ps ` `` | Same as CHA.  | <span title="supported">✓</span> |
| HPR | Horizontal Position Relative | ``CSI Ps a`` | Same as CUF.  | <span title="supported">✓</span> |
| REP | Repeat Preceding Character | ``CSI Ps b`` | Repeat preceding character `Ps` times (default=1). _[more](#repeat-preceding-character)_ | <span title="supported">✓</span> |
| DA1 | Primary Device Attributes | ``CSI c`` | Send primary device attributes.  | <span title="supported">✓</span> |
| DA2 | Secondary Device Attributes | ``CSI > c`` | Send primary device attributes.  | <span title="supported">✓</span> |
| VPA | Vertical Position Absolute | ``CSI Ps d`` | Move cursor to `Ps`-th row (default=1).  | <span title="supported">✓</span> |
| VPR | Vertical Position Relative | ``CSI Ps e`` | Move cursor `Ps` times down (default=1).  | <span title="supported">✓</span> |
| HVP | Horizontal and Vertical Position | ``CSI Ps ; Ps f`` | Same as CUP.  | <span title="supported">✓</span> |
| TBC | Tab Clear | ``CSI Ps g`` | Clear tab stops at current position (0) or all (3) (default=0). _[more](#tab-clear)_ | <span title="supported">✓</span> |
| SM | Set Mode | ``CSI Pm h`` | Set various terminal modes. _[more](#set-mode)_ | <span title="Only IRM is supported.">Partial</span> |
| DECSET | DEC Private Set Mode | ``CSI ? Pm h`` | Set various terminal attributes. _[more](#dec-private-set-mode)_ | <span title="See below for supported modes.">Partial</span> |
| RM | Reset Mode | ``CSI Pm l`` | Set various terminal attributes. _[more](#reset-mode)_ | <span title="Only IRM is supported.">Partial</span> |
| DECRST | DEC Private Reset Mode | ``CSI ? Pm l`` | Reset various terminal attributes. _[more](#dec-private-reset-mode)_ | <span title="See below for supported modes.">Partial</span> |
| SGR | Select Graphic Rendition | ``CSI Pm m`` | Set/Reset various text attributes. _[more](#select-graphic-rendition)_ | <span title="See below for supported attributes.">Partial</span> |
| DSR | Device Status Report | ``CSI Ps n`` | Request cursor position (CPR) with `Ps` = 6.  | <span title="supported">✓</span> |
| DECDSR | DEC Device Status Report | ``CSI ? Ps n`` | Only CPR is supported (same as DSR).  | <span title="Only CPR is supported.">Partial</span> |
| DECSTR | Soft Terminal Reset | ``CSI ! p`` | Reset several terminal attributes to initial state. _[more](#soft-terminal-reset)_ | <span title="supported">✓</span> |
| DECSCUSR | Set Cursor Style | ``CSI Ps SP q`` | Set cursor style. _[more](#set-cursor-style)_ | <span title="supported">✓</span> |
| DECSTBM | Set Top and Bottom Margin | ``CSI Ps ; Ps r`` | Set top and bottom margins of the viewport [top;bottom] (default = viewport size).  | <span title="supported">✓</span> |
| SCOSC | Save Cursor | ``CSI s`` | Save cursor position, charmap and text attributes.  | <span title="TODO...">Partial</span> |
| SCORC | Restore Cursor | ``CSI u`` | Restore cursor position, charmap and text attributes.  | <span title="TODO...">Partial</span> |
| DECIC | Insert Columns | ``CSI Ps ' }`` | Insert `Ps` columns at cursor position. _[more](#insert-columns)_ | <span title="supported">✓</span> |
| DECDC | Delete Columns | ``CSI Ps ' ~`` | Delete `Ps` columns at cursor position. _[more](#delete-columns)_ | <span title="supported">✓</span> |

### Insert Characters
The ICH sequence inserts `Ps` blank characters. The cursor remains at the beginning of the blank characters.
Text between the cursor and right margin moves to the right. Characters moved past the right margin are lost.

### Scroll Left
SL moves the content of all lines within the scroll margins `Ps` times to the left.
SL has no effect outside of the scroll margins.

### Cursor Up
If the cursor would pass the top scroll margin, it will stop there.

### Scroll Right
SL moves the content of all lines within the scroll margins `Ps` times to the right.
Content at the right margin is lost.
SL has no effect outside of the scroll margins.

### Cursor Down
If the cursor would pass the bottom scroll margin, it will stop there.

### Cursor Next Line
Same as CUD, additionally places the cursor at the first column.

### Cursor Backward
Same as CUU, additionally places the cursor at the first column.

### Cursor Position
If ORIGIN mode is set, places the cursor to the absolute position within the scroll margins.
If ORIGIN mode is not set, places the cursor to the absolute position within the viewport.
Note that the coordinates are 1-based, thus the top left position starts at `1 ; 1`.

### Erase In Display
Supported param values:

| Ps | Effect                                                       |
| -- | ------------------------------------------------------------ |
| 0  | Erase from the cursor through the end of the viewport.       |
| 1  | Erase from the beginning of the viewport through the cursor. |
| 2  | Erase complete viewport.                                     |
| 3  | Erase scrollback.                                            |

### Erase In Line
Supported param values:

| Ps | Effect                                                   |
| -- | -------------------------------------------------------- |
| 0  | Erase from the cursor through the end of the row.        |
| 1  | Erase from the beginning of the line through the cursor. |
| 2  | Erase complete line.                                     |

### Insert Line
For every inserted line at the scroll top one line at the scroll bottom gets removed.
The cursor is set to the first column.
IL has no effect if the cursor is outside the scroll margins.

### Delete Line
For every deleted line at the scroll top one blank line at the scroll bottom gets appended.
The cursor is set to the first column.
DL has no effect if the cursor is outside the scroll margins.

### Delete Character
As characters are deleted, the remaining characters between the cursor and right margin move to the left.
Character attributes move with the characters. The terminal adds blank characters at the right margin.

### Erase Character
ED erases `Ps` characters from current cursor position to the right.
ED works inside or outside the scrolling margins.

### Repeat Preceding Character
REP repeats the previous character `Ps` times advancing the cursor, also wrapping if DECAWM is set.
REP has no effect if the sequence does not follow a printable ASCII character
(NOOP for any other sequence in between or NON ASCII characters).

### Tab Clear
Clearing tabstops off the active row (Ps = 2, VT100) is currently not supported.

### Set Mode
Supported param values by SM:

| Param | Action                                 | Support |
| ----- | -------------------------------------- | ------- |
| 2     | Keyboard Action Mode (KAM). Always on. | <span title="unsupported">✗</span>      |
| 4     | Insert Mode (IRM).                     | <span title="supported">✓</span>      |
| 12    | Send/receive (SRM). Always off.        | <span title="unsupported">✗</span>      |
| 20    | Automatic Newline (LNM). Always off.   | <span title="unsupported">✗</span>      |

### DEC Private Set Mode
Supported param values by DECSET:

| param | Action                                                  | Support |
| ----- | ------------------------------------------------------- | --------|
| 1     | Application Cursor Keys (DECCKM).                       | <span title="supported">✓</span>      |
| 2     | Designate US-ASCII for character sets G0-G3 (DECANM).   | <span title="supported">✓</span>      |
| 3     | 132 Column Mode (DECCOLM).                              | <span title="supported">✓</span>      |
| 6     | Origin Mode (DECOM).                                    | <span title="supported">✓</span>      |
| 7     | Auto-wrap Mode (DECAWM).                                | <span title="supported">✓</span>      |
| 8     | Auto-repeat Keys (DECARM). Always on.                   | <span title="unsupported">✗</span>      |
| 9     | X10 xterm mouse protocol.                               | <span title="supported">✓</span>      |
| 12    | Start Blinking Cursor.                                  | <span title="supported">✓</span>      |
| 25    | Show Cursor (DECTCEM).                                  | <span title="supported">✓</span>      |
| 47    | Use Alternate Screen Buffer.                            | <span title="supported">✓</span>      |
| 66    | Application keypad (DECNKM).                            | <span title="supported">✓</span>      |
| 1000  | X11 xterm mouse protocol.                               | <span title="supported">✓</span>      |
| 1002  | Use Cell Motion Mouse Tracking.                         | <span title="supported">✓</span>      |
| 1003  | Use All Motion Mouse Tracking.                          | <span title="supported">✓</span>      |
| 1004  | Send FocusIn/FocusOut events                            | <span title="supported">✓</span>      |
| 1005  | Enable UTF-8 Mouse Mode.                                | <span title="unsupported">✗</span>      |
| 1006  | Enable SGR Mouse Mode.                                  | <span title="supported">✓</span>      |
| 1015  | Enable urxvt Mouse Mode.                                | <span title="unsupported">✗</span>      |
| 1047  | Use Alternate Screen Buffer.                            | <span title="supported">✓</span>      |
| 1048  | Save cursor as in DECSC.                                | <span title="supported">✓</span>      |
| 1049  | Save cursor and switch to alternate buffer clearing it. | <span title="Does not clear the alternate buffer.">Partial</span> |
| 2004  | Set bracketed paste mode.                               | <span title="supported">✓</span>      |

### Reset Mode
Supported param values by RM:

| Param | Action                                 | Support |
| ----- | -------------------------------------- | ------- |
| 2     | Keyboard Action Mode (KAM). Always on. | <span title="unsupported">✗</span>      |
| 4     | Replace Mode (IRM). (default)          | <span title="supported">✓</span>      |
| 12    | Send/receive (SRM). Always off.        | <span title="unsupported">✗</span>      |
| 20    | Normal Linefeed (LNM). Always off.     | <span title="unsupported">✗</span>      |

### DEC Private Reset Mode
Supported param values by DECRST:

| param | Action                                                  | Support |
| ----- | ------------------------------------------------------- | ------- |
| 1     | Normal Cursor Keys (DECCKM).                            | <span title="supported">✓</span>      |
| 2     | Designate VT52 mode (DECANM).                           | <span title="unsupported">✗</span>      |
| 3     | 80 Column Mode (DECCOLM).                               | <span title="Switches to old column width instead of 80.">Broken</span> |
| 6     | Normal Cursor Mode (DECOM).                             | <span title="supported">✓</span>      |
| 7     | No Wraparound Mode (DECAWM).                            | <span title="supported">✓</span>      |
| 8     | No Auto-repeat Keys (DECARM).                           | <span title="unsupported">✗</span>      |
| 9     | Don't send Mouse X & Y on button press.                 | <span title="supported">✓</span>      |
| 12    | Stop Blinking Cursor.                                   | <span title="supported">✓</span>      |
| 25    | Hide Cursor (DECTCEM).                                  | <span title="supported">✓</span>      |
| 47    | Use Normal Screen Buffer.                               | <span title="supported">✓</span>      |
| 66    | Numeric keypad (DECNKM).                                | <span title="supported">✓</span>      |
| 1000  | Don't send Mouse reports.                               | <span title="supported">✓</span>      |
| 1002  | Don't use Cell Motion Mouse Tracking.                   | <span title="supported">✓</span>      |
| 1003  | Don't use All Motion Mouse Tracking.                    | <span title="supported">✓</span>      |
| 1004  | Don't send FocusIn/FocusOut events.                     | <span title="supported">✓</span>      |
| 1005  | Disable UTF-8 Mouse Mode.                               | <span title="unsupported">✗</span>      |
| 1006  | Disable SGR Mouse Mode.                                 | <span title="supported">✓</span>      |
| 1015  | Disable urxvt Mouse Mode.                               | <span title="unsupported">✗</span>      |
| 1047  | Use Normal Screen Buffer (clearing screen if in alt).   | <span title="supported">✓</span>      |
| 1048  | Restore cursor as in DECRC.                             | <span title="supported">✓</span>      |
| 1049  | Use Normal Screen Buffer and restore cursor.            | <span title="supported">✓</span>      |
| 2004  | Reset bracketed paste mode.                             | <span title="supported">✓</span>      |

### Select Graphic Rendition
SGR selects one or more character attributes at the same time. Multiple params (up to 32)
are applied from in order from left to right. The changed attributes are applied to all new
characters received. If you move characters in the viewport by scrolling or any other means,
then the attributes move with the characters.

Supported param values by SGR:

| Param     | Meaning                                                  | Support |
| --------- | -------------------------------------------------------- | ------- |
| 0         | Normal (default). Resets any other preceding SGR.        | <span title="supported">✓</span>      |
| 1         | Bold. (also see `options.drawBoldTextInBrightColors`)    | <span title="supported">✓</span>      |
| 2         | Faint, decreased intensity.                              | <span title="supported">✓</span>      |
| 3         | Italic.                                                  | <span title="supported">✓</span>      |
| 4         | Underlined. (no support for newer underline styles)      | <span title="supported">✓</span>      |
| 5         | Slowly blinking.                                         | <span title="unsupported">✗</span>      |
| 6         | Rapidly blinking.                                        | <span title="unsupported">✗</span>      |
| 7         | Inverse. Flips foreground and background color.          | <span title="supported">✓</span>      |
| 8         | Invisible (hidden).                                      | <span title="supported">✓</span>      |
| 9         | Crossed-out characters.                                  | <span title="unsupported">✗</span>      |
| 21        | Doubly  underlined.                                      | <span title="unsupported">✗</span>      |
| 22        | Normal (neither bold nor faint).                         | <span title="supported">✓</span>      |
| 23        | No italic.                                               | <span title="supported">✓</span>      |
| 24        | Not underlined.                                          | <span title="supported">✓</span>      |
| 25        | Steady (not blinking).                                   | <span title="supported">✓</span>      |
| 27        | Positive (not inverse).                                  | <span title="supported">✓</span>      |
| 28        | Visible (not hidden).                                    | <span title="supported">✓</span>      |
| 29        | Not Crossed-out.                                         | <span title="unsupported">✗</span>      |
| 30        | Foreground color: Black.                                 | <span title="supported">✓</span>      |
| 31        | Foreground color: Red.                                   | <span title="supported">✓</span>      |
| 32        | Foreground color: Green.                                 | <span title="supported">✓</span>      |
| 33        | Foreground color: Yellow.                                | <span title="supported">✓</span>      |
| 34        | Foreground color: Blue.                                  | <span title="supported">✓</span>      |
| 35        | Foreground color: Magenta.                               | <span title="supported">✓</span>      |
| 36        | Foreground color: Cyan.                                  | <span title="supported">✓</span>      |
| 37        | Foreground color: White.                                 | <span title="supported">✓</span>      |
| 38        | Foreground color: Extended color.                        | <span title="Support for RGB and indexed colors, see below.">Partial</span> |
| 39        | Foreground color: Default (original).                    | <span title="supported">✓</span>      |
| 40        | Background color: Black.                                 | <span title="supported">✓</span>      |
| 41        | Background color: Red.                                   | <span title="supported">✓</span>      |
| 42        | Background color: Green.                                 | <span title="supported">✓</span>      |
| 43        | Background color: Yellow.                                | <span title="supported">✓</span>      |
| 44        | Background color: Blue.                                  | <span title="supported">✓</span>      |
| 45        | Background color: Magenta.                               | <span title="supported">✓</span>      |
| 46        | Background color: Cyan.                                  | <span title="supported">✓</span>      |
| 47        | Background color: White.                                 | <span title="supported">✓</span>      |
| 48        | Background color: Extended color.                        | <span title="Support for RGB and indexed colors, see below.">Partial</span> |
| 49        | Background color: Default (original).                    | <span title="supported">✓</span>      |
| 90 - 97   | Bright foreground color (analogous to 30 - 37).          | <span title="supported">✓</span>      |
| 100 - 107 | Bright background color (analogous to 40 - 47).          | <span title="supported">✓</span>      |

Extended colors are supported for foreground (Ps=38) and background (Ps=48) as follows:

| Ps + 1 | Meaning                                                       | Support |
| ------ | ------------------------------------------------------------- | ------- |
| 0      | Implementation defined.                                       | <span title="unsupported">✗</span>      |
| 1      | Transparent.                                                  | <span title="unsupported">✗</span>      |
| 2      | RGB color as `Ps ; 2 ; R ; G ; B` or `Ps : 2 : : R : G : B`.  | <span title="supported">✓</span>      |
| 3      | CMY color.                                                    | <span title="unsupported">✗</span>      |
| 4      | CMYK color.                                                   | <span title="unsupported">✗</span>      |
| 5      | Indexed (256 colors) as `Ps ; 5 ; INDEX` or `Ps : 5 : INDEX`. | <span title="supported">✓</span>      |

### Soft Terminal Reset
There are two terminal reset sequences - RIS and DECSTR. While RIS performs almost a full terminal bootstrap,
DECSTR only resets certain attributes. For most needs DECSTR should be sufficient.

The following terminal attributes are reset to default values:
- cursor is reset (default = visible, home position)
- IRM is reset (dafault = false)
- scroll margins are reset (default = viewport size)
- erase attributes are reset to default
- charsets are reset

### Set Cursor Style
Supported cursor styles:
 - empty, 0 or 1: steady block
 - 2: blink block
 - 3: steady underline
 - 4: blink underline
 - 5: steady bar
 - 6: blink bar

### Insert Columns
DECIC inserts `Ps` times blank columns at the cursor position for all lines with the scroll margins,
moving content to the right. Content at the right margin is lost.
DECIC has no effect outside the scrolling margins.

### Delete Columns
DECDC deletes `Ps` times columns at the cursor position for all lines with the scroll margins,
moving content to the left. Blank columns are added at the right margin.
DECDC has no effect outside the scrolling margins.




## DCS

| Mnemonic | Name | Sequence | Short Description | Support |
| -------- | ---- | -------- | ----------------- | ------- |
| DECRQSS | Request Selection or Setting | `DCS $ q Pt ST` | Request several terminal settings. _[more](#request-selection-or-setting)_ | <span title="See limited support below.">Partial</span> |
| DECUDK | User Defined Keys | `DCS Ps ; Ps | Pt ST` | Definitions for user-defined keys.  | <span title="unsupported">✗</span> |
| SIXEL | SIXEL Graphics | `DCS Ps ; Ps ; Ps ; q 	Pt ST` | Draw SIXEL image starting at cursor position.  | <span title="unsupported">✗</span> |
| XTGETTCAP | Request Terminfo String | `DCS + q Pt ST` | Request Terminfo String.  | <span title="unsupported">✗</span> |
| XTSETTCAP | Set Terminfo Data | `DCS + p Pt ST` | Set Terminfo Data.  | <span title="unsupported">✗</span> |

### Request Selection or Setting
Response is in the form `ESC P 1 $ r Pt ST` for valid requests, where `Pt` contains the corresponding CSI string,
`ESC P 0 ST` for invalid requests.

Supported requests and responses:

| Type                             | Request           | Response (`Pt`)                                       |
| -------------------------------- | ----------------- | ----------------------------------------------------- |
| Graphic Rendition (SGR)          | `DCS $ q m ST`    | always reporting `0m` (currently broken)              |
| Top and Bottom Margins (DECSTBM) | `DCS $ q r ST`    | `Ps ; Ps r`                                           |
| Cursor Style (DECSCUSR)          | `DCS $ q SP q ST` | `Ps SP q`                                             |
| Protection Attribute (DECSCA)    | `DCS $ q " q ST`  | always reporting `0 " q` (DECSCA is unsupported)      |
| Conformance Level (DECSCL)       | `DCS $ q " p ST`  | always reporting `61 ; 1 " p` (DECSCL is unsupported) |




## ESC

| Mnemonic | Name | Sequence | Short Description | Support |
| -------- | ---- | -------- | ----------------- | ------- |
| SC | Save Cursor | `ESC 7` | Save cursor position, charmap and text attributes.  | <span title="supported">✓</span> |
| DECALN | Screen Alignment Pattern | `ESC # 8` | Fill viewport with a test pattern (E).  | <span title="supported">✓</span> |
| RC | Restore Cursor | `ESC 8` | Restore cursor position, charmap and text attributes.  | <span title="supported">✓</span> |
| IND | Index | `ESC D` | Move the cursor one line down scrolling if needed.  | <span title="supported">✓</span> |
| NEL | Next Line | `ESC E` | Move the cursor to the beginning of the next row.  | <span title="supported">✓</span> |
| HTS | Horizontal Tabulation Set | `ESC H` | Places a tab stop at the current cursor position.  | <span title="supported">✓</span> |
| IR | Reverse Index | `ESC M` | Move the cursor one line up scrolling if needed.  | <span title="supported">✓</span> |
| DCS | Device Control String | `ESC P` | Start of a DCS sequence.  | <span title="supported">✓</span> |
| CSI | Control Sequence Introducer | `ESC [` | Start of a CSI sequence.  | <span title="supported">✓</span> |
| ST | String Terminator | `ESC \` | Terminator used for string type sequences.  | <span title="supported">✓</span> |
| OSC | Operating System Command | `ESC ]` | Start of an OSC sequence.  | <span title="supported">✓</span> |
| PM | Privacy Message | `ESC ^` | Start of a privacy message.  | <span title="supported">✓</span> |
| APC | Application Program Command | `ESC _` | Start of an APC sequence.  | <span title="supported">✓</span> |




## OSC

**Note**: Other than listed in the table, the parser recognizes both ST (ECMA-48) and BEL (xterm) as OSC sequence finalizer.

| Identifier | Sequence | Short Description | Support |
| ---------- | -------- | ----------------- | ------- |
| 0 | `OSC 0 ; Pt BEL` | Set window title and icon name. _[more](#set-windows-title-and-icon-name)_ | <span title="Icon name is not exposed.">Partial</span> |
| 1 | `OSC 1 ; Pt BEL` | Set icon name.  | <span title="unsupported">✗</span> |
| 2 | `OSC 2 ; Pt BEL` | Set window title. _[more](#set-windows-title)_ | <span title="supported">✓</span> |

### Set Windows Title and Icon Name
Icon name is not supported. For Window Title see below.

### Set Windows Title
xterm.js does not manipulate the title directly, instead exposes changes via the event `Terminal.onTitleChange`.



<script type="text/javascript">
  setTimeout(() => alert('test'), 1000);
</script>
