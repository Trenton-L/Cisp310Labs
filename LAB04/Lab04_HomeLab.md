# Lab 4 — The Packet Loop (Branching & Looping)

**Released:** Monday, Week 5 · **Due:** Sunday end of Week 6, 11:59 PM
**Reading:** Detmer Ch. 5 · **File:** `Lab04_PacketLoop.asm`

## Mission brief

Eight packets came down last night; two were mangled. You write the ground station's validation loop: check every packet, reject the bad ones, keep statistics on the good ones. This loop is the heart of the final project.

The loop skeleton is written for you — ESI points at the current packet, EBX counts, and the reject/advance paths are given. You fill in six short gaps.

## The one idea

**Signed and unsigned data use different jumps.** Temperature (signed) → `JL / JLE / JG / JGE`. Battery (unsigned) → `JB / JBE / JA / JAE`. Compare −73 with 382 using `JA` and the CPU swears −73 is bigger, because FFFFFFB7h is a large unsigned number.

## The TODOs

| TODO | Lines | What | Check affected |
|---|---|---|---|
| 1 | 2 | sync check → reject | `validCount` |
| 2 | 2 | checksum loop body | `validCount` |
| 3 | 3 | new minimum temp (signed, JGE) | `minTemp` → −73 |
| 4 | 3 | new maximum temp (signed, JLE) | `maxTemp` → 382 |
| 5 | 3 | new maximum battery (unsigned, JBE) | `maxBatt` → 4100 |
| 6 | 3 | TEST the low-battery bit | `lowBattCount` → 2 |

`validCount` must land on 6 (packets 5 and 6 rejected).

## Required experiment (README)

In TODO 5, use `JLE` instead of `JBE`. It still passes on tonight's data. Explain why the bug is dormant, and give one battery value (in hex) that would expose it.

## README questions

1. Why does `minTemp` start at 7FFFFFFFh instead of 0? What goes wrong with 0 on tonight's data?
2. TODO 2 sums into `AL` — no `AND 0FFh` anywhere. Where is the mod-256?
3. The outer loop uses `INC/CMP/JL` while the inner uses `LOOP`. Why would using `LOOP` for both be dangerous?

## Submit

Push to `cisp310-labs-<username>/Lab04/`: the `.asm` file, `README.md`, and a screenshot of `ALL 5 CHECKS PASSED`. Paste the folder URL into Canvas. Canvas timestamp is the official clock.

## Grading

Builds with 0 errors · all 5 checks PASS · code quality (right instruction for the data type, comments that say *why*) · README. Point values are in the syllabus.

## Survival notes

- Infinite loop? The given `advance:` block moves ESI. Check that your reject path reaches it (`jne reject`, not a fall-through).
- Both rejected packets print `REJECTED`. If you see 8 `ok` lines, TODO 1 or 2 isn't rejecting.
