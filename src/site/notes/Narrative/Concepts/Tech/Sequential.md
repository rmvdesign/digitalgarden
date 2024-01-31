---
{"aliases":["knitting"],"tags":null,"dg-publish":true,"permalink":"/narrative/concepts/tech/sequential/","dgPassFrontmatter":true}
---

# Sequential

Sequential is a dynamic, hybrid programming language tailored to the distinct hardware architecture of Semi-Linear Computers (SLCs). Developed by [[Narrative/Characters/WB Characters/Theo Rose\|Theodore Rose]] in year -3 to interact with the emerging sector of digital computers, Sequential represents a watershed in the evolution of programming, merging traditional linear code with the fluidity of analogue systems. Renowned for empowering programmers to 'knit' sequences of operations in a manner akin to setting up a complex musical synthesiser, Sequential forms the core software expression mode for SLCs across the galaxy.
## Background

Built to interface seamlessly with the revolutionary storage capacities of [[Narrative/Concepts/Tech/Magnetic Autotape\|Magnetic Autotape]] and to manipulate both digital and analogue information, Sequential was conceived in the summer estate of the Rose family by a small team of enthusiasts led by the visionary Theodore Rose. As SLCs began to proliferate, the need for a compatible programming language grew. In response, Sequential was crafted to bridge the gap between the computational models of the time and the advanced capacities of new Semi-Linear storage and processing systems.

## Design and Features

**Language Name:** Sequential
**Developer:** Theodore Rose
**First Appeared:** Year -3
**Typing Discipline:** Hybrid
**Platform:** Semi-Linear Computers (SLCs), Theotech MachineMind SLCs
**Word [[Narrative/Concepts/Tech/Rationale\|Rationale]]:** `LOGO`

Sequential takes advantage of the inherent flexibility offered by SLC hardware, specifically catered to use the [[Narrative/Concepts/Tech/Magnetic Autotape\|Magnetic Autotape]]'s ability to concurrently store and process variable-length 'words' of both analogue and digital data. Intended to facilitate a smooth translation between the binary rigidity of digital code and the rich variability of analogue signals, Sequential stands as the primary tool for navigating deep [[Narrative/Concepts/Tech/Magnetic Autotape\|Autotape]] layering and SLC functionality.

## Sequence Knitting

A key innovation brought forth by Sequential is Sequence Knitting, a tactile programming methodology that reflects the craftsmanship ethos deeply embedded in SLC culture. Programmers 'knit' together operations by physically adjusting sequences on an interactive panel complete with knobs, dials, and a step sequencer-like code matrix, similar to playing an instrument. This hands-on approach allows programmers to compose and execute intricate sequences, opening new dimensions for how software is articulated and thought of. Sequence Knitting requires the use of a Knitting Board, a physical programming and debugging device available for purchase from [[Narrative/Factions/Corporations/Theotech, LLC\|Theotech]]. Sequential can also be written by hand as direct line instructions, but this method is much more time-consuming and often produces unoptimised code.

## Syntax and Semantics

Syntax in Sequential is intentionally minimalist, drawing on the philosophies of early programming languages, fused with specialised controls for engaging with the SLC's tape-based sequencing. Each sequence is transcribed into line instructions, which the SLC executes by interpreting the depth-positioned data on the [[Narrative/Concepts/Tech/Magnetic Autotape\|tape]]. Common programming paradigms converge in Sequential, with procedural and functional elements underpinning its Analog-Digital Hybrid nature.

## In Practice

Sequential quickly found widespread application in data analysis, scientific research, and the digital art scene. Its multifaceted nature allows it to manipulate the immense datasets collected by interplanetary probes or to craft elegant, responsive multimedia installations. Financial institutions adopted Sequential for the secure interpolation of cryptographic tape sequences, while academic institutions harnessed it for modelling simulations across multidisciplinary studies.

## Cultural Impact

Beyond functionality, Sequential has engendered a renaissance in the conceptualisation of programming. With Sequence Knitting, programming transforms into a blend of art, craftsmanship, and science. Tape Scribes (often incorrectly called 'Tapeheads'), responsible for the 'Data Weaving' processes inherent in SLCs, often speak of their work with Sequential as a form of expressive creation, marrying logic and creativity in a palpable, interactive experience.

## How to use

A Sequential program contains at least one Sequence. A Sequence is made up of one or more patterns. By default, the sequence loops through all patterns defined in order. This also means that every Sequence has an "Intro" field that defines the initial pattern queue, which loops by default, unless a pattern includes the PBRK instruction within the highest loop. The current level of the execution loop is stored within the system's LPLV (loop level) register. If it is 0, only the sequence's main pattern loop is running. Calling PBRK will halt execution of the program at the end of the current pattern.

Interrupts are also defined using `PTIN` followed by their Interrupt name. They execute automatically, in line, without needing to be called in the sequence. However, they *may* be included in the sequence definition, and if so, execute their code without being triggered. Usually, waiting for input is accomplished using the `WAIT` instruction, which waits until an interrupt is triggered. It's highly recommended to overload `WAIT` with the name of an interrupt to wait for that interrupt specifically. This may also lead to unintended side effects due to interrupt registers like `INXX` not being populated.

