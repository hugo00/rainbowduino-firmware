# Function Reference #

## Introduction ##

Rainbowduino firmware has a set of graphic functions that are associated to the command received via I2C.
The graphic subsystem consists in two main buffer, `FRONT_BUFFER` and `BACK_BUFFER`. There are also 4 auxiliary buffers available to the user.

Every graphic function works on the `BACK_BUFFER` or on one of the auxiliary buffers, while the hardware shows the `FRONT BUFFER`.  This method is used to prevent _filckering_ on the display.

Moreover, the action of swapping the two main buffers is done at the end of every refresh cycle.

## List of Functions ##

Functions available in the firmware code with the related Wire command used via I2C.

| **Name** | **Arguments** | **Purpose** |  | **Wire command** |
|:---------|:--------------|:------------|:-|:-----------------|
| `swapBuffers` |               |  swap front and back buffer in sync with refresh rate |  | `CMD_SWAP_BUF`   |
| `copyFrontBuffer` | `<x-offset>, <y-offset>` | copy the front buffer to the back one with the given offset |  | `CMD_COPY_FRONT_BUF` |
| `showAuxBuffer` | `<num>`       | show the buffer number `<num>` |  | `CMD_SHOW_AUX_BUF` |
| `storeToAuxBuffer` | `<buffer>, <num>` | copy the selected buffer to auxilary buffer `<num>` |  | _(not yet implemented)_ |
| `getFromAuxBuffer` | `<num>`       | copy the auxiliary buffer `<num>` to the back buffer |  | _(not yet implemented)_ |
| `setGamma` | `<index>`     | use the gamma ramp at index `<index>` in the `GammaTab` array |  | _(not yet implemented)_ |
| `clearBuffer` | `<r>, <g>, <b>` | clear the back buffer to the specified color |  | `CMD_CLEAR_BUFFER` |
| `drawPixel` | `<x-pos>, <y-pos>, <r>, <g>, <b>` | draw a pixel in the back buffer at given position |  | `CMD_DRAW_PIXEL` |
| `drawLine` | `<x-start>, <y-start>, <x-end>, <y-end>, <r>, <g>, <b>` | draw a line from starting point to the end one |  | `CMD_DRAW_LINE`  |
| `drawSquare` | `<x-start>, <y-start>, <x-end>, <y-end>, <r>, <g>, <b>` | draw a square giving the position of two opposite corners |  | `CMD_DRAW_SQUARE` |
| `printChar` | `<x-pos>, <y-pos>, <r>, <g>, <b>, <char>` | print the character `<char>`, using the internal font, at the given position |  | `CMD_PRINT_CHAR` |
| `drawRowMask` | `<row>, <x-offset>, <r>, <g>, <b>, <bit-mask>` | color the given row using a 8 bit mask |  | `CMD_DRAW_ROW_MASK` |

## List of Wire commands ##

Command available via I2C communication with the related firmware function (or variable).

| **Bit-code** | **Wire command** | **Parameters** | **Purpose** | | **Function** |
|:-------------|:-----------------|:---------------|:------------|:|:-------------|
| `0x00`       | `CMD_NOP`        |                | non-operating command (used internally) | | _(none)_     |
| `0x10`       | `CMD_SWAP_BUF`   |                |  swap front and back buffer in sync with refresh rate | | `swapBuffers` |
| `0x11`       | `CMD_COPY_FRONT_BUF` | `<x-offset>, <y-offset>` | copy the front buffer to the back one with the given offset | | `copyFrontBuffer` |
| `0x12`       | `CMD_SHOW_AUX_BUF` | `<num>`        | show the buffer number `<num>` | | `showAuxBuffer` |
| `0x20`       | `CMD_CLEAR_BUF`  | `<r>, <g>, <b>` | clear the back buffer to the specified color | | `clearBuffer ` |
| `0x21`       | `CMD_SET_PAPER`  | `<r>, <g>, <b>` | set the paper (background) color to be used by following commands | | `paperR, paperG, paperB` _(vars)_ |
| `0x22`       | `CMD_SET_INK`    | `<r>, <g>, <b>` | set the ink (foreground) color to be used by following commands | | `inkR, inkG, inkB` _(vars)_ |
| `0x25`       | `CMD_CLEAR_PAPER` |                | clear the back buffer in paper color given by a previous command | | `clearBuffer` |
| `0x26`       | `CMD_DRAW_PIXEL` | `<x-pos>, <y-pos>` | draw a pixel in the back buffer at given position in ink color | | `drawPixel`  |
| `0x27`       | `CMD_DRAW_LINE`  | `<x-start>, <y-start>, <x-end>, <y-end>` | draw a line from starting point to the end one in ink color | | `drawLine`   |
| `0x28`       | `CMD_DRAW_SQUARE` | `<x-start>, <y-start>, <x-end>, <y-end>` | draw a square giving the position of two opposite corners in ink color | | `drawSquare` |
| `0x2A`       | `CMD_PRINT_CHAR` | `<x-pos>, <y-pos>, <char>` | print the character `<char>`, using the internal font, at the given position in ink color | | `printChar`  |
| `0x2B`       | `CMD_DRAW_ROW_MASK` | `<row>, <x-offset>, <bit-mask>` | color, using ink values, the given row using a 8 bit mask | | `drawRowMask` |


_Updated to version v3.0h on June 9, 2010_