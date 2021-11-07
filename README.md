# Z80 PUC

![Screenshot](/img/screenshot_3d_00.png)

Z80 PUC is a single-board Z80-based computer for embedded applications.

It's designed with all through-hole components for easy soldering and hacking,
as well as increased retro-cool factor. Despite its retro feel, all of its parts
are still readily available and in production as of writing this.

The goal is to have a Z80 "module" than can be quickly modified and then added
to a project instead of having to design and order a custom PCB every time.
Kind of like using a Raspberry Pi instead of designing your own ARM-based
computer for every project.

The "PUC" in Z80 **PUC** stands for "**P**revious **U**nit of **C**omputing".
Since the x86 CPUs in the Intel **NUC** (**N**ext **U**nit of **C**omputing) and
the Z80 both have a common ancestor (the Intel 8080 CPU), and both the NUC and
the Z80 PUC are small form factor computers, I thought it would be funny to call
them something similar.

# Memory map

## Memory

| From    | To      | Size | Use             |
|---------|---------|------|-----------------|
| `$0000` | `$1FFF` | 8K   | ROM             |
| `$2000` | `$3FFF` | 8K   | Banked Area RAM |
| `$4000` | `$7FFF` | 16K  | Bus Expansion   |
| `$8000` | `$FFFF` | 32K  | Static Area RAM |

## RAM Banks

The first 4 bits of Port B on the PIO select one out of 16 banks of 8K. The
static RAM area (`$8000`-`$FFFF`) is made up of 4 8K banks (`$C`-`$F`).

The total amount of addressable RAM built-in is 16x8K = 128K

## I/O

- The CTC uses I/O addresses `$00`-`$0F`.
- The PIO uses I/O addresses `$10`-`$1F`.

# Design choices

## Why a PIO instead of an SIO for interfacing with peripherals?

- The PIO gives us an additional 8 GPIO pins to play around with, 4 of which are
used for memory bank selection, and 1 for the on-board LED. Implementing those
without a PIO would increase component count, so since we're including it
regardless, might as well use it.
- Transferring 8 bits at a time is probably faster than serial, and if pin count
really is at a premium on your peripheral's microcontroller, you can still send
fewer bits at a time.
- The parallel interface works regardless of the peripheral's clock speed.
- It's what I already had lying around in the parts bin.

## Where are the CTC TRG and output pins?

I just included the CTC for timer interrupts. I've never needed any of the CTC's
other functionality, so I decided to save some space and not include a pin
header for that. If you really need it, you can just solder a few wires directly
to the board, or add an external CTC chip.

# License

Z80 PUC (c) by Ramy Tarchichy

Z80 PUC is licensed under a
Creative Commons Attribution-ShareAlike 4.0 International License.

You should have received a copy of the license along with this
work. If not, see [http://creativecommons.org/licenses/by-sa/4.0/](http://creativecommons.org/licenses/by-sa/4.0/).