A program in Sequential is defined first with a definition of the patterns in order at the start of execution. The `SEQD` instruction defines the initial sequence. It expects `PTIN`-defined identifiers as arguments. If desired, two `SEQD` instructions can be used to wrap a longer sequence of patterns. The following program prompts the user for input using the keyboard and then prints a multiplication table on the terminal.

```
; Sequential Sample Program: Multiplication Table Generator
; Sequence of Patterns for Multiplication Table Program

SEQD
	MODESET
    PROMPTSEQ
    INITSEQ 
    MULTSEQ
    OUTSEQ
    CHECKSEQ
SEQD

PTIN MODESET
	MOVE 1 MODE
PEND

; Prompt User for Multiplication Number Pattern
PTIN PROMPTSEQ
    MOVE "Enter multiplicand number: " ECHO ; Prompt message
    LIFT ECHO ; Lift prompt onto PILE
    DROP TTY0 ; Output prompt onto terminal
    WAIT IKEY ; Waits for keyboard input
PEND

; Register for Input Key Interrupt Handling
; IKEY is triggered upon key press events
PTIN IKEY
    LIFT IN99 ; Lift the character from IN99 (keyboard register)
    CMPR IN99 100 ; Compare input to NULL, checking for 'Commit' key (value 100)
    JZER TSTR INITSEQ ; If 'Enter' key is detected, initialize sequence
    DROP Y ; Otherwise, store the character into the multiplicand register Y
PEND

; Init Pattern - Initializes registers for storing calculation values
PTIN INITSEQ
    INIT X 1 ; Initialize X as our counter register
    INIT T 10 ; Set the limit for the multiplication table (e.g., 10 for a table up to 10xN)
    INIT U 0 ; Result register (unused at this point, for clarity)
PEND

; Multiplication Sequence Pattern - Computes a single multiplication step
PTIN MULTSEQ
    MULT X Y U ; Perform multiplication (X * Y) and store result in U
PEND

; Output Pattern - Constructs and displays the output
PTIN OUTSEQ
    MOVE X ECHO ; Add the multiplier to ECHO register
    LIFT ECHO ; Lift onto stack
    MOVE " x " ECHO ; Add multiplication sign
    LIFT ECHO ; Lift onto stack
    MOVE Y ECHO ; Add the user input number
    LIFT ECHO ; Lift onto stack
    MOVE " = " ECHO ; Add equals sign
    LIFT ECHO ; Lift onto stack
    MOVE U ECHO ; Add the result of the multiplication
    LIFT ECHO ; Lift onto stack
    MOVE "=COMMIT" ECHO ; Add newline command
    LIFT ECHO ; Lift onto stack

    ; Drop values in reverse order due to stack LIFO nature
    DROP TTY0 ; Drop newline
    DROP TTY0 ; Drop result multiplication
    DROP TTY0 ; Drop equals
    DROP TTY0 ; Drop user input number
    DROP TTY0 ; Drop multiplication sign
    DROP TTY0 ; Drop multiplier
PEND

; Check Sequence - Determines whether to continue or stop the multiplication table
PTIN CHECKSEQ
    CMPR X T ; Compare the counter X with the limit T
    JNZR TSTR HALTSEQ ; If X == T, halt program
    INCR X ; Otherwise, increment X for next multiplication line
    PJMP MULTSEQ ; Jump back to multiplication sequence
PEND

; Halt Sequence - Stops program execution
PTIN HALTSEQ
    HALT ; Halts the Sequential program
PEND

; Execution begins here
PSTR ; Start executing patterns from the sequence definition
```
## Instruction Set

System-wide registers and their formats:

NULL - Zero register. If called, returns 0. If written to, discards data.
TTYX - Direct character output of terminal X. Use TTY0 for default terminal. Write-only.
PROG - Current position of the program counter. Read-only. To move execution, use SEEK or JUMP.
CPAT - Current pattern. Returns PTIN identifier. Read-only. To move execution, use PNXT, PPRV or PJMP.
PILE - Stack register for the LIFT/DROP instructions. Reading returns the number of items in the PILE. Read-only.
INXX - Input register from device XX. Read-only.
OUXX - Output register for device XX. Write-only.
MODE - Special register. Changes the SLC's operation mode (analogue/digital) when written. Returns mode when read.
TIME - Stores current timestamp. Read-only.
PTME - Stores time elapsed since program started. Read-only.
TSTR - Stores latest test result. Read-only. Fires the ITST interrupt when changed.
ECHO - Text converter. Written text can be read back as character data for use in OUXX/TTYX registers for display.

System-wide interrupts:

