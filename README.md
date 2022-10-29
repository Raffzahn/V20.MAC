# V20.MAC
Macro collection for V20/V30 programming in 8086 and 8080 mode using MASM 5 and up

---


## What Is This V20.MAC?

It's a collection of macros to support programming of the [NEC V20](https://en.wikipedia.org/wiki/NEC_V20)/30/... family of 8086 compatible CPUs. 


## What's a V20?

V20/V30 are drop in replacements for Intel 8088/86 CPUs but operate some 10 to 30% faster thru improved circuitry.

There was a significant hype around them in 1984/85 as they offered a very cost efficient way to upgrade not only the somewhat sluggish original IBM PC or PC-XT. Likewise welcome by Amstrad PC-1520/1640 owners who replaced their 8086 by a V20. Except some, notably the Siemens PC-MX Unix system, had system tests that detected them as 'bad CPU' due being faster than expected :))

Beside being faster V20/V30 also included
- all 80186 extensions (same as 286 real mode),
- additional V series specific instructions for bit manipulation and BCD operation
- an 8080 compatible emulation mode

While the 186 extensions are nice and the 8080 mode is offering the whole world of CP/M-80 applications at good speed seem the V20 extensions not really thought thru - more as if Management wanted to have something added that would help their position in an expected [lawsuit by Intel](https://en.wikipedia.org/wiki/NEC_V20#Lawsuit) - which eventually and brought us [in 1989](https://www.nytimes.com/1989/02/08/business/intel-loses-copyright-case-to-nec.html) the dreadful decision that copyright applies to code as well.

## The Libraries

### V20.MAC

V20.MAC is a file with macros to generate all V20 specific instructions (as of now except for most memory operands). These are for native (x86) mode

- Additional REP prefixes - must be placed in a source line of its own
   - `REPC` - Prefix for Repeat While Carry
   - `REPNC` - Prefix for Repeat While No Carry

- Bit Field Instructions
   - `EXTBF` (NEC:`EXT`) - Extract Bitfield From Memory to AX
   - `INSBF` (NEC:`INS`) - Insert Bitfield From AX to Memory
- Bit Manipulation Instructions (Only register source supported ATM)
   - `TEST1` - Test Bit for Set (1) in R/M 8/16 Indicated by CL or Immediate
   - `NOT1` - Invert Bit in R/M 8/16 Indicated by CL or Immediate
   - `CLR1` - Clear (->0) Bit in R/M 8/16 Indicated by CL or Immediate
   - `SET1` - Set (->1) Bit in R/M 8/16 Indicated by CL or Immediate
- BCD String Arithmetic
   - `ADD4S` - Add BCD Stings
   - `SUB4S` - Subtract BCD Strings
   - `CMP4S` - Compare BCD Strings
- BCD Rotates (Only register source supported ATM)
   - `ROL4` - Rotate Nibble Left From Register Thru AL
   - `ROR4` - Rotate Nibble Right From Register Thru AL
- Emulation Handling
   - `BRKEM` - Enter Emulation Mode

In addition the two new, V20 specific, emulation (8080) mode instructions are provided:

- `RETEM` - Return from Emulation
- `CALLN` - Call Native Code Vector

### V20<span>@</span>80.MAC

V20<span>@</span>80.MAC includes all emulation mode (8080) instructions plus the the emulation related native mode (8086) instruction (BRKEM).

- V20 specific native (8086) mode instructions:

   - `BRKEM` - Enter Emulation Mode

- V20 specific emulation (8080) mode instructions:

   - `RETEM` - Return from Emulation
   - `CALLN` - Call Native Code Vector

- 8080 instructions
   - All instructions as per [Intel 8080 Microcomputer Systems Users Manual](http://bitsavers.trailing-edge.com/components/intel/MCS80/98-153B_Intel_8080_Microcomputer_Systems_Users_Manual_197509.pdf) of September 1975, [p. 4-4...14](https://archive.org/details/bitsavers_intelMCS80ocomputerSystemsUsersManual197509_43049640/page/n47/mode/2up), summary at [page 4-15](https://archive.org/details/bitsavers_intelMCS80ocomputerSystemsUsersManual197509_43049640/page/n58/mode/1up?view=theater)

All 8080 instructions are prefixed by an `@` symbol to avoid conflict with 8086 instructions. Example:

- `LXI D,1234` -> `@LXI D,1234`
- `MOV A,C` -> `@MOV A,C`
- `CMP M` -> `@CMP M`
- etc.

Note that his is not Z80 notation, but original Intel 8080, which might be confusing to anyone new to them. For example incrementing the register pair of `DE` is noted as `INX D`. It should thus be rather simple to convert existing sources - or rediscover this, by today's standards weird mnemonics.


### Usage

Just include them as usual at the start of your program

````
         INCLUDE  V20.MAC                ; Include V20 instructions
         INCLUDE  V20@80.MAC             ; Include 8080 instructions
````

Emulation related instructions (BRKEM) are in both libraries.


## Why And Why Now (2022)?

In Sept 2022 I was doing some consulting for [640-KB's GlaBIOS](https://github.com/640-KB/GLaBIOS), which offers a V20 option to enable the use of the 'new' 80186 operations, as well as the V20 ones. More a nerdy detail than really useful, but why not. 640-KB did at that time code the V20 add-ons data which I feel is a rather ugly way. So I decided to write some simple macros to help.

Not much later [a question](https://retrocomputing.stackexchange.com/questions/25304/anyone-up-for-decompiling-some-8080-code-for-kaleidoscope) on [RetroComputing.SE](https://retrocomputing.stackexchange.com/) about disassembling the classic Kaleidoscope program came up which triggerd my nerd-sense, so I [spend a night on decoding it](https://github.com/Raffzahn/Kaleidoscope). Having already pulled out my old V20 fitted XT clone (to do some tests for GlaBIOS) it came just natural wanting to use it's 8080 mode to test the code on that PC using some fitting CGA mode.

All missing was a macro library to support emulation mode and 8080 coding while using Microsoft's MASM 5 ... and the rest is history ... or at least some typing and testing :))


## Further Reading

- [NEC V20 V30 User's Manual](http://bitsavers.trailing-edge.com/components/nec/V20_V30_Users_Manual_Oct86.pdf) of October 1986 (at Bitsavers)

- [1990 16-Bit V-Series Microprocessor Data Book](http://bitsavers.trailing-edge.com/components/nec/_dataBooks/1990_NEC_16-bit_V-Series_Microprocessor_Data_Book.pdf) of May 1990 (at Bitsavers). Incuded as well information about other x86 compatible versions.

- [1991 16-Bit V-Series Microprocessor Data Book](http://bitsavers.trailing-edge.com/components/nec/_dataBooks/1991_16_bit_V-Series_Microprocessor_Data_Book.pdf) of May 1991 (at Bitsavers).


## Files (so far)

- [V20.MAC](V20.MAC) -> Macros for all V20 specific instructions in native (x86) mode.
- [V20<span>@</span>80.MAC](V20@80.MAC) -> Macros for all 8080 instructions plus emulation mode invocation
- README.md -> This file

