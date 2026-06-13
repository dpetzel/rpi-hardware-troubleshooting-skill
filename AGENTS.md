
## Board Index Pages
Each major board revision should have its own directory in references with a bare minimum
of a `README.md` which will serve as an index for other references

## Board Voltage Documentation
Each board should have a page for voltage references. We want distinct pages for
each major version variant (3B 3B +, etc) and revision. i.e. A 3B with two revisions
should have two different documents.

Each document should be reference in a `Voltages` section of the major versions
index doc. 

Within each voltage document we will want a pair of picture per situation/scenario.
Each document should start with a minimum of two sections:

```
## Bare Voltages
The voltages in this section represent a powered on board, with nothing
but the power attached. No display, no SD card inserted, no USB accessories
or ethernet.

## OS Idle
The OS idle scenario represents a powered on board sitting idle with the
operating system running. It is expected that 1 HDMI display is attached
on HDMI0, the SD card is inserted and the board is booted to the
dekstop/cli, but not running any load.
```

Each voltage document should have two images, one for the top of the board and
one for the bottom of the board. Each page should contain a legend of the color
codes:

| Color  | Hex Code  | Voltage |
| ------ | --------- | ------- |
| Black  | `#1a1a1a` | Ground  |
| Red    | `#e63226` | 5v      |
| Orange | `#f5a623` | 3.3v    |
| Teal   | `#008080` | 2.4/2.5v|
| Blue   | `#4a90d9` | 1.8v    |
| Pink   | `#FFB6C1` | 1.5v    |
| Purple | `#8F00FF` | 1.2v    |
| Brown  | `#895129` | 0.1v    |

These voltage maps will be an on-going evolution. Each voltage image is created
by starting with the board view image (top or bottom) opening that image in Gimp,
and then saving a `.xcf` and then exporting a `jpg`. The `.xcf` gives us the ability
to easily make edits, especially revisting previously placed annotations, while the
`jpg` will be used in the rendered views.