IDXX - Interrupt called when an input from device XX changes.
ITIM - Timer Interrupt: Triggered when a system or programmable timer reaches a certain value.
IKEY - Keyboard Interrupt: Occurs when a key is pressed or released on the keyboard. Alias for ID99.
IMOU - Mouse Interrupt: Activated by a change in mouse position or button state. Alias for ID98.
INET - Network Interrupt: Engaged when there is incoming network activity or a status change.
IMSG - Message Interrupt: Initiated by the arrival of a new message or data packet.
IERR - Error Interrupt: Fired when an error is detected, such as divide by zero, overflow, etc.
IDSK - Disk Interrupt: Triggered by a disk operation completion, such as read/write events.
IPWR - Power Interrupt: Occurs when there is a change in power status, like going to battery.
IUSR - User Interrupt: Custom interrupt that can be set by the user for specific applications.
IWAT - Watchdog Interrupt: Triggered when a watchdog timer expires, indicating a potential hang.
ISIG - Signal Interrupt: General purpose signal interrupt for external or internal events.
ICOM - Communication Interrupt: Triggered by events on serial or other communication ports.
ISEN - Sensor Interrupt: Activated by a reading change from an attached sensor.
IHUP - Hang Up Interrupt: Triggered by a disconnection or line drop on a communication interface.
IEXT - External Interrupt: A generic interrupt for external events not covered by other interrupts.
ITST - Test Interrupt: Triggered when a conditional instruction changes the TSTR register.

Pattern and Sequence control instructions:

SEQD - Sequence Define: Define initial sequence.
PTIN - Initialise Pattern: Define the start of a new pattern with a unique identifier.
PEND - End Pattern: Mark the end of the current pattern definition.
PLAD - Load Pattern: Load a pattern into the sequence queue by its identifier.
PSTR - Start Patterns: Start or resume pattern sequence playback from the current position.
PSTP - Stop Patterns: Stop pattern sequence playback, allows for pausing and manipulation.
PRES - Restart: Restart the sequence from the first pattern in the queue.
PNXT - Next Pattern: Jump to the next pattern in the sequence, regardless of the current one.
PPRV - Prev Pattern: Jump to the previous pattern in the sequence queue.
PJMP - Pattern Jump: Jump to a specified pattern in the queue, identified by its PTIN.
PRPT - Repeat Pattern: Repeat the current pattern for a given number of times or indefinitely.
PQUE - Queue Pattern: Queue a pattern to play immediately after the current one finishes.
PDYN - Dynamic Next: Conditionally choose the next pattern based on a register value or a flag.
PFOR - Forward Pattern: Advance the playback by a given number of patterns in the queue.
PBAK - Backward Pattern: Move the playback backward by a given number of patterns in the queue.
PSWP - Swap Patterns: Swap the positions of two patterns in the sequence queue.
PINS - Insert Pattern: Insert a pattern at a specific position in the active sequence.
PREM - Remove Pattern: Remove a pattern from the sequence queue by its identifier.
PSIZ - Size queue: Set the maximum size of the pattern sequence queue, may loop when reached.
PWAI - Wait: Insert a pause between pattern executions, can be conditioned on a register or flag.
PCND - Conditional play: Begin pattern playback only if a certain condition in a register or flag is met.
PLOP - Loop: Define a set of patterns to loop over within a larger sequence structure.
PBRK - Break Loop: Exit from a loop block within a pattern to continue with the next pattern.
PALL - All Patterns: Load all patterns defined so far into the sequence queue in a specified order.
PCLR - Clear Queue: Clear the current sequence queue, resetting playback.
PDMP - Dump Queue: Print out or otherwise output the current sequence queue for debugging.
PMOD - Modify Pattern: Modify the contents of a pattern at runtime, similar to editing a step in a sequencer.

Instructions to be used within a pattern:

INIT - Initialise a register to a specified value or to zero if no value given.
LREG - Load register with an immediate value (similar to INIT but always requires a value).
MOVE - Move contents from one register to another, or from tape position to register.
STOR - Store the content of a register to a specified tape position.
LOAD - Load content from specified tape position into a register.
LIFT - Lift the value at a specified tape position onto the top of the PILE stack.
DROP - Drop the top value from the PILE stack to a specified tape position.
VDRP - Visually drop the top value from PILE stack to a specified tape position without removing it from PILE.
SWAP - Swap contents between two registers or between a register and a tape position.
ADDV - Add values from two specified registers, storing the result in the first register.
SUBV - Subtract the value of the second specified register from the first.
MULT - Multiply contents of two registers, storing the result in the first register.
DIVV - Divide the first register by the second, storing the result in the first register.
INCR - Increment the value in the specified register.
DECR - Decrement the value in the specified register.
SEEK - Unconditional jump to a specified tape position (instruction address).
JNZR - Jump to a specified tape position if register does not equal zero.
JZER - Jump to a specified tape position if register equals zero.
CMPR - Compare the values in two registers, setting flags for equal, less than, or greater than.
TEST - Test the value in a specified register against a condition, setting a flag if true.
CALL - Call a subroutine at a specific tape position, storing return address.
RETN - Return from subroutine, using the stored return address.
LOGC - Logical AND, OR, XOR, or NOT operation on two registers or a register and a value.
SHFT - Shift bits left or right in a specified register.
WAIT - Waits for the next interrupt, then resumes execution. May be overloaded with an interrupt name, so only that interrupt will resume.
HALT - Halt execution of the program.
NOOP - No operation, can be used as a timing or synchronisation aid.