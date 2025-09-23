---
kind: unit

title: What is ASCII, ANSI and Unicode

name: 1-snake
---

Let's understand character encodings! These are the foundation for how computers represent text and symbols.

1. **ASCII**: 7 bits → 128 characters
   - English letters, digits, punctuation, control codes

2. **ANSI**: 8 bits → 256 characters
   - Keeps ASCII the same, adds accented letters, symbols, box-drawing

3. **Unicode**: covers all scripts and symbols worldwide
   - Defines over 1.1 million possible characters (code points)
   - Common encoding is **UTF-8**:
     - ASCII stays 1 byte (unchanged)
     - Other characters take 2-4 bytes

✅ **Key insight:** With Unicode you can freely mix text, symbols, and emojis in one program.
